---
title:    "Linux commands cheatsheet"
date:     2026-01-15T10:00:01Z
draft:    true
featured: true
toc:      true

categories:
  - troubleshooting
  - linux
tags:
  - linux
  - bash
  - troubleshooting
  - sysAdmin
  - cheatsheet

---

# Abstract

Collection of commands used on daily tasks as a sysAdmin


## Files and Log Parsing


### grep
Case-insensitive search with line numbers.
```bash
grep -in "error" application.log
```

Recursive search across directories.
```bash
grep -R "timeout" /var/log/
```

Exclude noisy lines.
```bash
grep "ERROR" app.log | grep -v "healthcheck"
```

Show context around a match (useful for stack traces).
```bash
grep -C 5 "Exception" app.log
```

Multiple patterns (OR).
```bash
grep -E "ERROR|FATAL|PANIC" app.log
```



### find
Find files modified in the last 24 hours.
```bash
find /var/log -type f -mtime -1
```
Find large files (over 1GB).
```bash
find / -type f -size +1G 2>/dev/null
```
Find files by name (case-insensitive).
```bash
find . -iname "*backup*"
```
Find and delete old logs (careful!).
```bash
find /var/log -type f -mtime +30 -delete
```

### awk

Print specific columns.
```bash
awk '{print $1, $7}' access.log
```

Filter based on column value.
```bash
awk '$9 >= 500 {print $0}' access.log
```

Count occurrences.
```bash
awk '{print $1}' access.log | sort | uniq -c | sort -nr
```

Custom delimiter (e.g. CSV).
```bash
awk -F',' '{print $2, $5}' data.csv
```

### jq

Pretty-print JSON.
```bash
cat log.json | jq .
```

Extract fields.
```bash
jq '.timestamp, .level, .message' log.json
```

Filter based on value.
```bash
jq 'select(.level=="error")' log.json
```

Count entries.
```bash
jq -s 'length' log.json
```


### sed

# Replace a string globally in a file (in-place)
sed -i 's/old_value/new_value/g' config.conf

# Print only matching lines (grep-like)
sed -n '/ERROR/p' app.log

# Print a specific line range
sed -n '100,150p' app.log

# Remove empty lines
sed '/^$/d' app.log

# Mask secrets before sharing logs
sed 's/password=[^ ]*/password=REDACTED/g' app.log


### Combining grep,sed and awk

Find errors and extract timestamps + message
grep -i "error" app.log | awk '{print $1, $2, $NF}'


count errors by type

grep "ERROR" app.log | awk '{print $NF}' | sort | uniq -c | sort -nr

Extract request paths from access logs (ignoring health checks)
grep -v "health" access.log | awk '{print $7}' | sort | uniq -c | sort -nr


show only slow requests

awk '$NF > 1 {print $0}' access.log



Find top IPs causing 5xx errors

grep " 5[0-9][0-9] " access.log | awk '{print $1}' | sort | uniq -c | sort -nr

Extract stack traces

grep -n "Exception" app.log | sed -n 's/:.*//p' | while read n; do sed -n "$((n-5)),$((n+10))p" app.log; done

Remove timestamps 

sed 's/^[0-9:-]\+ //' app.log | grep -v "DEBUG"



mask secrets 

grep -i "password" app.log | sed 's/\(password=\)[^ ]*/\1REDACTED/g'


Show Errors grouped by minute

grep "ERROR" app.log | awk '{print $1 ":" $2}' | cut -c1-16 | sort | uniq -c


Extract unique error messages

grep "ERROR" app.log | sed 's/^[^ ]* [^ ]* //' | sort | uniq


find requests with missing fields

awk 'NF < 9 {print NR ":" $0}' access.log

compare to two logs without timestamps

sed 's/^[^ ]* [^ ]* //' log1.log | diff - <(sed 's/^[^ ]* [^ ]* //' log2.log)


## `diff` — spotting what changed

Used when things *worked before* and now don’t.

---

### Basic file comparison

Human readeable
```bash
diff -u file1.conf file2.conf
``` 

Ignore whitespaces

```bash
diff -uw file1.conf file2.conf

```

compare directories
```bash

diff -r /etc/app-old /etc/app-new
```

Only changed files
```bash
diff -qr dir1 dir2
```

Ignoring timestamps or headers
```bash
diff <(sed '1,2d' log1.log) <(sed '1,2d' log2.log)

```

side by side comparison

```bash
diff -y file1.conf file2.conf

```

comparing command outputs

```bash
diff <(ps aux | sort) <(ps aux | sort)

```

### tar

#### Create a compressed archive
```bash
tar -czvf logs.tar.gz /var/log/app/
```
#### Extract
```bash
tar -xzvf logs.tar.gz
```

#### List contents without extracting
```bash
tar -tzvf logs.tar.gz
```
#### Extract a single file
```bash
tar -xzvf logs.tar.gz path/to/file.log
```

## Network

### nc

Test if a port is open.
```bash
nc -zv example.com 443
```

#### Opening a Listener
Listen on a port (debug incoming connections).
```bash
nc -l 9000
```

#### Querying
Send a raw HTTP request.
```bash
echo -e "GET / HTTP/1.1\nHost: example.com\n\n" | nc example.com 80
```


### dig

Resolve A records for a domain.
```bash
dig example.com A +short
```

Query a specific DNS server.
```bash
dig @8.8.8.8 example.com A
```

Trace the DNS resolution path.
```bash
dig example.com +trace
```

### nslookup

Lookup a domain (default resolver).
```bash
nslookup example.com
```

Query a specific DNS server.
```bash
nslookup example.com 1.1.1.1
```

### ss (new)

# Show listening ports
ss -lntp

# Show established connections
ss -ant

# Filter by port
ss -ant '( sport = :443 )'

# Show socket summary
ss -s


### netstat

```bash 
netstat -tulpn
#t → TCP
#u → UDP
#l → listening
#p → process
#n → numeric (no DNS)
```

#### Show active connections
```bash
netstat -ant

```

#### filter by port 

```bash
netstat -ant | grep ":443"

```

#### Show by TCP state

```bash

netstat -ant | awk '{print $6}' | sort | uniq -c

```

#### Check process by port

```bash
netstat -tulpn | grep ":8080"


```

#### show routing table

```bash

netstat -rn

```

#### Show interface stats

```bash
netstat -i
```


### ip
#  Show interfaces
ip addr show

# Show routing table
ip route show

# Bring an interface up/down
ip link set eth0 down
ip link set eth0 up

# Check which route will be used
ip route get 8.8.8.8
