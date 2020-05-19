# Services with YAML

## Step-01: Introduction

## Step-02: Create Backend Deployment & Cluster IP Service
- Write the Deployment template for backend REST application.
- Write the Cluster IP service template for backend REST application.
- **Important Notes:** 
  - Name of Cluster IP service should be `name: my-backend-service` because  same is configured in frontend nginx reverse proxy `default.conf`. 
  - Test with different name and understand the issue we face
```
cd <Course-Repo>\kubernetes-fundamentals\10-Services-with-YAML
kubectl get all
kubectl apply -f 01-backend-deployment.yml -f 02-backend-clusterip-service.yml
kubectl get all
```
- **01-backend-deployment.yml**
```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend-restapp
  labels:
    app: backend-restapp
    tier: backend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: backend-restapp
  template:
    metadata:
      labels:
        app: backend-restapp
        tier: backend
    spec:
      containers:
      - name: backend-restapp
        image: stacksimplify/kube-helloworld:1.0.0
        resources:
          requests:
            memory: "128Mi"
            cpu: "500m"
          limits:
            memory: "256Mi"
            cpu: "1000m"
        ports:
        - containerPort: 8080
```
- **02-backend-clusterip-service.yml**
```yml
apiVersion: v1
kind: Service
metadata:
  name: my-backend-service
  labels: 
    app: backend-restapp
    tier: backend
spec:
  selector:
    app: backend-restapp
  ports:
  - port: 8080
    targetPort: 8080
```

## Step-03: Create Frontend Deployment & NodePort Service
- Write the Deployment template for frontend Nginx Application
- Write the NodePort service template for frontend Nginx Application
```
cd <Course-Repo>\kubernetes-fundamentals\10-Services-with-YAML
kubectl get all
kubectl apply -f 03-frontend-deployment.yml -f 04-frontend-nodeport-service.yml
kubectl get all
```
- **03-frontend-deployment.yml**
```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend-nginxapp
  labels:
    app: frontend-nginxapp
    tier: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: frontend-nginxapp
  template:
    metadata:
      labels:
        app: frontend-nginxapp
        tier: frontend
    spec:
      containers:
      - name: frontend-nginxapp
        image: stacksimplify/kube-frontend-nginx:1.0.0
        resources:
          requests:
            memory: "128Mi"
            cpu: "250m"
          limits:
            memory: "256Mi"
            cpu: "500m"
        ports:
        - containerPort: 80
```
- **04-frontend-nodeport-service.yml**
```yml
apiVersion: v1
kind: Service
metadata:
  name: frontend-nginxapp-nodeport-service
  labels:
    app: frontend-nginxapp
    tier: frontend
spec:
  type: NodePort
  selector:
    app: frontend-nginxapp
  ports:
  - port: 80
    targetPort: 80
    nodePort: 31234
```
- **Access REST Application**
```
# Get External IP of nodes using
kubectl get nodes -o wide

# Access REST Application  (Port is static 31234 configured in frontend service template)
http://<node1-public-ip>:31234/hello
```

## Step-04: Delete & Recreate Objects using kubectl apply
### Delete Objects (file by file)
```
kubectl delete -f 01-backend-deployment.yml -f 02-backend-clusterip-service.yml -f 03-frontend-deployment.yml -f 04-frontend-nodeport-service.yml
kubectl get all
```
### Recreate Objects using YAML files in a folder
```
cd <Course-Repo>\kubernetes-fundamentals\
kubectl apply -f 10-Services-with-YAML
kubectl get all
```
### Delete Objects using YAML files in folder
```
cd <Course-Repo>\kubernetes-fundamentals\
kubectl delete -f 10-Services-with-YAML
kubectl get all
```

## Step-05: External Name Service

## Use Label Selectors for get and delete
- https://kubernetes.io/docs/concepts/cluster-administration/manage-deployment/#using-labels-effectively
- https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#label-selectors