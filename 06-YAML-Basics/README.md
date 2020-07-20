# YAML Basics

## Step-01: YAML Fundamentals
### Key Value Pairs & Comments
  - Space after colon is mandatory to differentiate key and value
```yml
Name: Kalyan
City: New Jersey
```

### Array / Lists
  - Dash indicates an element of an array
```yml
Employees:
  - Kalyan
  - John
  - David

Departments:
  - HR
  - Training
  - Development
  - Infrastructure
```  
### Dictionary / Map
  - Set of properties grouped together after an item
  - Equal amount of blank space required for all the items under a dictionary
```yml
Employee:
  name: Kalyan
  city: Hyderabad
  email: dkalyanreddy@gmail.com
  department: HR
```  

## Step-02: YAML for Kubernetes Manifests
- Comments start with `#` in YAML
- Discuss Parent, Child and Spaces

```yml
apiVersion: v1 # String
kind: Pod  # String
metadata: # Dictionary
  name: myapp-pod
  labels: # Dictionary 
    app: myapp         
spec:
  containers: # List
    - name: myapp
      image: stacksimplify/kubenginx:1.0.0
      ports:
        - containerPort: 80
          protocol: "TCP"
        - containerPort: 81
          protocol: "TCP"
```




