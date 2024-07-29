# Fork to GitHub repo

'''
https://github.com/xavyaly/blue-green
'''

# Install aws cli 

[Link](https://medium.com/@javedalam1303/install-awscli-2ea38c4873c3)

'''
aws --version
aws-cli/2.17.18 Python/3.11.9 Linux/6.8.0-1011-aws exe/x86_64.ubuntu.24
'''

# To configure aws cli and cross check 

$ aws configure 
AWS Access Key ID [****************5V7G]: 
AWS Secret Access Key [****************MF1Y]: 
Default region name [us-west-2]: 
Default output format [json]: 

'''
cat ~/.aws/config
cat ~/.aws/credentials

aws s3 ls # List all the existing buckets in AWS Account
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

Plan: 20 to add, 0 to change, 0 to destroy.

$ terraform apply 
OR
$ terraform apply --auto-approve 

Apply complete! Resources: 20 added, 0 changed, 0 destroyed.

'''

# To cross check the EKS cluster using CLI

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

# If you want to remove the EKS cluster using terraform

'''
$ terraform destroy --auto-approve

Destroy complete! Resources: 20 destroyed.
'''




