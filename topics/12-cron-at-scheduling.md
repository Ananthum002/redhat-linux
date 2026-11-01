# 12 - Scheduling: cron, at, systemd timers

## cron
```bash
crontab -e                     # edit my crontab
crontab -l                     # list
crontab -r                     # remove
crontab -u alice -e            # root edits user's
```
Format:
```
# min hour dom mon dow  command
  0   2    *   *   *    /scripts/backup.sh
  */5 *    *   *   *    /scripts/check.sh
  30  9    *   *   1-5  /scripts/report.sh
  0   0    1   *   *    /scripts/monthly.sh
  @reboot               /scripts/startup.sh
```
System locations:
```
/etc/crontab
/etc/cron.d/
/etc/cron.daily/  cron.hourly/  cron.weekly/  cron.monthly/
/var/spool/cron/<user>
```
Access control: `/etc/cron.allow`, `/etc/cron.deny`
```bash
systemctl status crond
grep CRON /var/log/cron
```

## at (one-time jobs)
```bash
at 14:30
at> /scripts/job.sh
at> Ctrl+D
at now + 2 hours
atq                            # list
atrm 3                         # remove job 3
systemctl enable --now atd
```

## anacron
`/etc/anacrontab` - runs missed jobs on machines not always on.

## systemd timer
`/etc/systemd/system/backup.timer`
```ini
[Timer]
OnCalendar=*-*-* 02:00:00
Persistent=true
[Install]
WantedBy=timers.target
```
```bash
systemctl enable --now backup.timer
systemctl list-timers
```

## logrotate
```bash
logrotate -d /etc/logrotate.conf    # dry run
logrotate -f /etc/logrotate.conf    # force
```
`/etc/logrotate.d/myapp`:
```
/var/log/myapp/*.log {
    weekly
    rotate 4
    compress
    missingok
    notifempty
}
```
