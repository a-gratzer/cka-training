## Exports
```bash
export do="--dry-run=client -o yaml"
export d="--dry-run=client"
export oy="-o yaml"
```
## Count entries
e.g. pods
    
    k get po --no-headers | wc -l

e.g. all

    k get all --no-headers --selector key=value,key-2=value | wc -l

## Get PVs sorted by capacity

    k get pv --sort-by='.spec.capacity.storage'
    # just 2 columns
    kubectl get pv --sort-by=.spec.capacity.storage -o=custom-columns=NAME:.metadata.name,CAPACITY:.spec.capacity.storage > /opt/outputs/pv-and-capacity-sorted.txt

## Get users from kube-conf

    k config --kubeconfig /root/my-kube-config view --output jsonpath={.users[*].name} > /opt/outputs/users.txt

## Get Context from specific user in kube-conf
    kubectl config view --kubeconfig=my-kube-config -o jsonpath="{.contexts[?(@.context.user=='aws-user')].name}" > /opt/outputs/aws-context-name

## Get osImages from nodes

    k get nodes -o=jsonpath='{.items[*].status.nodeInfo.osImage}' > /opt/outputs/nodes_os.txt