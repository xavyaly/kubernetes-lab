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



---------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------

