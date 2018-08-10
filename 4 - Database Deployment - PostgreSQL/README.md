# Database Deployment - PostgreSQL

### Retrieve the Postgres Storage Class
`master $ curl -Lo px-postgres-sc.yaml https://git.io/fNSoP`

### Apply the spec
`master $ kubectl apply -f px-postgres-sc.yaml`

### Check the Storage Class exists
`master $ kubectl get sc`

### Retrieve the Postgres Persistent Volume Claim
`master $ curl -Lo px-postgres-pvc.yaml https://git.io/fNSoE`

### Apply the spec
`master $ kubectl apply -f px-postgres-pvc.yaml`

### Check the PVC exists
`master $ kubectl get pvc`

### Retrieve the Postgres Deployment Spec
`master $ curl -Lo px-postgres-app.yaml https://git.io/fNSix`

```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: postgres
spec:
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  replicas: 1
  template:
    metadata:
      labels:
        app: postgres
    spec:
      schedulerName: stork
      containers:
      - name: postgres
        image: postgres
        imagePullPolicy: "IfNotPresent"
        ports:
        - containerPort: 5432
        env:
        - name: POSTGRES_USER
          value: pgbench
        - name: POSTGRES_PASSWORD
          value: superpostgres
        - name: PGBENCH_PASSWORD
          value: superpostgres
        - name: PGDATA
          value: /var/lib/postgresql/data/pgdata
        volumeMounts:
        - mountPath: /var/lib/postgresql/data
          name: postgredb
      volumes:
      - name: postgredb
        persistentVolumeClaim:
          claimName: postgres-data
```

### Apply the spec
`master $ kubectl apply -f px-postgres-app.yaml`

### Watch the pod come up
`master $ watch kubectl get pods`

### Store the pod ID
`POD=postgres-584bb46dd4-g6lr8`

### Exec into the Postgres pod
`master $ kubectl exec -it $POD bash`

### Change to the postgres user
`su -s /bin/bash postgres`

### Database ops
```
psql
```
```
postgres=# create database pxdemo;
```
```
postgres=# \l
…
pxdemo    | pgbench  | UTF8     | en_US.utf8 | en_US.utf8 |
…
```
```
postgres=# \q
```

### Run pgbench
`pgbench -i -s 40 pxdemo;`

### Verify data
```
psql pxdemo
```
```
\dt
```
```
select count(*) from pgbench_accounts;

 count  
---------
 5000000
(1 row)
```
```
\q
```
```
exit
exit
```

### Check the node Postgres is running on
`master $ kubectl get pods -o wide`

### Cordon the node
`master $ kubectl cordon node02`

### Inspect the Portworx volume
`node01 $ watch pxctl inspect <postgres volume>`

### Delete the postgres pod
`master $ kubectl delete pod $POD`

### Watch the pod come back on a new node
`master $ watch kubectl get pods -l app=postgres -o wide`

### Uncordon the node
`master $ kubectl uncordon node02`

### Exec into the new Postgres pod
`master $ kubectl exec -it postgres-<something> bash`

### Verify the data still exists
```
su -s /bin/bash postgres
```
```
psql pxdemo
```
```
pxdemo=# \dt
pxdemo=# select count(*) from pgbench_accounts;
pxdemo=# \q
```
```
pxdemo=# exit
```
```
# exit
```