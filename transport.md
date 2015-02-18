

#### NewTimeoutTransport

This is where all of the transport related functionality starts out.

It gets fired off early in the startup phase.

It is responsible for calling **NewTransport**

* package: etcdmain
* file: etcd.go
* method: startetcd

#####pkg/transport/listener.go

On startup of etcd the following 3 transport methods get called in this order:

```
func NewTransport(info TLSInfo) (*http.Transport, error) {

func (info TLSInfo) ClientConfig() (cfg *tls.Config, err error) {

func NewListener(addr string, scheme string, info TLSInfo) (net.Listener, error) {
```
