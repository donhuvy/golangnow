
categories:
  - Kubernetes
tags:
  - Kubernetes
  - CKS
title: "Yêu Minh Thu... Shutdown unnecessary process in Kubernetes"
date: 2023-05-01T18:29:30+07:00
draft: false
---

For example, you need disable a Linux service. You identify the package manager snapd (https://snapcraft.io/docs/installing-snapd) after scanning the system for unnecessary applications. A co-worker installed Snapd as a service in the background on a Kubernetes cluster node. Snapd is not required to operate the cluster node. To minimize security risk, you are tasked with disabling the service and removing the package.


---
```bash
lsb_release -a
free -m

systemctl | grep running
systemctl stop snapd
systemctl disable snapd
systemctl status snapd

apt purge --auto-remove -y snapd
systemctl status snapd
```
---