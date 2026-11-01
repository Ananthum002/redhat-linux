# Red Hat Linux (RHEL) Notes & Command Reference

A topic-wise collection of important Red Hat Enterprise Linux administration topics.
Each topic has its own file with practical commands. Works for RHEL 8/9, CentOS Stream, Rocky, AlmaLinux.

> Companion repo: **networking** (protocols, subnetting, routing theory).
> This repo covers the Linux/RHEL admin side.

## Repository structure
```
redhat-linux/
├── README.md
└── topics/
    ├── 01-basics-and-filesystem.md
    ├── ...
    └── 28-troubleshooting-cheatsheet.md
```

## Topic index

### Cluster 1 - Fundamentals
| # | Topic | Covers |
|---|---|---|
| 01 | [Basics & Filesystem](topics/01-basics-and-filesystem.md) | navigation, files, find, FHS, man pages |
| 02 | [File Permissions & ACL](topics/02-file-permissions.md) | chmod, chown, SUID/SGID, umask, setfacl |
| 03 | [Users, Groups & sudo](topics/03-users-groups-sudo.md) | useradd, usermod, chage, sudoers |
| 16 | [Text Processing](topics/16-text-processing.md) | grep, sed, awk, redirection, vim |
| 17 | [Shell Scripting](topics/17-shell-scripting.md) | variables, loops, functions, examples |

### Cluster 2 - System & Services
| # | Topic | Covers |
|---|---|---|
| 04 | [Package Management](topics/04-package-management.md) | dnf, rpm, repos, subscription-manager |
| 05 | [Process Management](topics/05-process-management.md) | ps, top, kill, nice, jobs, tuned |
| 06 | [systemd](topics/06-systemd-services.md) | services, targets, unit files, timers |
| 12 | [Scheduling](topics/12-cron-at-scheduling.md) | cron, at, logrotate |
| 14 | [Boot, GRUB & Recovery](topics/14-boot-grub-recovery.md) | GRUB2, rd.break, rescue mode |
| 22 | [Time Sync](topics/22-time-sync-chrony.md) | chrony, timedatectl |

### Cluster 3 - Storage
| # | Topic | Covers |
|---|---|---|
| 07 | [Storage, LVM & RAID](topics/07-storage-partitions-lvm.md) | fdisk, XFS/ext4, LVM, swap, mdadm, quotas |
| 15 | [NFS & Samba](topics/15-nfs-samba-fileshare.md) | exports, autofs, smb.conf |
| 18 | [Archive & Backup](topics/18-archive-compression-backup.md) | tar, rsync, dd, xfsdump, ReaR |

### Cluster 4 - Networking & Remote Access
| # | Topic | Covers |
|---|---|---|
| 08 | [Networking Basics](topics/08-networking-basics.md) | ip, nmcli, bonding, dig, tcpdump |
| 09 | [SSH](topics/09-ssh-and-remote-access.md) | keys, tunnels, sshd_config, scp/rsync |
| 10 | [firewalld](topics/10-firewalld.md) | zones, services, rich rules, NAT |
| 23 | [DNS, DHCP & Mail](topics/23-dns-dhcp-services.md) | BIND, dhcpd, Postfix |
| 20 | [Web Server](topics/20-web-server-httpd.md) | Apache, Nginx, SSL, vhosts |

### Cluster 5 - Security
| # | Topic | Covers |
|---|---|---|
| 11 | [SELinux](topics/11-selinux.md) | contexts, booleans, semanage, audit2allow |
| 24 | [Security Hardening](topics/24-security-hardening.md) | fail2ban, AIDE, LUKS, OpenSCAP, crypto policy |
| 13 | [Logs & Auditing](topics/13-logs-and-journalctl.md) | journalctl, rsyslog, auditd |

### Cluster 6 - Advanced / Enterprise
| # | Topic | Covers |
|---|---|---|
| 19 | [Performance Monitoring](topics/19-performance-monitoring.md) | sar, iostat, vmstat, sysctl |
| 21 | [Containers (Podman)](topics/21-containers-podman.md) | images, run, build, systemd integration |
| 25 | [Kernel, Modules & KVM](topics/25-kernel-modules-hardware.md) | modprobe, udev, hardware, virsh |
| 26 | [HA Cluster](topics/26-high-availability-cluster.md) | pcs, Pacemaker, Corosync, fencing |
| 27 | [Ansible](topics/27-ansible-automation.md) | inventory, ad-hoc, playbooks, vault |
| 28 | [Troubleshooting Cheat Sheet](topics/28-troubleshooting-cheatsheet.md) | quick fixes, one-liners |

## Suggested learning path
`01 -> 02 -> 03 -> 04 -> 06 -> 07 -> 08 -> 09 -> 10 -> 11 -> 13 -> 19 -> 28`, then advanced topics.

## Exam alignment
Covers most objectives of **RHCSA (EX200)** (topics 01-15) and parts of **RHCE (EX294)** (topic 27).

## Contributing
Add a new file in `topics/` using the next number, then add it to the index above.
