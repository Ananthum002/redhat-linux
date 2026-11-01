# 23 - DNS (BIND), DHCP & Mail basics

## DNS server (BIND / named)
```bash
dnf install bind bind-utils
systemctl enable --now named
named-checkconf
named-checkzone example.com /var/named/example.com.zone
rndc reload
rndc status
firewall-cmd --permanent --add-service=dns && firewall-cmd --reload
```
Zone file `/var/named/example.com.zone`:
```
$TTL 86400
@   IN SOA ns1.example.com. admin.example.com. (
        2026010101 3600 1800 604800 86400 )
    IN NS  ns1.example.com.
ns1 IN A   192.168.1.10
www IN A   192.168.1.20
```
In `/etc/named.conf`:
```
zone "example.com" IN { type master; file "example.com.zone"; };
```
Query:
```bash
dig @192.168.1.10 www.example.com
dig example.com MX ; dig example.com NS ; dig +trace example.com
```

## DHCP server
```bash
dnf install dhcp-server
```
`/etc/dhcp/dhcpd.conf`:
```
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.100 192.168.1.200;
    option routers 192.168.1.1;
    option domain-name-servers 8.8.8.8;
    default-lease-time 600;
}
host printer { hardware ethernet AA:BB:CC:DD:EE:FF; fixed-address 192.168.1.50; }
```
```bash
systemctl enable --now dhcpd
firewall-cmd --permanent --add-service=dhcp && firewall-cmd --reload
cat /var/lib/dhcpd/dhcpd.leases
```

## Mail (Postfix)
```bash
dnf install postfix mailx
systemctl enable --now postfix
postconf -n
mailq ; postqueue -f
echo "body" | mail -s "subject" user@example.com
```
