
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

Everything gets bootstrapped in *server.go* by the call to **NewTransporter**,
and then the Transporter interface method calls get invoked in *server.go* via **s.r.transport**.

CONTINUE FROM HERE:

```
etcdmain/etcd.go:	ph := etcdhttp.NewPeerHandler(s.Cluster, s.RaftHandler())

etcdserver/server.go:func (s *EtcdServer) RaftHandler() http.Handler { return s.r.transport.Handler() }
```
