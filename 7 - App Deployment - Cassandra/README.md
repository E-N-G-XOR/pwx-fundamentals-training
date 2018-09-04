# App Deployment - Cassandra

### Retrieve the Cassandra SC
`master $ curl -Lo px-cassandra-sc.yaml https://git.io/fABOR`

### View the SC
`cat px-cassandra-sc.yaml`

### Apple the Storage Class
`master $ kubectl create -f px-cassandra-sc.yaml`

### Retrieve the Cassandra StatefulSet
`master $ curl -Lo px-cassandra-ss.yaml https://git.io/fABY9`

### View the StatefulSet spec and take note of the volumeClaimTemplates section
`master $ cat px-cassandra-ss.yaml`

### Apply the StatefulSet
`master $ kubectl create -f px-cassandra-ss.yaml`

### Watch for the pods to come up
`master $ watch kubectl get pods`

### Inspect the Cassandra pod volume
`node01 $ pxctl v i <cassandra pvc>`

### Exec into the CLI pod
`kubectl exec -it cqlsh -- cqlsh cassandra-0.cassandra.default.svc.cluster.local --cqlversion=3.4.4`

### Populate some data
```bash
CREATE KEYSPACE portworx WITH REPLICATION = {'class':'SimpleStrategy','replication_factor':3};
USE portworx;
CREATE TABLE features (id varchar PRIMARY KEY, name varchar, value varchar);
INSERT INTO portworx.features (id, name, value) VALUES ('px-1', 'snapshots', 'point in time recovery!');
INSERT INTO portworx.features (id, name, value) VALUES ('px-2', 'cloudsnaps', 'backup/restore to/from any cloud!');
INSERT INTO portworx.features (id, name, value) VALUES ('px-3', 'STORK', 'convergence, scale, and high availability!');
INSERT INTO portworx.features (id, name, value) VALUES ('px-4', 'share-volumes', 'better than NFS, run wordpress on k8s!');
INSERT INTO portworx.features (id, name, value) VALUES ('px-5', 'DevOps', 'your data needs to be automated too!');
```

### Check the data
`SELECT id, name, value FROM portworx.features;`
