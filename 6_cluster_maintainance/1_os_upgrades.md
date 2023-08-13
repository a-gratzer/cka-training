# Drain - Cordon - Uncordon
Node down for more than 5mins then the PODs will be terminated.

> Time till termination is the POD eviction time. <br>
> kube-controller-manager --pod-eviction-timeout=5ms0s

If a node will be shut down for more than 5mins all the running pods on it will be scheduled to other node.
If they are not part of a e.g. replica-set, they will just be terminated.

Better approach is to reschedule the pods before the update.

> k drain node-1
> k drain node-1 --ignore-daemonsets

<b>Drain</b> will also <b>cordon</b> the node, which means that nothing will be scheduled on it anymore.
After the update the node has to be <b>uncordon</b>ed.
>k uncordon node-1

<b>Cordon</b> can be called as well. This does not terminate or move running PODs on a node.
It just ensures that nothing new gets scheduled on it anymore.
> k cordon node-1