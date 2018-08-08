# Lab Environment Setup

### Check the etcd container is running
`master $ docker container ls -a | grep etcd`

### Check the etcd endpoint is accessible from a worker node
```bash
master $ ssh root@node01 curl -s http://master:4001/version
{"etcdserver":"3.3.4","etcdcluster":"3.3.0"}
```
