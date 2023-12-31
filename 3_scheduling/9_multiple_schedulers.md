https://kubernetes.io/docs/reference/scheduling/config/
https://jamesdefabia.github.io/docs/admin/multiple-schedulers/

```yaml
apiVersion: kubescheduler.config.k8s.io/v1
kind: KubeSchedulerConfiguration
profiles:
  - schedulerName: my-custom-scheduler

leaderElection:
  leaderElect: true
  resourceNamespace: kube-system
  resourceName: lock-object-my-scheduler
```

## Deploy on host

```yaml
wget ...
ExecStart=/usr/local/bin/kube-scheduler -config=/etc/kubernetes/config/my-scheduler-config.yaml
```

## Deploy Scheduler as deployment

```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  labels:
    component: scheduler
    tier: control-plane
  name: my-custom-scheduler
  namespace: kube-system
spec:
  replicas: 1
  template:
    metadata:
      labels: 
        component: scheduler
        tier: control-plane
        version: second
    spec:
      containers:
      - command: 
        - /usr/local/bin/kube-scheduler 
        - --address=0.0.0.0
        - --scheduler-name=my-scheduler
        - --config=/etc/kubernetes/my-scheduler-config.yaml
        image: gcr.io/my-gcp-project/my-kube-scheduler:1.0
        livenessProbe:
          httpGet:
            path: /healthz
            port: 10251
          initialDelaySeconds: 15
        name: my-custom-scheduler
        readinessProbe:
          httpGet:
            path: /healthz
            port: 10251
        resources:
          requests:
            cpu: '0.1'
        securityContext:
          privileged: false
        volumeMounts: []
      hostNetwork: false
      hostPID: false
      volumes: []
```

## How to use the custom scheduler?

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
  schedulerName: my-custom-scheduler
```


## Which scheduler picked up a pod?

```yaml
k get events -o wide
k logs my-custom-scheduler --name-space=kube-system
```