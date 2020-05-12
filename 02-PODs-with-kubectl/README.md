# Kubernetes  - PODs

## Step-01: PODs Introduction
- What is a POD ?
- What is a Multi-Container POD?

## Step-02: PODs Demo
### Get Nodes
- Verify if kubernetes worker nodes are ready. 
```
kubectl get nodes
```

### Create Pod
- Create a Pod
```
kubectl run my-first-pod --image stacksimplify/kubenginx:1.0.0 --generator=run-pod/v1
```
- **Important Note:** Without **--generator=run-pod/v1** it will create a pod with a deployment. 

### List Pods
- Get the list of pods
```
kubectl get pods
kubectl get po
```

### List Pods with wide option
- List pods with wide option which also provide Node information on which Pod is running
```
kubectl get pods -o wide
```

### Describe Pod
- Describe the POD, primarily required during troubleshooting. 
- Events shown will be of a great help during troubleshooting. 
```
kubectl get pods
kubectl describe pod my-first-pod 
```

### Delete Pod
```
kubectl delete pod my-first-pod
```