# Cluster upgrades
https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/learn/lecture/24458188#overview

No component of the controlplane should have a higher version than the <b>kube-apiserver</b>.

If kube-apiserver has version <u>X</u>, then all other control-plane services can have version <u>X-1</u>.</br>
kubelet and kube-proxy can even be 2 versions lower.

- kube-apiserver: X
- Controller-manager: X-1
- kube-scheduler: X-1
- kubelet: X-2
- kube-proxy: X-2

kubectl can also be 1 version ahead <u>X+1</u>.

Recommended: Upgrade always 1 version at a time.

> kubeadm upgrade plan<br>
> kubeadm upgrade apply

- First: Upgrade master nodes
- After that upgrade worker

Upgrade strategies for workers:
- one by one
- all at once
- add new node with higher version and remove node with lower version afterwards.

## kubeadm
Also follows versioning.
Upgrade kubeadm first to newer version
> apt-get upgrade -y kubeadm=1.12.0-00

Then upgrade the cluster
> kubeadm upgrade apply v1.12.0

After this has been done, login to the master-nodes and upgrade kubelets.
> apt-get upgrade -y kubelet=1.12.0.-00<br>
> systemctl restart kubelet

After that the workers can be upgraded.

Worker:
```terminal
apt-get upgrade -y kubeadm=1.12.0-00
apt-get upgrade -y kubelet=1.12.0-00
kubeadm upgrade node config --kubelet-version v1.12.0
systemctl restart kubelet
```