---
description: >-
  Pixie runs entirely inside your Kubernetes clusters without storing any
  customer data outside. Avoid trading-off depth of visibility due to the hassle
  and cost of trucking petabytes of telemetry off-c
---

# Pixie

## 1. Install on k8s&#x20;

### 1.1. Install px binary &#x20;

```
root@bk8s-m:~# bash -c "$(curl -fsSL https://withpixie.ai/install.sh)"

  ___  _       _
 | _ \(_)__ __(_) ___
 |  _/| |\ \ /| |/ -_)
 |_|  |_|/_\_\|_|\___|

==> Info:
Pixie gives engineers access to no-instrumentation, streaming &
unsampled auto-telemetry to debug performance issues in real-time,
More information at: https://www.pixielabs.ai.

This command will install the Pixie CLI (px) in a location selected
by you, and performs authentication with Pixie's cloud hosted control
plane. After installation of the CLI you can easily manage Pixie
installations on your K8s clusters and execute scripts to collect
telemetry from your clusters using Pixie.

Docs:
  https://work.withpixie.ai/docs


==> Terms and Conditions https://www.pixielabs.ai/terms
I have read and accepted the Terms & Conditions [y/n]: 
<snipped>
```

```
root@bk8s-m:~# px auth login
Pixie CLI
Starting browser
Fetching refresh token ...
Failed to perform browser based auth. Will try manual auth error=browser failed to open

Please Visit: 
         https://work.withpixie.ai:443/login?local_mode=true

Copy and paste token here: <Auth Token>
```

{% hint style="warning" %}
If Self-hosted k8s may need to generate manually auth token and put the token for verification between pixie cloud and local k8s.
{% endhint %}

![](<../../../.gitbook/assets/image (1).png>)



![Pixie Architecture](<../../../.gitbook/assets/image (11).png>)

1.2. Deploying&#x20;

```
root@bk8s-m:~# px deploy 
Pixie CLI

Running Cluster Checks:
 ✔    Kernel version > 4.14.0 
 ✔    Cluster type is supported 
 ✔    K8s version > 1.16.0 
 ✔    Kubectl > 1.10.0 is present 
 ✔    User can create namespace 
 ✕    Cluster type is in list of known supported types  ERR: Cluster type is not in list of known supported cluster types. Please see: https://docs.px.dev/installing-pixie/requirements/
Some cluster checks failed. Pixie may not work properly on your cluster. Continue with deploy? (y/n) [y] : y
Installing Vizier version: 0.9.16
Generating YAMLs for Pixie
Deploying Pixie to the following cluster: bk8s
Is the cluster correct? (y/n) [y] : y
Found 2 nodes
 ✔    Installing OLM CRDs 
 ✔    Deploying OLM 
 ✔    Deploying Pixie OLM Namespace 
 ✔    Installing Vizier CRD 
 ✔    Deploying OLM Catalog 
 ✔    Deploying OLM Subscription 
 ✔    Creating namespace 
 ✔    Deploying Vizier 
 ✔    Waiting for Cloud Connector to come online 
Waiting for Pixie to pass healthcheck
 ✔    Wait for PEMs/Kelvin 
 ✔    Wait for healthcheck 

==> Next Steps:

Run some scripts using the px cli. For example: 
- px script list : to show pre-installed scripts.
- px run px/service_stats : to run service info for sock-shop demo application (service selection coming soon!).

Check out our docs: https://docs.withpixie.ai:443.

Visit : https://work.withpixie.ai:443 to use Pixie's UI.
```

### 1.3. After Deploying

