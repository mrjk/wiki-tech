# Local Devops Station

This setup aims to setup basic infra for local development.


Consists in:
* Providing DNS
* HTTP proxy



## DNS Management

There are few methods to do that:
* Split DNS with systemd-resolved
* Unbound
* NetworkManager + dnsmasq
* Run DNSMasq in docker

### NetworkManager + dnsmasq

Tell networkmanager to run its own dnsmasq:
```
cat /etc/NetworkManager/conf.d/dnsmasq.conf
[main]
dns=dnsmasq

```

Configure dnsmasq:
```
$ cat /etc/NetworkManager/dnsmasq.d/00-box.conf 

# This file directs dnsmasq to forward any request to resolve
# names under the .homelab domain to 172.31.0.1, my 
# home DNS server.
server=/box/10.127.0.10


# /etc/NetworkManager/dnsmasq.d/01-laplab.conf
# This file sets up the local lablab domain and 
# defines some aliases and a wildcard.
local=/laplab/
# The below defines a Wildcard DNS Entry.
address=/.ose.laplab/192.168.101.125
# Below I define some host names.  I also pull in   
address=/openshift.laplab/192.168.101.120
address=/openshift-int.laplab/192.168.101.120

# By default, the plugin does not read from /etc/hosts.  
# This forces the plugin to slurp in the file.
#
# If you didn't want to write to the /etc/hosts file.  This could
# be pointed to another file.
#
# addn-hosts=/etc/hosts


```

Reload Network-Manager:
```
systemctl  reload NetworkManager
```

Check dnsmasq is really running:
```
$ pstree -alup $(pidof NetworkManager)
NetworkManager,1438674 --no-daemon
  ├─dnsmasq,1441866,nobody --no-resolv --keep-in-foreground --no-hosts --bind-interfaces --pid-file=/var/run/NetworkManager/dnsmasq.pid --listen-address=127.0.0.1 --cache-size=400 --clear-on-reload --conf-file=/dev/null --proxy-dnssec --enable-dbus=org.freedesktop.NetworkManager.dnsmasq --conf-dir=/etc/NetworkManager/dnsmasq.d
  ├─{NetworkManager},1438675
  └─{NetworkManager},1438676
```

Check dnsmasq is really listening:
```
 netstat  -lntpu | grep 1:53
tcp        0      0 127.0.0.1:53            0.0.0.0:*               LISTEN      1441866/dnsmasq     
udp        0      0 127.0.0.1:53            0.0.0.0:*                           1441866/dnsmasq     

```

Check dnsmasq is really working:
```
dig @10.127.0.10 toto.box
dig toto.box
ping toto.box
```

