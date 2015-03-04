
#### package rafthttp

the **message** and **msgapp** {encoder, decoder} pairs are both referenced
in the file stream.go in the methods:

**startStreamReader** and **startStreamWriter** which both get fired off in **startPeer**
defined in *peer.go*

which gets fired off in the **AddPeer** method located in *transport.go*

The **AddPeer** method is called throughout *server.go*

where the method *rafthttp.NewTransporter* kicks everything off.

for further details go
[here](http://godoc.org/github.com/coreos/etcd/rafthttp)
to see the GoDoc on package rafthttp


From the file peer.go
```
Peer is the representative of a remote raft node. Local raft node sends messages to the remote through peer.
Each peer has two underlying mechanisms to send out a message: stream and pipeline.
A stream is a receiver initialized long-polling connection, which is always open to transfer messages.
Besides general stream, peer also has a optimized stream for sending msgApp since msgApp accounts for large part
of all messages. Only raft leader uses the optimized stream to send msgApp to the remote follower node.
A pipeline is a series of http clients that send http requests to the remote.
It is only used when the stream has not been established.
```
