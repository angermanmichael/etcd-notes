
* package: etcdmain, file: etcd.go, method: startetcd

All of the transport related functionality gets fired off early in the startup phase.
In the method **startetcd** the following two methods get called in order:

* package: transport, file: timeout_transport, method: NewTimeoutTransport
* package: transport, file: timeout_listener,  method: NewTimeoutListener

It is responsible for calling **NewTransport** and **NewListener** located here:

* package: transport, file: listener, method NewTransport
* package: transport, file: listener, method NewListener
