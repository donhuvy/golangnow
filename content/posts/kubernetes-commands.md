---
title: "Kubernetes commands"
date: 2023-04-27T15:01:53+07:00
draft: false
---

Kubernetes 1.25.4 on Docker for Windows (4.18.0) April 27th 2023

```bash
kubectl cluster-info
kubectl cluster-info dump
kubectl cluster-info dump > vy.json
code vy.json

kubectl proxy
kubectl get nodes

kubectl run nginx --image nginx
kubectl get pods
```

Kubernetes control plane: https://kubernetes.docker.internal:6443/

CoreDNS: https://kubernetes.docker.internal:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

Use command `kubectl proxy` for access services from outside world.

http://127.0.0.1:8001/apis/policy/v1

http://127.0.0.1:8001/healthz/poststarthook/kube-apiserver-autoregistration

```bash
kubectl apply -f hello-minikube.yaml
kubectl run hello-minikube
```

For learning: microk8s , minikube
https://kodekloud.com/pricing/


## Setup lab for practice
Practical at local: minikube.exe , kubectl --> Single node Kubernetes cluster (Virtual box)

VirtualBox 7.0.8 for Windows (latest at April 27th 2023) https://download.virtualbox.org/virtualbox/7.0.8/VirtualBox-7.0.8-156879-Win.exe (272 MB)

User manual https://www.virtualbox.org/manual/ch01.html#hostossupport/

Install Kubernetes on Linux https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/

Get latest version number at https://github.com/kubernetes/kubernetes

![vmbox.png](/img/2023_04_27_kubernetes/vmbox.png)

Ubuntu 23.04 https://releases.ubuntu.com/lunar/ubuntu-23.04-desktop-amd64.iso

https://hub.docker.com/_/redis

```bash
kubectl run redis --image redis
kubectl get pods
kubectl get nodes

kubectl run mongo --image mongo
```
No need pull docker image before, because internal process inside pod automatically download to local machine.

```bash
kubectl describe pod nginx
kubectl describe pod mongo
kubectl describe pod redis
```

Return more columns

```bash
kubectl get pods -o wide
```

## YAML file syntax

Key Value Pair

```yaml
FirstLove: Bich_Van
SecondLove: Phuong_Ly
ThirdLove: Minh_Thu
```

Array/List

```yaml
Lovers:
-   BichVan
-   PhuongLy
-   MinhThu

Stuff:
-   Nokia_G50
-   Nokia_23
-   Dell_Precision_7570
```

Dictionary / Map
```yaml
LoveAge:
    BichVan: 36
    PhuongLy: 29
    MinhThu:  35

Producers:
    Nokia_G50: Nokia
    Nokia23: Nokia
    Dell_Precision_7570: Dell
```

![yaml_space.png](/img/2023_04_27_kubernetes/yaml_space.png)

Dictionary: unordered

List: ordered (The order of item are different)

File `pod-definition.yml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: vy-pod
  labels:
    app: vy
    type: react-fe
spec:
  containers:
    - name: nginx-controller
      image: nginx
      resources:
        requests:
          memory: 128Mi
          cpu: "0.1"
        limits:
          memory: 256Mi
          cpu: "0.1"
# kubectl create -f vy.yaml
```

command
```bash
kubectl create -f vy.yml
kubectl describe pod vy
```

![kubernetes_create.png](/img/2023_04_27_kubernetes/kubernetes_create.png)

