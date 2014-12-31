

These files and function references all live in the **wal** package.

### wal.go ###


##### Writing data to disk #####

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
