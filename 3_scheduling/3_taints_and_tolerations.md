https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/

Define which node will accept a specific pod.
It might be that the pod will be scheduled on another node...

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