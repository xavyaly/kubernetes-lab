

# Install helm

[Link](https://helm.sh/docs/intro/install/)
```
$ brew install helm
Running `brew update --auto-update`...
==> Auto-updated Homebrew!
Updated 2 taps (homebrew/core and homebrew/cask).
==> New Formulae
aerleon               dolphie               legitify              massdriver            notation              tpm                   wtfis                 yazi
==> New Casks
dockx                                                                                     pieces

You have 6 outdated formulae and 1 outdated cask installed.

Warning: helm 3.12.3 is already installed and up-to-date.
To reinstall 3.12.3, run:
  brew reinstall helm
```

```
$ helm version --short
v3.12.3+g3a31588
```

# EKS Cluster is costly so installed minikube

[Link](https://minikube.sigs.k8s.io/docs/start/)

# Start your cluster

$ minikube start
```
$ minikube start
😄  minikube v1.30.1 on Darwin 13.4 (arm64)
🎉  minikube 1.31.2 is available! Download it: https://github.com/kubernetes/minikube/releases/tag/v1.31.2
💡  To disable this notice, run: 'minikube config set WantUpdateNotification false'

✨  Automatically selected the docker driver
📌  Using Docker Desktop driver with root privileges
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
💾  Downloading Kubernetes v1.26.3 preload ...
    > preloaded-images-k8s-v18-v1...:  330.52 MiB / 330.52 MiB  100.00% 4.00 Mi
    > gcr.io/k8s-minikube/kicbase...:  336.39 MiB / 336.39 MiB  100.00% 3.88 Mi
🔥  Creating docker container (CPUs=2, Memory=2200MB) ...
🐳  Preparing Kubernetes v1.26.3 on Docker 23.0.2 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔗  Configuring bridge CNI (Container Networking Interface) ...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🔎  Verifying Kubernetes components...
🌟  Enabled addons: storage-provisioner
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

# Play with minikube

$ minikube status
```
$ minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

```
$ kubectl get po -A
NAMESPACE     NAME                               READY   STATUS    RESTARTS        AGE
kube-system   coredns-787d4945fb-bnllt           1/1     Running   0               9m7s
kube-system   etcd-minikube                      1/1     Running   0               9m19s
kube-system   kube-apiserver-minikube            1/1     Running   0               9m22s
kube-system   kube-controller-manager-minikube   1/1     Running   0               9m22s
kube-system   kube-proxy-r6mzf                   1/1     Running   0               9m7s
kube-system   kube-scheduler-minikube            1/1     Running   0               9m19s
kube-system   storage-provisioner                1/1     Running   1 (8m37s ago)   9m17s
```

```
$ minikube dashboard
🔌  Enabling dashboard ...
    ▪ Using image docker.io/kubernetesui/dashboard:v2.7.0
    ▪ Using image docker.io/kubernetesui/metrics-scraper:v1.0.8
💡  Some dashboard features require the metrics-server addon. To enable all features please run:

        minikube addons enable metrics-server


🤔  Verifying dashboard health ...
🚀  Launching proxy ...
🤔  Verifying proxy health ...
🎉  Opening http://127.0.0.1:57387/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/ in your default browser...

redirected to 
http://127.0.0.1:57387/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/#/workloads?namespace=default
```


[Link](https://docs.aws.amazon.com/eks/latest/userguide/kubernetes-versions.html#kubernetes-release-calendar)
```
$ kubectl get pods --all-namespaces -o json | grep -E 'seccomp.security.alpha.kubernetes.io/pod|container.seccomp.security.alpha.kubernetes.io'
                    "seccomp.security.alpha.kubernetes.io/pod": "runtime/default"
```


# Follow the below link

[Link](https://cert-manager.io/docs/installation/helm/)

```
$ helm repo add jetstack https://charts.jetstack.io
"jetstack" has been added to your repositories
```


```
$ helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "jetstack" chart repository
Update Complete. ⎈Happy Helming!⎈
```


```
$ kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.12.0/cert-manager.crds.yaml
customresourcedefinition.apiextensions.k8s.io/certificaterequests.cert-manager.io created
customresourcedefinition.apiextensions.k8s.io/certificates.cert-manager.io created
customresourcedefinition.apiextensions.k8s.io/challenges.acme.cert-manager.io created
customresourcedefinition.apiextensions.k8s.io/clusterissuers.cert-manager.io created
customresourcedefinition.apiextensions.k8s.io/issuers.cert-manager.io created
customresourcedefinition.apiextensions.k8s.io/orders.acme.cert-manager.io created
```


```
$ helm install \
>   cert-manager jetstack/cert-manager \
>   --namespace cert-manager \
>   --create-namespace \
>   --version v1.12.0 \
>   # --set installCRDs=true

NAME: cert-manager
LAST DEPLOYED: Wed Aug 23 23:48:20 2023
NAMESPACE: cert-manager
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
cert-manager v1.12.0 has been deployed successfully!

In order to begin issuing certificates, you will need to set up a ClusterIssuer
or Issuer resource (for example, by creating a 'letsencrypt-staging' issuer).

More information on the different types of issuers and how to configure them
can be found in our documentation:

https://cert-manager.io/docs/configuration/

For information on how to configure cert-manager to automatically provision
Certificates for Ingress resources, take a look at the `ingress-shim`
documentation:

https://cert-manager.io/docs/usage/ingress/
```

# Install cmctl for verification

[Link](https://cert-manager.io/docs/installation/verify/)

```
$ brew install cmctl
==> Downloading https://formulae.brew.sh/api/formula.jws.json
############################################################################################################## 100.0%
==> Downloading https://formulae.brew.sh/api/cask.jws.json
##O#-  #                                                                                                            
==> Fetching cmctl
==> Downloading https://ghcr.io/v2/homebrew/core/cmctl/manifests/1.12.3
############################################################################################################## 100.0%
==> Downloading https://ghcr.io/v2/homebrew/core/cmctl/blobs/sha256:94dc21ab3d08eb5751657545dfde3ccbf286ee990a8f1b93e
############################################################################################################## 100.0%
==> Pouring cmctl--1.12.3.arm64_ventura.bottle.tar.gz
==> Caveats
Bash completion has been installed to:
  /opt/homebrew/etc/bash_completion.d
==> Summary
🍺  /opt/homebrew/Cellar/cmctl/1.12.3: 8 files, 62.5MB
==> Running `brew cleanup cmctl`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
```


# Verify the installation

[Link](https://cert-manager.io/docs/installation/verify/)

```
$ cmctl check api
The cert-manager API is ready
```


```
$ kubectl get pods --namespace cert-manager
NAME                                      READY   STATUS    RESTARTS   AGE
cert-manager-74654c4948-tn7pc             1/1     Running   0          5m41s
cert-manager-cainjector-77644bff8-wqpl9   1/1     Running   0          5m41s
cert-manager-webhook-54d7657dbb-m6gpk     1/1     Running   0          5m41s
```


```
$ cat <<EOF > test-resources.yaml
> apiVersion: v1
> kind: Namespace
> metadata:
>   name: cert-manager-test
> ---
> apiVersion: cert-manager.io/v1
> kind: Issuer
> metadata:
>   name: test-selfsigned
>   namespace: cert-manager-test
> spec:
>   selfSigned: {}
> ---
> apiVersion: cert-manager.io/v1
> kind: Certificate
> metadata:
>   name: selfsigned-cert
>   namespace: cert-manager-test
> spec:
>   dnsNames:
>     - example.com
>   secretName: selfsigned-cert-tls
>   issuerRef:
>     name: test-selfsigned
> EOF
```


```
$ kubectl apply -f test-resources.yaml 
namespace/cert-manager-test created
issuer.cert-manager.io/test-selfsigned created
certificate.cert-manager.io/selfsigned-cert created
```


```
$ kubectl describe certificates -n cert-manager-test
Name:         selfsigned-cert
Namespace:    cert-manager-test
Labels:       <none>
Annotations:  <none>
API Version:  cert-manager.io/v1
Kind:         Certificate
Metadata:
  Creation Timestamp:  2023-08-23T18:26:00Z
  Generation:          1
  Managed Fields:
    API Version:  cert-manager.io/v1
    Fields Type:  FieldsV1
    fieldsV1:
      f:status:
        f:revision:
    Manager:      cert-manager-certificates-issuing
    Operation:    Update
    Subresource:  status
    Time:         2023-08-23T18:26:00Z
    API Version:  cert-manager.io/v1
    Fields Type:  FieldsV1
    fieldsV1:
      f:status:
        .:
        f:conditions:
          .:
          k:{"type":"Ready"}:
            .:
            f:lastTransitionTime:
            f:message:
            f:observedGeneration:
            f:reason:
            f:status:
            f:type:
        f:notAfter:
        f:notBefore:
        f:renewalTime:
    Manager:      cert-manager-certificates-readiness
    Operation:    Update
    Subresource:  status
    Time:         2023-08-23T18:26:00Z
    API Version:  cert-manager.io/v1
    Fields Type:  FieldsV1
    fieldsV1:
      f:metadata:
        f:annotations:
          .:
          f:kubectl.kubernetes.io/last-applied-configuration:
      f:spec:
        .:
        f:dnsNames:
        f:issuerRef:
          .:
          f:name:
        f:secretName:
    Manager:         kubectl-client-side-apply
    Operation:       Update
    Time:            2023-08-23T18:26:00Z
  Resource Version:  2990
  UID:               4bc5d7d5-37f4-467d-84d5-3794df082be6
Spec:
  Dns Names:
    example.com
  Issuer Ref:
    Name:       test-selfsigned
  Secret Name:  selfsigned-cert-tls
Status:
  Conditions:
    Last Transition Time:  2023-08-23T18:26:00Z
    Message:               Certificate is up to date and has not expired
    Observed Generation:   1
    Reason:                Ready
    Status:                True
    Type:                  Ready
  Not After:               2023-11-21T18:26:00Z
  Not Before:              2023-08-23T18:26:00Z
  Renewal Time:            2023-10-22T18:26:00Z
  Revision:                1
Events:
  Type    Reason     Age   From                                       Message
  ----    ------     ----  ----                                       -------
  Normal  Issuing    57s   cert-manager-certificates-trigger          Issuing certificate as Secret does not exist
  Normal  Generated  57s   cert-manager-certificates-key-manager      Stored new private key in temporary Secret resource "selfsigned-cert-fxrn2"
  Normal  Requested  57s   cert-manager-certificates-request-manager  Created new CertificateRequest resource "selfsigned-cert-z8gg4"
  Normal  Issuing    57s   cert-manager-certificates-issuing          The certificate has been successfully issued

# Done

```


```
$ kubectl delete -f test-resources.yaml 
namespace "cert-manager-test" deleted
issuer.cert-manager.io "test-selfsigned" deleted
certificate.cert-manager.io "selfsigned-cert" deleted

# Verification Done

```

# Installing cert-manager as subchart

```
$ cat Chart.yaml 
apiVersion: v2
name: example_chart
description: A Helm chart with cert-manager as subchart
type: application
version: 0.1.0
appVersion: "0.1.0"
dependencies:
  - name: cert-manager
    version: v1.12.0
    repository: https://charts.jetstack.io
    alias: cert-manager
    condition: cert-manager.enabled
```

```
helm install example example_chart \
  --namespace example \
  --create-namespace \
  --set cert-manager.namespace=security
```

# ERROR

```
$ helm install example example_chart \
>   --namespace example \
>   --create-namespace \
>   --set cert-manager.namespace=security
Error: INSTALLATION FAILED: non-absolute URLs should be in form of repo_name/path_to_chart, got: example_chart
```



$ minikube stop
✋  Stopping node "minikube"  ...
🛑  Powering off "minikube" via SSH ...
🛑  1 node stopped.
