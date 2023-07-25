
```
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx
    app: App1
  name: nginx
spec:
  affinity:
    nodeAffinity:
        requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
            - key: size
              operator: In
              values:
              - Large
              - Medium
```
