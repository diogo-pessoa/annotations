---
title:    "Linux filesystem commands"
date:     2026-01-15T10:00:01Z
draft:    true
featured: false
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

## Disk & filesystem checks

Used during:
- disk full incidents
- unexplained application crashes
- “no space left on device” errors
- stuck log rotation


### `df -h` — disk usage by filesystem

```bash
df -h
````

Shows:

* total size
* used / available space
* mount points

---

### Check a specific mount

```bash
df -h /var
```

Quick answer to:

> “Is *this* filesystem full or another one?”

---

### `df -i` — inode exhaustion (silent killer)

```bash
df -i
```

When disk looks empty but errors say:

> `No space left on device`

This is often the reason.

---

### Identify inode-heavy directories

```bash
find /var -xdev -type f | wc -l
```

---

### Find directories with many small files

```bash
for d in /var/*; do echo -n "$d: "; find "$d" -type f | wc -l; done
```

Common offenders:

* mail spools
* temp directories
* cache folders

---

### `du` — where the space actually went

```bash
du -sh /*
```

High-level overview (may be slow on large systems).

---

### Drill down into a directory

```bash
du -sh /var/* | sort -h
```

Finds:

* log explosions
* forgotten backups
* cache growth

---

### Find large files

```bash
find / -type f -size +1G 2>/dev/null
```

Use carefully on production systems.

---

### Check mounted volumes

```bash
mount
```

Or, more readable:

```bash
findmnt
```

Answers:

> “Is this volume even mounted?”

---

### Check filesystem type & options

```bash
mount | grep /data
```

Useful for:

* read-only mounts
* `noexec`, `nosuid`, `nodev` surprises

---

### Verify persistent mounts

```bash
cat /etc/fstab
```

Critical after:

* reboots
* instance recreation
* disk reattachment

---

### Detect deleted files still using disk

```bash
lsof | grep deleted
```

Space won’t be freed until the process exits.

---

### Check block devices

```bash
lsblk
```

Shows:

* disks
* partitions
* mount points

Excellent for:

* cloud instances
* newly attached volumes

---

### Check filesystem usage per mount namespace

```bash
df -hT
```

Adds filesystem type (ext4, xfs, tmpfs, etc).

---

### Emergency cleanup example

```bash
du -sh /var/log/* | sort -h
```

Then:

```bash
> big.log
```

(Truncates without deleting the file)
