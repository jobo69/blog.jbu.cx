---
title: "How to create a Docker Network with IPv6 Support"
date: 2023-03-04T02:00:00+01:00
draft: false
tags: [docker,ipv6]
image: static/media/posts/how-to-create-a-docker-network-with-ipv6-support/fejuz-q6j5mSRpi50-unsplash.jpg
---

## Requirements
- Static IPv6 Prefix
- Understanding how IPv6 works
- The ability to create static routes inside a router
- Docker

[IPv6 Subnetting Chart](https://www.ripe.net/about-us/press-centre/ipv6-chart_2015.pdf)

## IPv6 Subnet hierarchy
- `2001:db8::/32` IPv6 Prefix of my ISP
    - `2001:db8:23f1::/48` IPv6 Prefix I received from my ISP
        - `2001:db8:23f1:40::/64` IPv6 Prefix of the VLAN
            - `2001:db8:23f1:40:10XX::/72` Sub Prefix for Docker
                - `2001:db8:23f1:40:1000::/80` Network 0
                - `2001:db8:23f1:40:1001::/80` Network 1
                - `2001:db8:23f1:40:1002::/80` Network 2
                - `2001:db8:23f1:40:1003::/80` Network 3
                - ...

## Configure IPv6 on Docker
Modify the `/etc/default/docker` file and add this:
```bash
DOCKER_OPTS="--ipv6 --fixed-cidr-v6='2001:db8:23f1:40:1000::/72'"
```
## Create a IPv6 Bridge
```bash
docker network create --driver bridge --ipv6 --subnet 2001:db8:23f1:40:1000::/80 network-name
```
## Add static route
Create a static route inside your router. This step is different from router to router.
![](/static/media/posts/how-to-create-a-docker-network-with-ipv6-support/static-route-1.png) 
![](/static/media/posts/how-to-create-a-docker-network-with-ipv6-support/static-route-2.png)

## Creating a Container with IPv6
You should now be able to create a container with native IPv6 support.
```bash
docker run --network network-name -it alpine /bin/sh 
```

## Testing
```bash
root@docker:~# docker run --network network-name -it alpine /bin/sh
/ > ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:AC:13:00:03  
          inet addr:172.19.0.3  Bcast:172.19.255.255  Mask:255.255.0.0
          inet6 addr: 2001:db8:23f1:40:1000::2/80 Scope:Global
          inet6 addr: fe80::42:acff:fe13:3/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:7 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:746 (746.0 B)  TX bytes:596 (596.0 B)

/ > ping 2600::
PING 2600:: (2600::): 56 data bytes
64 bytes from 2600::: seq=0 ttl=46 time=120.497 ms
64 bytes from 2600::: seq=1 ttl=46 time=119.503 ms
^C
--- 2600:: ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 119.503/120.000/120.497 ms
```
