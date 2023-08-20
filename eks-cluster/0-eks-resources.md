List all resources with examples for EKS

Amazon EKS (Elastic Kubernetes Service) involves various AWS resources to manage and operate Kubernetes clusters. Here's a list of key resources used in Amazon EKS, along with brief explanations and examples:

1. **Cluster**:
   An EKS cluster represents a managed Kubernetes control plane.

   Example:
   ```bash
   eksctl create cluster --name my-cluster --region us-west-2 --nodegroup-name my-nodegroup --node-type t3.medium --nodes 3 --nodes-min 1 --nodes-max 4
   ```

2. **Node Group**:
   Node groups are groups of Amazon EC2 instances that are registered with an EKS cluster.

   Example:
   ```bash
   eksctl create nodegroup --cluster my-cluster --region us-west-2 --name my-nodegroup --node-type t3.medium --nodes 3 --nodes-min 1 --nodes-max 4
   ```

3. **Fargate Profile**:
   A Fargate profile specifies how to run pods using AWS Fargate.

   Example:
   ```bash
   eksctl create fargateprofile --cluster my-cluster --name my-fargate-profile --namespace my-namespace --selectors app=my-app
   ```

4. **Service Account**:
   Service accounts provide an identity for processes that run in a container.

   Example:
   ```yaml
   apiVersion: v1
   kind: ServiceAccount
   metadata:
     name: my-service-account
     namespace: my-namespace
   ```

5. **IAM Roles for Service Accounts (IRSA)**:
   IRSA allows pods to use specific IAM roles to access AWS services.

   Example:
   ```bash
   eksctl create iamserviceaccount --name my-sa --namespace my-namespace --cluster my-cluster --attach-policy-arn arn:aws:iam::123456789012:policy/my-policy
   ```

6. **Namespace**:
   Kubernetes namespaces provide a way to divide cluster resources into logically named groups.

   Example:
   ```yaml
   apiVersion: v1
   kind: Namespace
   metadata:
     name: my-namespace
   ```

7. **Pod**:
   A pod is the smallest deployable unit in Kubernetes, representing a single instance of a running process.

   Example:
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: my-pod
     namespace: my-namespace
   spec:
     containers:
     - name: my-container
       image: nginx
   ```

8. **Deployment**:
   Deployments define a desired state for replicating pods and managing rolling updates.

   Example:
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: my-deployment
     namespace: my-namespace
   spec:
     replicas: 3
     selector:
       matchLabels:
         app: my-app
     template:
       metadata:
         labels:
           app: my-app
       spec:
         containers:
         - name: my-container
           image: nginx
   ```

9. **Service**:
   A service provides a stable network identity for a set of pods.

   Example:
   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: my-service
     namespace: my-namespace
   spec:
     selector:
       app: my-app
     ports:
     - protocol: TCP
       port: 80
       targetPort: 80
   ```

10. **ConfigMap**:
    ConfigMaps allow you to decouple configuration from pod definitions.

    Example:
    ```yaml
    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: my-configmap
      namespace: my-namespace
    data:
      key1: value1
      key2: value2
    ```

These examples provide a glimpse of the various resources you can create and manage in Amazon EKS. Remember that these are just basic examples; real-world scenarios can involve more complex configurations and interactions between resources.
