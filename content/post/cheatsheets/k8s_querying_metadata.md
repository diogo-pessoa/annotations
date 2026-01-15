---
title:              "K8S - kubectl querying metadata"
date:               2025-02-04T11:25:54Z
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
  - metadata
  - query
  - cheatsheet
  - troubleshooting
  - sysAdmin
---

### Sort resources by creation timestamp

```bash
kubectl get pods -A --sort-by=.metadata.creationTimestamp
```

### All namespaces

Use `--all-namespaces` or `-A` to query every namespace.

### List pods sorted by restart count

```bash
kubectl get pods --sort-by='.status.containerStatuses[0].restartCount'
```

### List services sorted by name

```bash
kubectl get services --sort-by='.metadata.name'
```

### List PersistentVolumes sorted by capacity

```bash
kubectl get pv --sort-by=.spec.capacity.storage
```
