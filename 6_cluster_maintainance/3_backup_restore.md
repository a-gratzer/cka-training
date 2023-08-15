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

## Working with ETCDCTL
https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/learn/lecture/18478622#overview

etcdctl is a command line client for etcd.



In all our Kubernetes Hands-on labs, the ETCD key-value database is deployed as a static pod on the master. The version used is v3.

To make use of etcdctl for tasks such as back up and restore, make sure that you set the ETCDCTL_API to 3.



You can do this by exporting the variable ETCDCTL_API prior to using the etcdctl client. This can be done as follows:

export ETCDCTL_API=3

On the Master Node:





To see all the options for a specific sub-command, make use of the -h or --help flag.



For example, if you want to take a snapshot of etcd, use:

etcdctl snapshot save -h and keep a note of the mandatory global options.



Since our ETCD database is TLS-Enabled, the following options are mandatory:

--cacert                                                verify certificates of TLS-enabled secure servers using this CA bundle

--cert                                                    identify secure client using this TLS certificate file

--endpoints=[127.0.0.1:2379]          This is the default as ETCD is running on master node and exposed on localhost 2379.

--key                                                      identify secure client using this TLS key file





Similarly use the help option for snapshot restore to see all available options for restoring the backup.

etcdctl snapshot restore -h

For a detailed explanation on how to make use of the etcdctl command line tool and work with the -h flags, check out the solution video for the Backup and Restore Lab.

# Restore

First Restore the snapshot:

root@controlplane:~# ETCDCTL_API=3 etcdctl  --data-dir /var/lib/etcd-from-backup \
snapshot restore /opt/snapshot-pre-boot.db


2022-03-25 09:19:27.175043 I | mvcc: restore compact to 2552
2022-03-25 09:19:27.266709 I | etcdserver/membership: added member 8e9e05c52164694d [http://localhost:2380] to cluster cdf818194e3a8c32
root@controlplane:~#


Note: In this case, we are restoring the snapshot to a different directory but in the same server where we took the backup (the controlplane node) As a result, the only required option for the restore command is the --data-dir.



Next, update the /etc/kubernetes/manifests/etcd.yaml:

We have now restored the etcd snapshot to a new path on the controlplane - /var/lib/etcd-from-backup, so, the only change to be made in the YAML file, is to change the hostPath for the volume called etcd-data from old directory (/var/lib/etcd) to the new directory (/var/lib/etcd-from-backup).

volumes:
- hostPath:
  path: /var/lib/etcd-from-backup
  type: DirectoryOrCreate
  name: etcd-data
  With this change, /var/lib/etcd on the container points to /var/lib/etcd-from-backup on the controlplane (which is what we want).

When this file is updated, the ETCD pod is automatically re-created as this is a static pod placed under the /etc/kubernetes/manifests directory.



Note 1: As the ETCD pod has changed it will automatically restart, and also kube-controller-manager and kube-scheduler. Wait 1-2 to mins for this pods to restart. You can run the command: watch "crictl ps | grep etcd" to see when the ETCD pod is restarted.

Note 2: If the etcd pod is not getting Ready 1/1, then restart it by kubectl delete pod -n kube-system etcd-controlplane and wait 1 minute.

Note 3: This is the simplest way to make sure that ETCD uses the restored data after the ETCD pod is recreated. You don't have to change anything else.



If you do change --data-dir to /var/lib/etcd-from-backup in the ETCD YAML file, make sure that the volumeMounts for etcd-data is updated as well, with the mountPath pointing to /var/lib/etcd-from-backup (THIS COMPLETE STEP IS OPTIONAL AND NEED NOT BE DONE FOR COMPLETING THE RESTORE)

# Task notes

1. Get etcdctl utility if it's not already present.
   go get github.com/coreos/etcd/etcdctl

2. Backup
   ETCDCTL_API=3 etcdctl --endpoints=https://[127.0.0.1]:2379 --cacert=/etc/kubernetes/pki/etcd/ca.crt \
   --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key \
   snapshot save /opt/snapshot-pre-boot.db

          -----------------------------

          Disaster Happens

          -----------------------------
3. Restore ETCD Snapshot to a new folder
   ETCDCTL_API=3 etcdctl --endpoints=https://[127.0.0.1]:2379 --cacert=/etc/kubernetes/pki/etcd/ca.crt \
   --name=master \
   --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key \
   --data-dir /var/lib/etcd-from-backup \
   --initial-cluster=master=https://127.0.0.1:2380 \
   --initial-cluster-token etcd-cluster-1 \
   --initial-advertise-peer-urls=https://127.0.0.1:2380 \
   snapshot restore /opt/snapshot-pre-boot.db

4. Modify /etc/kubernetes/manifests/etcd.yaml
   Update --data-dir to use new target location
   --data-dir=/var/lib/etcd-from-backup

Update new initial-cluster-token to specify new cluster
--initial-cluster-token=etcd-cluster-1

Update volumes and volume mounts to point to new path
volumeMounts:
- mountPath: /var/lib/etcd-from-backup
name: etcd-data
- mountPath: /etc/kubernetes/pki/etcd
name: etcd-certs
hostNetwork: true
priorityClassName: system-cluster-critical
volumes:
- hostPath:
  path: /var/lib/etcd-from-backup
  type: DirectoryOrCreate
  name: etcd-data
- hostPath:
  path: /etc/kubernetes/pki/etcd
  type: DirectoryOrCreate
  name: etcd-certs
