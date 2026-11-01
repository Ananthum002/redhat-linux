# 22 - Time Synchronization (chrony / NTP)

## timedatectl
```bash
timedatectl
timedatectl list-timezones | grep Asia
timedatectl set-timezone Asia/Kolkata
timedatectl set-time "2026-01-01 10:00:00"
timedatectl set-ntp true
hwclock --systohc              # system -> hardware clock
```

## chrony
```bash
dnf install chrony
systemctl enable --now chronyd
chronyc sources -v             # NTP servers
chronyc tracking               # offset, stratum
chronyc sourcestats
chronyc makestep               # step clock immediately
chronyc clients
```
`/etc/chrony.conf`:
```
server 0.pool.ntp.org iburst
pool 2.rhel.pool.ntp.org iburst
allow 192.168.1.0/24          # make this host an NTP server
```
```bash
systemctl restart chronyd
firewall-cmd --permanent --add-service=ntp && firewall-cmd --reload
```
