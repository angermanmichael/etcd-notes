
* package : etcdserver
* file : server.go
* method : NewServer

The first time through on startup this case gets called because no WAL file exists.

case !haveWAL && cfg.NewCluster:

Each subsequent call this case gets called:

case haveWAL:

This time through

* package : etcdserver
* file : cluster.go
* method : NewClusterFromStore

```
func NewClusterFromStore(token string, st store.Store) *Cluster {
```

See [cluster.md](https://github.com/stormasm/etcd-notes/blob/master/cluster.md)
for more details on clustering.
