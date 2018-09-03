# Installing Portworx

### Portworx spec generator
https://install.portworx.com/1.5/

### Values for the install generator

| Item                         | Value                 |
| -----------------------------|:---------------------:|
| Kubernetes Version           | 1.9.10                |
| ETCD                         | Built In (Beta)       |
| Storage (existing)           | /dev/sdb              |
| Data and Mngmt interface     | ens3                  |
| Enable Stork                 | Yes                   |
| Secrets type                 | Kubernetes            |
| Cluster name                 | px-demo               |

### Generated command
```
master $ kubectl apply -f 'https://install.portworx.com/1.5?kbver=1.9.10&b=true&s=%2Fdev%2Fsdb&m=ens3&d=ens3&c=px-demo&stork=true&st=k8s'
```

### Check Portworx pods are running
`master $ watch kubectl get pods -n kube-system -l name=portworx -o wide`