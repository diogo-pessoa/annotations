---
title:    "Linux `LogParsers` cheatsheet"
date:     2026-01-15
draft:    false
featured: false
toc:      true

categories:
  - troubleshooting
tags:
  - linux
  - bash
  - troubleshooting
  - sysAdmin
  - cheatsheet
  - commands
series:
  - linux_cheatsheet


---

## `grep`
### Case-insensitive search with line numbers.
```bash
grep -in "error" application.log
```

###  Recursive search across directories.
```bash
grep -R "timeout" /var/log/
```

###  Exclude noisy lines.
```bash
grep "ERROR" app.log | grep -v "healthcheck"
```

###  Show context around a match (useful for stack traces).
```bash
grep -C 5 "Exception" app.log
grep -A 5 "Exception" app.log # 5 Lines only after
grep -B 5 "Exception" app.log # 5 Lines only Before
```

###  Multiple patterns (OR).
```bash
grep -E "ERROR|FATAL|PANIC" app.log
```
----

## `find`
###  Find files modified in the last 24 hours.
```bash
find /var/log -type f -mtime -1
```
###  Find large files (over 1GB).
```bash
find / -type f -size +1G 2>/dev/null
```
* Find files by name (case-insensitive).
```bash
find . -iname "### backup*"
```
* Find and delete old logs `(careful!)`.
```bash
find /var/log -type f -mtime +30 -delete
```
-----

## `awk`

###  Print specific columns.
```bash
awk '{print $1, $7}' access.log
```

###  Filter based on column value.
```bash
awk '$9 >= 500 {print $0}' access.log
```

###  Count occurrences.
```bash
awk '{print $1}' access.log | sort | uniq -c | sort -nr
```

###  Custom delimiter (e.g. CSV).
```bash
awk -F',' '{print $2, $5}' data.csv
```

-----

## `jq`

###  Pretty-print JSON.
```bash
cat log.json | jq .
```

###  Extract fields.
```bash
jq '.timestamp, .level, .message' log.json
```

###  Filter based on value.
```bash
jq 'select(.level=="error")' log.json
```

###  Count entries.
```bash
jq -s 'length' log.json
```
----

## `sed`

###  Replace a string globally in a file (in-place)

```bash
sed -i 's/old_value/new_value/g' config.conf
```

###  Print only matching lines (grep-like)
```bash 
sed -n '/ERROR/p' app.log
````
###  Print a specific line range

```bash
sed -n '100,150p' app.log
```

###  Remove empty lines
```bash
sed '/^$/d' app.log
```

###  Mask secrets before sharing logs
```bash
sed 's/password=[^ ]*/password=REDACTED/g' app.log
```

## `diff`

### Human readeable
```bash
diff -u file1.conf file2.conf
``` 

### Ignore whitespaces

```bash
diff -uw file1.conf file2.conf

```

### compare directories
```bash

diff -r /etc/app-old /etc/app-new
```

### Only changed files
```bash
diff -qr dir1 dir2
```

### Ignoring timestamps or headers
```bash
diff <(sed '1,2d' log1.log) <(sed '1,2d' log2.log)

```

### side by side comparison

```bash
diff -y file1.conf file2.conf

```

### comparing command outputs

```bash
diff <(ps aux | sort) <(ps aux | sort)

```


## Combining `grep,sed and awk`

### Find errors and extract timestamps + message
```bash
grep -i "error" app.log | awk '{print $1, $2, $NF}'
```


### count errors by type
```bash
grep "ERROR" app.log | awk '{print $NF}' | sort | uniq -c | sort -nr
```

### Extract request paths from access logs (ignoring health checks)
```bash
grep -v "health" access.log | awk '{print $7}' | sort | uniq -c | sort -nr
```

### show only slow requests
```bash
awk '$NF > 1 {print $0}' access.log
```

### Find top IPs causing 5xx errors
```bash
grep " 5[0-9][0-9] " access.log | awk '{print $1}' | sort | uniq -c | sort -nr
```

### Extract stack traces
```bash
grep -n "Exception" app.log | sed -n 's/:.*//p' | while read n; do sed -n "$((n-5)),$((n+10))p" app.log; done
```

### Remove timestamps 
```bash
sed 's/^[0-9:-]\+ //' app.log | grep -v "DEBUG"
```

### mask secrets 
```bash
grep -i "password" app.log | sed 's/\(password=\)[^ ]*/\1REDACTED/g'
```

### Show Errors grouped by minute
```bash
grep "ERROR" app.log | awk '{print $1 ":" $2}' | cut -c1-16 | sort | uniq -c
```

### Extract unique error messages
```bash
grep "ERROR" app.log | sed 's/^[^ ]* [^ ]* //' | sort | uniq
```

### find requests with missing fields
```bash
awk 'NF < 9 {print NR ":" $0}' access.log
```

### compare to two logs without timestamps
```bash
sed 's/^[^ ]* [^ ]* //' log1.log | diff - <(sed 's/^[^ ]* [^ ]* //' log2.log)
```

