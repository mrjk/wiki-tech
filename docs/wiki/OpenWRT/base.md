# OpenWRT

## Stuffs

Update and upgrade all packages:
```
opkg list-upgradable | cut -f 1 -d ' ' | xargs -r echo opkg upgrade
```


