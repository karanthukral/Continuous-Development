# etcd 2.0

## What is etcd?
- distributed key-value store for config storage
- Cluster has one leader and the rest are followers
- The etcd hosts are surrounded by proxies which the application can talk to - removes any host dependancy
- **availability** -> design for host failure
- Also available if the leader fails -> does leader election
- Recommended 5 host cluster
  - you can expect 1 failure
  - you can still have another unexpected failure
  - once the majority fails, the cluster becomes unavailable
- super useful for schedulers
- **vulcan**, **confd** projects also use etcd

## What does etcd 2 have?
- new backup and restore system
  - can backup snapshots while cluster is running
- dns cluster bring up
  - another way to bootstrap the cluster
- cluster config tools added to ctl (cli)
- proxy mode -> replaces standby mode
  - you can set the machine as a proxy so it does not need to be part of the core cluster and the consensus alg.
