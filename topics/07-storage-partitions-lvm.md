# 07 - Storage: Partitions, Filesystems, LVM, Swap

## Inspect
```bash
lsblk ; lsblk -f
blkid
fdisk -l
df -hT                         # usage + fs type
du -sh /var/*                  # dir sizes
du -ah /var | sort -rh | head
findmnt
```

## Partitioning
```bash
fdisk /dev/sdb                 # MBR: n, p, w
gdisk /dev/sdb                 # GPT
parted /dev/sdb mklabel gpt
parted /dev/sdb mkpart primary xfs 1MiB 10GiB
partprobe /dev/sdb             # re-read partition table
```

## Filesystems
```bash
mkfs.xfs /dev/sdb1
mkfs.ext4 /dev/sdb1
xfs_growfs /mnt/data           # grow XFS (online)
resize2fs /dev/sdb1            # grow ext4
xfs_repair /dev/sdb1           # (unmounted)
fsck /dev/sdb1                 # ext4 check
xfs_info /mnt/data
```

## Mounting
```bash
mount /dev/sdb1 /mnt/data
mount -o ro,noexec /dev/sdb1 /mnt/data
umount /mnt/data
mount -a                       # test fstab
mount -o remount,rw /
```
`/etc/fstab` line (use UUID from `blkid`):
```
UUID=xxxx-xxxx  /data  xfs  defaults  0 0
```

## LVM workflow
```bash
pvcreate /dev/sdb /dev/sdc            # physical volumes
vgcreate vgdata /dev/sdb /dev/sdc     # volume group
lvcreate -L 10G -n lvapp vgdata       # logical volume
lvcreate -l 100%FREE -n lvall vgdata
mkfs.xfs /dev/vgdata/lvapp
mount /dev/vgdata/lvapp /app
```
Display:
```bash
pvs ; vgs ; lvs
pvdisplay ; vgdisplay ; lvdisplay
```
Extend:
```bash
vgextend vgdata /dev/sdd
lvextend -L +5G /dev/vgdata/lvapp
lvextend -r -L +5G /dev/vgdata/lvapp   # -r resizes the FS too
xfs_growfs /app                        # if no -r (XFS)
```
Reduce (ext4 only; XFS cannot shrink):
```bash
umount /app
e2fsck -f /dev/vgdata/lvapp
resize2fs /dev/vgdata/lvapp 5G
lvreduce -L 5G /dev/vgdata/lvapp
```
Remove:
```bash
lvremove /dev/vgdata/lvapp
vgremove vgdata
pvremove /dev/sdb
```
Snapshot:
```bash
lvcreate -s -n snap1 -L 2G /dev/vgdata/lvapp
lvconvert --merge /dev/vgdata/snap1
```

## Swap
```bash
mkswap /dev/sdb2 ; swapon /dev/sdb2
swapon --show ; free -h
swapoff /dev/sdb2
# swap file
dd if=/dev/zero of=/swapfile bs=1M count=2048
chmod 600 /swapfile ; mkswap /swapfile ; swapon /swapfile
```
fstab: `/swapfile swap swap defaults 0 0`

## Stratis / VDO (RHEL 8/9)
```bash
stratis pool create pool1 /dev/sdb
stratis filesystem create pool1 fs1
```

## RAID (mdadm)
```bash
mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdb /dev/sdc
cat /proc/mdstat
mdadm --detail /dev/md0
mdadm --detail --scan >> /etc/mdadm.conf
mdadm /dev/md0 --fail /dev/sdb --remove /dev/sdb
```

## Quotas (XFS)
```bash
mount -o uquota /dev/sdb1 /data
xfs_quota -x -c 'limit bsoft=900m bhard=1g alice' /data
xfs_quota -x -c report /data
```
