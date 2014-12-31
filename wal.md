

These files and function references all live in the **wal** package,
unless specifically stated that they live somewhere else.

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

### wal.go ###

The sync function below is called from here

```
func (w *WAL) Save(st raftpb.HardState, ents []raftpb.Entry) error {
  if err := w.SaveState(&st); err != nil {
    return err
  }
  for i := range ents {
    if err := w.SaveEntry(&ents[i]); err != nil {
      return err
    }
  }
  return w.sync()
}
```

This is where the data actually gets written to the wal file.

```
func (w *WAL) sync() error {
  if w.encoder != nil {
    if err := w.encoder.flush(); err != nil {
      return err
    }
  }
  return w.f.Sync()
}
```

All of this is possible because of this:
[http://golang.org/pkg/os/#File.Sync]
(http://golang.org/pkg/os/#File.Sync)
