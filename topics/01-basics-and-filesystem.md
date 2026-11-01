# 01 - Basics & Filesystem Navigation

## System info
```bash
cat /etc/redhat-release        # RHEL version
hostnamectl                    # hostname, OS, kernel
uname -a                       # kernel info
uptime                         # load and uptime
whoami; id                     # current user, UID/GID
date; timedatectl              # date/time
lscpu; free -h; lsblk          # CPU, memory, disks
```

## Navigation
```bash
pwd                            # current directory
ls -lah                        # long list, hidden, human sizes
ls -ltr                        # sort by time, oldest first
cd /etc ; cd - ; cd ~          # go to dir, previous dir, home
tree -L 2 /etc                 # tree view (dnf install tree)
```

## File operations
```bash
touch file1                    # create empty file
mkdir -p /a/b/c                # create nested dirs
cp -r src dst                  # copy recursively
cp -a src dst                  # copy preserving attributes
mv old new                     # move/rename
rm -rf dir                     # delete (careful!)
ln -s /path/target linkname    # symbolic link
ln target hardlink             # hard link
stat file                      # detailed file info
file filename                  # detect file type
```

## Viewing files
```bash
cat file ; tac file            # print / reverse print
less file                      # pager (q to quit, /word to search)
head -n 20 file ; tail -n 20 file
tail -f /var/log/messages      # follow log live
wc -l file                     # count lines
```

## Finding files
```bash
find / -name "*.conf" 2>/dev/null
find /var -size +100M
find /home -user alice
find / -mtime -1               # modified in last day
find / -perm -4000             # SUID files
locate filename                # (updatedb first)
which ls ; whereis ls ; type ls
```

## Filesystem Hierarchy (FHS)
| Dir | Purpose |
|---|---|
| `/etc` | configuration files |
| `/var` | logs, spool, variable data |
| `/home` | user home dirs |
| `/root` | root's home |
| `/usr` | user programs, libs |
| `/boot` | kernel, GRUB |
| `/dev` | device files |
| `/proc`, `/sys` | kernel/process virtual FS |
| `/tmp` | temporary files |
| `/opt` | third-party software |

## Help
```bash
man ls ; info ls ; ls --help
man -k keyword                 # search man pages (mandb)
apropos keyword
```
