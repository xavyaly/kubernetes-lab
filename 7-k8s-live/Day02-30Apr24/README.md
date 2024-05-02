'''
apiVersion: v1    # version of k8s APIs used to create and manage the resource
kind: Pod         
metadata:
  name: pod-nginx # specifies kind of k8s resource
  namespace: default
  labels:         # about pod, such as it's name, namespaces, annotations...
    app: nginx
    type: front-end 
spec:             # specification of the Pod, including the container to run, n/w config, storage volumes to use
  containers:
    - name: nginx-container
      image: nginx:1.18
'''

'''
# kubectl apply -f pod-def.yaml

1. kube-api authenticates the request. kube-api checks authorizations 
2. kube-api check the manifests file associated with the pod for syntax errors
3. kube-api writes the pod's manifest file to etcd 
4. The Scheduler find a new unassigned pod on etcd and attempts to schedule it to run on an available nodes 
5a. The Scheduler selects a node to assign the pod to it. The Scheduler reports the selectd node to the kube-api
5b. Kube-api updates etcd with the changes pod's status field
6a. kube-api sends a pod creation request to kubelet
6b. Kubelet pulls the container images and starting the containers, updated pod status 
7. kube-api reads pod information from kubelet and updates the pod status in etcd 

$ kubectl apply -f pod-def.yaml 
pod/pod-nginx created
'''