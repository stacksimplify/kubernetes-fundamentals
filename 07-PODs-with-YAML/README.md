# PODs with YAML

## Step-01: Create Pod
- We are going to create a very basic pod definition.
#### Imperative Style
```
kubectl create -f pod-definition.yml
kubectl get pods
```
#### Declarative Style
```
kubectl apply -f pod-definition.yml
kubectl get pods
```
- **pod-definition.yml**
```yml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    name: myapp
spec:
  containers:
  - name: myapp
    image: stacksimplify/kubenginx:1.0.0
    ports:
      - containerPort: 80
```

## Step-02: Change Image Version
- Change image version from 1.0.0 to 2.0.0
### Imperative Style
```
kubectl replace -f pod-definition.yml
kubectl get pods
```
### Declarative Style
```
kubectl apply -f pod-definition.yml
```
- Verify pods
```
kubectl get pods
kubectl describe pod <pod-name>
kubectl describe pod myapp-pod
```
- **Observation:** Only container inside pod gets killed and restarts with new container image.

- **pod-definition.yml**
```yml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    name: myapp
spec:
  containers:
  - name: myapp
    image: stacksimplify/kubenginx:2.0.0
    ports:
      - containerPort: 80
```
