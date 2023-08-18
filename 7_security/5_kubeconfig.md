
## API Requests
```yaml
curl https://...:6443/api/v1/pods \
  --key admin.key
  --cert admin.crt
  --cacert ca.crt
```
## CLI
```yaml
kubectl get pods 
  --server my-kube-playground:6443
  --client-certificate admin.crt
  --client-key admin.key
  --certificate-authority ca.crt
```
## CLI with kubeconfig
>kubectl get pods --kubeconfig config

The parameter <b>--kubeconfig<b> has not to be provided if the config file is under <b>$HOME/.kube/config</b>.

## Kubeconfig
### Structure
- Clusters
  - e.g. dev, prod, google
- Contexts
  - e.g. admin@prod, dev@google
- Users
  - e.g. admin, dev-user, prod-user

### Example
```yaml
apiVersion: v1
kind: Config
current-context: my-kube-admin@my-kube-playground

clusters:
- name: my-kube-playground
  cluster:
    certificate-authority: ca.crt
    server: https://my-kube-playground:6443

users:
- name: my-kube-admin
  user:
    client-certificate: admin.crt
    client-key: admin.key

contexts:
- name: my-kube-admin@my-kube-playground
  context:
    cluster: my-kube-playground
    user: my-kube-admin
- name: my-kube-admin@my-kube-playground-prod
  context:
    cluster: my-kube-playground
    user: my-kube-admin
    namespace: prod
- name: my-kube-admin@my-kube-playground-dev
  context:
    cluster: my-kube-playground
    user: my-kube-admin
    namespace: dev
```

### Usage
1. set <b>current-context</b> in kubeconfig.
2. update <b>current-context</b> via cli
> k config use-context prod-user@production 

Inspect kubeconfig
> k config view --kubeconfig=my-custom-config