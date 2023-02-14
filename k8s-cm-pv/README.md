# More Kubernetes: ConfigMaps and PersistentVolumes

![k8s-cm-pv](https://user-images.githubusercontent.com/116639830/218748652-04c37eb0-5357-4c78-b104-9721a3b154ba.png)

- [Medium Blog Walkthrough](https://medium.com/@dahmearjohnson/more-kubernetes-configmaps-and-persistentvolumes-7df6119ba58a "<more-kubernetes-configmaps-and-persistentvolumes-7df6119ba58a> Medium Blog Walkthrough")

## Objectives:

### - Deploy two Deployments with two Pods, each running the nginx image.
### - Include ConfigMaps that point to two different custom index.html pages.
### - Create a service that points to both deployments. Confirm reachability to both using the same IP address and port number.
### - Create a persistent storage manifest that gets approx. 250MB of disk space on the host node in the /temp/k8s directory. Then, create a corresponding persistent storage claim manifest file.

## Commands

### Create and update resources in the cluster
`kubectl apply -f <YAML file name>`

### View ConfigMap details
`kubectl get cm`

`kubectl describe cm <configmap name>`

### View Pods
`kubectl get pods -o wide`

### View Services
`kubectl get svc -o wide`

### View StorageClasses
`kubectl get sc`

`kubectl describe sc <storageclass name>`

### View PersistentVolumes
`kubectl get pv`

`kubectl descirbe pv <persistentvolume name>`

### View PersistentVolumeClaims
`kubectl get pvc`

`kubectl descirbe pvc <persistentvolumeclaim name>`
