# Deployments with YAML

## Step-01: Create Deployment
- Create Deployment
```
kubectl apply -f deployment-definition.yml
kubectl get deploy
kubectl get rs
kubectl get po
```
- **deployment-definition.yml**
```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
  labels:
    app: myapp 
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: stacksimplify/kubenginx:2.0.0
        ports:
          - containerPort: 80
```

## Step-02: Update container image to 3.0.0
- Update image version from `stacksimplify/kubenginx:2.0.0` to `stacksimplify/kubenginx:3.0.0`
```
kubectl apply -f deployment-definition.yml
kubectl get deploy
kubectl get rs
kubectl get po
```
- **deployment-definition.yml**
```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
  labels:
    app: myapp 
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: stacksimplify/kubenginx:3.0.0
        ports:
          - containerPort: 80
```
