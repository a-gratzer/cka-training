https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/
```
"metadata": {
  "labels": {
    "key1" : "value1",
    "key2" : "value2"
  }
}
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

```

    k get po --selector app=App1
    # multi select
    k get po --selector env=prod,bu=finance,tier=frontend