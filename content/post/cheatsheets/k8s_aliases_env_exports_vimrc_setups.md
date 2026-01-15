---
title:              "K8S - aliases & env exports for local terminal"
date:               2025-02-04T11:40:54Z
draft:              false
featured:           false
toc:                false
codeMaxLines:       10
codeLineNumbers:    false
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
# Pre-configured alias
alias k=kubectl
```

### Dry-run (client and server)

```bash
export doc="--dry-run=client -o yaml"
export dos="--dry-run=server -o yaml"

# Usage
deploy nginx --image=nginx $doc
```

### Force deletion alias

```bash
export now="--force --grace-period 0"
# k delete pod x $now
```

### .vimrc for better YAML navigation

```bash
set tabstop=2
set expandtab
set shiftwidth=2
set number
```
