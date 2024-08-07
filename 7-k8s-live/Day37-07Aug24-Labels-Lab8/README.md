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