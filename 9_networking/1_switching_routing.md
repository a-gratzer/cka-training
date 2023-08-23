2 networks
- 192.168.1.0/24
- 192.168.2.0/24

2 nodes per network

## Switch between nodes within one network
> ip link

to see the interfaces (e.g.: eth0)
### Connect
> ip addr add 192.168.1.10/24 dev eth0<br>
> ip addr add 192.168.1.11/24 dev eth0<br>

## Route between the networks
Connect the 2 networks
> 192.168.1.1 <->192.168.2.1

This is done via "Gateways".

Display routing-tables:
> route

Configure:
> ip route add 192.168.2.0/24 via 192.168.1.1

This has to be configured on all systems.

Internet needed on sub-network?<br>
Create a route:
> ip route add IP-OF-INET-INTERFACE via 192.168.2.1

Or route anything via this network.
> ip route add default via 192.168.2.1<br>
> or <br>
> ip route add 0.0.0.0 via 192.168.2.1


## Ip forwarding
By default, no traffic will be forwarded to another network.
So this has to be enabled:
> echo 1 > /proc/sys/net/ipv4/ip_forward<br>
> // set net.ipv4.ip_forward = 1 in /etc/sysctl.conf