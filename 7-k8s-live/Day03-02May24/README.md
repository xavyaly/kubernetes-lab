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