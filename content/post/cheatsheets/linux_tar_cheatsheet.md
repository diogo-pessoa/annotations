---
title:    "`tar` cheatsheet"
date:     2026-01-28
draft:    false
featured: false
toc:      true

categories:
  - troubleshooting
  - linux
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

## Create a compressed archive
```bash
tar -czvf logs.tar.gz /var/log/app/
```
## Extract
```bash
tar -xzvf logs.tar.gz
```

## List contents without extracting
```bash
tar -tzvf logs.tar.gz
```
## Extract a single file
```bash
tar -xzvf logs.tar.gz path/to/file.log
```
