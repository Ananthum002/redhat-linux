# 17 - Shell Scripting (Bash)

## Skeleton
```bash
#!/bin/bash
set -euo pipefail              # strict mode
echo "Hello $USER"
```
```bash
chmod +x script.sh
./script.sh arg1 arg2
bash -x script.sh              # debug
```

## Variables
```bash
name="Alice"
echo "$name"
readonly PI=3.14
today=$(date +%F)
count=$((5 + 3))
read -p "Enter: " input
export VAR=value
```
Special: `$0` script, `$1..$9` args, `$#` count, `$@` all args, `$?` last exit code, `$$` PID.

## Conditions
```bash
if [ "$a" -gt 5 ]; then
    echo "big"
elif [ -f /etc/hosts ]; then
    echo "file exists"
else
    echo "no"
fi

[[ "$s" == "abc" ]] && echo yes
```
Tests: `-f` file, `-d` dir, `-e` exists, `-r/-w/-x`, `-z` empty str, `-n` non-empty, `-eq -ne -gt -lt -ge -le`.

## case
```bash
case "$1" in
    start) echo "starting" ;;
    stop)  echo "stopping" ;;
    *)     echo "usage: $0 {start|stop}" ;;
esac
```

## Loops
```bash
for i in 1 2 3; do echo $i; done
for i in {1..10}; do echo $i; done
for f in /var/log/*.log; do echo "$f"; done
while read -r line; do echo "$line"; done < file
i=0; while [ $i -lt 5 ]; do ((i++)); done
until ping -c1 host &>/dev/null; do sleep 2; done
```

## Functions
```bash
backup() {
    local src=$1
    tar -czf /backup/$(basename "$src").tgz "$src"
    return 0
}
backup /etc
```

## Arrays
```bash
arr=(a b c)
echo "${arr[0]}" "${arr[@]}" "${#arr[@]}"
```

## Error handling
```bash
command || { echo "failed"; exit 1; }
trap 'echo cleanup' EXIT
```

## Example: disk alert
```bash
#!/bin/bash
usage=$(df / | awk 'NR==2 {gsub("%","",$5); print $5}')
[ "$usage" -gt 80 ] && echo "Disk ${usage}% full" | mail -s "ALERT" admin@example.com
```
