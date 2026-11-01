# 13 - Logging, journalctl, auditd

## Key log files
| File | Content |
|---|---|
| `/var/log/messages` | general system |
| `/var/log/secure` | auth, sudo, ssh |
| `/var/log/cron` | cron jobs |
| `/var/log/dmesg` | kernel ring buffer |
| `/var/log/boot.log` | boot messages |
| `/var/log/audit/audit.log` | auditd / SELinux |
| `/var/log/httpd/` | Apache |
| `/var/log/dnf.log` | package ops |

## journalctl
```bash
journalctl                     # everything
journalctl -xe                 # recent + explanation
journalctl -f                  # follow
journalctl -u sshd
journalctl -u httpd --since "1 hour ago"
journalctl --since "2026-01-01" --until "2026-01-02"
journalctl -p err              # priority err and above
journalctl -b                  # this boot
journalctl -b -1               # previous boot
journalctl --list-boots
journalctl _PID=1234
journalctl -k                  # kernel messages
journalctl -o json-pretty
journalctl --disk-usage
journalctl --vacuum-time=7d
journalctl --vacuum-size=500M
```
Make journal persistent:
```bash
mkdir -p /var/log/journal
systemctl restart systemd-journald
```
(or `Storage=persistent` in `/etc/systemd/journald.conf`)

## Kernel & boot
```bash
dmesg | tail
dmesg -T | grep -i error
dmesg -w
```

## Auth history
```bash
last ; lastb ; lastlog
grep "Failed password" /var/log/secure
```

## rsyslog
```bash
systemctl status rsyslog
logger "test message"          # write to syslog
```
Config: `/etc/rsyslog.conf`, `/etc/rsyslog.d/`
Remote forward: `*.* @@loghost:514`

## auditd
```bash
auditctl -l                    # list rules
auditctl -w /etc/passwd -p wa -k passwd_changes
ausearch -k passwd_changes
aureport --summary
aureport -au                   # auth report
```
