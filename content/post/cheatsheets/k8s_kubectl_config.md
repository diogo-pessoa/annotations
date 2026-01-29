---
title:              "K8S - kube-config cmds"
date:               2025-02-04T11:25:54Z
draft:              false
featured:           false
toc:                false
codeMaxLines:       10
codeLineNumbers:    false
figurePositionShow: true
categories:
  - troubleshooting
tags:
  - k8s
  - sysAdmin
  - troubleshooting
---

Kubernetes kubectl config snippets.

### Use context

```bash
kubectl config use-context kubernetes-admin@kubernetes
```

### Set current context namespace

```bash
kubectl config set-context --current --namespace=application1
```

### Working with multiple contexts

- [Authenticating Across Clusters with kubeconfig](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/)

```bash
# Show merged kubeconfig settings.
kubectl config view

# Use multiple kubeconfig files at the same time and view merged config.
KUBECONFIG=~/.kube/config:~/.kube/kubconfig2
kubectl config view
```

### View password

```bash
# Get the password for the e2e user.
kubectl config view \
  -o jsonpath='{.users[?(@.name == "e2e")].user.password}'
```

### View certificate for e2e user

```bash
# Get the certificate for the e2e user.
kubectl config view --raw \
  -o jsonpath='{.users[?(@.name == "e2e")].user.client-certificate-data}' \
  | base64 -d
```

### Get users

#### Display the first user

```bash
kubectl config view -o jsonpath='{.users[].name}'
```

#### List users

```bash
kubectl config view -o jsonpath='{.users[*].name}'
```