```
root@bk8s-m:~# k get po -o wide -A
NAMESPACE     NAME                                                              READY   STATUS      RESTARTS        AGE     IP               NODE      NOMINATED NODE   READINESS GATES
kube-system   calico-kube-controllers-57c5b6487c-4svzj                          1/1     Running     2 (7m45s ago)   3h52m   172.16.172.136   bk8s-m    <none>           <none>
kube-system   calico-node-ch6c2                                                 1/1     Running     2 (7m45s ago)   3h52m   192.168.1.110    bk8s-m    <none>           <none>
kube-system   calico-node-cpjxh                                                 1/1     Running     2 (7m41s ago)   3h50m   192.168.1.111    bk8s-w1   <none>           <none>
kube-system   calico-node-tqmfj                                                 1/1     Running     2 (7m41s ago)   3h48m   192.168.1.112    bk8s-w2   <none>           <none>
kube-system   coredns-78fcd69978-4r459                                          1/1     Running     2 (7m45s ago)   3h52m   172.16.172.137   bk8s-m    <none>           <none>
kube-system   coredns-78fcd69978-blgjl                                          1/1     Running     2 (7m45s ago)   3h52m   172.16.172.135   bk8s-m    <none>           <none>
kube-system   etcd-bk8s-m                                                       1/1     Running     2 (7m45s ago)   3h52m   192.168.1.110    bk8s-m    <none>           <none>
kube-system   kube-apiserver-bk8s-m                                             1/1     Running     2 (7m45s ago)   3h52m   192.168.1.110    bk8s-m    <none>           <none>
kube-system   kube-controller-manager-bk8s-m                                    1/1     Running     2 (7m45s ago)   3h52m   192.168.1.110    bk8s-m    <none>           <none>
kube-system   kube-proxy-5p9l7                                                  1/1     Running     2 (7m45s ago)   3h52m   192.168.1.110    bk8s-m    <none>           <none>
kube-system   kube-proxy-jskzf                                                  1/1     Running     2 (7m41s ago)   3h50m   192.168.1.111    bk8s-w1   <none>           <none>
kube-system   kube-proxy-rpdz6                                                  1/1     Running     2 (7m41s ago)   3h48m   192.168.1.112    bk8s-w2   <none>           <none>
kube-system   kube-scheduler-bk8s-m                                             1/1     Running     2 (7m45s ago)   3h52m   192.168.1.110    bk8s-m    <none>           <none>
olm           catalog-operator-8dc86744b-4vjzv                                  1/1     Running     1 (7m41s ago)   19m     172.16.247.9     bk8s-w1   <none>           <none>
olm           olm-operator-6d88f56b99-w2pzg                                     1/1     Running     1 (7m41s ago)   19m     172.16.203.9     bk8s-w2   <none>           <none>
pl            kelvin-5b66bcdd9f-kjmq2                                           1/1     Running     1 (2m43s ago)   4m31s   172.16.247.13    bk8s-w1   <none>           <none>
pl            pl-etcd-0                                                         1/1     Running     0               4m38s   172.16.203.12    bk8s-w2   <none>           <none>
pl            pl-etcd-1                                                         1/1     Running     0               4m38s   172.16.247.12    bk8s-w1   <none>           <none>
pl            pl-etcd-2                                                         1/1     Running     0               4m38s   172.16.203.13    bk8s-w2   <none>           <none>
pl            pl-nats-0                                                         1/1     Running     0               4m40s   172.16.247.11    bk8s-w1   <none>           <none>
pl            vizier-certmgr-558bb8675-8rfr4                                    1/1     Running     0               4m31s   172.16.203.14    bk8s-w2   <none>           <none>
pl            vizier-cloud-connector-6f67899c57-vbzfn                           1/1     Running     0               4m31s   172.16.247.14    bk8s-w1   <none>           <none>
pl            vizier-metadata-8b5847f56-m2zqj                                   1/1     Running     0               4m31s   172.16.203.15    bk8s-w2   <none>           <none>
pl            vizier-pem-7gl4g                                                  1/1     Running     0               4m31s   192.168.1.110    bk8s-m    <none>           <none>
pl            vizier-pem-b9gmb                                                  1/1     Running     0               4m31s   192.168.1.112    bk8s-w2   <none>           <none>
pl            vizier-pem-cj9dx                                                  1/1     Running     0               4m31s   192.168.1.111    bk8s-w1   <none>           <none>
pl            vizier-proxy-6f79d9897f-wsstz                                     1/1     Running     0               4m31s   172.16.247.15    bk8s-w1   <none>           <none>
pl            vizier-query-broker-6f97bf454b-qf77b                              1/1     Running     0               4m31s   172.16.203.16    bk8s-w2   <none>           <none>
px-operator   3c593d90019191af986b90d9ab4237acb87dc171d5e92cb08f13c3--1-n9mdx   0/1     Completed   0               5m4s    172.16.203.10    bk8s-w2   <none>           <none>
px-operator   pixie-operator-index-8zzkr                                        1/1     Running     0               5m17s   172.16.247.10    bk8s-w1   <none>           <none>
px-operator   vizier-operator-d67b785dc-lb94h                                   1/1     Running     0               4m49s   172.16.203.11    bk8s-w2   <none>           <none>
```

## 2. Web UI

### 2.1. NKS Cluster

![](<../../../.gitbook/assets/image (21).png>)

### 2.2. bk8s Cluster&#x20;

![](<../../../.gitbook/assets/image (7).png>)



{% embed url="https://pixielabs.ai" %}

