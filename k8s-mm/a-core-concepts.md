Create a new pod with the nginx image.

controlplane ~ ✖ kubectl run nginx --image=nginx

controlplane ~ ➜  kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          12s

What is the image used to create the new pods?
controlplane ~ ➜  kubectl describe pods newpods-vsdtx

Which nodes are these pods placed on?
controlplane ~ ✖ kubectl describe pods newpods-vsdtx
Node:         controlplane/172.25.0.13

------------------------------------------------------

Example: 
pod-definition.yml 
apiVersion: v1 
kind: Pod 
metadata: 
  name: myapp-pod 
  lebels: 
    app: myapp 
    type: front-end 
spec: 
  containers:
  - name: nginx-container 
    image: nginx 

rc-definition.yml 
apiVersion: v1  
kind: ReplicationController 
metadata: 
  name: myapp-rc  
  lebels: 
    app: myapp 
    type: front-end 
spec:
  template: 
    metadata: 
      name: myapp-pod 
      lebels: 
        app: myapp 
        type: front-end 
    spec: 
      containers:
      - name: nginx-container 
        image: nginx
  replicas: 3 

$ kubectl create -f rc-definition.yml 
$ kubectl get ReplicationController
$ kubectl get pods      # created automatically by the rc 

------------------------------------------------------

replicaset-definition.yml 
apiVersion: apps/v1 
kind: ReplicaSet 
metadata: 
  name: myapp-replicaset 
  lebels: 
    app: myapp 
    type: front-end 
spec: 
  template: 
    metadata: 
      name: myapp-pod 
      lebels: 
        app: myapp 
        type: front-end 
    spec: 
      containers:
      - name: nginx-container 
        image: nginx
  replicas: 3
  selector:                     # mandatory field and it is the only difference between rs and rc 
    matchLabels:
      type: front-end 

$ kubectl create -f replicaset-definition.yml 
$ kubectl get replicaset    # list of rs 
$ kubectl get pods 

------------------------------------------------------

replicaset: monitors the pods 

------------------------------------------------------

replicaset-definition.yml 
apiVersion: apps/v1 
kind: ReplicaSet 
metadata: 
  name: myapp-replicaset 
  lebels: 
    app: myapp 
    type: front-end 
spec: 
  template: 
    metadata: 
      name: myapp-pod 
      lebels: 
        app: myapp 
        type: front-end 
    spec: 
      containers:
      - name: nginx-container 
        image: nginx
  replicas: 3
  selector:                     # mandatory field and it is the only difference between rs and rc 
    matchLabels:
      type: front-end 

$ kubectl replace -f replicaset-definition.yml              # replicas: 6 
$ kubectl scale --replicas=6 -f replicaset-definition.yml   # original file won't change 
$ kubectl scale --replicas=6 replicaset my-app-replicaset   # type and name will be changed at here 

$ kubectl delete replicaset myapp-replicaset                # also delete underlysing PODs 

------------------------------------------------------

How many PODs exist on the system?
controlplane ~ ➜  kubectl get pods

How many ReplicaSets exist on the system?
controlplane ~ ➜  kubectl get replicasets

------------------------------------------------------

Create a ReplicaSet using the replicaset-definition-1.yaml file located at /root/.
controlplane ~ ➜  cat replicaset-definition-1.yaml 
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: replicaset-1
spec:
  replicas: 2
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: nginx
        image: nginx

controlplane ~ ➜  kubectl create -f replicaset-definition-1.yaml 

------------------------------------------------------

Fix the issue in the replicaset-definition-2.yaml file and create a ReplicaSet using it.
This file is located at /root/.
Name: replicaset-2

controlplane ~ ➜  kubectl create -f replicaset-definition-2.yaml 
The ReplicaSet "replicaset-2" is invalid: spec.template.metadata.labels: Invalid value: map[string]string{"tier":"nginx"}: `selector` does not match template `labels`

controlplane ~ ✖ cat replicaset-definition-2.yaml 
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: replicaset-2
spec:
  replicas: 2
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: nginx
    spec:
      containers:
      - name: nginx
        image: nginx

next 

