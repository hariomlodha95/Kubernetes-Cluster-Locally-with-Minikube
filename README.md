## Build a Kubernetes Cluster Locally with Minikube

🎯 Objective
  - Deploy and manage applications on Kubernetes using Minikube.
---
🧰 Tools Required
   - Minikube
   - kubectl
   - Docker
---
📦 Deliverables
   - Kubernetes manifests (deployment.yaml, service.yaml)
   - Screenshots of pods, services, scaling, and describe logs
---

🚀 Step 1: Install Minikube & Start Cluster

  Start a local Kubernetes cluster using Docker driver.
   - minikube start --driver=docker
```
✅ Sample Output:
😄  minikube v1.37.0 on Centos 10
✨  Using the docker driver based on user configuration
👍  Starting control plane node minikube in cluster minikube
🔄  Pulling base image ...
🌟  Kubernetes 1.30.0 is now running!
```
---
📄 Step 2: Create Deployment
Store the following file in: manifests/deployment.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp-container
        image: nginx
        command: ["/bin/bash", "-c", "while true; do echo build a kubernetes cluster locally with minikube; sleep 5; done"]
        ports:
        - containerPort: 80
```
Apply the deployment:
  - kubectl apply -f manifests/deployment.yaml
---

🌐 Step 3: Expose App Using Service
    Store the following file in: manifests/service.yaml
```
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: NodePort
  selector:
    app: myapp
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30007
```
Apply the service:
  - kubectl apply -f manifests/service.yaml
---
🔍 Step 4: Verify Pods & Services
List Pods:
  - kubectl get pods

✅ Sample Output:
```
NAME                                   READY   STATUS    RESTARTS   AGE
myapp-deployment-7f8c95d9f4-abcde      1/1     Running   0          40s
myapp-deployment-7f8c95d9f4-fghij      1/1     Running   0          40s
```
List Services:
  - kubectl get svc

✅ Sample Output:
```
NAME            TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)         AGE
kubernetes      ClusterIP  10.96.0.1        <none>        443/TCP         10m
myapp-service   NodePort   10.96.145.203    <none>        80:30007/TCP    20s
```
---
🌐 Step 5: Access App in Browser

Open the application using Minikube:
 - minikube service myapp-service

✅ Sample Output:
```
|-----------|--------------|-------------|--------------------------|
| NAMESPACE |     NAME     | TARGET PORT |           URL            |
|-----------|--------------|-------------|--------------------------|
| default   | myapp-service|          80 | http://192.168.49.2:30007|
|-----------|--------------|-------------|--------------------------|
```
🏃  Opening service default/myapp-service in default browser...
---
📈 Step 6: Scale Deployment

Scale replicas up/down:
  - kubectl scale deployment myapp-deployment --replicas=5
```
✅ Sample Output:
deployment.apps/myapp-deployment scaled
```
---
🛠 Step 7: Describe Resources

Use describe for debugging/log analysis.
Describe Deployment:
 - kubectl describe deployment myapp-deployment

✅ Sample Output:
```
Name:                   myapp-deployment
Namespace:              default
Selector:               app=myapp
Replicas:               5 desired | 5 updated | 5 total | 5 available | 0 unavailable
StrategyType:           RollingUpdate
Pod Template:
  Containers:
   myapp-container:
    Image:        nginx
    Port:         80/TCP
```
Describe Pod:
  - kubectl describe pod <pod-name>

✅ Sample Output:
```
Name:         myapp-deployment-7f8c95d9f4-abcde
Namespace:    default
Status:       Running
IP:           172.17.0.4
Containers:
  myapp-container:
    Image:        nginx
    Started:      True
```
---
✅ Summary
This project demonstrates:
 -	Running Minikube locally
 -	Deploying an NGINX app on Kubernetes
 -	Exposing using NodePort service
 -	Scaling deployment replicas
 -  Viewing logs & describing resources


