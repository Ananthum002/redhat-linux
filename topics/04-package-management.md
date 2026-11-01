# 04 - Package Management (DNF / YUM / RPM)

## DNF
```bash
dnf install httpd -y
dnf remove httpd
dnf update ; dnf upgrade
dnf update httpd
dnf search nginx
dnf info httpd
dnf list installed
dnf list available | grep php
dnf provides /usr/bin/dig      # which package gives this file
dnf whatprovides "*/semanage"
dnf check-update
dnf autoremove
dnf clean all ; dnf makecache
dnf history ; dnf history undo 5
dnf group list
dnf groupinstall "Development Tools"
dnf module list
dnf module enable nodejs:18
dnf module install nodejs:18
```

## Repositories
```bash
dnf repolist ; dnf repolist all
dnf config-manager --add-repo https://example.com/my.repo
dnf config-manager --set-enabled powertools
dnf --enablerepo=epel install htop
dnf install epel-release
```
Manual repo file `/etc/yum.repos.d/local.repo`:
```ini
[local]
name=Local Repo
baseurl=file:///mnt/repo
enabled=1
gpgcheck=0
```

## RPM
```bash
rpm -ivh pkg.rpm               # install
rpm -Uvh pkg.rpm               # upgrade
rpm -e pkgname                 # erase
rpm -qa                        # all installed
rpm -qi pkgname                # info
rpm -ql pkgname                # files in package
rpm -qc pkgname                # config files
rpm -qf /etc/hosts             # package owning file
rpm -qR pkgname                # dependencies
rpm -qp pkg.rpm -l             # inspect uncompressed rpm
rpm -V pkgname                 # verify
rpm --import RPM-GPG-KEY
```

## Subscription (RHEL)
```bash
subscription-manager register --username USER --password PASS
subscription-manager attach --auto
subscription-manager repos --list
subscription-manager repos --enable=rhel-9-for-x86_64-appstream-rpms
subscription-manager status
subscription-manager unregister
```

## Create local repo
```bash
dnf install createrepo
createrepo /opt/myrepo
```