controlplane ~ ➜  cat replicaset-definition-2.yaml 
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: replicaset-2
spec:
  replicas: 2
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: nginx
        image: nginx

controlplane ~ ➜  kubectl create -f replicaset-definition-2.yaml 
replicaset.apps/replicaset-2 created

------------------------------------------------------

controlplane ~ ➜  kubectl get replicaset
NAME              DESIRED   CURRENT   READY   AGE
new-replica-set   4         4         0       13m
replicaset-1      2         2         2       5m47s
replicaset-2      2         2         2       49s

controlplane ~ ➜  

controlplane ~ ➜  kubectl get pods
NAME                    READY   STATUS             RESTARTS   AGE
replicaset-1-7fz77      1/1     Running            0          5m52s
replicaset-1-mtfzq      1/1     Running            0          5m52s
new-replica-set-vzb77   0/1     ImagePullBackOff   0          9m50s
new-replica-set-dftg6   0/1     ImagePullBackOff   0          13m
new-replica-set-frrj8   0/1     ImagePullBackOff   0          13m
new-replica-set-q8skm   0/1     ImagePullBackOff   0          13m
replicaset-2-9ctt2      1/1     Running            0          54s
replicaset-2-hnz2v      1/1     Running            0          54s

controlplane ~ ➜  

------------------------------------------------------

Delete the two newly created ReplicaSets - replicaset-1 and replicaset-2

controlplane ~ ➜  kubectl delete replicasets.apps replicaset-1 
replicaset.apps "replicaset-1" deleted

controlplane ~ ➜  kubectl delete replicasets.apps replicaset-2
replicaset.apps "replicaset-2" deleted

controlplane ~ ➜  

controlplane ~ ➜  kubectl get replicaset
NAME              DESIRED   CURRENT   READY   AGE
new-replica-set   4         4         0       14m

controlplane ~ ➜  

controlplane ~ ➜  kubectl get pods
NAME                    READY   STATUS             RESTARTS   AGE
new-replica-set-vzb77   0/1     ImagePullBackOff   0          10m
new-replica-set-dftg6   0/1     ImagePullBackOff   0          14m
new-replica-set-frrj8   0/1     ImagePullBackOff   0          14m
new-replica-set-q8skm   0/1     ImagePullBackOff   0          14m

controlplane ~ ➜  

------------------------------------------------------

Fix the original replica set new-replica-set to use the correct busybox image.

Either delete and recreate the ReplicaSet or Update the existing ReplicaSet and then delete all PODs, so new ones with the correct image will be created.

Replicas: 4

controlplane ~ ➜  kubectl get pods
NAME                    READY   STATUS             RESTARTS   AGE
new-replica-set-vzb77   0/1     ImagePullBackOff   0          18m
new-replica-set-dftg6   0/1     ImagePullBackOff   0          22m
new-replica-set-frrj8   0/1     ImagePullBackOff   0          22m
new-replica-set-q8skm   0/1     ImagePullBackOff   0          22m


controlplane ~ ➜  kubectl edit replicaset new-replica-set
# Please edit the object below. Lines beginning with a '#' will be ignored,
# and an empty file will abort the edit. If an error occurs while saving this file will be
# reopened with the relevant failures.
#
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  creationTimestamp: "2022-12-03T19:26:45Z"
  generation: 1
  name: new-replica-set
  namespace: default
  resourceVersion: "956"
  uid: cb49e99f-c02c-44dd-8cab-4445e7d7f8ca
spec:
  replicas: 4
  selector:
    matchLabels:
      name: busybox-pod
  template:
    metadata:
      creationTimestamp: null
      labels:
        name: busybox-pod
    spec:
      containers:
      - command:
        - sh
        - -c
        - echo Hello Kubernetes! && sleep 3600
        image: busybox777
        imagePullPolicy: Always
        name: busybox-container
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  fullyLabeledReplicas: 4
  observedGeneration: 1
  replicas: 4

change the image name only  

image: busybox777 -> image: busybox

controlplane ~ ✖ kubectl delete pods new-replica-set-vzb77 
pod "new-replica-set-vzb77" deleted

controlplane ~ ➜  kubectl delete pods new-replica-set-dftg6 
pod "new-replica-set-dftg6" deleted

