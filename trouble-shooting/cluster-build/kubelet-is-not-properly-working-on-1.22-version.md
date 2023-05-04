# kubelet is not properly working on 1.22 version

### 0.TL; DR: Change docker cgroup driver from cgroupfs to systemd.

### 1.error during kubeadm init

If you met in k8s 1.22 version, you may consider to change docker driver.&#x20;

```
kubeadm init .....
<snipped>
    m-k8s-1.22: [etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"
    m-k8s-1.22: [wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
    m-k8s-1.22: [kubelet-check] Initial timeout of 40s passed.
    m-k8s-1.22: [kubelet-check] It seems like the kubelet isn't running or healthy.
    m-k8s-1.22: [kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get "http://localhost:10248/healthz": dial tcp 127.0.0.1:10248: connect: connection refused.
    m-k8s-1.22: [kubelet-check] It seems like the kubelet isn't running or healthy.
    m-k8s-1.22: [kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get "http://localhost:10248/healthz": dial tcp 127.0.0.1:10248: connect: connection refused.
    m-k8s-1.22: [kubelet-check] It seems like the kubelet isn't running or healthy.
    m-k8s-1.22: [kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get "http://localhost:10248/healthz": dial tcp 127.0.0.1:10248: connect: connection refused.
    m-k8s-1.22: [kubelet-check] It seems like the kubelet isn't running or healthy.
    m-k8s-1.22: [kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get "http://localhost:10248/healthz": dial tcp 127.0.0.1:10248: connect: connection refused.
    m-k8s-1.22: [kubelet-check] It seems like the kubelet isn't running or healthy.
    m-k8s-1.22: [kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get "http://localhost:10248/healthz": dial tcp 127.0.0.1:10248: connect: connection refused.
    m-k8s-1.22:
    m-k8s-1.22:         Unfortunately, an error has occurred:
    m-k8s-1.22:                 timed out waiting for the condition
    m-k8s-1.22:
    m-k8s-1.22:         This error is likely caused by:
    m-k8s-1.22:                 - The kubelet is not running
    m-k8s-1.22:                 - The kubelet is unhealthy due to a misconfiguration of the node in some way (required cgroups disabled)
    m-k8s-1.22:
    m-k8s-1.22:         If you are on a systemd-powered system, you can try to troubleshoot the error with the following commands:
    m-k8s-1.22:                 - 'systemctl status kubelet'
    m-k8s-1.22:                 - 'journalctl -xeu kubelet'
    m-k8s-1.22:
    m-k8s-1.22:         Additionally, a control plane component may have crashed or exited when started by the container runtime.
    m-k8s-1.22:         To troubleshoot, list all containers using your preferred container runtimes CLI.
    m-k8s-1.22:
    m-k8s-1.22:         Here is one example how you may list all Kubernetes containers running in docker:
    m-k8s-1.22:                 - 'docker ps -a | grep kube | grep -v pause'
    m-k8s-1.22:                 Once you have found the failing container, you can inspect its logs with:
    m-k8s-1.22:                 - 'docker logs CONTAINERID'
    m-k8s-1.22: error execution phase wait-control-plane: couldn't initialize a Kubernetes cluster
    m-k8s-1.22: To see the stack trace of this error execute with --v=5 or higher
    m-k8s-1.22: The connection to the server 192.168.1.10:6443 was refused - did you
```

### 2.Check current config&#x20;

```
[root@m-k8s ~]# docker system info | grep -i driver
 Storage Driver: overlay2
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
```

### 3.Change your docker driver&#x20;

```
# docker daemon config for systemd from cgroupfs & restart 
cat <<EOF > /etc/docker/daemon.json
{
  "exec-opts": ["native.cgroupdriver=systemd"]
}
EOF
systemctl daemon-reload && systemctl restart docker
```

### 4.Re-run kubeadm init with options

```
kubeadm init ....
<snipped>
    m-k8s-1.22: [etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"
    m-k8s-1.22: [wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
    m-k8s-1.22: [apiclient] All control plane components are healthy after 22.506471 seconds
    m-k8s-1.22: [upload-config] Storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
    m-k8s-1.22: [kubelet] Creating a ConfigMap "kubelet-config-1.22" in namespace kube-system with the configuration for the kubelets in the cluster
    m-k8s-1.22: [upload-certs] Skipping phase. Please see --upload-certs
    m-k8s-1.22: [mark-control-plane] Marking the node m-k8s as control-plane by adding the labels: [node-role.kubernetes.io/master(deprecated) node-role.kubernetes.io/control-plane node.kubernetes.io/exclude-from-external-load-balancers]
    m-k8s-1.22: [mark-control-plane] Marking the node m-k8s as control-plane by adding the taints [node-role.kubernetes.io/master:NoSchedule]
    m-k8s-1.22: [bootstrap-token] Using token: 123456.1234567890123456
    m-k8s-1.22: [bootstrap-token] Configuring bootstrap tokens, cluster-info ConfigMap, RBAC Roles
    m-k8s-1.22: [bootstrap-token] configured RBAC rules to allow Node Bootstrap tokens to get nodes
    m-k8s-1.22: [bootstrap-token] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
    m-k8s-1.22: [bootstrap-token] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
    m-k8s-1.22: [bootstrap-token] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
    m-k8s-1.22: [bootstrap-token] Creating the "cluster-info" ConfigMap in the "kube-public" namespace
    m-k8s-1.22: [kubelet-finalize] Updating "/etc/kubernetes/kubelet.conf" to point to a rotatable kubelet client certificate and key
    m-k8s-1.22: [addons] Applied essential addon: CoreDNS
    m-k8s-1.22: [addons] Applied essential addon: kube-proxy
    m-k8s-1.22:
    m-k8s-1.22: Your Kubernetes control-plane has initialized successfully!
    m-k8s-1.22:
    m-k8s-1.22: To start using your cluster, you need to run the following as a regular user:
```

### 5. Check Current config (systemd is coming!!!) &#x20;

```
[root@m-k8s ~]# docker system info | grep -i driver
 Storage Driver: overlay2
 Logging Driver: json-file
 Cgroup Driver: systemd
```

{% hint style="info" %}
It is not happened to issue all the times. Only limited environment. &#x20;
{% endhint %}

Additional Info(Journal log):

