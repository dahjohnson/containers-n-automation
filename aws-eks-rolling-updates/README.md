# Performing Rolling Updates with AWS EKS

![aws-eks-rolling-update](https://user-images.githubusercontent.com/116639830/219545628-972e675b-74b7-4b3d-b265-d0109a92eb91.png)

- [Medium Blog Walkthrough](https://medium.com/@dahmearjohnson/performing-rolling-updates-with-aws-eks-375d33b89f7d "<performing-rolling-updates-with-aws-eks-375d33b89f7> Medium Blog Walkthrough")

## Objectives:
### - Create EKS cluster and node group.
### - Create a single pod deployment with a MySQL image and a load balancer service via a YAML file.
### - Scale the deployment to 5 replicas.
### - Perform a rolling update and monitor the update status.
### - Execute a rollback.

## Commands

### Create EKS cluster
`eksctl create cluster --name demo-cluster --region us-east-1 \
  --zones us-east-1a,us-east-1b,us-east-1c \
  --without-nodegroup`
  
### Create NodeGroup
`eksctl create nodegroup \
  --cluster demo-cluster \
  --region us-east-1 \
  --name demo-nodegroup \
  --node-ami-family Ubuntu2004 \
  --node-type t3.medium \
  --nodes 3 \
  --nodes-min 2 \
  --nodes-max 4 \
  --ssh-access \
  --ssh-public-key <EC2-ssh-key>`
  
### List CloudFormation Stacks
`aws cloudformation describe-stacks --query 'Stacks[].StackName'`
  
### Encode MySQL secret to base 64
`echo -n S3cr3tP@$sw0rd | base64`
  
### Create mysql-deployment.yml file
`vi mysql-deployment.yml`
  
### Deploy MySQL deployment and secret
`kubectl apply -f mysql-deployment.yml`
  
### Describe secret
`kubectl describe secret`
  
### View deployments
`kubectl get deployment`
  
### Describe MySQL deployment
`kubectl describe deployment mysql-deployment`
  
### View pods
`kubectl get pods -o wide`
  
### Create mysql-lb.yml file
`vi mysql-lb.yml`
  
### Deploy Load Balancer service
`kubectl apply -f mysql-lb.yml`
  
### List services
`kubectl get service`
  
### Describe Load Balancer service
`kubectl describe service mysql-lb`
  
### Install MySQL client
`sudo apt update -y # Update package index`

`sudo apt install -y mysql-client`

### Connect to MySQL
`mysql -h <load-balancer-URL> -u root -p<password>`

### List databases in MySQL
`SHOW DATABASES;`

### Edit deployment
`kubectl edit deployment mysql-deployment --record`

### View replica sets
`kubectl get rs`

### Update container image
`kubectl set image deployment/mysql-deployment mysql=mysql:8.0.32 --record`

### View rollout status
`kubectl rollout status deployment mysql-deployment`

### View rollout history
`kubectl rollout history deployment/mysql-deployment`

### Roll back changes
`kubectl rollout undo deployment mysql-deployment --to-revision=1`

### List and delete Load Balancer
`aws elb describe-load-balancers --query LoadBalancerDescriptions[].[LoadBalancerName,DNSName]`

`aws elb delete-load-balancer --load-balancer-name <load balancer name>`

### List CloudFormation stacks
`aws cloudformation describe-stacks --query 'Stacks[].StackName'`

### Delete CloudFormation stacks
`aws cloudformation delete-stack --stack-name <node-group-stack-name>`

`aws cloudformation delete-stack --stack-name <eks-cluster-stack-name>`

### Show CloudFormation stack status
`aws cloudformation describe-stacks --query 'Stacks[].[StackName,StackStatus]'`
 
