# 24 - Security & Hardening

## Updates
```bash
dnf update --security
dnf updateinfo list security
dnf install dnf-automatic
systemctl enable --now dnf-automatic.timer
```

## Users & auth
```bash
passwd -l user                 # lock
grep -v '^#' /etc/ssh/sshd_config
awk -F: '$2==""' /etc/shadow   # empty passwords
awk -F: '$3==0' /etc/passwd    # UID 0 accounts
lastb | head                   # failed logins
```
Password policy: `/etc/security/pwquality.conf`
```
minlen = 12
minclass = 3
```
Lockout (faillock):
```bash
faillock --user alice
faillock --user alice --reset
```
`/etc/security/faillock.conf`: `deny = 5`, `unlock_time = 600`

## fail2ban (EPEL)
```bash
dnf install fail2ban
systemctl enable --now fail2ban
fail2ban-client status sshd
fail2ban-client set sshd unbanip 1.2.3.4
```

## Open ports / services
```bash
ss -tulnp
nmap -sV localhost
systemctl list-unit-files --state=enabled
```

## File integrity & audit
```bash
dnf install aide
aide --init
mv /var/lib/aide/aide.db.new.gz /var/lib/aide/aide.db.gz
aide --check
find / -perm -4000 -type f 2>/dev/null     # SUID
find / -perm -0002 -type f 2>/dev/null     # world-writable
find / -nouser -o -nogroup 2>/dev/null
```

## Crypto policy & FIPS
```bash
update-crypto-policies --show
update-crypto-policies --set FUTURE
fips-mode-setup --check
```

## Compliance scanning
```bash
dnf install openscap-scanner scap-security-guide
oscap xccdf eval --profile xccdf_org.ssgproject.content_profile_cis \
  --report report.html /usr/share/xml/scap/ssg/content/ssg-rhel9-ds.xml
```

## Encryption (LUKS)
```bash
cryptsetup luksFormat /dev/sdb1
cryptsetup open /dev/sdb1 secure
mkfs.xfs /dev/mapper/secure
mount /dev/mapper/secure /mnt/secure
umount /mnt/secure && cryptsetup close secure
```

## GPG / OpenSSL
```bash
gpg --gen-key ; gpg --list-keys
gpg -c file ; gpg -d file.gpg
openssl genrsa -out key.pem 2048
openssl x509 -in cert.pem -text -noout
openssl s_client -connect host:443
```
