```terminal
k get namespaces
k get ns
# set default
kubectl config set-context --current --namespace=<insert-namespace-name-here>

# access service from different namespace
SERVICE-NAME.NAMESPACE.svc.cluster.local

k create ns dev
k create -f pod.yaml --namespace=dev
k get po -n dev 

k create -f pod_with_ns.yaml 
k get po -n dev

k create -f ns_dev_quota.yaml

k get pods --all-namespaces
k get pods -A
```