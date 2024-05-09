[Link](https://dev.to/apenney/kubernetes-namespaces-the-ultimate-guide-39ek)


'''
$ kubectl run nginx --image=nginx
pod/nginx created


$ kubectl get pods nginx
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          8s


$ kubectl get pods nginx -o wide
NAME    READY   STATUS    RESTARTS   AGE   IP             NODE       NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          13s   10.244.0.118   minikube   <none>           <none>


$ kubectl run nginx-teamA --image=nginx
The Pod "nginx-teamA" is invalid: 
* metadata.name: Invalid value: "nginx-teamA": a lowercase RFC 1123 subdomain must consist of lower case alphanumeric characters, '-' or '.', and must start and end with an alphanumeric character (e.g. 'example.com', regex used for validation is '[a-z0-9]([-a-z0-9]*[a-z0-9])?(\.[a-z0-9]([-a-z0-9]*[a-z0-9])?)*')
* spec.containers[0].name: Invalid value: "nginx-teamA": a lowercase RFC 1123 label must consist of lower case alphanumeric characters or '-', and must start and end with an alphanumeric character (e.g. 'my-name',  or '123-abc', regex used for validation is '[a-z0-9]([-a-z0-9]*[a-z0-9])?')


$ kubectl get namespace
NAME              STATUS   AGE
argocd            Active   225d
default           Active   226d
dev               Active   46m
ingress-nginx     Active   226d
kube-node-lease   Active   226d
kube-public       Active   226d
kube-system       Active   226d
nginx             Active   181d
todo              Active   181d

$ kubectl get ns
NAME              STATUS   AGE
argocd            Active   225d
default           Active   226d
dev               Active   47m
ingress-nginx     Active   226d
kube-node-lease   Active   226d
kube-public       Active   226d
kube-system       Active   226d
nginx             Active   181d
todo              Active   181d

$ kubectl create ns frontend
namespace/frontend created

$ kubectl get ns frontend
NAME       STATUS   AGE
frontend   Active   14s

$ kubectl run nginx --image=nginx --restart=Never --namespace=frontend
pod/nginx created

$ kubectl get pods --namespace=frontend
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          15s

$ kubectl get pods --namespace=frontend -o wide
NAME    READY   STATUS    RESTARTS   AGE   IP             NODE       NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          18s   10.244.0.119   minikube   <none>           <none>

$ kubectl describe ns frontend
Name:         frontend
Labels:       kubernetes.io/metadata.name=frontend
Annotations:  <none>
Status:       Active

No resource quota.

No LimitRange resource.


$ kubectl get ns frontend
NAME       STATUS   AGE
frontend   Active   4m36s


$ kubectl delete ns frontend
namespace "frontend" deleted


'''