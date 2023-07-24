# crictl
Inspect/interact with container-runtimes connected via CRI (Contianer Runtime Interface)
https://github.com/kubernetes-sigs/cri-tools/blob/master/docs/crictl.md

## Install
```terminal
VERSION="v1.27.1"
wget https://github.com/kubernetes-sigs/cri-tools/releases/download/$VERSION/crictl-$VERSION-linux-amd64.tar.gz
sudo tar zxvf crictl-$VERSION-linux-amd64.tar.gz -C /usr/local/bin
rm -f crictl-$VERSION-linux-amd64.tar.gz
```

### Inspect
    crictl [global options] command [command options] [arguments...]
    crictl -h

#### k3s
    k3s crictl ps -a
