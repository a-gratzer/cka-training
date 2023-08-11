```yaml
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleep-pod
spec:
  containers:
    - name: ubuntu-sleeper
      image: ubuntu-sleeper
      command: ["sleep2.0"] #ENTRYPOINT override
      args: ["10"] #CMD override
```