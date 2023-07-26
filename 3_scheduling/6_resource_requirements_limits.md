Kube-Scheduler identifies the best node to schedule a deployment on, 
based on:
- available CPU and mem
- needed CPU and mem

## Requirements per POD

### Requests
Minimum required amount of CPU/Memory for a container.
Those numbers are used by the kube-scheduler to identify the best node.

```yaml
spec:
  container:
    resources:
      requests:
        memory: "200Mi"
        cpu: "700m"
```

### Limits 
Without limit a pod can consume up to 100% of the native resources.
```yaml
spec:
  container:
    resources:
      limits:
        memory: "200Mi"
        cpu: "700m"
```

### CPU
1 CPU is
- 1 AWS vCPU, 
- 1 GPC Core,
- 1 Azure Core or
- 1 Hyperthread

<b>1</b> cpu can be represented as <b>1000m</b>.
1m lowest.

If a pod consumes more than the defined limit then it will be <b>throttled</br>.
On the other hand a container can consume more memory then defined in the limit. 
If this happens to often the POD will be TERMINATED with an OOM error.

Default: No limits.

- No requests, No Limits
  - One POD can consume all resources which will have a negative effect on other pods.
- No requests, but limits
  - K8s sets requests = limits
- Requests and limits set
  - pod 1 will be throttled, even if there are resources left and pod 2 does not use them.
- Requests set but NO limits
  - Might be the best solution

### Memory
e.g.:
- 256m
- 256mi

1G (Gigabyte) is 1.000.000.000 bytes </br>
1M (Megabyte) is 1.000.000 bytes </br>
1K (Kilobyte) is 1.000 bytes </br>

1 Gi (Gibibyte) is 1024*1024*1024 bytes </br>
1 Gi (Mebibyte) is 1024*1024 bytes </br>
1 Gi (Kibibyte) is 1024 bytes


### LimitRanges

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-resource-constraint
spec:
  limits:
  - default: # this section defines default limits
      cpu: 500m
    defaultRequest: # this section defines default requests
      cpu: 500m
    max: # max and min define the limit range
      cpu: "1"
    min:
      cpu: 100m
    type: Container
```

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: memory-resource-constraint
spec:
  limits:
  - default: # this section defines default limits
      memory: 500m
    defaultRequest: # this section defines default requests
      memory: 500m
    max: # max and min define the limit range
      memory: "1"
    min:
      memory: 100m
    type: Container
```

### Quotas

```yaml
apiVersion: v1
kind: List
items:
- apiVersion: v1
  kind: ResourceQuota
  metadata:
    name: pods-high
  spec:
    hard:
      cpu: "1000"
      memory: 200Gi
      pods: "10"
    scopeSelector:
      matchExpressions:
      - operator : In
        scopeName: PriorityClass
        values: ["high"]
- apiVersion: v1
  kind: ResourceQuota
  metadata:
    name: pods-medium
  spec:
    hard:
      cpu: "10"
      memory: 20Gi
      pods: "10"
    scopeSelector:
      matchExpressions:
      - operator : In
        scopeName: PriorityClass
        values: ["medium"]
- apiVersion: v1
  kind: ResourceQuota
  metadata:
    name: pods-low
  spec:
    hard:
      cpu: "5"
      memory: 10Gi
      pods: "10"
    scopeSelector:
      matchExpressions:
      - operator : In
        scopeName: PriorityClass
        values: ["low"]
```
  kubectl describe quota

```yaml
# Pod
apiVersion: v1
kind: Pod
metadata:
  name: high-priority
spec:
  containers:
    - name: high-priority
      image: ubuntu
      command: ["/bin/sh"]
      args: ["-c", "while true; do echo hello; sleep 10;done"]
      resources:
        requests:
          memory: "10Gi"
          cpu: "500m"
        limits:
          memory: "10Gi"
          cpu: "500m"
  priorityClassName: high
```