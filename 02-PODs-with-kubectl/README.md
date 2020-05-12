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
- **What happened in the backgroup when above command is run?**
  1. Kubernetes created a pod
  2. Pulled the docker image from docker hub
  3. Created the container in the pod
  4. Started the container present in the pod
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
# To get list of pod names
kubectl get pods

# Describe the Pod
kubectl describe pod my-first-pod 
```

### Access Application
- Currently we can access this application only inside worker nodes. 
- To access it externally, we need to learn important concept **Services**. 
- We will access the application and test when we reach the **Services** section.
- For now we have learned about the below. 
  - Creating a Pod
  - Getting List of Pods
  - Describe the Pod
  - Deleting the Pod.


### Delete Pod
```
# To get list of pod names
kubectl get pods

# Delete Pod
kubectl delete pod my-first-pod
```