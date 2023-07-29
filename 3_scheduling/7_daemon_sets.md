https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/
  
  A DaemonSet ensures that all (or some) Nodes run a copy of a Pod. As nodes are added to the cluster, Pods are added to them. As nodes are removed from the cluster, those Pods are garbage collected. Deleting a DaemonSet will clean up the Pods it created.

Some typical uses of a DaemonSet are:

  running a cluster storage daemon on every node
  running a logs collection daemon on every node
  running a node monitoring daemon on every node
  In a simple case, one DaemonSet, covering all nodes, would be used for each type of daemon. A more complex setup might use multiple DaemonSets for a single type of daemon, but with different flags and/or different memory and cpu requests for different hardware types.

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd-elasticsearch
  namespace: kube-system
  labels:
    k8s-app: fluentd-logging
spec:
  selector:
    matchLabels:
      name: fluentd-elasticsearch
  template:
    metadata:
      labels:
        name: fluentd-elasticsearch
...
```
```yaml
  k get daemonsets -A
  k describe daemonsets fluentd-logging
```

## Questions
### How many ds are created in the cluster in all namespaces?
```yaml
  k get daemonsets -A
```