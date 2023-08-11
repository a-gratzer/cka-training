```yaml
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleep-pod
spec:
  containers:
    - name: ubuntu-sleeper
      image: ubuntu-sleeper
      args: ["10"]
```