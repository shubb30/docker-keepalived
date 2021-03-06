# Docker image containing Keepalived

Basic Docker image to run keepalived on host and set additional ip on interface 

To run docker in docker you must add this options:
- --net=host
- --cap-add=NET_ADMIN
- --name keepalived0 (optionally)

Load needed kernel module:
- ip_vs

Configure sysctl:
- `net.ipv4.ip_forward = 1`
- `net.ipv4.ip_nonlocal_bind = 1`

You need edit (add) this env:
- **VIP**: (optionally) provide virtual ip
- **VIP_N**: (optionally) provide a few virtual ip's
- **VROUTERID**: provide arbitary unique number 0..255
- **STATE**: provide transition state (MASTER, BACKUP, FAULT)
- **INTERFACE**: provide interface name
- **PRIORITY**: provide election priority (default 100)
- **AUTHPASS**: provide authentication password
- **INTERVAL_VRRP_SCRIPT_CHECK**: (optional) how ofter vrrp script should be run (default 3)

> To implement custom vrrp script you should replace file `/bin/check-status.sh`

Usage: 
```
docker run --net=host --cap-add=NET_ADMIN -e VIP=192.168.1.175 -e VROUTERID=112 -e STATE=BACKUP -e INTERFACE=eno1 -e PRIORITY=100 -e AUTHPASS=blah --name keepalived0 -d oberthur/docker-keepalived
```
OR
```
docker run --net=host --cap-add=NET_ADMIN -e VIP_1=192.168.1.175 -e VIP_2=192.168.1.176 -e VIP_3=192.168.1.199 -e VROUTERID=112 -e STATE=BACKUP -e INTERFACE=eno1 -e PRIORITY=100 -e AUTHPASS=blah --name keepalived0 -d oberthur/docker-keepalived 
```


