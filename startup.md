
```
./bin/etcd -peer-heartbeat-interval=100 -peer-election-timeout=500
./bin/etcd -peer-heartbeat-interval=5000 -peer-election-timeout=25000
```

The default **snapshot-count** is 10,000, to change it to something else pass
in this flag when starting etcd.

```
./bin/etcd -snapshot-count=500
```

This is the default

```
./bin/etcd -heartbeat-interval=100
```

To change it to something else do this:

```
./bin/etcd -heartbeat-interval=200
```

The snapshot-count and the election-timeout automatically gets calculated from the heartbeat-interval.
Both parameters are shown when etcd starts up so be sure and check them if you are curious.

For more details go to the doc on
[tuning]
(https://github.com/coreos/etcd/blob/master/Documentation/tuning.md).
