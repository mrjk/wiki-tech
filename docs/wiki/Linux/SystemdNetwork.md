# Systemd-network

This describes a possible network setup for a Debian 11 server. 


Under Debian like distros, three way to manage network:

* Old `/etc/networks/interfaces{,.d}` files
* NetworkManager service
* Systemd-network interfaces, under ``

Systemd network resources:

* Debian manual: https://wiki.debian.org/SystemdNetworkd


## Disabling other network manager

Disable Network Manager:
```
systemctl  disable --now  NetworkManager
```
And ensure network scripts are empty (under Debian based OSes)
```
$ tree /etc/network/interfaces*
/etc/network/interfaces [error opening dir]
/etc/network/interfaces.d

0 directories, 0 files
```

```
systemctl status systemd-networkd
systemctl  disable --noNetworkManager.service 
```

## Basic networking

Enable and check systemd:
```
systemctl enable systemd-networkd
systemctl status systemd-networkd
```

Basic DHCP network:
```
$ cat /etc/systemd/network/99-default.network
[Match]
Name = en* eth*

[Network]
DHCP = yes

```

When a files has been modified, there is no `reload`:
```
systemctl restart systemd-networkd
```

### DHCP and statics IPs on the same interface

```
$ cat /etc/systemd/network/50-server1.network 
[Match]
Name=enp1s0

[Network]
DHCP=yes
Address=192.168.42.16/24
Address=192.168.42.17/24
Address=192.168.42.18/24
Address=192.168.42.19/24
#DNS=8.8.8.8
#DNS=8.8.4.4

```


## Other stuffs

Some other stuffs to try.

### Local bridge with static IP

Let's create a new `netwdev` unit:
```
$ cat /etc/systemd/network/20-br_cloud.netdev
[NetDev]
Name=br_cloud
Kind=bridge
# To avoid random mac at each boot
# MACAddress=xx:xx:xx:xx:xx:xx

[Bridge]
STP=false
```

With an IP:
```bash
$ cat /etc/systemd/network/21-br_cloud.network
[Match]
Name=br_cloud

[Network]
Address=10.1.0.11/24
ConfigureWithoutCarrier=yes
```




