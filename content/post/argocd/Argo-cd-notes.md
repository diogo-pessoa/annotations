---
title: "ArgoCD - notes"
date: 2025-07-06
featured: false
draft: false
toc: true
categories:
  - automation
tags:
  - argocd
  - gitops
  - k8s
series:
  - argocd
---


### How ArgoCD runs helm charts

Why? Argo CD takes the philosophical approach of only working with “RAW Kubernetes manifests” directly. This means that
Argo CD wants to “own” the manifests and not have to rely on trying to interface with another tool. Argo CD achieves
this by doing the equivalent of running: helm template <options> | kubectl apply -f -
From ArgoCd Up and Running link