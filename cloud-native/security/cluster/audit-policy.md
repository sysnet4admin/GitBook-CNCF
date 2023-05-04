---
description: >-
  Audit policy defines rules about what events should be recorded and what data
  they should include. The audit policy object structure is defined in the
  audit.k8s.io API group. When an event is processe
---

# Audit Policy

## Main Reference&#x20;

{% embed url="https://zetawiki.com/wiki/%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4_audit_log_%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0" %}

{% embed url="https://kubernetes.io/docs/tasks/debug-application-cluster/audit/" %}



## Check logs after configuration.

```
[root@m-k8s ~]# cat /var/log/kubernetes/kubernetes.audit | more
{"kind":"Event","apiVersion":"audit.k8s.io/v1","level":"Metadata","auditID":"f9e91b43-77c3-4622-9d2a-13838f0f4984","stage":"ResponseComplete","requestURI":"/apis/coordination.k8s.io/v1/namespaces/kube-system/lea
ses/kube-controller-manager?timeout=5s","verb":"get","user":{"username":"system:kube-controller-manager","groups":["system:authenticated"]},"sourceIPs":["192.168.1.10"],"userAgent":"kube-controller-manager/v1.22
.0 (linux/amd64) kubernetes/c2b5237/leader-election","objectRef":{"resource":"leases","namespace":"kube-system","name":"kube-controller-manager","apiGroup":"coordination.k8s.io","apiVersion":"v1"},"responseStatu
s":{"metadata":{},"status":"Failure","reason":"Forbidden","code":403},"requestReceivedTimestamp":"2021-09-13T10:00:06.493243Z","stageTimestamp":"2021-09-13T10:00:06.581845Z","annotations":{"authorization.k8s.io/
decision":"forbid","authorization.k8s.io/reason":""}}
{"kind":"Event","apiVersion":"audit.k8s.io/v1","level":"Request","auditID":"41133c9d-ed47-48ba-8fdf-613a72adba18","stage":"ResponseComplete","requestURI":"/apis/storage.k8s.io/v1/storageclasses?limit=500\u0026re
sourceVersion=0","verb":"list","user":{"username":"system:apiserver","uid":"def46273-989c-4460-89c3-b84589f691c8","groups":["system:masters"]},"sourceIPs":["127.0.0.1"],"userAgent":"kube-apiserver/v1.22.0 (linux
/amd64) kubernetes/c2b5237","objectRef":{"resource":"storageclasses","apiGroup":"storage.k8s.io","apiVersion":"v1"},"responseStatus":{"metadata":{},"code":200},"requestReceivedTimestamp":"2021-09-13T10:00:06.602
391Z","stageTimestamp":"2021-09-13T10:00:06.608894Z","annotations":{"authorization.k8s.io/decision":"allow","authorization.k8s.io/reason":""}}
{"kind":"Event","apiVersion":"audit.k8s.io/v1","level":"Metadata","auditID":"473efcdb-dd39-4f41-bf17-79883bb689e0","stage":"ResponseComplete","requestURI":"/apis/apiextensions.k8s.io/v1/customresourcedefinitions
?limit=500\u0026resourceVersion=0","verb":"list","user":{"username":"system:apiserver","uid":"def46273-989c-4460-89c3-b84589f691c8","groups":["system:masters"]},"sourceIPs":["127.0.0.1"],"userAgent":"kube-apiser
ver/v1.22.0 (linux/amd64) kubernetes/c2b5237","objectRef":{"resource":"customresourcedefinitions","apiGroup":"apiextensions.k8s.io","apiVersion":"v1"},"responseStatus":{"metadata":{},"code":200},"requestReceived
Timestamp":"2021-09-13T10:00:06.609632Z","stageTimestamp":"2021-09-13T10:00:06.622899Z","annotations":{"authorization.k8s.io/decision":"allow","authorization.k8s.io/reason":""}}
{"kind":"Event","apiVersion":"audit.k8s.io/v1","level":"Metadata","auditID":"b03b2bec-3636-4480-9819-10e21eb68723","stage":"ResponseComplete","requestURI":"/apis/apiregistration.k8s.io/v1/apiservices?limit=500\u
0026resourceVersion=0","verb":"list","user":{"username":"system:apiserver","uid":"def46273-989c-4460-89c3-b84589f691c8","groups":["system:masters"]},"sourceIPs":["127.0.0.1"],"userAgent":"kube-apiserver/v1.22.0
(linux/amd64) kubernetes/c2b5237","objectRef":{"resource":"apiservices","apiGroup":"apiregistration.k8s.io","apiVersion":"v1"},"responseStatus":{"metadata":{},"code":200},"requestReceivedTimestamp":"2021-09-13T1
0:00:06.617566Z","stageTimestamp":"2021-09-13T10:00:06.623058Z","annotations":{"authorization.k8s.io/decision":"allow","authorization.k8s.io/reason":""}}
{"kind":"Event","apiVersion":"audit.k8s.io/v1","level":"Metadata","auditID":"7b712e38-424e-4e1a-8c38-ace61cf2da6b","stage":"ResponseComplete","requestURI":"/api/v1/namespaces/kube-system/configmaps?limit=500\u00
26resourceVersion=0","verb":"list","user":{"username":"system:apiserver","uid":"def46273-989c-4460-89c3-b84589f691c8","groups":["system:masters"]},"sourceIPs":["127.0.0.1"],"userAgent":"kube-apiserver/v1.22.0 (l
inux/amd64) kubernetes/c2b5237","objectRef":{"resource":"configmaps","namespace":"kube-system","apiVersion":"v1"},"responseStatus":{"metadata":{},"code":200},"requestReceivedTimestamp":"2021-09-13T10:00:06.61827
1Z","stageTimestamp":"2021-09-13T10:00:06.623160Z","annotations":{"authorization.k8s.io/decision":"allow","authorization.k8s.io/reason":""}}
{"kind":"Event","apiVersion":"audit.k8s.io/v1","level":"Request","auditID":"6c1aae46-9021-4957-8992-7a25390e8144","stage":"ResponseComplete","requestURI":"/api/v1/nodes?limit=500\u0026resourceVersion=0","verb":"
list","user":{"username":"system:kube-scheduler","groups":["system:authenticated"]},"sourceIPs":["192.168.1.10"],"userAgent":"kube-scheduler/v1.22.0 (linux/amd64) kubernetes/c2b5237/scheduler","objectRef":{"reso
urce":"nodes","apiVersion":"v1"},"responseStatus":{"metadata":{},"status":"Failure","reason":"Forbidden","code":403},"requestReceivedTimestamp":"2021-09-13T10:00:06.652170Z","stageTimestamp":"2021-09-13T10:00:06
.652818Z","annotations":{"authorization.k8s.io/decision":"forbid","authorization.k8s.io/reason":""}}
{"kind":"Event","apiVersion":"audit.k8s.io/v1","level":"Request","auditID":"56d7a258-2736-4162-8e09-e2f50acba85d","stage":"ResponseComplete","requestURI":"/api/v1/pods?fieldSelector=status.phase%21%3DSucceeded%2
Cstatus.phase%21%3DFailed\u0026limit=500\u0026resourceVersion=0","verb":"list","user":{"username":"system:kube-scheduler","groups":["system:authenticated"]},"sourceIPs":["192.168.1.10"],"userAgent":"kube-schedul
er/v1.22.0 (linux/amd64) kubernetes/c2b5237/scheduler","objectRef":{"resource":"pods","apiVersion":"v1"},"responseStatus":{"metadata":{},"status":"Failure","reason":"Forbidden","code":403},"requestReceivedTimest
amp":"2021-09-13T10:00:06.653308Z","stageTimestamp":"2021-09-13T10:00:06.654013Z","annotations":{"authorization.k8s.io/decision":"forbid","authorization.k8s.io/reason":""}}
{"kind":"Event","apiVersion":"audit.k8s.io/v1","level":"Metadata","auditID":"3e7440c0-b31f-4001-9fef-4af71498b18d","stage":"ResponseStarted","requestURI":"/api/v1/namespaces/kube-system/configmaps?allowWatchBook
marks=true\u0026fieldSelector=metadata.name%3Dkube-root-ca.crt\u0026resourceVersion=1454089\u0026timeout=6m30s\u0026timeoutSeconds=390\u0026watch=true","verb":"watch","user":{"username":"system:node:w1-k8s","gro
ups":["system:nodes","system:authenticated"]},"sourceIPs":["192.168.1.101"],"userAgent":"kubelet/v1.22.0 (linux/amd64) kubernetes/c2b5237","objectRef":{"resource":"configmaps","namespace":"kube-system","name":"k
ube-root-ca.crt","apiVersion":"v1"},"responseStatus":{"metadata":{},"status":"Failure","reason":"Forbidden","code":403},"requestReceivedTimestamp":"2021-09-13T10:00:06.655851Z","stageTimestamp":"2021-09-13T10:00
:06.656464Z","annotations":{"authorization.k8s.io/decision":"forbid","authorization.k8s.io/reason":"no relationship found between node 'w1-k8s' and this object"}}
{"kind":"Event","apiVersion":"audit.k8s.io/v1","level":"Metadata","auditID":"3e7440c0-b31f-4001-9fef-4af71498b18d","stage":"ResponseComplete","requestURI":"/api/v1/namespaces/kube-system/configmaps?allowWatchBoo
kmarks=true\u0026fieldSelector=metadata.name%3Dkube-root-ca.crt\u0026resourceVersion=1454089\u0026timeout=6m30s\u0026timeoutSeconds=390\u0026watch=true","verb":"watch","user":{"username":"system:node:w1-k8s","gr
oups":["system:nodes","system:authenticated"]},"sourceIPs":["192.168.1.101"],"userAgent":"kubelet/v1.22.0 (linux/amd64) kubernetes/c2b5237","objectRef":{"resource":"configmaps","namespace":"kube-system","name":"
kube-root-ca.crt","apiVersion":"v1"},"responseStatus":{"metadata":{},"status":"Failure","reason":"Forbidden","code":403},"requestReceivedTimestamp":"2021-09-13T10:00:06.655851Z","stageTimestamp":"2021-09-13T10:0
0:06.656714Z","annotations":{"authorization.k8s.io/decision":"forbid","authorization.k8s.io/reason":"no relationship found between node 'w1-k8s' and this object"}}
{"kind":"Event","apiVersion":"audit.k8s.io/v1","level":"RequestResponse","auditID":"da76ae52-52ef-47e7-b52f-848aebdcb5a5","stage":"ResponseComplete","requestURI":"/api/v1/namespaces/ingress-nginx/serviceaccounts
/ingress-nginx/token","verb":"create","user":{"username":"system:node:w1-k8s","groups":["system:nodes","system:authenticated"]},"sourceIPs":["192.168.1.101"],"userAgent":"kubelet/v1.22.0 (linux/amd64) kubernetes
/c2b5237","objectRef":{"resource":"serviceaccounts","namespace":"ingress-nginx","name":"ingress-nginx","apiVersion":"v1","subresource":"token"},"responseStatus":{"metadata":{},"status":"Failure","reason":"Forbid
den","code":403},"responseObject":{"kind":"Status","apiVersion":"v1","metadata":{},"status":"Failure","message":"serviceaccounts \"ingress-nginx\" is forbidden: User \"system:node:w1-k8s\" cannot create resource
 \"serviceaccounts/token\" in API group \"\" in the namespace \"ingress-nginx\": no relationship found between node 'w1-k8s' and this object","reason":"Forbidden","details":{"name":"ingress-nginx","kind":"servic
eaccounts"},"code":403},"requestReceivedTimestamp":"2021-09-13T10:00:06.658111Z","stageTimestamp":"2021-09-13T10:00:06.658893Z","annotations":{"authorization.k8s.io/decision":"forbid","authorization.k8s.io/reaso
n":"no relationship found between node 'w1-k8s' and this object"}}

<snipped>
```

## Check capacity&#x20;

it looks very big even it is testing environment)

```
[root@m-k8s ~]# ls -rlth /var/log/kubernetes/kubernetes.audit
-rw-------. 1 root root 4.2M Sep 13 19:03 /var/log/kubernetes/kubernetes.audit

<time goes by but nothing to do>

[root@m-k8s ~]# ls -rlth /var/log/kubernetes/kubernetes.audit
-rw-------. 1 root root 7.3M Sep 13 19:10 /var/log/kubernetes/kubernetes.audit

```
