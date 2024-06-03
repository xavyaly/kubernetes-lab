Minikube & Docker Installation:

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

------