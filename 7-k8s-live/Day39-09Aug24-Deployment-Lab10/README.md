
# Kubernetes Deployment

'''
A Kubernetes Deployment provides declarative updates to applications and ensures that the desired state specified in the Deployment YAML is always met. It manages Replica Sets and provides functionalities such as rolling updates, rollbacks, scaling, and more.
'''

# Example of a Deployment

# Here's an example of a Kubernetes Deployment for an Nginx application:

'''
cat deployment.yaml 

apiVersion: apps/v1
kind: Deployment
metadata:
   name: deployment
spec:
   replicas: 3
   selector:     
    matchLabels:
     name: labels-pods
   template:
     metadata:
       name: meta-pods
       labels:
         name: labels-pods
     spec:
      containers:
        - name: ubuntu-dep
          image: ubuntu
          command: ["/bin/bash", "-c", "while true; do echo Initial version of deployment….; sleep 5; done"]
'''

'''
kubectl apply -f deployment.yaml 
deployment.apps/deployment created


kubectl get all
NAME                             READY   STATUS    RESTARTS   AGE
pod/deployment-86c7bb4d7-jbqch   1/1     Running   0          6m8s
pod/deployment-86c7bb4d7-nt69z   1/1     Running   0          6m8s
pod/deployment-86c7bb4d7-qg7dx   1/1     Running   0          6m8s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   7m30s

NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/deployment   3/3     3            3           6m8s

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/deployment-86c7bb4d7   3         3         3       6m8s


kubectl delete pods deployment-86c7bb4d7-jbqch
pod "deployment-86c7bb4d7-jbqch" deleted


kubectl get all
NAME                             READY   STATUS    RESTARTS   AGE
pod/deployment-86c7bb4d7-58kbk   1/1     Running   0          34s
pod/deployment-86c7bb4d7-nt69z   1/1     Running   0          8m3s
pod/deployment-86c7bb4d7-qg7dx   1/1     Running   0          8m3s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   9m25s

NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/deployment   3/3     3            3           8m3s

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/deployment-86c7bb4d7   3         3         3       8m3s


kubectl get pods -o wide
NAME                         READY   STATUS    RESTARTS   AGE     IP           NODE       NOMINATED NODE   READINESS GATES
deployment-86c7bb4d7-58kbk   1/1     Running   0          68s     10.244.0.6   minikube   <none>           <none>
deployment-86c7bb4d7-nt69z   1/1     Running   0          8m37s   10.244.0.5   minikube   <none>           <none>
deployment-86c7bb4d7-qg7dx   1/1     Running   0          8m37s   10.244.0.4   minikube   <none>           <none>


kubectl logs -f deployment-86c7bb4d7-58kbk --tail=5
Initial version of deployment….
Initial version of deployment….
Initial version of deployment….
Initial version of deployment….
Initial version of deployment….
Initial version of deployment….
^C

kubectl exec deployment-86c7bb4d7-58kbk -- cat /etc/os-release
PRETTY_NAME="Ubuntu 24.04 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo
'''

# Change done on new image

'''
cat deployment.yaml 

apiVersion: apps/v1
kind: Deployment
metadata:
   name: deployment
spec:
   replicas: 3
   selector:     
    matchLabels:
     name: labels-pods
   template:
     metadata:
       name: meta-pods
       labels:
         name: labels-pods
     spec:
      containers:
        - name: centos-dep
          image: centos
          command: ["/bin/bash", "-c", "while true; do echo Second version of deployment….; sleep 5; done"]
'''

'''
kubectl apply -f deployment.yaml 
deployment.apps/deployment configured


kubectl get all
NAME                             READY   STATUS    RESTARTS   AGE
pod/deployment-96db5d779-bvrz9   1/1     Running   0          89s
pod/deployment-96db5d779-nx5g8   1/1     Running   0          2m4s
pod/deployment-96db5d779-zchn5   1/1     Running   0          85s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   32m

NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/deployment   3/3     3            3           31m

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/deployment-86c7bb4d7   0         0         0       31m
replicaset.apps/deployment-96db5d779   3         3         3       2m4s


kubectl logs -f pod/deployment-96db5d779-bvrz9 --tail=5
Second version of deployment….
Second version of deployment….
Second version of deployment….
Second version of deployment….
Second version of deployment….
^C

kubectl exec pod/deployment-96db5d779-bvrz9 -- cat /etc/os-release
NAME="CentOS Linux"
VERSION="8"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="8"
PLATFORM_ID="platform:el8"
PRETTY_NAME="CentOS Linux 8"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:8"
HOME_URL="https://centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"
CENTOS_MANTISBT_PROJECT="CentOS-8"
CENTOS_MANTISBT_PROJECT_VERSION="8"


kubectl scale --replicas=5 deployment deployment
deployment.apps/deployment scaled


kubectl get all 
NAME                             READY   STATUS    RESTARTS   AGE
pod/deployment-96db5d779-bvrz9   1/1     Running   0          3m54s
pod/deployment-96db5d779-m4gwd   1/1     Running   0          27s
pod/deployment-96db5d779-nx5g8   1/1     Running   0          4m29s
pod/deployment-96db5d779-pmh4r   1/1     Running   0          27s
pod/deployment-96db5d779-zchn5   1/1     Running   0          3m50s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   35m

NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/deployment   5/5     5            5           33m

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/deployment-86c7bb4d7   0         0         0       33m
replicaset.apps/deployment-96db5d779   5         5         5       4m29s
'''

# Clean the resources

'''
kubectl delete deployment deployment 
deployment.apps "deployment" deleted

kubectl get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   4h41m
'''