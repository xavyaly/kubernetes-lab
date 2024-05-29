Creating a Kubernetes DaemonSet ensures that a copy of a pod runs on all (or some) nodes in your cluster. Here’s a step-by-step example of how to create, deploy, and verify a DaemonSet in Kubernetes.

### Step 1: Setup Your Kubernetes Cluster

Ensure you have a running Kubernetes cluster. You can use Minikube, a cloud provider, or any other Kubernetes environment.
Ref to install minikube: [Link](https://minikube.sigs.k8s.io/docs/start/?arch=%2Fmacos%2Fx86-64%2Fstable%2Fbinary+download)

### Step 2: Create a DaemonSet YAML File

Create a YAML file named `daemonset-example.yaml` with the following content. This example deploys a simple Nginx web server on all nodes.

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx-daemonset
  labels:
    app: nginx
spec:
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
        image: nginx:latest
        ports:
        - containerPort: 80
```

### Step 3: Apply the DaemonSet Configuration

Deploy the DaemonSet to your Kubernetes cluster using the `kubectl apply` command:

```bash
kubectl apply -f daemonset-example.yaml
```

### Step 4: Verify the DaemonSet

Check the status of the DaemonSet to ensure it's running correctly:

```bash
kubectl get daemonsets
```

You should see output similar to:

```
NAME              DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
nginx-daemonset   1         1         1       1            1           <none>          14s
```

This output indicates that the DaemonSet is running on all three nodes.

### Step 5: Verify the Pods

List all pods to see the ones created by the DaemonSet:

```bash
kubectl get pods -o wide
```

You should see output showing that there is one Nginx pod running on each node:

```
NAME                    READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
nginx-daemonset-fztcq   1/1     Running   0          72s   10.244.0.4   minikube   <none>           <none>
```

### Step 6: Accessing the Nginx Pods

Since DaemonSets usually run on each node, you can access the Nginx server on each node’s IP address. Use `kubectl port-forward` to access one of the pods:

```bash
POD_NAME=$(kubectl get pods -l app=nginx -o jsonpath="{.items[0].metadata.name}")
kubectl port-forward $POD_NAME 8080:80
```

Now you can access the Nginx web server at `http://localhost:8080`.

### Step 7: Clean Up

When you’re done, you can delete the DaemonSet with:

```bash
kubectl delete -f daemonset-example.yaml
```

### Conclusion

This example demonstrates how to create and manage a DaemonSet in Kubernetes. DaemonSets are particularly useful for running system daemons and monitoring agents across all nodes in a cluster.
