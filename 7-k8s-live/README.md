# Kubernetes Contents #

# Core Concept #

Kubernetes
    Why K8s required ?
    CRI/CNI/CSI
Kubernetes Architecture
    Master Components
    Worker Components
Methods of building k8s cluster
Kubectl
Pod
Workload
Deployment
Namespace
Resource quota, Limit range
Resource requirements & Limit
Service
Endpoint
Dns
Daemonset
Static pod
Autoscaling (HPA ,VPA)
Job & Cronjob 36 Statefulset
Headless service
Statefulset & storage

# Scheduling #

How scheduling works? 12 Label & selector
Annotations
Node selector
Affinity & anti-affinity
Taint & toleration
Taint/tolerate & node affinity
Priority class & preemption
Pod distribution budget 15 Bin packing

# Lifecycle Management # 

Configmap, Secret
Init Container
Pod Lifecycle
Sidecar Container
Rollout & Rollback
Probes
Node Maintenance
Cluster upgrade
Backup & Restore

# Security # 

Security
Authentication
Authorization (RBAC)
Admission control
Service Account
Api groups
Kubeconfig
Authentication with X509
Auditing
RuntimeClass
Network Policy
Security Context
Image security
Gatekeeper

# Storage # 

HostPath volume 34 EmptyDir volume
Persistent volume(pv) & pvc
Static & Dynamic provisioning

# Addons # 

How to deploy an application in k8s? 
Kustomize
Helm
Operator
Ingress
Cert-Managek

---

# Day01-26Apr24

# In Kubernetes:

Pods: 
ReplicaSet:
Deployments:
Services:
Labels:
Endpoints:
Service Publishing:
Namespaces:
Context:
Ingress:
DNS:
Configmap:
Secrets:
Persistent Volume:
Storage Class:
Stateful Sets:
RBAC:
Pod Backups:
Helm Charts:
Taint and Tolerations:
Daemon Set:
Network Policies:
Resource Requests and Limits:
Pod Autoscaling:
Liveness and Readiness Probes:
Validating Admission Controllers:
Jobs and CronJobs:

----

# Day02-30Apr24: Architecture

# Master Worker architecture

# Master Node:
    etcd
        key-value db used as backlog store for all cluster configuration data
    kube-controller-manager
        run k8s controllers
    kube-scheduler
        decides which node should be used for each Pod
    kube-apiserver
        allows interacting with the control plane

# Worker Node:
    kube proxy
        maintain n/w rules on nodes
    kubelet
        manage containers on node
    container-runtime
        run containers on node

---

# Kubernetes Components:

In Kubernetes, there are several key components that work together to manage containerized applications. Here are some of the core components along with brief definitions:

1. Node:
   - Definition: A physical or virtual machine that runs your applications and other Kubernetes components.
   - Key Components:
     - Kubelet: An agent that runs on each node and ensures that containers are running in a Pod.
     - Container Runtime: Software responsible for running containers (e.g., Docker, containerd).

2. Pod:
   - Definition: The smallest and simplest unit in the Kubernetes object model, representing a single instance of a running process in a cluster.
   - Key Components:
     - Containers: Docker containers or other container runtimes that run within the Pod.
     - PodSpec: Defines how to run the containers within the Pod.

3. Controller:
   - Definition: A higher-level abstraction that manages the deployment, scaling, and maintenance of multiple instances of Pods.
   - Examples:
     - Deployment Controller: Manages the deployment of applications using declarative configurations.
     - ReplicaSet Controller: Ensures that a specified number of replicas of a Pod are running.
     - StatefulSet Controller: Manages the deployment and scaling of a set of Pods with unique identities.

4. Service:
   - Definition: An abstraction that defines a logical set of Pods and a policy by which to access them.
   - Types:
     - ClusterIP: Exposes the Service on an internal IP within the cluster.
     - NodePort: Exposes the Service on a static port on each node's IP.
     - LoadBalancer: Creates an external load balancer and assigns a fixed, external IP to the Service.
     - ExternalName: Maps the Service to the contents of the externalName field.

5. Namespace:
   - Definition: A way to divide cluster resources between multiple users or projects.
   - Use Cases:
     - Logical partitioning of resources.
     - Access control and resource quota enforcement.

6. ConfigMap and Secret:
   - Definition: ConfigMaps and Secrets are used to store configuration data and sensitive information, respectively.
   - ConfigMap Use Case: Storing configuration files, command-line arguments, etc.
   - Secret Use Case: Storing sensitive data like passwords, API keys, and tokens.

7. Volume:
   - Definition: A directory that is accessible to all containers in a Pod.
   - Types:
     - EmptyDir: An initially empty directory shared by all containers in a Pod.
     - HostPath: Mounts a file or directory from the host node's filesystem.
     - PersistentVolume: Represents a piece of network storage provisioned by an administrator.

8. Ingress:
   - Definition: Manages external access to services within a cluster, typically HTTP.
   - Key Components:
     - Ingress Controller: A component that manages external access to services based on Ingress rules.

9. Kubernetes API Server:
   - Definition: Exposes the Kubernetes API, which is used by other components to interact with the cluster.
   - Key Functions:
     - Validates and configures data for the api objects.
     - Serves the Kubernetes API over HTTPS.

These components work together to provide a robust and scalable platform for deploying, managing, and scaling containerized applications in a Kubernetes cluster.


# Kubernetes Architecture:

Kubernetes has a distributed architecture designed for managing containerized applications across a cluster of machines. Below is an overview of the key components in the Kubernetes architecture:

1. Control Plane (Master Node):
   - API Server: Exposes the Kubernetes API, which is used by all other components. It serves as the frontend for the Kubernetes control plane.
   - etcd: A distributed key-value store used for storing the cluster configuration and the state of the entire cluster.
   - Controller Manager: Watches the state of the cluster through the API server and makes changes to bring the current state closer to the desired state.
   - Scheduler: Assigns nodes to newly created Pods based on resource requirements, policies, and other constraints.

2. Nodes (Minion/Worker Nodes):
   - Kubelet: An agent that runs on each node and is responsible for ensuring that containers are running in a Pod.
   - Container Runtime: Software responsible for running containers, such as Docker, containerd, or cri-o.
   - Kube Proxy: Maintains network rules on nodes, allowing communication between Pods and external traffic.

3. Pod:
   - The basic building block of a Kubernetes application, representing the smallest deployable units that can be created, scheduled, and managed.

4. Service:
   - An abstraction that defines a logical set of Pods and a policy by which to access them.

5. Volume:
   - A directory that is accessible to all containers in a Pod, providing a mechanism for persisting data.

6. ConfigMap and Secret:
   - ConfigMaps store configuration data, and Secrets store sensitive information.

7. Namespace:
   - A way to divide cluster resources between multiple users or projects.

8. Ingress:
   - Manages external access to services within a cluster, typically HTTP.

9. Persistent Volumes (PVs) and Persistent Volume Claims (PVCs):
   - PVs represent physical storage resources, while PVCs are requests for those resources by Pods.

10. StatefulSets:
    - Manages the deployment and scaling of a set of Pods, with unique identities and stable network identities.

11. DaemonSets:
    - Ensures that a copy of a Pod is running across a set of nodes in the cluster.

12. ReplicaSets:
    - Ensures that a specified number of replicas of a Pod are running.

13. Secrets Controller:
    - Manages the lifecycle of Secrets.

14. Horizontal Pod Autoscaler:
    - Automatically adjusts the number of Pods in a deployment or replica set based on observed CPU utilization.

15. Security Contexts:
    - Define privilege and access control settings for Pods.

This architecture allows Kubernetes to manage the deployment, scaling, and operation of application containers across clusters of hosts, providing a consistent and declarative way to deploy and manage applications. The distributed nature of the architecture ensures high availability, scalability, and fault tolerance.

---

# Example: pod.yaml

'''
apiVersion: v1 
kind: Pod 
metadata:
  name: pod-nginx 
  namespace: default 
  labels:
    app: nginx
    type: front-end 
spec:
  containers:
    - name: nginx-container
      image: nginx:1.18
'''

# Elaboration of pod.yaml

'''
apiVersion: v1    # version of k8s APIs used to create and manage the resource
kind: Pod         
metadata:
  name: pod-nginx # specifies kind of k8s resource
  namespace: default
  labels:         # about pod, such as it's name, namespaces, annotations...
    app: nginx
    type: front-end 
spec:             # specification of the Pod, including the container to run, n/w config, storage volumes to use
  containers:
    - name: nginx-container
      image: nginx:1.18
'''

'''
# kubectl apply -f pod-def.yaml

1. kube-api authenticates the request. kube-api checks authorizations 
2. kube-api check the manifests file associated with the pod for syntax errors
3. kube-api writes the pod's manifest file to etcd 
4. The Scheduler find a new unassigned pod on etcd and attempts to schedule it to run on an available nodes 
5a. The Scheduler selects a node to assign the pod to it. The Scheduler reports the selectd node to the kube-api
5b. Kube-api updates etcd with the changes pod's status field
6a. kube-api sends a pod creation request to kubelet
6b. Kubelet pulls the container images and starting the containers, updated pod status 
7. kube-api reads pod information from kubelet and updates the pod status in etcd 

$ kubectl apply -f pod-def.yaml 
pod/pod-nginx created
'''

----

# Day03-02May24

'''
apiVersion: apps/v1 
kind: ReplicaSet
metadata:
  name: replicaset-nginx 
spec:
  template: 
    metadata:
      labels:
        app: nginx
    spec: 
      containers:
        - name: nginx
          image: nginx:1.17
  replicas: 3 
  selector:
    matchLabels: 
      app: nginx

'''
$ kubectl apply -f rs-def.yaml 
replicaset.apps/replicaset-nginx created

$ kubectl delete pods replicaset-nginx-z5269
pod "replicaset-nginx-z5269" deleted

$ kubectl get pods
NAME                               READY   STATUS    RESTARTS   AGE
replicaset-nginx-djm67             1/1     Running   0          4s
replicaset-nginx-zhtdd             1/1     Running   0          2m53s
replicaset-nginx-zrqbf             1/1     Running   0          3m8s

$ kubectl get rs
NAME                         DESIRED   CURRENT   READY   AGE
my-web-app-79b94d5979        3         3         3       219d
my-web-app-fbf59755          0         0         0       219d
my-web-app-fd6c96b48         0         0         0       219d
nginx-deployment-fdcbb5f99   2         2         2       173d
replicaset-nginx             3         3         3       5m29s

$ alias k=kubectl

$ k get pods
NAME                               READY   STATUS    RESTARTS   AGE
my-web-app-79b94d5979-hzvvm        1/1     Running   0          124d
my-web-app-79b94d5979-wh7v2        1/1     Running   0          124d
my-web-app-79b94d5979-zdmh2        1/1     Running   0          6m27s
nginx-deployment-fdcbb5f99-4ffxq   1/1     Running   0          5m11s
nginx-deployment-fdcbb5f99-bjv8v   1/1     Running   0          5m
replicaset-nginx-djm67             1/1     Running   0          2m50s
replicaset-nginx-zhtdd             1/1     Running   0          5m39s
replicaset-nginx-zrqbf             1/1     Running   0          5m54s

$ k get pods -o wide
NAME                               READY   STATUS    RESTARTS   AGE     IP             NODE       NOMINATED NODE   READINESS GATES
my-web-app-79b94d5979-hzvvm        1/1     Running   0          124d    10.244.0.91    minikube   <none>           <none>
my-web-app-79b94d5979-wh7v2        1/1     Running   0          124d    10.244.0.90    minikube   <none>           <none>
my-web-app-79b94d5979-zdmh2        1/1     Running   0          7m40s   10.244.0.104   minikube   <none>           <none>
nginx-deployment-fdcbb5f99-4ffxq   1/1     Running   0          6m24s   10.244.0.108   minikube   <none>           <none>
nginx-deployment-fdcbb5f99-bjv8v   1/1     Running   0          6m13s   10.244.0.109   minikube   <none>           <none>
replicaset-nginx-djm67             1/1     Running   0          4m3s    10.244.0.110   minikube   <none>           <none>
replicaset-nginx-zhtdd             1/1     Running   0          6m52s   10.244.0.106   minikube   <none>           <none>
replicaset-nginx-zrqbf             1/1     Running   0          7m7s    10.244.0.105   minikube   <none>     
'''

'''
change the replica: 3 to 4

