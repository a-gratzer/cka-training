    k create ns testing  
    k config set-context --current --namespace=NAMESPACE
    k get pods -o wide