controlplane ~ ➜  kubectl delete pods new-replica-set-frrj8 
pod "new-replica-set-frrj8" deleted

controlplane ~ ➜  kubectl delete pods new-replica-set-q8skm 
pod "new-replica-set-q8skm" deleted

controlplane ~ ➜  kubectl get pods
NAME                    READY   STATUS    RESTARTS   AGE
new-replica-set-m8q6h   1/1     Running   0          34s
new-replica-set-dwwb6   1/1     Running   0          27s
new-replica-set-d46pk   1/1     Running   0          20s
new-replica-set-nthgx   1/1     Running   0          7s

------------------------------------------------------

Scale the ReplicaSet to 5 PODs.

Use kubectl scale command or edit the replicaset using kubectl edit replicaset.

Replicas: 5

controlplane ~ ➜  kubectl get replicasets.apps 
NAME              DESIRED   CURRENT   READY   AGE
new-replica-set   4         4         4       27m

controlplane ~ ➜  

controlplane ~ ➜  kubectl get pods
NAME                    READY   STATUS    RESTARTS   AGE
new-replica-set-m8q6h   1/1     Running   0          3m24s
new-replica-set-dwwb6   1/1     Running   0          3m17s
new-replica-set-d46pk   1/1     Running   0          3m10s
new-replica-set-nthgx   1/1     Running   0          2m57s

Solution:

controlplane ~ ✖ kubectl scale --replicas=5 replicaset new-replica-set 
replicaset.apps/new-replica-set scaled

controlplane ~ ➜  

controlplane ~ ➜  kubectl get replicasets.apps 
NAME              DESIRED   CURRENT   READY   AGE
new-replica-set   5         5         5       29m

controlplane ~ ➜  

controlplane ~ ➜  kubectl get pods
NAME                    READY   STATUS    RESTARTS   AGE
new-replica-set-m8q6h   1/1     Running   0          5m1s
new-replica-set-dwwb6   1/1     Running   0          4m54s
new-replica-set-d46pk   1/1     Running   0          4m47s
new-replica-set-nthgx   1/1     Running   0          4m34s
new-replica-set-ks4jb   1/1     Running   0          9s

controlplane ~ ➜  

or

controlplane ~ ➜  kubectl edit replicasets.apps new-replica-set    # change the replicas to 5 
replicaset.apps/new-replica-set edited

-----------------------------------------------------

Now scale the ReplicaSet down to 2 PODs.

Use the kubectl scale command or edit the replicaset using kubectl edit replicaset.

Replicas: 2

Solution:

controlplane ~ ➜  kubectl scale --replicas=2 replicaset new-replica-set 
replicaset.apps/new-replica-set scaled

controlplane ~ ➜  

controlplane ~ ➜  kubectl get pods 
NAME                    READY   STATUS        RESTARTS   AGE
new-replica-set-dwwb6   1/1     Running       0          11m
new-replica-set-nthgx   1/1     Running       0          11m
new-replica-set-d46pk   1/1     Terminating   0          11m
new-replica-set-p98xj   1/1     Terminating   0          5m34s

controlplane ~ ➜  

controlplane ~ ➜  kubectl get replicasets.apps 
NAME              DESIRED   CURRENT   READY   AGE
new-replica-set   2         2         2       36m

-----------------------------------------------------

deployment: create a complete object 

kubectl create -f deployment-definition.yml
kubectl get deployment 
kubectl get replicaset 
kubectl get pods 

OR 

kubectl get all 

------------------------------------------------------

SERVICES

How many Services exist on the system?

controlplane ~ ✖ kubectl get services

What is the type of the default kubernetes service?

LoadBalancer; NodePort; ClusterIP; External 

Q: Service TargetPort ??

controlplane ~ ➜  kubectl describe services kubernetes 
Name:              kubernetes
Namespace:         default
Labels:            component=apiserver
                   provider=kubernetes
Annotations:       <none>
Selector:          <none>
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                10.43.0.1
IPs:               10.43.0.1
Port:              https  443/TCP
TargetPort:        6443/TCP
Endpoints:         10.55.213.6:6443
Session Affinity:  None
Events:            <none>

How many labels are configured on the kubernetes service?