```
Aug 05 10:31:39 m-k8s kubelet[12849]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:31:39 m-k8s kubelet[12849]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:31:39 m-k8s systemd[1]: Started Kubernetes systemd probe.
Aug 05 10:31:39 m-k8s kubelet[12849]: I0805 10:31:39.653961   12849 server.go:440] "Kubelet version" kubeletVersion="v1.22.0"
Aug 05 10:31:39 m-k8s kubelet[12849]: I0805 10:31:39.655693   12849 server.go:868] "Client rotation is on, will bootstrap in background"
Aug 05 10:31:39 m-k8s kubelet[12849]: I0805 10:31:39.665790   12849 certificate_store.go:130] Loading cert/key pair from "/var/lib/kubelet/pki/kubelet-client-current.pem".
Aug 05 10:31:39 m-k8s kubelet[12849]: I0805 10:31:39.670899   12849 dynamic_cafile_content.go:155] "Starting controller" name="client-ca-bundle::/etc/kubernetes/pki/ca.crt"
Aug 05 10:31:39 m-k8s kubelet[12849]: I0805 10:31:39.844096   12849 server.go:687] "--cgroups-per-qos enabled, but --cgroup-root was not specified.  defaulting to /"
Aug 05 10:31:39 m-k8s kubelet[12849]: I0805 10:31:39.844559   12849 container_manager_linux.go:280] "Container manager verified user specified cgroup-root exists" cgroupRoot=[]
Aug 05 10:31:39 m-k8s kubelet[12849]: I0805 10:31:39.844879   12849 container_manager_linux.go:285] "Creating Container Manager object based on Node Config" nodeConfig={RuntimeCgroupsName: SystemCgroupsName: Kub
Aug 05 10:31:39 m-k8s kubelet[12849]: I0805 10:31:39.844939   12849 topology_manager.go:133] "Creating topology manager with policy per scope" topologyPolicyName="none" topologyScopeName="container"
Aug 05 10:31:39 m-k8s kubelet[12849]: I0805 10:31:39.844968   12849 container_manager_linux.go:320] "Creating device plugin manager" devicePluginEnabled=true
Aug 05 10:31:39 m-k8s kubelet[12849]: I0805 10:31:39.845063   12849 state_mem.go:36] "Initialized new in-memory state store"
Aug 05 10:31:39 m-k8s kubelet[12849]: I0805 10:31:39.845162   12849 kubelet.go:314] "Using dockershim is deprecated, please consider using a full-fledged CRI implementation"
Aug 05 10:31:39 m-k8s kubelet[12849]: I0805 10:31:39.845216   12849 client.go:78] "Connecting to docker on the dockerEndpoint" endpoint="unix:///var/run/docker.sock"
Aug 05 10:31:39 m-k8s kubelet[12849]: I0805 10:31:39.845247   12849 client.go:97] "Start docker client with request timeout" timeout="2m0s"
Aug 05 10:31:39 m-k8s kubelet[12849]: I0805 10:31:39.876721   12849 docker_service.go:566] "Hairpin mode is set but kubenet is not enabled, falling back to HairpinVeth" hairpinMode=promiscuous-bridge
Aug 05 10:31:39 m-k8s kubelet[12849]: I0805 10:31:39.876770   12849 docker_service.go:242] "Hairpin mode is set" hairpinMode=hairpin-veth
Aug 05 10:31:39 m-k8s kubelet[12849]: I0805 10:31:39.877071   12849 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:31:39 m-k8s kubelet[12849]: I0805 10:31:39.884909   12849 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:31:39 m-k8s kubelet[12849]: I0805 10:31:39.885020   12849 docker_service.go:257] "Docker cri networking managed by the network plugin" networkPluginName="cni"
Aug 05 10:31:39 m-k8s kubelet[12849]: I0805 10:31:39.885175   12849 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:31:39 m-k8s kubelet[12849]: I0805 10:31:39.907320   12849 docker_service.go:264] "Docker Info" dockerInfo=&{ID:RXTP:LTUA:M4Z5:JXNJ:K3ID:KR2N:RKLR:PUSN:JUVB:IV7H:52EF:7G25 Containers:0 ContainersRunning
Aug 05 10:31:39 m-k8s kubelet[12849]: E0805 10:31:39.907380   12849 server.go:294] "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docke
Aug 05 10:31:39 m-k8s systemd[1]: kubelet.service: main process exited, code=exited, status=1/FAILURE
Aug 05 10:31:39 m-k8s systemd[1]: Unit kubelet.service entered failed state.
Aug 05 10:31:39 m-k8s systemd[1]: kubelet.service failed.
Aug 05 10:31:49 m-k8s systemd[1]: kubelet.service holdoff time over, scheduling restart.
Aug 05 10:31:49 m-k8s systemd[1]: Stopped kubelet: The Kubernetes Node Agent.
Aug 05 10:31:49 m-k8s systemd[1]: Started kubelet: The Kubernetes Node Agent.
Aug 05 10:31:50 m-k8s kubelet[12932]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:31:50 m-k8s kubelet[12932]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:31:50 m-k8s systemd[1]: Started Kubernetes systemd probe.
Aug 05 10:31:50 m-k8s kubelet[12932]: I0805 10:31:50.169830   12932 server.go:440] "Kubelet version" kubeletVersion="v1.22.0"
Aug 05 10:31:50 m-k8s kubelet[12932]: I0805 10:31:50.170299   12932 server.go:868] "Client rotation is on, will bootstrap in background"
Aug 05 10:31:50 m-k8s kubelet[12932]: I0805 10:31:50.176410   12932 certificate_store.go:130] Loading cert/key pair from "/var/lib/kubelet/pki/kubelet-client-current.pem".
Aug 05 10:31:50 m-k8s kubelet[12932]: I0805 10:31:50.179116   12932 dynamic_cafile_content.go:155] "Starting controller" name="client-ca-bundle::/etc/kubernetes/pki/ca.crt"
Aug 05 10:31:50 m-k8s kubelet[12932]: I0805 10:31:50.352872   12932 server.go:687] "--cgroups-per-qos enabled, but --cgroup-root was not specified.  defaulting to /"
Aug 05 10:31:50 m-k8s kubelet[12932]: I0805 10:31:50.353198   12932 container_manager_linux.go:280] "Container manager verified user specified cgroup-root exists" cgroupRoot=[]
Aug 05 10:31:50 m-k8s kubelet[12932]: I0805 10:31:50.353319   12932 container_manager_linux.go:285] "Creating Container Manager object based on Node Config" nodeConfig={RuntimeCgroupsName: SystemCgroupsName: Kub
Aug 05 10:31:50 m-k8s kubelet[12932]: I0805 10:31:50.353375   12932 topology_manager.go:133] "Creating topology manager with policy per scope" topologyPolicyName="none" topologyScopeName="container"
Aug 05 10:31:50 m-k8s kubelet[12932]: I0805 10:31:50.353403   12932 container_manager_linux.go:320] "Creating device plugin manager" devicePluginEnabled=true
Aug 05 10:31:50 m-k8s kubelet[12932]: I0805 10:31:50.353488   12932 state_mem.go:36] "Initialized new in-memory state store"
Aug 05 10:31:50 m-k8s kubelet[12932]: I0805 10:31:50.353585   12932 kubelet.go:314] "Using dockershim is deprecated, please consider using a full-fledged CRI implementation"
Aug 05 10:31:50 m-k8s kubelet[12932]: I0805 10:31:50.353657   12932 client.go:78] "Connecting to docker on the dockerEndpoint" endpoint="unix:///var/run/docker.sock"
Aug 05 10:31:50 m-k8s kubelet[12932]: I0805 10:31:50.353686   12932 client.go:97] "Start docker client with request timeout" timeout="2m0s"
Aug 05 10:31:50 m-k8s kubelet[12932]: I0805 10:31:50.376650   12932 docker_service.go:566] "Hairpin mode is set but kubenet is not enabled, falling back to HairpinVeth" hairpinMode=promiscuous-bridge
Aug 05 10:31:50 m-k8s kubelet[12932]: I0805 10:31:50.376757   12932 docker_service.go:242] "Hairpin mode is set" hairpinMode=hairpin-veth
Aug 05 10:31:50 m-k8s kubelet[12932]: I0805 10:31:50.376943   12932 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:31:50 m-k8s kubelet[12932]: I0805 10:31:50.385991   12932 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:31:50 m-k8s kubelet[12932]: I0805 10:31:50.386090   12932 docker_service.go:257] "Docker cri networking managed by the network plugin" networkPluginName="cni"
Aug 05 10:31:50 m-k8s kubelet[12932]: I0805 10:31:50.386242   12932 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:31:50 m-k8s kubelet[12932]: I0805 10:31:50.411102   12932 docker_service.go:264] "Docker Info" dockerInfo=&{ID:RXTP:LTUA:M4Z5:JXNJ:K3ID:KR2N:RKLR:PUSN:JUVB:IV7H:52EF:7G25 Containers:0 ContainersRunning
Aug 05 10:31:50 m-k8s kubelet[12932]: E0805 10:31:50.411153   12932 server.go:294] "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docke
Aug 05 10:31:50 m-k8s systemd[1]: kubelet.service: main process exited, code=exited, status=1/FAILURE
Aug 05 10:31:50 m-k8s systemd[1]: Unit kubelet.service entered failed state.
Aug 05 10:31:50 m-k8s systemd[1]: kubelet.service failed.
Aug 05 10:32:00 m-k8s systemd[1]: kubelet.service holdoff time over, scheduling restart.
Aug 05 10:32:00 m-k8s systemd[1]: Stopped kubelet: The Kubernetes Node Agent.
Aug 05 10:32:00 m-k8s systemd[1]: Started kubelet: The Kubernetes Node Agent.
Aug 05 10:32:00 m-k8s kubelet[13013]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:32:00 m-k8s kubelet[13013]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:32:00 m-k8s systemd[1]: Started Kubernetes systemd probe.
Aug 05 10:32:00 m-k8s kubelet[13013]: I0805 10:32:00.932753   13013 server.go:440] "Kubelet version" kubeletVersion="v1.22.0"
Aug 05 10:32:00 m-k8s kubelet[13013]: I0805 10:32:00.933224   13013 server.go:868] "Client rotation is on, will bootstrap in background"
Aug 05 10:32:00 m-k8s kubelet[13013]: I0805 10:32:00.939270   13013 certificate_store.go:130] Loading cert/key pair from "/var/lib/kubelet/pki/kubelet-client-current.pem".
Aug 05 10:32:00 m-k8s kubelet[13013]: I0805 10:32:00.942306   13013 dynamic_cafile_content.go:155] "Starting controller" name="client-ca-bundle::/etc/kubernetes/pki/ca.crt"
Aug 05 10:32:01 m-k8s kubelet[13013]: I0805 10:32:01.122321   13013 server.go:687] "--cgroups-per-qos enabled, but --cgroup-root was not specified.  defaulting to /"
Aug 05 10:32:01 m-k8s kubelet[13013]: I0805 10:32:01.122774   13013 container_manager_linux.go:280] "Container manager verified user specified cgroup-root exists" cgroupRoot=[]
Aug 05 10:32:01 m-k8s kubelet[13013]: I0805 10:32:01.122930   13013 container_manager_linux.go:285] "Creating Container Manager object based on Node Config" nodeConfig={RuntimeCgroupsName: SystemCgroupsName: Kub
Aug 05 10:32:01 m-k8s kubelet[13013]: I0805 10:32:01.122970   13013 topology_manager.go:133] "Creating topology manager with policy per scope" topologyPolicyName="none" topologyScopeName="container"
Aug 05 10:32:01 m-k8s kubelet[13013]: I0805 10:32:01.122996   13013 container_manager_linux.go:320] "Creating device plugin manager" devicePluginEnabled=true
Aug 05 10:32:01 m-k8s kubelet[13013]: I0805 10:32:01.123073   13013 state_mem.go:36] "Initialized new in-memory state store"
Aug 05 10:32:01 m-k8s kubelet[13013]: I0805 10:32:01.123277   13013 kubelet.go:314] "Using dockershim is deprecated, please consider using a full-fledged CRI implementation"
Aug 05 10:32:01 m-k8s kubelet[13013]: I0805 10:32:01.123329   13013 client.go:78] "Connecting to docker on the dockerEndpoint" endpoint="unix:///var/run/docker.sock"
Aug 05 10:32:01 m-k8s kubelet[13013]: I0805 10:32:01.123358   13013 client.go:97] "Start docker client with request timeout" timeout="2m0s"
Aug 05 10:32:01 m-k8s kubelet[13013]: I0805 10:32:01.143834   13013 docker_service.go:566] "Hairpin mode is set but kubenet is not enabled, falling back to HairpinVeth" hairpinMode=promiscuous-bridge
Aug 05 10:32:01 m-k8s kubelet[13013]: I0805 10:32:01.143887   13013 docker_service.go:242] "Hairpin mode is set" hairpinMode=hairpin-veth
Aug 05 10:32:01 m-k8s kubelet[13013]: I0805 10:32:01.144087   13013 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:32:01 m-k8s kubelet[13013]: I0805 10:32:01.153068   13013 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:32:01 m-k8s kubelet[13013]: I0805 10:32:01.153188   13013 docker_service.go:257] "Docker cri networking managed by the network plugin" networkPluginName="cni"
Aug 05 10:32:01 m-k8s kubelet[13013]: I0805 10:32:01.153362   13013 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:32:01 m-k8s kubelet[13013]: I0805 10:32:01.177891   13013 docker_service.go:264] "Docker Info" dockerInfo=&{ID:RXTP:LTUA:M4Z5:JXNJ:K3ID:KR2N:RKLR:PUSN:JUVB:IV7H:52EF:7G25 Containers:0 ContainersRunning
Aug 05 10:32:01 m-k8s kubelet[13013]: E0805 10:32:01.177966   13013 server.go:294] "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docke
Aug 05 10:32:01 m-k8s systemd[1]: kubelet.service: main process exited, code=exited, status=1/FAILURE
Aug 05 10:32:01 m-k8s systemd[1]: Unit kubelet.service entered failed state.
Aug 05 10:32:01 m-k8s systemd[1]: kubelet.service failed.
Aug 05 10:32:11 m-k8s systemd[1]: kubelet.service holdoff time over, scheduling restart.
Aug 05 10:32:11 m-k8s systemd[1]: Stopped kubelet: The Kubernetes Node Agent.
Aug 05 10:32:11 m-k8s systemd[1]: Started kubelet: The Kubernetes Node Agent.
Aug 05 10:32:11 m-k8s kubelet[13092]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:32:11 m-k8s kubelet[13092]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:32:11 m-k8s systemd[1]: Started Kubernetes systemd probe.
Aug 05 10:32:11 m-k8s kubelet[13092]: I0805 10:32:11.609517   13092 server.go:440] "Kubelet version" kubeletVersion="v1.22.0"
Aug 05 10:32:11 m-k8s kubelet[13092]: I0805 10:32:11.610925   13092 server.go:868] "Client rotation is on, will bootstrap in background"
Aug 05 10:32:11 m-k8s kubelet[13092]: I0805 10:32:11.619947   13092 certificate_store.go:130] Loading cert/key pair from "/var/lib/kubelet/pki/kubelet-client-current.pem".
Aug 05 10:32:11 m-k8s kubelet[13092]: I0805 10:32:11.624637   13092 dynamic_cafile_content.go:155] "Starting controller" name="client-ca-bundle::/etc/kubernetes/pki/ca.crt"
Aug 05 10:32:11 m-k8s kubelet[13092]: I0805 10:32:11.803268   13092 server.go:687] "--cgroups-per-qos enabled, but --cgroup-root was not specified.  defaulting to /"
Aug 05 10:32:11 m-k8s kubelet[13092]: I0805 10:32:11.803720   13092 container_manager_linux.go:280] "Container manager verified user specified cgroup-root exists" cgroupRoot=[]
Aug 05 10:32:11 m-k8s kubelet[13092]: I0805 10:32:11.803884   13092 container_manager_linux.go:285] "Creating Container Manager object based on Node Config" nodeConfig={RuntimeCgroupsName: SystemCgroupsName: Kub
Aug 05 10:32:11 m-k8s kubelet[13092]: I0805 10:32:11.803938   13092 topology_manager.go:133] "Creating topology manager with policy per scope" topologyPolicyName="none" topologyScopeName="container"
Aug 05 10:32:11 m-k8s kubelet[13092]: I0805 10:32:11.803964   13092 container_manager_linux.go:320] "Creating device plugin manager" devicePluginEnabled=true
Aug 05 10:32:11 m-k8s kubelet[13092]: I0805 10:32:11.804166   13092 state_mem.go:36] "Initialized new in-memory state store"
Aug 05 10:32:11 m-k8s kubelet[13092]: I0805 10:32:11.804268   13092 kubelet.go:314] "Using dockershim is deprecated, please consider using a full-fledged CRI implementation"
Aug 05 10:32:11 m-k8s kubelet[13092]: I0805 10:32:11.804311   13092 client.go:78] "Connecting to docker on the dockerEndpoint" endpoint="unix:///var/run/docker.sock"
Aug 05 10:32:11 m-k8s kubelet[13092]: I0805 10:32:11.804339   13092 client.go:97] "Start docker client with request timeout" timeout="2m0s"
Aug 05 10:32:11 m-k8s kubelet[13092]: I0805 10:32:11.833233   13092 docker_service.go:566] "Hairpin mode is set but kubenet is not enabled, falling back to HairpinVeth" hairpinMode=promiscuous-bridge
Aug 05 10:32:11 m-k8s kubelet[13092]: I0805 10:32:11.833375   13092 docker_service.go:242] "Hairpin mode is set" hairpinMode=hairpin-veth
Aug 05 10:32:11 m-k8s kubelet[13092]: I0805 10:32:11.833579   13092 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:32:11 m-k8s kubelet[13092]: I0805 10:32:11.842728   13092 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:32:11 m-k8s kubelet[13092]: I0805 10:32:11.843633   13092 docker_service.go:257] "Docker cri networking managed by the network plugin" networkPluginName="cni"
Aug 05 10:32:11 m-k8s kubelet[13092]: I0805 10:32:11.843701   13092 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:32:11 m-k8s kubelet[13092]: I0805 10:32:11.866964   13092 docker_service.go:264] "Docker Info" dockerInfo=&{ID:RXTP:LTUA:M4Z5:JXNJ:K3ID:KR2N:RKLR:PUSN:JUVB:IV7H:52EF:7G25 Containers:0 ContainersRunning
Aug 05 10:32:11 m-k8s kubelet[13092]: E0805 10:32:11.870180   13092 server.go:294] "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docke
Aug 05 10:32:11 m-k8s systemd[1]: kubelet.service: main process exited, code=exited, status=1/FAILURE
Aug 05 10:32:11 m-k8s systemd[1]: Unit kubelet.service entered failed state.
Aug 05 10:32:11 m-k8s systemd[1]: kubelet.service failed.
Aug 05 10:32:21 m-k8s systemd[1]: kubelet.service holdoff time over, scheduling restart.
Aug 05 10:32:21 m-k8s systemd[1]: Stopped kubelet: The Kubernetes Node Agent.
Aug 05 10:32:21 m-k8s systemd[1]: Started kubelet: The Kubernetes Node Agent.
Aug 05 10:32:22 m-k8s kubelet[13169]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:32:22 m-k8s kubelet[13169]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:32:22 m-k8s systemd[1]: Started Kubernetes systemd probe.
Aug 05 10:32:22 m-k8s kubelet[13169]: I0805 10:32:22.166751   13169 server.go:440] "Kubelet version" kubeletVersion="v1.22.0"
Aug 05 10:32:22 m-k8s kubelet[13169]: I0805 10:32:22.168725   13169 server.go:868] "Client rotation is on, will bootstrap in background"
Aug 05 10:32:22 m-k8s kubelet[13169]: I0805 10:32:22.174533   13169 certificate_store.go:130] Loading cert/key pair from "/var/lib/kubelet/pki/kubelet-client-current.pem".
Aug 05 10:32:22 m-k8s kubelet[13169]: I0805 10:32:22.177998   13169 dynamic_cafile_content.go:155] "Starting controller" name="client-ca-bundle::/etc/kubernetes/pki/ca.crt"
Aug 05 10:32:22 m-k8s kubelet[13169]: I0805 10:32:22.368269   13169 server.go:687] "--cgroups-per-qos enabled, but --cgroup-root was not specified.  defaulting to /"
Aug 05 10:32:22 m-k8s kubelet[13169]: I0805 10:32:22.368722   13169 container_manager_linux.go:280] "Container manager verified user specified cgroup-root exists" cgroupRoot=[]
Aug 05 10:32:22 m-k8s kubelet[13169]: I0805 10:32:22.368837   13169 container_manager_linux.go:285] "Creating Container Manager object based on Node Config" nodeConfig={RuntimeCgroupsName: SystemCgroupsName: Kub
Aug 05 10:32:22 m-k8s kubelet[13169]: I0805 10:32:22.368865   13169 topology_manager.go:133] "Creating topology manager with policy per scope" topologyPolicyName="none" topologyScopeName="container"
Aug 05 10:32:22 m-k8s kubelet[13169]: I0805 10:32:22.368882   13169 container_manager_linux.go:320] "Creating device plugin manager" devicePluginEnabled=true
Aug 05 10:32:22 m-k8s kubelet[13169]: I0805 10:32:22.368936   13169 state_mem.go:36] "Initialized new in-memory state store"
Aug 05 10:32:22 m-k8s kubelet[13169]: I0805 10:32:22.369075   13169 kubelet.go:314] "Using dockershim is deprecated, please consider using a full-fledged CRI implementation"
Aug 05 10:32:22 m-k8s kubelet[13169]: I0805 10:32:22.369125   13169 client.go:78] "Connecting to docker on the dockerEndpoint" endpoint="unix:///var/run/docker.sock"
Aug 05 10:32:22 m-k8s kubelet[13169]: I0805 10:32:22.369155   13169 client.go:97] "Start docker client with request timeout" timeout="2m0s"
Aug 05 10:32:22 m-k8s kubelet[13169]: I0805 10:32:22.398395   13169 docker_service.go:566] "Hairpin mode is set but kubenet is not enabled, falling back to HairpinVeth" hairpinMode=promiscuous-bridge
Aug 05 10:32:22 m-k8s kubelet[13169]: I0805 10:32:22.398433   13169 docker_service.go:242] "Hairpin mode is set" hairpinMode=hairpin-veth
Aug 05 10:32:22 m-k8s kubelet[13169]: I0805 10:32:22.398625   13169 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:32:22 m-k8s kubelet[13169]: I0805 10:32:22.407401   13169 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:32:22 m-k8s kubelet[13169]: I0805 10:32:22.407531   13169 docker_service.go:257] "Docker cri networking managed by the network plugin" networkPluginName="cni"
Aug 05 10:32:22 m-k8s kubelet[13169]: I0805 10:32:22.407668   13169 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:32:22 m-k8s kubelet[13169]: I0805 10:32:22.433543   13169 docker_service.go:264] "Docker Info" dockerInfo=&{ID:RXTP:LTUA:M4Z5:JXNJ:K3ID:KR2N:RKLR:PUSN:JUVB:IV7H:52EF:7G25 Containers:0 ContainersRunning
Aug 05 10:32:22 m-k8s kubelet[13169]: E0805 10:32:22.433590   13169 server.go:294] "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docke
Aug 05 10:32:22 m-k8s systemd[1]: kubelet.service: main process exited, code=exited, status=1/FAILURE
Aug 05 10:32:22 m-k8s systemd[1]: Unit kubelet.service entered failed state.
Aug 05 10:32:22 m-k8s systemd[1]: kubelet.service failed.
Aug 05 10:32:32 m-k8s systemd[1]: kubelet.service holdoff time over, scheduling restart.
Aug 05 10:32:32 m-k8s systemd[1]: Stopped kubelet: The Kubernetes Node Agent.
Aug 05 10:32:32 m-k8s systemd[1]: Started kubelet: The Kubernetes Node Agent.
Aug 05 10:32:33 m-k8s kubelet[13248]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:32:33 m-k8s kubelet[13248]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:32:33 m-k8s systemd[1]: Started Kubernetes systemd probe.
Aug 05 10:32:33 m-k8s kubelet[13248]: I0805 10:32:33.115489   13248 server.go:440] "Kubelet version" kubeletVersion="v1.22.0"
Aug 05 10:32:33 m-k8s kubelet[13248]: I0805 10:32:33.157458   13248 server.go:868] "Client rotation is on, will bootstrap in background"
Aug 05 10:32:33 m-k8s kubelet[13248]: I0805 10:32:33.191620   13248 certificate_store.go:130] Loading cert/key pair from "/var/lib/kubelet/pki/kubelet-client-current.pem".
Aug 05 10:32:33 m-k8s kubelet[13248]: I0805 10:32:33.197197   13248 dynamic_cafile_content.go:155] "Starting controller" name="client-ca-bundle::/etc/kubernetes/pki/ca.crt"
Aug 05 10:32:33 m-k8s kubelet[13248]: I0805 10:32:33.575876   13248 server.go:687] "--cgroups-per-qos enabled, but --cgroup-root was not specified.  defaulting to /"
Aug 05 10:32:33 m-k8s kubelet[13248]: I0805 10:32:33.577026   13248 container_manager_linux.go:280] "Container manager verified user specified cgroup-root exists" cgroupRoot=[]
Aug 05 10:32:33 m-k8s kubelet[13248]: I0805 10:32:33.577227   13248 container_manager_linux.go:285] "Creating Container Manager object based on Node Config" nodeConfig={RuntimeCgroupsName: SystemCgroupsName: Kub
Aug 05 10:32:33 m-k8s kubelet[13248]: I0805 10:32:33.577368   13248 topology_manager.go:133] "Creating topology manager with policy per scope" topologyPolicyName="none" topologyScopeName="container"
Aug 05 10:32:33 m-k8s kubelet[13248]: I0805 10:32:33.577407   13248 container_manager_linux.go:320] "Creating device plugin manager" devicePluginEnabled=true
Aug 05 10:32:33 m-k8s kubelet[13248]: I0805 10:32:33.577515   13248 state_mem.go:36] "Initialized new in-memory state store"
Aug 05 10:32:33 m-k8s kubelet[13248]: I0805 10:32:33.577625   13248 kubelet.go:314] "Using dockershim is deprecated, please consider using a full-fledged CRI implementation"
Aug 05 10:32:33 m-k8s kubelet[13248]: I0805 10:32:33.577679   13248 client.go:78] "Connecting to docker on the dockerEndpoint" endpoint="unix:///var/run/docker.sock"
Aug 05 10:32:33 m-k8s kubelet[13248]: I0805 10:32:33.577711   13248 client.go:97] "Start docker client with request timeout" timeout="2m0s"
Aug 05 10:32:33 m-k8s kubelet[13248]: I0805 10:32:33.625604   13248 docker_service.go:566] "Hairpin mode is set but kubenet is not enabled, falling back to HairpinVeth" hairpinMode=promiscuous-bridge
Aug 05 10:32:33 m-k8s kubelet[13248]: I0805 10:32:33.626012   13248 docker_service.go:242] "Hairpin mode is set" hairpinMode=hairpin-veth
Aug 05 10:32:33 m-k8s kubelet[13248]: I0805 10:32:33.626287   13248 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:32:33 m-k8s kubelet[13248]: I0805 10:32:33.640850   13248 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:32:33 m-k8s kubelet[13248]: I0805 10:32:33.640968   13248 docker_service.go:257] "Docker cri networking managed by the network plugin" networkPluginName="cni"
Aug 05 10:32:33 m-k8s kubelet[13248]: I0805 10:32:33.642479   13248 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:32:33 m-k8s kubelet[13248]: I0805 10:32:33.676496   13248 docker_service.go:264] "Docker Info" dockerInfo=&{ID:RXTP:LTUA:M4Z5:JXNJ:K3ID:KR2N:RKLR:PUSN:JUVB:IV7H:52EF:7G25 Containers:0 ContainersRunning
Aug 05 10:32:33 m-k8s kubelet[13248]: E0805 10:32:33.676574   13248 server.go:294] "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docke
Aug 05 10:32:33 m-k8s systemd[1]: kubelet.service: main process exited, code=exited, status=1/FAILURE
Aug 05 10:32:33 m-k8s systemd[1]: Unit kubelet.service entered failed state.
Aug 05 10:32:33 m-k8s systemd[1]: kubelet.service failed.
Aug 05 10:32:43 m-k8s systemd[1]: kubelet.service holdoff time over, scheduling restart.
Aug 05 10:32:43 m-k8s systemd[1]: Stopped kubelet: The Kubernetes Node Agent.
Aug 05 10:32:43 m-k8s systemd[1]: Started kubelet: The Kubernetes Node Agent.
Aug 05 10:32:44 m-k8s kubelet[13329]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:32:44 m-k8s kubelet[13329]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:32:44 m-k8s systemd[1]: Started Kubernetes systemd probe.
Aug 05 10:32:44 m-k8s kubelet[13329]: I0805 10:32:44.140963   13329 server.go:440] "Kubelet version" kubeletVersion="v1.22.0"
Aug 05 10:32:44 m-k8s kubelet[13329]: I0805 10:32:44.141448   13329 server.go:868] "Client rotation is on, will bootstrap in background"
Aug 05 10:32:44 m-k8s kubelet[13329]: I0805 10:32:44.147503   13329 certificate_store.go:130] Loading cert/key pair from "/var/lib/kubelet/pki/kubelet-client-current.pem".
Aug 05 10:32:44 m-k8s kubelet[13329]: I0805 10:32:44.154827   13329 dynamic_cafile_content.go:155] "Starting controller" name="client-ca-bundle::/etc/kubernetes/pki/ca.crt"
Aug 05 10:32:44 m-k8s kubelet[13329]: I0805 10:32:44.390322   13329 server.go:687] "--cgroups-per-qos enabled, but --cgroup-root was not specified.  defaulting to /"
Aug 05 10:32:44 m-k8s kubelet[13329]: I0805 10:32:44.391124   13329 container_manager_linux.go:280] "Container manager verified user specified cgroup-root exists" cgroupRoot=[]
Aug 05 10:32:44 m-k8s kubelet[13329]: I0805 10:32:44.391331   13329 container_manager_linux.go:285] "Creating Container Manager object based on Node Config" nodeConfig={RuntimeCgroupsName: SystemCgroupsName: Kub
Aug 05 10:32:44 m-k8s kubelet[13329]: I0805 10:32:44.391383   13329 topology_manager.go:133] "Creating topology manager with policy per scope" topologyPolicyName="none" topologyScopeName="container"
Aug 05 10:32:44 m-k8s kubelet[13329]: I0805 10:32:44.391407   13329 container_manager_linux.go:320] "Creating device plugin manager" devicePluginEnabled=true
Aug 05 10:32:44 m-k8s kubelet[13329]: I0805 10:32:44.391591   13329 state_mem.go:36] "Initialized new in-memory state store"
Aug 05 10:32:44 m-k8s kubelet[13329]: I0805 10:32:44.391699   13329 kubelet.go:314] "Using dockershim is deprecated, please consider using a full-fledged CRI implementation"
Aug 05 10:32:44 m-k8s kubelet[13329]: I0805 10:32:44.391776   13329 client.go:78] "Connecting to docker on the dockerEndpoint" endpoint="unix:///var/run/docker.sock"
Aug 05 10:32:44 m-k8s kubelet[13329]: I0805 10:32:44.391813   13329 client.go:97] "Start docker client with request timeout" timeout="2m0s"
Aug 05 10:32:44 m-k8s kubelet[13329]: I0805 10:32:44.424531   13329 docker_service.go:566] "Hairpin mode is set but kubenet is not enabled, falling back to HairpinVeth" hairpinMode=promiscuous-bridge
Aug 05 10:32:44 m-k8s kubelet[13329]: I0805 10:32:44.424642   13329 docker_service.go:242] "Hairpin mode is set" hairpinMode=hairpin-veth
Aug 05 10:32:44 m-k8s kubelet[13329]: I0805 10:32:44.424961   13329 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:32:44 m-k8s kubelet[13329]: I0805 10:32:44.437531   13329 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:32:44 m-k8s kubelet[13329]: I0805 10:32:44.437901   13329 docker_service.go:257] "Docker cri networking managed by the network plugin" networkPluginName="cni"
Aug 05 10:32:44 m-k8s kubelet[13329]: I0805 10:32:44.440358   13329 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:32:44 m-k8s kubelet[13329]: I0805 10:32:44.463067   13329 docker_service.go:264] "Docker Info" dockerInfo=&{ID:RXTP:LTUA:M4Z5:JXNJ:K3ID:KR2N:RKLR:PUSN:JUVB:IV7H:52EF:7G25 Containers:0 ContainersRunning
Aug 05 10:32:44 m-k8s kubelet[13329]: E0805 10:32:44.463123   13329 server.go:294] "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docke
Aug 05 10:32:44 m-k8s systemd[1]: kubelet.service: main process exited, code=exited, status=1/FAILURE
Aug 05 10:32:44 m-k8s systemd[1]: Unit kubelet.service entered failed state.
Aug 05 10:32:44 m-k8s systemd[1]: kubelet.service failed.
Aug 05 10:32:54 m-k8s systemd[1]: kubelet.service holdoff time over, scheduling restart.
Aug 05 10:32:54 m-k8s systemd[1]: Stopped kubelet: The Kubernetes Node Agent.
Aug 05 10:32:54 m-k8s systemd[1]: Started kubelet: The Kubernetes Node Agent.
Aug 05 10:32:54 m-k8s kubelet[13409]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:32:54 m-k8s kubelet[13409]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:32:54 m-k8s systemd[1]: Started Kubernetes systemd probe.
Aug 05 10:32:54 m-k8s kubelet[13409]: I0805 10:32:54.849176   13409 server.go:440] "Kubelet version" kubeletVersion="v1.22.0"
Aug 05 10:32:54 m-k8s kubelet[13409]: I0805 10:32:54.849710   13409 server.go:868] "Client rotation is on, will bootstrap in background"
Aug 05 10:32:54 m-k8s kubelet[13409]: I0805 10:32:54.855353   13409 certificate_store.go:130] Loading cert/key pair from "/var/lib/kubelet/pki/kubelet-client-current.pem".
Aug 05 10:32:54 m-k8s kubelet[13409]: I0805 10:32:54.858947   13409 dynamic_cafile_content.go:155] "Starting controller" name="client-ca-bundle::/etc/kubernetes/pki/ca.crt"
Aug 05 10:32:55 m-k8s kubelet[13409]: I0805 10:32:55.038935   13409 server.go:687] "--cgroups-per-qos enabled, but --cgroup-root was not specified.  defaulting to /"
Aug 05 10:32:55 m-k8s kubelet[13409]: I0805 10:32:55.039394   13409 container_manager_linux.go:280] "Container manager verified user specified cgroup-root exists" cgroupRoot=[]
Aug 05 10:32:55 m-k8s kubelet[13409]: I0805 10:32:55.039715   13409 container_manager_linux.go:285] "Creating Container Manager object based on Node Config" nodeConfig={RuntimeCgroupsName: SystemCgroupsName: Kub
Aug 05 10:32:55 m-k8s kubelet[13409]: I0805 10:32:55.039762   13409 topology_manager.go:133] "Creating topology manager with policy per scope" topologyPolicyName="none" topologyScopeName="container"
Aug 05 10:32:55 m-k8s kubelet[13409]: I0805 10:32:55.039789   13409 container_manager_linux.go:320] "Creating device plugin manager" devicePluginEnabled=true
Aug 05 10:32:55 m-k8s kubelet[13409]: I0805 10:32:55.039891   13409 state_mem.go:36] "Initialized new in-memory state store"
Aug 05 10:32:55 m-k8s kubelet[13409]: I0805 10:32:55.039999   13409 kubelet.go:314] "Using dockershim is deprecated, please consider using a full-fledged CRI implementation"
Aug 05 10:32:55 m-k8s kubelet[13409]: I0805 10:32:55.040043   13409 client.go:78] "Connecting to docker on the dockerEndpoint" endpoint="unix:///var/run/docker.sock"
Aug 05 10:32:55 m-k8s kubelet[13409]: I0805 10:32:55.040070   13409 client.go:97] "Start docker client with request timeout" timeout="2m0s"
Aug 05 10:32:55 m-k8s kubelet[13409]: I0805 10:32:55.072184   13409 docker_service.go:566] "Hairpin mode is set but kubenet is not enabled, falling back to HairpinVeth" hairpinMode=promiscuous-bridge
Aug 05 10:32:55 m-k8s kubelet[13409]: I0805 10:32:55.072294   13409 docker_service.go:242] "Hairpin mode is set" hairpinMode=hairpin-veth
Aug 05 10:32:55 m-k8s kubelet[13409]: I0805 10:32:55.072492   13409 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:32:55 m-k8s kubelet[13409]: I0805 10:32:55.082184   13409 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:32:55 m-k8s kubelet[13409]: I0805 10:32:55.082367   13409 docker_service.go:257] "Docker cri networking managed by the network plugin" networkPluginName="cni"
Aug 05 10:32:55 m-k8s kubelet[13409]: I0805 10:32:55.084217   13409 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:32:55 m-k8s kubelet[13409]: I0805 10:32:55.110958   13409 docker_service.go:264] "Docker Info" dockerInfo=&{ID:RXTP:LTUA:M4Z5:JXNJ:K3ID:KR2N:RKLR:PUSN:JUVB:IV7H:52EF:7G25 Containers:0 ContainersRunning
Aug 05 10:32:55 m-k8s kubelet[13409]: E0805 10:32:55.111033   13409 server.go:294] "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docke
Aug 05 10:32:55 m-k8s systemd[1]: kubelet.service: main process exited, code=exited, status=1/FAILURE
Aug 05 10:32:55 m-k8s systemd[1]: Unit kubelet.service entered failed state.
Aug 05 10:32:55 m-k8s systemd[1]: kubelet.service failed.
Aug 05 10:33:05 m-k8s systemd[1]: kubelet.service holdoff time over, scheduling restart.
Aug 05 10:33:05 m-k8s systemd[1]: Stopped kubelet: The Kubernetes Node Agent.
Aug 05 10:33:05 m-k8s systemd[1]: Started kubelet: The Kubernetes Node Agent.
Aug 05 10:33:05 m-k8s kubelet[13489]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:33:05 m-k8s kubelet[13489]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:33:05 m-k8s systemd[1]: Started Kubernetes systemd probe.
Aug 05 10:33:05 m-k8s kubelet[13489]: I0805 10:33:05.392442   13489 server.go:440] "Kubelet version" kubeletVersion="v1.22.0"
Aug 05 10:33:05 m-k8s kubelet[13489]: I0805 10:33:05.393039   13489 server.go:868] "Client rotation is on, will bootstrap in background"
Aug 05 10:33:05 m-k8s kubelet[13489]: I0805 10:33:05.399172   13489 certificate_store.go:130] Loading cert/key pair from "/var/lib/kubelet/pki/kubelet-client-current.pem".
Aug 05 10:33:05 m-k8s kubelet[13489]: I0805 10:33:05.401832   13489 dynamic_cafile_content.go:155] "Starting controller" name="client-ca-bundle::/etc/kubernetes/pki/ca.crt"
Aug 05 10:33:05 m-k8s kubelet[13489]: I0805 10:33:05.583166   13489 server.go:687] "--cgroups-per-qos enabled, but --cgroup-root was not specified.  defaulting to /"
Aug 05 10:33:05 m-k8s kubelet[13489]: I0805 10:33:05.583712   13489 container_manager_linux.go:280] "Container manager verified user specified cgroup-root exists" cgroupRoot=[]
Aug 05 10:33:05 m-k8s kubelet[13489]: I0805 10:33:05.583827   13489 container_manager_linux.go:285] "Creating Container Manager object based on Node Config" nodeConfig={RuntimeCgroupsName: SystemCgroupsName: Kub
Aug 05 10:33:05 m-k8s kubelet[13489]: I0805 10:33:05.584102   13489 topology_manager.go:133] "Creating topology manager with policy per scope" topologyPolicyName="none" topologyScopeName="container"
Aug 05 10:33:05 m-k8s kubelet[13489]: I0805 10:33:05.584128   13489 container_manager_linux.go:320] "Creating device plugin manager" devicePluginEnabled=true
Aug 05 10:33:05 m-k8s kubelet[13489]: I0805 10:33:05.584202   13489 state_mem.go:36] "Initialized new in-memory state store"
Aug 05 10:33:05 m-k8s kubelet[13489]: I0805 10:33:05.584293   13489 kubelet.go:314] "Using dockershim is deprecated, please consider using a full-fledged CRI implementation"
Aug 05 10:33:05 m-k8s kubelet[13489]: I0805 10:33:05.584622   13489 client.go:78] "Connecting to docker on the dockerEndpoint" endpoint="unix:///var/run/docker.sock"
Aug 05 10:33:05 m-k8s kubelet[13489]: I0805 10:33:05.584652   13489 client.go:97] "Start docker client with request timeout" timeout="2m0s"
Aug 05 10:33:05 m-k8s kubelet[13489]: I0805 10:33:05.609347   13489 docker_service.go:566] "Hairpin mode is set but kubenet is not enabled, falling back to HairpinVeth" hairpinMode=promiscuous-bridge
Aug 05 10:33:05 m-k8s kubelet[13489]: I0805 10:33:05.609388   13489 docker_service.go:242] "Hairpin mode is set" hairpinMode=hairpin-veth
Aug 05 10:33:05 m-k8s kubelet[13489]: I0805 10:33:05.609627   13489 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:33:05 m-k8s kubelet[13489]: I0805 10:33:05.618288   13489 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:33:05 m-k8s kubelet[13489]: I0805 10:33:05.618924   13489 docker_service.go:257] "Docker cri networking managed by the network plugin" networkPluginName="cni"
Aug 05 10:33:05 m-k8s kubelet[13489]: I0805 10:33:05.619416   13489 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:33:05 m-k8s kubelet[13489]: I0805 10:33:05.638366   13489 docker_service.go:264] "Docker Info" dockerInfo=&{ID:RXTP:LTUA:M4Z5:JXNJ:K3ID:KR2N:RKLR:PUSN:JUVB:IV7H:52EF:7G25 Containers:0 ContainersRunning
Aug 05 10:33:05 m-k8s kubelet[13489]: E0805 10:33:05.638416   13489 server.go:294] "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docke
Aug 05 10:33:05 m-k8s systemd[1]: kubelet.service: main process exited, code=exited, status=1/FAILURE
Aug 05 10:33:05 m-k8s systemd[1]: Unit kubelet.service entered failed state.
Aug 05 10:33:05 m-k8s systemd[1]: kubelet.service failed.
Aug 05 10:33:15 m-k8s systemd[1]: kubelet.service holdoff time over, scheduling restart.
Aug 05 10:33:15 m-k8s systemd[1]: Stopped kubelet: The Kubernetes Node Agent.
Aug 05 10:33:15 m-k8s systemd[1]: Started kubelet: The Kubernetes Node Agent.
Aug 05 10:33:15 m-k8s kubelet[13568]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:33:15 m-k8s kubelet[13568]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:33:15 m-k8s systemd[1]: Started Kubernetes systemd probe.
Aug 05 10:33:15 m-k8s kubelet[13568]: I0805 10:33:15.877487   13568 server.go:440] "Kubelet version" kubeletVersion="v1.22.0"
Aug 05 10:33:15 m-k8s kubelet[13568]: I0805 10:33:15.878493   13568 server.go:868] "Client rotation is on, will bootstrap in background"
Aug 05 10:33:15 m-k8s kubelet[13568]: I0805 10:33:15.884105   13568 certificate_store.go:130] Loading cert/key pair from "/var/lib/kubelet/pki/kubelet-client-current.pem".
Aug 05 10:33:15 m-k8s kubelet[13568]: I0805 10:33:15.886342   13568 dynamic_cafile_content.go:155] "Starting controller" name="client-ca-bundle::/etc/kubernetes/pki/ca.crt"
Aug 05 10:33:16 m-k8s kubelet[13568]: I0805 10:33:16.071986   13568 server.go:687] "--cgroups-per-qos enabled, but --cgroup-root was not specified.  defaulting to /"
Aug 05 10:33:16 m-k8s kubelet[13568]: I0805 10:33:16.072591   13568 container_manager_linux.go:280] "Container manager verified user specified cgroup-root exists" cgroupRoot=[]
Aug 05 10:33:16 m-k8s kubelet[13568]: I0805 10:33:16.072744   13568 container_manager_linux.go:285] "Creating Container Manager object based on Node Config" nodeConfig={RuntimeCgroupsName: SystemCgroupsName: Kub
Aug 05 10:33:16 m-k8s kubelet[13568]: I0805 10:33:16.072781   13568 topology_manager.go:133] "Creating topology manager with policy per scope" topologyPolicyName="none" topologyScopeName="container"
Aug 05 10:33:16 m-k8s kubelet[13568]: I0805 10:33:16.072810   13568 container_manager_linux.go:320] "Creating device plugin manager" devicePluginEnabled=true
Aug 05 10:33:16 m-k8s kubelet[13568]: I0805 10:33:16.072879   13568 state_mem.go:36] "Initialized new in-memory state store"
Aug 05 10:33:16 m-k8s kubelet[13568]: I0805 10:33:16.072966   13568 kubelet.go:314] "Using dockershim is deprecated, please consider using a full-fledged CRI implementation"
Aug 05 10:33:16 m-k8s kubelet[13568]: I0805 10:33:16.073008   13568 client.go:78] "Connecting to docker on the dockerEndpoint" endpoint="unix:///var/run/docker.sock"
Aug 05 10:33:16 m-k8s kubelet[13568]: I0805 10:33:16.073032   13568 client.go:97] "Start docker client with request timeout" timeout="2m0s"
Aug 05 10:33:16 m-k8s kubelet[13568]: I0805 10:33:16.103074   13568 docker_service.go:566] "Hairpin mode is set but kubenet is not enabled, falling back to HairpinVeth" hairpinMode=promiscuous-bridge
Aug 05 10:33:16 m-k8s kubelet[13568]: I0805 10:33:16.103115   13568 docker_service.go:242] "Hairpin mode is set" hairpinMode=hairpin-veth
Aug 05 10:33:16 m-k8s kubelet[13568]: I0805 10:33:16.103334   13568 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:33:16 m-k8s kubelet[13568]: I0805 10:33:16.113412   13568 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:33:16 m-k8s kubelet[13568]: I0805 10:33:16.113574   13568 docker_service.go:257] "Docker cri networking managed by the network plugin" networkPluginName="cni"
Aug 05 10:33:16 m-k8s kubelet[13568]: I0805 10:33:16.115860   13568 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:33:16 m-k8s kubelet[13568]: I0805 10:33:16.140249   13568 docker_service.go:264] "Docker Info" dockerInfo=&{ID:RXTP:LTUA:M4Z5:JXNJ:K3ID:KR2N:RKLR:PUSN:JUVB:IV7H:52EF:7G25 Containers:0 ContainersRunning
Aug 05 10:33:16 m-k8s kubelet[13568]: E0805 10:33:16.140326   13568 server.go:294] "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docke
Aug 05 10:33:16 m-k8s systemd[1]: kubelet.service: main process exited, code=exited, status=1/FAILURE
Aug 05 10:33:16 m-k8s systemd[1]: Unit kubelet.service entered failed state.
Aug 05 10:33:16 m-k8s systemd[1]: kubelet.service failed.
Aug 05 10:33:26 m-k8s systemd[1]: kubelet.service holdoff time over, scheduling restart.
Aug 05 10:33:26 m-k8s systemd[1]: Stopped kubelet: The Kubernetes Node Agent.
Aug 05 10:33:26 m-k8s systemd[1]: Started kubelet: The Kubernetes Node Agent.
Aug 05 10:33:26 m-k8s kubelet[13649]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:33:26 m-k8s kubelet[13649]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:33:26 m-k8s systemd[1]: Started Kubernetes systemd probe.
Aug 05 10:33:26 m-k8s kubelet[13649]: I0805 10:33:26.357496   13649 server.go:440] "Kubelet version" kubeletVersion="v1.22.0"
Aug 05 10:33:26 m-k8s kubelet[13649]: I0805 10:33:26.358024   13649 server.go:868] "Client rotation is on, will bootstrap in background"
Aug 05 10:33:26 m-k8s kubelet[13649]: I0805 10:33:26.363615   13649 certificate_store.go:130] Loading cert/key pair from "/var/lib/kubelet/pki/kubelet-client-current.pem".
Aug 05 10:33:26 m-k8s kubelet[13649]: I0805 10:33:26.365624   13649 dynamic_cafile_content.go:155] "Starting controller" name="client-ca-bundle::/etc/kubernetes/pki/ca.crt"
Aug 05 10:33:26 m-k8s kubelet[13649]: I0805 10:33:26.569016   13649 server.go:687] "--cgroups-per-qos enabled, but --cgroup-root was not specified.  defaulting to /"
Aug 05 10:33:26 m-k8s kubelet[13649]: I0805 10:33:26.569433   13649 container_manager_linux.go:280] "Container manager verified user specified cgroup-root exists" cgroupRoot=[]
Aug 05 10:33:26 m-k8s kubelet[13649]: I0805 10:33:26.569567   13649 container_manager_linux.go:285] "Creating Container Manager object based on Node Config" nodeConfig={RuntimeCgroupsName: SystemCgroupsName: Kub
Aug 05 10:33:26 m-k8s kubelet[13649]: I0805 10:33:26.569716   13649 topology_manager.go:133] "Creating topology manager with policy per scope" topologyPolicyName="none" topologyScopeName="container"
Aug 05 10:33:26 m-k8s kubelet[13649]: I0805 10:33:26.569744   13649 container_manager_linux.go:320] "Creating device plugin manager" devicePluginEnabled=true
Aug 05 10:33:26 m-k8s kubelet[13649]: I0805 10:33:26.569811   13649 state_mem.go:36] "Initialized new in-memory state store"
Aug 05 10:33:26 m-k8s kubelet[13649]: I0805 10:33:26.569903   13649 kubelet.go:314] "Using dockershim is deprecated, please consider using a full-fledged CRI implementation"
Aug 05 10:33:26 m-k8s kubelet[13649]: I0805 10:33:26.569945   13649 client.go:78] "Connecting to docker on the dockerEndpoint" endpoint="unix:///var/run/docker.sock"
Aug 05 10:33:26 m-k8s kubelet[13649]: I0805 10:33:26.569968   13649 client.go:97] "Start docker client with request timeout" timeout="2m0s"
Aug 05 10:33:26 m-k8s kubelet[13649]: I0805 10:33:26.591668   13649 docker_service.go:566] "Hairpin mode is set but kubenet is not enabled, falling back to HairpinVeth" hairpinMode=promiscuous-bridge
Aug 05 10:33:26 m-k8s kubelet[13649]: I0805 10:33:26.591738   13649 docker_service.go:242] "Hairpin mode is set" hairpinMode=hairpin-veth
Aug 05 10:33:26 m-k8s kubelet[13649]: I0805 10:33:26.592300   13649 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:33:26 m-k8s kubelet[13649]: I0805 10:33:26.611293   13649 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:33:26 m-k8s kubelet[13649]: I0805 10:33:26.613106   13649 docker_service.go:257] "Docker cri networking managed by the network plugin" networkPluginName="cni"
Aug 05 10:33:26 m-k8s kubelet[13649]: I0805 10:33:26.613146   13649 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:33:26 m-k8s kubelet[13649]: I0805 10:33:26.634236   13649 docker_service.go:264] "Docker Info" dockerInfo=&{ID:RXTP:LTUA:M4Z5:JXNJ:K3ID:KR2N:RKLR:PUSN:JUVB:IV7H:52EF:7G25 Containers:0 ContainersRunning
Aug 05 10:33:26 m-k8s kubelet[13649]: E0805 10:33:26.634297   13649 server.go:294] "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docke
Aug 05 10:33:26 m-k8s systemd[1]: kubelet.service: main process exited, code=exited, status=1/FAILURE
Aug 05 10:33:26 m-k8s systemd[1]: Unit kubelet.service entered failed state.
Aug 05 10:33:26 m-k8s systemd[1]: kubelet.service failed.
Aug 05 10:33:36 m-k8s systemd[1]: kubelet.service holdoff time over, scheduling restart.
Aug 05 10:33:36 m-k8s systemd[1]: Stopped kubelet: The Kubernetes Node Agent.
Aug 05 10:33:36 m-k8s systemd[1]: Started kubelet: The Kubernetes Node Agent.
Aug 05 10:33:36 m-k8s kubelet[13743]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:33:36 m-k8s kubelet[13743]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:33:36 m-k8s systemd[1]: Started Kubernetes systemd probe.
Aug 05 10:33:36 m-k8s kubelet[13743]: I0805 10:33:36.848174   13743 server.go:440] "Kubelet version" kubeletVersion="v1.22.0"
Aug 05 10:33:36 m-k8s kubelet[13743]: I0805 10:33:36.853053   13743 server.go:868] "Client rotation is on, will bootstrap in background"
Aug 05 10:33:36 m-k8s kubelet[13743]: I0805 10:33:36.860216   13743 certificate_store.go:130] Loading cert/key pair from "/var/lib/kubelet/pki/kubelet-client-current.pem".
Aug 05 10:33:36 m-k8s kubelet[13743]: I0805 10:33:36.864334   13743 dynamic_cafile_content.go:155] "Starting controller" name="client-ca-bundle::/etc/kubernetes/pki/ca.crt"
Aug 05 10:33:37 m-k8s kubelet[13743]: I0805 10:33:37.048274   13743 server.go:687] "--cgroups-per-qos enabled, but --cgroup-root was not specified.  defaulting to /"
Aug 05 10:33:37 m-k8s kubelet[13743]: I0805 10:33:37.048932   13743 container_manager_linux.go:280] "Container manager verified user specified cgroup-root exists" cgroupRoot=[]
Aug 05 10:33:37 m-k8s kubelet[13743]: I0805 10:33:37.049102   13743 container_manager_linux.go:285] "Creating Container Manager object based on Node Config" nodeConfig={RuntimeCgroupsName: SystemCgroupsName: Kub
Aug 05 10:33:37 m-k8s kubelet[13743]: I0805 10:33:37.049150   13743 topology_manager.go:133] "Creating topology manager with policy per scope" topologyPolicyName="none" topologyScopeName="container"
Aug 05 10:33:37 m-k8s kubelet[13743]: I0805 10:33:37.049179   13743 container_manager_linux.go:320] "Creating device plugin manager" devicePluginEnabled=true
Aug 05 10:33:37 m-k8s kubelet[13743]: I0805 10:33:37.049254   13743 state_mem.go:36] "Initialized new in-memory state store"
Aug 05 10:33:37 m-k8s kubelet[13743]: I0805 10:33:37.049345   13743 kubelet.go:314] "Using dockershim is deprecated, please consider using a full-fledged CRI implementation"
Aug 05 10:33:37 m-k8s kubelet[13743]: I0805 10:33:37.049387   13743 client.go:78] "Connecting to docker on the dockerEndpoint" endpoint="unix:///var/run/docker.sock"
Aug 05 10:33:37 m-k8s kubelet[13743]: I0805 10:33:37.049413   13743 client.go:97] "Start docker client with request timeout" timeout="2m0s"
Aug 05 10:33:37 m-k8s kubelet[13743]: I0805 10:33:37.073953   13743 docker_service.go:566] "Hairpin mode is set but kubenet is not enabled, falling back to HairpinVeth" hairpinMode=promiscuous-bridge
Aug 05 10:33:37 m-k8s kubelet[13743]: I0805 10:33:37.074002   13743 docker_service.go:242] "Hairpin mode is set" hairpinMode=hairpin-veth
Aug 05 10:33:37 m-k8s kubelet[13743]: I0805 10:33:37.074175   13743 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:33:37 m-k8s kubelet[13743]: I0805 10:33:37.081988   13743 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:33:37 m-k8s kubelet[13743]: I0805 10:33:37.082543   13743 docker_service.go:257] "Docker cri networking managed by the network plugin" networkPluginName="cni"
Aug 05 10:33:37 m-k8s kubelet[13743]: I0805 10:33:37.083022   13743 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:33:37 m-k8s kubelet[13743]: I0805 10:33:37.106782   13743 docker_service.go:264] "Docker Info" dockerInfo=&{ID:RXTP:LTUA:M4Z5:JXNJ:K3ID:KR2N:RKLR:PUSN:JUVB:IV7H:52EF:7G25 Containers:0 ContainersRunning
Aug 05 10:33:37 m-k8s kubelet[13743]: E0805 10:33:37.106837   13743 server.go:294] "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docke
Aug 05 10:33:37 m-k8s systemd[1]: kubelet.service: main process exited, code=exited, status=1/FAILURE
Aug 05 10:33:37 m-k8s systemd[1]: Unit kubelet.service entered failed state.
Aug 05 10:33:37 m-k8s systemd[1]: kubelet.service failed.
Aug 05 10:33:47 m-k8s systemd[1]: kubelet.service holdoff time over, scheduling restart.
Aug 05 10:33:47 m-k8s systemd[1]: Stopped kubelet: The Kubernetes Node Agent.
Aug 05 10:33:47 m-k8s systemd[1]: Started kubelet: The Kubernetes Node Agent.
Aug 05 10:33:47 m-k8s kubelet[13829]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:33:47 m-k8s kubelet[13829]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:33:47 m-k8s systemd[1]: Started Kubernetes systemd probe.
Aug 05 10:33:47 m-k8s kubelet[13829]: I0805 10:33:47.383623   13829 server.go:440] "Kubelet version" kubeletVersion="v1.22.0"
Aug 05 10:33:47 m-k8s kubelet[13829]: I0805 10:33:47.385162   13829 server.go:868] "Client rotation is on, will bootstrap in background"
Aug 05 10:33:47 m-k8s kubelet[13829]: I0805 10:33:47.394652   13829 certificate_store.go:130] Loading cert/key pair from "/var/lib/kubelet/pki/kubelet-client-current.pem".
Aug 05 10:33:47 m-k8s kubelet[13829]: I0805 10:33:47.405005   13829 dynamic_cafile_content.go:155] "Starting controller" name="client-ca-bundle::/etc/kubernetes/pki/ca.crt"
Aug 05 10:33:47 m-k8s kubelet[13829]: I0805 10:33:47.617780   13829 server.go:687] "--cgroups-per-qos enabled, but --cgroup-root was not specified.  defaulting to /"
Aug 05 10:33:47 m-k8s kubelet[13829]: I0805 10:33:47.618303   13829 container_manager_linux.go:280] "Container manager verified user specified cgroup-root exists" cgroupRoot=[]
Aug 05 10:33:47 m-k8s kubelet[13829]: I0805 10:33:47.618461   13829 container_manager_linux.go:285] "Creating Container Manager object based on Node Config" nodeConfig={RuntimeCgroupsName: SystemCgroupsName: Kub
Aug 05 10:33:47 m-k8s kubelet[13829]: I0805 10:33:47.618543   13829 topology_manager.go:133] "Creating topology manager with policy per scope" topologyPolicyName="none" topologyScopeName="container"
Aug 05 10:33:47 m-k8s kubelet[13829]: I0805 10:33:47.618586   13829 container_manager_linux.go:320] "Creating device plugin manager" devicePluginEnabled=true
Aug 05 10:33:47 m-k8s kubelet[13829]: I0805 10:33:47.618660   13829 state_mem.go:36] "Initialized new in-memory state store"
Aug 05 10:33:47 m-k8s kubelet[13829]: I0805 10:33:47.618743   13829 kubelet.go:314] "Using dockershim is deprecated, please consider using a full-fledged CRI implementation"
Aug 05 10:33:47 m-k8s kubelet[13829]: I0805 10:33:47.618780   13829 client.go:78] "Connecting to docker on the dockerEndpoint" endpoint="unix:///var/run/docker.sock"
Aug 05 10:33:47 m-k8s kubelet[13829]: I0805 10:33:47.618803   13829 client.go:97] "Start docker client with request timeout" timeout="2m0s"
Aug 05 10:33:47 m-k8s kubelet[13829]: I0805 10:33:47.640897   13829 docker_service.go:566] "Hairpin mode is set but kubenet is not enabled, falling back to HairpinVeth" hairpinMode=promiscuous-bridge
Aug 05 10:33:47 m-k8s kubelet[13829]: I0805 10:33:47.640993   13829 docker_service.go:242] "Hairpin mode is set" hairpinMode=hairpin-veth
Aug 05 10:33:47 m-k8s kubelet[13829]: I0805 10:33:47.641262   13829 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:33:47 m-k8s kubelet[13829]: I0805 10:33:47.649837   13829 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:33:47 m-k8s kubelet[13829]: I0805 10:33:47.649958   13829 docker_service.go:257] "Docker cri networking managed by the network plugin" networkPluginName="cni"
Aug 05 10:33:47 m-k8s kubelet[13829]: I0805 10:33:47.651780   13829 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:33:47 m-k8s kubelet[13829]: I0805 10:33:47.673515   13829 docker_service.go:264] "Docker Info" dockerInfo=&{ID:RXTP:LTUA:M4Z5:JXNJ:K3ID:KR2N:RKLR:PUSN:JUVB:IV7H:52EF:7G25 Containers:0 ContainersRunning
Aug 05 10:33:47 m-k8s kubelet[13829]: E0805 10:33:47.673590   13829 server.go:294] "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docke
Aug 05 10:33:47 m-k8s systemd[1]: kubelet.service: main process exited, code=exited, status=1/FAILURE
Aug 05 10:33:47 m-k8s systemd[1]: Unit kubelet.service entered failed state.
Aug 05 10:33:47 m-k8s systemd[1]: kubelet.service failed.
Aug 05 10:33:57 m-k8s systemd[1]: kubelet.service holdoff time over, scheduling restart.
Aug 05 10:33:57 m-k8s systemd[1]: Stopped kubelet: The Kubernetes Node Agent.
Aug 05 10:33:57 m-k8s systemd[1]: Started kubelet: The Kubernetes Node Agent.
Aug 05 10:33:58 m-k8s kubelet[13914]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:33:58 m-k8s kubelet[13914]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:33:58 m-k8s systemd[1]: Started Kubernetes systemd probe.
Aug 05 10:33:58 m-k8s kubelet[13914]: I0805 10:33:58.135121   13914 server.go:440] "Kubelet version" kubeletVersion="v1.22.0"
Aug 05 10:33:58 m-k8s kubelet[13914]: I0805 10:33:58.140526   13914 server.go:868] "Client rotation is on, will bootstrap in background"
Aug 05 10:33:58 m-k8s kubelet[13914]: I0805 10:33:58.150737   13914 certificate_store.go:130] Loading cert/key pair from "/var/lib/kubelet/pki/kubelet-client-current.pem".
Aug 05 10:33:58 m-k8s kubelet[13914]: I0805 10:33:58.153877   13914 dynamic_cafile_content.go:155] "Starting controller" name="client-ca-bundle::/etc/kubernetes/pki/ca.crt"
Aug 05 10:33:58 m-k8s kubelet[13914]: I0805 10:33:58.334737   13914 server.go:687] "--cgroups-per-qos enabled, but --cgroup-root was not specified.  defaulting to /"
Aug 05 10:33:58 m-k8s kubelet[13914]: I0805 10:33:58.335275   13914 container_manager_linux.go:280] "Container manager verified user specified cgroup-root exists" cgroupRoot=[]
Aug 05 10:33:58 m-k8s kubelet[13914]: I0805 10:33:58.335699   13914 container_manager_linux.go:285] "Creating Container Manager object based on Node Config" nodeConfig={RuntimeCgroupsName: SystemCgroupsName: Kub
Aug 05 10:33:58 m-k8s kubelet[13914]: I0805 10:33:58.335851   13914 topology_manager.go:133] "Creating topology manager with policy per scope" topologyPolicyName="none" topologyScopeName="container"
Aug 05 10:33:58 m-k8s kubelet[13914]: I0805 10:33:58.336043   13914 container_manager_linux.go:320] "Creating device plugin manager" devicePluginEnabled=true
Aug 05 10:33:58 m-k8s kubelet[13914]: I0805 10:33:58.336203   13914 state_mem.go:36] "Initialized new in-memory state store"
Aug 05 10:33:58 m-k8s kubelet[13914]: I0805 10:33:58.338974   13914 kubelet.go:314] "Using dockershim is deprecated, please consider using a full-fledged CRI implementation"
Aug 05 10:33:58 m-k8s kubelet[13914]: I0805 10:33:58.339039   13914 client.go:78] "Connecting to docker on the dockerEndpoint" endpoint="unix:///var/run/docker.sock"
Aug 05 10:33:58 m-k8s kubelet[13914]: I0805 10:33:58.339068   13914 client.go:97] "Start docker client with request timeout" timeout="2m0s"
Aug 05 10:33:58 m-k8s kubelet[13914]: I0805 10:33:58.364516   13914 docker_service.go:566] "Hairpin mode is set but kubenet is not enabled, falling back to HairpinVeth" hairpinMode=promiscuous-bridge
Aug 05 10:33:58 m-k8s kubelet[13914]: I0805 10:33:58.364610   13914 docker_service.go:242] "Hairpin mode is set" hairpinMode=hairpin-veth
Aug 05 10:33:58 m-k8s kubelet[13914]: I0805 10:33:58.364762   13914 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:33:58 m-k8s kubelet[13914]: I0805 10:33:58.372861   13914 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:33:58 m-k8s kubelet[13914]: I0805 10:33:58.373818   13914 docker_service.go:257] "Docker cri networking managed by the network plugin" networkPluginName="cni"
Aug 05 10:33:58 m-k8s kubelet[13914]: I0805 10:33:58.373861   13914 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:33:58 m-k8s kubelet[13914]: I0805 10:33:58.393785   13914 docker_service.go:264] "Docker Info" dockerInfo=&{ID:RXTP:LTUA:M4Z5:JXNJ:K3ID:KR2N:RKLR:PUSN:JUVB:IV7H:52EF:7G25 Containers:0 ContainersRunning
Aug 05 10:33:58 m-k8s kubelet[13914]: E0805 10:33:58.398347   13914 server.go:294] "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docke
Aug 05 10:33:58 m-k8s systemd[1]: kubelet.service: main process exited, code=exited, status=1/FAILURE
Aug 05 10:33:58 m-k8s systemd[1]: Unit kubelet.service entered failed state.
Aug 05 10:33:58 m-k8s systemd[1]: kubelet.service failed.
Aug 05 10:34:08 m-k8s systemd[1]: kubelet.service holdoff time over, scheduling restart.
Aug 05 10:34:08 m-k8s systemd[1]: Stopped kubelet: The Kubernetes Node Agent.
Aug 05 10:34:08 m-k8s systemd[1]: Started kubelet: The Kubernetes Node Agent.
Aug 05 10:34:08 m-k8s kubelet[14000]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:34:08 m-k8s kubelet[14000]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:34:08 m-k8s systemd[1]: Started Kubernetes systemd probe.
Aug 05 10:34:08 m-k8s kubelet[14000]: I0805 10:34:08.601842   14000 server.go:440] "Kubelet version" kubeletVersion="v1.22.0"
Aug 05 10:34:08 m-k8s kubelet[14000]: I0805 10:34:08.602782   14000 server.go:868] "Client rotation is on, will bootstrap in background"
Aug 05 10:34:08 m-k8s kubelet[14000]: I0805 10:34:08.608822   14000 certificate_store.go:130] Loading cert/key pair from "/var/lib/kubelet/pki/kubelet-client-current.pem".
Aug 05 10:34:08 m-k8s kubelet[14000]: I0805 10:34:08.619203   14000 dynamic_cafile_content.go:155] "Starting controller" name="client-ca-bundle::/etc/kubernetes/pki/ca.crt"
Aug 05 10:34:08 m-k8s kubelet[14000]: I0805 10:34:08.783213   14000 server.go:687] "--cgroups-per-qos enabled, but --cgroup-root was not specified.  defaulting to /"
Aug 05 10:34:08 m-k8s kubelet[14000]: I0805 10:34:08.783818   14000 container_manager_linux.go:280] "Container manager verified user specified cgroup-root exists" cgroupRoot=[]
Aug 05 10:34:08 m-k8s kubelet[14000]: I0805 10:34:08.783949   14000 container_manager_linux.go:285] "Creating Container Manager object based on Node Config" nodeConfig={RuntimeCgroupsName: SystemCgroupsName: Kub
Aug 05 10:34:08 m-k8s kubelet[14000]: I0805 10:34:08.783978   14000 topology_manager.go:133] "Creating topology manager with policy per scope" topologyPolicyName="none" topologyScopeName="container"
Aug 05 10:34:08 m-k8s kubelet[14000]: I0805 10:34:08.783995   14000 container_manager_linux.go:320] "Creating device plugin manager" devicePluginEnabled=true
Aug 05 10:34:08 m-k8s kubelet[14000]: I0805 10:34:08.784053   14000 state_mem.go:36] "Initialized new in-memory state store"
Aug 05 10:34:08 m-k8s kubelet[14000]: I0805 10:34:08.784129   14000 kubelet.go:314] "Using dockershim is deprecated, please consider using a full-fledged CRI implementation"
Aug 05 10:34:08 m-k8s kubelet[14000]: I0805 10:34:08.784177   14000 client.go:78] "Connecting to docker on the dockerEndpoint" endpoint="unix:///var/run/docker.sock"
Aug 05 10:34:08 m-k8s kubelet[14000]: I0805 10:34:08.784204   14000 client.go:97] "Start docker client with request timeout" timeout="2m0s"
Aug 05 10:34:08 m-k8s kubelet[14000]: I0805 10:34:08.806404   14000 docker_service.go:566] "Hairpin mode is set but kubenet is not enabled, falling back to HairpinVeth" hairpinMode=promiscuous-bridge
Aug 05 10:34:08 m-k8s kubelet[14000]: I0805 10:34:08.806522   14000 docker_service.go:242] "Hairpin mode is set" hairpinMode=hairpin-veth
Aug 05 10:34:08 m-k8s kubelet[14000]: I0805 10:34:08.806727   14000 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:34:08 m-k8s kubelet[14000]: I0805 10:34:08.815814   14000 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:34:08 m-k8s kubelet[14000]: I0805 10:34:08.816080   14000 docker_service.go:257] "Docker cri networking managed by the network plugin" networkPluginName="cni"
Aug 05 10:34:08 m-k8s kubelet[14000]: I0805 10:34:08.817577   14000 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:34:08 m-k8s kubelet[14000]: I0805 10:34:08.841613   14000 docker_service.go:264] "Docker Info" dockerInfo=&{ID:RXTP:LTUA:M4Z5:JXNJ:K3ID:KR2N:RKLR:PUSN:JUVB:IV7H:52EF:7G25 Containers:0 ContainersRunning
Aug 05 10:34:08 m-k8s kubelet[14000]: E0805 10:34:08.841682   14000 server.go:294] "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docke
Aug 05 10:34:08 m-k8s systemd[1]: kubelet.service: main process exited, code=exited, status=1/FAILURE
Aug 05 10:34:08 m-k8s systemd[1]: Unit kubelet.service entered failed state.
Aug 05 10:34:08 m-k8s systemd[1]: kubelet.service failed.
Aug 05 10:34:18 m-k8s systemd[1]: kubelet.service holdoff time over, scheduling restart.
Aug 05 10:34:18 m-k8s systemd[1]: Stopped kubelet: The Kubernetes Node Agent.
Aug 05 10:34:18 m-k8s systemd[1]: Started kubelet: The Kubernetes Node Agent.
Aug 05 10:34:19 m-k8s kubelet[14081]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:34:19 m-k8s kubelet[14081]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:34:19 m-k8s systemd[1]: Started Kubernetes systemd probe.
Aug 05 10:34:19 m-k8s kubelet[14081]: I0805 10:34:19.115443   14081 server.go:440] "Kubelet version" kubeletVersion="v1.22.0"
Aug 05 10:34:19 m-k8s kubelet[14081]: I0805 10:34:19.116103   14081 server.go:868] "Client rotation is on, will bootstrap in background"
Aug 05 10:34:19 m-k8s kubelet[14081]: I0805 10:34:19.122723   14081 certificate_store.go:130] Loading cert/key pair from "/var/lib/kubelet/pki/kubelet-client-current.pem".
Aug 05 10:34:19 m-k8s kubelet[14081]: I0805 10:34:19.125952   14081 dynamic_cafile_content.go:155] "Starting controller" name="client-ca-bundle::/etc/kubernetes/pki/ca.crt"
Aug 05 10:34:19 m-k8s kubelet[14081]: I0805 10:34:19.310619   14081 server.go:687] "--cgroups-per-qos enabled, but --cgroup-root was not specified.  defaulting to /"
Aug 05 10:34:19 m-k8s kubelet[14081]: I0805 10:34:19.311073   14081 container_manager_linux.go:280] "Container manager verified user specified cgroup-root exists" cgroupRoot=[]
Aug 05 10:34:19 m-k8s kubelet[14081]: I0805 10:34:19.311267   14081 container_manager_linux.go:285] "Creating Container Manager object based on Node Config" nodeConfig={RuntimeCgroupsName: SystemCgroupsName: Kub
Aug 05 10:34:19 m-k8s kubelet[14081]: I0805 10:34:19.311354   14081 topology_manager.go:133] "Creating topology manager with policy per scope" topologyPolicyName="none" topologyScopeName="container"
Aug 05 10:34:19 m-k8s kubelet[14081]: I0805 10:34:19.311388   14081 container_manager_linux.go:320] "Creating device plugin manager" devicePluginEnabled=true
Aug 05 10:34:19 m-k8s kubelet[14081]: I0805 10:34:19.311468   14081 state_mem.go:36] "Initialized new in-memory state store"
Aug 05 10:34:19 m-k8s kubelet[14081]: I0805 10:34:19.312680   14081 kubelet.go:314] "Using dockershim is deprecated, please consider using a full-fledged CRI implementation"
Aug 05 10:34:19 m-k8s kubelet[14081]: I0805 10:34:19.312745   14081 client.go:78] "Connecting to docker on the dockerEndpoint" endpoint="unix:///var/run/docker.sock"
Aug 05 10:34:19 m-k8s kubelet[14081]: I0805 10:34:19.312783   14081 client.go:97] "Start docker client with request timeout" timeout="2m0s"
Aug 05 10:34:19 m-k8s kubelet[14081]: I0805 10:34:19.340186   14081 docker_service.go:566] "Hairpin mode is set but kubenet is not enabled, falling back to HairpinVeth" hairpinMode=promiscuous-bridge
Aug 05 10:34:19 m-k8s kubelet[14081]: I0805 10:34:19.340262   14081 docker_service.go:242] "Hairpin mode is set" hairpinMode=hairpin-veth
Aug 05 10:34:19 m-k8s kubelet[14081]: I0805 10:34:19.340505   14081 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:34:19 m-k8s kubelet[14081]: I0805 10:34:19.349429   14081 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:34:19 m-k8s kubelet[14081]: I0805 10:34:19.349675   14081 docker_service.go:257] "Docker cri networking managed by the network plugin" networkPluginName="cni"
Aug 05 10:34:19 m-k8s kubelet[14081]: I0805 10:34:19.349834   14081 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:34:19 m-k8s kubelet[14081]: I0805 10:34:19.375845   14081 docker_service.go:264] "Docker Info" dockerInfo=&{ID:RXTP:LTUA:M4Z5:JXNJ:K3ID:KR2N:RKLR:PUSN:JUVB:IV7H:52EF:7G25 Containers:0 ContainersRunning
Aug 05 10:34:19 m-k8s kubelet[14081]: E0805 10:34:19.375919   14081 server.go:294] "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docke
Aug 05 10:34:19 m-k8s systemd[1]: kubelet.service: main process exited, code=exited, status=1/FAILURE
Aug 05 10:34:19 m-k8s systemd[1]: Unit kubelet.service entered failed state.
Aug 05 10:34:19 m-k8s systemd[1]: kubelet.service failed.
Aug 05 10:34:29 m-k8s systemd[1]: kubelet.service holdoff time over, scheduling restart.
Aug 05 10:34:29 m-k8s systemd[1]: Stopped kubelet: The Kubernetes Node Agent.
Aug 05 10:34:29 m-k8s systemd[1]: Started kubelet: The Kubernetes Node Agent.
Aug 05 10:34:29 m-k8s kubelet[14160]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:34:29 m-k8s kubelet[14160]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:34:29 m-k8s systemd[1]: Started Kubernetes systemd probe.
Aug 05 10:34:29 m-k8s kubelet[14160]: I0805 10:34:29.893092   14160 server.go:440] "Kubelet version" kubeletVersion="v1.22.0"
Aug 05 10:34:29 m-k8s kubelet[14160]: I0805 10:34:29.893682   14160 server.go:868] "Client rotation is on, will bootstrap in background"
Aug 05 10:34:29 m-k8s kubelet[14160]: I0805 10:34:29.902600   14160 certificate_store.go:130] Loading cert/key pair from "/var/lib/kubelet/pki/kubelet-client-current.pem".
Aug 05 10:34:29 m-k8s kubelet[14160]: I0805 10:34:29.905562   14160 dynamic_cafile_content.go:155] "Starting controller" name="client-ca-bundle::/etc/kubernetes/pki/ca.crt"
Aug 05 10:34:30 m-k8s kubelet[14160]: I0805 10:34:30.187421   14160 server.go:687] "--cgroups-per-qos enabled, but --cgroup-root was not specified.  defaulting to /"
Aug 05 10:34:30 m-k8s kubelet[14160]: I0805 10:34:30.187887   14160 container_manager_linux.go:280] "Container manager verified user specified cgroup-root exists" cgroupRoot=[]
Aug 05 10:34:30 m-k8s kubelet[14160]: I0805 10:34:30.188324   14160 container_manager_linux.go:285] "Creating Container Manager object based on Node Config" nodeConfig={RuntimeCgroupsName: SystemCgroupsName: Kub
Aug 05 10:34:30 m-k8s kubelet[14160]: I0805 10:34:30.188377   14160 topology_manager.go:133] "Creating topology manager with policy per scope" topologyPolicyName="none" topologyScopeName="container"
Aug 05 10:34:30 m-k8s kubelet[14160]: I0805 10:34:30.188404   14160 container_manager_linux.go:320] "Creating device plugin manager" devicePluginEnabled=true
Aug 05 10:34:30 m-k8s kubelet[14160]: I0805 10:34:30.188481   14160 state_mem.go:36] "Initialized new in-memory state store"
Aug 05 10:34:30 m-k8s kubelet[14160]: I0805 10:34:30.188578   14160 kubelet.go:314] "Using dockershim is deprecated, please consider using a full-fledged CRI implementation"
Aug 05 10:34:30 m-k8s kubelet[14160]: I0805 10:34:30.188625   14160 client.go:78] "Connecting to docker on the dockerEndpoint" endpoint="unix:///var/run/docker.sock"
Aug 05 10:34:30 m-k8s kubelet[14160]: I0805 10:34:30.188657   14160 client.go:97] "Start docker client with request timeout" timeout="2m0s"
Aug 05 10:34:30 m-k8s kubelet[14160]: I0805 10:34:30.242432   14160 docker_service.go:566] "Hairpin mode is set but kubenet is not enabled, falling back to HairpinVeth" hairpinMode=promiscuous-bridge
Aug 05 10:34:30 m-k8s kubelet[14160]: I0805 10:34:30.242483   14160 docker_service.go:242] "Hairpin mode is set" hairpinMode=hairpin-veth
Aug 05 10:34:30 m-k8s kubelet[14160]: I0805 10:34:30.242664   14160 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:34:30 m-k8s kubelet[14160]: I0805 10:34:30.253232   14160 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:34:30 m-k8s kubelet[14160]: I0805 10:34:30.253413   14160 docker_service.go:257] "Docker cri networking managed by the network plugin" networkPluginName="cni"
Aug 05 10:34:30 m-k8s kubelet[14160]: I0805 10:34:30.253571   14160 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:34:30 m-k8s kubelet[14160]: I0805 10:34:30.279627   14160 docker_service.go:264] "Docker Info" dockerInfo=&{ID:RXTP:LTUA:M4Z5:JXNJ:K3ID:KR2N:RKLR:PUSN:JUVB:IV7H:52EF:7G25 Containers:0 ContainersRunning
Aug 05 10:34:30 m-k8s kubelet[14160]: E0805 10:34:30.279789   14160 server.go:294] "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docke
Aug 05 10:34:30 m-k8s systemd[1]: kubelet.service: main process exited, code=exited, status=1/FAILURE
Aug 05 10:34:30 m-k8s systemd[1]: Unit kubelet.service entered failed state.
Aug 05 10:34:30 m-k8s systemd[1]: kubelet.service failed.
Aug 05 10:34:40 m-k8s systemd[1]: kubelet.service holdoff time over, scheduling restart.
Aug 05 10:34:40 m-k8s systemd[1]: Stopped kubelet: The Kubernetes Node Agent.
Aug 05 10:34:40 m-k8s systemd[1]: Started kubelet: The Kubernetes Node Agent.
Aug 05 10:34:40 m-k8s kubelet[14241]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:34:40 m-k8s kubelet[14241]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:34:40 m-k8s systemd[1]: Started Kubernetes systemd probe.
Aug 05 10:34:40 m-k8s kubelet[14241]: I0805 10:34:40.614199   14241 server.go:440] "Kubelet version" kubeletVersion="v1.22.0"
Aug 05 10:34:40 m-k8s kubelet[14241]: I0805 10:34:40.614745   14241 server.go:868] "Client rotation is on, will bootstrap in background"
Aug 05 10:34:40 m-k8s kubelet[14241]: I0805 10:34:40.620891   14241 certificate_store.go:130] Loading cert/key pair from "/var/lib/kubelet/pki/kubelet-client-current.pem".
Aug 05 10:34:40 m-k8s kubelet[14241]: I0805 10:34:40.626430   14241 dynamic_cafile_content.go:155] "Starting controller" name="client-ca-bundle::/etc/kubernetes/pki/ca.crt"
Aug 05 10:34:40 m-k8s kubelet[14241]: I0805 10:34:40.796449   14241 server.go:687] "--cgroups-per-qos enabled, but --cgroup-root was not specified.  defaulting to /"
Aug 05 10:34:40 m-k8s kubelet[14241]: I0805 10:34:40.797010   14241 container_manager_linux.go:280] "Container manager verified user specified cgroup-root exists" cgroupRoot=[]
Aug 05 10:34:40 m-k8s kubelet[14241]: I0805 10:34:40.797251   14241 container_manager_linux.go:285] "Creating Container Manager object based on Node Config" nodeConfig={RuntimeCgroupsName: SystemCgroupsName: Kub
Aug 05 10:34:40 m-k8s kubelet[14241]: I0805 10:34:40.797293   14241 topology_manager.go:133] "Creating topology manager with policy per scope" topologyPolicyName="none" topologyScopeName="container"
Aug 05 10:34:40 m-k8s kubelet[14241]: I0805 10:34:40.797316   14241 container_manager_linux.go:320] "Creating device plugin manager" devicePluginEnabled=true
Aug 05 10:34:40 m-k8s kubelet[14241]: I0805 10:34:40.797389   14241 state_mem.go:36] "Initialized new in-memory state store"
Aug 05 10:34:40 m-k8s kubelet[14241]: I0805 10:34:40.797486   14241 kubelet.go:314] "Using dockershim is deprecated, please consider using a full-fledged CRI implementation"
Aug 05 10:34:40 m-k8s kubelet[14241]: I0805 10:34:40.797531   14241 client.go:78] "Connecting to docker on the dockerEndpoint" endpoint="unix:///var/run/docker.sock"
Aug 05 10:34:40 m-k8s kubelet[14241]: I0805 10:34:40.797558   14241 client.go:97] "Start docker client with request timeout" timeout="2m0s"
Aug 05 10:34:40 m-k8s kubelet[14241]: I0805 10:34:40.821016   14241 docker_service.go:566] "Hairpin mode is set but kubenet is not enabled, falling back to HairpinVeth" hairpinMode=promiscuous-bridge
Aug 05 10:34:40 m-k8s kubelet[14241]: I0805 10:34:40.821065   14241 docker_service.go:242] "Hairpin mode is set" hairpinMode=hairpin-veth
Aug 05 10:34:40 m-k8s kubelet[14241]: I0805 10:34:40.821290   14241 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:34:40 m-k8s kubelet[14241]: I0805 10:34:40.831095   14241 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:34:40 m-k8s kubelet[14241]: I0805 10:34:40.831339   14241 docker_service.go:257] "Docker cri networking managed by the network plugin" networkPluginName="cni"
Aug 05 10:34:40 m-k8s kubelet[14241]: I0805 10:34:40.831499   14241 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:34:40 m-k8s kubelet[14241]: I0805 10:34:40.858251   14241 docker_service.go:264] "Docker Info" dockerInfo=&{ID:RXTP:LTUA:M4Z5:JXNJ:K3ID:KR2N:RKLR:PUSN:JUVB:IV7H:52EF:7G25 Containers:0 ContainersRunning
Aug 05 10:34:40 m-k8s kubelet[14241]: E0805 10:34:40.858326   14241 server.go:294] "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docke
Aug 05 10:34:40 m-k8s systemd[1]: kubelet.service: main process exited, code=exited, status=1/FAILURE
Aug 05 10:34:40 m-k8s systemd[1]: Unit kubelet.service entered failed state.
Aug 05 10:34:40 m-k8s systemd[1]: kubelet.service failed.
Aug 05 10:34:50 m-k8s systemd[1]: kubelet.service holdoff time over, scheduling restart.
Aug 05 10:34:50 m-k8s systemd[1]: Stopped kubelet: The Kubernetes Node Agent.
Aug 05 10:34:50 m-k8s systemd[1]: Started kubelet: The Kubernetes Node Agent.
Aug 05 10:34:51 m-k8s kubelet[14321]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:34:51 m-k8s kubelet[14321]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:34:51 m-k8s systemd[1]: Started Kubernetes systemd probe.
Aug 05 10:34:51 m-k8s kubelet[14321]: I0805 10:34:51.108300   14321 server.go:440] "Kubelet version" kubeletVersion="v1.22.0"
Aug 05 10:34:51 m-k8s kubelet[14321]: I0805 10:34:51.108829   14321 server.go:868] "Client rotation is on, will bootstrap in background"
Aug 05 10:34:51 m-k8s kubelet[14321]: I0805 10:34:51.115827   14321 certificate_store.go:130] Loading cert/key pair from "/var/lib/kubelet/pki/kubelet-client-current.pem".
Aug 05 10:34:51 m-k8s kubelet[14321]: I0805 10:34:51.120217   14321 dynamic_cafile_content.go:155] "Starting controller" name="client-ca-bundle::/etc/kubernetes/pki/ca.crt"
Aug 05 10:34:51 m-k8s kubelet[14321]: I0805 10:34:51.290840   14321 server.go:687] "--cgroups-per-qos enabled, but --cgroup-root was not specified.  defaulting to /"
Aug 05 10:34:51 m-k8s kubelet[14321]: I0805 10:34:51.291265   14321 container_manager_linux.go:280] "Container manager verified user specified cgroup-root exists" cgroupRoot=[]
Aug 05 10:34:51 m-k8s kubelet[14321]: I0805 10:34:51.291488   14321 container_manager_linux.go:285] "Creating Container Manager object based on Node Config" nodeConfig={RuntimeCgroupsName: SystemCgroupsName: Kub
Aug 05 10:34:51 m-k8s kubelet[14321]: I0805 10:34:51.291537   14321 topology_manager.go:133] "Creating topology manager with policy per scope" topologyPolicyName="none" topologyScopeName="container"
Aug 05 10:34:51 m-k8s kubelet[14321]: I0805 10:34:51.291563   14321 container_manager_linux.go:320] "Creating device plugin manager" devicePluginEnabled=true
Aug 05 10:34:51 m-k8s kubelet[14321]: I0805 10:34:51.291643   14321 state_mem.go:36] "Initialized new in-memory state store"
Aug 05 10:34:51 m-k8s kubelet[14321]: I0805 10:34:51.291756   14321 kubelet.go:314] "Using dockershim is deprecated, please consider using a full-fledged CRI implementation"
Aug 05 10:34:51 m-k8s kubelet[14321]: I0805 10:34:51.291802   14321 client.go:78] "Connecting to docker on the dockerEndpoint" endpoint="unix:///var/run/docker.sock"
Aug 05 10:34:51 m-k8s kubelet[14321]: I0805 10:34:51.291829   14321 client.go:97] "Start docker client with request timeout" timeout="2m0s"
Aug 05 10:34:51 m-k8s kubelet[14321]: I0805 10:34:51.319388   14321 docker_service.go:566] "Hairpin mode is set but kubenet is not enabled, falling back to HairpinVeth" hairpinMode=promiscuous-bridge
Aug 05 10:34:51 m-k8s kubelet[14321]: I0805 10:34:51.319427   14321 docker_service.go:242] "Hairpin mode is set" hairpinMode=hairpin-veth
Aug 05 10:34:51 m-k8s kubelet[14321]: I0805 10:34:51.319584   14321 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:34:51 m-k8s kubelet[14321]: I0805 10:34:51.330333   14321 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:34:51 m-k8s kubelet[14321]: I0805 10:34:51.331315   14321 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:34:51 m-k8s kubelet[14321]: I0805 10:34:51.331318   14321 docker_service.go:257] "Docker cri networking managed by the network plugin" networkPluginName="cni"
Aug 05 10:34:51 m-k8s kubelet[14321]: I0805 10:34:51.360365   14321 docker_service.go:264] "Docker Info" dockerInfo=&{ID:RXTP:LTUA:M4Z5:JXNJ:K3ID:KR2N:RKLR:PUSN:JUVB:IV7H:52EF:7G25 Containers:0 ContainersRunning
Aug 05 10:34:51 m-k8s kubelet[14321]: E0805 10:34:51.360418   14321 server.go:294] "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docke
Aug 05 10:34:51 m-k8s systemd[1]: kubelet.service: main process exited, code=exited, status=1/FAILURE
Aug 05 10:34:51 m-k8s systemd[1]: Unit kubelet.service entered failed state.
Aug 05 10:34:51 m-k8s systemd[1]: kubelet.service failed.
Aug 05 10:35:01 m-k8s systemd[1]: kubelet.service holdoff time over, scheduling restart.
Aug 05 10:35:01 m-k8s systemd[1]: Stopped kubelet: The Kubernetes Node Agent.
Aug 05 10:35:01 m-k8s systemd[1]: Started kubelet: The Kubernetes Node Agent.
Aug 05 10:35:01 m-k8s kubelet[14402]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:35:01 m-k8s kubelet[14402]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:35:01 m-k8s systemd[1]: Started Kubernetes systemd probe.
Aug 05 10:35:01 m-k8s kubelet[14402]: I0805 10:35:01.647539   14402 server.go:440] "Kubelet version" kubeletVersion="v1.22.0"
Aug 05 10:35:01 m-k8s kubelet[14402]: I0805 10:35:01.648175   14402 server.go:868] "Client rotation is on, will bootstrap in background"
Aug 05 10:35:01 m-k8s kubelet[14402]: I0805 10:35:01.656215   14402 certificate_store.go:130] Loading cert/key pair from "/var/lib/kubelet/pki/kubelet-client-current.pem".
Aug 05 10:35:01 m-k8s kubelet[14402]: I0805 10:35:01.663895   14402 dynamic_cafile_content.go:155] "Starting controller" name="client-ca-bundle::/etc/kubernetes/pki/ca.crt"
Aug 05 10:35:01 m-k8s kubelet[14402]: I0805 10:35:01.835153   14402 server.go:687] "--cgroups-per-qos enabled, but --cgroup-root was not specified.  defaulting to /"
Aug 05 10:35:01 m-k8s kubelet[14402]: I0805 10:35:01.835640   14402 container_manager_linux.go:280] "Container manager verified user specified cgroup-root exists" cgroupRoot=[]
Aug 05 10:35:01 m-k8s kubelet[14402]: I0805 10:35:01.835951   14402 container_manager_linux.go:285] "Creating Container Manager object based on Node Config" nodeConfig={RuntimeCgroupsName: SystemCgroupsName: Kub
Aug 05 10:35:01 m-k8s kubelet[14402]: I0805 10:35:01.836004   14402 topology_manager.go:133] "Creating topology manager with policy per scope" topologyPolicyName="none" topologyScopeName="container"
Aug 05 10:35:01 m-k8s kubelet[14402]: I0805 10:35:01.836030   14402 container_manager_linux.go:320] "Creating device plugin manager" devicePluginEnabled=true
Aug 05 10:35:01 m-k8s kubelet[14402]: I0805 10:35:01.836098   14402 state_mem.go:36] "Initialized new in-memory state store"
Aug 05 10:35:01 m-k8s kubelet[14402]: I0805 10:35:01.836185   14402 kubelet.go:314] "Using dockershim is deprecated, please consider using a full-fledged CRI implementation"
Aug 05 10:35:01 m-k8s kubelet[14402]: I0805 10:35:01.836219   14402 client.go:78] "Connecting to docker on the dockerEndpoint" endpoint="unix:///var/run/docker.sock"
Aug 05 10:35:01 m-k8s kubelet[14402]: I0805 10:35:01.836239   14402 client.go:97] "Start docker client with request timeout" timeout="2m0s"
Aug 05 10:35:01 m-k8s kubelet[14402]: I0805 10:35:01.863191   14402 docker_service.go:566] "Hairpin mode is set but kubenet is not enabled, falling back to HairpinVeth" hairpinMode=promiscuous-bridge
Aug 05 10:35:01 m-k8s kubelet[14402]: I0805 10:35:01.863240   14402 docker_service.go:242] "Hairpin mode is set" hairpinMode=hairpin-veth
Aug 05 10:35:01 m-k8s kubelet[14402]: I0805 10:35:01.863506   14402 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:35:01 m-k8s kubelet[14402]: I0805 10:35:01.872542   14402 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:35:01 m-k8s kubelet[14402]: I0805 10:35:01.872813   14402 docker_service.go:257] "Docker cri networking managed by the network plugin" networkPluginName="cni"
Aug 05 10:35:01 m-k8s kubelet[14402]: I0805 10:35:01.872981   14402 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:35:01 m-k8s kubelet[14402]: I0805 10:35:01.895678   14402 docker_service.go:264] "Docker Info" dockerInfo=&{ID:RXTP:LTUA:M4Z5:JXNJ:K3ID:KR2N:RKLR:PUSN:JUVB:IV7H:52EF:7G25 Containers:0 ContainersRunning
Aug 05 10:35:01 m-k8s kubelet[14402]: E0805 10:35:01.895753   14402 server.go:294] "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docke
Aug 05 10:35:01 m-k8s systemd[1]: kubelet.service: main process exited, code=exited, status=1/FAILURE
Aug 05 10:35:01 m-k8s systemd[1]: Unit kubelet.service entered failed state.
Aug 05 10:35:01 m-k8s systemd[1]: kubelet.service failed.
Aug 05 10:35:11 m-k8s systemd[1]: kubelet.service holdoff time over, scheduling restart.
Aug 05 10:35:11 m-k8s systemd[1]: Stopped kubelet: The Kubernetes Node Agent.
Aug 05 10:35:11 m-k8s systemd[1]: Started kubelet: The Kubernetes Node Agent.
Aug 05 10:35:12 m-k8s kubelet[14484]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:35:12 m-k8s kubelet[14484]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:35:12 m-k8s systemd[1]: Started Kubernetes systemd probe.
Aug 05 10:35:12 m-k8s kubelet[14484]: I0805 10:35:12.099893   14484 server.go:440] "Kubelet version" kubeletVersion="v1.22.0"
Aug 05 10:35:12 m-k8s kubelet[14484]: I0805 10:35:12.101925   14484 server.go:868] "Client rotation is on, will bootstrap in background"
Aug 05 10:35:12 m-k8s kubelet[14484]: I0805 10:35:12.107317   14484 certificate_store.go:130] Loading cert/key pair from "/var/lib/kubelet/pki/kubelet-client-current.pem".
Aug 05 10:35:12 m-k8s kubelet[14484]: I0805 10:35:12.109868   14484 dynamic_cafile_content.go:155] "Starting controller" name="client-ca-bundle::/etc/kubernetes/pki/ca.crt"
Aug 05 10:35:12 m-k8s kubelet[14484]: I0805 10:35:12.299323   14484 server.go:687] "--cgroups-per-qos enabled, but --cgroup-root was not specified.  defaulting to /"
Aug 05 10:35:12 m-k8s kubelet[14484]: I0805 10:35:12.300659   14484 container_manager_linux.go:280] "Container manager verified user specified cgroup-root exists" cgroupRoot=[]
Aug 05 10:35:12 m-k8s kubelet[14484]: I0805 10:35:12.300849   14484 container_manager_linux.go:285] "Creating Container Manager object based on Node Config" nodeConfig={RuntimeCgroupsName: SystemCgroupsName: Kub
Aug 05 10:35:12 m-k8s kubelet[14484]: I0805 10:35:12.300891   14484 topology_manager.go:133] "Creating topology manager with policy per scope" topologyPolicyName="none" topologyScopeName="container"
Aug 05 10:35:12 m-k8s kubelet[14484]: I0805 10:35:12.300957   14484 container_manager_linux.go:320] "Creating device plugin manager" devicePluginEnabled=true
Aug 05 10:35:12 m-k8s kubelet[14484]: I0805 10:35:12.301119   14484 state_mem.go:36] "Initialized new in-memory state store"
Aug 05 10:35:12 m-k8s kubelet[14484]: I0805 10:35:12.301216   14484 kubelet.go:314] "Using dockershim is deprecated, please consider using a full-fledged CRI implementation"
Aug 05 10:35:12 m-k8s kubelet[14484]: I0805 10:35:12.301259   14484 client.go:78] "Connecting to docker on the dockerEndpoint" endpoint="unix:///var/run/docker.sock"
Aug 05 10:35:12 m-k8s kubelet[14484]: I0805 10:35:12.301318   14484 client.go:97] "Start docker client with request timeout" timeout="2m0s"
Aug 05 10:35:12 m-k8s kubelet[14484]: I0805 10:35:12.324488   14484 docker_service.go:566] "Hairpin mode is set but kubenet is not enabled, falling back to HairpinVeth" hairpinMode=promiscuous-bridge
Aug 05 10:35:12 m-k8s kubelet[14484]: I0805 10:35:12.324594   14484 docker_service.go:242] "Hairpin mode is set" hairpinMode=hairpin-veth
Aug 05 10:35:12 m-k8s kubelet[14484]: I0805 10:35:12.324866   14484 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:35:12 m-k8s kubelet[14484]: I0805 10:35:12.332062   14484 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:35:12 m-k8s kubelet[14484]: I0805 10:35:12.333093   14484 docker_service.go:257] "Docker cri networking managed by the network plugin" networkPluginName="cni"
Aug 05 10:35:12 m-k8s kubelet[14484]: I0805 10:35:12.333668   14484 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:35:12 m-k8s kubelet[14484]: I0805 10:35:12.354649   14484 docker_service.go:264] "Docker Info" dockerInfo=&{ID:RXTP:LTUA:M4Z5:JXNJ:K3ID:KR2N:RKLR:PUSN:JUVB:IV7H:52EF:7G25 Containers:0 ContainersRunning
Aug 05 10:35:12 m-k8s systemd[1]: kubelet.service: main process exited, code=exited, status=1/FAILURE
Aug 05 10:35:12 m-k8s kubelet[14484]: E0805 10:35:12.354743   14484 server.go:294] "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docke
Aug 05 10:35:12 m-k8s systemd[1]: Unit kubelet.service entered failed state.
Aug 05 10:35:12 m-k8s systemd[1]: kubelet.service failed.
Aug 05 10:35:22 m-k8s systemd[1]: kubelet.service holdoff time over, scheduling restart.
Aug 05 10:35:22 m-k8s systemd[1]: Stopped kubelet: The Kubernetes Node Agent.
Aug 05 10:35:22 m-k8s systemd[1]: Started kubelet: The Kubernetes Node Agent.
Aug 05 10:35:22 m-k8s kubelet[14570]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:35:22 m-k8s kubelet[14570]: Flag --network-plugin has been deprecated, will be removed along with dockershim.
Aug 05 10:35:22 m-k8s systemd[1]: Started Kubernetes systemd probe.
Aug 05 10:35:22 m-k8s kubelet[14570]: I0805 10:35:22.693978   14570 server.go:440] "Kubelet version" kubeletVersion="v1.22.0"
Aug 05 10:35:22 m-k8s kubelet[14570]: I0805 10:35:22.695494   14570 server.go:868] "Client rotation is on, will bootstrap in background"
Aug 05 10:35:22 m-k8s kubelet[14570]: I0805 10:35:22.714962   14570 certificate_store.go:130] Loading cert/key pair from "/var/lib/kubelet/pki/kubelet-client-current.pem".
Aug 05 10:35:22 m-k8s kubelet[14570]: I0805 10:35:22.721420   14570 dynamic_cafile_content.go:155] "Starting controller" name="client-ca-bundle::/etc/kubernetes/pki/ca.crt"
Aug 05 10:35:22 m-k8s kubelet[14570]: I0805 10:35:22.962053   14570 server.go:687] "--cgroups-per-qos enabled, but --cgroup-root was not specified.  defaulting to /"
Aug 05 10:35:22 m-k8s kubelet[14570]: I0805 10:35:22.962688   14570 container_manager_linux.go:280] "Container manager verified user specified cgroup-root exists" cgroupRoot=[]
Aug 05 10:35:22 m-k8s kubelet[14570]: I0805 10:35:22.962853   14570 container_manager_linux.go:285] "Creating Container Manager object based on Node Config" nodeConfig={RuntimeCgroupsName: SystemCgroupsName: Kub
Aug 05 10:35:22 m-k8s kubelet[14570]: I0805 10:35:22.962894   14570 topology_manager.go:133] "Creating topology manager with policy per scope" topologyPolicyName="none" topologyScopeName="container"
Aug 05 10:35:22 m-k8s kubelet[14570]: I0805 10:35:22.962921   14570 container_manager_linux.go:320] "Creating device plugin manager" devicePluginEnabled=true
Aug 05 10:35:22 m-k8s kubelet[14570]: I0805 10:35:22.962996   14570 state_mem.go:36] "Initialized new in-memory state store"
Aug 05 10:35:22 m-k8s kubelet[14570]: I0805 10:35:22.963095   14570 kubelet.go:314] "Using dockershim is deprecated, please consider using a full-fledged CRI implementation"
Aug 05 10:35:22 m-k8s kubelet[14570]: I0805 10:35:22.963140   14570 client.go:78] "Connecting to docker on the dockerEndpoint" endpoint="unix:///var/run/docker.sock"
Aug 05 10:35:22 m-k8s kubelet[14570]: I0805 10:35:22.963164   14570 client.go:97] "Start docker client with request timeout" timeout="2m0s"
Aug 05 10:35:23 m-k8s kubelet[14570]: I0805 10:35:23.000916   14570 docker_service.go:566] "Hairpin mode is set but kubenet is not enabled, falling back to HairpinVeth" hairpinMode=promiscuous-bridge
Aug 05 10:35:23 m-k8s kubelet[14570]: I0805 10:35:23.001044   14570 docker_service.go:242] "Hairpin mode is set" hairpinMode=hairpin-veth
Aug 05 10:35:23 m-k8s kubelet[14570]: I0805 10:35:23.001241   14570 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:35:23 m-k8s kubelet[14570]: I0805 10:35:23.011557   14570 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:35:23 m-k8s kubelet[14570]: I0805 10:35:23.011786   14570 docker_service.go:257] "Docker cri networking managed by the network plugin" networkPluginName="cni"
Aug 05 10:35:23 m-k8s kubelet[14570]: I0805 10:35:23.019468   14570 cni.go:239] "Unable to update cni config" err="no networks found in /etc/cni/net.d"
Aug 05 10:35:23 m-k8s kubelet[14570]: I0805 10:35:23.045114   14570 docker_service.go:264] "Docker Info" dockerInfo=&{ID:RXTP:LTUA:M4Z5:JXNJ:K3ID:KR2N:RKLR:PUSN:JUVB:IV7H:52EF:7G25 Containers:0 ContainersRunning
Aug 05 10:35:23 m-k8s kubelet[14570]: E0805 10:35:23.045243   14570 server.go:294] "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docke
Aug 05 10:35:23 m-k8s systemd[1]: kubelet.service: main process exited, code=exited, status=1/FAILURE
Aug 05 10:35:23 m-k8s systemd[1]: Unit kubelet.service entered failed state.
Aug 05 10:35:23 m-k8s systemd[1]: kubelet.service failed.


```
