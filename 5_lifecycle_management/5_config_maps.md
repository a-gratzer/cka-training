https://kubernetes.io/docs/concepts/configuration/configmap/
https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/

## Create
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: game-demo
data:
  # property-like keys; each key maps to a simple value
  player_initial_lives: "3"
  ui_properties_file_name: "user-interface.properties"

  # file-like keys
  game.properties: |
    enemy.types=aliens,monsters
    player.maximum-lives=5    
  user-interface.properties: |
    color.good=purple
    color.bad=yellow
    allow.textmode=true 
```

```yaml
k create config map \
  app-config --from-literal=APP_COLOR=blue \
--from-literal=...
```

## Get

```yaml
k get configmaps
k describe configmaps
```