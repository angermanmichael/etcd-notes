

THIS CODE AND DESCRIPTION NEEDS TO BE UPDATED !!


### Storage ###

I hope this helps clarify some of the different terms that are used for storing
information in etcd.

In the **etcdserver** package in the file **storage.go** there is a storage type that talks to the WAL (write ahead log) and the Snapshotter

```
type storage struct {
  *wal.WAL
  *snap.Snapshotter
}
```

storage is used in the

```
type Storage interface {
  // Save function saves ents and state to the underlying stable storage.
  // Save MUST block until st and ents are on stable storage.
  Save(st raftpb.HardState, ents []raftpb.Entry) error
  // SaveSnap function saves snapshot to the underlying stable storage.
  SaveSnap(snap raftpb.Snapshot) error

  // TODO: WAL should be able to control cut itself. After implement self-controlled cut,
  // remove it in this interface.
  // Cut cuts out a new wal file for saving new state and entries.
  Cut() error
  // Close closes the Storage and performs finalization.
  Close() error
}
```


##### Writing data to disk #####


In etcdserver/server.go :

```
func (s *EtcdServer) run() {
```

There is an infinite for loop whose job it is to do multiple things including
writing data out to the wal file every n ticks.  It is inside this case
statement

```
case rd := <-s.node.Ready():
```

where this method gets called

```
if err := s.storage.Save(rd.HardState, rd.Entries); err != nil {
  log.Fatalf("etcdserver: save state and entries error: %v", err)
}
```

These files and function references all live in the **wal** package,
unless specifically stated that they live somewhere else.

### wal.go ###

The Save function writes the data to the wal file via sync below.

This is where the data actually gets written to the wal file.

```
func (w *WAL) sync() error {
	if w.encoder != nil {
		if err := w.encoder.flush(); err != nil {
			return err
		}
	}
	start := time.Now()
	err := w.f.Sync()
	syncDurations.Observe(float64(time.Since(start).Nanoseconds() / int64(time.Microsecond)))
	return err
}
```

All of this is possible because of this:
[http://golang.org/pkg/os/#File.Sync]
(http://golang.org/pkg/os/#File.Sync)
