
#### package rafthttp

#### s.r.transport

*	Handler
*	Send
*	AddPeer
*	RemovePeer
*	RemoveAllPeers
*	UpdatePeer
*	Stop

#### Hierarchy

Filename      | Method
------------- | -------
transport.go  | AddPeer
peer.go       | startPeer
stream.go     | startStreamReader, startStreamWriter




the **message** and **msgapp** {encoder, decoder} pairs are both referenced
in the file stream.go in the methods:

**startStreamReader** and **startStreamWriter** which both get fired off in **startPeer**
defined in *peer.go*

which gets fired off in the **AddPeer** method located in *transport.go*

The **AddPeer** method is called throughout *server.go*

where the method *rafthttp.NewTransporter* kicks everything off as defined in *transport.go*

By looking at the code below one can see:

server.go sends messages to the peers via the transport after appending messages
to the log.

The rafthttp.transport sends the message via the rafthttp.peer

As defined in rafthttp/peer.go

```
func (p *peer) Send(m raftpb.Message) {
	select {
	case p.sendc <- m:
```

As defined in server.go

```
s.r.raftStorage.Append(rd.Entries)
s.send(rd.Messages)

...

As shown in the actual send method in server.go
s.r.transport.Send(ms)
```

for further details go
[here](http://godoc.org/github.com/coreos/etcd/rafthttp)
to see the GoDoc on package rafthttp


From the file peer.go

Peer is the representative of a remote raft node. Local raft node sends messages to the remote through peer.
Each peer has two underlying mechanisms to send out a message: stream and pipeline.
A stream is a receiver initialized long-polling connection, which is always open to transfer messages.
Besides general stream, peer also has a optimized stream for sending msgApp since msgApp accounts for large part
of all messages. Only raft leader uses the optimized stream to send msgApp to the remote follower node.
A pipeline is a series of http clients that send http requests to the remote.
It is only used when the stream has not been established.

In transport.go is defined

```
func (t *transport) Handler() http.Handler {
	pipelineHandler := NewHandler(t.raft, t.clusterID)
	streamHandler := newStreamHandler(t, t.id, t.clusterID)
	mux := http.NewServeMux()
	mux.Handle(RaftPrefix, pipelineHandler)
	mux.Handle(RaftStreamPrefix+"/", streamHandler)
	return mux
}
```

In server.go this is how the above Handler gets called:

```
func (s *EtcdServer) RaftHandler() http.Handler { return s.r.transport.Handler() }
```

which is then turned around and called in etcdmain/etcd.go

```
ph := etcdhttp.NewPeerHandler(s.Cluster, etcdserver.RaftTimer(s), s.RaftHandler())
// Start the peer server in a goroutine
for _, l := range plns {
	go func(l net.Listener) {
		log.Fatal(serveHTTP(l, ph, 5*time.Minute))
	}(l)
}
```

#### Test Table

First Header  | Second Header
------------- | -------------
Content Cell  | Content Cell
Content Cell  | Content Cell

#### Testing Notes

The following other test files need to be in place to run **stream_test.go**

* functional_test.go
* pipeline_test.go
* stream_test.go
* urlpick_test.go
