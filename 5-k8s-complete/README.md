# K8s: End to End 

# PODs: 
```
Pods are the smallest deployable units of computing that you can create and manage
in Kubernetes.
A pod is a group of containers that run on a Kubernetes cluster, which is a collection of pods. A container is a package of software that holds your application or service and everything it needs to run.
```

# Types of PODs

# Single Container PODs:

```
One container per POD, When you deploy your applications and you’ve got a web container and an app container,  each of them could be in their own pods.
```

# Examples:
$ kubectl run nginx --image=nginx:alpine
```
$ kubectl run nginx --image=nginx:alpine
pod/nginx created
$ kubectl run tomcat --image=tomcat:8.0
pod/tomcat created
$ kubectl get pods
NAME                               READY   STATUS              RESTARTS      AGE
my-web-app-79b94d5979-f4jkr        1/1     Running             2 (36d ago)   89d
my-web-app-79b94d5979-kh2cs        1/1     Running             2 (36d ago)   89d
my-web-app-79b94d5979-mvnlc        1/1     Running             2 (36d ago)   89d
nginx                              1/1     Running             0             17s
nginx-deployment-fdcbb5f99-8zvdx   1/1     Running             1 (36d ago)   43d
nginx-deployment-fdcbb5f99-tcm8c   1/1     Running             1 (36d ago)   43d
tomcat                             0/1     ContainerCreating   0             3s
$ kubectl describe pods tomcat | grep ImagePullBackOff
  Warning  Failed     8s (x2 over 39s)   kubelet            Error: ImagePullBackOff
```

OR

$ cat nginx.yaml
```
apiVersion: v1          # version of the API to use
kind: Pod               # kind of the object while deploying
metadata:               # information of the object we're deploying
  name: nginx
  labels:
    name: nginx
spec:                   # specification for our object 
  containers:
  - name: nginx         # the name of the container within the POD
    image: nginx:alpine # which container image should be pulled
    resources:
      limits:
        memory: "128Mi"
        cpu: "500m"
    ports:
      - containerPort: 8080 # the port of the container within the POD
    imagePullPolicy: Always

$ kubectl create -f nginx.yaml 
pod/nginx created

$ kubectl delete pod nginx
pod "nginx" deleted
```

# Multi Container PODs:

$ cat nginx-mongo.yaml 
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    name: nginx 
spec:
  containers:
  - name: nginx
    image: nginx:alpine
    # resources:
    #   limits:
    #     memory: "128Mi"
    #     cpu: "500m"
    ports:
      - containerPort: 7501
    imagePullPolicy: Always
  - name: database
    image: mongo:7.0.5-rc0-jammy    # https://hub.docker.com/_/mongo/tags
    # resources:
    #   limits:
    #     memory: "128Mi"
    #     cpu: "500m"
    ports:
      - containerPort: 7502
    imagePullPolicy: Always
      
$ kubectl create -f nginx-mongoDB.yaml 
pod/nginx created

$ kubectl get pods -o wide
NAME                               READY   STATUS    RESTARTS      AGE     IP            NODE       NOMINATED NODE   READINESS GATES
nginx                              2/2     Running   0             3m51s   10.244.0.66   minikube   <none>           <none>
```

# Refer errors.md for the errors we encountered 
```
ImagePullBackOff
ErrImagePull
```

# Resolutions: image name was wrong

-------------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------------

# ReplicaSet:

% A pod could die for all kinds of reasons such as a node that it was running on had failed, it ran out of resources, 
% it was stopped for some reason, etc. If the pod dies, it stays dead until someone fixes it which is not ideal, but with containers we should expect them to be short lived anyway, 

$ cat replicaset.yaml
```

```


-------------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------------

Deployments:

Service and Labels:

Endpoints:

Service Publishing:

Namespaces:

Context:

DNS:

ConfigMaps:

Secrets:

Persistent Volume:

Cloud Providers and Storage Classes:

Stateful Sets:

Role Based Access:

POD Backups:

HELM Charts:

Taints and Tolerations:

Daemon Sets:

Network Policies:

POD Security Policies:

Resource Requests and Limits:

POD Autoscaling:

Liveness and Readiness Probes:

Validating Admission Controllers:

Jobs and CronJobs:


