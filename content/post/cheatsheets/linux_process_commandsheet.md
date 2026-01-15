---
title:    "Linux commands checking process information"
date:     2026-01-15T10:00:01Z
draft:    true
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

Cheatsheet specific to checking process information

````md
## Processes & process stats — what is running and why?

Used when the system is slow, stuck, or behaving strangely — and you need to understand **what is consuming resources right now**.

---

### `top` — real-time process overview

```bash
top
````

Key interactions:

* `P` → sort by CPU
* `M` → sort by memory
* `k` → kill a process
* `1` → show per-core CPU usage

Good for:

* quick system health checks
* spotting runaway processes

---

### `htop` — interactive and human-friendly

```bash
htop
```

Advantages over `top`:

* mouse support
* tree view (parent/child processes)
* easier sorting and filtering
* visual CPU & memory bars

Common actions:

* `F3` → search
* `F5` → tree view
* `F9` → kill process

Best when:

* diagnosing complex process trees
* teaching or collaborating during incidents

---

### `ps aux` — snapshot of everything

```bash
ps aux
```

Breakdown:

* `a` → all users
* `u` → user-oriented format
* `x` → processes without TTY

---

### Find top CPU consumers

```bash
ps aux --sort=-%cpu | head
```

### Find top memory consumers

```bash
ps aux --sort=-%mem | head
```

---

### Search for a specific process

```bash
ps aux | grep my_service | grep -v grep
```

Classic, simple, effective.

---

### Show process tree

```bash
ps auxf
```

Useful for:

* understanding forks
* debugging supervisors / workers
* spotting orphaned processes

---

### `lsof` — what processes have open?

When things won’t restart, delete, or release resources.

---

### Files opened by a process

```bash
lsof -p 1234
```

---

### Processes using a file

```bash
lsof /var/log/app.log
```

---

### Processes listening on a port

```bash
lsof -i :8080
```

---

### Find deleted files still consuming disk

```bash
lsof | grep deleted
```

Critical during:

* disk full incidents
* log rotation failures

---

### Combine process + file/network inspection

```bash
ps aux | grep nginx
lsof -p <PID>
```

Answers:

> “What is this process actually doing?”

---

### Watch process behavior over time

```bash
watch -n 1 "ps aux --sort=-%cpu | head"
```

Simple live monitoring without dashboards.

---

### Kill a runaway process (last resort)

```bash
kill -9 <PID>
```






