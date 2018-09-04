# Cloud Snapshots - PostgreSQL

### Explore ObjectStore options
`node01 $ pxctl objectstore`

### Create the ObjectStore
`node01 $ pxctl objectstore create --size 5`

### View ObjectStore volumes
`node01 $ pxctl volume list`

### Start the ObjectStore
`node01 sudo pxctl objectstore start`

### Check ObjectStore status
`node01 $ pxctl objectstore status`

### View secrets options
`node01 $ pxctl secrets`

### Login to K8s secrets
`pxctl secrets k8s login`

### Create secrets store credentials
```bash
node01 $ pxctl credentials create --provider s3 \
--s3-access-key <access key> \
--s3-secret-key <secret key> \
--s3-region us-east-1 \
--s3-endpoint node01:9010 \
--s3-disable-ssl
```
### Check credentials exist
`node01 $ pxctl credentials list`

### Validate credentials
`node01 $ pxctl credentials validate --uuid <UUID>`

### Get UUID of original Postgres snapshot
`node01 $ pxctl volume list`

### Backup this snapshot to the ObjectStore
`node01 $ pxctl cloudsnap backup <snapshot volume ID>`

### Check the backup completes
`node01 $ pxctl cloudsnap status`

### Delete the local snapshot
`node01 $ pxctl volume list`
`node01 $ pxctl volume delete <snapshot volume ID>`

### Retrieve the cloudsnap ID
`node01 $ pxctl cloudsnap list`

### Restore the snapshot
`node01 $ pxctl cloudsnap restore --snap <snap ID>`

