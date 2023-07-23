
    k create -f rc-definitions.yaml
    k get po --selector=app=myapp --output=jsonpath={.items..metadata.name}
    k get rc

    # update. replacasets are immutable, so they have to be replaced
    k replace -f replicaset-definition.yaml
    # or
    k scale --replicas=2 -f replicaset-definition.yaml 

     k get rs frontend -o yaml > tmp.yaml
