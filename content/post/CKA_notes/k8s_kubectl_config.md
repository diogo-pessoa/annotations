---
title: "K8S - kube-config cmds"
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
---

k8s kubectl config

### use-context

```
kubectl config use-context kubernetes-admin@kubernetes
```


#### setting current context namespace


```
kubectl config set-context --current --namespace=application1
```

### Working with multiple contexts

* [Authenticating Across Clusters with kubeconfig](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/)

```bash
# Show Merged kubeconfig settings.
kubectl config view 

# use multiple kubeconfig files 
#at the same time and view merged config
KUBECONFIG=~/.kube/config:~/.kube/kubconfig2
```

### config  view password

```bash
# get the password for the e2e user
kubectl config view \
-o jsonpath='{.users[?(@.name == "e2e")].user.password}'
```

### config view certificate for e2e User
```bash
# get the certificate for the e2e user
kubectl config view --raw \
-o jsonpath='{.users[?(.name == 'e2e')].user.client-certificate-data}'\
| base64 -d
```


### config get users

##### display the first user

```bash kubectl config view -o jsonpath='{.users[].name}'```

##### list users
```bash kubectl config view -o jsonpath='{.users[*].name}' ```