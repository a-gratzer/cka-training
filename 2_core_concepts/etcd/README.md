# GitHub
    https://github.com/etcd-io/etcd/

# Install
```terminal
ETCD_VER=v3.4.27

# choose either URL
GOOGLE_URL=https://storage.googleapis.com/etcd
GITHUB_URL=https://github.com/etcd-io/etcd/releases/download
DOWNLOAD_URL=${GOOGLE_URL}

rm -f /tmp/etcd-${ETCD_VER}-linux-amd64.tar.gz
rm -rf /tmp/etcd-download-test && mkdir -p /tmp/etcd-download-test

curl -L ${DOWNLOAD_URL}/${ETCD_VER}/etcd-${ETCD_VER}-linux-amd64.tar.gz -o /tmp/etcd-${ETCD_VER}-linux-amd64.tar.gz
tar xzvf /tmp/etcd-${ETCD_VER}-linux-amd64.tar.gz -C /tmp/etcd-download-test --strip-components=1
rm -f /tmp/etcd-${ETCD_VER}-linux-amd64.tar.gz

/tmp/etcd-download-test/etcd --version
/tmp/etcd-download-test/etcdctl version
```

# Start
    /tmp/etcd-download-test/etcd

# Interact
    # write,read to etcd
    /tmp/etcd-download-test/etcdctl --endpoints=localhost:2379 put foo bar
    /tmp/etcd-download-test/etcdctl --endpoints=localhost:2379 get foo

# Create Snapshot
```
/tmp/etcd-download-test/etcdctl --endpoints=localhost:2379 snapshot save /tmp/etcd-download-test/snapshot-1
 
# output:
ag@fedora-ag  ~/workspace/git/private/udemy/cka  /tmp/etcd-download-test/etcdctl --endpoints=localhost:2379 snapshot save /tmp/etcd-download-test/snapshot-1
{"level":"info","ts":1690026966.6654613,"caller":"snapshot/v3_snapshot.go:119","msg":"created temporary db file","path":"/tmp/etcd-download-test/snapshot-1.part"}
{"level":"info","ts":"2023-07-22T13:56:06.66634+0200","caller":"clientv3/maintenance.go:200","msg":"opened snapshot stream; downloading"}
{"level":"info","ts":1690026966.6663682,"caller":"snapshot/v3_snapshot.go:127","msg":"fetching snapshot","endpoint":"localhost:2379"}
{"level":"info","ts":"2023-07-22T13:56:06.667079+0200","caller":"clientv3/maintenance.go:208","msg":"completed snapshot read; closing"}
{"level":"info","ts":1690026966.6671815,"caller":"snapshot/v3_snapshot.go:142","msg":"fetched snapshot","endpoint":"localhost:2379","size":"20 kB","took":0.001687378}
{"level":"info","ts":1690026966.6673543,"caller":"snapshot/v3_snapshot.go:152","msg":"saved","path":"/tmp/etcd-download-test/snapshot-1"}
Snapshot saved at /tmp/etcd-download-test/snapshot-1

# status of snapshot
/tmp/etcd-download-test/etcdctl --endpoints=localhost:2379 snapshot status /tmp/etcd-download-test/snapshot-1
# output:
baf8d97b, 2, 5, 20 kB
```

# Restore Snapshot
```
/tmp/etcd-download-test/etcdctl --endpoints=localhost:2379 snapshot restore /tmp/etcd-download-test/snapshot-1
# output:
{"level":"info","ts":1690027176.007679,"caller":"snapshot/v3_snapshot.go:306","msg":"restoring snapshot","path":"/tmp/etcd-download-test/snapshot-1","wal-dir":"default.etcd/member/wal","data-dir":"default.etcd","snap-dir":"default.etcd/member/snap"}
{"level":"info","ts":1690027176.0174778,"caller":"membership/cluster.go:392","msg":"added member","cluster-id":"cdf818194e3a8c32","local-member-id":"0","added-peer-id":"8e9e05c52164694d","added-peer-peer-urls":["http://localhost:2380"]}
{"level":"info","ts":1690027176.0221968,"caller":"snapshot/v3_snapshot.go:326","msg":"restored snapshot","path":"/tmp/etcd-download-test/snapshot-1","wal-dir":"default.etcd/member/wal","data-dir":"default.etcd","snap-dir":"default.etcd/member/snap"}

# check:
/tmp/etcd-download-test/etcdctl --endpoints=localhost:2379 get foo
foo
bar

```

# Health
    /tmp/etcd-download-test/etcdctl --endpoints=localhost:2379 endpoint health

# Run on cluster with certs
```
kubectl exec etcd-master -n kube-system -- sh -c "ETCDCTL_API=3 etcdctl get / --prefix --keys-only --limit=10 --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt  --key /etc/kubernetes/pki/etcd/server.key" 
```