controlplane ~ ➜  kubectl describe services kubernetes 

How many Endpoints are attached on the kubernetes service?

How many Deployments exist on the system now? 1 

controlplane ~ ➜  kubectl get deployments
NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
simple-webapp-deployment   4/4     4            4           10s


What is the image used to create the pods in the deployment?

controlplane ~ ➜  kubectl get rs
NAME                                 DESIRED   CURRENT   READY   AGE
simple-webapp-deployment-8c46cb57c   4         4         4       50s

controlplane ~ ➜  kubectl get pods
NAME                                       READY   STATUS    RESTARTS   AGE
simple-webapp-deployment-8c46cb57c-4nrss   1/1     Running   0          67s
simple-webapp-deployment-8c46cb57c-8kqbb   1/1     Running   0          67s
simple-webapp-deployment-8c46cb57c-l8mk6   1/1     Running   0          67s
simple-webapp-deployment-8c46cb57c-2fnh9   1/1     Running   0          67s

controlplane ~ ➜  kubectl describe pods simple-webapp-deployment-8c46cb57c-8kqbb 


Are you able to accesss the Web App UI? 

https://30080-port-6763ccb84f5e4e20.labs.kodekloud.com/ -> No 

END 

Create a new service to access the web application using the service-definition-1.yaml file.

HINTS Provided

Name: webapp-service
Type: NodePort
targetPort: 8080
port: 8080
nodePort: 30080
selector:
name: simple-webapp

Existed services 

controlplane ~ ➜  kubectl get service
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.43.0.1    <none>        443/TCP   22m

controlplane ~ ➜  kubectl get deployments.apps 
NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
simple-webapp-deployment   4/4     4            4           6m30s

controlplane ~ ➜  kubectl get rs 
NAME                                 DESIRED   CURRENT   READY   AGE
simple-webapp-deployment-8c46cb57c   4         4         4       6m36s

controlplane ~ ➜  kubectl get pods
NAME                                       READY   STATUS    RESTARTS   AGE
simple-webapp-deployment-8c46cb57c-4nrss   1/1     Running   0          6m41s
simple-webapp-deployment-8c46cb57c-8kqbb   1/1     Running   0          6m41s
simple-webapp-deployment-8c46cb57c-l8mk6   1/1     Running   0          6m41s
simple-webapp-deployment-8c46cb57c-2fnh9   1/1     Running   0          6m41s

Manually created 

controlplane ~ ➜  cat service-definition-1.yaml  -> created manually with the above hints 
---
apiVersion: v1
kind: Service
metadata:
  name: webapp-service
spec:
  type: NodePort
  ports:
    - targetPort: 8080
      port: 8080
      nodePort: 30080
  selector:
    name: simpleweb

controlplane ~ ➜  kubectl create -f service-definition-1.yaml 
service/webapp-service created

controlplane ~ ➜  kubectl get services
NAME             TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
kubernetes       ClusterIP   10.43.0.1       <none>        443/TCP          28m
webapp-service   NodePort    10.43.220.254   <none>        8080:30080/TCP   4s

END 

NAMESPACES

How many Namespaces exist on the system?

controlplane ~ ➜  kubectl get namespaces 
NAME              STATUS   AGE
kube-system       Active   9m47s
default           Active   9m47s
kube-public       Active   9m47s
kube-node-lease   Active   9m47s
finance           Active   72s
marketing         Active   72s
dev               Active   72s
prod              Active   72s
manufacturing     Active   72s
research          Active   71s


How many pods exist in the research namespace?

controlplane ~ ➜  kubectl get pods --namespace=research 
NAME    READY   STATUS             RESTARTS      AGE
dna-1   0/1     CrashLoopBackOff   4 (28s ago)   2m13s
dna-2   0/1     CrashLoopBackOff   4 (27s ago)   2m13s


Create a POD in the finance namespace.
Name: redis
Image Name: redis

controlplane ~ ➜  kubectl run redis --image=redis -n=finance
controlplane ~ ✖ kubectl get pods --namespace=finance 


Which namespace has the blue pod in it?

controlplane ~ ➜  kubectl get pods --all-namespaces 

END

