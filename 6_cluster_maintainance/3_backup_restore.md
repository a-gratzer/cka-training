# Backup

## Via Kube-Api
> k get all --all-namespaces -o yaml > all-deploy-services.yaml
 
## ETCD
> ETCDCTL_API=3 etcdctl snapshot save snapshot.db