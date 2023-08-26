
```terminal
docker run --network none nginx // no nw-connectivity
docker run --network host nginx // host-nw -> http://192.168.0.1:80
docker run --network host nginx // will not work -> 2 processes can not listen on same port at the same time
docker run nginx // bridge
docker run nginx // bridge
```


```terminal
docker network ls
ip link
ip netns
```

nginx is reachable from the host, but not outside.<br>
To make it reachable a port-mapping has to be defined:
> docker run -p 8080:80 nginx

How to see the mapping?
> iptables -nvL -t nat