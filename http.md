
There are three
[ServeMux's](http://golang.org/pkg/net/http/#ServeMux) in etcd.

* *rafthttp.transport*
* *etcdhttp.client*
* *etcdhttp.peer*

#### package: rafthttp

Located in *transport.go*

* NewTransporter
* NewHandler
* NewStreamHandler

Everything gets bootstrapped in etcdserver by the call to **NewTransporter**
