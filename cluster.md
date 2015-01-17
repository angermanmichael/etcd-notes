
* package : etcdmain
* file : etcd.go
* method : startEtcd

This is the first log message upon startup:

It is here that

```
func setupCluster(cfg *config) (*etcdserver.Cluster, error) {
```

gets called and the default case gets triggered
firing off this method:

```
cls, err = etcdserver.NewClusterFromString(cfg.initialClusterToken, cfg.initialCluster)
```

After bringing down etcd and bringing it up again with the WAL file in place
the same method gets fired off again.

* package : etcdserver
* file : cluster.go
* method : NewClusterFromString

```
log.Printf("etcdserver: cluster.go, token = %s, cluster = %s",token,cluster)
```

produces this output:

token = etcd-cluster,
cluster = default=http://localhost:2380,  default=http://localhost:7001

You can test to make sure these URL's are up and running by pointing your browser
at them.  You will get a *404 page not found* proving the endpoint is up and running.
