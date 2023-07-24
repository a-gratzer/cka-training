https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/

- NoSchedule
- PreferNoSchedule
- NoExecute

```
k taint nodes node1 app=blue:NoSchedule
```

```
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx
    app: App1
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
  tolerations:
  - key: "app"
    operator: "Equal"
    value: "blue"
    effect: "NoSchedule"
```