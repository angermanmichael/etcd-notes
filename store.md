
Replacing the
[store package]
(http://godoc.org/github.com/coreos/etcd/store)
in etcd is not a good idea because Etcd itself uses the store to save
off configuration information related to cluster members.

In fact, if you look where store is referenced in Etcd it is only referenced
in 3 files in the
[etcdserver package]
(http://godoc.org/github.com/coreos/etcd/etcdserver)
namely:

* cluster.go
* member.go
* server.go

Along with one file in
[etcdhttp]
(http://godoc.org/github.com/coreos/etcd/etcdserver/etcdhttp)

* client.go

If for some reason you want to replace the store package then you would need to find some place to store the members belonging to the cluster configuration information.
