# 11 - SELinux

## Status & modes
```bash
getenforce                     # Enforcing / Permissive / Disabled
sestatus
setenforce 0                   # permissive (temporary)
setenforce 1                   # enforcing
```
Permanent: edit `/etc/selinux/config` -> `SELINUX=enforcing` (reboot; relabel needed if coming from disabled: `touch /.autorelabel`).

## Contexts
```bash
ls -Z /var/www/html
ps -eZ | grep httpd
id -Z
```
Format: `user:role:type:level` (type is the most important).

## File contexts
```bash
semanage fcontext -a -t httpd_sys_content_t "/web(/.*)?"
restorecon -Rv /web            # apply
restorecon -Rv /var/www
chcon -t httpd_sys_content_t file   # temporary change
semanage fcontext -l | grep httpd
```

## Booleans
```bash
getsebool -a
getsebool httpd_can_network_connect
setsebool -P httpd_can_network_connect on     # -P = persistent
semanage boolean -l
```

## Ports
```bash
semanage port -l | grep http
semanage port -a -t http_port_t -p tcp 8888
semanage port -d -t http_port_t -p tcp 8888
```

## Troubleshooting
```bash
ausearch -m avc -ts recent
ausearch -m avc -ts today | audit2why
sealert -a /var/log/audit/audit.log
grep denied /var/log/audit/audit.log
audit2allow -a -M mypolicy     # build custom module
semodule -i mypolicy.pp
semodule -l
```
Install tools: `dnf install policycoreutils-python-utils setroubleshoot-server`

## Workflow when something is blocked
1. `ls -Z` check file label  -> `restorecon`
2. Check boolean -> `setsebool -P`
3. Check port label -> `semanage port`
4. Last resort: custom policy via `audit2allow`
Never disable SELinux as the fix.
