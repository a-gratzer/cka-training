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

### CPU
1 CPU is
- 1 AWS vCPU, 
- 1 GPC Core,
- 1 Azure Core or
- 1 Hyperthread

<b>1</b> cpu can be represented as <b>1000m</b>.
1m lowest-

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