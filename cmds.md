## Count entries
e.g. pods
    
    k get po --no-headers | wc -l

e.g. all

    k get all --no-headers --selector key=value,key-2=value | wc -l