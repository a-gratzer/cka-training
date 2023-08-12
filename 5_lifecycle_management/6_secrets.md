https://kubernetes.io/docs/concepts/configuration/secret/

## Create

### Imperative

    k create secret generic
        <secret-name> --from-literal=<key>=<value>
        app-secret --from-literal=DB_HOST=mysql

### Declarative
    k create -f ...

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: dotfile-secret
data:
  .secret-file: dmFsdWUtMg0KDQo=
---
apiVersion: v1
kind: Pod
metadata:
  name: secret-dotfiles-pod
spec:
  volumes:
    - name: secret-volume
      secret:
        secretName: dotfile-secret
  containers:
    - name: dotfile-test-container
      image: registry.k8s.io/busybox
      command:
        - ls
        - "-l"
        - "/etc/secret-volume"
      volumeMounts:
        - name: secret-volume
          readOnly: true
          mountPath: "/etc/secret-volume"

---
apiVersion: v1
kind: Pod
metadata:
  name: secret-dotfiles-pod
spec:
  containers:
    - name: dotfile-test-container
      image: registry.k8s.io/busybox
      envFrom:
        - secretRef:
          name: app-secret
```

#### Encode
    echo -n "test" | base64
    dGVzdA==

#### Decode
    echo -n "dGVzdA==" | base64 --decode
    test

## Get

    k get secrets
    k describe secrets