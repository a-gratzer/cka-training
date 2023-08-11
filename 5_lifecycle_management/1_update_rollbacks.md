```terminal
k rollout status deployment/whoami-deployment -n tools
k rollout history deployment/whoami-deployment -n tools
k rollout undo deployment/whoami-deployment -n tools

# default deployment strategy = RollingUpdate
```