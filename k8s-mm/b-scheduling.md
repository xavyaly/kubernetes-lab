Manual Scheduling:

A pod definition file nginx.yaml is given. Create a pod using the file.
Only create the POD for now. We will inspect its status next.
Pod nginx Created

SOl:
controlplane ~ ➜  cat nginx.yaml 
---
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  -  image: nginx
     name: nginx

controlplane ~ ➜  kubectl create -f nginx.yaml 
pod/nginx created

controlplane ~ ➜  kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
nginx   0/1     Pending   0          6s

END

What is the status of the created POD?

Sol:

controlplane ~ ➜  kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
nginx   0/1     Pending   0          47s

END

Why is the POD in a pending state?
Inspect the environment for various kubernetes control plane components.

Sol: 

Run the command: 
kubectl get pods --namespace kube-system 
  to see the status of scheduler pod. We have removed the scheduler from this Kubernetes cluster. 
  As a result, as it stands, the pod will remain in a pending state forever.

controlplane ~ ✖ kubectl describe pod nginx 
Name:         nginx
Namespace:    default
Priority:     0
Node:         <none>
Labels:       <none>
Annotations:  <none>
Status:       Pending
......
......

controlplane ~ ➜  kubectl get namespace
NAME              STATUS   AGE
default           Active   17m
kube-node-lease   Active   17m
kube-public       Active   17m
kube-system       Active   17m

controlplane ~ ➜  kubectl get pods --namespace=kube-system 
NAME                                   READY   STATUS    RESTARTS   AGE
coredns-6d4b75cb6d-6h2x5               1/1     Running   0          18m
coredns-6d4b75cb6d-qgxjg               1/1     Running   0          18m
etcd-controlplane                      1/1     Running   0          18m
kube-apiserver-controlplane            1/1     Running   0          18m
kube-controller-manager-controlplane   1/1     Running   0          18m
kube-flannel-ds-9qjpm                  1/1     Running   0          17m
kube-flannel-ds-sddxb                  1/1     Running   0          18m
kube-proxy-chpv4                       1/1     Running   0          17m
kube-proxy-f5rnb                       1/1     Running   0          18m

controlplane ~ ➜  

controlplane ~ ➜  kubectl get pods --namespace=default 
NAME    READY   STATUS    RESTARTS   AGE
nginx   0/1     Pending   0          5m55s

No scheduler present 

END 

Manually schedule the pod on node01.
Delete and recreate the POD if necessary.
Status: Running
Pod: nginx

Sol: 

controlplane ~ ➜  kubectl get nodes 
NAME           STATUS   ROLES           AGE   VERSION
controlplane   Ready    control-plane   30m   v1.24.0
node01         Ready    <none>          29m   v1.24.0

controlplane ~ ➜  cat nginx.yaml 
---
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  -  image: nginx
     name: nginx
  nodeName: node01

controlplane ~ ➜  kubectl delete pods nginx 
pod "nginx" deleted

controlplane ~ ➜  

controlplane ~ ➜  kubectl create -f nginx.yaml 
pod/nginx created

controlplane ~ ➜  

controlplane ~ ➜  kubectl get pods -o wide
NAME    READY   STATUS    RESTARTS   AGE   IP           NODE     NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          11s   10.244.1.2   node01   <none>           <none>

END 

Now schedule the same pod on the controlplane node.
Delete and recreate the POD if necessary.

Status: Running
Pod: nginx
Node: controlplane?

Sol: 

controlplane ~ ➜  kubectl get nodes 
NAME           STATUS   ROLES           AGE   VERSION
controlplane   Ready    control-plane   30m   v1.24.0
node01         Ready    <none>          29m   v1.24.0

controlplane ~ ➜  kubectl get pods -o wide
NAME    READY   STATUS    RESTARTS   AGE     IP           NODE     NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          2m10s   10.244.1.2   node01   <none>           <none>

controlplane ~ ➜  kubectl delete pods nginx 
pod "nginx" deleted

controlplane ~ ➜  vim nginx.yaml 

controlplane ~ ➜  cat nginx.yaml 
---
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  -  image: nginx
     name: nginx
  nodeName: controlplane

controlplane ~ ➜  kubectl create -f nginx.yaml 
pod/nginx created

controlplane ~ ➜  kubectl get pods -o wide
NAME    READY   STATUS              RESTARTS   AGE   IP       NODE           NOMINATED NODE   READINESS GATES
nginx   0/1     ContainerCreating   0          9s    <none>   controlplane   <none>           <none>

END 

https://patroware.medium.com/how-to-schedule-a-pod-manually-685dbbc457e6

END

We have deployed a number of PODs. 
They are labelled with tier, env and bu. How many PODs exist in the dev environment (env)?
Use selectors to filter the output

Sol: 

