# Native-k8s(Ubuntu) - Dec 12 2023

## Version List&#x20;

Grab version list thru my own cluster. (+ a little version change)

{% embed url="https://github.com/sysnet4admin/IaC/tree/master/k8s/U/k8s-multicontext" %}

### 0.TL; DR: **`1.25.4`** Now Ready to Support&#x20;

```
root@cp-k8s:~# apt update
Get:1 https://download.docker.com/linux/ubuntu jammy InRelease [48.8 kB]
<snipped>
```

### 1. kubelet

```bash
root@m-k8s:~# apt list kubelet -a | head 

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

Listing...
kubelet/kubernetes-xenial 1.25.4-00 amd64 [upgradable from: 1.23.5-00]
kubelet/kubernetes-xenial 1.25.3-00 amd64
kubelet/kubernetes-xenial 1.25.2-00 amd64
kubelet/kubernetes-xenial 1.25.1-00 amd64
kubelet/kubernetes-xenial 1.25.0-00 amd64
kubelet/kubernetes-xenial 1.24.8-00 amd64
kubelet/kubernetes-xenial 1.24.7-00 amd64
kubelet/kubernetes-xenial 1.24.6-00 amd64
kubelet/kubernetes-xenial 1.24.5-00 amd64
```

### 2. docker-ce&#x20;

```bash
root@m-k8s:~# apt list docker-ce -a | head

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

Listing...
docker-ce/focal 5:20.10.21~3-0~ubuntu-focal amd64 [upgradable from: 5:20.10.14~3-0~ubuntu-focal]
docker-ce/focal 5:20.10.20~3-0~ubuntu-focal amd64
docker-ce/focal 5:20.10.19~3-0~ubuntu-focal amd64
docker-ce/focal 5:20.10.18~3-0~ubuntu-focal amd64
docker-ce/focal 5:20.10.17~3-0~ubuntu-focal amd64
docker-ce/focal 5:20.10.16~3-0~ubuntu-focal amd64
docker-ce/focal 5:20.10.15~3-0~ubuntu-focal amd64
docker-ce/focal,now 5:20.10.14~3-0~ubuntu-focal amd64 [installed,upgradable to: 5:20.10.21~3-0~ubuntu-focal]
docker-ce/focal 5:20.10.13~3-0~ubuntu-focal amd64
```

### 3. containerd.io

```bash
root@m-k8s:~# apt list containerd.io -a | head 

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

Listing...
containerd.io/focal 1.6.10-1 amd64 [upgradable from: 1.5.11-1]
containerd.io/focal 1.6.9-1 amd64
containerd.io/focal 1.6.8-1 amd64
containerd.io/focal 1.6.7-1 amd64
containerd.io/focal 1.6.6-1 amd64
containerd.io/focal 1.6.4-1 amd64
containerd.io/focal,now 1.5.11-1 amd64 [installed,upgradable to: 1.6.10-1]
containerd.io/focal 1.5.10-1 amd64
containerd.io/focal 1.4.13-1 amd64
```

{% hint style="info" %}
&#x20;May change after few days or later.&#x20;
{% endhint %}



