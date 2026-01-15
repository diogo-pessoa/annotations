---
title: "K8S - aliases & env exports for local terminal"
date: 2025-02-04T11:40:54Z
draft: true
featured: false
toc: false

codeMaxLines: 10
codeLineNumbers: false
figurePositionShow: true
categories:
  - k8s
  - troubleshooting
tags:
  - k8s
  - aliases
  - cheatsheet
  - sysAdmin

---

### Exports
```bash
# will already be pre-configured:
alias k=kubectl 
```

### dry-run  client and server
```bash

export doc="--dry-run=client -o yaml"   
export dos="--dry-run=server -o yaml"   

# usage
deploy nginx --image=nginx $do
```

* force deletion alias

```bash
export now="--force --grace-period 0"
# k delete pod x $now
```

### .vimrc for better yaml navigation

```bash 
set tabstop=2
set expandtab
set shiftwidth=2
set number
```