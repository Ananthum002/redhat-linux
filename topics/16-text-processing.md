# 16 - Text Processing: grep, sed, awk, pipes

## Redirection & pipes
```bash
cmd > file           # overwrite stdout
cmd >> file          # append
cmd 2> err.log       # stderr
cmd > out 2>&1       # both
cmd &> all.log
cmd < input
cmd1 | cmd2
cmd | tee file       # display + save
cmd > /dev/null 2>&1
```

## grep
```bash
grep "error" file
grep -i "error" file           # ignore case
grep -r "pattern" /etc         # recursive
grep -n "pattern" file         # line numbers
grep -v "pattern" file         # invert
grep -c "pattern" file         # count
grep -E "a|b" file             # regex OR
grep -w "word" file
grep -A3 -B3 "error" file      # context
grep -l "pattern" *            # filenames only
```

## sed
```bash
sed 's/old/new/' file
sed 's/old/new/g' file
sed -i 's/old/new/g' file      # in place
sed -i.bak 's/a/b/' file       # with backup
sed -n '5,10p' file
sed '3d' file                  # delete line 3
sed '/^#/d' file               # delete comments
sed '/^$/d' file               # delete blank lines
sed '2i\new line' file         # insert before line 2
```

## awk
```bash
awk '{print $1}' file
awk -F: '{print $1,$3}' /etc/passwd
awk '$3 > 1000' /etc/passwd
awk '{sum+=$1} END {print sum}' file
awk 'NR==5' file
awk '/error/ {print $0}' file
df -h | awk 'NR>1 {print $5,$6}'
```

## Other tools
```bash
sort file ; sort -n ; sort -r ; sort -u
sort -k2 -t: file
uniq ; uniq -c                 # often: sort | uniq -c | sort -rn
cut -d: -f1,3 /etc/passwd
cut -c1-10 file
tr 'a-z' 'A-Z'
tr -d '\r'
paste f1 f2
diff file1 file2 ; diff -u
comm file1 file2
xargs                          # find . -name "*.tmp" | xargs rm
column -t
```

## vim essentials
```
i / a / o     insert mode          Esc    normal mode
:w  :q  :wq  :q!                   dd     delete line
yy  p                              /text  search
:%s/old/new/g                      u      undo
```
