# 14 - Boot Process, GRUB2 & Recovery

## Boot sequence
BIOS/UEFI -> bootloader (GRUB2) -> kernel + initramfs -> systemd (PID 1) -> target

## GRUB2
```bash
grub2-mkconfig -o /boot/grub2/grub.cfg                # BIOS
grub2-mkconfig -o /boot/efi/EFI/redhat/grub.cfg       # UEFI
grubby --default-kernel
grubby --set-default /boot/vmlinuz-<version>
grubby --info=ALL
grubby --update-kernel=ALL --args="quiet"
grubby --update-kernel=ALL --remove-args="rhgb"
grub2-install /dev/sda                                # BIOS only
```
Config: `/etc/default/grub` (then regenerate grub.cfg).

## Kernel
```bash
uname -r
rpm -q kernel
dnf install kernel
dracut -f                      # rebuild initramfs
dracut -f /boot/initramfs-$(uname -r).img $(uname -r)
```

## Reset forgotten root password (RHEL 8/9)
1. At GRUB menu press `e`
2. On the `linux` line append: `rd.break`
3. Press `Ctrl+X`
4. In the emergency shell:
```bash
mount -o remount,rw /sysroot
chroot /sysroot
passwd root
touch /.autorelabel
exit
exit
```

## Rescue modes
```bash
systemctl rescue               # single-user
systemctl emergency
```
At GRUB, append `systemd.unit=rescue.target` or `systemd.unit=emergency.target`.

## Rescue from installation media
Boot ISO -> Troubleshooting -> Rescue a Red Hat system.
```bash
chroot /mnt/sysroot
```

## Fix common issues
```bash
# Broken fstab -> emergency shell
mount -o remount,rw /
vi /etc/fstab
# Reinstall GRUB
grub2-install /dev/sda && grub2-mkconfig -o /boot/grub2/grub.cfg
```
