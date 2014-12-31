

These files and function references all live in the **wal** package.

#### wal.go ####

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
