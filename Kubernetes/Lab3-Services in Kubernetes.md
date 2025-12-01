## Services in Kubernetes
In Kubernetes, a **service** is an abstraction that exposes a logical set of pods as a network endpoint. It provides consistent networking and load balancing for pods, even as they are dynamically created or terminated.

**Why Use Services?**
  * *Pod IP Address Ephemerality:* Pods are temporary and their IPs change as they restart. Services provide stable access to pods.
  * *Load Balancing:* Services distribute traffic across multiple pods.
  * *Service Discovery:* Services enable other components or external clients to locate pods without hardcoding IPs.

**Types of Services**

1. **ClusterIP (Default):**
  * Exposes the service only within the cluster.
  * Suitable for internal communication between services.

2. **NodePort:**
  * Exposes the service on a static port (30000-32767) on each node's IP.
  * Accessible externally by using <NodeIP>:<NodePort>

3. **LoadBalancer:**
  * Integrates with a cloud provider's load balancer to expose the service externally.
  * Suitable for public-facing applications.
  
----------------------------------------------------------------------
### Task 1: Create a pod using below yaml
```
vi httpd-pod.yaml
```

Add the given content, by pressing `INSERT`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: httpd-pod
  labels:
    env: prod 
    type: front-end
    app: httpd-ws
spec:
  containers:
  - name: httpd-container
    image: httpd
    ports:
       - containerPort: 80
```
save the file using `ESCAPE + :wq!`

Apply the pod definition yaml
```
kubectl apply -f httpd-pod.yaml
```
Check the newly created Pod
```
kubectl get pods
```
```
kubectl get pods -o wide
```
Describe Pod using below command
```
kubectl describe pod httpd-pod
```


### Task 2: Setup ClusterIP service
Create  a ClusterIP service using below YAML
```
vi httpd-svc.yaml
```
Add the given content, by pressing `INSERT`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: httpd-svc
spec:
  selector:
    env: prod
    type: front-end
    app: httpd-ws
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: ClusterIP
```
save the file using `ESCAPE + :wq!`

Apply above definition using below to create a ClusterIP service
```
kubectl apply -f httpd-svc.yaml
```
Describe the service and verify it has populated the endpoints with IP address matching Pod label
```
kubectl get svc
```
```
kubectl describe svc httpd-svc
```
Get EndPoint of the service
```
kubectl get ep  
```
Get External IPs of the machines in the cluster using below.
```
kubectl get nodes -o wide | awk '{print $7}'
```
SSH to one of the machines and rerun the command in the previous task
```
ssh -t ubuntu@<Node_IP> curl <Cluster_IP>:<Service_Port>
```

### Task 3: Setup NodePort Service
Modify the service created in the previous task to type NodePort
```
vi httpd-svc.yaml
```
Add the given content, by pressing `INSERT`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: httpd-svc
spec:
  selector:
    env: prod
    type: front-end
    app: httpd-ws
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: NodePort
```
save the file using `ESCAPE + :wq!`

Apply the changes using below command
```
kubectl apply -f httpd-svc.yaml
```
View details of the modified service
```
kubectl describe svc httpd-svc
```
Validate connectivity using External IP on NodePort using below or via browser
```
curl <EXTERNAL-IP>:NodePort
```
Get External IPs of the machines in the cluster. SSH to one of the machines and rerun the command in the previous task
```
kubectl get nodes -o wide | awk '{print $7}'
```
```
ssh -t ubuntu@<Node_IP> curl <Cluster_IP>:<Service_Port>
```

### Task 4: Setup LoadBalancer Service
Modify the service created in the previous task to type LoadBalancer 
```
vi httpd-svc.yaml
```
Add the given content, by pressing `INSERT`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: httpd-svc
spec:
  selector:
    env: prod
    type: front-end
    app: httpd-ws
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: LoadBalancer
```
save the file using `ESCAPE + :wq!`

Apply the changes using below command
```
kubectl apply -f httpd-svc.yaml
```
Verify that a new service of type LoadBalancer has been created
```
kubectl get svc
```
```
kubectl describe svc httpd-svc
```
Access the LoadBalancer on the kops instance or via browser
```
curl <LoadBalancer_DNS>
```

### Task 5: Delete and recreate httpd Pod
Delete the existing httpd-pod using below
```
kubectl delete -f httpd-pod.yaml
```
View the service details and notice that the Endpoints field is empty
```
kubectl describe svc httpd-svc
```
Recreate the httpd Pod and view service details Verify that the endpoints is updated with new Pod IP
```
kubectl apply -f httpd-pod.yaml
```
```
kubectl describe svc httpd-svc
```

### Task 6: Cleanup the resources using below command
```
kubectl delete -f httpd-pod.yaml
```
```
kubectl delete -f httpd-svc.yaml
```
