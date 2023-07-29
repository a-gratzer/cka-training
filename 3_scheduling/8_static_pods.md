# Static Pods
Can be seen as definition of a Control-plane on nodes.

## Pod Manifest Path
Kubelet parameter:

    --pod-manifest-path=/etc/kubernetes/manifests

The kubelet periodically checks this folder.

## Config param

The path can also be defined inside the kubeconfig.yaml

    --config=kubeconfig.yaml

kubeconfig.yaml
```yaml
...
staticPodPath: /etc/kubernetes/manifests
...
```