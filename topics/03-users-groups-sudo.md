# 03 - User, Group & sudo Management

## Users
```bash
useradd alice
useradd -m -s /bin/bash -G wheel -c "Alice A" alice
useradd -u 1500 -g dev -d /data/alice alice
passwd alice
usermod -aG wheel alice        # add to group (append!)
usermod -L alice ; usermod -U alice   # lock / unlock
usermod -s /sbin/nologin alice
userdel -r alice               # delete with home
id alice ; groups alice
last ; lastlog ; w ; who
```

## Groups
```bash
groupadd dev
groupadd -g 2000 dev
groupmod -n newname dev
groupdel dev
gpasswd -a alice dev           # add member
gpasswd -d alice dev           # remove member
```

## Password aging
```bash
chage -l alice                 # show policy
chage -M 90 -m 7 -W 14 alice   # max/min/warn days
chage -E 2026-12-31 alice      # account expiry
chage -d 0 alice               # force change at next login
```

## Important files
| File | Purpose |
|---|---|
| `/etc/passwd` | user accounts |
| `/etc/shadow` | password hashes |
| `/etc/group` | groups |
| `/etc/gshadow` | group passwords |
| `/etc/login.defs` | defaults (UID range, aging) |
| `/etc/skel` | template for new home dirs |

## sudo
```bash
visudo                         # edit /etc/sudoers safely
sudo -l                        # list my sudo rights
sudo -u bob command
sudo su -
```
Examples in `/etc/sudoers.d/alice`:
```
alice ALL=(ALL) ALL
%dev  ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart httpd
```
Add to admin group: `usermod -aG wheel alice`

## Switch user
```bash
su - bob                       # login shell
su -c "command" bob
```
