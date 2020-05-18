# ReplicaSets with YAML

## Step-01: Understand Labels & Selectors
- Discuss about Labels and Selectors

## Step-02: Create ReplicaSet
- Create ReplicaSet with 3 Replicas
- Verify pod names, one pod which we created using `pod-definition.yml` will be part of replicaset because it also has `labels: app=myapp`
```
kubectl apply -f replicaset-definition.yml 
kubectl get rs
kubectl get pods
```
- Delete the pod which we created using `pod-definition.yml`
- ReplicaSet immediately creates the pod. 
```
kubectl delete pod myapp-pod
kubectl get pods
```
- **replicaset-definition.yml**
```yml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-rs
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