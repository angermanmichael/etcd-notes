
* package: etcdmain, file: etcd, method: startetcd

All of the transport related functionality gets fired off early in the startup phase.
In the method **startetcd** the following 3 methods get called in order:

* package: transport, file: timeout_transport, method: NewTimeoutTransport
* package: transport, file: timeout_listener,  method: NewTimeoutListener
* package: transport, file: keepalive_listener, method: NewKeepAliveListener

It is responsible for calling **NewTransport** and **NewListener** located here:

* package: transport, file: listener, method NewTransport
* package: transport, file: listener, method NewListener

Transport Layer Security (TLS) is by default set to Empty as noted here
in *listener.go* in the transport package.

```
func (info TLSInfo) Empty() bool {
	return info.CertFile == "" && info.KeyFile == ""
}
```

All of the **transport** and **listener** setup is to drive the starting of the clients
that talk to the **etcdserver**.  

So first the server starts and then all of the subsequent clients start there after.

* package: etcdmain, file: etcd, method: startEtcd

**startEtcd** launches the etcd server and HTTP handlers for client/server communication.
