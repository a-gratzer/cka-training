# Backup

For all following command we also have to add information about endpoints and certificates.

```yaml
ETCDCTL_API=3 etcdctl \
  snapshot save snapshot.db \
  --endpoints=https://...
  --cacert=...
  --cert=...
  --key=...
```

## Via Kube-Api
> k get all --all-namespaces -o yaml > all-deploy-services.yaml
 
## ETCD
> ETCDCTL_API=3 etcdctl snapshot save snapshot.db
> ETCDCTL_API=3 etcdctl snapshot status snapshot.db

### Restore
> service kube-apiserver stop
> ETCDCTL_API=3 etcdctl restore snapshot.db --data-dir ...
> etcd.service: update --data-dir path
> systemctl daemon-reload
> service etcd restart
> service kube-apiserver start
