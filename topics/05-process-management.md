# 05 - Process Management & Job Control

## Viewing
```bash
ps aux
ps -ef --forest
ps -u alice
pstree -p
top                            # q quit, k kill, M sort mem, P sort cpu
htop                           # (EPEL)
pgrep -a sshd
pidof httpd
```

## Killing & signals
```bash
kill PID                       # SIGTERM (15)
kill -9 PID                    # SIGKILL
kill -HUP PID                  # reload config
killall httpd
pkill -u alice
kill -l                        # list signals
```

## Priority
```bash
nice -n 10 command
renice -n 5 -p PID
renice -n -5 -u alice
```
Range: -20 (highest) to 19 (lowest).

## Job control
```bash
command &                      # run in background
jobs
fg %1 ; bg %1
Ctrl+Z                         # suspend
nohup ./script.sh &            # survive logout
disown %1
screen ; tmux                  # persistent sessions
tmux new -s work ; tmux attach -t work
```

## Inspecting a process
```bash
lsof -p PID                    # open files
lsof -i :80                    # who uses port 80
lsof +D /var/log
strace -p PID                  # syscalls
ls -l /proc/PID/
cat /proc/PID/status
```

## Tuned profiles
```bash
tuned-adm list
tuned-adm active
tuned-adm profile throughput-performance
```
