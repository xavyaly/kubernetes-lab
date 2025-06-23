# Practice Questions:

# List all the pods 

How many pods exist on the system?
In the current(default) namespace.

controlplane ~ ➜  kubectl get pods
No resources found in default namespace.

controlplane ~ ➜  kubectl get ns
NAME              STATUS   AGE
default           Active   2m38s
kube-node-lease   Active   2m38s
kube-public       Active   2m38s
kube-system       Active   2m38s

controlplane ~ ➜  kubectl get svc
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.43.0.1    <none>        443/TCP   2m44s

---

# Create a new POD with the nginx image

Create a new pod using the nginx image.

controlplane ~ ➜  kubectl run k8s-pod --image=nginx
pod/k8s-pod created

controlplane ~ ➜  kubectl get pods k8s-pod
NAME      READY   STATUS    RESTARTS   AGE
k8s-pod   1/1     Running   0          23s

---

# How many pods are created now?
# Note: We have created a few more pods. So please check again in the current(default) namespace.

controlplane ~ ➜  kubectl get pods
NAME            READY   STATUS    RESTARTS   AGE
k8s-pod         1/1     Running   0          101s
newpods-gmxxt   1/1     Running   0          2m
newpods-rppnh   1/1     Running   0          2m
newpods-xqqq7   1/1     Running   0          2m

---

# Which image is specified for the pods whose names begin with the newpods- prefix?
# You must look at one of the new pods in detail to figure this out.

controlplane ~ ✖ kubectl describe pods newpods-gmxxt

---

# Which nodes are these pods placed on?
# You must look at all the pods in detail to figure this out.

controlplane ~ ✖ kubectl describe pods newpods-gmxxt | grep controlplane
Node:             controlplane/192.168.96.230
  Normal  Scheduled  6m3s  default-scheduler  Successfully assigned default/newpods-gmxxt to controlplane

---

# We just created a new pod named webapp. How many containers are part of the webapp pod?
# Note: Ignore the state of the pod for now.

controlplane ~ ✖ kubectl describe pods webapp
nginx:
agentx:

---

# What images are used in the new webapp pod?
# You must look at all the pods in detail to figure this out.

controlplane ~ ➜  kubectl describe pods webapp | grep image
  Normal   Pulling    3m17s                kubelet            Pulling image "nginx"
  Normal   Pulled     3m17s                kubelet            Successfully pulled image "nginx" in 184ms (184ms including waiting). Image size: 72406859 bytes.
  Normal   Pulling    19s (x5 over 3m17s)  kubelet            Pulling image "agentx"
  Warning  Failed     19s (x5 over 3m16s)  kubelet            Failed to pull image "agentx": failed to pull and unpack image "docker.io/library/agentx:latest": failed to resolve reference "docker.io/library/agentx:latest": pull access denied, repository does not exist or may require authorization: server message: insufficient_scope: authorization failed
  Normal   BackOff    8s (x13 over 3m16s)  kubelet            Back-off pulling image "agentx"

---

# What is the state of the container agentx in the pod webapp?
# Wait for it to finish the ContainerCreating state

controlplane ~ ➜  kubectl get pods webapp
NAME     READY   STATUS             RESTARTS   AGE
webapp   1/2     ImagePullBackOff   0          5m29s

controlplane ~ ➜  kubectl describe pods webapp | grep agentx:
  agentx:
  Warning  Failed     2m37s (x5 over 5m34s)  kubelet            Failed to pull image "agentx": failed to pull and unpack image "docker.io/library/agentx:latest": failed to resolve reference "docker.io/library/agentx:latest": pull access denied, repository does not exist or may require authorization: server message: insufficient_scope: authorization failed

---

# Why do you think the container agentx in pod webapp is in error?
# Try to figure it out from the events section of the pod.

controlplane ~ ➜  kubectl get pods webapp
NAME     READY   STATUS             RESTARTS   AGE
webapp   1/2     ImagePullBackOff   0          5m29s

controlplane ~ ➜  kubectl describe pods webapp | grep agentx:
  agentx:
  Warning  Failed     2m37s (x5 over 5m34s)  kubelet            Failed to pull image "agentx": failed to pull and unpack image "docker.io/library/agentx:latest": failed to resolve reference "docker.io/library/agentx:latest": pull access denied, repository does not exist or may require authorization: server message: insufficient_scope: authorization failed

Ans: Docker name with this image doesn't exist

---

# What does the READY column in the output of the kubectl get pods command indicate?

controlplane ~ ➜  kubectl get pods 
NAME            READY   STATUS             RESTARTS   AGE
k8s-pod         1/1     Running            0          11m
newpods-gmxxt   1/1     Running            0          11m
newpods-rppnh   1/1     Running            0          11m
newpods-xqqq7   1/1     Running            0          11m
webapp          1/2     ImagePullBackOff   0          5m17s

Ans: running containers in pod/total containers in pod 

---

# Delete the webapp Pod.
# Once deleted, wait for the pod to fully terminate.

controlplane ~ ➜  kubectl delete pods webapp 
pod "webapp" deleted

controlplane ~ ➜  kubectl get pods webapp
Error from server (NotFound): pods "webapp" not found

---

# Create a new pod with the name redis and the image redis123.
# Use a pod-definition YAML file. And yes the image name is wrong!

controlplane ~ ➜  kubectl get pods
NAME            READY   STATUS             RESTARTS        AGE
k8s-pod         1/1     Running            0               21m
newpods-gmxxt   1/1     Running            1 (5m10s ago)   21m
newpods-rppnh   1/1     Running            1 (5m10s ago)   21m
newpods-xqqq7   1/1     Running            1 (5m10s ago)   21m
redis           0/1     ImagePullBackOff   0               22s

---

# Now change the image on this pod to redis.
# Once done, the pod should be in a running state.

controlplane ~ ➜  kubectl run redis --image=redis
Error from server (AlreadyExists): pods "redis" already exists

controlplane ~ ✖ kubectl delete pods redis
pod "redis" deleted

controlplane ~ ➜  kubectl run redis --image=redis
pod/redis created

controlplane ~ ➜  kubectl get pods
NAME            READY   STATUS    RESTARTS        AGE
k8s-pod         1/1     Running   0               22m
newpods-gmxxt   1/1     Running   1 (6m28s ago)   23m
newpods-rppnh   1/1     Running   1 (6m28s ago)   23m
newpods-xqqq7   1/1     Running   1 (6m28s ago)   23m
redis           1/1     Running   0               5s

---