
* package : etcdserver
* file : server.go
* method : NewServer

The first time through on startup this case gets called by default:

case !haveWAL && cfg.NewCluster:

Each subsequent call this case gets called:

case haveWAL:

See [cluster.md](https://github.com/stormasm/etcd-notes/blob/master/cluster.md)
for more details on clustering.
