# Kubernetes Architecture

## Step-01: Why Kubernetes?

## Step-02: Kubernetes Architecture

In the master Node we have the componments as

1 - Container Run Time (Docker/Containerd)

2 - etcd - HA - Key value store to store all clsuter data all master and worker node information

3 - Kube scheduler - For distrubuting contianer to new Node - watch pod and assign the pod to the node 

4 - Kube API-Server -  It exposes Kuberenets API - kubectl/etcd everything talk with the API server 

5 - Kube control Manager - If node / pod go down it will manage that - combinitaion of multiple controller -
    1 - Node controller 
    2 - replication controller - Mantaining the correct number of the pod
    3 - endpoint controller - populate endpoints as the service and the pod 
    4 - service account and the token controller - create default api for new namespace 

6 - Cloud Control Manager  - It is responsible for cloud specific logic - On prem not have this componment 

7 - Route controller - For setting the routes inside the cloud onfra

8 - Service controller - For creating/updating/deleting the cloud provider load balancer

In the NodeGroup we have componments as 

1 - Container Run Time 

2- Kubelet - Run on every node in the cluster - Respolsible to make sure that the containers running in the pod on the node

3 - Kube Proxy - Maintain the pod connectivity within / outside the cluster 

Please Note that when we use the EKS then 
    we no need to worry about any of the componments 
    Our focus is to design and deploy the applicaion in the AWS
    HA / Maintance we no need to worry 
