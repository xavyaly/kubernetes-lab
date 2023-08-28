# Create a cluster 
```
$ eksctl create cluster
```

# List the clusters
```
$ aws eks list-clusters
```

# List all nodes 
```
$ kubectl get nodes
```

# Get the node group
```
$ eksctl get nodegroup --cluster  exciting-rainbow-1693224998
OR
$ eksctl get nodegroup --cluster <cluster-name> | awk 'NR>1 {print $1}'
```

# Delete the node group
```
$ eksctl delete nodegroup --cluster exciting-rainbow-1693224998 --name ng-58bb5cd1
```

# Delete the cluster 
```
$ eksctl delete cluster --name exciting-rainbow-1693224998
```