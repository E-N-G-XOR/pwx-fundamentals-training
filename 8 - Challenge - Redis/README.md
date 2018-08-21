# Final Challenge - Redis Deployment and Management

1. Get in pairs!
2. Create a Storage Class and PVC for Redis on one cluster (repl 1)
3. Deploy Redis - https://gitlab.com/snippets/1728241/raw 
4. Populate the database with some sample data
5. Resize the data volume
6. Scale the HA value to 3
7. Create a cloud backup to a local object store
8. Restore snapshot to your partner’s cluster
9. Deploy Redis in second cluster using the snapshot (create PVC)
10. Validate data availability

### Sample Data for Redis
```bash
master $ kubectl exec -it <redis pod> sh 
/data # redis-cli
127.0.0.1:6379> auth password
OK
127.0.0.1:6379> SET Me Joe
OK
127.0.0.1:6379> SET You Class
OK
127.0.0.1:6379> GET Me
"Joe"
127.0.0.1:6379> GET You
"Class"
```