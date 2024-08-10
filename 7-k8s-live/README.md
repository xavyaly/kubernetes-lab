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

