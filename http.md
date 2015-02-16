
There are three
[ServeMux's](http://golang.org/pkg/net/http/#ServeMux) in etcd.

* *rafthttp.transport*
* *etcdhttp.client*
* *etcdhttp.peer*

#### package: rafthttp

There are two core interfaces in *transport.go*

```
type Raft interface {
  Process(ctx context.Context, m raftpb.Message) error
}

type Transporter interface {
  Handler() http.Handler
  Send(m []raftpb.Message)
  AddPeer(id types.ID, urls []string)
  RemovePeer(id types.ID)
  UpdatePeer(id types.ID, urls []string)
  Stop()
}

```

Located in *transport.go*

* NewTransporter
* NewHandler
* NewStreamHandler

Everything gets bootstrapped in etcdserver by the call to **NewTransporter**
