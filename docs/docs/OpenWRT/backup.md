# Backup and restore OpenWRT

## Extra tools

* sysopkg: See: https://forum.openwrt.org/t/how-to-keep-packages-settings-after-upgrading/38998
* Newer versions now implement this.

## Default backup configuration

My backup configuration:
```
# Source: https://gist.github.com/mrjk/93b923ba97b71ca4226ac13048318685
## JEZ_VERSION=2021_09_06_01

# Documentation
# =======================
# This file contains files and directories that should
# be preserved during an upgrade.

# Generate backup:
#  /usr/sbin/sysopkg write
#  umask go=
#  sysupgrade -b /tmp/backup-${HOSTNAME}-$(date +%F).tar.gz

# Restore backup:
#  sysupgrade -r /tmp/backup-*.tar.gz
#  /usr/sbin/sysopkg install


# Base OpenWRT
# =======================

# Preserve this file configuration file
/etc/sysupgrade.conf

# Backup root directory
/root

# Backup SSH keys for autossh
/etc/ssh/

# Preserve shell files
/etc/profile.d/


# Other files
# =======================

# Support for sysopkg
/usr/sbin/sysopkg
/etc/opkg/packages.list

# Support for autossh
# See: https://openwrt.org/docs/guide-user/services/ssh/autossh#run_as_service
/etc/init.d/autossh
```

