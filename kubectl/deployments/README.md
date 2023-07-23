# Deployments

https://kubernetes.io/docs/concepts/workloads/controllers/deployment/

```terminal
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > deployment-nginx.yaml
k create -f deployment-nginx.yaml

k get all
```