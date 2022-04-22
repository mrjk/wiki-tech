# Firewalld

Links:
* https://docs.rockylinux.org/de/guides/security/firewalld-beginners/

## Basics

Ensure the firewalld service is running:
```
systemctl enable --now firewalld
```

Let's check current status:
```
# systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2022-02-23 12:36:26 EST; 1 months 10 days ago
     Docs: man:firewalld(1)
 Main PID: 23834 (firewalld)
    Tasks: 3 (limit: 5065)
   Memory: 9.7M
   CGroup: /system.slice/firewalld.service
           └─23834 /usr/libexec/platform-python -s /usr/sbin/firewalld --nofork --nopid

# firewall-cmd --state
running

# firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: ens3
  sources: 
  services: cockpit consul_api_dns consul_api_http consul_lan_serf consul_rpc consul_wan_serf dhcpv6-client dns docker-swarm glusterfs http https node_exporter ssh
  ports: 9100/tcp
  protocols: 
  forward: no
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
```

We have a running service and some firewalld services. After any permanent changes, you need to reload the firewall:
```
firewall-cmd --reload
```

    Note: If you reload your configurations that haven't been made permanent, they'll disappear on you.


List all opened ports
```
firewall-cmd --list-ports


```

To get an overview of effectively opened ports:
```
for i in $(firewall-cmd --list-services); do echo "Service: $i" ;firewall-cmd --info-service $i | grep ' ports:' ; done ; echo "Direct ports:"; firewall-cmd --list-ports
```


## Modifications

### Using services

### Direct port

```
firewall-cmd --permanent --add-port=22/TCP
firewall-cmd --reload
```

```
firewall-cmd --add-port=22/tcp

# To save it permanently
firewall-cmd --runtime-to-permanent
```


