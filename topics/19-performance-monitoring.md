# 19 - Performance Monitoring & Troubleshooting

## CPU / Load
```bash
uptime                         # load avg (1,5,15 min)
top ; htop
mpstat -P ALL 2                # per-CPU
vmstat 2 5
sar -u 2 5                     # CPU
pidstat -u 2
nproc ; lscpu
```

## Memory
```bash
free -h
vmstat -s
cat /proc/meminfo
ps aux --sort=-%mem | head
slabtop
sar -r 2 5
```

## Disk I/O
```bash
iostat -xz 2                   # (sysstat)
iotop -o                       # (dnf install iotop)
df -hT ; df -i                 # space ; inodes
du -sh /* 2>/dev/null | sort -h
lsof | grep deleted            # space held by deleted files
```

## Network
```bash
ss -s
iftop ; nload
sar -n DEV 2 5
ip -s link
```

## sysstat
```bash
dnf install sysstat
systemctl enable --now sysstat
sar -A
sar -f /var/log/sa/sa15
```

## Performance Co-Pilot / Cockpit
```bash
dnf install cockpit
systemctl enable --now cockpit.socket     # https://host:9090
dnf install pcp pcp-system-tools
pmstat
```

## Kernel tuning (sysctl)
```bash
sysctl -a
sysctl vm.swappiness
sysctl -w vm.swappiness=10
```
Persistent: `/etc/sysctl.d/99-custom.conf` then `sysctl --system`.
```
vm.swappiness = 10
net.ipv4.ip_forward = 1
net.core.somaxconn = 1024
fs.file-max = 500000
```

## Limits
```bash
ulimit -a
ulimit -n 65535
```
`/etc/security/limits.conf`: `alice soft nofile 65535`

## Troubleshooting checklist
1. `uptime` -> load high?
2. `top` -> CPU vs memory vs iowait
3. `free -h` / `vmstat` -> swapping?
4. `iostat -xz` -> disk saturated?
5. `df -h`, `df -i` -> full?
6. `journalctl -p err -b`, `dmesg -T`
7. `ss -tulnp`, `systemctl --failed`
