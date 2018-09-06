# Basic Volume Management

### Create a volume using pxctl
`node01 $ pxctl volume create --shared --size 2 testvol`

### Check the volume exists
`node01 $ pxctl volume list`

### Inspect the volume
`node01 $ pxctl volume inspect testvol`

### Get the nginx pod spec
`master $ curl -Lo nginx-pod.yaml https://git.io/fNSoJ`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
    volumeMounts:
    - name: testvol
      mountPath: /usr/share/nginx/html
    ports:
    - containerPort: 80
  volumes:
  - name: testvol
    portworxVolume:
      volumeID: "testvol"
```

### Apply the nginx pod spec
`master $ kubectl apply -f nginx-pod.yaml`

### Check the pod is running
`master $ watch kubectl get pods -o wide -n default`

### View the attached volume
`node01 $ pxctl volume list -a`

### Verify volume in the pod
`master $ kubectl exec nginx -- df -h | grep px`
