# Kubernetes - Deployment




## Step-01: Introduction to Deployments

## Step-02: Create Deployment
- Create Deployment to rollout a ReplicaSet
- Verify Deployment, ReplicaSet & Pods
```
# Create Deployment
kubectl create deployment my-first-deployment --image=stacksimplify/kubenginx:1.0.0 

# Verify & Describe Deployment
kubectl get deployments
kubectl describe deployment <deployment-name>
kubectl describe deployment my-first-deployment

# Verify ReplicaSet
kubectl get rs

# Verify Pod
kubectl get po
```
## Step-03: Scaling a Deployment
- Scale the deployment to increase the number of replicas (pods)
```
# Scale Up the Deployment
kubectl scale deployment my-first-deployment --replicas=20

# Verify Deployment
kubectl get deploy

# Verify ReplicaSet
kubectl get rs

# Verify Pods
kubectl get po

# Scale Down the Deployment
kubectl scale deployment my-first-deployment --replicas=10
kubectl get deploy
```

## Step-04: Expose Deployment as a Service
- Expose **Deployment** with a service (NodePort Service) to access the application externally (from internet)
```
# Expose Deployment as a Service
kubectl expose deployment <Deployment-Name>  --type=NodePort --port=80 --target-port=8080 --name=<Service-Name-To-Be-Created>
kubectl expose deployment my-first-deployment --type=NodePort --port=80 --target-port=80 --name=my-first-deployment-service

# Get Service Info
kubectl get svc
Observation: Make a note of port which starts with 3 (Example: 80:3xxxx/TCP). Capture the port 3xxxx and use it in application URL below. 

# Get Public IP of Worker Nodes
kubectl get nodes -o wide
Observation: Make a note of "EXTERNAL-IP" if your Kubernetes cluster is setup on AWS EKS.
```
- **Access the Application using Public IP**
```
http://<node1-public-ip>:<Node-Port>
```

## Step-05: Updating a Deployment
#### Update the Application verion from 1.0.0 to 2.0.0 using "Set Image" Option
```
# Update Deployment -- SHOULD FAIL DUE TO WRONG CONTAINER NAME
kubectl set image deployment/my-first-deployment abcdefg=stacksimplify/kubenginx:2.0.0 --record=true

# Get Container Name from current deployment
kubectl get deployment my-first-deployment -o yaml
Observation: Please Check the container name in spec.container.name and make a note of it and replace in below command <Container-Name>

# Update Deployment - SHOULD WORK NOW
kubectl set image deployment/<Deployment-Name> <Container-Name>=<Container-Image> --record=true
kubectl set image deployment/my-first-deployment kubenginx=stacksimplify/kubenginx:2.0.0 --record=true

# Verify Rollout Status 
kubectl rollout status deployment/my-first-deployment
Observation: Rollout happens in a rolling update model, so no downtime.

# Verify Deployment
kubectl get deploy

# Descibe Deployment
kubectl describe deployment my-first-deployment
Observation: Verify the Events and understand that Kubernetes by default do  "Rolling Update" for new application releases. With that said, we will not have downtime for our application.

# Verify ReplicaSet
kubectl get rs
Observation: New ReplicaSet will be created for new version

# Verify Pods
kubectl get po
Observation: Pod template hash label of new replicaset should be present for PODs letting us know these pods belong to new ReplicaSet.

# Check the Rollout History of a Deployment
kubectl rollout history deployment/<Deployment-Name>
kubectl rollout history deployment/my-first-deployment  
Observation: We have the rollout history, so we can switch back to older revisions using revision history available to us.  
```

### Update the Application from 2.0.0 to 3.0.0 using "Edit Deployment" Option
```
# Edit Deployment
kubectl edit deployment/<Deployment-Name> --record=true
kubectl edit deployment/my-first-deployment --record=true

# Verify Rollout Status 
kubectl rollout status deployment/my-first-deployment
Observation: Rollout happens in a rolling update model, so no downtime.

# Verify ReplicaSet and Pods
kubectl get rs
kubectl get po
Observation:  We should see 3 ReplicaSets now, as we have updated our application to 3rd version 3.0.0

# Check the Rollout History of a Deployment
kubectl rollout history deployment/<Deployment-Name>
kubectl rollout history deployment/my-first-deployment   
```

## Step-06: Rollback a Deployment

```
# Check the Rollout History of a Deployment
kubectl rollout history deployment/<Deployment-Name>
kubectl rollout history deployment/my-first-deployment  
```
