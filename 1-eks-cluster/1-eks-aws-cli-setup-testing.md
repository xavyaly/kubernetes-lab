[Reference Link](https://github.com/dche25/EKS-Cluster-Setup/blob/main/eks-setup)

# Create EKS Cluster with Node Groups

## Step-00: Introduction
- Understand about EKS Core Objects
  - Control Plane
  - Worker Nodes & Node Groups
  - Fargate Profiles
  - VPC
- Create EKS Cluster
- Associate EKS Cluster to IAM OIDC Provider
- Create EKS Node Groups
- Verify Cluster, Node Groups, EC2 Instances, IAM Policies and Node Groups


## Step-01: Create EKS Cluster using eksctl
- It will take 15 to 20 minutes to create the Cluster Control Plane 


# Create Cluster
eksctl create cluster --name=myeks \
                      --region=us-east-1 \
                      --zones=us-east-1a,us-east-1b \
                      --without-nodegroup 


```
$ eksctl create cluster --name=myeks \
>                       --region=us-east-1 \
>                       --zones=us-east-1a,us-east-1b \
>                       --without-nodegroup 
2023-08-20 16:25:32 [ℹ]  eksctl version 0.153.0
2023-08-20 16:25:32 [ℹ]  using region us-east-1
2023-08-20 16:25:32 [ℹ]  subnets for us-east-1a - public:192.168.0.0/19 private:192.168.64.0/19
2023-08-20 16:25:32 [ℹ]  subnets for us-east-1b - public:192.168.32.0/19 private:192.168.96.0/19
2023-08-20 16:25:32 [ℹ]  using Kubernetes version 1.25
2023-08-20 16:25:32 [ℹ]  creating EKS cluster "myeks" in "us-east-1" region with 
2023-08-20 16:25:32 [ℹ]  if you encounter any issues, check CloudFormation console or try 'eksctl utils describe-stacks --region=us-east-1 --cluster=myeks'
2023-08-20 16:25:32 [ℹ]  Kubernetes API endpoint access will use default of {publicAccess=true, privateAccess=false} for cluster "myeks" in "us-east-1"
2023-08-20 16:25:32 [ℹ]  CloudWatch logging will not be enabled for cluster "myeks" in "us-east-1"
2023-08-20 16:25:32 [ℹ]  you can enable it with 'eksctl utils update-cluster-logging --enable-types={SPECIFY-YOUR-LOG-TYPES-HERE (e.g. all)} --region=us-east-1 --cluster=myeks'
2023-08-20 16:25:32 [ℹ]  
2 sequential tasks: { create cluster control plane "myeks", wait for control plane to become ready 
}
2023-08-20 16:25:32 [ℹ]  building cluster stack "eksctl-myeks-cluster"
2023-08-20 16:25:34 [ℹ]  deploying stack "eksctl-myeks-cluster"
2023-08-20 16:26:04 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-cluster"
2023-08-20 16:26:35 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-cluster"
2023-08-20 16:27:37 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-cluster"
2023-08-20 16:28:38 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-cluster"
2023-08-20 16:29:40 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-cluster"
2023-08-20 16:30:41 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-cluster"
2023-08-20 16:31:42 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-cluster"
2023-08-20 16:32:43 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-cluster"
2023-08-20 16:33:44 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-cluster"
2023-08-20 16:34:45 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-cluster"
2023-08-20 16:35:46 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-cluster"
2023-08-20 16:37:54 [ℹ]  waiting for the control plane to become ready
2023-08-20 16:37:55 [✔]  saved kubeconfig as "/Users/javedalam/.kube/config"
2023-08-20 16:37:55 [ℹ]  no tasks
2023-08-20 16:37:55 [✔]  all EKS cluster resources for "myeks" have been created
2023-08-20 16:37:58 [ℹ]  kubectl command should work with "/Users/javedalam/.kube/config", try 'kubectl get nodes'
2023-08-20 16:37:58 [✔]  EKS cluster "myeks" in "us-east-1" region is ready
```

# What happens at the backend

In the provided `eksctl` command, you are creating an Amazon EKS cluster named "myeks" in the "us-east-1" region, spanning across availability zones "us-east-1a" and "us-east-1b", and you are using the `--without-nodegroup` flag, which means you are not creating a nodegroup as part of the cluster creation process.

Here's what happens at the backend when you run this `eksctl` command:

1. **Cluster Creation**: The `eksctl` tool communicates with the Amazon EKS service using the AWS API to initiate the creation of a new EKS cluster named "myeks" in the "us-east-1" region.

2. **Networking**: The EKS control plane infrastructure is provisioned, including networking components like the Amazon VPC (Virtual Private Cloud), subnets, and security groups. The control plane is the management plane of the EKS cluster.

3. **Availability Zones**: The specified availability zones, "us-east-1a" and "us-east-1b", are used to distribute the control plane's components across different data centers for high availability.

4. **Nodegroup (Skipped in Your Command)**: Since you are using the `--without-nodegroup` flag, a nodegroup (a set of worker nodes) is not created as part of the cluster creation. This means that the EKS cluster will not have any worker nodes initially. You can add worker nodes later using the `eksctl create nodegroup` command.

5. **Kubeconfig**: After the cluster is created, `eksctl` configures `kubectl` to work with the new EKS cluster by updating the kubeconfig file on your local machine. This allows you to interact with the cluster using `kubectl`.

6. **Cluster Information**: The `eksctl` command provides output that includes essential information about the created EKS cluster, such as the cluster name, region, control plane endpoint, and more.

Remember that without worker nodes (nodegroup), the cluster is not yet capable of running Kubernetes workloads. You'll need to add a nodegroup with worker nodes to start deploying and managing applications on the EKS cluster.

It's important to note that EKS cluster creation also involves many other steps, such as IAM role creation, setting up authentication and authorization, and provisioning security configurations. `eksctl` abstracts away much of the complexity by providing a simplified command-line interface for creating and managing EKS clusters.


# aws cli commands to verify the above created cluster

After creating an Amazon EKS cluster using the `eksctl` command, you can use the AWS CLI to verify the cluster's status and get additional information about it. Here are some AWS CLI commands you can use:

1. **List Clusters**:
   To list all EKS clusters in your AWS account, you can use the following command:

   ```bash
   aws eks list-clusters

   $ aws eks list-clusters
    {
    "clusters": [
        "myeks"
    ]
    }
   ```

   This will provide a list of EKS cluster names.

2. **Describe Cluster**:
   To get detailed information about a specific EKS cluster, you can use the `describe-cluster` command with the cluster name:

   ```bash
   aws eks describe-cluster --name myeks

   $ aws eks describe-cluster --name myeks 
    {
        "cluster": {
            "name": "myeks",
    .....
        }
    }
   ```

   Replace `myeks` with your actual cluster name.

3. **Get Token for `kubectl`**:
   To configure `kubectl` to work with the newly created EKS cluster, you can use the following command:

   ```bash
   aws eks update-kubeconfig --name myeks
   ```

   This command updates your local `kubectl` configuration with the credentials and cluster information.

4. **List Nodegroups**:
   To list the nodegroups associated with your EKS cluster, you can use the following command:

   ```bash
   aws eks list-nodegroups --cluster-name myeks

   $ aws eks list-nodegroups --cluster-name myeks
    {
        "nodegroups": []
    }
   ```

   Replace `myeks` with your actual cluster name.

5. **Describe Nodegroup**:
   If you later create a nodegroup for your cluster, you can get detailed information about the nodegroup using the `describe-nodegroup` command:

   ```bash
   aws eks describe-nodegroup --cluster-name myeks --nodegroup-name my-nodegroup
   ```

   Replace `myeks` with your cluster name and `my-nodegroup` with your nodegroup name.

These AWS CLI commands will help you verify the status and details of your EKS cluster, as well as any associated nodegroups. Remember to replace placeholders like `myeks` and `my-nodegroup` with the actual names you used during the cluster and nodegroup creation.


# Note: Above CLI commans doesn't create any nodegroup so, you can skip few commands 


# How to delete the above myeks created cluster

To delete an Amazon EKS cluster using the AWS CLI, you can use the `eksctl` tool or directly use the AWS CLI. Here's how to delete the cluster "myeks" using both methods:

Using `eksctl`:

```bash
eksctl delete cluster --name myeks
```

Using AWS CLI directly:

```bash
aws eks delete-cluster --name myeks
```

Both commands will initiate the process of deleting the EKS cluster named "myeks". The cluster, its associated resources (such as worker nodes), and control plane components will be removed. Please note that the deletion process might take some time to complete.

Remember that deleting an EKS cluster is a permanent action and will result in the loss of all data and configurations associated with the cluster. Make sure you have backups or copies of any important data before proceeding with the deletion.

---------------------------------------------------------------------------------------------------------------------

## Step-02: Create & Associate IAM OIDC Provider for our EKS Cluster

- To enable and use AWS IAM roles for Kubernetes service accounts on our EKS cluster, we must create &  associate OIDC identity provider.
- To do so using `eksctl` we can use the  below command. 
- Use latest eksctl version (as on today the latest version is `0.21.0`)
```                   
# Replace with region & cluster name
eksctl utils associate-iam-oidc-provider \
    --region us-east-1 \
    --cluster myeks \
    --approve
```

```
$ eksctl utils associate-iam-oidc-provider \
>     --region us-east-1 \
>     --cluster myeks \
>     --approve
2023-08-20 16:54:44 [ℹ]  will create IAM Open ID Connect provider for cluster "myeks" in "us-east-1"
2023-08-20 16:54:46 [✔]  created IAM Open ID Connect provider for cluster "myeks" in "us-east-1"
```

# What happens at the backend of above code

The `eksctl utils associate-iam-oidc-provider` command is used to associate an IAM OIDC identity provider with your Amazon EKS cluster. This command is necessary if you plan to use IAM roles for service accounts (IRSA) within your Kubernetes applications running on the EKS cluster. Here's what the command does:

1. **IAM OIDC Identity Provider**:
   Amazon EKS uses IAM OIDC identity providers to authenticate service accounts within the cluster. When you run the `eksctl utils associate-iam-oidc-provider` command, you're associating the IAM OIDC identity provider for your EKS cluster.

2. **--region**:
   The `--region` flag specifies the AWS region where your EKS cluster is located, in this case, "us-east-1". Replace it with the actual region of your cluster.

3. **--cluster**:
   The `--cluster` flag specifies the name of your EKS cluster, in this case, "myeks". Replace it with the actual name of your cluster.

4. **--approve**:
   The `--approve` flag indicates that you approve the association of the IAM OIDC identity provider with the EKS cluster. By providing this flag, you avoid the need to manually confirm the operation.

When you run the command, it associates the IAM OIDC identity provider for the specified EKS cluster in the specified region. This step is required for enabling IAM roles for service accounts (IRSA) within the Kubernetes applications running on the EKS cluster. IRSA allows you to assign IAM roles directly to Kubernetes service accounts, enabling more granular access control and authentication mechanisms for your applications.

After running this command, you'll be able to use IAM roles for service accounts in your Kubernetes workloads on the EKS cluster, allowing you to manage access to AWS resources without directly embedding AWS credentials in your application code or configuration.


# AWS CLI to verify the above commands

To verify the result of the `eksctl utils associate-iam-oidc-provider` command, you can use the AWS CLI to list the OIDC identity providers associated with your EKS cluster. Here's the command:

```bash
aws eks describe-cluster --name myeks --query "cluster.identity.oidc.issuer" --output text
```
```
$ aws eks describe-cluster --name myeks --query "cluster.identity.oidc.issuer" --output text
https://oidc.eks.us-east-1.amazonaws.com/id/2D434A2C9FA338D78742590694E13CB2
```

Replace `myeks` with the actual name of your EKS cluster.

This command queries the EKS cluster's information and specifically retrieves the OIDC issuer URL. If the OIDC provider association was successful, you will see the OIDC issuer URL as output, indicating that the association is in place.

The OIDC issuer URL is used for authenticating service accounts and establishing trust between your Kubernetes applications and AWS IAM roles.

Remember that the `describe-cluster` command also provides other information about your cluster, so you can explore further if needed.


# AWS CLI to remove the above 

To remove the association of the IAM OIDC identity provider with your Amazon EKS cluster using the AWS CLI, you can use the `eksctl` tool or directly use the AWS CLI. Here's how to do it using both methods:

Using `eksctl`:

```bash
eksctl utils disassociate-iam-oidc-provider \
  --region us-east-1 \
  --cluster myeks \
  --approve
```

Using AWS CLI directly:

```bash
aws eks disassociate-identity-provider \
  --region us-east-1 \
  --cluster-name myeks \
  --identity-provider-arn <identity-provider-arn>
```

Replace the placeholders with the appropriate values:
- `<identity-provider-arn>`: The Amazon Resource Name (ARN) of the OIDC identity provider that you want to disassociate. You can get this ARN from the output of the `describe-cluster` command.

Both commands will initiate the process of removing the association of the IAM OIDC identity provider from your EKS cluster. This action will prevent the cluster from using IAM roles for service accounts (IRSA).

---------------------------------------------------------------------------------------------------------------------

## Step-03: Create EC2 Keypair

- Create a new EC2 Keypair with name as `kube-demo`
- This keypair we will use it when creating the EKS NodeGroup.
- This will help us to login to the EKS Worker Nodes using Terminal.

To create a new EC2 key pair named `kube-demo` using the AWS CLI, you can use the following command:

```bash
aws ec2 create-key-pair --key-name kube-demo --query 'KeyMaterial' --output text > kube-demo.pem
```

This command creates a new key pair named `kube-demo` and saves the private key material into a file named `kube-demo.pem`.

Please note the following:
- The private key material will be saved in the `kube-demo.pem` file. Ensure that you protect this private key, as it is crucial for accessing and authenticating to EC2 instances launched using this key pair.
- You might need to adjust the permissions of the `kube-demo.pem` file to ensure it's secure. You can use the `chmod` command to do this.
- Make sure you have the AWS CLI installed and properly configured with your AWS credentials before running this command.


# AWS CLI command to see the key pair created successfully

To verify that the EC2 key pair `kube-demo` was created successfully using the AWS CLI, you can use the following command:

```bash
aws ec2 describe-key-pairs --key-names kube-demo
```

```
$ aws ec2 describe-key-pairs --key-names kube-demo
{
    "KeyPairs": [
        {
            "KeyPairId": "key-0c3f24a824a8737cf",
            "KeyFingerprint": "b1:49:b0:b3:a7:63:2d:58:92:81:f8:7c:4f:92:4c:17:24:02:95:ab",
            "KeyName": "kube-demo",
            "KeyType": "rsa",
            "Tags": [],
            "CreateTime": "2023-08-20T11:30:57.131000+00:00"
        }
    ]
}
```

This command queries the AWS EC2 service and describes the details of the specified key pair. If the key pair was created successfully, you should see detailed information about the `kube-demo` key pair in the command's output, including its name and other metadata.

If the key pair exists and was created successfully, you will receive JSON output with information about the key pair. If the key pair does not exist, you will receive an error message indicating that the key pair does not exist.

Remember to ensure that you have the AWS CLI properly configured with your AWS credentials before running this command.

---------------------------------------------------------------------------------------------------------------------

## Step-04: Create Node Group with additional Add-Ons in Public Subnets

- These add-ons will create the respective IAM policies for us automatically within our Node Group role.
 ```
# Create Public Node Group   
eksctl create nodegroup --cluster=myeks \
                        --region=us-east-1 \
                        --name=myeks-ng-public1 \
                        --node-type=t3.medium \
                        --nodes=3 \
                        --nodes-min=3 \
                        --nodes-max=14 \
                        --node-volume-size=20 \
                        --ssh-access \
                        --ssh-public-key=kube-demo \
                        --managed \
                        --asg-access \
                        --external-dns-access \
                        --full-ecr-access \
                        --appmesh-access \
                        --alb-ingress-access 
```

```
$ eksctl create nodegroup --cluster=myeks \
>                         --region=us-east-1 \
>                         --name=myeks-ng-public1 \
>                         --node-type=t3.medium \
>                         --nodes=3 \
>                         --nodes-min=3 \
>                         --nodes-max=14 \
>                         --node-volume-size=20 \
>                         --ssh-access \
>                         --ssh-public-key=kube-demo \
>                         --managed \
>                         --asg-access \
>                         --external-dns-access \
>                         --full-ecr-access \
>                         --appmesh-access \
>                         --alb-ingress-access
2023-08-20 17:11:51 [ℹ]  will use version 1.25 for new nodegroup(s) based on control plane version
2023-08-20 17:11:59 [ℹ]  nodegroup "myeks-ng-public1" will use "" [AmazonLinux2/1.25]
2023-08-20 17:11:59 [ℹ]  using EC2 key pair %!q(*string=<nil>)
2023-08-20 17:12:02 [ℹ]  1 nodegroup (myeks-ng-public1) was included (based on the include/exclude rules)
2023-08-20 17:12:02 [ℹ]  will create a CloudFormation stack for each of 1 managed nodegroups in cluster "myeks"
2023-08-20 17:12:02 [ℹ]  
2 sequential tasks: { fix cluster compatibility, 1 task: { 1 task: { create managed nodegroup "myeks-ng-public1" } } 
}
2023-08-20 17:12:02 [ℹ]  checking cluster stack for missing resources
2023-08-20 17:12:03 [ℹ]  cluster stack has all required resources
2023-08-20 17:12:04 [ℹ]  building managed nodegroup stack "eksctl-myeks-nodegroup-myeks-ng-public1"
2023-08-20 17:12:06 [ℹ]  deploying stack "eksctl-myeks-nodegroup-myeks-ng-public1"
2023-08-20 17:12:06 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-nodegroup-myeks-ng-public1"
2023-08-20 17:12:37 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-nodegroup-myeks-ng-public1"
2023-08-20 17:13:12 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-nodegroup-myeks-ng-public1"
2023-08-20 17:14:37 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-nodegroup-myeks-ng-public1"
2023-08-20 17:14:37 [ℹ]  no tasks
2023-08-20 17:14:37 [✔]  created 0 nodegroup(s) in cluster "myeks"
2023-08-20 17:14:38 [ℹ]  nodegroup "myeks-ng-public1" has 3 node(s)
2023-08-20 17:14:38 [ℹ]  node "ip-192-168-0-203.ec2.internal" is ready
2023-08-20 17:14:38 [ℹ]  node "ip-192-168-49-252.ec2.internal" is ready
2023-08-20 17:14:38 [ℹ]  node "ip-192-168-54-229.ec2.internal" is ready
2023-08-20 17:14:38 [ℹ]  waiting for at least 3 node(s) to become ready in "myeks-ng-public1"
2023-08-20 17:14:39 [ℹ]  nodegroup "myeks-ng-public1" has 3 node(s)
2023-08-20 17:14:39 [ℹ]  node "ip-192-168-0-203.ec2.internal" is ready
2023-08-20 17:14:39 [ℹ]  node "ip-192-168-49-252.ec2.internal" is ready
2023-08-20 17:14:39 [ℹ]  node "ip-192-168-54-229.ec2.internal" is ready
2023-08-20 17:14:39 [✔]  created 1 managed nodegroup(s) in cluster "myeks"
2023-08-20 17:14:40 [ℹ]  checking security group configuration for all nodegroups
2023-08-20 17:14:40 [ℹ]  all nodegroups have up-to-date cloudformation templates
```

# what happens at the backed while executing of above command

The `eksctl create nodegroup` command creates an Amazon EKS nodegroup within your existing EKS cluster. This nodegroup consists of worker nodes that run your Kubernetes workloads. The command you provided has several flags that configure various aspects of the nodegroup. Here's what happens at the backend when you execute the provided command:

1. **Cluster and Region Information**:
   - `--cluster=myeks`: Specifies the name of the EKS cluster to which the nodegroup should be associated.
   - `--region=us-east-1`: Specifies the AWS region in which the EKS cluster is located.

2. **Nodegroup Configuration**:
   - `--name=myeks-ng-public1`: Specifies the name of the nodegroup you are creating.
   - `--node-type=t3.medium`: Specifies the EC2 instance type for the worker nodes.
   - `--nodes=3`: Specifies the desired number of nodes in the nodegroup.
   - `--nodes-min=3`: Specifies the minimum number of nodes in the nodegroup.
   - `--nodes-max=14`: Specifies the maximum number of nodes in the nodegroup.
   - `--node-volume-size=20`: Specifies the size (in GB) of the root volume for each worker node.
   
3. **Access and Permissions Configuration**:
   - `--ssh-access`: Enables SSH access to the worker nodes.
   - `--ssh-public-key=mykey`: Specifies the name of the EC2 key pair to use for SSH access.
   - `--managed`: Specifies that the nodegroup is managed, which means EKS will handle scaling and updates.
   - `--asg-access`: Grants the necessary permissions for Auto Scaling Group (ASG) operations.
   - `--external-dns-access`: Grants permissions for ExternalDNS integration.
   - `--full-ecr-access`: Grants full access to Amazon ECR repositories.
   - `--appmesh-access`: Grants permissions for AWS App Mesh integration.
   - `--alb-ingress-access`: Grants permissions for ALB Ingress Controller integration.

4. **Nodegroup Creation**:
   The `eksctl` tool communicates with the AWS API to create the specified nodegroup within the EKS cluster. It provisions the worker nodes, configures the Auto Scaling Group, attaches IAM roles, and establishes networking.

5. **Scaling and Management**:
   Since you specified the `--managed` flag, EKS will automatically handle scaling and updates for the nodegroup. It will ensure that the number of nodes stays within the range defined by `--nodes-min` and `--nodes-max`.

The command essentially defines the characteristics and permissions of the nodegroup and then triggers the creation process. Once the nodegroup is created, the worker nodes will join the EKS cluster, making them available to run Kubernetes workloads.


# AWS CLI to verify the above resources

To verify the resources created using the `eksctl create nodegroup` command, you can use the AWS CLI to describe the details of the EKS cluster and its associated nodegroups. Here's how to do it:

1. **Describe EKS Cluster**:
   To get detailed information about the EKS cluster, use the following command:

   ```bash
   aws eks describe-cluster --name myeks --region us-east-1
   ```

   This command will provide you with detailed information about the EKS cluster, including its status, endpoint, and other metadata.

2. **List Nodegroups**:
   To list the nodegroups associated with your EKS cluster, you can use the following command:

   ```bash
   aws eks list-nodegroups --cluster-name myeks --region us-east-1
   ```
   ```
   $ aws eks list-nodegroups --cluster-name myeks --region us-east-1
    {
        "nodegroups": [
            "myeks-ng-public1"
        ]
    }
   ```

   This command will list the names of the nodegroups that belong to the specified EKS cluster.

3. **Describe Nodegroup**:
   If you want to get detailed information about a specific nodegroup, you can use the following command:

   ```bash
   aws eks describe-nodegroup --cluster-name myeks --nodegroup-name myeks-ng-public1 --region us-east-1
   ```

   ```
   $ aws eks describe-nodegroup --cluster-name myeks --nodegroup-name myeks-ng-public1 --region us-east-1
    {
    "nodegroup": {
        "nodegroupName": "myeks-ng-public1",
    ......
    ......
    }
    }
   ```

   Replace `myeks` with your actual EKS cluster name and `myeks-ng-public1` with your nodegroup name.

These AWS CLI commands will allow you to verify the details of the EKS cluster and the created nodegroup, including their configurations, status, and associated resources. Make sure you have the AWS CLI properly configured with your AWS credentials before running these commands.


# AWS CLI to remove the above nodegroups

To remove the resources created using the `eksctl create nodegroup` command, you can use the AWS CLI to delete the nodegroup and its associated resources. Here's how to do it:

1. **Delete Nodegroup**:
   To delete the nodegroup you created, use the following command:

   ```bash
   eksctl delete nodegroup --cluster=myeks --name=myeks-ng-public1 --region=us-east-1
   ```

   Replace `myeks` with your actual EKS cluster name and `myeks-ng-public1` with your nodegroup name.

2. **Delete Cluster** (Optional):
   If you want to delete the entire EKS cluster, including all its nodegroups, you can use the following command:

   ```bash
   eksctl delete cluster --name=myeks --region=us-east-1
   ```

   This will remove the entire EKS cluster, including all associated resources such as nodegroups, networking components, and more.

Please note the following:
- Deleting resources is a permanent action, and data associated with these resources will be lost.
- Ensure that you have backups of any important data before performing deletion operations.
- Make sure you have the AWS CLI properly configured with your AWS credentials before running these commands.

---------------------------------------------------------------------------------------------------------------------

## Step-05: Verify Cluster & Nodes

### Verify NodeGroup subnets to confirm EC2 Instances are in Public Subnet

- Verify the node group subnet to ensure it created in public subnets -> X
  - Go to Services -> EKS -> eksdemo -> eksdemo1-ng1-public
  - Click on Associated subnet in **Details** tab
  - Click on **Route Table** Tab.
  - We should see that internet route via Internet Gateway (0.0.0.0/0 -> igw-xxxxxxxx)

### Verify Cluster, NodeGroup in EKS Management Console 

- Go to Services -> Elastic Kubernetes Service -> eksdemo1 -> X

### List Worker Nodes
```
# List EKS clusters
eksctl get cluster

```
$ eksctl get cluster
NAME    REGION          EKSCTL CREATED
myeks   us-east-1       True
```

# List NodeGroups in a cluster
eksctl get nodegroup --cluster=<clusterName>

```
$ eksctl get nodegroup --cluster=myeks
CLUSTER NODEGROUP               STATUS  CREATED                 MIN SIZE        MAX SIZE        DESIRED CAPACITY        INSTANCE TYPE       IMAGE ID        ASG NAME                                                        TYPE
myeks   myeks-ng-public1        ACTIVE  2023-08-20T11:42:30Z    3               14              3                       t3.medium   AL2_x86_64      eks-myeks-ng-public1-d6c50960-7871-fcdb-38cd-dd31e03cb9a7       managed
```

# List Nodes in current kubernetes cluster
kubectl get nodes -o wide

```
$ kubectl get nodes -o wide
NAME                             STATUS   ROLES    AGE   VERSION                INTERNAL-IP      EXTERNAL-IP      OS-IMAGE         KERNEL-VERSION                  CONTAINER-RUNTIME
ip-192-168-0-203.ec2.internal    Ready    <none>   15m   v1.25.11-eks-a5565ad   192.168.0.203    54.236.247.206   Amazon Linux 2   5.10.186-179.751.amzn2.x86_64   containerd://1.6.19
ip-192-168-49-252.ec2.internal   Ready    <none>   15m   v1.25.11-eks-a5565ad   192.168.49.252   18.212.84.13     Amazon Linux 2   5.10.186-179.751.amzn2.x86_64   containerd://1.6.19
ip-192-168-54-229.ec2.internal   Ready    <none>   15m   v1.25.11-eks-a5565ad   192.168.54.229   54.221.14.81     Amazon Linux 2   5.10.186-179.751.amzn2.x86_64   containerd://1.6.19
```

# Our kubectl context should be automatically changed to new cluster
kubectl config view --minify
```
$ kubectl config view --minify
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://2D434A2C9FA338D78742590694E13CB2.gr7.us-east-1.eks.amazonaws.com
  name: myeks.us-east-1.eksctl.io
contexts:
- context:
    cluster: myeks.us-east-1.eksctl.io
    user: javed@myeks.us-east-1.eksctl.io
  name: javed@myeks.us-east-1.eksctl.io
current-context: javed@myeks.us-east-1.eksctl.io
kind: Config
preferences: {}
users:
- name: javed@myeks.us-east-1.eksctl.io
  user:
    exec:
      apiVersion: client.authentication.k8s.io/v1beta1
      args:
      - token
      - -i
      - myeks
      command: aws-iam-authenticator
      env:
      - name: AWS_STS_REGIONAL_ENDPOINTS
        value: regional
      - name: AWS_DEFAULT_REGION
        value: us-east-1
      interactiveMode: IfAvailable
      provideClusterInfo: false
```

### Verify Worker Node IAM Role and list of Policies
- Go to Services -> EC2 -> Worker Nodes
- Click on **IAM Role associated to EC2 Worker Nodes**

### Verify the running nodes/instances
```
$ aws ec2 describe-instances --filters "Name=instance-state-name,Values=running" --query "Reservations[].Instances[].{Instance:InstanceId,Type:InstanceType,PrivateIP:PrivateIpAddress,PublicIP:PublicIpAddress}" --output json
[
    {
        "Instance": "i-0c88c55a0afea38ab",
        "Type": "t3.medium",
        "PrivateIP": "192.168.49.252",
        "PublicIP": "18.212.84.13"
    },
    {
        "Instance": "i-05deaeed0915b0171",
        "Type": "t3.medium",
        "PrivateIP": "192.168.54.229",
        "PublicIP": "54.221.14.81"
    },
    {
        "Instance": "i-0e5bc57936315f5c4",
        "Type": "t3.medium",
        "PrivateIP": "192.168.0.203",
        "PublicIP": "54.236.247.206"
    }
]
```
### Verify the iam roles attached to running nodes/instances
```
$ aws ec2 describe-instances --filters "Name=instance-state-name,Values=running" --query "Reservations[].Instances[].{InstanceId:InstanceId, IAMRole: IamInstanceProfile.Arn}" --output json
[
    {
        "InstanceId": "i-0c88c55a0afea38ab",
        "IAMRole": "arn:aws:iam::252473277340:instance-profile/eks-d6c50960-7871-fcdb-38cd-dd31e03cb9a7"
    },
    {
        "InstanceId": "i-05deaeed0915b0171",
        "IAMRole": "arn:aws:iam::252473277340:instance-profile/eks-d6c50960-7871-fcdb-38cd-dd31e03cb9a7"
    },
    {
        "InstanceId": "i-0e5bc57936315f5c4",
        "IAMRole": "arn:aws:iam::252473277340:instance-profile/eks-d6c50960-7871-fcdb-38cd-dd31e03cb9a7"
    }
]
```

### Verify Security Group Associated to Worker Nodes
- Go to Services -> EC2 -> Worker Nodes
- Click on **Security Group** associated to EC2 Instance which contains `remote` in the name.

```
$ aws ec2 describe-instances --filters "Name=instance-state-name,Values=running" --query "Reservations[].Instances[].{InstanceId:InstanceId, SecurityGroups:SecurityGroups[].GroupId}" --output json
[
    {
        "InstanceId": "i-0c88c55a0afea38ab",
        "SecurityGroups": [
            "sg-05e11f36aa4bd6165",
            "sg-0f69894ad0c52098a"
        ]
    },
    {
        "InstanceId": "i-05deaeed0915b0171",
        "SecurityGroups": [
            "sg-05e11f36aa4bd6165",
            "sg-0f69894ad0c52098a"
        ]
    },
    {
        "InstanceId": "i-0e5bc57936315f5c4",
        "SecurityGroups": [
            "sg-05e11f36aa4bd6165",
            "sg-0f69894ad0c52098a"
        ]
    }
]
```

### Verify CloudFormation Stacks
- Verify Control Plane Stack & Events
- Verify NodeGroup Stack & Events

```
### List all CF Stacks

$ aws cloudformation describe-stacks --query "Stacks[].{StackName:StackName, StackStatus:StackStatus}" --output json
[
    {
        "StackName": "eksctl-myeks-nodegroup-myeks-ng-public1",
        "StackStatus": "CREATE_COMPLETE"
    },
    {
        "StackName": "eksctl-myeks-cluster",
        "StackStatus": "CREATE_COMPLETE"
    }
]

```
```
- Verify Control Plane Stack & Events

To verify the control plane stack and events of an Amazon EKS cluster using the AWS CLI, you can use the CloudFormation service's `describe-stacks` command. Here's how to do it:

1. **List Stacks**:
   To list all the CloudFormation stacks associated with your EKS cluster, use the following command:

   ```bash
   aws cloudformation describe-stacks --stack-name <cluster-name>-eksctl-cluster

   $ aws cloudformation describe-stacks --stack-name eksctl-myeks-nodegroup-myeks-ng-public1
    {
    "Stacks": [
        .......
        .......
    ]
    }
   ```

   Replace `<cluster-name>` with your actual EKS cluster name.

2. **Describe Events**:
   To retrieve events related to the CloudFormation stack (EKS control plane stack), use the following command:

   ```bash
   aws cloudformation describe-stack-events --stack-name <cluster-name>-eksctl-cluster

   $ aws cloudformation describe-stack-events --stack-name eksctl-myeks-nodegroup-myeks-ng-public1
    {
    "StackEvents": [
        .......
        .......
    ]
        {
        }
    }
   ```

   Replace `<cluster-name>` with your actual EKS cluster name.

These commands will provide information about the CloudFormation stack associated with your EKS cluster, including its status, creation events, and any updates performed on the stack. The events will give you insights into the changes and actions taken on the control plane stack.

Remember to ensure that you have the AWS CLI properly configured with your AWS credentials and permissions to access CloudFormation and EKS resources.

```

### Login to Worker Node using Keypai kube-demo
- Login to worker node
```
# For MAC or Linux or Windows10
ssh -i kube-demo.pem ec2-user@<Public-IP-of-Worker-Node>

### Play inside nodegroup
```
$ chmod 400 kube-demo.pem
$ ssh -i kube-demo.pem ec2-user@18.212.84.13
Last login: Wed Aug 16 02:49:50 2023 from 205.251.233.111

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
[ec2-user@ip-192-168-49-252 ~]$ python3 --version
Python 3.7.16
[ec2-user@ip-192-168-49-252 ~]$ sudo systemctl status kubelet
[ec2-user@ip-192-168-49-252 ~]$ journalctl -u kubelet
```

# For Windows 7
Use putty
```

## Step-06: Update Worker Nodes Security Group to allow all traffic
- We need to allow `All Traffic` on worker node security group

## Additional References
- https://docs.aws.amazon.com/eks/latest/userguide/enable-iam-roles-for-service-accounts.html
- https://docs.aws.amazon.com/eks/latest/userguide/create-service-account-iam-policy-and-role.html

---------------------------------------------------------------------------------------------------------------------

### Executed few commands to check the EKS SetUp properly

$ kubectl get nodes 
NAME                             STATUS   ROLES    AGE   VERSION
ip-192-168-0-203.ec2.internal    Ready    <none>   43m   v1.25.11-eks-a5565ad
ip-192-168-49-252.ec2.internal   Ready    <none>   43m   v1.25.11-eks-a5565ad
ip-192-168-54-229.ec2.internal   Ready    <none>   43m   v1.25.11-eks-a5565ad


$ kubectl get nodes -o wide
NAME                             STATUS   ROLES    AGE   VERSION                INTERNAL-IP      EXTERNAL-IP      OS-IMAGE         KERNEL-VERSION                  CONTAINER-RUNTIME
ip-192-168-0-203.ec2.internal    Ready    <none>   43m   v1.25.11-eks-a5565ad   192.168.0.203    54.236.247.206   Amazon Linux 2   5.10.186-179.751.amzn2.x86_64   containerd://1.6.19
ip-192-168-49-252.ec2.internal   Ready    <none>   43m   v1.25.11-eks-a5565ad   192.168.49.252   18.212.84.13     Amazon Linux 2   5.10.186-179.751.amzn2.x86_64   containerd://1.6.19
ip-192-168-54-229.ec2.internal   Ready    <none>   43m   v1.25.11-eks-a5565ad   192.168.54.229   54.221.14.81     Amazon Linux 2   5.10.186-179.751.amzn2.x86_64   containerd://1.6.19


$ kubectl get namespace
NAME              STATUS   AGE
default           Active   85m
kube-node-lease   Active   85m
kube-public       Active   85m
kube-system       Active   85m


$ kubectl get svc
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.100.0.1   <none>        443/TCP   85m


$ kubectl get pods --all-namespaces
NAMESPACE     NAME                       READY   STATUS    RESTARTS   AGE
kube-system   aws-node-cjn4p             1/1     Running   0          44m
kube-system   aws-node-gv8b5             1/1     Running   0          44m
kube-system   aws-node-rtczt             1/1     Running   0          44m
kube-system   coredns-7975d6fb9b-87j8r   1/1     Running   0          85m
kube-system   coredns-7975d6fb9b-b6dcp   1/1     Running   0          85m
kube-system   kube-proxy-4zfwm           1/1     Running   0          44m
kube-system   kube-proxy-5t7h6           1/1     Running   0          44m
kube-system   kube-proxy-ctwcw           1/1     Running   0          44m


$ kubectl get svc --all-namespaces
NAMESPACE     NAME         TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)         AGE
default       kubernetes   ClusterIP   10.100.0.1    <none>        443/TCP         87m
kube-system   kube-dns     ClusterIP   10.100.0.10   <none>        53/UDP,53/TCP   87m


$ kubectl get deployments --all-namespaces
NAMESPACE     NAME      READY   UP-TO-DATE   AVAILABLE   AGE
kube-system   coredns   2/2     2            2           87m


$ kubectl logs aws-node-cjn4p  -n kube-system
Defaulted container "aws-node" out of: aws-node, aws-vpc-cni-init (init)
Installed /host/opt/cni/bin/aws-cni
Installed /host/opt/cni/bin/egress-v4-cni
time="2023-08-20T11:43:38Z" level=info msg="Starting IPAM daemon... "
time="2023-08-20T11:43:38Z" level=info msg="Checking for IPAM connectivity... "
time="2023-08-20T11:43:39Z" level=info msg="Copying config file... "
time="2023-08-20T11:43:39Z" level=info msg="Successfully copied CNI plugin binary and config file."


$ kubectl get events --all-namespaces


$ kubectl exec -it aws-node-cjn4p -n kube-system -- /bin/sh
Defaulted container "aws-node" out of: aws-node, aws-vpc-cni-init (init)
error: Internal error occurred: error executing command in container: failed to exec in container: failed to start exec "a17f876b0b816f3a518fb6fc95af231bb4e9eba1085bca4cef0e2991e2de7063": OCI runtime exec failed: exec failed: unable to start container process: exec: "/bin/sh": stat /bin/sh: no such file or directory: unknown


$ kubectl cluster-info
Kubernetes control plane is running at https://2D434A2C9FA338D78742590694E13CB2.gr7.us-east-1.eks.amazonaws.com
CoreDNS is running at https://2D434A2C9FA338D78742590694E13CB2.gr7.us-east-1.eks.amazonaws.com/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.


$ kubectl scale deployment coredns --replicas=5
Error from server (NotFound): deployments.apps "coredns" not found


$ kubectl get deployment
No resources found in default namespace.


---------------------------------------------------------------------------------------------------------------------

### First Scenario to test the EKS SetUp

# A small scenario to test the eks new setup with example

Certainly! Let's go through a simple scenario to test your Amazon EKS cluster setup. In this scenario, we'll deploy a basic Nginx web server using Kubernetes manifests.

1. **Create a Deployment**:
   Create a file named `nginx-deployment.yaml` with the following content to create a deployment of Nginx pods:

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: nginx-deployment
   spec:
     replicas: 3
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
   ```

2. **Apply the Deployment**:
   Apply the deployment to your EKS cluster using the following command:

   ```bash
   kubectl apply -f nginx-deployment.yaml

   $ kubectl apply -f nginx-deployment.yaml
    deployment.apps/nginx-deployment created
   ```

3. **Verify Pods**:
   Check if the pods are running:

   ```bash
   kubectl get pods

   $ kubectl get pods
    NAME                                READY   STATUS    RESTARTS   AGE
    nginx-deployment-6d666844f6-gwgwh   1/1     Running   0          45s
    nginx-deployment-6d666844f6-lldzl   1/1     Running   0          45s
    nginx-deployment-6d666844f6-qnl57   1/1     Running   0          45s
   ```

4. **Create a Service**:
   Create a file named `nginx-service.yaml` with the following content to expose the Nginx deployment using a ClusterIP service:

   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: nginx-service
   spec:
     selector:
       app: nginx
     ports:
       - protocol: TCP
         port: 80
         targetPort: 80
   ```

5. **Apply the Service**:
   Apply the service configuration:

   ```bash
   kubectl apply -f nginx-service.yaml

   $ kubectl apply -f nginx-service.yaml 
    service/nginx-service created
   ```

6. **Access the Service**:
   Get the external IP of the nodes (usually AWS Load Balancer IP or NodePort IP):

   ```bash
   kubectl get svc nginx-service

   $ kubectl get svc nginx-service
    NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
    nginx-service   ClusterIP   10.100.40.255   <none>        80/TCP    29s

   $ kubectl get deployment nginx-deployment
    NAME               READY   UP-TO-DATE   AVAILABLE   AGE
    nginx-deployment   3/3     3            3           2m17s
   ```

   Open a web browser and enter the external IP. You should see the default Nginx welcome page.

7. **Scale Deployment**:
   Scale the deployment to see how EKS handles the increase in load:

   ```bash
   kubectl scale deployment nginx-deployment --replicas=5

   $ kubectl scale deployment nginx-deployment --replicas=5
    deployment.apps/nginx-deployment scaled

    $ kubectl get deployment nginx-deployment
    NAME               READY   UP-TO-DATE   AVAILABLE   AGE
    nginx-deployment   5/5     5            5           2m34s

   ```

8. **Clean Up**:
   To clean up the resources, delete the deployment and service:

   ```bash
   kubectl delete deployment nginx-deployment
   kubectl delete service nginx-service

    $ kubectl delete deployment nginx-deployment
    kubectl delete service nginx-service
    deployment.apps "nginx-deployment" deleted
    
    $ kubectl delete service nginx-service
    service "nginx-service" deleted
   ```

This scenario tests basic deployment, scaling, and access to pods in your Amazon EKS cluster. Remember that this is just a simple example to get you started. Real-world scenarios would involve more complex applications and configurations.


---------------------------------------------------------------------------------------------------------------------

### Second cenario to test the EKS SetUp

Of course! Let's explore another scenario to further test your Amazon EKS setup. In this scenario, we'll deploy a stateful application using a WordPress and MySQL stack.

1. **Create a PersistentVolumeClaim**:
   Create a file named `mysql-pvc.yaml` with the following content to define a PersistentVolumeClaim for MySQL data:

   ```yaml
   apiVersion: v1
   kind: PersistentVolumeClaim
   metadata:
     name: mysql-pvc
   spec:
     accessModes:
       - ReadWriteOnce
     resources:
       requests:
         storage: 1Gi
   ```

2. **Apply the PVC**:
   Apply the PVC configuration:

   ```bash
   kubectl apply -f mysql-pvc.yaml

   $ kubectl apply -f mysql-pvc.yaml
    persistentvolumeclaim/mysql-pvc created
   ```

3. **Create MySQL Deployment and Service**:
   Create a file named `mysql-deployment.yaml` with the following content to deploy MySQL:

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: mysql-deployment
   spec:
     replicas: 1
     selector:
       matchLabels:
         app: mysql
     template:
       metadata:
         labels:
           app: mysql
       spec:
         containers:
         - name: mysql
           image: mysql:5.7
           env:
           - name: MYSQL_ROOT_PASSWORD
             value: examplepassword
           ports:
           - containerPort: 3306
           volumeMounts:
           - name: mysql-persistent-storage
             mountPath: /var/lib/mysql
         volumes:
         - name: mysql-persistent-storage
           persistentVolumeClaim:
             claimName: mysql-pvc
   ---
   apiVersion: v1
   kind: Service
   metadata:
     name: mysql-service
   spec:
     selector:
       app: mysql
     ports:
       - protocol: TCP
         port: 3306
         targetPort: 3306
   ```

4. **Apply the MySQL Deployment and Service**:
   Apply the deployment and service configurations:

   ```bash
   kubectl apply -f mysql-deployment.yaml

   $ kubectl apply -f mysql-deployment.yaml
    deployment.apps/mysql-deployment created
    service/mysql-service created
   ```

5. **Create WordPress Deployment and Service**:
   Create a file named `wordpress-deployment.yaml` with the following content to deploy WordPress:

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: wordpress-deployment
   spec:
     replicas: 1
     selector:
       matchLabels:
         app: wordpress
     template:
       metadata:
         labels:
           app: wordpress
       spec:
         containers:
         - name: wordpress
           image: wordpress:latest
           env:
           - name: WORDPRESS_DB_HOST
             value: mysql-service
           - name: WORDPRESS_DB_PASSWORD
             value: examplepassword
           ports:
           - containerPort: 80
   ---
   apiVersion: v1
   kind: Service
   metadata:
     name: wordpress-service
   spec:
     type: NodePort  # Change the service type to NodePort
     selector:
       app: wordpress
     ports:
       - protocol: TCP
         port: 80
         targetPort: 80
   ```

6. **Apply the WordPress Deployment and Service**:
   Apply the deployment and service configurations:

   ```bash
   kubectl apply -f wordpress-deployment.yaml

   $ kubectl apply -f wordpress-deployment.yaml
    deployment.apps/wordpress-deployment created
    service/wordpress-service created
   ```

7. **Access WordPress**:
   Get the external IP of the nodes (usually AWS Load Balancer IP or NodePort IP):

   ```bash
   kubectl get svc wordpress-service

   $ kubectl get svc wordpress-service
    NAME                TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE
    wordpress-service   ClusterIP   10.100.248.219   <none>        80/TCP    22s


    # Note:
    Access WordPress:
    If you're running your EKS cluster in a cloud environment, you might need to configure security group rules to allow traffic to the NodePort.
    Once configured, you can access WordPress by entering the external IP of any of your EKS nodes along with the NodePort in a web browser. For example, if one of your nodes has an external IP of x.x.x.x and the NodePort is 32000, you would enter http://x.x.x.x:32000 in your browser's address bar.

   ```

   Open a web browser and enter the external IP. You should be able to set up WordPress.

8. **Clean Up**:
   To clean up the resources, delete the deployments, services, and PVC:

   ```bash
   kubectl delete deployment wordpress-deployment
   kubectl delete service wordpress-service
   kubectl delete deployment mysql-deployment
   kubectl delete service mysql-service
   kubectl delete pvc mysql-pvc

    $ kubectl delete service wordpress-service
    service "wordpress-service" deleted
    $ kubectl delete deployment mysql-deployment
    deployment.apps "mysql-deployment" deleted
    $ kubectl delete service mysql-service
    service "mysql-service" deleted
    $ kubectl delete pvc mysql-pvc
    persistentvolumeclaim "mysql-pvc" deleted
   ```

This scenario tests deploying a stateful application (WordPress) that relies on a MySQL database. It demonstrates the use of PersistentVolumeClaims for data persistence. Remember that real-world scenarios might involve more advanced configurations and optimizations.


---------------------------------------------------------------------------------------------------------------------

# Remove the complete EKS SetUp:

To completely remove an Amazon EKS cluster that you've created, you need to perform a few steps to ensure all associated resources are deleted. Here's how you can do it using the AWS CLI:

1. **Delete Node Groups**:
   Before deleting the EKS cluster, you should delete any associated node groups. Replace `<cluster-name>` with your actual cluster name:

   ```bash
   eksctl delete nodegroup --cluster <cluster-name> --name <node-group-name>

   ```2 mins```
    $ eksctl delete nodegroup --cluster myeks --name myeks-ng-public1 
    2023-08-20 18:58:16 [ℹ]  1 nodegroup (myeks-ng-public1) was included (based on the include/exclude rules)
    2023-08-20 18:58:16 [ℹ]  will drain 1 nodegroup(s) in cluster "myeks"
    2023-08-20 18:58:16 [ℹ]  starting parallel draining, max in-flight of 1
    2023-08-20 18:58:19 [ℹ]  cordon node "ip-192-168-0-203.ec2.internal"
    2023-08-20 18:58:20 [ℹ]  cordon node "ip-192-168-49-252.ec2.internal"
    2023-08-20 18:58:20 [ℹ]  cordon node "ip-192-168-54-229.ec2.internal"
    2023-08-20 18:58:35 [✔]  drained all nodes: [ip-192-168-54-229.ec2.internal ip-192-168-49-252.ec2.internal ip-192-168-0-203.ec2.internal]
    2023-08-20 18:58:35 [ℹ]  will delete 1 nodegroups from cluster "myeks"
    2023-08-20 18:58:36 [ℹ]  1 task: { 1 task: { delete nodegroup "myeks-ng-public1" [async] } }
    2023-08-20 18:58:37 [ℹ]  will delete stack "eksctl-myeks-nodegroup-myeks-ng-public1"
    2023-08-20 18:58:37 [ℹ]  will delete 0 nodegroups from auth ConfigMap in cluster "myeks"
    2023-08-20 18:58:37 [✔]  deleted 1 nodegroup(s) from cluster "myeks"
   ```

   If you used eksctl to create the node group, you need to delete each node group individually.

2. **Delete the Cluster**:
   Once all node groups are deleted, you can delete the EKS cluster itself:

   ```bash
   eksctl delete cluster --name <cluster-name>

    ```10-15 mins```
    $ eksctl delete cluster --name myeks
    2023-08-20 18:59:51 [ℹ]  deleting EKS cluster "myeks"
    2023-08-20 18:59:53 [ℹ]  will drain 0 unmanaged nodegroup(s) in cluster "myeks"
    2023-08-20 18:59:53 [ℹ]  starting parallel draining, max in-flight of 1
    2023-08-20 18:59:55 [ℹ]  deleted 0 Fargate profile(s)
    2023-08-20 18:59:59 [✔]  kubeconfig has been updated
    2023-08-20 18:59:59 [ℹ]  cleaning up AWS load balancers created by Kubernetes objects of Kind Service or Ingress
    2023-08-20 19:00:04 [ℹ]  
    3 sequential tasks: { delete nodegroup "myeks-ng-public1", delete IAM OIDC provider, delete cluster control plane "myeks" [async] 
    }
    2023-08-20 19:00:04 [ℹ]  will delete stack "eksctl-myeks-nodegroup-myeks-ng-public1"
    2023-08-20 19:00:04 [ℹ]  waiting for stack "eksctl-myeks-nodegroup-myeks-ng-public1" to get deleted
    2023-08-20 19:00:05 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-nodegroup-myeks-ng-public1"
    2023-08-20 19:00:36 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-nodegroup-myeks-ng-public1"
    2023-08-20 19:01:30 [ℹ]  waiting for CloudFormation stack "eksctl-myeks-nodegroup-myeks-ng-public1"
    2023-08-20 19:01:32 [ℹ]  will delete stack "eksctl-myeks-cluster"
    2023-08-20 19:01:33 [✔]  all cluster resources were deleted
   ```

3. **Verify Deletion**:
   After initiating the deletion, you might want to check the status of the cluster deletion:

   ```bash
   eksctl get cluster --name <cluster-name>

   $ eksctl get cluster --name myeks
    Error: unable to describe control plane "myeks": operation error EKS: DescribeCluster, https response error StatusCode: 404, RequestID: ee78a5d1-8845-4ce1-a01c-3e7fc7f317e6, ResourceNotFoundException: No cluster found for name: myeks.
   ```

   If the cluster is being deleted, you will see that the `STATUS` field is set to `DELETING`.

4. **Clean Up IAM Roles**:
   If you used `eksctl`, it will attempt to clean up IAM roles created for the cluster automatically. However, it's a good practice to double-check and ensure that there are no leftover IAM roles that are no longer needed.

Remember to replace `<cluster-name>` with the actual name of your EKS cluster, and `<node-group-name>` with the name of the node group you created (if any).

Please be cautious while performing these actions, as they will irreversibly delete your EKS cluster and associated resources. Make sure you have backups or snapshots of any critical data before proceeding.

---------------------------------------------------------------------------------------------------------------------









