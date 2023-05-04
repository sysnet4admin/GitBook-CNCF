# GKE PVC Resize

## TL; DR: No impact when PVC re-claim &#x20;

### 1.Deploy pvc to standard class&#x20;

```bash
$ k get pv,pvc
No resources found
```

### 2.Check storageclass (which is predefined)

```bash
k get storageclasses.storage.k8s.io
NAME                 PROVISIONER             RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
premium-rwo          pd.csi.storage.gke.io   Delete          WaitForFirstConsumer   true                   13d
standard (default)   kubernetes.io/gce-pd    Delete          Immediate              true                   13d
standard-rwo         pd.csi.storage.gke.io   Delete          WaitForFirstConsumer   true                   13d

```

### 3.Check dynamic-provisioner to storageclass&#x20;

```bash
$ cat pvc-dynamic.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-dynamic
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 20Gi
  storageClassName: standard
```

### 4. apply it!!!

```bash
ka pvc-dynamic.yaml                                                          
persistentvolumeclaim/pvc-dynamic configured
```

### 5. Check pv,pvc&#x20;

```bash
$ k get pv,pvc
NAME                                                        CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                 STORAGECLASS   REASON   AGE
persistentvolume/pvc-03943091-ba68-4491-aa53-f07481a68d32   20Gi       RWO            Delete           Bound    default/pvc-dynamic   standard                43s

NAME                                STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
persistentvolumeclaim/pvc-dynamic   Bound    pvc-03943091-ba68-4491-aa53-f07481a68d32   10Gi       RWO            standard       45s

```

{% hint style="info" %}
PVC's Capacity is lazy to update. so wait for seconds&#x20;
{% endhint %}

### 6.Check deployment to use this pvc

```bash
$ cat deploy-pvc.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deploy-pvc
spec:
  replicas: 1
  selector:
    matchLabels:
      app: deploy-pvc
  template:
    metadata:
      labels:
        app: deploy-pvc
    spec:
      containers:
      - name: chk-log
        image: sysnet4admin/chk-log
        volumeMounts:
        - name: pvc-vol
          mountPath: /audit
      volumes:
      - name: pvc-vol
        persistentVolumeClaim:
          claimName: pvc-dynamic
```

### 7. Create bundle data in the deployment(pod)&#x20;

```bash
k run net --image=sysnet4admin/net-tools-ifn                                 
pod/net created

k get po -o wide
NAME                          READY   STATUS    RESTARTS   AGE    IP          NODE                                        NOMINATED NODE   READINESS GATES
deploy-pvc-5f958886d7-vm8sl   1/1     Running   0          2m3s   10.80.2.3   gke-node-label-dev1-pool-f1253061-c30d      <none>           <none>
net                           1/1     Running   0          16s    10.80.1.4   gke-node-label-default-pool-b9b36c6f-c9jd   <none>           <none>

k exec net -it -- /bin/bash
[root@net /]# curl 10.80.2.3
pod_n: deploy-pvc-5f958886d7-vm8sl | ip_dest: 10.80.2.3
[root@net /]# exit
exit
```

### 8.Check bundle data in the container&#x20;

```bash
k exec deploy-pvc-5f958886d7-vm8sl -it -- /bin/bash                          
root@deploy-pvc-5f958886d7-vm8sl:/# ls /audit
audit_deploy-pvc-5f958886d7-vm8sl.log  lost+found
root@deploy-pvc-5f958886d7-vm8sl:/# exit
```

### 9. Change storage capacity from 20Gi to 30Gi  and apply it&#x20;

```bash
$ cat pvc-dynamic.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-dynamic
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 30Gi
  storageClassName: standard
  
$ ka pvc-dynamic.yaml
persistentvolumeclaim/pvc-dynamic configured
```

### 10. Check again pv,pvc

```bash
k get pv,pvc
NAME                                                        CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                 STORAGECLASS   REASON   AGE
persistentvolume/pvc-03943091-ba68-4491-aa53-f07481a68d32   30Gi       RWO            Delete           Bound    default/pvc-dynamic   standard                10m

NAME                                STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
persistentvolumeclaim/pvc-dynamic   Bound    pvc-03943091-ba68-4491-aa53-f07481a68d32   20Gi       RWO            standard       10m

```

### 11. re-deploy deployment by delete&#x20;

```bash
k delete po deploy-pvc-5f958886d7-vm8sl
pod "deploy-pvc-5f958886d7-vm8sl" deleted
```

### &#x20;12. Check data is sustained&#x20;

```bash
k exec deploy-pvc-5f958886d7-jl7k5 -it -- /bin/bash
root@deploy-pvc-5f958886d7-jl7k5:/# ls /audit
audit_deploy-pvc-5f958886d7-vm8sl.log  lost+found
root@deploy-pvc-5f958886d7-jl7k5:/#
```

### DONE!!!! Successfully&#x20;

**Reference:**&#x20;

{% embed url="https://kubernetes.io/blog/2018/07/12/resizing-persistent-volumes-using-kubernetes/" %}

