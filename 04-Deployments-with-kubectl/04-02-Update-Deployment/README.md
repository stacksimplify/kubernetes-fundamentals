# Update Deployments

## Step-00: Introduction
- We can update deployments using two options
  - Set Image
  - Edit Deployment

## Step-01: Updating Application version V1 to V2 using "Set Image" Option
```
# Get Container Name from current deployment
kubectl get deployment my-first-deployment -o yaml
Observation: Please Check the container name in spec.container.name and make a note of it and 
replace in below command <Container-Name>

# Update Deployment - SHOULD WORK NOW
kubectl set image deployment/<Deployment-Name> <Container-Name>=<Container-Image> --record=true
kubectl set image deployment/my-first-deployment kubenginx=stacksimplify/kubenginx:2.0.0 --record=true

# Verify Rollout Status 
kubectl rollout status deployment/my-first-deployment
Observation: By default, rollout happens in a rolling update model, so no downtime.

# Verify Deployment
kubectl get deploy

# Descibe Deployment
kubectl describe deployment my-first-deployment
Observation: Verify the Events and understand that Kubernetes by default do  "Rolling Update" 
for new application releases. 
With that said, we will not have downtime for our application.

# Verify ReplicaSet
kubectl get rs
Observation: New ReplicaSet will be created for new version

# Verify Pods
kubectl get po
Observation: Pod template hash label of new replicaset should be present for PODs letting us 
know these pods belong to new ReplicaSet.

# Check the Rollout History of a Deployment
kubectl rollout history deployment/<Deployment-Name>
kubectl rollout history deployment/my-first-deployment  
Observation: We have the rollout history, so we can switch back to older revisions using 
revision history available to us.  
```

## Step-02: Update the Application from V2 to V3 using "Edit Deployment" Option
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