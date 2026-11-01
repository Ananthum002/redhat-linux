# 02 - File Permissions, Ownership, ACL

## Basics
```
-rwxr-xr-- 1 alice dev 1024 Jan 1 file
 u  g  o       owner group
r=4 w=2 x=1
```

```bash
chmod 755 file                 # rwxr-xr-x
chmod u+x,g-w,o-r file         # symbolic
chmod -R 750 /dir              # recursive
chown alice file
chown alice:dev file
chown -R alice:dev /dir
chgrp dev file
```

## Special permissions
```bash
chmod u+s file                 # SUID (4000)
chmod g+s dir                  # SGID (2000) - inherit group
chmod +t dir                   # Sticky bit (1000) - e.g. /tmp
chmod 2775 /shared
```

## umask
```bash
umask                          # show current
umask 027                      # new files 640, dirs 750
```
Persistent: set in `/etc/profile` or `~/.bashrc`.

## ACL (fine-grained)
```bash
getfacl file
setfacl -m u:bob:rw file       # give user bob rw
setfacl -m g:dev:rx dir
setfacl -R -m u:bob:rwx dir
setfacl -d -m u:bob:rwx dir    # default ACL for new files
setfacl -x u:bob file          # remove entry
setfacl -b file                # remove all ACLs
```

## Attributes
```bash
lsattr file
chattr +i file                 # immutable
chattr -i file
chattr +a file                 # append only
```
