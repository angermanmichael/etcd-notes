
This is one of the most confusing aspects of the system for me.

Basically how the data moves from MemoryStorage to the WAL file.

It happens here in *server.go*

```
if err := s.r.storage.Save(rd.HardState, rd.Entries); err != nil {
  log.Fatalf("etcdserver: save state and entries error: %v", err)
}
```

**s.r.storage** gets created in the file *etcdserver/storage.go* via
this command.

```
func NewStorage(w *wal.WAL, s *snap.Snapshotter) Storage
```

data moves into **rd.HardState** and **rd.Entries** from the MemoryStorage
in the raft package and NOT in the etcdserver package (I think)
