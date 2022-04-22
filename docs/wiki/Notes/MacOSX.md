# MacOSX tricks

## One liners

Equivalent of `nestat -lntp`:
```
netstat -p tcp -van | grep '^Proto\|LISTEN'
netstat -p udp -van | grep '\.[0-9]'
```
