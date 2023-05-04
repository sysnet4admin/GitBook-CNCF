---
description: Security risk analysis for Kubernetes resources
---

# kubesec

## install kubesec from tar.gz&#x20;

```
[root@m-k8s ~]# curl -OL https://github.com/controlplaneio/kubesec/releases/download/v2.11.2/kubesec_linux_amd64.tar.gz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   634  100   634    0     0   2164      0 --:--:-- --:--:-- --:--:--  2171
100 3970k  100 3970k    0     0  2115k      0  0:00:01  0:00:01 --:--:-- 2569k

[root@m-k8s ~]# tar xvfz kubesec_linux_amd64.tar.gz
CHANGELOG.md
LICENSE
README.md
kubesec

[root@m-k8s ~]# mv kubesec /usr/local/bin
```

## run kubesec scan&#x20;

```
[root@m-k8s ~]# kubesec scan components.yaml
[
  {
    "object": "ServiceAccount/metrics-server.kube-system",
    "valid": true,
    "fileName": "components.yaml",
    "message": "This resource kind is not supported by kubesec",
    "score": 0,
    "scoring": {}
  },
  {
    "object": "ClusterRole/system:aggregated-metrics-reader.default",
    "valid": true,
    "fileName": "components.yaml",
    "message": "This resource kind is not supported by kubesec",
    "score": 0,
    "scoring": {}
  },
  {
    "object": "ClusterRole/system:metrics-server.default",
    "valid": true,
    "fileName": "components.yaml",
    "message": "This resource kind is not supported by kubesec",
    "score": 0,
    "scoring": {}
  },
  {
    "object": "RoleBinding/metrics-server-auth-reader.kube-system",
    "valid": true,
    "fileName": "components.yaml",
    "message": "This resource kind is not supported by kubesec",
    "score": 0,
    "scoring": {}
  },
  {
    "object": "ClusterRoleBinding/metrics-server:system:auth-delegator.default",
    "valid": true,
    "fileName": "components.yaml",
    "message": "This resource kind is not supported by kubesec",
    "score": 0,
    "scoring": {}
  },
  {
    "object": "ClusterRoleBinding/system:metrics-server.default",
    "valid": true,
    "fileName": "components.yaml",
    "message": "This resource kind is not supported by kubesec",
    "score": 0,
    "scoring": {}
  },
  {
    "object": "Service/metrics-server.kube-system",
    "valid": true,
    "fileName": "components.yaml",
    "message": "This resource kind is not supported by kubesec",
    "score": 0,
    "scoring": {}
  },
  {
    "object": "Deployment/metrics-server.kube-system",
    "valid": true,
    "fileName": "components.yaml",
    "message": "Passed with a score of 7 points",
    "score": 7,
    "scoring": {
      "passed": [
        {
          "id": "ServiceAccountName",
          "selector": ".spec .serviceAccountName",
          "reason": "Service accounts restrict Kubernetes API access and should be configured with least privilege",
          "points": 3
        },
        {
          "id": "RequestsCPU",
          "selector": "containers[] .resources .requests .cpu",
          "reason": "Enforcing CPU requests aids a fair balancing of resources across the cluster",
          "points": 1
        },
        {
          "id": "RequestsMemory",
          "selector": "containers[] .resources .requests .memory",
          "reason": "Enforcing memory requests aids a fair balancing of resources across the cluster",
          "points": 1
        },
        {
          "id": "ReadOnlyRootFilesystem",
          "selector": "containers[] .securityContext .readOnlyRootFilesystem == true",
          "reason": "An immutable root filesystem can prevent malicious binaries being added to PATH and increase attack cost",
          "points": 1
        },
        {
          "id": "RunAsNonRoot",
          "selector": "containers[] .securityContext .runAsNonRoot == true",
          "reason": "Force the running image to run as a non-root user to ensure least privilege",
          "points": 1
        }
      ],
      "advise": [
        {
          "id": "ApparmorAny",
          "selector": ".metadata .annotations .\"container.apparmor.security.beta.kubernetes.io/nginx\"",
          "reason": "Well defined AppArmor policies may provide greater protection from unknown threats. WARNING: NOT PRODUCTION READY",
          "points": 3
        },
        {
          "id": "SeccompAny",
          "selector": ".metadata .annotations .\"container.seccomp.security.alpha.kubernetes.io/pod\"",
          "reason": "Seccomp profiles set minimum privilege and secure against unknown threats",
          "points": 1
        },
        {
          "id": "LimitsCPU",
          "selector": "containers[] .resources .limits .cpu",
          "reason": "Enforcing CPU limits prevents DOS via resource exhaustion",
          "points": 1
        },
        {
          "id": "LimitsMemory",
          "selector": "containers[] .resources .limits .memory",
          "reason": "Enforcing memory limits prevents DOS via resource exhaustion",
          "points": 1
        },
        {
          "id": "CapDropAny",
          "selector": "containers[] .securityContext .capabilities .drop",
          "reason": "Reducing kernel capabilities available to a container limits its attack surface",
          "points": 1
        },
        {
          "id": "CapDropAll",
          "selector": "containers[] .securityContext .capabilities .drop | index(\"ALL\")",
          "reason": "Drop all capabilities and add only those required to reduce syscall attack surface",
          "points": 1
        },
        {
          "id": "RunAsUser",
          "selector": "containers[] .securityContext .runAsUser -gt 10000",
          "reason": "Run as a high-UID user to avoid conflicts with the host's user table",
          "points": 1
        }
      ]
    }
  },
  {
    "object": "APIService/v1beta1.metrics.k8s.io.default",
    "valid": true,
    "fileName": "components.yaml",
    "message": "This resource kind is not supported by kubesec",
    "score": 0,
    "scoring": {}
  }
]

```



