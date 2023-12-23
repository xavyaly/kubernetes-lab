kubectl get pods -o wide
```
$ kubectl get pods -o wide
NAME                               READY   STATUS             RESTARTS      AGE     IP            NODE       NOMINATED NODE   READINESS GATES
my-web-app-79b94d5979-f4jkr        1/1     Running            2 (36d ago)   89d     10.244.0.52   minikube   <none>           <none>
my-web-app-79b94d5979-kh2cs        1/1     Running            2 (36d ago)   89d     10.244.0.57   minikube   <none>           <none>
my-web-app-79b94d5979-mvnlc        1/1     Running            2 (36d ago)   89d     10.244.0.43   minikube   <none>           <none>
nginx                              1/2     ImagePullBackOff   0             3m17s   10.244.0.63   minikube   <none>           <none>
nginx-deployment-fdcbb5f99-8zvdx   1/1     Running            1 (36d ago)   43d     10.244.0.45   minikube   <none>           <none>
nginx-deployment-fdcbb5f99-tcm8c   1/1     Running            1 (36d ago)   43d     10.244.0.42   minikube   <none>           <none>
```

$ kubectl describe pod nginx
```
$ kubectl describe pod nginx
Name:             nginx
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/10.0.2.15
Start Time:       Sun, 24 Dec 2023 00:00:07 +0530
Labels:           name=nginx
Annotations:      <none>
Status:           Pending
IP:               10.244.0.63
IPs:
  IP:  10.244.0.63
Containers:
  nginx:
    Container ID:   docker://b238ff414b0742dd6c03bdcdf8dca41c98316e1595dcee9389155718408172f1
    Image:          nginx:alpine
    Image ID:       docker-pullable://nginx@sha256:a59278fd22a9d411121e190b8cec8aa57b306aa3332459197777583beb728f59
    Port:           7501/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Sun, 24 Dec 2023 00:00:11 +0530
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-kx6q6 (ro)
  database:
    Container ID:   
    Image:          mongodb
    Image ID:       
    Port:           7502/TCP
    Host Port:      0/TCP
    State:          Waiting
      Reason:       ImagePullBackOff
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-kx6q6 (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             False 
  ContainersReady   False 
  PodScheduled      True 
Volumes:
  kube-api-access-kx6q6:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason     Age                    From               Message
  ----     ------     ----                   ----               -------
  Normal   Scheduled  3m21s                  default-scheduler  Successfully assigned default/nginx to minikube
  Normal   Pulling    3m20s                  kubelet            Pulling image "nginx:alpine"
  Normal   Pulled     3m18s                  kubelet            Successfully pulled image "nginx:alpine" in 2.434292302s (2.434342343s including waiting)
  Normal   Created    3m18s                  kubelet            Created container nginx
  Normal   Started    3m17s                  kubelet            Started container nginx
  Warning  Failed     2m26s (x3 over 3m14s)  kubelet            Error: ErrImagePull
  Normal   BackOff    106s (x5 over 3m14s)   kubelet            Back-off pulling image "mongodb"
  Warning  Failed     106s (x5 over 3m14s)   kubelet            Error: ImagePullBackOff
  Normal   Pulling    94s (x4 over 3m17s)    kubelet            Pulling image "mongodb"
  Warning  Failed     92s (x4 over 3m14s)    kubelet            Failed to pull image "mongodb": rpc error: code = Unknown desc = Error response from daemon: pull access denied for mongodb, repository does not exist or may require 'docker login': denied: requested access to the resource is denied
```

$ kubectl get pods -o wide
```
$ kubectl get pods -o wide
NAME                               READY   STATUS         RESTARTS      AGE   IP            NODE       NOMINATED NODE   READINESS GATES
my-web-app-79b94d5979-f4jkr        1/1     Running        2 (36d ago)   89d   10.244.0.52   minikube   <none>           <none>
my-web-app-79b94d5979-kh2cs        1/1     Running        2 (36d ago)   89d   10.244.0.57   minikube   <none>           <none>
my-web-app-79b94d5979-mvnlc        1/1     Running        2 (36d ago)   89d   10.244.0.43   minikube   <none>           <none>
nginx                              1/2     ErrImagePull   0             15s   10.244.0.64   minikube   <none>           <none>
nginx-deployment-fdcbb5f99-8zvdx   1/1     Running        1 (36d ago)   43d   10.244.0.45   minikube   <none>           <none>
nginx-deployment-fdcbb5f99-tcm8c   1/1     Running        1 (36d ago)   43d   10.244.0.42   minikube   <none>           <none>
```

$ kubectl describe pod nginx
```
$ kubectl describe pod nginx
Name:             nginx
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/10.0.2.15
Start Time:       Sun, 24 Dec 2023 00:04:49 +0530
Labels:           name=nginx
Annotations:      <none>
Status:           Pending
IP:               10.244.0.64
IPs:
  IP:  10.244.0.64
Containers:
  nginx:
    Container ID:   docker://e7006484388d3fe5bb575508db005ccb75561887377aa5a08e2c43381e463884
    Image:          nginx:alpine
    Image ID:       docker-pullable://nginx@sha256:a59278fd22a9d411121e190b8cec8aa57b306aa3332459197777583beb728f59
    Port:           7501/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Sun, 24 Dec 2023 00:04:57 +0530
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-xzl9x (ro)
  database:
    Container ID:   
    Image:          mongodb
    Image ID:       
    Port:           7502/TCP
    Host Port:      0/TCP
    State:          Waiting
      Reason:       ErrImagePull
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-xzl9x (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             False 
  ContainersReady   False 
  PodScheduled      True 
Volumes:
  kube-api-access-xzl9x:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason     Age   From               Message
  ----     ------     ----  ----               -------
  Normal   Scheduled  20s   default-scheduler  Successfully assigned default/nginx to minikube
  Normal   Pulling    20s   kubelet            Pulling image "nginx:alpine"
  Normal   Pulled     12s   kubelet            Successfully pulled image "nginx:alpine" in 7.674390323s (7.674413947s including waiting)
  Normal   Created    12s   kubelet            Created container nginx
  Normal   Started    12s   kubelet            Started container nginx
  Normal   Pulling    12s   kubelet            Pulling image "mongodb"
  Warning  Failed     9s    kubelet            Failed to pull image "mongodb": rpc error: code = Unknown desc = Error response from daemon: pull access denied for mongodb, repository does not exist or may require 'docker login': denied: requested access to the resource is denied
  Warning  Failed     9s    kubelet            Error: ErrImagePull
  Normal   BackOff    8s    kubelet            Back-off pulling image "mongodb"
  Warning  Failed     8s    kubelet            Error: ImagePullBackOff
```