| Word | Context | Definition | Links |
|------|---------|------------|-------|
| OSD | Ceph, Rook | "the object storage daemon for the Ceph distributed file system. It is responsible for storing objects on a local file system and providing access to them over the network." | [Ceph Docs](https://docs.ceph.com/en/latest/man/8/ceph-osd/), [Rook Docs](https://rook.io/docs/rook/v1.5/ceph-osd-mgmt.html) |
| Mon | Ceph, Rook | "the cluster monitor daemon for the Ceph distributed file system. One or more instances of ceph-mon form a Paxos part-time parliament cluster that provides extremely reliable and durable storage of cluster membership, configuration, and state." | [Docs](https://docs.ceph.com/en/latest/man/8/ceph-mon/) |
| Ceph | Software | "Ceph replicates data and makes it fault-tolerant, using commodity hardware and requiring no specific hardware support. As a result of its design, the system is both self-healing and self-managing..." | [ceph.io](https://ceph.io/ceph-storage/), [Wikipedia](https://en.wikipedia.org/wiki/Ceph_(software)) |
| Rook | Software | A Kubernetes operator that creates and manages Ceph clusters. | [rook.io](https://rook.io/) |
| Kubernetes | Software | A container orchestration platform. | [Wikipedia](https://en.wikipedia.org/wiki/Kubernetes) |
| k3s | Kubernetes | A version of Kubernetes with reduced hardware requirements. | [k3s.io](https://k3s.io/) |
| Stretch Cluster | Ceph, Rook | A cluster where parts of the cluster are located in separate geographical locations. | [Example](https://github.com/rook/rook/blob/master/Documentation/ceph-cluster-crd.md#host-based-cluster), [Docs](https://docs.ceph.com/en/latest/rados/operations/stretch-mode/) |
| CRD | Kubernetes | Custom Resource Definition. "Custom resources are extensions of the Kubernetes API." | [Docs](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)|
| Rook Operator | Rook, Kubernetes | The Rook operator is the Kubernetes operator that performs the actual work of managing the Ceph cluster. | |
| Cluster CRD | Ceph, Rook, Kubernetes | The definition of the Ceph cluster. | [Rook Docs](https://rook.io/docs/rook/v1.5/ceph-cluster-crd.html) |