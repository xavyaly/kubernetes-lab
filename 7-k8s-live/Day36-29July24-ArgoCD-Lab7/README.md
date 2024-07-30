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

# Day37 

# Namespaces

'''
kubectl get ns
NAME              STATUS   AGE
default           Active   22m
kube-node-lease   Active   22m
kube-public       Active   22m
kube-system       Active   22m

kubectl  create namespace argocd
namespace/argocd created

kubectl get ns
NAME              STATUS   AGE
argocd            Active   5s
'''

# Install argoCD

'''
cat argocd.sh 
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

chmod +x argocd.sh

./argocd.sh
'''

# List all

'''
kubectl get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   172.20.0.1   <none>        443/TCP   26m

kubectl get all -n argocd
NAME                                                    READY   STATUS    RESTARTS   AGE
pod/argocd-application-controller-0                     1/1     Running   0          64s
pod/argocd-applicationset-controller-8485455fd5-7grqh   1/1     Running   0          67s
pod/argocd-dex-server-66779d96df-bzr9h                  1/1     Running   0          67s
pod/argocd-notifications-controller-c4b69fb67-9nbw4     1/1     Running   0          66s
pod/argocd-redis-7bf7cb9748-vqxn6                       1/1     Running   0          65s
pod/argocd-repo-server-795d79dfb6-4s82x                 1/1     Running   0          65s
pod/argocd-server-544b7f897d-wftlr                      1/1     Running   0          64s

NAME                                              TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
service/argocd-applicationset-controller          ClusterIP   172.20.30.70     <none>        7000/TCP,8080/TCP            73s
service/argocd-dex-server                         ClusterIP   172.20.188.250   <none>        5556/TCP,5557/TCP,5558/TCP   72s
service/argocd-metrics                            ClusterIP   172.20.254.148   <none>        8082/TCP                     72s
service/argocd-notifications-controller-metrics   ClusterIP   172.20.213.209   <none>        9001/TCP                     71s
service/argocd-redis                              ClusterIP   172.20.72.194    <none>        6379/TCP                     71s
service/argocd-repo-server                        ClusterIP   172.20.48.110    <none>        8081/TCP,8084/TCP            70s
service/argocd-server                             ClusterIP   172.20.208.215   <none>        80/TCP,443/TCP               69s
service/argocd-server-metrics                     ClusterIP   172.20.53.90     <none>        8083/TCP                     69s

NAME                                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/argocd-applicationset-controller   1/1     1            1           68s
deployment.apps/argocd-dex-server                  1/1     1            1           68s
deployment.apps/argocd-notifications-controller    1/1     1            1           67s
deployment.apps/argocd-redis                       1/1     1            1           67s
deployment.apps/argocd-repo-server                 1/1     1            1           66s
deployment.apps/argocd-server                      1/1     1            1           65s

NAME                                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/argocd-applicationset-controller-8485455fd5   1         1         1       69s
replicaset.apps/argocd-dex-server-66779d96df                  1         1         1       69s
replicaset.apps/argocd-notifications-controller-c4b69fb67     1         1         1       68s
replicaset.apps/argocd-redis-7bf7cb9748                       1         1         1       68s
replicaset.apps/argocd-repo-server-795d79dfb6                 1         1         1       67s
replicaset.apps/argocd-server-544b7f897d                      1         1         1       66s

NAME                                             READY   AGE
statefulset.apps/argocd-application-controller   1/1     66s


kubectl get svc -n argocd
NAME                                      TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
argocd-applicationset-controller          ClusterIP   172.20.30.70     <none>        7000/TCP,8080/TCP            2m46s
argocd-dex-server                         ClusterIP   172.20.188.250   <none>        5556/TCP,5557/TCP,5558/TCP   2m45s
argocd-metrics                            ClusterIP   172.20.254.148   <none>        8082/TCP                     2m45s
argocd-notifications-controller-metrics   ClusterIP   172.20.213.209   <none>        9001/TCP                     2m44s
argocd-redis                              ClusterIP   172.20.72.194    <none>        6379/TCP                     2m44s
argocd-repo-server                        ClusterIP   172.20.48.110    <none>        8081/TCP,8084/TCP            2m43s
argocd-server                             ClusterIP   172.20.208.215   <none>        80/TCP,443/TCP               2m42s
argocd-server-metrics                     ClusterIP   172.20.53.90     <none>        8083/TCP                     2m42s
'''

# Note

