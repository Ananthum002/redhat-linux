# 08 - Networking (RHEL quick reference)

> Detailed networking notes live in the separate **networking** repository.
> This file is the RHEL-specific admin cheat sheet.

## Inspect
```bash
ip a ; ip addr show eth0
ip link
ip route ; ip r
ip neigh                       # ARP
ss -tulnp                      # listening ports + process
ss -s
netstat -tulnp                 # legacy (net-tools)
hostname -I
cat /etc/resolv.conf
```

## NetworkManager (nmcli)
```bash
nmcli device status
nmcli connection show
nmcli connection show eth0
nmcli connection add type ethernet ifname eth0 con-name static1 \
  ipv4.addresses 192.168.1.10/24 ipv4.gateway 192.168.1.1 \
  ipv4.dns 8.8.8.8 ipv4.method manual
nmcli connection modify eth0 ipv4.addresses 10.0.0.5/24
nmcli connection modify eth0 +ipv4.dns 1.1.1.1
nmcli connection up eth0 ; nmcli connection down eth0
nmcli connection reload
nmcli general hostname server1
nmtui                          # text UI
```
Config files: `/etc/NetworkManager/system-connections/*.nmconnection` (RHEL 9), `/etc/sysconfig/network-scripts/ifcfg-*` (RHEL 7/8).

## Hostname & hosts
```bash
hostnamectl set-hostname server1.example.com
cat /etc/hosts
```

## Temporary changes
```bash
ip addr add 192.168.1.50/24 dev eth0
ip route add 10.0.0.0/8 via 192.168.1.1
ip link set eth0 up
```

## Bonding / teaming / VLAN
```bash
nmcli con add type bond con-name bond0 ifname bond0 bond.options "mode=active-backup"
nmcli con add type bond-slave ifname eth1 master bond0
nmcli con add type vlan con-name vlan10 ifname eth0.10 dev eth0 id 10
```

## Testing
```bash
ping -c 4 8.8.8.8
traceroute google.com ; tracepath google.com
dig example.com ; dig +short A example.com ; dig -x 8.8.8.8
nslookup example.com
host example.com
curl -I https://example.com
curl -v telnet://host:22
wget URL
nc -zv host 80                 # port test
mtr google.com
tcpdump -i eth0 port 80
tcpdump -nn -i any host 10.0.0.5 -w cap.pcap
```
