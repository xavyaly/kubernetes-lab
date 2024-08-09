
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



