# 25 - Kernel, Modules & Hardware

## Kernel modules
```bash
lsmod
modinfo e1000e
modprobe vfat                  # load
modprobe -r vfat               # unload
depmod -a
```
Persistent load: `/etc/modules-load.d/mymod.conf`
Blacklist: `/etc/modprobe.d/blacklist.conf` -> `blacklist nouveau`
Then `dracut -f`.

## Hardware info
```bash
lspci ; lspci -v
lsusb
lscpu
lsmem
lsblk
dmidecode -t memory
dmidecode -t system
hwinfo --short                 # if installed
sensors                        # lm_sensors
smartctl -a /dev/sda           # smartmontools
udevadm info /dev/sda
```

## udev
Rules: `/etc/udev/rules.d/`
```bash
udevadm control --reload
udevadm trigger
udevadm monitor
```

## Kernel parameters
```bash
cat /proc/cmdline
sysctl -a | less
```

## Kernel updates & live patch
```bash
dnf install kernel
grubby --default-kernel
dnf install kpatch
kpatch list
```

## Virtualization (KVM)
```bash
dnf install qemu-kvm libvirt virt-install virt-manager
systemctl enable --now libvirtd
virsh list --all
virsh start vm1 ; virsh shutdown vm1 ; virsh destroy vm1
virsh console vm1
virt-install --name vm1 --memory 2048 --vcpus 2 \
  --disk size=20 --cdrom /iso/rhel.iso --os-variant rhel9.0
```