'''
Agrocd-server service is mapping to “ClusterIP”. We need to change it to “LoadBalancer” or
“NodePort” to access the agrocd UI from outside world.

kubectl patch svc argocd-server -n argocd -p '{"spec": {"type":"LoadBalancer"}}'
service/argocd-server patched

kubectl get svc -n argocd
NAME                                      TYPE           CLUSTER-IP       EXTERNAL-IP                                                                PORT(S)                      AGE
argocd-applicationset-controller          ClusterIP      172.20.30.70     <none>                                                                     7000/TCP,8080/TCP            4m33s
argocd-dex-server                         ClusterIP      172.20.188.250   <none>                                                                     5556/TCP,5557/TCP,5558/TCP   4m32s
argocd-metrics                            ClusterIP      172.20.254.148   <none>                                                                     8082/TCP                     4m32s
argocd-notifications-controller-metrics   ClusterIP      172.20.213.209   <none>                                                                     9001/TCP                     4m31s
argocd-redis                              ClusterIP      172.20.72.194    <none>                                                                     6379/TCP                     4m31s
argocd-repo-server                        ClusterIP      172.20.48.110    <none>                                                                     8081/TCP,8084/TCP            4m30s
argocd-server                             LoadBalancer   172.20.208.215   a510c2a7743ba4d5e8a6b985e934728d-1187100072.ap-south-1.elb.amazonaws.com   80:31348/TCP,443:31646/TCP   4m29s
argocd-server-metrics                     ClusterIP      172.20.53.90     <none>                                                                     8083/TCP                     4m29s

kubectl get svc argocd-server -n argocd
NAME            TYPE           CLUSTER-IP       EXTERNAL-IP                                                                PORT(S)                      AGE
argocd-server   LoadBalancer   172.20.208.215   a510c2a7743ba4d5e8a6b985e934728d-1187100072.ap-south-1.elb.amazonaws.com   80:31348/TCP,443:31646/TCP   5m35s
'''

# Now we can login to argocd using Loadbalancer.

'''
URL: https://a510c2a7743ba4d5e8a6b985e934728d-1187100072.ap-south-1.elb.amazonaws.com

Creds: admin/58Nk5a2gAfp***u # Refer below
'''

# Fetch the password 

'''
kubectl get secret -n argocd
NAME                          TYPE     DATA   AGE
argocd-initial-admin-secret   Opaque   1      8m5s
argocd-notifications-secret   Opaque   0      8m25s
argocd-redis                  Opaque   1      8m7s
argocd-secret                 Opaque   5      8m25s

kubectl get secret -n argocd argocd-initial-admin-secret -o yaml
apiVersion: v1
data:
  password: NThOazVhMmdBZnAzcnh***==
kind: Secret
metadata:
  creationTimestamp: "2024-07-30T03:15:29Z"
  name: argocd-initial-admin-secret
  namespace: argocd
  resourceVersion: "4851"
  uid: 56f77a7a-e7d5-4ad1-8ff7-c37e7f949ff4
type: Opaque

echo "NThOazVhMmdBZnAzcnhFdQ==" | base64 -d
58Nk5a2gAfp***u
'''

# Install argocd cli

'''
cat argocdcli.sh 
sudo curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo chmod +x /usr/local/bin/argocd

./argocdcli.sh

argocd version
argocd: v2.11.7+e4a0246
  BuildDate: 2024-07-24T10:10:59Z
  GitCommit: e4a0246c4d920bc1e5ee5f9048a99eca7e1d53cb
  GitTreeState: clean
  GoVersion: go1.21.12
  Compiler: gc
  Platform: linux/amd64
FATA[0000] Failed to establish connection to a21518392c4a24591aee5fe3f2aee6ae-1593474350.ap-south-1.elb.amazonaws.com:443: dial tcp: lookup a21518392c4a24591aee5fe3f2aee6ae-1593474350.ap-south-1.elb.amazonaws.com on 127.0.0.53:53: no such host
'''

# Login to argocd cli

'''
argocd login $(kubectl get service argocd-server -n argocd --output=jsonpath='{.status.loadBalancer.ingress[0].hostname}') --username admin --password $(kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo) --insecure
'admin:login' logged in successfully
Context 'a510c2a7743ba4d5e8a6b985e934728d-1187100072.ap-south-1.elb.amazonaws.com' updated
'''

# Depoy one python flask application 

# Once deployed from argoCD UI

'''
kubectl get deploy
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
python-flask   1/1     1            1           2m25s

kubectl get svc 
NAME          TYPE           CLUSTER-IP      EXTERNAL-IP                                                               PORT(S)          AGE
kubernetes    ClusterIP      172.20.0.1      <none>                                                                    443/TCP          50m
python-load   LoadBalancer   172.20.218.39   abcab9d6de1cc4b56b4a9b3ba5b55811-230405789.ap-south-1.elb.amazonaws.com   5001:32287/TCP   2m43s
'''

# Fetch the UI 

'''
http://abcab9d6de1cc4b56b4a9b3ba5b55811-230405789.ap-south-1.elb.amazonaws.com:5001

welcome to the flask tutorials updated
'''

# Now we can update our python app and create new image and push to dockerhub. 

# Update that image details in our deploy.yml file in our repo and click on sync in app.

'''
cat flask-demo/demo.py 
from flask import Flask 
app = Flask(__name__) 
  
@app.route('/') 
def hello(): 
    return "welcome to the flask tutorials updated..."
  
  
if __name__ == "__main__": 
    app.run(host ='0.0.0.0', port = 5001, debug = True)
'''

# To build a new images from the recent changes

'''
docker build -t 782782/python-flask:0.1 flask-demo/.

docker images
REPOSITORY            TAG         IMAGE ID       CREATED          SIZE
782782/python-flask   0.1         6524a26d3686   28 seconds ago   92.4MB

docker push 782782/python-flask:0.1
'''

# Re sync suppose to work 