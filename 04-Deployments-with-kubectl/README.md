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
kubectl scale --replicas=20 deployment/<Deployment-Name>
kubectl scale --replicas=20 deployment/my-first-deployment 

# Verify Deployment
kubectl get deploy

# Verify ReplicaSet
kubectl get rs

# Verify Pods
kubectl get po

# Scale Down the Deployment
kubectl scale --replicas=10 deployment/my-first-deployment 
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
#### Update the Application verion from V1 to V2 using "Set Image" Option
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

### Update the Application from V2 to V3 using "Edit Deployment" Option
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

### Rollback to Previous Version
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
#### Rollback to specific revision
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

## Step-07: Rolling Restarts of Application
- Rolling restarts will kill the existing pods and recreate new pods. 
```
# Rolling Restarts
kubectl rollout restart deployment/<Deployment-Name>
kubectl rollout restart deployment/my-first-deployment

# Get list of Pods
kubectl get po
```

## Step-08: Pausing & Resuming Deployments
- Why do we need Pausing & Resuming Deployments?
  - If we want to make multiple changes to our Deployment, we can pause the deployment make all changes and resume it. 
- We are going to update our Application Version from **V3 to V4** as part of learning "Pause and Resume Deployments"   
```
# Check the Rollout History of a Deployment
kubectl rollout history deployment/my-first-deployment  
Observation: Make a note of last version number

# Get list of ReplicaSets
kubectl get rs
Observation: Make a note of number of replicaSets present.

# Access the Application 
http://<node1-public-ip>:<Node-Port>
Observation: Make a note of application version

# Pause the Deployment
kubectl rollout pause deployment/<Deployment-Name>
kubectl rollout pause deployment/my-first-deployment

# Update Deployment - Application Version from V3 to V4
kubectl set image deployment/my-first-deployment kubenginx=stacksimplify/kubenginx:4.0.0 --record=true

# Check the Rollout History of a Deployment
kubectl rollout history deployment/my-first-deployment  
Observation: No new rollout should start, we should see same number of versions as we check earlier with last version number matches which we have noted earlier.

# Get list of ReplicaSets
kubectl get rs
Observation: No new replicaSet created. We should have same number of replicaSets as earlier when we took note. 

# Make one more change: set limits to our container
kubectl set resources deployment/my-first-deployment -c=kubenginx --limits=cpu=200m,memory=512Mi

# Resume the Deployment
kubectl rollout resume deployment/my-first-deployment

# Check the Rollout History of a Deployment
kubectl rollout history deployment/my-first-deployment  
Observation: You should see a new version got created

# Get list of ReplicaSets
kubectl get rs
Observation: You should see new ReplicaSet.

# Access the Application 
http://<node1-public-ip>:<Node-Port>
Observation: You should see Application V4 version
```
