---
title: "`Investigating process` information"
date: 2026-01-25
draft: false
toc: true

categories:
  - troubleshooting
tags:
  - linux
  - bash
  - troubleshooting
  - sysAdmin
  - cheatsheet
series:
  - linux_cheatsheet

---

## `top`

```bash
top
````

Key interactions:

* `P` → sort by CPU
* `M` → sort by memory
* `k` → kill a process
* `1` → show per-core CPU usage

---

## `htop`

```bash
htop
```

Common actions:

* `F3` → search
* `F5` → tree view
* `F9` → kill process

---

## `ps`

### ps aux - my to go-to identify process&users

```bash
ps aux
```

Breakdown:

* `a` → all users
* `u` → user-oriented format
* `x` → processes without TTY

### Find top `CPU` consumers

```bash
ps aux --sort=-%cpu | head
```

### Find top `memory` consumers

```bash
ps aux --sort=-%mem | head
```



### Search for a `specific process` (grep)

```bash
ps aux | grep my_service | grep -v grep
```



> The `grep -v grep` is to hide the grep process

### Show process `tree`

```bash
ps auxf
```

Useful for:

* understanding forks
* debugging supervisors / workers
* spotting orphaned processes



## `lsof`

### `Files opened` by a process

```bash
lsof -p 1234
```



### Proces using a file

```bash
lsof /var/log/app.log
```



### Processes `listening on a port`

```bash
lsof -i :8080
```



### Find `deleted files` still consuming disk

```bash
lsof | grep deleted
```

Critical during:

* disk full incidents
* log rotation failures



## `Kill`

### `Kill -9` (instant process interruption)

```bash
kill -9 <PID>
```

### `kill -15` (graceful shutdown)

```bash
kill -9 <PID>
```

## `strace`

## attach to process
```bash
sudo strace -p <PID>
strace: Process 3413 attached
ppoll([{fd=3, events=POLLIN}, {fd=4, events=POLLIN}], 2, NULL, [], 8

```

### attach to process showing more details

```bash
sudo strace -p 3413 -yy
ppoll([{fd=3<TCP:[0.0.0.0:22]>, events=POLLIN}, {fd=4<TCPv6:[[::]:22]>, events=POLLIN}], 2, NULL, [], 8
```

```bash 
strace --help | grep '\-y' -A 4
  -y, --decode-fds[=path]
                 print paths associated with file descriptor arguments
  -yy, --decode-fds=all
                 print all available information associated with file
                 descriptors in addition to paths
```


## systemctl

### general process inform `<status>`

```bash
systemctl status <sshd>

```



## Combining commands

### Combine process + `file/network` inspection - ` ps aux + lsof `

```bash
ps aux | grep nginx
lsof -p <PID>
```



### `Watch process` behavior over time

```bash
watch -n 1 "ps aux --sort=-%cpu | head"
```


## Putting it all together

Inspecting the `sshd` in a local vm.


#### systemctl status 

```bash
$ sudo systemctl status sshd
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; disabled; preset: enabled)
     Active: active (running) since Thu 2026-01-29 14:26:01 GMT; 12min ago
```

#### `strace`


```bash
$ sudo strace -p 3413
strace: Process 3413 attached
ppoll([{fd=3, events=POLLIN}, {fd=4, events=POLLIN}], 2, NULL, [], 8

```
* `ppoll(...)`  a “wait for I/O” syscall

#### lsof 
```bash
$ sudo lsof -p 3413 -a -d 3,4
lsof: WARNING: can't stat() fuse.portal file system /run/user/1000/doc
      Output information may be incomplete.
COMMAND  PID USER FD   TYPE DEVICE SIZE/OFF NODE NAME
sshd    3413 root 3u  IPv4  25880      0t0  TCP *:ssh (LISTEN)
sshd    3413 root 4u  IPv6  25882      0t0  TCP *:ssh (LISTEN)

```

### strace to show socket details

```bash
sudo strace -p 3413 -yy
strace: Process 3413 attached
ppoll([{fd=3<TCP:[0.0.0.0:22]>, events=POLLIN}, {fd=4<TCPv6:[[::]:22]>, events=POLLIN}], 2, NULL, [], 8
```