## Namespace in Kubernetes

Namespaces in Kubernetes provide a way to logically divide cluster resources. They enable:

 * **Resource Isolation:** Segregate resources for different projects, teams, or environments.
 
 * **Multi-Tenancy:** Allow multiple users or teams to share a single cluster without interference.
 
 * **Efficient Resource Management:** Apply resource quotas and policies to different groups.

**Default Namespaces**

  *default:* Used when no namespace is specified for resources.
  
  *kube-system:* Hosts Kubernetes system components like the scheduler, API server, and DNS.
  
  *kube-public:* A public namespace accessible across the cluster, often used for cluster information.
  
  *kube-node-lease:* Stores lease objects for node heartbeats.
## Labels & Selector in Kubernetes

**Labels** in Kubernetes are key-value pairs that attach metadata to objects such as pods, nodes, and services. Labels are used to:

 * Organize resources.
 * Facilitate filtering and selection of objects.
 * Support operational management without modifying resource specifications.
   
**Selectors** are expressions used to filter Kubernetes objects based on their labels. They are essential for grouping and managing resources dynamically.

----------------------------------------------------------------------------------------------------------------------------------------------
### Task 1: Understanding Namespace commands

To create a namespace
```
kubectl create ns test-ns
```
To list all the available namespaces
```
kubectl get ns
```
To list all  the objects in a particular name-space
```
kubectl -n kube-system get all
```
To describe a namespace
```
kubectl describe ns kube-system
```


## Pod in Kubernetes

### Task 2: Create a pod using below Commands
```
kubectl run pod-1 --image nginx --port 80 
```
Check the newly created Pod
```
kubectl get pod
```
```
kubectl get pod -o wide
```
Describe Pod using below command
``` 
kubectl describe pod pod-1
```
Generate spec for running pod nginx and write it into a file called pod.yaml 
```
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml
``` 

### Task 3: Create a pod using below yaml
To check the version of the Object you are creating
```
kubectl api-resources
```
Let's create the YAML file
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
Check the labels of the Pod.
```
kubectl get pod --show-labels
```
To add label to a running pod
```
kubectl label pod <pod-name> cloudthat=k8s
```
Check the labels again
```
kubectl get pod <pod-name> --show-labels
```
To add a label when creating a prod
```
kubectl run <pod-name> --image nginx --labels <any-label>
```
Enter inside a single container pod
```
kubectl exec -it <pod-name> -- /bin/bash
```
```
exit
```
Now edit a single container pod to create a multi-container pod and add one more container
```
kubectl edit po <pod-name>
```
Enter into a particular container of multi-container pod
```
kubectl exec -it <pod-name> -c <container-name> -- /bin/bash
```
