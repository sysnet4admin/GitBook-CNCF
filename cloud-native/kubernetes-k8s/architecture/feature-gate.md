# Feature Gate

## CentOS

### 체크 kubelet kubeconfig&#x20;

```
[root@w1-k8s ~]# cat /usr/lib/systemd/system/kubelet.service.d/10-kubeadm.conf
# Note: This dropin only works with kubeadm and kubelet v1.11+
[Service]
Environment="KUBELET_KUBECONFIG_ARGS=--bootstrap-kubeconfig=/etc/kubernetes/bootstrap-kubelet.conf --kubeconfig=/etc/kubernetes/kubelet.conf"
Environment="KUBELET_CONFIG_ARGS=--config=/var/lib/kubelet/config.yaml"
# This is a file that "kubeadm init" and "kubeadm join" generates at runtime, populating the KUBELET_KUBEADM_ARGS variable dynamically
EnvironmentFile=-/var/lib/kubelet/kubeadm-flags.env
# This is a file that the user can use for overrides of the kubelet args as a last resort. Preferably, the user should use
# the .NodeRegistration.KubeletExtraArgs object in the configuration files instead. KUBELET_EXTRA_ARGS should be sourced from this file.
EnvironmentFile=-/etc/sysconfig/kubelet
ExecStart=
ExecStart=/usr/bin/kubelet $KUBELET_KUBECONFIG_ARGS $KUBELET_CONFIG_ARGS $KUBELET_KUBEADM_ARGS $KUBELET_EXTRA_ARGS
```



### 환경 변수 추가필요

```
[root@w1-k8s ~]# cat /etc/sysconfig/kubelet
KUBELET_EXTRA_ARGS=--feature-gates=SizeMemoryBackedVolumes=true

#만약 모두 키고 싶다면 
--feature-gates=AllAlpha=true
```

### 변경 후&#x20;

```
systemctl daemon-reload
systemctl restart kubelet.service
```



## Ubuntu

### 체크 kubelet의 kubeconfig&#x20;

```
root@hk8s-w1:/var/lib/kubelet# cat /etc/systemd/system/kubelet.service.d/10-kubeadm.conf 
# Note: This dropin only works with kubeadm and kubelet v1.11+
[Service]
Environment="KUBELET_KUBECONFIG_ARGS=--bootstrap-kubeconfig=/etc/kubernetes/bootstrap-kubelet.conf --kubeconfig=/etc/kubernetes/kubelet.conf"
Environment="KUBELET_CONFIG_ARGS=--config=/var/lib/kubelet/config.yaml"
# This is a file that "kubeadm init" and "kubeadm join" generates at runtime, populating the KUBELET_KUBEADM_ARGS variable dynamically
EnvironmentFile=-/var/lib/kubelet/kubeadm-flags.env
# This is a file that the user can use for overrides of the kubelet args as a last resort. Preferably, the user should use
# the .NodeRegistration.KubeletExtraArgs object in the configuration files instead. KUBELET_EXTRA_ARGS should be sourced from this file.
EnvironmentFile=-/etc/default/kubelet
ExecStart=
ExecStart=/usr/bin/kubelet $KUBELET_KUBECONFIG_ARGS $KUBELET_CONFIG_ARGS $KUBELET_KUBEADM_ARGS $KUBELET_EXTRA_ARGS
```



### 환경 변수 추가필요

```
root@hk8s-w1:/var/lib/kubelet# cat /etc/default/kubelet
KUBELET_EXTRA_ARGS=--feature-gates=SizeMemoryBackedVolumes=true

#만약 모두 키고 싶다면 
--feature-gates=AllAlpha=true
```

### 변경 후&#x20;

```
systemctl daemon-reload
systemctl restart kubelet.service
```





&#x20;
