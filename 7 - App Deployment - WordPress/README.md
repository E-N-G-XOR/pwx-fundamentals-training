# App Deployment - WordPress

### Retrieve MySQL spec for SC and PVC
`master $ curl -Lo mysql-vol.yaml https://git.io/fAfwx`

### Retrieve WordPress spec for SC and PVC
`master $ curl -Lo wp-vol.yaml https://git.io/fAfry`

### Apply both specs
`master $ kubectl apply -f mysql-vol.yaml`
`master $ kubectl apply -f wp-vol.yaml`

### Verify creation
`master $ kubectl get sc`
`master $ kubectl get pvc`

### Apply MySQL deployment
`kubectl apply -f https://git.io/fAfoq`

### Apply WordPress deployment
`kubectl apply -f https://git.io/fAfoB`

### Get WordPress Pod ID
`kubectl get pods`

### Replace WordPress theme header file
`master $ kubectl exec <pod> -- curl -o wp-content/themes/twentyfifteen/header.php https://git.io/fAfod`

### Scale WordPress deployment
`master $ kubectl scale --replicas=5 deployment/wordpress`