# 15 - File Sharing: NFS & Samba

## NFS server
```bash
dnf install nfs-utils
mkdir /share
```
`/etc/exports`:
```
/share  192.168.1.0/24(rw,sync,no_root_squash)
/pub    *(ro)
```
```bash
systemctl enable --now nfs-server
exportfs -rav                  # re-export
exportfs -v
firewall-cmd --permanent --add-service={nfs,rpc-bind,mountd}
firewall-cmd --reload
setsebool -P nfs_export_all_rw on
```

## NFS client
```bash
showmount -e server
mount -t nfs server:/share /mnt/nfs
umount /mnt/nfs
```
fstab:
```
server:/share  /mnt/nfs  nfs  defaults,_netdev  0 0
```
```bash
nfsstat
```

## autofs (on-demand mounts)
```bash
dnf install autofs
```
`/etc/auto.master`: `/mnt/auto  /etc/auto.nfs`
`/etc/auto.nfs`: `data  -rw  server:/share`
```bash
systemctl enable --now autofs
```

## Samba server
```bash
dnf install samba samba-client
```
`/etc/samba/smb.conf`:
```ini
[shared]
    path = /srv/samba
    writable = yes
    valid users = alice
```
```bash
useradd -s /sbin/nologin alice
smbpasswd -a alice
testparm
systemctl enable --now smb nmb
firewall-cmd --permanent --add-service=samba && firewall-cmd --reload
setsebool -P samba_enable_home_dirs on
chcon -t samba_share_t /srv/samba
```

## Samba client
```bash
smbclient -L //server -U alice
smbclient //server/shared -U alice
mount -t cifs //server/shared /mnt/smb -o username=alice
```
