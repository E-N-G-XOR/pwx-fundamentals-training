### Create a volume using pxctl
`pxctl volume create --shared --size 2 testvol`

### Check the volume exists
`pxctl volume list`

# Basic Volume Management

### Inspect the volume
`pxctl volume inspect testvol`

### Get the nginx pod spec
`curl -o nginx-pod.yaml https://gitlab.com/snippets/1721417/raw`

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
`kubectl apply -f nginx-pod.yaml`

### Check the pod is running
`watch kubectl get pods -o wide -n default`

### View the attached volume
`pxctl volume list -a`

### Verify volume in the pod
`kubectl exec nginx -- df -h | grep px`