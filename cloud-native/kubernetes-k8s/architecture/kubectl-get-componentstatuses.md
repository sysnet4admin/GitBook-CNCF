# kubectl get componentstatuses

Run each of managed kubernetes&#x20;

## EKS Componentstatuses

```bash
❯ k get componentstatuses
Warning: v1 ComponentStatus is deprecated in v1.19+
NAME                 STATUS    MESSAGE             ERROR
scheduler            Healthy   ok                  
controller-manager   Healthy   ok                  
etcd-0               Healthy   {"health":"true"}  
```

## AKS Componentstatuses

```bash
❯ k get componentstatuses
Warning: v1 ComponentStatus is deprecated in v1.19+
NAME                 STATUS      MESSAGE                                                                                       ERROR
controller-manager   Unhealthy   Get "http://127.0.0.1:10252/healthz": dial tcp 127.0.0.1:10252: connect: connection refused   
scheduler            Unhealthy   Get "http://127.0.0.1:10251/healthz": dial tcp 127.0.0.1:10251: connect: connection refused   
etcd-0               Healthy     {"health":"true"} 
```

## GKE Componentstatuses

```bash
hj@cloudshell:~ (hj-int-20200908)$ (gke_hj-int-20200908_us-central1-c_gke) k get componentstatuses
Warning: v1 ComponentStatus is deprecated in v1.19+
NAME                 STATUS    MESSAGE             ERROR
controller-manager   Healthy   ok
scheduler            Healthy   ok
etcd-0               Healthy   {"health":"true"}
etcd-1               Healthy   {"health":"true"}
```

## NKS Componentstatuses

```bash
❯ k get componentstatuses
Warning: v1 ComponentStatus is deprecated in v1.19+
NAME                 STATUS      MESSAGE                                                                                       ERROR
controller-manager   Unhealthy   Get "http://127.0.0.1:10252/healthz": dial tcp 127.0.0.1:10252: connect: connection refused   
scheduler            Healthy     ok                                                                                            
etcd-2               Healthy     {"health":"true"}                                                                             
etcd-1               Healthy     {"health":"true"}                                                                             
etcd-0               Healthy     {"health":"true"}  
```



