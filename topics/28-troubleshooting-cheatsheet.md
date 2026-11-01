# 28 - Troubleshooting Cheat Sheet

## Service won't start
```bash
systemctl status svc -l
journalctl -u svc -xe
systemctl --failed
ss -tulnp | grep PORT            # port conflict?
ausearch -m avc -ts recent       # SELinux?
firewall-cmd --list-all
```

## Can't SSH
```bash
systemctl status sshd
ss -tlnp | grep 22
firewall-cmd --list-all
tail /var/log/secure
ls -ld ~/.ssh ; ls -l ~/.ssh/authorized_keys
restorecon -Rv ~/.ssh
ssh -vvv user@host
```

## Disk full
```bash
df -h ; df -i
du -ahx / | sort -rh | head -20
lsof +L1                         # deleted-but-open files
journalctl --vacuum-size=200M
dnf clean all
```

## Network down
```bash
ip a ; ip r
nmcli device status
ping 127.0.0.1 ; ping GATEWAY ; ping 8.8.8.8 ; ping google.com
cat /etc/resolv.conf
dig example.com
systemctl restart NetworkManager
```

## High load
```bash
uptime ; top ; vmstat 2 5 ; iostat -xz 2
ps aux --sort=-%cpu | head
```

## Won't boot
- GRUB `e` -> add `systemd.unit=rescue.target`
- Broken fstab: boot rescue, `mount -o remount,rw /`, fix `/etc/fstab`
- Bad kernel: choose older kernel at GRUB
- Password: `rd.break` method (see 14-boot-grub-recovery.md)

## Package problems
```bash
dnf clean all && dnf makecache
rpm --rebuilddb
dnf distro-sync
dnf history ; dnf history undo ID
```

## Permission denied
```bash
ls -l file ; ls -Z file ; getfacl file
id ; namei -l /full/path
getenforce
```

## Quick one-liners
```bash
ps aux | awk '{print $1}' | sort | uniq -c | sort -rn
ss -tan | awk '{print $1}' | sort | uniq -c
find / -xdev -type f -size +500M 2>/dev/null
grep -rl "string" /etc 2>/dev/null
watch -n 2 'df -h'
```
