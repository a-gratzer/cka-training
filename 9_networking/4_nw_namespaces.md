# Network namespaces

## Create
Create namespace "red"
> ip netns add red
> Create namespace "blue"
> ip netns add blue

## Delete
> ip -n red link del veth-red // blue will be deleted automatically 

## Show
> ip netns
## what is visible to red?
> ip netns exec red ip link <br>
> or <br>
> ip -n red link
## Link namespaces

> ip link add veth-red type veth peer name veth-blue <br>
> ip link set vet-red netsn red <br>
> ip link set vet-blue netsn blue <br>

## Assign IPs
> ip -n red addr add 192.168.15.1 dev veth-red <br>
> ip -n blue addr add 192.168.15.2 dev veth-blue <br>

# Bridge
## Create the bridge-interface
> ip link add v-net-0 type bridge
## Enable the bridge
> ip link set dev v-net-0 up
## Connect
> ip link add veth-red type veth peer name veth-red-br <br>
> ip link add veth-blue type veth peer name veth-blue-br <br>
> 
> ip link set veth-red netns red <br>
> ip link set veth-red-br master v-net-0
>
> ip link set veth-blue netns blue <br>
> ip link set veth-blue-br master v-net-0
> 
> ip -n red addr add 192.168.15.1  dev veth-red
> ip -n blue addr add 192.168.15.2  dev veth-blue
> 
> ip n- red link set veth-red up <br>
> ip n- blue link set veth-blue up