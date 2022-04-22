# No Core Utils


Sometimes, while troubleshooting containers, you are lacking of some most basic commands, such as ping, ifconfig/ip commands.
This guide is intended for running containers, when it's already too late.

## Core utils alternatives


### Test if a TCP is open or not

```
$ telnet $host $port
$ (echo >/dev/tcp/$host/$port) &>/dev/null && echo "open" || echo "closed"
```

## Alternative images

Usually you will want this to troublehoot network issue. This image provides a bunch of tools of network troubleshooting. You
can inject
```
# No persistancy
docker run --rm -ti --network barbu wbitt/network-multitool:latest  /bin/bash

# As daemon
docker run -d -n netdebug wbitt/network-multitool
docker exec -it netdebug /bin/bash
```


## Install coreutils temporarly

We can download static builds of busybox/toybox:
```
$ wget http://landley.net/toybox/bin/toybox-i686 -O /usr/local/bin/toybox ; chmod +x /usr/local/bin/toybox
```
To list all available commands:
```
$ toybox
```
Then you can use this way:
```
toybox ping google.com
toybox netstat -lntpu
toybox telnet toto.com 80
```


## Tips and tricks

Get a root mysql shell:
```
mysql -p$MYSQL_ROOT_PASSWORD
mysqldump -p$MYSQL_ROOT_PASSWORD DATABASE | less
watch -n 0.2 mysqladmin -p$MYSQL_ROOT_PASSWORD proc
```

