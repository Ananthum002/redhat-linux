# 09 - SSH & Remote Access

## Connect
```bash
ssh user@host
ssh -p 2222 user@host
ssh -i ~/.ssh/key.pem user@host
ssh user@host 'uptime'         # run remote command
ssh -X user@host               # X11 forwarding
```

## Key-based auth
```bash
ssh-keygen -t ed25519 -C "me@host"
ssh-copy-id user@host
ssh-add ~/.ssh/id_ed25519      # after: eval $(ssh-agent)
```
Permissions: `~/.ssh` = 700, `authorized_keys` = 600.

## Tunnels
```bash
ssh -L 8080:localhost:80 user@host      # local forward
ssh -R 9090:localhost:3000 user@host    # remote forward
ssh -D 1080 user@host                   # SOCKS proxy
ssh -J jump@bastion user@internal       # jump host
```

## Client config `~/.ssh/config`
```
Host web1
    HostName 10.0.0.10
    User admin
    Port 22
    IdentityFile ~/.ssh/id_ed25519
```

## Server hardening `/etc/ssh/sshd_config`
```
Port 2222
PermitRootLogin no
PasswordAuthentication no
AllowUsers alice bob
MaxAuthTries 3
ClientAliveInterval 300
```
```bash
sshd -t                        # test config
systemctl restart sshd
# if changing port with SELinux + firewall:
semanage port -a -t ssh_port_t -p tcp 2222
firewall-cmd --permanent --add-port=2222/tcp && firewall-cmd --reload
```

## File transfer
```bash
scp file user@host:/tmp/
scp -r dir user@host:/tmp/
scp user@host:/var/log/messages .
rsync -avz --progress src/ user@host:/dest/
rsync -avz --delete src/ dest/
sftp user@host
```
