---
title:    "Linux ulimit"
date:     2026-01-15T10:00:01Z
draft:    true
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

---


````md
## `ulimit` — process limits & hidden constraints

Used when:
- processes fail mysteriously
- services won’t start under load
- errors mention files, memory, or threads

---

### Check current limits (shell)
```bash
ulimit -a
````

Shows:

* open files
* max processes
* stack size
* core dump settings

---

### Check a specific limit

#### Open files

```bash
ulimit -n
```

#### Max user processes

```bash
ulimit -u
```

---

### Temporarily increase limits (current shell only)

```bash
ulimit -n 65535
ulimit -u 65535
```

⚠️ Applies **only to the current shell and child processes**.

---

### Check limits for a running process

```bash
cat /proc/<PID>/limits
```

Best way to debug:

> “Why does this service hit limits but my shell doesn’t?”

---

### Common failure symptom (too many open files)

```bash
lsof -p <PID> | wc -l
```

If this approaches `ulimit -n`, you’ve found the problem.

---

### Permanent limits (system-wide)

Edit:

```bash
/etc/security/limits.conf
```

Example:

```conf
* soft nofile 65535
* hard nofile 65535
```

Or per user:

```conf
appuser soft nofile 65535
appuser hard nofile 65535
```

---

### Verify PAM limits are enabled

```bash
grep pam_limits /etc/pam.d/common-session
```

Without this, `limits.conf` is ignored.

---

### Systemd services (important)

Systemd ignores shell limits.

Check:

```bash
systemctl show myservice | grep LimitNOFILE
```

Override:

```bash
systemctl edit myservice
```

Add:

```ini
[Service]
LimitNOFILE=65535
```

Then:

```bash
systemctl daemon-reexec
systemctl restart myservice
```

---