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

[Link](https://www.youtube.com/watch?v=dSOS-7SZ_BA&list=PLrMP04WSdCjrkNYSFvFeiHrfpsSVDFMDR&index=3)

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
```
$ minikube version
minikube version: v1.30.1
commit: 08896fd1dc362c097c925146c4a0d0dac715ace0
```




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
---------------------------------------------------------------------------------------------

