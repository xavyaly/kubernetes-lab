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


