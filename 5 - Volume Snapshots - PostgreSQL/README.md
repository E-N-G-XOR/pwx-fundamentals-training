# Volume Snapshots - PostgreSQL

### Curl the snapshot spec
`master $ curl -Lo px-snap.yaml https://git.io/fAfJE`

### Apple the spec
`kubectl create -f px-snap.yaml`

### Drop the Postgres database
Exec into the pod
`master $ kubectl exec -it <postgres-pod> bash`

```bash
$ su -s /bin/bash postgres
$ psql
drop database pxdemo;
\l
\q
exit
exit
```

### Curl the restore spec
`master $ curl -Lo px-snap-pvc.yaml https://git.io/fAfTf`

### Apply the restore spec
`master $ kubectl create -f px-snap-pvc.yaml`

### View the created PVCs
`master $ kubectl get pvc`

### Curl the new Postgres deploy spec
`master $ curl -Lo postgres-app-restore.yaml https://git.io/fAfTi`

### Apply the spec
`master $ kubectl apply -f postgres-app-restore.yaml`

### Wait for the pod to come up
`watch kubectl get pods`

### Check the data is restored
`master $ kubectl exec -it postgres-snap-<id> bash`

```bash
$ su -s /bin/bash postgres
$ psql pxdemo
# \dt
# select count(*) from pgbench_accounts;
# \q
# exit
# exit
```