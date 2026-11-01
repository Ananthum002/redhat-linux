# 10 - Firewall (firewalld / nftables)

## Basics
```bash
systemctl enable --now firewalld
firewall-cmd --state
firewall-cmd --reload
firewall-cmd --get-default-zone
firewall-cmd --get-active-zones
firewall-cmd --get-zones
firewall-cmd --list-all
firewall-cmd --list-all --zone=public
```

## Services & ports
```bash
firewall-cmd --permanent --add-service=http
firewall-cmd --permanent --add-service=https
firewall-cmd --permanent --remove-service=cockpit
firewall-cmd --permanent --add-port=8080/tcp
firewall-cmd --permanent --add-port=5000-5100/udp
firewall-cmd --permanent --remove-port=8080/tcp
firewall-cmd --reload
firewall-cmd --get-services
```
Runtime-only (lost on reload): omit `--permanent`.
Make runtime config permanent: `firewall-cmd --runtime-to-permanent`

## Zones
```bash
firewall-cmd --set-default-zone=internal
firewall-cmd --permanent --zone=trusted --add-source=192.168.1.0/24
firewall-cmd --permanent --zone=public --change-interface=eth0
```

## Rich rules
```bash
firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="10.0.0.5" port port="22" protocol="tcp" accept'
firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="1.2.3.4" reject'
firewall-cmd --list-rich-rules
```

## NAT / port forwarding
```bash
firewall-cmd --permanent --add-masquerade
firewall-cmd --permanent --add-forward-port=port=80:proto=tcp:toport=8080
firewall-cmd --permanent --add-forward-port=port=80:proto=tcp:toaddr=10.0.0.20:toport=80
```

## Emergency
```bash
firewall-cmd --panic-on        # drop all traffic
firewall-cmd --panic-off
```

## nftables / iptables
```bash
nft list ruleset
iptables -L -n -v
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```
