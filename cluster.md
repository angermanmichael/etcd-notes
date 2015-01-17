
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
