# Pre-Requisites

Install AWS CLI and Configure

Command to execute: 
$ aws configure 

```
$ aws configure 
AWS Access Key ID [****************5V7G]: 
AWS Secret Access Key [****************MF1Y]: 
Default region name [us-west-2]:          
Default output format [json]:       
```

----

$ aws s3 ls 
2023-07-06 10:39:17 knowledgeadda1
2023-07-01 14:17:49 learnzone
2023-08-09 16:08:34 unique-bucket-09082023

----

AWS CLI to create EKS Cluster: [Link](https://docs.aws.amazon.com/cli/latest/reference/eks/create-cluster.html)

----

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

-----


