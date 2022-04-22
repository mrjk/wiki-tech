# VPN as users

This page is relevant when you have to deal with corporate VPN and associated stuffs.

## Force split tunneling

Split tunneling allows you to access your local network resources, even if your VPN provider does not allow it. The trick of those VPN client is to remove locale routes and keep only the local router route. There are few scenarios:

* The VPN client does not remove LAN routes: You are fine, you are already in split tunneling
* The VPN client remove LAN routes: You can manually add the route after the VPN started
* The VPN client enforce removal of LAN routes: When a route is added, the VPN client will remove it straight after it has been added.

Be aware that bypassing your VPN policy may expose your VPN network, or make your IT security team unhappy. But hopefully, this is completely transparent, and there is no way to see that you are bypassing those policy from a network point of view.


### Not enforced Route Removal

This setup is relevant when you have only one pizzabox that connect to the VPN, but you don't want to work on that pizzabox.

#### MacOSX

The command is the folllowing:
```
sudo route -n add -net NETWORK/MASK ROUTER
```

As examples:
```
sudo route -n add -net 192.168.0.1/24 192.168.0.1
sudo route -n add -net 10.0.0.0/8 192.168.0.254
```
If you want to change the default route:
```
route delete default
route add default 192.168.0.1
```

### Enforced Route Removal

For this scenario, you need to have control on your LAN router. 

#### Allow SSH connections

* Easiest scenario is to have a SSH access on your router, which is easy if you run OpenWRT,OPNSense or any linux based router that provides a SSH access.
* If no SSH is available, you will need to forward internally a SSH port to your pizzabox from the router IP. Port forward should be something like: 192.168.0.1:2222 -> pizzabox:22

Then configure your SSH client to access your pizzabox, either by:

* setting up your router as a SSH jumphost
* setting up your SSH client on the forwarded port of your router.

Check you can access your pizza box via SSH when the VPN is connected. If that works, we can leverage `sshuttle`. Of course, you can access to you local LAN resources by doing the same thing, but in the other way (but it will not be practical if you're using port forwqarding)

#### Expose VPN resources via sshuttle

sshuttle will create a SSH tunnel to your pizzabox:
```
sshuttle -v --remote pizzabox-via-router 10.55.0.0/16 10.50.58.0/24 142.117.120.0/22  142.117.244.0/22  142.117.120.0/22 10.36.100.0/22 10.36.132.0/22 10.50.50.0/24 142.182.130.85/22 142.112.0.0/12  142.182.0.0/16
```

You can add the `--dns` option if you also want to forward the DNS traffic.

#### Expose web resources

The trick here is to configure your browser to use a socks proxy
```
ssh -vv -N -D 7777 pizzabox
```
Then you can configure your favorite browser to use a socks proxy on `127.0.0.1:7777`

#### Expose more resources

You can set SSH reverse proxy, for example to access to your socks resources, here we want to access to barrier server:
```
ssh -vv -N  -R 24800:localhost:24800 pizzabox
```
On the client side, you just have to connect to `localhost:24800` instead of `pizzabox:24800`


## Pizzabox tools

There is a list of useful tools to have:

* SSH listening on port 22
* SOCKS proxy (for web access)
* dnsmasq instance, for DNS traffic (to resolve any VPN domains)
* Barrier, for software KVM


### dnsmasq

Run a dnsmasq instance locally:
```
/usr/local/Cellar/dnsmasq/2.86/sbin/dnsmasq --no-daemon  --listen-address=127.0.0.1 --port 5353
```





