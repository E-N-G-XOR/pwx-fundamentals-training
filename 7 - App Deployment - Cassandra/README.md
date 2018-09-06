# App Deployment - Cassandra

### Retrieve the Cassandra SC
`master $ curl -Lo px-cassandra-sc.yaml https://git.io/fABOR`

### View the SC
`master $ cat px-cassandra-sc.yaml`

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
`master $ kubectl exec -it cqlsh -- cqlsh cassandra-0.cassandra.default.svc.cluster.local --cqlversion=3.4.4`

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

### Then quit the Cassandra CLI
`quit`

### Scale the cluster
`master $ kubectl scale sts cassandra --replicas=3`

### Watch the replicas come up
`master $ watch kubectl get pods -o wide`

### Retrieve the Cassandra StatefulSet snapshot yaml
`master $ curl -Lo px-cassandra-snap.yaml https://git.io/fAE2e`

### Apply the snapshot yaml
`master $ kubectl create -f px-cassandra-snap.yaml`

### Check the snapshots are created
`master $ kubectl get volumesnapshot,volumesnapshotdatas`

### Drop the Portworx Features table
```
master $ kubectl exec -it cqlsh -- cqlsh cassandra-0.cassandra.default.svc.cluster.local --cqlversion=3.4.4 
DROP TABLE IF EXISTS portworx.features; 
```

### Verify it's deleted
`SELECT id, name, value FROM portworx.features;`

### Quit out of exec
`quit`

### Scale the Cassandra SS down
`master $ kubectl scale sts cassandra --replicas=0`

### Verify pods are deleted
`master $ watch kubectl get pods`

### Shell into a Portworx container
```
master $ PX_POD=$(kubectl get pods -l name=portworx -n kube-system -o jsonpath='{.items[0].metadata.name}') 
master $ kubectl exec -n kube-system -it $PX_POD bash
```

### Clone and restore volumes with the same name from snapshots
```bash
for pvc in /opt/pwx/bin/pxctl v l | grep group | awk '{print $2}'; do /opt/pwx/bin/pxctl v d ${pvc:24:100} -f; /opt/pwx/bin/pxctl v clone --name ${pvc:24:100} $pvc; done exit
```

### Scale the Cassandra StatefulSet back up to 3
`master $ kubectl scale --replicas=3 sts cassandra`

### Verify the pods scale back up
`master $ watch kubectl get pods`

### Start a csql shell
`master $ kubectl exec -it cqlsh -- cqlsh cassandra-0.cassandra.default.svc.cluster.local --cqlversion=3.4.4`

### Check our table is there
`SELECT id, name, value FROM portworx.features;`

### Quit the CLI
`quit`

