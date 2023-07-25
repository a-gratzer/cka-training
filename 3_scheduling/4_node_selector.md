# Node selector

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
  nodeSelector:
    size: Large # label needed on node
```

    k label nodes node01 size=Large