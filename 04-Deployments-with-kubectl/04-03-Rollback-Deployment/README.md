# Rollback Deployment

## Step-00: Introduction
- We can rollback a deployment in two ways.
  - Previous Version
  - Specific Version

## Step-01: Rollback a Deployment to previous version
```
# Check the Rollout History of a Deployment
kubectl rollout history deployment/<Deployment-Name>
kubectl rollout history deployment/my-first-deployment  

# Verify changes in each revision
kubectl rollout history deployment/my-first-deployment --revision=1
kubectl rollout history deployment/my-first-deployment --revision=2
kubectl rollout history deployment/my-first-deployment --revision=3
Observation: Review the "Annotations" and "Image" tags for clear understanding about changes.

# Rollback to previous version
kubectl rollout undo deployment/my-first-deployment
kubectl rollout history deployment/my-first-deployment  
Observation: If we rollback, it will go back to revision-2 and its number increases to revision-4

# Access the Application and Test if we are "Application Version: V2"
http://<node1-public-ip>:<Node-Port>

# Verify Deployment, Pods, ReplicaSets
kubectl get deploy
kubectl get rs
kubectl get po
kubectl describe deploy my-first-deployment
```
## Step-02: Rollback to specific revision
```
# Check the Rollout History of a Deployment
kubectl rollout history deployment/<Deployment-Name>
kubectl rollout history deployment/my-first-deployment 

# Rollback to specific revision
kubectl rollout undo deployment/my-first-deployment --to-revision=3
kubectl rollout history deployment/my-first-deployment  
Observation: If we rollback to revision 3, it will go back to revision-3 and its number increases to revision-5

# Access the Application and Test if we are "Application Version: V3"
http://<node1-public-ip>:<Node-Port>
```

## Step-03: Rolling Restarts of Application
- Rolling restarts will kill the existing pods and recreate new pods. 
```
# Rolling Restarts
kubectl rollout restart deployment/<Deployment-Name>
kubectl rollout restart deployment/my-first-deployment

# Get list of Pods
kubectl get po
```