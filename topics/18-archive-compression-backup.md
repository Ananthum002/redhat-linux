# 18 - Archive, Compression & Backup

## tar
```bash
tar -cvf archive.tar dir/            # create
tar -czvf archive.tar.gz dir/        # gzip
tar -cjvf archive.tar.bz2 dir/       # bzip2
tar -cJvf archive.tar.xz dir/        # xz
tar -tvf archive.tar.gz              # list
tar -xvf archive.tar                 # extract
tar -xzvf archive.tar.gz -C /dest    # extract elsewhere
tar -xzvf a.tgz path/to/file         # single file
tar --exclude='*.log' -czvf a.tgz dir/
```

## Compression
```bash
gzip file ; gunzip file.gz
bzip2 file ; bunzip2 file.bz2
xz file ; unxz file.xz
zip -r a.zip dir/ ; unzip a.zip
zcat file.gz ; zless file.gz
```

## rsync backup
```bash
rsync -av /data/ /backup/
rsync -avz --delete /data/ user@host:/backup/
rsync -av --exclude='tmp/' src/ dst/
rsync -avn src/ dst/                 # dry run
```

## dd / disk images
```bash
dd if=/dev/sda of=/backup/sda.img bs=4M status=progress
dd if=/dev/zero of=/file bs=1M count=100
dd if=disk.iso of=/dev/sdb bs=4M
```

## Checksums
```bash
sha256sum file ; sha256sum -c sums.txt
md5sum file
```

## XFS dump / restore
```bash
xfsdump -l 0 -f /backup/home.dump /home
xfsrestore -f /backup/home.dump /home
```

## ReaR (Relax-and-Recover)
```bash
dnf install rear
rear -v mkbackup
```
