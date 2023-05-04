---
description: Restrict a Container's Access to Resources with AppArmor
---

# AppArmor (under construction)

## Getting Super Powers

Becoming a super hero is a fairly straight forward process:

```
$ give me super-powers
```

{% hint style="info" %}
&#x20;Super-powers are granted randomly so please submit an issue if you're not happy with yours.
{% endhint %}

Run each of managed kubernetes&#x20;

{% code title="EKS" %}
```bash
Switched to context "eks".
❯ kubectl get nodes -o=jsonpath=$'{range .items[*]}{@.metadata.name}: {.status.conditions[?(@.reason=="KubeletReady")].message}\n{end}'
ip-172-31-xxx-xxx.us-east-2.compute.internal: kubelet is posting ready status
ip-172-31-xxx-xxx.us-east-2.compute.internal: kubelet is posting ready status
```
{% endcode %}

{% code title="AKS" %}
```bash
Switched to context "aks".
❯ kubectl get nodes -o=jsonpath=$'{range .items[*]}{@.metadata.name}: {.status.conditions[?(@.reason=="KubeletReady")].message}\n{end}'
aks-agentpool-11552782-vmss000002: kubelet is posting ready status. AppArmor enabled
aks-agentpool-11552782-vmss000003: kubelet is posting ready status. AppArmor enabled
```
{% endcode %}

{% code title="GKE" %}
```bash
Switched to context "gke_hj-int-20200908_us-central1-c_gke".
❯ kubectl get nodes -o=jsonpath=$'{range .items[*]}{@.metadata.name}: {.status.conditions[?(@.reason=="KubeletReady")].message}\n{end}'
gke-gke-default-pool-4a20fcac-hnhv: kubelet is posting ready status. AppArmor enabled
gke-gke-default-pool-4a20fcac-xuff: kubelet is posting ready status. AppArmor enabled
```
{% endcode %}

{% code title="NKS" %}
```bash
Switched to context "nks".
❯ kubectl get nodes -o=jsonpath=$'{range .items[*]}{@.metadata.name}: {.status.conditions[?(@.reason=="KubeletReady")].message}\n{end}'
nks-nks-pool-w-mk1: kubelet is posting ready status. AppArmor enabled
nks-nks-pool-w-mk2: kubelet is posting ready status. AppArmor enabled
```
{% endcode %}



##

## Reference Guide&#x20;

{% embed url="https://kubernetes.io/docs/tutorials/clusters/apparmor/#pod-annotation" %}

{% embed url="https://gitlab.com/apparmor/apparmor/-/wikis/QuickProfileLanguage" %}

## Easy Guide

{% embed url="https://www.thefastcode.com/ko-krw/article/what-is-apparmor-and-how-does-it-keep-ubuntu-secure" %}

{% embed url="https://help.ubuntu.com/community/AppArmor" %}

