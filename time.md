
To slow everything down dramatically 3 parameters need to change

Add 1 zero or 2 zeros to all of the parameters

* // TODO: calculate based on heartbeat interval
* defaultPublishRetryInterval = 500 * time.Second
*
* Ticker and SyncTicker in EtcdServer below

```
srv := &EtcdServer{
  cfg:         cfg,
  store:       st,
  node:        n,
  raftStorage: s,
  id:          id,
  attributes:  Attributes{Name: cfg.Name, ClientURLs: cfg.ClientURLs.StringSlice()},
  Cluster:     cfg.Cluster,
  storage:     NewStorage(w, ss),
  stats:       sstats,
  lstats:      lstats,
  Ticker:      time.Tick(10000 * time.Millisecond),
  SyncTicker:  time.Tick(50000 * time.Millisecond),
  snapCount:   cfg.SnapCount,
  reqIDGen:    idutil.NewGenerator(uint8(id), time.Now()),
}
```
