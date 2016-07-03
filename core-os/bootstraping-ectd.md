# Bootstrap the right way
- static
- discovery
- dns

## etcd bootstrap
- Only used during initial setup
- Setups up initial state
- rebooting the cluster does not bootstrap again
- reconfigure the cluster at runtime using members API

### Static Bootstrap
- Explicit config
- Need to know the host IPs upfront

### Discovery Bootstrap
- uses the etcd discovery protocol
- public service available
- self hosted option possible
- explicit about the cluster size (NEED TO DEFINE CLUSTER SIZE)

### DNS Bootstrap
- use dns srv to discover other etcd members
- uses dns names
- srv -> record type
- Need to be explicit about cluster SIZE
- used both dns and A records



PS: [Cluster Architecture](https://coreos.com/os/docs/latest/cluster-architectures.html)