```
[root@m-k8s ~]# cat <<EOF > kubesec-test.yaml
> apiVersion: v1
> kind: Pod
> metadata:
>   name: kubesec-demo
> spec:
>   containers:
>   - name: kubesec-demo
>     image: gcr.io/google-samples/node-hello:1.0
>     securityContext:
>       readOnlyRootFilesystem: true
> EOF

[root@m-k8s ~]# kubesec scan kubesec-test.yaml
[
  {
    "object": "Pod/kubesec-demo.default",
    "valid": true,
    "fileName": "kubesec-test.yaml",
    "message": "Passed with a score of 1 points",
    "score": 1,
    "scoring": {
      "passed": [
        {
          "id": "ReadOnlyRootFilesystem",
          "selector": "containers[] .securityContext .readOnlyRootFilesystem == true",
          "reason": "An immutable root filesystem can prevent malicious binaries being added to PATH and increase attack cost",
          "points": 1
        }
      ],
      "advise": [
        {
          "id": "ApparmorAny",
          "selector": ".metadata .annotations .\"container.apparmor.security.beta.kubernetes.io/nginx\"",
          "reason": "Well defined AppArmor policies may provide greater protection from unknown threats. WARNING: NOT PRODUCTION READY",
          "points": 3
        },
        {
          "id": "ServiceAccountName",
          "selector": ".spec .serviceAccountName",
          "reason": "Service accounts restrict Kubernetes API access and should be configured with least privilege",
          "points": 3
        },
        {
          "id": "SeccompAny",
          "selector": ".metadata .annotations .\"container.seccomp.security.alpha.kubernetes.io/pod\"",
          "reason": "Seccomp profiles set minimum privilege and secure against unknown threats",
          "points": 1
        },
        {
          "id": "LimitsCPU",
          "selector": "containers[] .resources .limits .cpu",
          "reason": "Enforcing CPU limits prevents DOS via resource exhaustion",
          "points": 1
        },
        {
          "id": "LimitsMemory",
          "selector": "containers[] .resources .limits .memory",
          "reason": "Enforcing memory limits prevents DOS via resource exhaustion",
          "points": 1
        },
        {
          "id": "RequestsCPU",
          "selector": "containers[] .resources .requests .cpu",
          "reason": "Enforcing CPU requests aids a fair balancing of resources across the cluster",
          "points": 1
        },
        {
          "id": "RequestsMemory",
          "selector": "containers[] .resources .requests .memory",
          "reason": "Enforcing memory requests aids a fair balancing of resources across the cluster",
          "points": 1
        },
        {
          "id": "CapDropAny",
          "selector": "containers[] .securityContext .capabilities .drop",
          "reason": "Reducing kernel capabilities available to a container limits its attack surface",
          "points": 1
        },
        {
          "id": "CapDropAll",
          "selector": "containers[] .securityContext .capabilities .drop | index(\"ALL\")",
          "reason": "Drop all capabilities and add only those required to reduce syscall attack surface",
          "points": 1
        },
        {
          "id": "RunAsNonRoot",
          "selector": "containers[] .securityContext .runAsNonRoot == true",
          "reason": "Force the running image to run as a non-root user to ensure least privilege",
          "points": 1
        },
        {
          "id": "RunAsUser",
          "selector": "containers[] .securityContext .runAsUser -gt 10000",
          "reason": "Run as a high-UID user to avoid conflicts with the host's user table",
          "points": 1
        }
      ]
    }
  }
]

```



{% embed url="https://github.com/controlplaneio/kubesec#download-kubesec" %}



