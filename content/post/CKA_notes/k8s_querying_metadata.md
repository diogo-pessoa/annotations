---
title: "K8S - kubectl querying metadata"
date: 2025-02-04T11:25:54Z
draft: false
featured: false
toc: false

codeMaxLines: 10
codeLineNumbers: false
figurePositionShow: true
categories:
  - k8s
tags:
  - k8s
  - metadata
  - query
  - cheatsheet

---

#### Sorting  resources by creation timestamp

```bash 
k get pod -A --sort-by=.metadata.creationTimestamp
```
---
#### All namespaces

```bash
 --all-namespaces & kubectl -A
 ```
---

#### List pods Sorted by Restart Count

```bash
kubectl get pods --sort-by='.status.containerStatuses[0].restartCount' 
```
---

#### List Services Sorted by Name
```bash 
kubectl get services --sort-by='.metadata.name'
```

---
#### List PersistentVolumes sorted by capacity

```bash
kubectl get pv --sort-by=.spec.capacity.storage
```