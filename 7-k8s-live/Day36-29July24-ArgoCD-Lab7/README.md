# Fork to GitHub repo

'''
https://github.com/xavyaly/blue-green
'''

# Install aws cli 

[Link](https://medium.com/@javedalam1303/install-awscli-2ea38c4873c3)

# For cross check 

'''
cat ~/.aws/config
cat ~/.aws/credentials

aws s3 ls 
2023-07-06 05:09:17 knowledgeadda1
2023-07-01 08:47:49 learnzone
2023-08-09 10:38:34 unique-bucket-09082023
'''

# Install kubectl 

[Link](https://k8s-docs.netlify.app/en/docs/tasks/tools/install-kubectl/)

'''
kubectl version --client
Client Version: v1.30.3
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
'''

# Install terraform

[Link](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

'''
terraform version
Terraform v1.9.3
on linux_amd64
'''

# Clone the fork repo

'''
git clone https://github.com/xavyaly/blue-green.git
Cloning into 'blue-green'...
remote: Enumerating objects: 103, done.
remote: Counting objects: 100% (103/103), done.
remote: Compressing objects: 100% (69/69), done.
remote: Total 103 (delta 47), reused 74 (delta 32), pack-reused 0
Receiving objects: 100% (103/103), 15.66 KiB | 1.96 MiB/s, done.
Resolving deltas: 100% (47/47), done.
ubuntu@ip-172-31-21-244:~$ ls blue-green/

cd blue-green/
ls
README.md  blue  eks-cluster.tf  eks-out.tf  eks-vpc.tf  eks-worker-nodes.tf  green  vars.tf  version.tf
'''

# Create an EKS Cluster with the help of terraform code 

'''
$ terraform init

$ ls  .terraform/providers/registry.terraform.io/hashicorp/aws/5.60.0/linux_amd64/
LICENSE.txt  terraform-provider-aws_v5.60.0_x5

$ terraform plan 

'''
Plan: 20 to add, 0 to change, 0 to destroy.
'''

$ terraform apply 
OR
$ terraform apply --auto-approve 

'''
Apply complete! Resources: 20 added, 0 changed, 0 destroyed.
'''

'''

# To cross check the EKS cluster 

'''
aws eks list-clusters --region ap-south-1
{
    "clusters": [
        "eks_cluster_demo"
    ]
}
'''

# Update the kubeconfig

'''
aws eks --region ap-south-1 update-kubeconfig --name eks_cluster_demo
Updated context arn:aws:eks:ap-south-1:252473277340:cluster/eks_cluster_demo in /home/ubuntu/.kube/config
'''

# 



