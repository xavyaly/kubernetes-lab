# kubernetes-lab

Feature of K8s:
Monitoring ??
Self healing / resilience in K8s ??
High Availability ??
Load Balancing ??
Auto Scaling ??
Automatic Bin Packing ??
Rolling and Canary Deployment ??
Automatic Rollout and Rollback ??
Secret and Configuration Mangement ??

---------------------------------------------------------------------------------------------

K8s Alternatives:
Docker Swarm ??
Kubernetes ??
Mesos ??
AWS ECS ??

---------------------------------------------------------------------------------------------

Type of Controllers:
Replication Controllers ??
Node Controllers ??
Endpoint Controllers ??

---------------------------------------------------------------------------------------------

Desired and Actual State of PODs in K8s ??

---------------------------------------------------------------------------------------------

Useful Links:

Kubectl Installation: https://kubernetes.io/docs/tasks/tools/
Minikube Installation: https://minikube.sigs.k8s.io/docs/start/
Kind Installation: https://kind.sigs.k8s.io/docs/user/qu...
Kubeadm Installation: https://kubernetes.io/docs/setup/prod...
K3s Installation: https://rancher.com/docs/k3s/latest/e...


SetUp K8s minikube and Kubectl:

Kubernetes SetUp:
Minikube ??
Kind ??
K3s ??
Kubeadm ??

AWS EKS ??
GCP GKE ??
Azure AKS ??

---------------------------------------------------------------------------------------------

