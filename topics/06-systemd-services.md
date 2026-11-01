# 06 - systemd: Services, Targets, Units

## Service control
```bash
systemctl start httpd
systemctl stop httpd
systemctl restart httpd
systemctl reload httpd
systemctl status httpd
systemctl enable httpd         # start at boot
systemctl enable --now httpd   # enable + start
systemctl disable httpd
systemctl mask httpd           # prevent starting
systemctl unmask httpd
systemctl is-active httpd
systemctl is-enabled httpd
```

## Listing
```bash
systemctl list-units --type=service
systemctl list-unit-files
systemctl --failed
systemctl list-dependencies httpd
systemctl list-timers
```

## Targets (runlevels)
```bash
systemctl get-default
systemctl set-default multi-user.target
systemctl isolate rescue.target
systemctl isolate graphical.target
```
| Target | Old runlevel |
|---|---|
| poweroff.target | 0 |
| rescue.target | 1 |
| multi-user.target | 3 |
| graphical.target | 5 |
| reboot.target | 6 |

## Power
```bash
systemctl reboot ; systemctl poweroff
shutdown -h now ; shutdown -r +10 "Rebooting"
shutdown -c                    # cancel
```

## Custom unit file `/etc/systemd/system/myapp.service`
```ini
[Unit]
Description=My App
After=network.target

[Service]
Type=simple
User=appuser
ExecStart=/opt/myapp/run.sh
Restart=on-failure

[Install]
WantedBy=multi-user.target
```
```bash
systemctl daemon-reload
systemctl enable --now myapp
systemctl edit httpd           # drop-in override
systemctl cat httpd
```

## Timer (cron alternative)
```bash
systemctl list-timers --all
systemctl enable --now dnf-makecache.timer
```

## Analyze boot
```bash
systemd-analyze
systemd-analyze blame
systemd-analyze critical-chain
```
