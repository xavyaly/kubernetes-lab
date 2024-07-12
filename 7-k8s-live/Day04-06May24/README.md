apiVersion: apps/v1 
kind: Deployment 
metadata:
  name: nginx
  namespace: dev
spec:
  template: 
    metadata:
      labels:
        app: nginx
    spec: 
      containers:
        - name: nginx 
          image: nginx:1.17
  strategy:
    type: RollingUpdate
  replicas: 3 
  selector:
    matchLabels: 
      app: nginx

'''
$ kubectl create namespace dev
namespace/dev created

$ kubectl get ns dev
NAME   STATUS   AGE
dev    Active   85s


'''