[Minikube & kubectl](https://www.youtube.com/watch?v=dSOS-7SZ_BA&list=PLrMP04WSdCjrkNYSFvFeiHrfpsSVDFMDR&index=3)


# Install kubectl: https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/

# Execute the below binary commands to install kubectl
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/darwin/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
sudo chown root: /usr/local/bin/kubectl
kubectl version --client
```

```
$ kubectl version --client
WARNING: This version information is deprecated and will be replaced with the output from kubectl version --short.  Use --output=yaml|json to get the full version.
Client Version: version.Info{Major:"1", Minor:"25+", GitVersion:"v1.25.9-dispatcher", GitCommit:"6ed97cc2601c54f907320513513db38e446aa2ee", GitTreeState:"clean", BuildDate:"2023-05-09T18:09:24Z", GoVersion:"go1.19.8", Compiler:"gc", Platform:"darwin/arm64"}
Kustomize Version: v4.5.7
```

```
$ kubectl version --client --output yaml
clientVersion:
  buildDate: "2023-05-09T18:09:24Z"
  compiler: gc
  gitCommit: 6ed97cc2601c54f907320513513db38e446aa2ee
  gitTreeState: clean
  gitVersion: v1.25.9-dispatcher
  goVersion: go1.19.8
  major: "1"
  minor: 25+
  platform: darwin/arm64
kustomizeVersion: v4.5.7
```

# kubeclt installation done


# Install minikube if not present, to talk to cluster

# What you’ll need 
```
2 CPUs or more
2GB of free memory
20GB of free disk space
Internet connection
Container or virtual machine manager, such as: Docker, QEMU, Hyperkit, Hyper-V, KVM, Parallels, Podman, VirtualBox, or VMware Fusion/Workstation
```

# how to install [Install minikube](https://minikube.sigs.k8s.io/docs/start/)

# Binary
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64
sudo install minikube-darwin-amd64 /usr/local/bin/minikube
```
```
$ minikube version
minikube version: v1.30.1
commit: 08896fd1dc362c097c925146c4a0d0dac715ace0
```

OR

$ minikube start
```
$ minikube start
😄  minikube v1.30.1 on Darwin 13.4 (arm64)
🎉  minikube 1.31.2 is available! Download it: https://github.com/kubernetes/minikube/releases/tag/v1.31.2
💡  To disable this notice, run: 'minikube config set WantUpdateNotification false'

✨  Using the docker driver based on existing profile
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
🤷  docker "minikube" container is missing, will recreate.
🔥  Creating docker container (CPUs=2, Memory=2200MB) ...
🐳  Preparing Kubernetes v1.26.3 on Docker 23.0.2 ...
🔗  Configuring bridge CNI (Container Networking Interface) ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
    ▪ Using image docker.io/kubernetesui/dashboard:v2.7.0
    ▪ Using image docker.io/kubernetesui/metrics-scraper:v1.0.8
💡  Some dashboard features require the metrics-server addon. To enable all features please run:

        minikube addons enable metrics-server


🌟  Enabled addons: storage-provisioner, default-storageclass, dashboard
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default 
```

$ minikube version
```
$ minikube version
minikube version: v1.30.1
commit: 08896fd1dc362c097c925146c4a0d0dac715ace0
```

$ minikube stop
```
$ minikube stop
✋  Stopping node "minikube"  ...
🛑  Powering off "minikube" via SSH ...
🛑  1 node stopped.
```

# remove the minikube
```
$ docker ps -a
CONTAINER ID   IMAGE                                 COMMAND                  CREATED             STATUS                        PORTS     NAMES
5c824e777ba1   gcr.io/k8s-minikube/kicbase:v0.0.39   "/usr/local/bin/entr…"   About an hour ago   Exited (137) 33 seconds ago             minikube
20847a3e2b93   nginx                                 "/docker-entrypoint.…"   2 hours ago         Up 2 hours                              my_nginx
$ docker rm 5c
5c
```

# start minikube again

$ minikube start --nodes 2 -p local-cluster --driver=docker
```
$ minikube start --nodes 2 -p local-cluster --driver=docker
😄  [local-cluster] minikube v1.30.1 on Darwin 13.4 (arm64)
✨  Using the docker driver based on user configuration
📌  Using Docker Desktop driver with root privileges
👍  Starting control plane node local-cluster in cluster local-cluster
🚜  Pulling base image ...
🔥  Creating docker container (CPUs=2, Memory=2200MB) ...
🐳  Preparing Kubernetes v1.26.3 on Docker 23.0.2 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔗  Configuring CNI (Container Networking Interface) ...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🔎  Verifying Kubernetes components...
🌟  Enabled addons: storage-provisioner, default-storageclass

👍  Starting worker node local-cluster-m02 in cluster local-cluster
🚜  Pulling base image ...
🔥  Creating docker container (CPUs=2, Memory=2200MB) ...
🌐  Found network options:
    ▪ NO_PROXY=192.168.58.2
🐳  Preparing Kubernetes v1.26.3 on Docker 23.0.2 ...
    ▪ env NO_PROXY=192.168.58.2
🔎  Verifying Kubernetes components...
🏄  Done! kubectl is now configured to use "local-cluster" cluster and "default" namespace by default
```

$ minikube status -p local-cluster
```
$ minikube status -p local-cluster
local-cluster
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured

local-cluster-m02
type: Worker
host: Running
kubelet: Running
```

# kubectl syntax:

```
$ kubectl [command][TYPE][NAME][flags]

kubectl -> CLI
command -> create, get, describe, delete, etc 
TYPE -> pods, services, etc 
flags -> -f, -o, etc 
```

# Examples:

$ kubectl get nodes -o wide
```
$ kubectl get nodes -o wide
NAME                STATUS   ROLES           AGE     VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION     CONTAINER-RUNTIME
local-cluster       Ready    control-plane   7m36s   v1.26.3   192.168.58.2   <none>        Ubuntu 20.04.5 LTS   5.15.49-linuxkit   docker://23.0.2
local-cluster-m02   Ready    <none>          6m19s   v1.26.3   192.168.58.3   <none>        Ubuntu 20.04.5 LTS   5.15.49-linuxkit   docker://23.0.2
```

# Two Containers 

$ docker ps -a
```
$ docker ps -a
CONTAINER ID   IMAGE                                 COMMAND                  CREATED         STATUS         PORTS                                                                                                                                  NAMES
7077da093a61   gcr.io/k8s-minikube/kicbase:v0.0.39   "/usr/local/bin/entr…"   6 minutes ago   Up 6 minutes   127.0.0.1:57852->22/tcp, 127.0.0.1:57853->2376/tcp, 127.0.0.1:57855->5000/tcp, 127.0.0.1:57851->8443/tcp, 127.0.0.1:57854->32443/tcp   local-cluster-m02
819d21a17709   gcr.io/k8s-minikube/kicbase:v0.0.39   "/usr/local/bin/entr…"   7 minutes ago   Up 7 minutes   127.0.0.1:57804->22/tcp, 127.0.0.1:57805->2376/tcp, 127.0.0.1:57802->5000/tcp, 127.0.0.1:57803->8443/tcp, 127.0.0.1:57806->32443/tcp   local-cluster
```


# Switch back and forth to different clusters

$ kubectl config get-contexts
```
$ kubectl config get-contexts
CURRENT   NAME                                                                CLUSTER                                                             AUTHINFO                                                            NAMESPACE
          arn:aws:eks:us-east-2:252473277340:cluster/education-eks-kBqcKMkt   arn:aws:eks:us-east-2:252473277340:cluster/education-eks-kBqcKMkt   arn:aws:eks:us-east-2:252473277340:cluster/education-eks-kBqcKMkt   
*         local-cluster                                                       local-cluster                                                       local-cluster                                                       default
```

$ kubectl config set-context local-cluster
```
$ kubectl config set-context local-cluster
Context "local-cluster" modified.
```

$ kubectl config get-contexts
```
$ kubectl config get-contexts
CURRENT   NAME                                                                CLUSTER                                                             AUTHINFO                                                            NAMESPACE
          arn:aws:eks:us-east-2:252473277340:cluster/education-eks-kBqcKMkt   arn:aws:eks:us-east-2:252473277340:cluster/education-eks-kBqcKMkt   arn:aws:eks:us-east-2:252473277340:cluster/education-eks-kBqcKMkt   
*         local-cluster                                                       local-cluster                                                       local-cluster                                                       default
```

# Add an additional worker nodes in minikube cluster, we can add both master or worker

$ minikube node add --worker -p local-cluster
```
$ minikube node add --worker -p local-cluster
😄  Adding node m03 to cluster local-cluster
👍  Starting worker node local-cluster-m03 in cluster local-cluster
🚜  Pulling base image ...
🔥  Creating docker container (CPUs=2, Memory=2200MB) ...
🐳  Preparing Kubernetes v1.26.3 on Docker 23.0.2 ...
🔎  Verifying Kubernetes components...
🏄  Successfully added m03 to local-cluster!
```

# For confirmation 

$ kubectl get nodes -o wide
```
$ kubectl get nodes -o wide
NAME                STATUS   ROLES           AGE   VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION     CONTAINER-RUNTIME
local-cluster       Ready    control-plane   18m   v1.26.3   192.168.58.2   <none>        Ubuntu 20.04.5 LTS   5.15.49-linuxkit   docker://23.0.2
local-cluster-m02   Ready    <none>          17m   v1.26.3   192.168.58.3   <none>        Ubuntu 20.04.5 LTS   5.15.49-linuxkit   docker://23.0.2
local-cluster-m03   Ready    <none>          82s   v1.26.3   192.168.58.4   <none>        Ubuntu 20.04.5 LTS   5.15.49-linuxkit   docker://23.0.2
```

# Delete worker node added above

$ minikube node delete local-cluster-m03 -p local-cluster
```
$ minikube node delete local-cluster-m03 -p local-cluster
🔥  Deleting node local-cluster-m03 from cluster local-cluster
✋  Stopping node "local-cluster-m03"  ...
🛑  Powering off "local-cluster-m03" via SSH ...
🔥  Deleting "local-cluster-m03" in docker ...
💀  Node local-cluster-m03 was successfully deleted.
```

# For verification

$ kubectl get nodes -o wide
```
$ kubectl get nodes -o wide
NAME                STATUS   ROLES           AGE   VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION     CONTAINER-RUNTIME
local-cluster       Ready    control-plane   22m   v1.26.3   192.168.58.2   <none>        Ubuntu 20.04.5 LTS   5.15.49-linuxkit   docker://23.0.2
local-cluster-m02   Ready    <none>          21m   v1.26.3   192.168.58.3   <none>        Ubuntu 20.04.5 LTS   5.15.49-linuxkit   docker://23.0.2
```

# Access minikube dashboard

$ minikube dashboard --url -p local-cluster
```
$ minikube dashboard --url -p local-cluster
🔌  Enabling dashboard ...
    ▪ Using image docker.io/kubernetesui/dashboard:v2.7.0
    ▪ Using image docker.io/kubernetesui/metrics-scraper:v1.0.8
💡  Some dashboard features require the metrics-server addon. To enable all features please run:

        minikube -p local-cluster addons enable metrics-server


🤔  Verifying dashboard health ...
🚀  Launching proxy ...
🤔  Verifying proxy health ...
http://127.0.0.1:58080/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/
```

# Paste the above URL on browser
```
http://127.0.0.1:58080/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/
```

# Click on Nodes; both nodes should be reflect --> Interesting
# Click on Services, 
# Configmap for ca certs
# Storage Class
# Cluster Role Bindings
# Cluster Roles
# Events
# Namespaces
# Nodes
# Service Accounts etc 

---------------------------------------------------------------------------------------------

[Introduction to YAML](https://www.youtube.com/watch?v=3nsOxJR1-pY&list=PLrMP04WSdCjrkNYSFvFeiHrfpsSVDFMDR&index=4)

# This is my local-cluster information on minikube

# Refer to YAML Folder

$ touch cluster.yaml
```
# This is my local cluster information
# This is on minikube
- name: local-cluster
  isActive: true
  createdAt: 2021-09-25 21:11:15
  version: !!str &clusterVersion 1.22.1
  access:
    ip: 192.168.58.2
    port: 8443
  nodes:
    - name: local-vluster-01
      type: master
      tags: ["local", "master"]
    - name: local-cluster-02
      type: worker
      tags: ["local", "worker"]
  description: >
    This is the local cluser created on minikube.
    And the driver I uswed is Docker!
  longDescription: |
    This is the local cluser created on minikube.
    And the driver I uswed is Docker!
- name: dev-cluster
  isActive: true
  createdAt: 2021-09-24 21:11:15
  version: *clusterVersion
  access:
    ip: 192.168.58.3
    port: 8443
  nodes:
    - name: dev-vluster-01
      type: master
      tags: ["dev", "master"]
    - name: dev-cluster-02
      type: worker
      tags: ["dev", "worker"]
```

$ touch cluster.json
```
[
  {
    "name": "local-cluster",
    "isActive": true,
    "createdAt": "2021-09-25T21:11:15.000Z",
    "version": "1.22.1",
    "access": {
      "ip": "192.168.58.2",
      "port": 8443
    },
    "nodes": [
      {
        "name": "local-vluster-01",
        "type": "master",
        "tags": [
          "local",
          "master"
        ]
      },
      {
        "name": "local-cluster-02",
        "type": "worker",
        "tags": [
          "local",
          "worker"
        ]
      }
    ],
    "description": "This is the local cluser created on minikube. And the driver I uswed is Docker!\n",
    "longDescription": "This is the local cluser created on minikube.\nAnd the driver I uswed is Docker!\n"
  },
  {
    "name": "dev-cluster",
    "isActive": true,
    "createdAt": "2021-09-24T21:11:15.000Z",
    "version": "1.22.1",
    "access": {
      "ip": "192.168.58.3",
      "port": 8443
    },
    "nodes": [
      {
        "name": "dev-vluster-01",
        "type": "master",
        "tags": [
          "dev",
          "master"
        ]
      },
      {
        "name": "dev-cluster-02",
        "type": "worker",
        "tags": [
          "dev",
          "worker"
        ]
      }
    ]
  }
]
```

---------------------------------------------------------------------------------------------

[All about PODs](https://www.youtube.com/watch?v=nEyPwKelcYg&list=PLrMP04WSdCjrkNYSFvFeiHrfpsSVDFMDR&index=5)

[K8s PODs](https://kubernetes.io/docs/concepts/workloads/pods/)

Sidecar container ??


# Implementation

$ kubectl run nginx-pod --image=nginx
```
$ kubectl run nginx-pod --image=nginx
The connection to the server 127.0.0.1:57803 was refused - did you specify the right host or port?
```

```
$ minikube start
😄  minikube v1.30.1 on Darwin 13.4 (arm64)
✨  Using the docker driver based on existing profile
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
🤷  docker "minikube" container is missing, will recreate.
🔥  Creating docker container (CPUs=2, Memory=2200MB) ...
🐳  Preparing Kubernetes v1.26.3 on Docker 23.0.2 ...
🔗  Configuring bridge CNI (Container Networking Interface) ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
    ▪ Using image docker.io/kubernetesui/dashboard:v2.7.0
    ▪ Using image docker.io/kubernetesui/metrics-scraper:v1.0.8
💡  Some dashboard features require the metrics-server addon. To enable all features please run:

        minikube addons enable metrics-server


🌟  Enabled addons: storage-provisioner, default-storageclass, dashboard
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

```
$ kubectl get nodes
NAME       STATUS   ROLES           AGE     VERSION
minikube   Ready    control-plane   2d21h   v1.26.3
```

---------------------------------------------------------------------------------------------

[Microk8s](https://microk8s.io/tutorials)

---------------------------------------------------------------------------------------------

$ kubectl run nginx-pod --image=nginx
```
$ kubectl run nginx-pod --image=nginx
pod/nginx-pod created
```

$ kubectl get pods -o wide
```
$ kubectl get pods -o wide
NAME        READY   STATUS    RESTARTS   AGE   IP            NODE       NOMINATED NODE   READINESS GATES
nginx-pod   1/1     Running   0          20s   10.244.0.33   minikube   <none>           <none>
```

$ kubectl api-resources | grep pods
```
$ kubectl api-resources | grep pods
pods                              po           v1                                     true         Pod
```

# Refer nginx-pod.yaml 

# Better to maintain in *.yaml file to manage the PODs

$ touch nginx-pod.yaml
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod1
  labels:
    team: integration
    app: todo
spec:
  containers:
  - name: nginx-container
    image: nginx:latest
    resources:
      limits:
        memory: "128Mi"
        cpu: "500m"
    ports:
      - containerPort: 80
```

# Apply 

$ kubectl apply -f nginx-pod.yaml 
```
$ kubectl apply -f nginx-pod.yaml 
pod/nginx-pod1 created
```

$ kubectl get pods -o wide
```
$ kubectl get pods -o wide
NAME         READY   STATUS    RESTARTS   AGE   IP            NODE       NOMINATED NODE   READINESS GATES
nginx-pod    1/1     Running   0          18m   10.244.0.33   minikube   <none>           <none>
nginx-pod1   1/1     Running   0          91s   10.244.0.34   minikube   <none>           <none>
```

$ kubectl delete pods nginx-pod 
```
$ kubectl delete pods nginx-pod 
pod "nginx-pod" deleted
```

$ kubectl get pods -o wide
```
$ kubectl get pods -o wide
NAME         READY   STATUS    RESTARTS   AGE    IP            NODE       NOMINATED NODE   READINESS GATES
nginx-pod1   1/1     Running   0          3m3s   10.244.0.34   minikube   <none>           <none>
```

$ kubectl apply -f nginx-pod.yaml 
```
$ kubectl apply -f nginx-pod.yaml 
pod/nginx-pod1 unchanged
```

# Filter the pods with labels 

$ kubectl get pods -l team=integration
```
$ kubectl get pods -l team=integration
NAME         READY   STATUS    RESTARTS   AGE
nginx-pod1   1/1     Running   0          7m56s
```

$ kubectl get pods -l team=integration,app=todo
```
$ kubectl get pods -l team=integration,app=todo
NAME         READY   STATUS    RESTARTS   AGE
nginx-pod1   1/1     Running   0          8m5s
```

# Additional info about pods

$ kubectl get pods
```
$ kubectl get pods
NAME         READY   STATUS    RESTARTS   AGE
nginx-pod1   1/1     Running   0          26m
```

$ kubectl get pod nginx-pod1 -o wide
```
$ kubectl get pod nginx-pod1 -o wide
NAME         READY   STATUS    RESTARTS   AGE   IP            NODE       NOMINATED NODE   READINESS GATES
nginx-pod1   1/1     Running   0          26m   10.244.0.34   minikube   <none>           <none>
```

$ kubectl get pod nginx-pod1 -o yaml
```
$ kubectl get pod nginx-pod1 -o yaml
apiVersion: v1
kind: Pod
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","kind":"Pod","metadata":{"annotations":{},"labels":{"app":"todo","team":"integration"},"name":"nginx-pod1","namespace":"default"},"spec":{"containers":[{"image":"nginx:latest","name":"nginx-container","ports":[{"containerPort":80}],"resources":{"limits":{"cpu":"500m","memory":"128Mi"}}}]}}
  creationTimestamp: "2023-08-26T12:39:54Z"
  labels:
    app: todo
    team: integration
  name: nginx-pod1
  namespace: default
  resourceVersion: "27080"
  uid: 1b68646c-1fbf-4c2b-873b-96363ac9b13e
spec:
  containers:
  - image: nginx:latest
    imagePullPolicy: Always
    name: nginx-container
    ports:
    - containerPort: 80
      protocol: TCP
    resources:
      limits:
        cpu: 500m
        memory: 128Mi
      requests:
        cpu: 500m
        memory: 128Mi
    terminationMessagePath: /dev/termination-log
    terminationMessagePolicy: File
    volumeMounts:
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: kube-api-access-ncjn5
      readOnly: true
  dnsPolicy: ClusterFirst
  enableServiceLinks: true
  nodeName: minikube
  preemptionPolicy: PreemptLowerPriority
  priority: 0
  restartPolicy: Always
  schedulerName: default-scheduler
  securityContext: {}
  serviceAccount: default
  serviceAccountName: default
  terminationGracePeriodSeconds: 30
  tolerations:
  - effect: NoExecute
    key: node.kubernetes.io/not-ready
    operator: Exists
    tolerationSeconds: 300
  - effect: NoExecute
    key: node.kubernetes.io/unreachable
    operator: Exists
    tolerationSeconds: 300
  volumes:
  - name: kube-api-access-ncjn5
    projected:
      defaultMode: 420
      sources:
      - serviceAccountToken:
          expirationSeconds: 3607
          path: token
      - configMap:
          items:
          - key: ca.crt
            path: ca.crt
          name: kube-root-ca.crt
      - downwardAPI:
          items:
          - fieldRef:
              apiVersion: v1
              fieldPath: metadata.namespace
            path: namespace
status:
  conditions:
  - lastProbeTime: null
    lastTransitionTime: "2023-08-26T12:39:54Z"
    status: "True"
    type: Initialized
  - lastProbeTime: null
    lastTransitionTime: "2023-08-26T12:39:58Z"
    status: "True"
    type: Ready
  - lastProbeTime: null
    lastTransitionTime: "2023-08-26T12:39:58Z"
    status: "True"
    type: ContainersReady
  - lastProbeTime: null
    lastTransitionTime: "2023-08-26T12:39:54Z"
    status: "True"
    type: PodScheduled
  containerStatuses:
  - containerID: docker://60fdcf9248e72c3ea9b667fd04e7b38063aa350cd78b126e0aaa2646d910b207
    image: nginx:latest
    imageID: docker-pullable://nginx@sha256:104c7c5c54f2685f0f46f3be607ce60da7085da3eaa5ad22d3d9f01594295e9c
    lastState: {}
    name: nginx-container
    ready: true
    restartCount: 0
    started: true
    state:
      running:
        startedAt: "2023-08-26T12:39:58Z"
  hostIP: 192.168.49.2
  phase: Running
  podIP: 10.244.0.34
  podIPs:
  - ip: 10.244.0.34
  qosClass: Guaranteed
  startTime: "2023-08-26T12:39:54Z"
```

$ kubectl describe pod nginx-pod1
```
$ kubectl describe pod nginx-pod1
Name:             nginx-pod1
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Sat, 26 Aug 2023 18:09:54 +0530
Labels:           app=todo
                  team=integration
Annotations:      <none>
Status:           Running
IP:               10.244.0.34
IPs:
  IP:  10.244.0.34
Containers:
  nginx-container:
    Container ID:   docker://60fdcf9248e72c3ea9b667fd04e7b38063aa350cd78b126e0aaa2646d910b207
    Image:          nginx:latest
    Image ID:       docker-pullable://nginx@sha256:104c7c5c54f2685f0f46f3be607ce60da7085da3eaa5ad22d3d9f01594295e9c
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Sat, 26 Aug 2023 18:09:58 +0530
    Ready:          True
    Restart Count:  0
    Limits:
      cpu:     500m
      memory:  128Mi
    Requests:
      cpu:        500m
      memory:     128Mi
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-ncjn5 (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  kube-api-access-ncjn5:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   Guaranteed
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  28m   default-scheduler  Successfully assigned default/nginx-pod1 to minikube
  Normal  Pulling    28m   kubelet            Pulling image "nginx:latest"
  Normal  Pulled     28m   kubelet            Successfully pulled image "nginx:latest" in 2.851903835s (2.851926043s including waiting)
  Normal  Created    28m   kubelet            Created container nginx-container
  Normal  Started    28m   kubelet            Started container nginx-container
```

$ kubectl describe pod nginx-pod1 | grep Port
```
$ kubectl describe pod nginx-pod1 | grep Port
    Port:           80/TCP
    Host Port:      0/TCP
```

# Login to a POD:

$ kubectl exec -it nginx-pod1 -- bash
```
$ kubectl exec -it nginx-pod1 -- bash
root@nginx-pod1:/# ls
bin  boot  dev  docker-entrypoint.d  docker-entrypoint.sh  etc  home  lib  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@nginx-pod1:/# ls -l
total 64
lrwxrwxrwx   1 root root    7 Aug 14 00:00 bin -> usr/bin
drwxr-xr-x   2 root root 4096 Jul 14 16:00 boot
drwxr-xr-x   5 root root  360 Aug 26 12:39 dev
drwxr-xr-x   1 root root 4096 Aug 15 23:58 docker-entrypoint.d
-rwxrwxr-x   1 root root 1620 Aug 15 23:58 docker-entrypoint.sh
drwxr-xr-x   1 root root 4096 Aug 26 12:39 etc
drwxr-xr-x   2 root root 4096 Jul 14 16:00 home
lrwxrwxrwx   1 root root    7 Aug 14 00:00 lib -> usr/lib
drwxr-xr-x   2 root root 4096 Aug 14 00:00 media
drwxr-xr-x   2 root root 4096 Aug 14 00:00 mnt
drwxr-xr-x   2 root root 4096 Aug 14 00:00 opt
dr-xr-xr-x 271 root root    0 Aug 26 12:39 proc
drwx------   2 root root 4096 Aug 14 00:00 root
drwxr-xr-x   1 root root 4096 Aug 26 12:39 run
lrwxrwxrwx   1 root root    8 Aug 14 00:00 sbin -> usr/sbin
drwxr-xr-x   2 root root 4096 Aug 14 00:00 srv
dr-xr-xr-x  13 root root    0 Aug 26 12:39 sys
drwxrwxrwt   1 root root 4096 Aug 15 23:58 tmp
drwxr-xr-x   1 root root 4096 Aug 14 00:00 usr
drwxr-xr-x   1 root root 4096 Aug 14 00:00 var
root@nginx-pod1:/# 
root@nginx-pod1:/# exit
exit
```

# Login to container reside inside a POD

$ kubectl exec -it nginx-pod1 -c nginx-container -- bash 
```
$ kubectl exec -it nginx-pod1 -c nginx-container -- bash 
root@nginx-pod1:/# 
root@nginx-pod1:/# ls -la
total 72
drwxr-xr-x   1 root root 4096 Aug 26 12:39 .
drwxr-xr-x   1 root root 4096 Aug 26 12:39 ..
-rwxr-xr-x   1 root root    0 Aug 26 12:39 .dockerenv
lrwxrwxrwx   1 root root    7 Aug 14 00:00 bin -> usr/bin
drwxr-xr-x   2 root root 4096 Jul 14 16:00 boot
drwxr-xr-x   5 root root  360 Aug 26 12:39 dev
drwxr-xr-x   1 root root 4096 Aug 15 23:58 docker-entrypoint.d
-rwxrwxr-x   1 root root 1620 Aug 15 23:58 docker-entrypoint.sh
drwxr-xr-x   1 root root 4096 Aug 26 12:39 etc
drwxr-xr-x   2 root root 4096 Jul 14 16:00 home
lrwxrwxrwx   1 root root    7 Aug 14 00:00 lib -> usr/lib
drwxr-xr-x   2 root root 4096 Aug 14 00:00 media
drwxr-xr-x   2 root root 4096 Aug 14 00:00 mnt
drwxr-xr-x   2 root root 4096 Aug 14 00:00 opt
dr-xr-xr-x 270 root root    0 Aug 26 12:39 proc
drwx------   1 root root 4096 Aug 26 13:12 root
drwxr-xr-x   1 root root 4096 Aug 26 12:39 run
lrwxrwxrwx   1 root root    8 Aug 14 00:00 sbin -> usr/sbin
drwxr-xr-x   2 root root 4096 Aug 14 00:00 srv
dr-xr-xr-x  13 root root    0 Aug 26 12:39 sys
drwxrwxrwt   1 root root 4096 Aug 15 23:58 tmp
drwxr-xr-x   1 root root 4096 Aug 14 00:00 usr
drwxr-xr-x   1 root root 4096 Aug 14 00:00 var
root@nginx-pod1:/# exit
exit
```

# Port forwading

$ kubectl port-forward nginx-pod1 8083:80 
```
$ kubectl port-forward nginx-pod1 8083:80 
Forwarding from 127.0.0.1:8083 -> 80
Forwarding from [::1]:8083 -> 80
Handling connection for 8083

# access the nginx page -> http://localhost:8083/ 
```

# POD Logs 

$ kubectl logs nginx-pod1 | grep "error"
```
$ kubectl logs nginx-pod1 | grep "error"
2023/08/26 13:18:58 [error] 29#29: *1 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file or directory), client: 127.0.0.1, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "localhost:8083", referrer: "http://localhost:8083/"
```

# Delete the POD created through YAML

$ kubectl delete -f nginx-pod.yaml 
```
$ kubectl delete -f nginx-pod.yaml 
pod "nginx-pod1" deleted
```

$ kubectl get pods 
```
$ kubectl get pods 
No resources found in default namespace.
```

# Create a POD, delete a POD 

$ kubectl run pod --image=nginx 
```
$ kubectl run pod --image=nginx 
pod/pod created
$ kubectl get pods
NAME   READY   STATUS    RESTARTS   AGE
pod    1/1     Running   0          6s
$ kubectl delete pods pod
pod "pod" deleted
$ kubectl api-resources | grep pod
pods                              po           v1                                     true         Pod
podtemplates                                   v1                                     true         PodTemplate
horizontalpodautoscalers          hpa          autoscaling/v2                         true         HorizontalPodAutoscaler
poddisruptionbudgets              pdb          policy/v1                              true         PodDisruptionBudget
```

---------------------------------------------------------------------------------------------

# ReplicaSets and Deployments | Self Healing, High Availability, Rollout, and Rollback in Kubernetes

[ReplicaSets and Deployments](https://www.youtube.com/watch?v=mEnCFazQ8BM&list=PLrMP04WSdCjrkNYSFvFeiHrfpsSVDFMDR&index=6)

# Self Healing and High Availability
# Self Healing: bringing back to PODs
# High Availability: 


```
$ kubectl api-resources | grep pods
pods                              po           v1                                     true         Pod

$ kubectl api-resources | grep rs
persistentvolumeclaims            pvc          v1                                     true         PersistentVolumeClaim
persistentvolumes                 pv           v1                                     false        PersistentVolume
replicationcontrollers            rc           v1                                     true         ReplicationController
orders                                         acme.cert-manager.io/v1                true         Order
replicasets                       rs           apps/v1                                true         ReplicaSet
horizontalpodautoscalers          hpa          autoscaling/v2                         true         HorizontalPodAutoscaler
certificaterequests               cr,crs       cert-manager.io/v1                     true         CertificateRequest
clusterissuers                                 cert-manager.io/v1                     false        ClusterIssuer
issuers                                        cert-manager.io/v1                     true         Issuer
csidrivers                                     storage.k8s.io/v1                      false        CSIDriver

$ kubectl api-resources | grep replicaset
replicasets                       rs           apps/v1                                true         ReplicaSet
```

# Def of replicaset:
```
A ReplicaSet in Kubernetes is a resource that ensures a specified number of replicas (pods) are running at all times, 
even in the face of failures or scaling events. It's a lower-level primitive compared to the more common Deployment, 
which provides a higher-level abstraction and additional features like rolling updates.
```

# Execution

$ cat nginx-replicaset.yaml
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
spec:
  replicas: 3
  selector:
    matchLabels: 
      app: nginx 
  template:
    metadata:                           ----> copy from POD from here
      name: nginx-pod
      labels:
        app: nginx 
    spec:
      containers:
      - name: nginx-container
        image: nginx:latest
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
          - containerPort: 80
```

```
$ kubectl create -f nginx-replicaset.yaml 
Error from server (BadRequest): error when creating "nginx-replicaset.yaml": ReplicaSet in version "v1" cannot be handled as a ReplicaSet: strict decoding error: unknown field "metadata.label", unknown field "spec.selector.matchLevel"

# resolved, typo mistake was there

$ kubectl create -f nginx-replicaset.yaml 
replicaset.apps/nginx-replicaset created

$ kubectl get rs
NAME               DESIRED   CURRENT   READY   AGE
nginx-replicaset   3         3         3       2m9s

$ kubectl get po -o wide
NAME                     READY   STATUS    RESTARTS   AGE     IP            NODE       NOMINATED NODE   READINESS GATES
nginx-replicaset-2chqx   1/1     Running   0          3m21s   10.244.0.36   minikube   <none>           <none>
nginx-replicaset-6g6cc   1/1     Running   0          3m21s   10.244.0.37   minikube   <none>           <none>
nginx-replicaset-hmxt2   1/1     Running   0          3m21s   10.244.0.38   minikube   <none>           <none>

$ kubectl get rs
NAME               DESIRED   CURRENT   READY   AGE
nginx-replicaset   3         3         3       5m2s

$ kubectl get po
NAME                     READY   STATUS    RESTARTS   AGE
nginx-replicaset-2chqx   1/1     Running   0          5m7s
nginx-replicaset-6g6cc   1/1     Running   0          5m7s
nginx-replicaset-hmxt2   1/1     Running   0          5m7s

$ kubectl delete pod nginx-replicaset-2chqx
pod "nginx-replicaset-2chqx" deleted

$ kubectl get po
NAME                     READY   STATUS              RESTARTS   AGE
nginx-replicaset-6g6cc   1/1     Running             0          5m19s
nginx-replicaset-gbkgn   0/1     ContainerCreating   0          3s
nginx-replicaset-hmxt2   1/1     Running             0          5m19s

$ kubectl get rs
NAME               DESIRED   CURRENT   READY   AGE
nginx-replicaset   3         3         3       5m22s


$ minikube start --nodes 2 -p local-cluster --driver=docker
😄  [local-cluster] minikube v1.30.1 on Darwin 13.4 (arm64)
✨  Using the docker driver based on existing profile
👍  Starting control plane node local-cluster in cluster local-cluster
🚜  Pulling base image ...
🔄  Restarting existing docker container for "local-cluster" ...
🐳  Preparing Kubernetes v1.26.3 on Docker 23.0.2 ...
🔗  Configuring CNI (Container Networking Interface) ...
🔎  Verifying Kubernetes components...
    ▪ Using image docker.io/kubernetesui/dashboard:v2.7.0
    ▪ Using image docker.io/kubernetesui/metrics-scraper:v1.0.8
💡  Some dashboard features require the metrics-server addon. To enable all features please run:

        minikube -p local-cluster addons enable metrics-server


🌟  Enabled addons: dashboard
❗  The cluster local-cluster already exists which means the --nodes parameter will be ignored. Use "minikube node add" to add nodes to an existing cluster.
👍  Starting worker node local-cluster-m02 in cluster local-cluster
🚜  Pulling base image ...
🔄  Restarting existing docker container for "local-cluster-m02" ...
🌐  Found network options:
    ▪ NO_PROXY=192.168.58.2
🐳  Preparing Kubernetes v1.26.3 on Docker 23.0.2 ...
    ▪ env NO_PROXY=192.168.58.2
🔎  Verifying Kubernetes components...
🏄  Done! kubectl is now configured to use "local-cluster" cluster and "default" namespace by default


$ kubectl get nodes
NAME                STATUS   ROLES           AGE     VERSION
local-cluster       Ready    control-plane   3h53m   v1.26.3
local-cluster-m02   Ready    <none>          17s     v1.26.3


$ minikube node add --worker -p local-cluster
😄  Adding node m03 to cluster local-cluster
👍  Starting worker node local-cluster-m03 in cluster local-cluster
🚜  Pulling base image ...
🔥  Creating docker container (CPUs=2, Memory=2200MB) ...
🐳  Preparing Kubernetes v1.26.3 on Docker 23.0.2 ...
🔎  Verifying Kubernetes components...
🏄  Successfully added m03 to local-cluster!


# 2 worker nodes are running now
$ kubectl get nodes
NAME                STATUS   ROLES           AGE     VERSION
local-cluster       Ready    control-plane   3h56m   v1.26.3
local-cluster-m02   Ready    <none>          2m35s   v1.26.3
local-cluster-m03   Ready    <none>          18s     v1.26.3


$ kubectl create -f nginx-replicaset.yaml 
replicaset.apps/nginx-replicaset created


$ kubectl get po -o wide 
NAME                     READY   STATUS              RESTARTS   AGE   IP       NODE                NOMINATED NODE   READINESS GATES
nginx-replicaset-6pq6z   0/1     ContainerCreating   0          4s    <none>   local-cluster-m03   <none>           <none>
nginx-replicaset-bxk6h   0/1     ContainerCreating   0          4s    <none>   local-cluster-m02   <none>           <none>
nginx-replicaset-xbql2   0/1     ContainerCreating   0          4s    <none>   local-cluster       <none>           <none>


$ minikube node delete local-cluster-m02 -p local-cluster
🔥  Deleting node local-cluster-m02 from cluster local-cluster
✋  Stopping node "local-cluster-m02"  ...
🛑  Powering off "local-cluster-m02" via SSH ...
🔥  Deleting "local-cluster-m02" in docker ...
💀  Node local-cluster-m02 was successfully deleted.


$ kubectl get po -o wide 
NAME                     READY   STATUS    RESTARTS   AGE    IP           NODE                NOMINATED NODE   READINESS GATES
nginx-replicaset-6pq6z   1/1     Running   0          2m5s   10.244.2.2   local-cluster-m03   <none>           <none>
nginx-replicaset-l6lf8   1/1     Running   0          10s    10.244.2.3   local-cluster-m03   <none>           <none>
nginx-replicaset-xbql2   1/1     Running   0          2m5s   10.244.0.5   local-cluster       <none>           <none>

# Till here we have seen how self healing and high availability handles in case if nodes or pods deleted 
```

# Rollout and Rollback

# PODs: Smallest unit in K8s

# When we create a Deployment, replicaset automatically created

$ kubectl api-resources | grep deployment
```
$ kubectl api-resources | grep deployment
deployments                       deploy       apps/v1                                true         Deployment
```

$ cat nginx-deployment.yaml     --> same as replicaset format
```
apiVersion: apps/v1
kind: Deployment 
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels: 
      app: nginx 
  template:
    metadata:
      name: nginx-pod
      labels:
        app: nginx 
    spec:
      containers:
      - name: nginx-container
        image: nginx:latest
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
          - containerPort: 80
```

# remove all before creating the deployment

```
$ kubectl get pods -o wide
NAME                     READY   STATUS    RESTARTS   AGE   IP           NODE                NOMINATED NODE   READINESS GATES
nginx-replicaset-6pq6z   1/1     Running   0          22m   10.244.2.2   local-cluster-m03   <none>           <none>
nginx-replicaset-l6lf8   1/1     Running   0          20m   10.244.2.3   local-cluster-m03   <none>           <none>
nginx-replicaset-xbql2   1/1     Running   0          22m   10.244.0.5   local-cluster       <none>           <none>

$ kubectl get nodes -o wide
NAME                STATUS   ROLES           AGE     VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION     CONTAINER-RUNTIME
local-cluster       Ready    control-plane   4h19m   v1.26.3   192.168.58.2   <none>        Ubuntu 20.04.5 LTS   5.15.49-linuxkit   docker://23.0.2
local-cluster-m03   Ready    <none>          23m     v1.26.3   192.168.58.4   <none>        Ubuntu 20.04.5 LTS   5.15.49-linuxkit   docker://23.0.2

$ kubectl get rs -o wide
NAME               DESIRED   CURRENT   READY   AGE   CONTAINERS        IMAGES         SELECTOR
nginx-replicaset   3         3         3       22m   nginx-container   nginx:latest   app=nginx

$ kubectl get svc
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   4h20m

$ kubectl get ns
NAME                   STATUS   AGE
default                Active   4h20m
kube-node-lease        Active   4h20m
kube-public            Active   4h20m
kube-system            Active   4h20m
kubernetes-dashboard   Active   3h56m

$ kubectl get deployment
No resources found in default namespace.
```

# Delete all resources 

```
$ kubectl delete all --all
pod "nginx-replicaset-6pq6z" deleted
pod "nginx-replicaset-l6lf8" deleted
pod "nginx-replicaset-xbql2" deleted
service "kubernetes" deleted
replicaset.apps "nginx-replicaset" deleted


$ kubectl get pods -o wide
No resources found in default namespace.
$ kubectl get nodes -o wide
NAME                STATUS   ROLES           AGE     VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION     CONTAINER-RUNTIME
local-cluster       Ready    control-plane   4h22m   v1.26.3   192.168.58.2   <none>        Ubuntu 20.04.5 LTS   5.15.49-linuxkit   docker://23.0.2
local-cluster-m03   Ready    <none>          26m     v1.26.3   192.168.58.4   <none>        Ubuntu 20.04.5 LTS   5.15.49-linuxkit   docker://23.0.2

$ kubectl get rs -o wide
No resources found in default namespace.

$ kubectl get svc -o wide
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE   SELECTOR
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   49s   <none>

$ kubectl get deployment -o wide
No resources found in default namespace.

$ kubectl get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   2m30s
```

# Create a deployment

$ kubectl create -f nginx-deployment.yaml 
```
$ kubectl create -f nginx-deployment.yaml 
deployment.apps/nginx-deployment created
```

# List all the resources created
```
$ kubectl get all
NAME                                    READY   STATUS              RESTARTS   AGE
pod/nginx-deployment-654b769bc9-9mkz8   0/1     ContainerCreating   0          4s
pod/nginx-deployment-654b769bc9-cxk8k   0/1     ContainerCreating   0          4s
pod/nginx-deployment-654b769bc9-gqhmm   0/1     ContainerCreating   0          4s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   3m48s

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   0/3     3            0           4s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deployment-654b769bc9   3         3         0       4s
```

# After some time 

```
$ kubectl get all
NAME                                    READY   STATUS    RESTARTS   AGE
pod/nginx-deployment-654b769bc9-9mkz8   1/1     Running   0          119s
pod/nginx-deployment-654b769bc9-cxk8k   1/1     Running   0          119s
pod/nginx-deployment-654b769bc9-gqhmm   1/1     Running   0          119s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   5m43s

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   3/3     3            3           119s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deployment-654b769bc9   3         3         3       119s
```

$ kubectl get po --show-labels -o wide
```
$ kubectl get po --show-labels -o wide
NAME                                READY   STATUS    RESTARTS   AGE     IP           NODE                NOMINATED NODE   READINESS GATES   LABELS
nginx-deployment-654b769bc9-9mkz8   1/1     Running   0          3m35s   10.244.2.6   local-cluster-m03   <none>           <none>            app=nginx,pod-template-hash=654b769bc9
nginx-deployment-654b769bc9-cxk8k   1/1     Running   0          3m35s   10.244.2.7   local-cluster-m03   <none>           <none>            app=nginx,pod-template-hash=654b769bc9
nginx-deployment-654b769bc9-gqhmm   1/1     Running   0          3m35s   10.244.0.7   local-cluster       <none>           <none>            app=nginx,pod-template-hash=654b769bc9
```


# Increase/Decrease the replicas

$ cat nginx-deployment.yaml     --> same as replicaset format
```
apiVersion: apps/v1
kind: Deployment 
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels: 
      app: nginx 
  template:
    metadata:
      name: nginx-pod
      labels:
        app: nginx 
    spec:
      containers:
      - name: nginx-container
        image: nginx:latest
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
          - containerPort: 80
```

$ kubectl apply -f nginx-deployment.yaml 
```
$ kubectl apply -f nginx-deployment.yaml 
Warning: resource deployments/nginx-deployment is missing the kubectl.kubernetes.io/last-applied-configuration annotation which is required by kubectl apply. kubectl apply should only be used on resources created declaratively by either kubectl create --save-config or kubectl apply. The missing annotation will be patched automatically.
deployment.apps/nginx-deployment configured
```

$ kubectl get all  --> decrease as per deployment.yaml file gr8
```
$ kubectl get all
NAME                                    READY   STATUS    RESTARTS   AGE
pod/nginx-deployment-654b769bc9-9mkz8   1/1     Running   0          8m34s
pod/nginx-deployment-654b769bc9-gqhmm   1/1     Running   0          8m34s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   12m

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   2/2     2            2           8m35s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deployment-654b769bc9   2         2         2       8m35s
```

# Increase the replicas via runtime

$ kubectl scale --replicas=4 deployment/nginx-deployment
```
$ kubectl scale --replicas=4 deployment/nginx-deployment
deployment.apps/nginx-deployment scaled
```

$ kubectl get all
```
$ kubectl get all
NAME                                    READY   STATUS    RESTARTS   AGE
pod/nginx-deployment-654b769bc9-5w5xl   1/1     Running   0          5s
pod/nginx-deployment-654b769bc9-9mkz8   1/1     Running   0          10m
pod/nginx-deployment-654b769bc9-crfbm   1/1     Running   0          5s
pod/nginx-deployment-654b769bc9-gqhmm   1/1     Running   0          10m

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   14m       --> default

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   4/4     4            4           10m

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deployment-654b769bc9   4         4         4       10m
```


# How ROLLBACK OR ROLLOUT Works in K8s 

# ROLLOUT 

$ cat nginx-deployment.yaml
```
apiVersion: apps/v1
kind: Deployment 
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels: 
      app: nginx 
  template:
    metadata:
      name: nginx-pod
      labels:
        app: nginx 
    spec:
      containers:
      - name: nginx-container
        image: nginx:1.21.3
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
          - containerPort: 80
```

$ kubectl apply -f nginx-deployment.yaml 
```
$ kubectl apply -f nginx-deployment.yaml 
deployment.apps/nginx-deployment configured
```

# A rs: "replicaset.apps/nginx-deployment-5d574fdbbf" 
# will be created whenever rollout occurs; old rs not deleted; stored 10 rs in k8s helps in rollback

$ kubectl get all
```
$ kubectl get all
NAME                                    READY   STATUS              RESTARTS   AGE
pod/nginx-deployment-5d574fdbbf-zg578   0/1     ContainerCreating   0          2s
pod/nginx-deployment-654b769bc9-9mkz8   1/1     Running             0          14m
pod/nginx-deployment-654b769bc9-gqhmm   1/1     Running             0          14m

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   17m

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   2/2     1            2           14m

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deployment-5d574fdbbf   1         1         0       2s
replicaset.apps/nginx-deployment-654b769bc9   2         2         2       14m


# after some moment

$ kubectl get all
NAME                                    READY   STATUS    RESTARTS   AGE
pod/nginx-deployment-5d574fdbbf-wn5nb   1/1     Running   0          2m38s
pod/nginx-deployment-5d574fdbbf-zg578   1/1     Running   0          2m53s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   20m

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   2/2     2            2           17m

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deployment-5d574fdbbf   2         2         2       2m53s
replicaset.apps/nginx-deployment-654b769bc9   0         0         0       17m
```

# ROLLBACK

$ kubectl set image deployment/nginx-deployment nginx-container=nginx:1.21
```
$ kubectl set image deployment/nginx-deployment nginx-container=nginx:1.21
deployment.apps/nginx-deployment image updated
```

$ kubectl get all
```
$ kubectl get all
NAME                                    READY   STATUS              RESTARTS   AGE
pod/nginx-deployment-5d574fdbbf-wn5nb   1/1     Running             0          5m18s
pod/nginx-deployment-5d574fdbbf-zg578   1/1     Running             0          5m33s
pod/nginx-deployment-6d77cc77db-d4hpq   0/1     ContainerCreating   0          8s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   23m

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   2/2     1            2           19m

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deployment-5d574fdbbf   2         2         2       5m33s
replicaset.apps/nginx-deployment-654b769bc9   0         0         0       19m
replicaset.apps/nginx-deployment-6d77cc77db   1         1         0       8s
```

# After few secs

$ kubectl get all
```
$ kubectl get all
NAME                                    READY   STATUS    RESTARTS   AGE
pod/nginx-deployment-6d77cc77db-d4hpq   1/1     Running   0          48s
pod/nginx-deployment-6d77cc77db-wkpg9   1/1     Running   0          31s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   24m

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   2/2     2            2           20m

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deployment-5d574fdbbf   0         0         0       6m13s
replicaset.apps/nginx-deployment-654b769bc9   0         0         0       20m
replicaset.apps/nginx-deployment-6d77cc77db   2         2         2       48s
```

# Cross verification the image

$ kubectl describe pod/nginx-deployment-6d77cc77db-d4hpq | grep "image"
```
$ kubectl describe pod/nginx-deployment-6d77cc77db-d4hpq | grep "image"
  Normal  Pulling    119s  kubelet            Pulling image "nginx:1.21"
  Normal  Pulled     104s  kubelet            Successfully pulled image "nginx:1.21" in 15.220363465s (15.220697424s including waiting)
```


# HISTORY

$ kubectl rollout history deployment/nginx-deployment
```
$ kubectl rollout history deployment/nginx-deployment
deployment.apps/nginx-deployment 
REVISION  CHANGE-CAUSE
1         <none>
2         <none>
3         <none>
```

$ kubectl set image deployment/nginx-deployment nginx-container:1.20 --record
```
$ kubectl set image deployment/nginx-deployment nginx-container:1.20 --record
Flag --record has been deprecated, --record will be removed in the future
error: there is no need to specify a resource type as a separate argument when passing arguments in resource/name form (e.g. 'kubectl get resource/<resource_name>' instead of 'kubectl get resource resource/<resource_name>'
```

$ kubectl set image deployment/nginx-deployment nginx-container=nginx:1.20 --record
```
$ kubectl set image deployment/nginx-deployment nginx-container=nginx:1.20 --record
Flag --record has been deprecated, --record will be removed in the future
deployment.apps/nginx-deployment image updated
```

$ kubectl rollout history deployment/nginx-deployment
```
$ kubectl rollout history deployment/nginx-deployment
deployment.apps/nginx-deployment 
REVISION  CHANGE-CAUSE
1         <none>
2         <none>
3         <none>
4         kubectl set image deployment/nginx-deployment nginx-container=nginx:1.20 --record=true
```

$ cat nginx-deployment.yaml
```
apiVersion: apps/v1
kind: Deployment 
metadata:
  name: nginx-deployment
  annotations:
    kubernetes.io/change-cause: "Updating to alpine version"
spec:
  replicas: 2
  selector:
    matchLabels: 
      app: nginx 
  template:
    metadata:
      name: nginx-pod
      labels:
        app: nginx 
    spec:
      containers:
      - name: nginx-container
        image: nginx:1.21.3
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
          - containerPort: 80
```

$ kubectl apply -f nginx-deployment.yaml 
```
$ kubectl apply -f nginx-deployment.yaml 
deployment.apps/nginx-deployment configured
```

$ kubectl rollout history deployment/nginx-deployment
```
$ kubectl rollout history deployment/nginx-deployment
deployment.apps/nginx-deployment 
REVISION  CHANGE-CAUSE
1         <none>
3         <none>
4         kubectl set image deployment/nginx-deployment nginx-container=nginx:1.20 --record=true
5         Updating to alpine version
```

# This is like Rollout works 

$ kubectl rollout history deployment/nginx-deployment
```
$ kubectl rollout history deployment/nginx-deployment
deployment.apps/nginx-deployment 
REVISION  CHANGE-CAUSE
1         <none>
3         <none>
4         kubectl set image deployment/nginx-deployment nginx-container=nginx:1.20 --record=true
5         Updating to alpine version
```

$ kubectl rollout undo deployment/nginx-deployment --to-revision=1
```
$ kubectl rollout undo deployment/nginx-deployment --to-revision=1
deployment.apps/nginx-deployment rolled back
```

$ kubectl rollout history deployment/nginx-deployment
```
$ kubectl rollout history deployment/nginx-deployment
deployment.apps/nginx-deployment 
REVISION  CHANGE-CAUSE
3         <none>
4         kubectl set image deployment/nginx-deployment nginx-container=nginx:1.20 --record=true
5         Updating to alpine version
6         <none>
```

# Check the status for roll out 

$ kubectl rollout status deployment/nginx-deployment
```
$ kubectl rollout status deployment/nginx-deployment
deployment "nginx-deployment" successfully rolled out
```

$ kubectl get all 
```
$ kubectl get all 
NAME                                    READY   STATUS    RESTARTS   AGE
pod/nginx-deployment-654b769bc9-5xhg7   1/1     Running   0          3m8s
pod/nginx-deployment-654b769bc9-b99pp   1/1     Running   0          3m12s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   41m

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   2/2     2            2           37m

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deployment-5d574fdbbf   0         0         0       23m
replicaset.apps/nginx-deployment-654b769bc9   2         2         2       37m
replicaset.apps/nginx-deployment-6b8dbb4d65   0         0         0       10m
replicaset.apps/nginx-deployment-6d77cc77db   0         0         0       17m
```

# Cross check the images --> suppose to be latest one 
$ kubectl describe pod nginx-deployment-654b769bc9-5xhg7 | grep image
```
$ kubectl describe pod nginx-deployment-654b769bc9-5xhg7 | grep image
  Normal  Pulling    4m19s  kubelet            Pulling image "nginx:latest"
  Normal  Pulled     4m17s  kubelet            Successfully pulled image "nginx:latest" in 2.258209876s (2.258253293s including waiting)
```
---------------------------------------------------------------------------------------------

# Services | ClusterIP vs NodePort vs LoadBalancer Service | Load Balancing Hands-on

[Services](https://www.youtube.com/watch?v=zCW1TewkoOk&list=PLrMP04WSdCjrkNYSFvFeiHrfpsSVDFMDR&index=7)

