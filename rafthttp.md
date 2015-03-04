
#### package rafthttp

the **message** and **msgapp** {encoder, decoder} pairs are both referenced
in the file stream.go in the methods:

**startStreamReader** and **startStreamWriter** which both get fired off in **startPeer**.

which gets fired off in the **AddPeer** method located in *transport.go*

The **AddPeer** method is called throughout *server.go*

where the method *rafthttp.NewTransporter* kicks everything off.

see the GoDoc on package rafthttp
[here].
(http://godoc.org/github.com/coreos/etcd/rafthttp)