IMPERATIVE AND DECLARATIVE COMMANDS:

IMPERATIVE -> CREATE OBJECTS:
kubectl run --image=nginx nginx 
kubectl create deployment --image=nginx nginx 
kubectl expose deployment nginx port 80

DECLARATIVE -> CREATE OBJECTS:
kubectl apply -f nginx.yml 

IMPERATIVE -> UPDATE OBJECTS:
kubectl edit deployment nginx 
kubectl scale deployment nginx --replicas 5
kubectl set image deployment nginx nginx=nginx:1.18 

DECLARATIVE -> CREATE OBJECTS:
kubectl apply -f nginx.yml 

Certification Tips - Imperative Commands with Kubectl

While you would be working mostly the declarative way - using definition files, imperative commands can help in getting one time tasks done quickly, as well as generate a definition template easily. This would help save considerable amount of time during your exams.

Before we begin, familiarize with the two options that can come in handy while working with the below commands:

--dry-run: By default as soon as the command is run, the resource will be created. If you simply want to test your command , use the --dry-run=client option. This will not create the resource, instead, tell you whether the resource can be created and if your command is right.

-o yaml: This will output the resource definition in YAML format on screen.

Use the above two in combination to generate a resource definition file quickly, that you can then modify and create resources as required, instead of creating the files from scratch.


POD

Create an NGINX Pod

kubectl run nginx --image=nginx

Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)

kubectl run nginx --image=nginx --dry-run=client -o yaml


Deployment:

Create a deployment

kubectl create deployment --image=nginx nginx

Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)

kubectl create deployment --image=nginx nginx --dry-run=client -o yaml

Generate Deployment with 4 Replicas

kubectl create deployment nginx --image=nginx --replicas=4

You can also scale a deployment using the kubectl scale command.

kubectl scale deployment nginx --replicas=4

Another way to do this is to save the YAML definition to a file and modify

kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > nginx-deployment.yaml

You can then update the YAML file with the replicas or any other field before creating the deployment.


Service:

Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379

kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml

