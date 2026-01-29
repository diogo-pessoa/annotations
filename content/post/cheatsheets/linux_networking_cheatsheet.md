---
title:    "Linux `networking tools` cheatsheet"
date:     2026-01-29
draft:    true
featured: false
toc:      true

categories:
  - troubleshooting
tags:
  - linux
  - bash
  - commands
  - troubleshooting
  - sysAdmin
  - cheatsheet
series:
  - linux_cheatsheet
---
## `nc`

### Test if a port is open.
```bash
$ nc -zv 172.20.10.3 22
  Connection to 172.20.10.3 port 22 [tcp/ssh] succeeded! 
$ ssh vboxuser@172.20.10.3
vboxuser@172.20.10.3's password: 
Welcome to Ubuntu 25.10 (GNU/Linux 6.17.0-8-generic x86_64)
```

### Opening a Listener
Listen on a port (debug incoming connections).
```bash
nc -l 9000
```

### Querying
Send a raw HTTP request.
```bash
echo -e "GET / HTTP/1.1\nHost: localhost\n\n" | nc localhost 1313
```

## `dig`

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
