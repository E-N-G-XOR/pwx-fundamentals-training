# Installing Portworc

### Portworx spec generator
https://install.portworx.com/1.4/

### Values for the install generator

| Item               | Value                 |
| -------------------|:---------------------:|
| Cluster Name       | px-demo               |
| Key Value Store    | etcd://master:4001    |
| Kubernetes Version | 1.10.0                |
| List of drives     | /dev/sdb              |
| Enable Stork       | Yes                   |
| Secrets type       | Kubernetes            |

### Generated command
```
kubectl apply -f 'http://install.portworx.com/1.4/?k=etcd://master:4001&kbver=1.10.0&c=px-demo-551f1e41-ab39-4400-ba2d-e493ef6e7790&s=/dev/sdb&st=k8s&stork=true'
```

### Check Portworx pods are running
`kubectl get pods -n kube-system -l name=portworx -o wide`