$ kubectl apply -f rs-def.yaml 
replicaset.apps/replicaset-nginx configured

$ kubectl get pods
NAME                               READY   STATUS    RESTARTS   AGE
my-web-app-79b94d5979-hzvvm        1/1     Running   0          124d
my-web-app-79b94d5979-wh7v2        1/1     Running   0          124d
my-web-app-79b94d5979-zdmh2        1/1     Running   0          11m
nginx-deployment-fdcbb5f99-4ffxq   1/1     Running   0          9m57s
nginx-deployment-fdcbb5f99-bjv8v   1/1     Running   0          9m46s
replicaset-nginx-2mhxv             1/1     Running   0          6s
replicaset-nginx-djm67             1/1     Running   0          7m36s
replicaset-nginx-zhtdd             1/1     Running   0          10m
replicaset-nginx-zrqbf             1/1     Running   0          10m

$ kubectl delete pods replicaset-nginx-zrqbf replicaset-nginx-zhtdd
pod "replicaset-nginx-zrqbf" deleted
pod "replicaset-nginx-zhtdd" deleted

$ kubectl get pods
NAME                               READY   STATUS    RESTARTS   AGE
my-web-app-79b94d5979-hzvvm        1/1     Running   0          124d
my-web-app-79b94d5979-wh7v2        1/1     Running   0          124d
my-web-app-79b94d5979-zdmh2        1/1     Running   0          12m
nginx-deployment-fdcbb5f99-4ffxq   1/1     Running   0          11m
nginx-deployment-fdcbb5f99-bjv8v   1/1     Running   0          10m
replicaset-nginx-2mhxv             1/1     Running   0          73s
replicaset-nginx-6vwtf             1/1     Running   0          4s
replicaset-nginx-djm67             1/1     Running   0          8m43s
replicaset-nginx-nbcbs             1/1     Running   0          4s
'''

'''
# Increas and Decreate the PODs runtime 

$ kubectl scale --replicas=5 -f rs-def.yaml 
replicaset.apps/replicaset-nginx scaled

$ kubectl get pods 
NAME                               READY   STATUS    RESTARTS   AGE
my-web-app-79b94d5979-hzvvm        1/1     Running   0          124d
my-web-app-79b94d5979-wh7v2        1/1     Running   0          124d
my-web-app-79b94d5979-zdmh2        1/1     Running   0          17m
nginx-deployment-fdcbb5f99-4ffxq   1/1     Running   0          16m
nginx-deployment-fdcbb5f99-bjv8v   1/1     Running   0          16m
replicaset-nginx-2mhxv             1/1     Running   0          6m44s
replicaset-nginx-6vwtf             1/1     Running   0          5m35s
replicaset-nginx-djm67             1/1     Running   0          14m
replicaset-nginx-nbcbs             1/1     Running   0          5m35s
replicaset-nginx-wfvdm             1/1     Running   0          6s

$ kubectl scale --replicas=2 -f rs-def.yaml 
replicaset.apps/replicaset-nginx scaled

$ kubectl get pods 
NAME                               READY   STATUS    RESTARTS   AGE
my-web-app-79b94d5979-hzvvm        1/1     Running   0          124d
my-web-app-79b94d5979-wh7v2        1/1     Running   0          124d
my-web-app-79b94d5979-zdmh2        1/1     Running   0          18m
nginx-deployment-fdcbb5f99-4ffxq   1/1     Running   0          17m
nginx-deployment-fdcbb5f99-bjv8v   1/1     Running   0          16m
replicaset-nginx-2mhxv             1/1     Running   0          7m13s
replicaset-nginx-djm67             1/1     Running   0          14m
'''

'''
Official Tutorial

