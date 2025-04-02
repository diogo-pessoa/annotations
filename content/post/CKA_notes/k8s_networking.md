---
title: "K8S - Networking references"
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
  - networking
  - references

---

• Understand host networking configuration on the cluster nodes
• Understand connectivity between Pods
• Understand ClusterIP, NodePort, LoadBalancer service types and endpoints
• Know how to use Ingress controllers and Ingress resources
• Know how to configure and use CoreDNS
• Choose an appropriate container network interface plugin


[docs](https://kubernetes.io/docs/concepts/cluster-administration/networking/)

[ilustrated guide to k8s networking](https://speakerdeck.com/thockin/illustrated-guide-to-kubernetes-networking)

[Container networking interface](https://github.com/containernetworking/cni)

[Addons](https://kubernetes.io/docs/concepts/cluster-administration/addons/)

[Cluster Networking](https://kubernetes.io/docs/concepts/cluster-administration/networking/)


Some frameworks to enable and configure networking:
>Basically, all IPs involved (nodes and pods) are routable without NAT. 
This can be achieved at the physical network infrastructure if you have access to it (e.g. GKE). 
Or, this can be achieved with a software defined overlay with solutions like:
>Weave
>Flannel
>Calico
>Romana