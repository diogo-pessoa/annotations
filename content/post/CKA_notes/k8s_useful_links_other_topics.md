---
title: "K8S - other notes & useful links"
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

#### Upgrading Kubernetes clusters:

kubeadm [page](https://kubernetes.io/docs/reference/setup-tools/kubeadm/)

docs [operators pattern](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/)

control plane pod manifests:

`/etc/kubernetes/manifests/`

[k8s docs](https://kubernetes.io/docs/concepts/overview/components/)

components in the control plane:

* kube-apiserver
* kube-scheduler
* etcd database
* kube-controller manager
* CoreDns



![ControlPlane-workerNodesDiagram.png](../../_resources/ControlPlane-workerNodesDiagram.png)