[Link](https://kubernetes.io/docs/reference/kubectl/generated/kubectl_scale/)
'''
---

# Day04-06May24: Deployment

'''
apiVersion: apps/v1 
kind: Deployment 
metadata:
  name: nginx
  namespace: dev
spec:
  template: 
    metadata:
      labels:
        app: nginx
    spec: 
      containers:
        - name: nginx 
          image: nginx:1.17
  strategy:
    type: RollingUpdate
  replicas: 3 
  selector:
    matchLabels: 
      app: nginx
'''

'''
$ kubectl create namespace dev
namespace/dev created

$ kubectl get ns dev
NAME   STATUS   AGE
dev    Active   85s
'''

----

# Day04-06May24: Deployment

'''
apiVersion: apps/v1 
kind: Deployment 
metadata:
  name: nginx
  namespace: dev
spec:
  template: 
    metadata:
      labels:
        app: nginx
    spec: 
      containers:
        - name: nginx 
          image: nginx:1.17
  strategy:
    type: RollingUpdate
  replicas: 3 
  selector:
    matchLabels: 
      app: nginx
'''

[Link](https://codefresh.io/learn/kubernetes-deployment/kubernetes-deployment-examples-create-update-rollback-and-more/)

---

# Day05-09May24: Namespaces

'''
cat backend.yaml

apiVersion: v1
kind: Namespace
metadata:
  name: backend
'''
  
'''
$ kubectl apply -f backend.yaml 
namespace/backend created

$ kubectl get ns --namespace=backend
'''

'''
cat namespace.yaml

apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: backend
  labels:
    name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
'''

'''
$ kubectl get pods --namespace=backend
No resources found in backend namespace.

$ kubectl apply -f namespace.yaml --namespace=backend
pod/nginx created

$ kubectl get pods --namespace=backend
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          37s

$ kubectl get pods --namespace=backend -o wide
NAME    READY   STATUS    RESTARTS   AGE   IP             NODE       NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          41s   10.244.0.121   minikube   <none>           <none>
'''

[Link](https://dev.to/apenney/kubernetes-namespaces-the-ultimate-guide-39ek)


'''
$ kubectl run nginx --image=nginx
pod/nginx created


$ kubectl get pods nginx
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          8s


$ kubectl get pods nginx -o wide
NAME    READY   STATUS    RESTARTS   AGE   IP             NODE       NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          13s   10.244.0.118   minikube   <none>           <none>


$ kubectl run nginx-teamA --image=nginx
The Pod "nginx-teamA" is invalid: 
* metadata.name: Invalid value: "nginx-teamA": a lowercase RFC 1123 subdomain must consist of lower case alphanumeric characters, '-' or '.', and must start and end with an alphanumeric character (e.g. 'example.com', regex used for validation is '[a-z0-9]([-a-z0-9]*[a-z0-9])?(\.[a-z0-9]([-a-z0-9]*[a-z0-9])?)*')
* spec.containers[0].name: Invalid value: "nginx-teamA": a lowercase RFC 1123 label must consist of lower case alphanumeric characters or '-', and must start and end with an alphanumeric character (e.g. 'my-name',  or '123-abc', regex used for validation is '[a-z0-9]([-a-z0-9]*[a-z0-9])?')


$ kubectl get namespace
NAME              STATUS   AGE
argocd            Active   225d
default           Active   226d
dev               Active   46m
ingress-nginx     Active   226d
kube-node-lease   Active   226d
kube-public       Active   226d
kube-system       Active   226d
nginx             Active   181d
todo              Active   181d

$ kubectl get ns
NAME              STATUS   AGE
argocd            Active   225d
default           Active   226d
dev               Active   47m
ingress-nginx     Active   226d
kube-node-lease   Active   226d
kube-public       Active   226d
kube-system       Active   226d
nginx             Active   181d
todo              Active   181d

$ kubectl create ns frontend
namespace/frontend created

$ kubectl get ns frontend
NAME       STATUS   AGE
frontend   Active   14s

$ kubectl run nginx --image=nginx --restart=Never --namespace=frontend
pod/nginx created

$ kubectl get pods --namespace=frontend
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          15s

$ kubectl get pods --namespace=frontend -o wide
NAME    READY   STATUS    RESTARTS   AGE   IP             NODE       NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          18s   10.244.0.119   minikube   <none>           <none>

$ kubectl describe ns frontend
Name:         frontend
Labels:       kubernetes.io/metadata.name=frontend
Annotations:  <none>
Status:       Active

No resource quota.

No LimitRange resource.


$ kubectl get ns frontend
NAME       STATUS   AGE
frontend   Active   4m36s


$ kubectl delete ns frontend
namespace "frontend" deleted
'''

---

# Day06-10May24:

'''
apiVersion: apps/v1
kind: Deployment
metadata:
  name: dep-nginx
  labels:
    app: nginx-app
spec:
  replicas: 4
  selector:
    matchLabels:
      app: nginx-app
  template:
    metadata:
      labels:
        app: nginx-app
    spec:
      containers:
      - name: nginx-app
        image: nginx:1.20.2
        ports:
        - containerPort: 80
'''

[Link](https://codefresh.io/learn/kubernetes-deployment/kubernetes-deployment-examples-create-update-rollback-and-more/)

---

# Day07-14May-24: Services

Refer Documentation

---

# Day08-14May24: Servicers

# Minikube & Docker Installation:

sudo apt-get update
sudo apt-get install -y curl apt-transport-https

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

sudo install minikube-linux-amd64 /usr/local/bin/minikube

sudo apt-get install -y docker.io
sudo systemctl enable docker
sudo systemctl start docker

sudo usermod -aG docker $USER
newgrp docker

minikube start --driver=docker

minikube status

kubectl config use-context minikube
kubectl get nodes

-----

Kubectl Installation: 

https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/

$ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
   
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   138  100   138    0     0   1135      0 --:--:-- --:--:-- --:--:--  1159
100 49.0M  100 49.0M    0     0   107M      0 --:--:-- --:--:-- --:--:--  107M
$ 

$ ls
Javed-KeyPair.pem  kubectl
$ 

$ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   138  100   138    0     0   1348      0 --:--:-- --:--:-- --:--:--  1339
100    64  100    64    0     0    337      0 --:--:-- --:--:-- --:--:--   337

$ ls
Javed-KeyPair.pem  kubectl  kubectl.sha256
$ 

$ echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
kubectl: OK
$ 

$ sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
$ 

$ kubectl version --client
Client Version: v1.30.0
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
$ 

$ kubectl version --client --output=yaml
clientVersion:
  buildDate: "2024-04-17T17:36:05Z"
  compiler: gc
  gitCommit: 7c48c2bd72b9bf5c44d21d7338cc7bea77d0ad2a
  gitTreeState: clean
  gitVersion: v1.30.0
  goVersion: go1.22.2
  major: "1"
  minor: "30"
  platform: linux/amd64
kustomizeVersion: v5.0.4-0.20230601165947-6ce0bf390ce3

-----

Services-Demo:

https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/ - Demo done 

https://semaphoreci.com/blog/kubernetes-ingress - Try from your end

---

# Day09-20May24: ServiceNext

Refer Page 11 

---

# Day10-21May24: Scheduling

Refer Page 12

---

# Day11-23May24: TaintAffinity

Refer Page 13-14

---

# Day12-28May24: BinPacking

Refer Page 15-16

ELK Monitoring Tool

[Link](https://www.digitalocean.com/community/tutorials/how-to-install-elasticsearch-logstash-and-kibana-elastic-stack-on-ubuntu-20-04)

---

# Day13-29May24: DaemonSet

Refer Page 17

'''
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx-daemonset
  labels:
    app: nginx
spec:
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
'''

Creating a Kubernetes DaemonSet ensures that a copy of a pod runs on all (or some) nodes in your cluster. Here’s a step-by-step example of how to create, deploy, and verify a DaemonSet in Kubernetes.

### Step 1: Setup Your Kubernetes Cluster

Ensure you have a running Kubernetes cluster. You can use Minikube, a cloud provider, or any other Kubernetes environment.
Ref to install minikube: [Link](https://minikube.sigs.k8s.io/docs/start/?arch=%2Fmacos%2Fx86-64%2Fstable%2Fbinary+download)

### Step 2: Create a DaemonSet YAML File

Create a YAML file named `daemonset-example.yaml` with the following content. This example deploys a simple Nginx web server on all nodes.

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx-daemonset
  labels:
    app: nginx
spec:
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

### Step 3: Apply the DaemonSet Configuration

Deploy the DaemonSet to your Kubernetes cluster using the `kubectl apply` command:

```bash
kubectl apply -f daemonset-example.yaml
```

### Step 4: Verify the DaemonSet

Check the status of the DaemonSet to ensure it's running correctly:

```bash
kubectl get daemonsets
```

You should see output similar to:

```
NAME              DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
nginx-daemonset   1         1         1       1            1           <none>          14s
```

This output indicates that the DaemonSet is running on all three nodes.

### Step 5: Verify the Pods

List all pods to see the ones created by the DaemonSet:

```bash
kubectl get pods -o wide
```

You should see output showing that there is one Nginx pod running on each node:

```
NAME                    READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
nginx-daemonset-fztcq   1/1     Running   0          72s   10.244.0.4   minikube   <none>           <none>
```

### Step 6: Accessing the Nginx Pods

Since DaemonSets usually run on each node, you can access the Nginx server on each node’s IP address. Use `kubectl port-forward` to access one of the pods:

```bash
POD_NAME=$(kubectl get pods -l app=nginx -o jsonpath="{.items[0].metadata.name}")
kubectl port-forward $POD_NAME 8080:80
```

Now you can access the Nginx web server at `http://localhost:8080`.

### Step 7: Clean Up

When you’re done, you can delete the DaemonSet with:

```bash
kubectl delete -f daemonset-example.yaml
```

### Conclusion

This example demonstrates how to create and manage a DaemonSet in Kubernetes. DaemonSets are particularly useful for running system daemons and monitoring agents across all nodes in a cluster.

---

# The differences between HPA and VPA in Kubernetes:

HPA scales by adding or removing pods, thus scaling capacity horizontally.
VPA scales by increasing or decreasing CPU and memory resources within the existing pod containers, thus scaling capacity vertically.

HPA adjusts the number of replicas of an application.
VPA adjusts the resource requests and limits of a container.

HPA works on the pod and cluster levels.
VPA works on the container level.

---

# Day14-30May24: SecretsSidecarContainer

Refer Page 18-19

---

# Day15-31May24: SideCarContainer

Refer Page 20

[Link](https://tecadmin.net/crontab-in-linux-with-20-examples-of-cron-schedule/)

---

# Day16-03June24: RollingUpdateRollback

Refer Page 21

---

# Day17-04June24: SelfHealing-NodeMaintenance

Refer Page 22-23

---

# Day18-05June24: ClusterUpgrade

Refer Page 24

---

# Day19-10June24: BackupandSecurity

Refer Page 25-26

---

# Day20-11June24: AuthenticationAuthorization

Refer Page 27

---

# Day21-13June24: ServiceAccount

Refer Page 28

---

# Day22-21June24: APIKubeConfig

Refer Page 29-30

---

# Day23-24June: TrivyAuditing

Refer Page 31-32

---

# Day24-26June: Storage

Refer Page 33-34

---

# Day25-28June24: StatefulSet

Refer Page 35-36

---

# Day26-02July24: StorageDeployments

Refer Page 37-38

---

# Day27-03July24: K8sCheatSheet & Questions

Refer cheatsheet and questionanswers pdf

---

# Day27a-03July24: helm

Refer Page 38-39

[Link](https://helm.sh/docs/)

[Link](https://aakibkhan1.medium.com/helm-the-ship-controller-be5dcf9395b0)

---

# Day28-04July24: Ingress

Refer Page 40

---

# Day29-08July24: TLS

Refer Page 41-42

---

# Day30-11July24: minikube-Lab1

[Link](https://medium.com/@javedalam1303/install-minikube-on-ubuntu-22-04-ec2-9afe7ca51fdd)

[LInk](https://devopscube.com/kubernetes-minikube-tutorial/)

[Link](https://minikube.sigs.k8s.io/docs/)

---

# Day31-12July24: EKS-Lab2

# Pre-Requisites

Install AWS CLI and Configure

Command to execute: 
$ aws configure 

```
$ aws configure 
AWS Access Key ID [****************5V7G]: 
AWS Secret Access Key [****************MF1Y]: 
Default region name [us-west-2]:          
Default output format [json]:       
```

----

$ aws s3 ls 
2023-07-06 10:39:17 knowledgeadda1
2023-07-01 14:17:49 learnzone
2023-08-09 16:08:34 unique-bucket-09082023

----

AWS CLI to create EKS Cluster: [Link](https://docs.aws.amazon.com/cli/latest/reference/eks/create-cluster.html)

----

[Reference Link](https://github.com/dche25/EKS-Cluster-Setup/blob/main/eks-setup)

# Create EKS Cluster with Node Groups

## Step-00: Introduction
- Understand about EKS Core Objects
  - Control Plane
  - Worker Nodes & Node Groups
  - Fargate Profiles
  - VPC
- Create EKS Cluster
- Associate EKS Cluster to IAM OIDC Provider
- Create EKS Node Groups
- Verify Cluster, Node Groups, EC2 Instances, IAM Policies and Node Groups


## Step-01: Create EKS Cluster using eksctl
- It will take 15 to 20 minutes to create the Cluster Control Plane 


# Create Cluster
eksctl create cluster --name=myeks \
                      --region=us-east-1 \
                      --zones=us-east-1a,us-east-1b \
                      --without-nodegroup 


```
$ eksctl create cluster --name=myeks \
>                       --region=us-east-1 \
>                       --zones=us-east-1a,us-east-1b \
>                       --without-nodegroup 
2023-08-20 16:25:32 [ℹ]  eksctl version 0.153.0
2023-08-20 16:25:32 [ℹ]  using region us-east-1
2023-08-20 16:25:32 [ℹ]  subnets for us-east-1a - public:192.168.0.0/19 private:192.168.64.0/19
2023-08-20 16:25:32 [ℹ]  subnets for us-east-1b - public:192.168.32.0/19 private:192.168.96.0/19
2023-08-20 16:25:32 [ℹ]  using Kubernetes version 1.25
2023-08-20 16:25:32 [ℹ]  creating EKS cluster "myeks" in "us-east-1" region with 
2023-08-20 16:25:32 [ℹ]  if you encounter any issues, check CloudFormation console or try 'eksctl utils describe-stacks --region=us-east-1 --cluster=myeks'
2023-08-20 16:25:32 [ℹ]  Kubernetes API endpoint access will use default of {publicAccess=true, privateAccess=false} for cluster "myeks" in "us-east-1"
2023-08-20 16:25:32 [ℹ]  CloudWatch logging will not be enabled for cluster "myeks" in "us-east-1"
2023-08-20 16:25:32 [ℹ]  you can enable it with 'eksctl utils update-cluster-logging --enable-types={SPECIFY-YOUR-LOG-TYPES-HERE (e.g. all)} --region=us-east-1 --cluster=myeks'
2023-08-20 16:25:32 [ℹ]  
2 sequential tasks: { create cluster control plane "myeks", wait for control plane to become ready 
}
2023-08-20 16:25:32 [ℹ]  building cluster stack "eksctl-myeks-cluster"
2023-08-20 16:25:34 [ℹ]  deploying stack "eksctl-myeks-cluster"
2023-08-20 16:26:04 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-cluster"
2023-08-20 16:26:35 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-cluster"
2023-08-20 16:27:37 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-cluster"
2023-08-20 16:28:38 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-cluster"
2023-08-20 16:29:40 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-cluster"
2023-08-20 16:30:41 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-cluster"
2023-08-20 16:31:42 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-cluster"
2023-08-20 16:32:43 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-cluster"
2023-08-20 16:33:44 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-cluster"
2023-08-20 16:34:45 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-cluster"
2023-08-20 16:35:46 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-cluster"
2023-08-20 16:37:54 [ℹ]  waiting for the control plane to become ready
2023-08-20 16:37:55 [✔]  saved kubeconfig as "/Users/javedalam/.kube/config"
2023-08-20 16:37:55 [ℹ]  no tasks
2023-08-20 16:37:55 [✔]  all EKS cluster resources for "myeks" have been created
2023-08-20 16:37:58 [ℹ]  kubectl command should work with "/Users/javedalam/.kube/config", try 'kubectl get nodes'
2023-08-20 16:37:58 [✔]  EKS cluster "myeks" in "us-east-1" region is ready
```

# What happens at the backend

In the provided `eksctl` command, you are creating an Amazon EKS cluster named "myeks" in the "us-east-1" region, spanning across availability zones "us-east-1a" and "us-east-1b", and you are using the `--without-nodegroup` flag, which means you are not creating a nodegroup as part of the cluster creation process.

Here's what happens at the backend when you run this `eksctl` command:

1. **Cluster Creation**: The `eksctl` tool communicates with the Amazon EKS service using the AWS API to initiate the creation of a new EKS cluster named "myeks" in the "us-east-1" region.

2. **Networking**: The EKS control plane infrastructure is provisioned, including networking components like the Amazon VPC (Virtual Private Cloud), subnets, and security groups. The control plane is the management plane of the EKS cluster.

3. **Availability Zones**: The specified availability zones, "us-east-1a" and "us-east-1b", are used to distribute the control plane's components across different data centers for high availability.

4. **Nodegroup (Skipped in Your Command)**: Since you are using the `--without-nodegroup` flag, a nodegroup (a set of worker nodes) is not created as part of the cluster creation. This means that the EKS cluster will not have any worker nodes initially. You can add worker nodes later using the `eksctl create nodegroup` command.

5. **Kubeconfig**: After the cluster is created, `eksctl` configures `kubectl` to work with the new EKS cluster by updating the kubeconfig file on your local machine. This allows you to interact with the cluster using `kubectl`.

6. **Cluster Information**: The `eksctl` command provides output that includes essential information about the created EKS cluster, such as the cluster name, region, control plane endpoint, and more.

Remember that without worker nodes (nodegroup), the cluster is not yet capable of running Kubernetes workloads. You'll need to add a nodegroup with worker nodes to start deploying and managing applications on the EKS cluster.

It's important to note that EKS cluster creation also involves many other steps, such as IAM role creation, setting up authentication and authorization, and provisioning security configurations. `eksctl` abstracts away much of the complexity by providing a simplified command-line interface for creating and managing EKS clusters.


# aws cli commands to verify the above created cluster

After creating an Amazon EKS cluster using the `eksctl` command, you can use the AWS CLI to verify the cluster's status and get additional information about it. Here are some AWS CLI commands you can use:

1. **List Clusters**:
   To list all EKS clusters in your AWS account, you can use the following command:

   ```bash
   aws eks list-clusters

   $ aws eks list-clusters
    {
    "clusters": [
        "myeks"
    ]
    }
   ```

   This will provide a list of EKS cluster names.

2. **Describe Cluster**:
   To get detailed information about a specific EKS cluster, you can use the `describe-cluster` command with the cluster name:

   ```bash
   aws eks describe-cluster --name myeks

   $ aws eks describe-cluster --name myeks 
    {
        "cluster": {
            "name": "myeks",
    .....
        }
    }
   ```

   Replace `myeks` with your actual cluster name.

3. **Get Token for `kubectl`**:
   To configure `kubectl` to work with the newly created EKS cluster, you can use the following command:

   ```bash
   aws eks update-kubeconfig --name myeks
   ```

   This command updates your local `kubectl` configuration with the credentials and cluster information.

4. **List Nodegroups**:
   To list the nodegroups associated with your EKS cluster, you can use the following command:

   ```bash
   aws eks list-nodegroups --cluster-name myeks

   $ aws eks list-nodegroups --cluster-name myeks
    {
        "nodegroups": []
    }
   ```

   Replace `myeks` with your actual cluster name.

5. **Describe Nodegroup**:
   If you later create a nodegroup for your cluster, you can get detailed information about the nodegroup using the `describe-nodegroup` command:

   ```bash
   aws eks describe-nodegroup --cluster-name myeks --nodegroup-name my-nodegroup
   ```

   Replace `myeks` with your cluster name and `my-nodegroup` with your nodegroup name.

These AWS CLI commands will help you verify the status and details of your EKS cluster, as well as any associated nodegroups. Remember to replace placeholders like `myeks` and `my-nodegroup` with the actual names you used during the cluster and nodegroup creation.


# Note: Above CLI commans doesn't create any nodegroup so, you can skip few commands 


# How to delete the above myeks created cluster

To delete an Amazon EKS cluster using the AWS CLI, you can use the `eksctl` tool or directly use the AWS CLI. Here's how to delete the cluster "myeks" using both methods:

Using `eksctl`:

```bash
eksctl delete cluster --name myeks
```

Using AWS CLI directly:

```bash
aws eks delete-cluster --name myeks
```

Both commands will initiate the process of deleting the EKS cluster named "myeks". The cluster, its associated resources (such as worker nodes), and control plane components will be removed. Please note that the deletion process might take some time to complete.

Remember that deleting an EKS cluster is a permanent action and will result in the loss of all data and configurations associated with the cluster. Make sure you have backups or copies of any important data before proceeding with the deletion.

---

# Day32-15July24: 3NodeK8s-Lab3

# Create 3 Instances from AWS 

# 3 Node Cluster SetUp

[Link](https://k21academy.com/docker-kubernetes/three-node-kubernetes-cluster/)

# Docker Installation

[Link](https://medium.com/@javedalam1303/install-minikube-on-ubuntu-22-04-ec2-9afe7ca51fdd0)

---

# Day33-23July24: K8s

'''
$ mkdir docker

$ cd docker/

$ git clone https://github.com/komal-max/Build-and-Push-Your-Own-Docker-Images.git
Cloning into 'Build-and-Push-Your-Own-Docker-Images'...
remote: Enumerating objects: 20, done.
remote: Counting objects: 100% (20/20), done.
remote: Compressing objects: 100% (15/15), done.
remote: Total 20 (delta 4), reused 17 (delta 3), pack-reused 0
Receiving objects: 100% (20/20), 74.58 KiB | 14.92 MiB/s, done.
Resolving deltas: 100% (4/4), done.

$ cd Build-and-Push-Your-Own-Docker-Images/

$ cat Dockerfile 
FROM nginx:latest 
# use tag alpine to have a smaller image size 
# our base image is based on nginx and that is coming from dockerhub

COPY source/html /usr/share/nginx/html 
# Takes two parameters, source - copy what? and destination - copy where? 

# expose port, port 80 is used by default by nginx
#EXPOSE 80, we can comment it out

CMD ["nginx", "-g","daemon off;"]
# this is the same command that nginx image passes in when starts up by default
#we can comment out this line too if we like


$ docker build -t docker-build .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  226.8kB
Step 1/3 : FROM nginx:latest
latest: Pulling from library/nginx
f11c1adaa26e: Pull complete 
c6b156574604: Pull complete 
ea5d7144c337: Pull complete 
1bbcb9df2c93: Pull complete 
537a6cfe3404: Pull complete 
767bff2cc03e: Pull complete 
adc73cb74f25: Pull complete 
Digest: sha256:67682bda769fae1ccf5183192b8daf37b64cae99c6c3302650f6f8bf5f0f95df
Status: Downloaded newer image for nginx:latest
 ---> fffffc90d343
Step 2/3 : COPY source/html /usr/share/nginx/html
 ---> 585b211a1b11
Step 3/3 : CMD ["nginx", "-g","daemon off;"]
 ---> Running in c541e0c4de95
Removing intermediate container c541e0c4de95
 ---> fd7ed64030a3
Successfully built fd7ed64030a3
Successfully tagged docker-build:latest


$ docker images
REPOSITORY                    TAG       IMAGE ID       CREATED          SIZE
docker-build                  latest    fd7ed64030a3   16 seconds ago   188MB

$ docker run -d -p 80:80 fd7ed64030a3
6893082751ca4e0b237a31d36a2e2367a2e91221e4312292262641e5392342b4
'''

# Access

'''
http://localhost:80
'''

# Create a docker hub account

'''Login to docker hub and create a new repository'''

'''
$ docker images
REPOSITORY                    TAG       IMAGE ID       CREATED         SIZE
docker-build                  latest    fd7ed64030a3   6 minutes ago   188MB


$ docker tag docker-build:latest 782782/docker-images:0.2


$ docker login
Log in with your Docker ID or email address to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com/ to create one.
You can log in with your password or a Personal Access Token (PAT). Using a limited-scope PAT grants better security and is required for organizations using SSO. Learn more at https://docs.docker.com/go/access-tokens/

Username: 782782
Password: 
WARNING! Your password will be stored unencrypted in /home/ubuntu/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
'''

# Push the local image to remote(docker hub), here repo created by own 
'''
$ docker push 782782/docker-images:0.2 
The push refers to repository [docker.io/782782/docker-images]
deaa280ccea4: Pushed 
56b6d3be75f9: Mounted from library/nginx 
0c6c257920c8: Mounted from library/nginx 
92d0d4e97019: Mounted from library/nginx 
7190c87a0e8a: Mounted from library/nginx 
933a3ce2c78a: Mounted from library/nginx 
32cfaf91376f: Mounted from library/nginx 
32148f9f6c5a: Mounted from library/nginx 
0.2: digest: sha256:11cdcbfd878978749620accc6827031ec6156787e46c3c8d363f495ef96c0916 size: 1987
'''

---

# Launch an EC2 Machine from AWS Console with below pre-requisites

'''
2 CPUs or more
2GB of free memory
20GB of free disk space
Internet connection
'''

'''
sudo apt update -y && sudo apt upgrade -y
'''

# Install Docker
'''
$ cat docker.sh 
sudo apt install docker.io
sudo usermod -aG docker $USER && newgrp docker
sudo apt install -y curl wget apt-transport-https

$ docker version
Client:
 Version:           24.0.7
 API version:       1.43
 Go version:        go1.22.2
 Git commit:        24.0.7-0ubuntu4
 Built:             Wed Apr 17 20:08:25 2024
 OS/Arch:           linux/amd64
 Context:           default

Server:
 Engine:
  Version:          24.0.7
  API version:      1.43 (minimum version 1.12)
  Go version:       go1.22.2
  Git commit:       24.0.7-0ubuntu4
  Built:            Wed Apr 17 20:08:25 2024
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.7.12
  GitCommit:        
 runc:
  Version:          1.1.12-0ubuntu3
  GitCommit:        
 docker-init:
  Version:          0.19.0
  GitCommit:        
'''

# Install minikube

'''
$ cat minikube.sh 
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

minikube version
minikube version: v1.33.1
commit: 5883c09216182566a63dff4c326a6fc9ed2982ff
'''

# Install kubectl
'''
$ cat kubectl.sh 
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x kubectl
sudo mv kubectl /usr/local/bin

kubectl version -o yaml
clientVersion:
  buildDate: "2024-07-16T23:54:40Z"
  compiler: gc
  gitCommit: 6fc0a69044f1ac4c13841ec4391224a2df241460
  gitTreeState: clean
  gitVersion: v1.30.3
  goVersion: go1.22.5
  major: "1"
  minor: "30"
  platform: linux/amd64
kustomizeVersion: v5.0.4-0.20230601165947-6ce0bf390ce3

The connection to the server localhost:8080 was refused - did you specify the right host or port?

kubectl version
Client Version: v1.30.3
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
The connection to the server localhost:8080 was refused - did you specify the right host or port?
'''

# Start minikube
'''
$ minikube start - vm-driver=docker
😄  minikube v1.33.1 on Ubuntu 24.04 (xen/amd64)
✨  Automatically selected the docker driver. Other choices: none, ssh
📌  Using Docker driver with root privileges
👍  Starting "minikube" primary control-plane node in "minikube" cluster
🚜  Pulling base image v0.0.44 ...
💾  Downloading Kubernetes v1.30.0 preload ...
    > preloaded-images-k8s-v18-v1...:  342.90 MiB / 342.90 MiB  100.00% 58.70 M
    > gcr.io/k8s-minikube/kicbase...:  481.55 MiB / 481.58 MiB  99.99% 67.45 Mi
🔥  Creating docker container (CPUs=2, Memory=2200MB) ...
🐳  Preparing Kubernetes v1.30.0 on Docker 26.1.1 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔗  Configuring bridge CNI (Container Networking Interface) ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: default-storageclass, storage-provisioner
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default

$ minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
'''

'''
$ kubectl get nodes
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   48s   v1.30.0
'''

# Create a deployment file
'''
$ cat pod.yaml 
kind: Pod
apiVersion: v1
metadata:
 name: minikubek8s
spec:
 containers:
 - name: container1
   image: ubuntu
   command: ["/bin/bash","-c","while true; do echo Hello-container1-Kubernetes; sleep 5 ; done"]
 - name: container2
   image: ubuntu
   command: ["/bin/bash","-c","while true; do echo Hello-container1-Kubernetes; sleep 3 ; done"]
 restartPolicy: Never
'''

'''
$ kubectl apply -f pod.yaml 
pod/minikubek8s created

$ kubectl get pods -o wide
NAME          READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
minikubek8s   2/2     Running   0          32s   10.244.0.3   minikube   <none>           <none>

$ kubectl logs -f minikubek8s
Defaulted container "container1" out of: container1, container2
Hello-container1-Kubernetes
Hello-container1-Kubernetes
Hello-container1-Kubernetes
Hello-container1-Kubernetes

$ kubectl logs -f minikubek8s -c container1
Hello-container1-Kubernetes
Hello-container1-Kubernetes
Hello-container1-Kubernetes
Hello-container1-Kubernetes
Hello-container1-Kubernetes

$ kubectl logs -f minikubek8s -c container2
Hello-container1-Kubernetes
Hello-container1-Kubernetes
Hello-container1-Kubernetes
Hello-container1-Kubernetes
Hello-container1-Kubernetes

$ kubectl exec -it minikubek8s -c container1 -- hostname -i
10.244.0.3

$ kubectl exec -it minikubek8s -c container2 -- hostname -i
10.244.0.3

$ kubectl exec -it minikubek8s -c container2 /bin/bash
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
root@minikubek8s:/# 
root@minikubek8s:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@minikubek8s:/# 
root@minikubek8s:/# exit
exit
command terminated with exit code 127
'''

---

# Day34-25July24: K8sDocker-Lab5

[Link](https://docs.docker.com/guides/workshop/02_our_app/)

[Link](https://docs.docker.com/guides/workshop/03_updating_app/)

[Link](https://docs.docker.com/guides/workshop/04_sharing_app/)

[Link](https://docs.docker.com/guides/workshop/05_persisting_data/)

[Link](https://docs.docker.com/guides/workshop/06_bind_mounts/)


# Docker Installation
[Link](https://docs.docker.com/desktop/install/mac-install/)

---

# Day35-26July24: k8sDocker-Lab6

# For cleaning

'''
$ docker system prune 
WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all dangling images
  - all dangling build cache

Are you sure you want to continue? [y/N] y

$ docker volume prune

$ docker network prune
'''

# Remove all the stopped containers

'''
$ docker rm -aq $(docker ps -a)
'''

# Development containers

[Link](https://docs.docker.com/guides/workshop/06_bind_mounts/)

# Bridge network

[Link](https://docs.docker.com/network/drivers/bridge/)

# Storage

[Link](https://docs.docker.com/storage/)

# Multi container apps

[Link](https://docs.docker.com/guides/workshop/07_multi_container/)

# Docker compose

[Link](https://docs.docker.com/guides/workshop/08_using_compose/)

# Multi Stage builds

[Links](https://docs.docker.com/guides/workshop/09_image_best/)

---

# Day36-29July24: ArgoCD-Lab7

# Fork to GitHub repo

'''
https://github.com/xavyaly/blue-green
'''

# Install aws cli 

[Link](https://medium.com/@javedalam1303/install-awscli-2ea38c4873c3)

'''
aws --version
aws-cli/2.17.18 Python/3.11.9 Linux/6.8.0-1011-aws exe/x86_64.ubuntu.24
'''

# To configure aws cli and cross check 

$ aws configure 
AWS Access Key ID [****************5V7G]: 
AWS Secret Access Key [****************MF1Y]: 
Default region name [us-west-2]: 
Default output format [json]: 

'''
cat ~/.aws/config
cat ~/.aws/credentials

aws s3 ls # List all the existing buckets in AWS Account
'''

# Install kubectl 

[Link](https://k8s-docs.netlify.app/en/docs/tasks/tools/install-kubectl/)

'''
kubectl version --client
Client Version: v1.30.3
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
'''

# Install terraform

[Link](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

'''
terraform version
Terraform v1.9.3
on linux_amd64
'''

# Clone the fork repo

'''
git clone https://github.com/xavyaly/blue-green.git

cd blue-green/
ls
README.md  blue  eks-cluster.tf  eks-out.tf  eks-vpc.tf  eks-worker-nodes.tf  green  vars.tf  version.tf
'''

# Create an EKS Cluster with the help of terraform code 

'''
$ terraform init

$ ls  .terraform/providers/registry.terraform.io/hashicorp/aws/5.60.0/linux_amd64/
LICENSE.txt  terraform-provider-aws_v5.60.0_x5

$ terraform plan 

Plan: 20 to add, 0 to change, 0 to destroy.

$ terraform apply 
OR
$ terraform apply --auto-approve 

Apply complete! Resources: 20 added, 0 changed, 0 destroyed.

'''

# To cross check the EKS cluster using CLI

'''
aws eks list-clusters --region ap-south-1
{
    "clusters": [
        "eks_cluster_demo"
    ]
}
'''

# Update the kubeconfig

'''
aws eks --region ap-south-1 update-kubeconfig --name eks_cluster_demo
Updated context arn:aws:eks:ap-south-1:252473277340:cluster/eks_cluster_demo in /home/ubuntu/.kube/config
'''

# If you want to remove the EKS cluster using terraform

'''
$ terraform destroy --auto-approve

Destroy complete! Resources: 20 destroyed.
'''

# Day37 

# Namespaces

'''
kubectl get ns
NAME              STATUS   AGE
default           Active   22m
kube-node-lease   Active   22m
kube-public       Active   22m
kube-system       Active   22m

kubectl  create namespace argocd
namespace/argocd created

kubectl get ns
NAME              STATUS   AGE
argocd            Active   5s
'''

# Install argoCD

'''
cat argocd.sh 
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

chmod +x argocd.sh

./argocd.sh
'''

# List all

'''
kubectl get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   172.20.0.1   <none>        443/TCP   26m

kubectl get all -n argocd
NAME                                                    READY   STATUS    RESTARTS   AGE
pod/argocd-application-controller-0                     1/1     Running   0          64s
pod/argocd-applicationset-controller-8485455fd5-7grqh   1/1     Running   0          67s
pod/argocd-dex-server-66779d96df-bzr9h                  1/1     Running   0          67s
pod/argocd-notifications-controller-c4b69fb67-9nbw4     1/1     Running   0          66s
pod/argocd-redis-7bf7cb9748-vqxn6                       1/1     Running   0          65s
pod/argocd-repo-server-795d79dfb6-4s82x                 1/1     Running   0          65s
pod/argocd-server-544b7f897d-wftlr                      1/1     Running   0          64s

NAME                                              TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
service/argocd-applicationset-controller          ClusterIP   172.20.30.70     <none>        7000/TCP,8080/TCP            73s
service/argocd-dex-server                         ClusterIP   172.20.188.250   <none>        5556/TCP,5557/TCP,5558/TCP   72s
service/argocd-metrics                            ClusterIP   172.20.254.148   <none>        8082/TCP                     72s
service/argocd-notifications-controller-metrics   ClusterIP   172.20.213.209   <none>        9001/TCP                     71s
service/argocd-redis                              ClusterIP   172.20.72.194    <none>        6379/TCP                     71s
service/argocd-repo-server                        ClusterIP   172.20.48.110    <none>        8081/TCP,8084/TCP            70s
service/argocd-server                             ClusterIP   172.20.208.215   <none>        80/TCP,443/TCP               69s
service/argocd-server-metrics                     ClusterIP   172.20.53.90     <none>        8083/TCP                     69s

NAME                                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/argocd-applicationset-controller   1/1     1            1           68s
deployment.apps/argocd-dex-server                  1/1     1            1           68s
deployment.apps/argocd-notifications-controller    1/1     1            1           67s
deployment.apps/argocd-redis                       1/1     1            1           67s
deployment.apps/argocd-repo-server                 1/1     1            1           66s
deployment.apps/argocd-server                      1/1     1            1           65s

NAME                                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/argocd-applicationset-controller-8485455fd5   1         1         1       69s
replicaset.apps/argocd-dex-server-66779d96df                  1         1         1       69s
replicaset.apps/argocd-notifications-controller-c4b69fb67     1         1         1       68s
replicaset.apps/argocd-redis-7bf7cb9748                       1         1         1       68s
replicaset.apps/argocd-repo-server-795d79dfb6                 1         1         1       67s
replicaset.apps/argocd-server-544b7f897d                      1         1         1       66s

NAME                                             READY   AGE
statefulset.apps/argocd-application-controller   1/1     66s


kubectl get svc -n argocd
NAME                                      TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
argocd-applicationset-controller          ClusterIP   172.20.30.70     <none>        7000/TCP,8080/TCP            2m46s
argocd-dex-server                         ClusterIP   172.20.188.250   <none>        5556/TCP,5557/TCP,5558/TCP   2m45s
argocd-metrics                            ClusterIP   172.20.254.148   <none>        8082/TCP                     2m45s
argocd-notifications-controller-metrics   ClusterIP   172.20.213.209   <none>        9001/TCP                     2m44s
argocd-redis                              ClusterIP   172.20.72.194    <none>        6379/TCP                     2m44s
argocd-repo-server                        ClusterIP   172.20.48.110    <none>        8081/TCP,8084/TCP            2m43s
argocd-server                             ClusterIP   172.20.208.215   <none>        80/TCP,443/TCP               2m42s
argocd-server-metrics                     ClusterIP   172.20.53.90     <none>        8083/TCP                     2m42s
'''

# Note

'''
Agrocd-server service is mapping to “ClusterIP”. We need to change it to “LoadBalancer” or
“NodePort” to access the agrocd UI from outside world.

kubectl patch svc argocd-server -n argocd -p '{"spec": {"type":"LoadBalancer"}}'
service/argocd-server patched

kubectl get svc -n argocd
NAME                                      TYPE           CLUSTER-IP       EXTERNAL-IP                                                                PORT(S)                      AGE
argocd-applicationset-controller          ClusterIP      172.20.30.70     <none>                                                                     7000/TCP,8080/TCP            4m33s
argocd-dex-server                         ClusterIP      172.20.188.250   <none>                                                                     5556/TCP,5557/TCP,5558/TCP   4m32s
argocd-metrics                            ClusterIP      172.20.254.148   <none>                                                                     8082/TCP                     4m32s
argocd-notifications-controller-metrics   ClusterIP      172.20.213.209   <none>                                                                     9001/TCP                     4m31s
argocd-redis                              ClusterIP      172.20.72.194    <none>                                                                     6379/TCP                     4m31s
argocd-repo-server                        ClusterIP      172.20.48.110    <none>                                                                     8081/TCP,8084/TCP            4m30s
argocd-server                             LoadBalancer   172.20.208.215   a510c2a7743ba4d5e8a6b985e934728d-1187100072.ap-south-1.elb.amazonaws.com   80:31348/TCP,443:31646/TCP   4m29s
argocd-server-metrics                     ClusterIP      172.20.53.90     <none>                                                                     8083/TCP                     4m29s

kubectl get svc argocd-server -n argocd
NAME            TYPE           CLUSTER-IP       EXTERNAL-IP                                                                PORT(S)                      AGE
argocd-server   LoadBalancer   172.20.208.215   a510c2a7743ba4d5e8a6b985e934728d-1187100072.ap-south-1.elb.amazonaws.com   80:31348/TCP,443:31646/TCP   5m35s
'''

# Now we can login to argocd using Loadbalancer.

'''
URL: https://a510c2a7743ba4d5e8a6b985e934728d-1187100072.ap-south-1.elb.amazonaws.com

Creds: admin/58Nk5a2gAfp***u # Refer below
'''

# Fetch the password 

'''
kubectl get secret -n argocd
NAME                          TYPE     DATA   AGE
argocd-initial-admin-secret   Opaque   1      8m5s
argocd-notifications-secret   Opaque   0      8m25s
argocd-redis                  Opaque   1      8m7s
argocd-secret                 Opaque   5      8m25s

kubectl get secret -n argocd argocd-initial-admin-secret -o yaml
apiVersion: v1
data:
  password: NThOazVhMmdBZnAzcnh***==
kind: Secret
metadata:
  creationTimestamp: "2024-07-30T03:15:29Z"
  name: argocd-initial-admin-secret
  namespace: argocd
  resourceVersion: "4851"
  uid: 56f77a7a-e7d5-4ad1-8ff7-c37e7f949ff4
type: Opaque

echo "NThOazVhMmdBZnAzcnhFdQ==" | base64 -d
58Nk5a2gAfp***u
'''

# Install argocd cli

'''
cat argocdcli.sh 
sudo curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo chmod +x /usr/local/bin/argocd

./argocdcli.sh

argocd version
argocd: v2.11.7+e4a0246
  BuildDate: 2024-07-24T10:10:59Z
  GitCommit: e4a0246c4d920bc1e5ee5f9048a99eca7e1d53cb
  GitTreeState: clean
  GoVersion: go1.21.12
  Compiler: gc
  Platform: linux/amd64
FATA[0000] Failed to establish connection to a21518392c4a24591aee5fe3f2aee6ae-1593474350.ap-south-1.elb.amazonaws.com:443: dial tcp: lookup a21518392c4a24591aee5fe3f2aee6ae-1593474350.ap-south-1.elb.amazonaws.com on 127.0.0.53:53: no such host
'''

# Login to argocd cli

'''
argocd login $(kubectl get service argocd-server -n argocd --output=jsonpath='{.status.loadBalancer.ingress[0].hostname}') --username admin --password $(kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo) --insecure
'admin:login' logged in successfully
Context 'a510c2a7743ba4d5e8a6b985e934728d-1187100072.ap-south-1.elb.amazonaws.com' updated
'''

# Depoy one python flask application 

# Once deployed from argoCD UI

'''
kubectl get deploy
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
python-flask   1/1     1            1           2m25s

kubectl get svc 
NAME          TYPE           CLUSTER-IP      EXTERNAL-IP                                                               PORT(S)          AGE
kubernetes    ClusterIP      172.20.0.1      <none>                                                                    443/TCP          50m
python-load   LoadBalancer   172.20.218.39   abcab9d6de1cc4b56b4a9b3ba5b55811-230405789.ap-south-1.elb.amazonaws.com   5001:32287/TCP   2m43s
'''

# Fetch the UI 

'''
http://abcab9d6de1cc4b56b4a9b3ba5b55811-230405789.ap-south-1.elb.amazonaws.com:5001

welcome to the flask tutorials updated
'''

# Now we can update our python app and create new image and push to dockerhub. 

# Update that image details in our deploy.yml file in our repo and click on sync in app.

# Apply the changes on demo.py 

'''
cat flask-demo/demo.py 
from flask import Flask 
app = Flask(__name__) 
  
@app.route('/') 
def hello(): 
    return "welcome to the flask tutorials updated..."
  
  
if __name__ == "__main__": 
    app.run(host ='0.0.0.0', port = 5001, debug = True)
'''

# Apply the changes on deploy.yaml 

'''
image: 782782/python-flask:0.1
'''

# Push the changes to GitHub

'''
git branch 
git status
git add demo.py deploy.yaml
git commit -m "<msg>"
git push 
'''

# Build a new images, tag and push to Docker Hub

'''
docker build -t 782782/python-flask:0.1 flask-demo/.

docker images
REPOSITORY            TAG         IMAGE ID       CREATED          SIZE
782782/python-flask   0.1         6524a26d3686   28 seconds ago   92.4MB

docker push 782782/python-flask:0.1
'''

# Visit argocd UI and click on sync 

# Changes will reflect accordingly

'''
http://abcab9d6de1cc4b56b4a9b3ba5b55811-230405789.ap-south-1.elb.amazonaws.com:5001
'''

---

# Day37-07Aug24: Labels-Lab8

Kubernetes Labels, Selectors, and Node Selectors

Labels are key-value pairs attached to Kubernetes objects like pods, services, and deployments. They provide a way to categorize and organize objects based on attributes that are meaningful and relevant to users.

Selectors allow you to filter Kubernetes resources based on their labels. There are two types of selectors:

1. Equality-based Selectors: These are simple checks like `key = value` or `key != value`.
2. Set-based Selectors: These allow you to check if a key's value is in a set of values (`in`) or not in a set of values (`notin`).

Node Selectors are a simple way to constrain pods to only be scheduled on nodes with specific labels. They are used to ensure that certain pods are only deployed on nodes with certain characteristics.

Example: Using Labels, Selectors, and Node Selectors

# Pre-Requisites

# Create an EC2 Instance

# Download and start minikube
'''
Refer installation repo
'''

# Install kubectl
'''
Refer installation repo
'''

# Install docker
'''
Refer installation repo
'''

# Create a POD: pod.yaml

'''
cat pod.yaml 
apiVersion: v1
kind: Pod
metadata:
  name: aug
  labels:
    env: production
    department: sre
spec:
  containers:
    - name: sre-cs
      image: ubuntu
      command: ["/bin/bash", "-c", "while true; do echo This is aug of Kubernetes; sleep 5 ; done"]
'''

'''
kubectl apply -f pod.yaml 
pod/aug created

kubectl get nodes
NAME       STATUS   ROLES           AGE     VERSION
minikube   Ready    control-plane   3m38s   v1.30.0

kubectl get all
NAME      READY   STATUS    RESTARTS   AGE
pod/aug   1/1     Running   0          21s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   3m51s

kubectl get pods --show-labels
NAME   READY   STATUS    RESTARTS   AGE    LABELS
aug    1/1     Running   0          106s   department=sre,env=production
'''

# Create another pod: pods.yaml

'''
cat pods.yaml 
apiVersion: v1
kind: Pod
metadata:
  name: aug-1
spec:
  containers:
    - name: sre-csc
      image: ubuntu
      command: ["/bin/bash", "-c", "while true; do echo This is aug-1 of Kubernetes; sleep 5 ; done"]
'''

'''
kubectl apply -f pods.yaml 
pod/aug-1 created

kubectl get all
NAME        READY   STATUS    RESTARTS   AGE
pod/aug     1/1     Running   0          4m23s
pod/aug-1   1/1     Running   0          4s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   7m53s
'''

'''
kubectl get pods --show-labels
NAME    READY   STATUS    RESTARTS   AGE    LABELS
aug     1/1     Running   0          5m4s   department=sre,env=production
aug-1   1/1     Running   0          45s    <none>

kubectl get pods -l env=production
NAME   READY   STATUS    RESTARTS   AGE
aug    1/1     Running   0          5m44s

kubectl get pods -l env!=production
NAME    READY   STATUS    RESTARTS   AGE
aug-1   1/1     Running   0          91s
'''

# Apply the labels

'''
kubectl label pods aug place=us
pod/aug labeled

kubectl get pods --show-labels
NAME    READY   STATUS    RESTARTS   AGE     LABELS
aug     1/1     Running   0          8m28s   department=sre,env=production,place=us
aug-1   1/1     Running   0          4m9s    <none>
'''

'''
kubectl get pods -l 'env in (production)'
NAME   READY   STATUS    RESTARTS   AGE
aug    1/1     Running   0          10m

kubectl get pods -l 'env notin (production)'
NAME    READY   STATUS    RESTARTS   AGE
aug-1   1/1     Running   0          6m42s

kubectl get pods -l 'place notin (India, us)'
NAME    READY   STATUS    RESTARTS   AGE
aug-1   1/1     Running   0          8m2s

kubectl get pods -l 'place in (us)'
NAME   READY   STATUS    RESTARTS   AGE
aug    1/1     Running   0          12m

kubectl delete pod -l place!=India
pod "aug" deleted
pod "aug-1" deleted
'''

# NodeSelector

'''
cat podss.yaml 
apiVersion: v1
kind: Pod
metadata:
  name: aug-2
  labels:
    env: production
    department: sre
spec:
  containers:
    - name: sre-css
      image: ubuntu
      command: ["/bin/bash", "-c", "while true; do echo This is aug of Kubernetes; sleep 5 ; done"]
  nodeSelectors:
    instanceType: t2.xlarge
'''

'''
kubectl apply -f podss.yaml 
pod/aug-2 created

kubectl get all
NAME        READY   STATUS    RESTARTS   AGE
pod/aug-2   0/1     Pending   0          6s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   20m

kubectl get nodes
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   22m   v1.30.0

kubectl label nodes minikube instanceType=t2.xlarge
node/minikube labeled

kubectl get all
NAME        READY   STATUS    RESTARTS   AGE
pod/aug-2   1/1     Running   0          2m53s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   23m
'''

# Reference

[Link](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#built-in-node-labels)

---

# Day38-08Aug24: ReplicaSet,ReplicationController-Lab9


Replication Controller

A Replication Controller (RC) is a Kubernetes object that ensures a specified number of pod replicas are running at any given time. If a pod fails or is deleted, the Replication Controller creates a new pod to replace it. Conversely, if there are more pods than the desired number, the Replication Controller will terminate the excess pods.

# Pre-Requisites

# Create an EC2 Instance

# Download and start minikube
'''
Refer installation repo
'''

# Install kubectl
'''
Refer installation repo
'''

# Install docker
'''
Refer installation repo
'''

# Example of a Replication Controller

'''
cat rc.yaml 

apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx-rc
spec:
  replicas: 3
  selector:
    app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
'''

# In this example:

'''
* The nginx-rc Replication Controller ensures that 3 replicas of the nginx pod are running.
* The selector field matches pods with the label app: nginx.

'''

# Creating and Managing a Replication Controller

'''
kubectl apply -f rc.yaml 
replicationcontroller/nginx-rc created

kubectl get all 
NAME                 READY   STATUS    RESTARTS   AGE
pod/nginx-rc-6wq78   1/1     Running   0          9s
pod/nginx-rc-8h5v8   1/1     Running   0          9s
pod/nginx-rc-tdbqt   1/1     Running   0          9s

NAME                             DESIRED   CURRENT   READY   AGE
replicationcontroller/nginx-rc   3         3         3       9s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   8h

kubectl get pods -o wide
NAME             READY   STATUS    RESTARTS   AGE   IP            NODE       NOMINATED NODE   READINESS GATES
nginx-rc-6wq78   1/1     Running   0          45s   10.244.0.50   minikube   <none>           <none>
nginx-rc-8h5v8   1/1     Running   0          45s   10.244.0.51   minikube   <none>           <none>
nginx-rc-tdbqt   1/1     Running   0          45s   10.244.0.52   minikube   <none>           <none>

kubectl get rc
NAME       DESIRED   CURRENT   READY   AGE
nginx-rc   3         3         3       57s

kubectl get deployment
No resources found in default namespace.

kubectl get ns
NAME              STATUS   AGE
default           Active   8h
kube-node-lease   Active   8h
kube-public       Active   8h
kube-system       Active   8h

kubectl delete pods nginx-rc-6wq78
pod "nginx-rc-6wq78" deleted


kubectl get all
NAME                 READY   STATUS    RESTARTS   AGE
pod/nginx-rc-8h5v8   1/1     Running   0          4m18s
pod/nginx-rc-tdbqt   1/1     Running   0          4m18s
pod/nginx-rc-znn74   1/1     Running   0          7s

NAME                             DESIRED   CURRENT   READY   AGE
replicationcontroller/nginx-rc   3         3         3       4m18s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   8h


kubectl get pods -o wide
NAME             READY   STATUS    RESTARTS   AGE     IP            NODE       NOMINATED NODE   READINESS GATES
nginx-rc-8h5v8   1/1     Running   0          5m11s   10.244.0.51   minikube   <none>           <none>
nginx-rc-tdbqt   1/1     Running   0          5m11s   10.244.0.52   minikube   <none>           <none>
nginx-rc-znn74   1/1     Running   0          60s     10.244.0.53   minikube   <none>           <none>


kubectl scale --replicas=5 rc -l app=nginx
replicationcontroller/nginx-rc scaled


kubectl get all
NAME                 READY   STATUS    RESTARTS   AGE
pod/nginx-rc-8h5v8   1/1     Running   0          7m10s
pod/nginx-rc-c59cz   1/1     Running   0          10s
pod/nginx-rc-tdbqt   1/1     Running   0          7m10s
pod/nginx-rc-vtn8t   1/1     Running   0          10s
pod/nginx-rc-znn74   1/1     Running   0          2m59s

NAME                             DESIRED   CURRENT   READY   AGE
replicationcontroller/nginx-rc   5         5         5       7m10s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   8h


kubectl delete pods nginx-rc-8h5v8
pod "nginx-rc-8h5v8" deleted


kubectl get all
NAME                 READY   STATUS    RESTARTS   AGE
pod/nginx-rc-c59cz   1/1     Running   0          56s
pod/nginx-rc-dzcdp   1/1     Running   0          3s
pod/nginx-rc-tdbqt   1/1     Running   0          7m56s
pod/nginx-rc-vtn8t   1/1     Running   0          56s
pod/nginx-rc-znn74   1/1     Running   0          3m45s

NAME                             DESIRED   CURRENT   READY   AGE
replicationcontroller/nginx-rc   5         5         5       7m56s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   8h


kubectl scale --replicas=2 rc -l app=nginx
replicationcontroller/nginx-rc scaled


kubectl get all
NAME                 READY   STATUS    RESTARTS   AGE
pod/nginx-rc-tdbqt   1/1     Running   0          8m24s
pod/nginx-rc-znn74   1/1     Running   0          4m13s

NAME                             DESIRED   CURRENT   READY   AGE
replicationcontroller/nginx-rc   2         2         2       8m24s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   8h


kubectl get all
NAME                 READY   STATUS    RESTARTS   AGE
pod/nginx-rc-tdbqt   1/1     Running   0          9m11s
pod/nginx-rc-znn74   1/1     Running   0          5m

NAME                             DESIRED   CURRENT   READY   AGE
replicationcontroller/nginx-rc   2         2         2       9m11s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   8h
'''

# Delete the rc 

'''
kubectl delete rc nginx-rc
replicationcontroller "nginx-rc" deleted

kubectl get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   8h
'''

---

# Replica Set

'''
A Replica Set (RS) is the next-generation Replication Controller. It supports the same functionalities but with more efficient and expressive ways to define selectors. The primary use case for a Replica Set is to maintain a stable set of replica pods running at any given time.

'''

# Example of a Replica Set

'''
cat rs.yaml 
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
'''

# In this example:

* The nginx-rs Replica Set ensures that 3 replicas of the nginx pod are running.
* The selector uses matchLabels to match pods with the label app: nginx.

# Creating and Managing a Replica Set

'''
kubectl apply -f rs.yaml 
replicaset.apps/nginx-rs created


kubectl get all
NAME                 READY   STATUS    RESTARTS   AGE
pod/nginx-rs-7jrgr   1/1     Running   0          7s
pod/nginx-rs-ch8fd   1/1     Running   0          7s
pod/nginx-rs-hcj52   1/1     Running   0          7s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   8h

NAME                       DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-rs   3         3         3       7s


kubectl get pods -o wide 
NAME             READY   STATUS    RESTARTS   AGE   IP            NODE       NOMINATED NODE   READINESS GATES
nginx-rs-7jrgr   1/1     Running   0          19s   10.244.0.57   minikube   <none>           <none>
nginx-rs-ch8fd   1/1     Running   0          19s   10.244.0.58   minikube   <none>           <none>
nginx-rs-hcj52   1/1     Running   0          19s   10.244.0.59   minikube   <none>           <none>


kubectl get rs
NAME       DESIRED   CURRENT   READY   AGE
nginx-rs   3         3         3       32s


kubectl logs -f nginx-rs-7jrgr
^C
'''

# We can play with all the above executed in rc: please refer above

# Delete the rs

'''
kubectl delete rs nginx-rs
replicaset.apps "nginx-rs" deleted

kubectl get all 
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   8h
'''

----

# Differences Between Replication Controller and Replica Set

1. Selectors:
    * Replication Controller uses equality-based selectors.
    * Replica Set uses both equality and set-based selectors, allowing for more flexible and powerful queries.
2. Usage in Deployments:
    * Replica Set is used by Deployments to manage pod replicas, providing more advanced features like rolling updates and rollbacks.
3. Obsolescence:
    * Replication Controllers are considered legacy and have largely been replaced by Replica Sets in most modern Kubernetes deployments.

---

# Day39-09Aug24: Deployment-Lab10


# Kubernetes Deployment

'''
A Kubernetes Deployment provides declarative updates to applications and ensures that the desired state specified in the Deployment YAML is always met. It manages Replica Sets and provides functionalities such as rolling updates, rollbacks, scaling, and more.
'''

# Pre-Requisites

# Create an EC2 Instance

# Download and start minikube
'''
Refer installation repo
'''

# Install kubectl
'''
Refer installation repo
'''

# Install docker
'''
Refer installation repo
'''

# Example of a Deployment

# Here's an example of a Kubernetes Deployment for an Nginx application:

'''
cat deployment.yaml 

apiVersion: apps/v1
kind: Deployment
metadata:
   name: deployment
spec:
   replicas: 3
   selector:     
    matchLabels:
     name: labels-pods
   template:
     metadata:
       name: meta-pods
       labels:
         name: labels-pods
     spec:
      containers:
        - name: ubuntu-dep
          image: ubuntu
          command: ["/bin/bash", "-c", "while true; do echo Initial version of deployment….; sleep 5; done"]
'''

'''
kubectl apply -f deployment.yaml 
deployment.apps/deployment created


kubectl get all
NAME                             READY   STATUS    RESTARTS   AGE
pod/deployment-86c7bb4d7-jbqch   1/1     Running   0          6m8s
pod/deployment-86c7bb4d7-nt69z   1/1     Running   0          6m8s
pod/deployment-86c7bb4d7-qg7dx   1/1     Running   0          6m8s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   7m30s

NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/deployment   3/3     3            3           6m8s

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/deployment-86c7bb4d7   3         3         3       6m8s


kubectl delete pods deployment-86c7bb4d7-jbqch
pod "deployment-86c7bb4d7-jbqch" deleted


kubectl get all
NAME                             READY   STATUS    RESTARTS   AGE
pod/deployment-86c7bb4d7-58kbk   1/1     Running   0          34s
pod/deployment-86c7bb4d7-nt69z   1/1     Running   0          8m3s
pod/deployment-86c7bb4d7-qg7dx   1/1     Running   0          8m3s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   9m25s

NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/deployment   3/3     3            3           8m3s

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/deployment-86c7bb4d7   3         3         3       8m3s


kubectl get pods -o wide
NAME                         READY   STATUS    RESTARTS   AGE     IP           NODE       NOMINATED NODE   READINESS GATES
deployment-86c7bb4d7-58kbk   1/1     Running   0          68s     10.244.0.6   minikube   <none>           <none>
deployment-86c7bb4d7-nt69z   1/1     Running   0          8m37s   10.244.0.5   minikube   <none>           <none>
deployment-86c7bb4d7-qg7dx   1/1     Running   0          8m37s   10.244.0.4   minikube   <none>           <none>


kubectl logs -f deployment-86c7bb4d7-58kbk --tail=5
Initial version of deployment….
Initial version of deployment….
Initial version of deployment….
Initial version of deployment….
Initial version of deployment….
Initial version of deployment….
^C

kubectl exec deployment-86c7bb4d7-58kbk -- cat /etc/os-release
PRETTY_NAME="Ubuntu 24.04 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo
'''

# Change done on new image

'''
cat deployment.yaml 

apiVersion: apps/v1
kind: Deployment
metadata:
   name: deployment
spec:
   replicas: 3
   selector:     
    matchLabels:
     name: labels-pods
   template:
     metadata:
       name: meta-pods
       labels:
         name: labels-pods
     spec:
      containers:
        - name: centos-dep
          image: centos
          command: ["/bin/bash", "-c", "while true; do echo Second version of deployment….; sleep 5; done"]
'''

'''
kubectl apply -f deployment.yaml 
deployment.apps/deployment configured


kubectl get all
NAME                             READY   STATUS    RESTARTS   AGE
pod/deployment-96db5d779-bvrz9   1/1     Running   0          89s
pod/deployment-96db5d779-nx5g8   1/1     Running   0          2m4s
pod/deployment-96db5d779-zchn5   1/1     Running   0          85s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   32m

NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/deployment   3/3     3            3           31m

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/deployment-86c7bb4d7   0         0         0       31m
replicaset.apps/deployment-96db5d779   3         3         3       2m4s


kubectl logs -f pod/deployment-96db5d779-bvrz9 --tail=5
Second version of deployment….
Second version of deployment….
Second version of deployment….
Second version of deployment….
Second version of deployment….
^C

kubectl exec pod/deployment-96db5d779-bvrz9 -- cat /etc/os-release
NAME="CentOS Linux"
VERSION="8"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="8"
PLATFORM_ID="platform:el8"
PRETTY_NAME="CentOS Linux 8"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:8"
HOME_URL="https://centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"
CENTOS_MANTISBT_PROJECT="CentOS-8"
CENTOS_MANTISBT_PROJECT_VERSION="8"


kubectl scale --replicas=5 deployment deployment
deployment.apps/deployment scaled


kubectl get all 
NAME                             READY   STATUS    RESTARTS   AGE
pod/deployment-96db5d779-bvrz9   1/1     Running   0          3m54s
pod/deployment-96db5d779-m4gwd   1/1     Running   0          27s
pod/deployment-96db5d779-nx5g8   1/1     Running   0          4m29s
pod/deployment-96db5d779-pmh4r   1/1     Running   0          27s
pod/deployment-96db5d779-zchn5   1/1     Running   0          3m50s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   35m

NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/deployment   5/5     5            5           33m

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/deployment-86c7bb4d7   0         0         0       33m
replicaset.apps/deployment-96db5d779   5         5         5       4m29s
'''

# Clean the resources

'''
kubectl delete deployment deployment 
deployment.apps "deployment" deleted

kubectl get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   4h41m
'''

'''
minikube stop
✋  Stopping node "minikube"  ...
🛑  Powering off "minikube" via SSH ...
🛑  1 node stopped.
'''

---

# Day40-13Aug24: Services-Lab11

Types of services:

In Kubernetes, a Service is an abstraction that defines a logical set of Pods and a policy by which to access them. Services enable external traffic to reach your Pods, and they ensure reliable networking between Pods, even if Pods are distributed across different nodes in the cluster.

 Types of Kubernetes Services:

1. ClusterIP (default):
   - Exposes the service on a cluster-internal IP.
   - This service type is only accessible within the Kubernetes cluster.
   - Use Case: Internal communication between microservices.

   Example:
   ''' 
   apiVersion: v1
   kind: Service
   metadata:
     name: my-clusterip-service
   spec:
     selector:
       app: my-app
     ports:
       - protocol: TCP
         port: 80
         targetPort: 8080
  '''
   - In this example, the service is exposing port `80` inside the cluster, which forwards traffic to port `8080` on the selected Pods.

2. NodePort:

   - Exposes the service on each Node's IP at a static port.
   - This makes the service accessible from outside the cluster using `<NodeIP>:<NodePort>`.
   - Use Case: Accessing the service externally without a load balancer.

   Example:
   '''
   apiVersion: v1
   kind: Service
   metadata:
     name: my-nodeport-service
   spec:
     type: NodePort
     selector:
       app: my-app
     ports:
       - protocol: TCP
         port: 80
         targetPort: 8080
         nodePort: 30007
   '''
   - Here, the service will be accessible externally at `http://<NodeIP>:30007`.

3. LoadBalancer:
   - Exposes the service externally using a cloud provider's load balancer.
   - The load balancer forwards traffic to the NodePort of the service.
   - Use Case: Production environments where you need a public IP address for accessing the service.

   Example:
   '''
   apiVersion: v1
   kind: Service
   metadata:
     name: my-loadbalancer-service
   spec:
     type: LoadBalancer
     selector:
       app: my-app
     ports:
       - protocol: TCP
         port: 80
         targetPort: 8080
   '''
   - The cloud provider assigns a public IP to the service, making it accessible globally.

4. ExternalName:
   - Maps the service to the contents of the `externalName` field by returning a CNAME record.
   - Use Case: Useful for directing traffic to an external DNS name.

   Example:
   '''
   apiVersion: v1
   kind: Service
   metadata:
     name: my-externalname-service
   spec:
     type: ExternalName
     externalName: example.com
   '''
   - The service `my-externalname-service` will resolve to `example.com`.

 Creating a Service:

Here’s how you create a simple ClusterIP service in Kubernetes:

1. Create a `deployment.yaml` to deploy your application:

   '''
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: my-app
   spec:
     replicas: 3
     selector:
       matchLabels:
         app: my-app
     template:
       metadata:
         labels:
           app: my-app
       spec:
         containers:
         - name: my-app
           image: nginx
           ports:
           - containerPort: 80
   '''

2. Then create a `service.yaml`:

   '''
   apiVersion: v1
   kind: Service
   metadata:
     name: my-app-service
   spec:
     selector:
       app: my-app
     ports:
       - protocol: TCP
         port: 80
         targetPort: 80
   '''

3. Apply the YAML files:

   '''
   kubectl apply -f deployment.yaml
   '''

 Managing Services:

- Get all Services:
  
  '''
  kubectl get services
  kubectl get all     # list all 
  '''

- Get detailed information about a Service:
  
  '''
  kubectl describe service <service-name>
  '''

- Delete a Service:
  '''
  kubectl delete service <service-name>
  '''

Final Notes:

Services are crucial for Kubernetes-based applications, especially in microservices architectures where communication between services needs to be reliable and resilient. By using different types of services, you can control how your application is exposed within the cluster and to the outside world.

# Lab

# Pre-Requisites

# Create an EC2 Instance

# Download and start minikube
'''
Refer installation repo
'''

# Install kubectl
'''
Refer installation repo
'''

# Install docker
'''
Refer installation repo
'''

# Login to EC2 machine 

'''
sudo apt update
'''

mkdir services
cd services


'''
cat dep.yaml 

apiVersion: v1
kind: Pod
metadata:
  name: cont
  labels:
    env: prod
    department: sre
spec:
  containers:
    - name: cont1
      image: nginx
    - name: cont2
      image: ubuntu
      command: ["/bin/bash", "-c", "while true; do echo Learning K8s....; sleep 5 ; done"]
      ports:
        - containerPort: 80
'''

'''
kubectl apply -f dep.yaml 
pod/cont created

kubectl get all 
NAME       READY   STATUS    RESTARTS   AGE
pod/cont   2/2     Running   0          5s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   5d8h

kubectl get pods -o wide 
NAME   READY   STATUS    RESTARTS   AGE   IP             NODE       NOMINATED NODE   READINESS GATES
cont   2/2     Running   0          75s   10.244.0.101   minikube   <none>           <none>
'''

'''
kubectl exec cont -it cont1 -- /bin/bash  
Defaulted container "cont1" out of: cont1, cont2
root@cont:/# 
root@cont:/# curl 10.244.0.101
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
root@cont:/# 
root@cont:/# apt update && apt install curl 
Get:1 http://deb.debian.org/debian bookworm InRelease [151 kB]
Get:2 http://deb.debian.org/debian bookworm-updates InRelease [55.4 kB]
Get:3 http://deb.debian.org/debian-security bookworm-security InRelease [48.0 kB]
Get:4 http://deb.debian.org/debian bookworm/main amd64 Packages [8788 kB]
Get:5 http://deb.debian.org/debian bookworm-updates/main amd64 Packages [13.8 kB]
Get:6 http://deb.debian.org/debian-security bookworm-security/main amd64 Packages [169 kB]
Fetched 9225 kB in 1s (8350 kB/s)                         
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
All packages are up to date.
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
curl is already the newest version (7.88.1-10+deb12u6).
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
root@cont:/# 

root@cont:/# curl localhost:80
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
root@cont:/# 
'''

'''
kubectl exec cont -it cont2 -- /bin/bash  
Defaulted container "cont1" out of: cont1, cont2
root@cont:/# 
root@cont:/# curl 10.244.0.101
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
root@cont:/# 
'''

# Next

'''
cat d-pod1.yaml 

apiVersion: v1
kind: Pod
metadata:
  name: d-pod1
  labels:
    env: prod
    department: sre
spec:
  containers:
    - name: cont1
      image: nginx
      ports:
        - containerPort: 80
'''

'''
cat d-pod2.yaml 

apiVersion: v1
kind: Pod
metadata:
  name: d-pod2
  labels:
    env: prod
    department: sre
spec:
  containers:
    - name: cont2
      image: nginx
      ports:
        - containerPort: 80
'''

'''
kubectl apply -f d-pod1.yaml 
pod/d-pod1 created

kubectl apply -f d-pod2.yaml 
pod/d-pod2 created

kubectl get all 
NAME         READY   STATUS    RESTARTS   AGE
pod/cont     2/2     Running   0          7m37s
pod/d-pod1   1/1     Running   0          53s
pod/d-pod2   1/1     Running   0          6s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   5d8h
'''

'''
kubectl get pods -o wide
NAME     READY   STATUS    RESTARTS   AGE     IP             NODE       NOMINATED NODE   READINESS GATES
cont     2/2     Running   0          9m21s   10.244.0.101   minikube   <none>           <none>
d-pod1   1/1     Running   0          2m37s   10.244.0.102   minikube   <none>           <none>
d-pod2   1/1     Running   0          110s    10.244.0.103   minikube   <none>           <none>
'''

'''
kubectl exec d-pod1 -it cont1 -- /bin/bash  
root@d-pod1:/# 
root@d-pod1:/# curl 10.244.0.102
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
root@d-pod1:/# 
root@d-pod1:/# exit 
exit
'''

'''
kubectl exec d-pod2 -it cont1 -- /bin/bash  
root@d-pod2:/# 
root@d-pod2:/# curl 10.244.0.103
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
root@d-pod2:/# 
root@d-pod2:/# 
root@d-pod2:/# exit 
exit
'''

# Clean: [Link](https://kubernetes.io/docs/reference/kubectl/generated/kubectl_delete/)

'''
kubectl delete pods d-pod1
pod "d-pod1" deleted

kubectl delete pods d-pod2
pod "d-pod2" deleted

kubectl delete -f dep.yaml 
pod "cont" deleted
'''

'''
minikube stop
✋  Stopping node "minikube"  ...
🛑  Powering off "minikube" via SSH ...
🛑  1 node stopped.
'''

# Stop the instance

---

# Day41-19Aug24: Volumes-Lab12

'''

minikube status
minikube start      # In case stop 

'''

'''
kubectl get all 
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   11d

kubectl get nodes
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   11d   v1.30.0
'''

# emptydir 

'''
cat empty.yaml 

apiVersion: v1
kind: Pod
metadata:
  name: emptydir
spec:
  containers:
  - name: container1
    image: ubuntu
    command: ["/bin/bash", "-c", "while true; do echo This is Day13 of 30DaysOfKubernetes; sleep 5 ; done"]
    volumeMounts:
      - name: day13
        mountPath: "/tmp/container1"
  - name: container2
    image: centos
    command: ["/bin/bash", "-c", "while true; do echo Chak de INDIA!; sleep 5 ; done"]
    volumeMounts:
      - name: day13
        mountPath: "/tmp/container2"
  volumes:
  - name: day13
    emptyDir: {}
'''

'''
kubectl apply -f empty.yaml 
pod/emptydir created

kubectl get all 
NAME           READY   STATUS    RESTARTS   AGE
pod/emptydir   2/2     Running   0          6s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   11d


kubectl exec -it emptydir -c container1 -- /bin/bash
root@emptydir:/# 
root@emptydir:/# cd /tmp/container1/
root@emptydir:/tmp/container1# ls
root@emptydir:/tmp/container1# cat >> abc.txt
hello world 
a new program 
^C
root@emptydir:/tmp/container1# 
root@emptydir:/tmp/container1# exit 
exit
command terminated with exit code 130


kubectl exec -it emptydir -c container2 -- /bin/bash
[root@emptydir /]# 
[root@emptydir /]# cd /tmp/container2/
[root@emptydir container2]# ls
abc.txt
[root@emptydir container2]# cat abc.txt 
hello world
a new program 
[root@emptydir container2]# cat >> cba.txt
this is the second 
container
^C
[root@emptydir container2]# ls
abc.txt  cba.txt
[root@emptydir container2]# exit 
exit


kubectl exec -it emptydir -c container1 -- /bin/bash
root@emptydir:/# cd /tmp/container1/
root@emptydir:/tmp/container1# ls
abc.txt  cba.txt
root@emptydir:/tmp/container1# exit
exit


kubectl delete pods emptydir
pod "emptydir" deleted
----

# hostpath

'''
cat hostPath.yaml 

apiVersion: v1
kind: Pod
metadata:
  name: hostpath
spec:
  containers:
  - name: container1
    image: ubuntu
    command: ["/bin/bash", "-c", "while true; do echo This is Day13 of 30DaysOfKubernetes; sleep 5 ; done"]
    volumeMounts:
    - mountPath: "/tmp/cont"
      name: hp-vm
  volumes:
  - name: hp-vm
    hostPath: 
      path: /tmp/data
'''

'''
kubectl apply -f hostPath.yaml 
pod/hostpath created


ls /tmp/data
ls: cannot access '/tmp/data': No such file or directory


kubectl get all 
NAME           READY   STATUS    RESTARTS   AGE
pod/hostpath   1/1     Running   0          10s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   11d


kubectl exec -it hostpath -c container1 /bin/bash 
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
root@hostpath:/# 
root@hostpath:/# cd /tmp/cont/
root@hostpath:/tmp/cont# ls
root@hostpath:/tmp/cont# echo "This is the part of container..." >> abc.txt
root@hostpath:/tmp/cont# ls   
abc.txt
root@hostpath:/tmp/cont# cat abc.txt 
This is the part of container...
root@hostpath:/tmp/cont# exit 
exit


ls /tmp/data
ls: cannot access '/tmp/data': No such file or directory

kubectl delete pods hostpath 
pod "hostpath" deleted
'''

# PV and PVC 

# Create a volume from the EBS -> 20 GB 

# pv.yaml

'''
cat pv.yaml 

apiVersion: v1
kind: PersistentVolume
metadata:
  name: myebsvol
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Recycle
  awsElasticBlockStore:
    volumeID: vol-0308a2410419b25bb
    fsType: ext4
'''

'''
kubectl apply -f pv.yaml 
Warning: spec.persistentVolumeReclaimPolicy: The Recycle reclaim policy is deprecated. Instead, the recommended approach is to use dynamic provisioning.
persistentvolume/myebsvol created

kubectl get pv
NAME       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   VOLUMEATTRIBUTESCLASS   REASON   AGE
myebsvol   1Gi        RWO            Recycle          Available                          <unset>                          66s
'''

# pvc.yaml

'''
cat pvc.yaml 

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myebsvolclaim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
'''

'''
kubectl apply -f pvc.yaml 
persistentvolumeclaim/myebsvolclaim create

kubectl get pv
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM                   STORAGECLASS   VOLUMEATTRIBUTESCLASS   REASON   AGE
myebsvol                                   1Gi        RWO            Recycle          Available                                          <unset>                          2m48s
pvc-bf4057f0-6cb6-4341-872b-605cdb5d9c6d   1Gi        RWO            Delete           Bound       default/myebsvolclaim   standard       <unset>                          5s
'''

'''
cat deployment.yaml 

apiVersion: apps/v1
kind: Deployment
metadata:
  name: pvdeploy
spec:
  replicas: 1
  selector:     
    matchLabels:
     app: mypv
  template:
    metadata:
      labels:
        app: mypv
    spec:
      containers:
      - name: shell
        image: centos
        command: ["bin/bash", "-c", "sleep 10000"]
        volumeMounts:
        - name: mypd
          mountPath: "/tmp/persistent"
      volumes:
        - name: mypd
          persistentVolumeClaim:
            claimName: myebsvolclaim
'''

'''
kubectl apply -f deployment.yaml 
deployment.apps/pvdeploy created

kubectl get all 
NAME                            READY   STATUS    RESTARTS   AGE
pod/pvdeploy-8556986f75-zmb8g   1/1     Running   0          5s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   11d

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/pvdeploy   1/1     1            1           5s

NAME                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/pvdeploy-8556986f75   1         1         1       5s


kubectl get pods -o wide 
NAME                        READY   STATUS    RESTARTS   AGE   IP             NODE       NOMINATED NODE   READINESS GATES
pvdeploy-8556986f75-zmb8g   1/1     Running   0          45s   10.244.0.114   minikube   <none>           <none>


kubectl get deploy 
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
pvdeploy   1/1     1            1           71s

kubectl exec -it pvdeploy-8556986f75-zmb8g -c shell /bin/bash
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
[root@pvdeploy-8556986f75-zmb8g /]# 
[root@pvdeploy-8556986f75-zmb8g /]# cd /tmp/persistent
[root@pvdeploy-8556986f75-zmb8g persistent]# 
[root@pvdeploy-8556986f75-zmb8g persistent]# ls
[root@pvdeploy-8556986f75-zmb8g persistent]# 
[root@pvdeploy-8556986f75-zmb8g persistent]# 
[root@pvdeploy-8556986f75-zmb8g persistent]# echo "Data should be persistent" >> abc.txt 
[root@pvdeploy-8556986f75-zmb8g persistent]# 
[root@pvdeploy-8556986f75-zmb8g persistent]# cat abc.txt 
Data should be persistent
[root@pvdeploy-8556986f75-zmb8g persistent]# 
[root@pvdeploy-8556986f75-zmb8g persistent]# exit 
exit


kubectl delete pods pvdeploy-8556986f75-zmb8g
pod "pvdeploy-8556986f75-zmb8g" deleted

kubectl get pods -o wide 
NAME                        READY   STATUS    RESTARTS   AGE   IP             NODE       NOMINATED NODE   READINESS GATES
pvdeploy-8556986f75-dn6kr   1/1     Running   0          40s   10.244.0.115   minikube   <none>           <none>


kubectl exec -it pvdeploy-8556986f75-dn6kr -c shell /bin/bash
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
[root@pvdeploy-8556986f75-dn6kr /]# 
[root@pvdeploy-8556986f75-dn6kr /]# 
[root@pvdeploy-8556986f75-dn6kr /]# cd /tmp/persistent
[root@pvdeploy-8556986f75-dn6kr persistent]# ls
abc.txt
[root@pvdeploy-8556986f75-dn6kr persistent]# 
[root@pvdeploy-8556986f75-dn6kr persistent]# cat abc.txt 
Data should be persistent
[root@pvdeploy-8556986f75-dn6kr persistent]# 
[root@pvdeploy-8556986f75-dn6kr persistent]# 
[root@pvdeploy-8556986f75-dn6kr persistent]# exi t
bash: exi: command not found
[root@pvdeploy-8556986f75-dn6kr persistent]# exit
exit
command terminated with exit code 127
'''

# Cleaning

# Remove the pv and pvc 

'''
kubectl delete pv myebsvol
persistentvolume "myebsvol" deleted

kubectl delete pv pvc-bf4057f0-6cb6-4341-872b-605cdb5d9c6d
persistentvolume "pvc-bf4057f0-6cb6-4341-872b-605cdb5d9c6d" deleted
'''

# Remove the vol from AWS

kubectl delete deployment pvdeploy
deployment.apps "pvdeploy" deleted

----

# Remove the deployment file 


---