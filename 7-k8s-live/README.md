# Kubernetes Contents #

# Core Concept #

Kubernetes
    Why K8s required ?
    CRI/CNI/CSI
Kubernetes Architecture
    Master Components
    Worker Components
Methods of building k8s cluster
Kubectl
Pod
Workload
Deployment
Namespace
Resource quota, Limit range
Resource requirements & Limit
Service
Endpoint
Dns
Daemonset
Static pod
Autoscaling (HPA ,VPA)
Job & Cronjob 36 Statefulset
Headless service
Statefulset & storage

# Scheduling #

How scheduling works? 12 Label & selector
Annotations
Node selector
Affinity & anti-affinity
Taint & toleration
Taint/tolerate & node affinity
Priority class & preemption
Pod distribution budget 15 Bin packing

# Lifecycle Management # 

Configmap, Secret
Init Container
Pod Lifecycle
Sidecar Container
Rollout & Rollback
Probes
Node Maintenance
Cluster upgrade
Backup & Restore

# Security # 

Security
Authentication
Authorization (RBAC)
Admission control
Service Account
Api groups
Kubeconfig
Authentication with X509
Auditing
RuntimeClass
Network Policy
Security Context
Image security
Gatekeeper

# Storage # 

HostPath volume 34 EmptyDir volume
Persistent volume(pv) & pvc
Static & Dynamic provisioning

# Addons # 

How to deploy an application in k8s? 
Kustomize
Helm
Operator
Ingress
Cert-Manager