(This will automatically use the pod's labels as selectors)

Or

kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml (This will not use the pods labels as selectors, instead it will assume selectors as app=redis. You cannot pass in selectors as an option. So it does not work very well if your pod has a different label set. So generate the file and modify the selectors before creating the service)

Create a Service named nginx of type NodePort to expose pod nginx's port 80 on port 30080 on the nodes:

kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service --dry-run=client -o yaml

(This will automatically use the pod's labels as selectors, but you cannot specify the node port. You have to generate a definition file and then add the node port in manually before creating the service with the pod.)

Or

kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml

(This will not use the pods labels as selectors)

Both the above commands have their own challenges. While one of it cannot accept a selector the other cannot accept a node port. I would recommend going with the kubectl expose command. If you need to specify a node port, generate a definition file using the same command and manually input the nodeport before creating the service.

Reference:

https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands

https://kubernetes.io/docs/reference/kubectl/conventions/


Practice:

In this lab, you will get hands-on practice with creating Kubernetes objects imperatively.

All the questions in this lab can be done imperatively. 
However, for some questions, you may need to first create the YAML file using imperative methods. 
You can then modify the YAML according to the need and create the object using kubectl apply -f command.


Deploy a pod named nginx-pod using the nginx:alpine image.
Name: nginx=pod 
Image: nginx:alpine 

controlplane ~ ➜  kubectl run nginx-pod --image=nginx:alpine
pod/nginx-pod created

controlplane ~ ➜  kubectl get pods
NAME        READY   STATUS    RESTARTS   AGE
nginx-pod   1/1     Running   0          49s


Deploy a redis pod using the redis:alpine image with the labels set to tier=db.

Either use imperative commands to create the pod with the labels. 
Or else use imperative commands to generate the pod definition file, 
then add the labels before creating the pod using the file.

Pod Name: redis
Image: redis:alpine
Labels: tier=db

controlplane ~ ➜  kubectl run redis --image=redis:alpine --dry-run=client -o yaml > redis-pod.yaml

controlplane ~ ➜  kubectl get pods
NAME        READY   STATUS    RESTARTS   AGE
nginx-pod   1/1     Running   0          6m55s

controlplane ~ ➜  ls
redis-pod.yaml  sample.yaml

controlplane ~ ➜  cat redis-pod.yaml 
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: redis
  name: redis
spec:
  containers:
  - image: redis:alpine
    name: redis
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

controlplane ~ ➜  vim redis-pod.yaml 
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: redis
    tier: db              # added 
  name: redis
spec:
  containers:
  - image: redis:alpine
    name: redis
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

controlplane ~ ➜  kubectl get pods
NAME        READY   STATUS    RESTARTS   AGE
nginx-pod   1/1     Running   0          10m

controlplane ~ ➜  

controlplane ~ ➜  kubectl create -f redis-pod.yaml 
pod/redis created

controlplane ~ ➜  kubectl get pods
NAME        READY   STATUS              RESTARTS   AGE
nginx-pod   1/1     Running             0          11m
redis       0/1     ContainerCreating   0          5s

controlplane ~ ➜  

OR 

controlplane ~ ➜  kubectl delete pods redis
pod "redis" deleted

controlplane ~ ➜  

controlplane ~ ➜  kubectl run redis -l tier=db --image=redis:alpine
pod/redis created

controlplane ~ ➜  

controlplane ~ ➜  kubectl get pods
NAME        READY   STATUS              RESTARTS   AGE
nginx-pod   1/1     Running             0          13m
redis       0/1     ContainerCreating   0          5s

controlplane ~ ➜  


Create a service redis-service to expose the redis application within the cluster on port 6379.

Use imperative commands.
Service: redis-service
Port: 6379
Type: ClusterIP

controlplane ~ ➜  kubectl expose pod redis --port=6379 --name redis-service
service/redis-service exposed

controlplane ~ ➜  

controlplane ~ ➜  kubectl get servicesNAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
kubernetes      ClusterIP   10.43.0.1       <none>        443/TCP    34m
redis-service   ClusterIP   10.43.186.205   <none>        6379/TCP   9s


Create a deployment named webapp using the image kodekloud/webapp-color with 3 replicas.

Try to use imperative commands only. Do not create definition files.

Name: webapp
Image: kodekloud/webapp-color
Replicas: 3

controlplane ~ ➜  kubectl create deployment --name=webapp --image=kodekloud/webapp-color --replicas=3 
error: unknown flag: --name
See 'kubectl create deployment --help' for usage.

controlplane ~ ✖ kubectl create deployment webapp --image=kodekloud/webapp-color --replicas=3 
deployment.apps/webapp created

controlplane ~ ➜  kubectl get deployments.apps 
NAME     READY   UP-TO-DATE   AVAILABLE   AGE
webapp   0/3     3            0           8s

controlplane ~ ➜  kubectl get rs
NAME                DESIRED   CURRENT   READY   AGE
webapp-79c4cc587b   3         3         3       72s

controlplane ~ ➜  kubectl get pods
NAME                      READY   STATUS    RESTARTS   AGE
nginx-pod                 1/1     Running   0          28m
redis                     1/1     Running   0          15m
webapp-79c4cc587b-ssnkf   1/1     Running   0          43s
webapp-79c4cc587b-z2ttb   1/1     Running   0          43s
webapp-79c4cc587b-w75cs   1/1     Running   0          43s


Create a new pod called custom-nginx using the nginx image and expose it on container port 8080.

controlplane ~ ✖ kubectl run custom-nginx --image=nginx --port=8080pod/custom-nginx created

controlplane ~ ➜  

controlplane ~ ➜  kubectl get pods
NAME                      READY   STATUS              RESTARTS   AGE
nginx-pod                 1/1     Running             0          34m
redis                     1/1     Running             0          21m
webapp-79c4cc587b-ssnkf   1/1     Running             0          6m3s
webapp-79c4cc587b-z2ttb   1/1     Running             0          6m3s
webapp-79c4cc587b-w75cs   1/1     Running             0          6m3s
custom-nginx              0/1     ContainerCreating   0          6s


Create a new namespace called dev-ns.

Use imperative commands.

controlplane ~ ➜  kubectl create ns dev-ns
namespace/dev-ns created

controlplane ~ ➜  

controlplane ~ ➜  kubectl get ns
NAME              STATUS   AGE
kube-system       Active   44m
default           Active   44m
kube-public       Active   44m
kube-node-lease   Active   44m
dev-ns            Active   9s

controlplane ~ ➜  

controlplane ~ ➜  kubectl describe pods --namespace dev-ns
No resources found in dev-ns namespace.


Create a new deployment called redis-deploy in the dev-ns namespace with the redis image. It should have 2 replicas.
Use imperative commands.

'redis-deploy' created in the 'dev-ns' namespace?
replicas: 2

controlplane ~ ➜  kubectl get deployments.apps 
NAME     READY   UP-TO-DATE   AVAILABLE   AGE
webapp   3/3     3            3           8m53s

controlplane ~ ➜  kubectl create deployment redis-deploy --namespace=dev-ns redis --replicas=2 
error: required flag(s) "image" not set

controlplane ~ ✖ 

controlplane ~ ✖ kubectl create deployment redis-deploy --namespace=dev-ns --image=redis --replicas=2 
deployment.apps/redis-deploy created

controlplane ~ ➜  

controlplane ~ ➜  kubectl get deployments.apps 
NAME     READY   UP-TO-DATE   AVAILABLE   AGE
webapp   3/3     3            3           10m

controlplane ~ ➜  kubectl get deployments --namespace=dev-ns
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
redis-deploy   2/2     2            2           71s

controlplane ~ ➜  

OR

Step 1: Create the deployment YAML file
kubectl create deployment redis-deploy --image redis --namespace=dev-ns --dry-run=client -o yaml > deploy.yaml
Step 2: Edit the YAML file and add update the replicas to 2
Step 3: Run kubectl apply -f deploy.yaml to create the deployment in the dev-ns namespace.
You can also use kubectl scale deployment or kubectl edit deployment to change the number of replicas once the object has been created.

controlplane ~ ✖ kubectl delete deployments.apps redis-deploy --namespace=dev-ns
deployment.apps "redis-deploy" deleted

controlplane ~ ➜  kubectl create deployment redis-deploy --namespace=dev-ns --image=redis --dry-run=client -o yaml > deploy.yaml 

controlplane ~ ➜  vim deploy.yaml 

controlplane ~ ➜  

controlplane ~ ➜  kubectl apply -f deploy.yaml 
deployment.apps/redis-deploy created

controlplane ~ ➜  

controlplane ~ ➜  kubectl get deployments.apps 
NAME     READY   UP-TO-DATE   AVAILABLE   AGE
webapp   3/3     3            3           15m

controlplane ~ ➜ 



Create a pod called httpd using the image httpd:alpine in the default namespace. Next, create a service of type ClusterIP by the same name (httpd). The target port for the service should be 80.

Try to do this with as few steps as possible.

'httpd' pod created with the correct image?

'httpd' service is of type 'ClusterIP'?

'httpd' service uses correct target port 80?

'httpd' service exposes the 'httpd' pod?

controlplane ~ ✖ kubectl run httpd --image=https:alpine 
pod/httpd created

controlplane ~ ➜  kubectl get pods
NAME                      READY   STATUS              RESTARTS   AGE
nginx-pod                 1/1     Running             0          47m
redis                     1/1     Running             0          34m
webapp-79c4cc587b-ssnkf   1/1     Running             0          19m
webapp-79c4cc587b-z2ttb   1/1     Running             0          19m
webapp-79c4cc587b-w75cs   1/1     Running             0          19m
custom-nginx              1/1     Running             0          13m
pod                       0/1     ImagePullBackOff    0          2m25s
httpd                     0/1     ContainerCreating   0          4s

controlplane ~ ➜  

controlplane ~ ➜  kubectl run httpd --image=httpd:alpine --port=80 --expose
service/httpd created
pod/httpd created

controlplane ~ ➜  kubectl get pods httpd
NAME    READY   STATUS    RESTARTS   AGE
httpd   1/1     Running   0          2m11s

controlplane ~ ➜  kubectl get service httpd
NAME    TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
httpd   ClusterIP   10.43.31.38   <none>        80/TCP    2m23s

--------------------------------------------------------------------------------------------------------



