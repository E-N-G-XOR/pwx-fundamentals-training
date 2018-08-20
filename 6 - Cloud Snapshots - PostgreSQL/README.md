# Cloud Snapshots - PostgreSQL

### Explore ObjectStore options
`node01 $ pxctl objectstore`

### Create the ObjectStore
`node01 $ pxctl objectstore create --size 5`

### View ObjectStore volumes
`node01 $ pxctl volume list`

### Start the ObjectStore
`sudo pxctl objectstore start`

### Check ObjectStore status
`node01 $ pxctl objectstore status`

### View secrets options
`node01 $ pxctl secrets`

### Login to K8s secrets
`pxctl secrets k8s login`

### Create secrets store credentials
```bash
node01 $ pxctl credentials create --provider s3 \
--s3-access-key X0IK2K7XPFQ5UWWXN5YY \
--s3-secret-key 5gytOkRYf46zQZ2J9XcO7xJdqQLhiWUEYmIhKfHq \
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
`node01 $ pxctl cloudsnap backup 944766022210045359`

### Check the backup completes
`node01 $ pxctl cloudsnap status`

### Retrieve the cloudsnap ID
`node01 $ pxctl cloudsnap list`

### Restore the snapshot
`node01 $ pxctl cloudsnap restore --snap 181112018587037740-545317760526242886`

