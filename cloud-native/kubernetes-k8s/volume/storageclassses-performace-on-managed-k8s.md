---
description: Performance Test (updated May 03 2021)
---

# Storageclassses Performace on Managed k8s

## 0.TL;DR

|                PROVIDER               |      INSTANCE     |        NAME       | read\_iops | wrtie\_iops | read\_bw | wrtie\_bw |
| :-----------------------------------: | :---------------: | :---------------: | :--------: | :---------: | :------: | :-------: |
|         <p>GKE</p><p>(GCP)</p>        |     e2-medium     |    premium-rwo    |     692    |     375     |    685   |    367    |
|                                       |                   |    standard-rwo   |     83     |      72     |    84    |     71    |
|                                       |                   |      standard     |     81     |      80     |    82    |     77    |
|         <p>EKS</p><p>(AWS)</p>        |     t3.medium     |        gp2        |     378    |     394     |    390   |    555    |
|        <p>AKS</p><p>(Azure)</p>       | Standard\_DS2\_v2 |     azurefile     |     206    |     216     |    196   |    181    |
|                                       |                   | azurefile-premium |     204    |      94     |    202   |     95    |
|                                       |                   |      default      |    1295    |     307     |   1188   |    257    |
|                                       |                   |  managed-premium  |     830    |     641     |    718   |    636    |
|   <p>Native-k8s</p><p>(MacBook)</p>   |       Native      |     nfs-client    |     207    |     206     |    207   |    206    |
| <p>Unknown-k8s</p><p>(Bare-Metal)</p> |      Unknown      |     Local-NVMe    |    8064    |     8853    |   7149   |    7646   |

{% hint style="info" %}
* Deploy as Default(only eks node change from 2 to 3)
* Master 1ea + Worker 3ea (except bare-metal)
{% endhint %}

## 1.GKE on GCP

### Storageclassses List&#x20;

```bash
hj@cloudshell:~/kubestr (hj-int-20200908)$ k get storageclasses.storage.k8s.io
NAME                 PROVISIONER             RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
premium-rwo          pd.csi.storage.gke.io   Delete          WaitForFirstConsumer   true                   23d
standard (default)   kubernetes.io/gce-pd    Delete          Immediate              true                   23d
standard-rwo         pd.csi.storage.gke.io   Delete          WaitForFirstConsumer   true                   23d
```

### FIO thru premium-rwo&#x20;

```bash
hj@cloudshell:~/kubestr (hj-int-20200908)$ kubestr fio -s premium-rwo
PVC created kubestr-fio-pvc-g9txn
Pod created kubestr-fio-pod-8sw84
Running FIO test (default-fio) on StorageClass (premium-rwo) with a PVC of Size (100Gi)
Elapsed time- 1m24.510291222s
FIO test results:

FIO version - fio-3.20
Global options - ioengine=libaio verify=0 direct=1 gtod_reduce=1

JobName: read_iops
  blocksize=4K filesize=2G iodepth=64 rw=randread
read:
  IOPS=692.495239 BW(KiB/s)=2786
  iops: min=328 max=1904 avg=697.500000
  bw(KiB/s): min=1312 max=7616 avg=2790.100098

JobName: write_iops
  blocksize=4K filesize=2G iodepth=64 rw=randwrite
write:
  IOPS=375.618439 BW(KiB/s)=1519
  iops: min=256 max=570 avg=378.333344
  bw(KiB/s): min=1024 max=2280 avg=1513.500000

JobName: read_bw
  blocksize=128K filesize=2G iodepth=64 rw=randread
read:
  IOPS=685.625793 BW(KiB/s)=88291
  iops: min=353 max=1866 avg=692.833313
  bw(KiB/s): min=45221 max=238848 avg=88696.031250

JobName: write_bw
  blocksize=128k filesize=2G iodepth=64 rw=randwrite
write:
  IOPS=367.465118 BW(KiB/s)=47566
  iops: min=272 max=464 avg=371.899994
  bw(KiB/s): min=34816 max=59392 avg=47612.800781

Disk stats (read/write):
  sdb: ios=28787/13090 merge=0/272 ticks=2178446/2138357 in_queue=4294880, util=99.430069%
  -  OK
hj@cloudshell:~/kubestr (hj-int-20200908)$
```

### FIO thru standard-rwo

```bash
hj@cloudshell:~/kubestr (hj-int-20200908)$ kubestr fio -s standard-rwo
PVC created kubestr-fio-pvc-5hntf
Pod created kubestr-fio-pod-rhmqd
Running FIO test (default-fio) on StorageClass (standard-rwo) with a PVC of Size (100Gi)
Elapsed time- 2m12.821241035s
FIO test results:

FIO version - fio-3.20
Global options - ioengine=libaio verify=0 direct=1 gtod_reduce=1

JobName: read_iops
  blocksize=4K filesize=2G iodepth=64 rw=randread
read:
  IOPS=83.536697 BW(KiB/s)=349
  iops: min=11 max=164 avg=85.677422
  bw(KiB/s): min=47 max=656 avg=343.806458

JobName: write_iops
  blocksize=4K filesize=2G iodepth=64 rw=randwrite
write:
  IOPS=72.239296 BW(KiB/s)=304
  iops: min=1 max=128 avg=88.076920
  bw(KiB/s): min=7 max=512 avg=353.653839

JobName: read_bw
  blocksize=128K filesize=2G iodepth=64 rw=randread
read:
  IOPS=84.910332 BW(KiB/s)=11367
  iops: min=31 max=165 avg=85.312500
  bw(KiB/s): min=4063 max=21205 avg=10963.750000

JobName: write_bw
  blocksize=128k filesize=2G iodepth=64 rw=randwrite
write:
  IOPS=71.885185 BW(KiB/s)=9706
  iops: min=10 max=128 avg=81.571426
  bw(KiB/s): min=1280 max=16384 avg=10478.357422

Disk stats (read/write):
  sdb: ios=4191/3138 merge=0/112 ticks=2271063/2045801 in_queue=4313429, util=99.420975%
  -  OK
hj@cloudshell:~/kubestr (hj-int-20200908)$
```

### FIO thru standard&#x20;

```bash
hj@cloudshell:~/kubestr (hj-int-20200908)$ kubestr fio -s standard
PVC created kubestr-fio-pvc-trnc2
Pod created kubestr-fio-pod-q79g5
Running FIO test (default-fio) on StorageClass (standard) with a PVC of Size (100Gi)
Elapsed time- 58.248687657s
FIO test results:

FIO version - fio-3.20
Global options - ioengine=libaio verify=0 direct=1 gtod_reduce=1

JobName: read_iops
  blocksize=4K filesize=2G iodepth=64 rw=randread
read:
  IOPS=81.962128 BW(KiB/s)=343
  iops: min=17 max=132 avg=84.677422
  bw(KiB/s): min=71 max=528 avg=339.677429

JobName: write_iops
  blocksize=4K filesize=2G iodepth=64 rw=randwrite
write:
  IOPS=80.070168 BW(KiB/s)=336
  iops: min=2 max=128 avg=97.959999
  bw(KiB/s): min=8 max=512 avg=392.679993

JobName: read_bw
  blocksize=128K filesize=2G iodepth=64 rw=randread
read:
  IOPS=82.278091 BW(KiB/s)=11029
  iops: min=46 max=132 avg=85.451614
  bw(KiB/s): min=5888 max=16896 avg=10971.613281

JobName: write_bw
  blocksize=128k filesize=2G iodepth=64 rw=randwrite
write:
  IOPS=77.182274 BW(KiB/s)=10392
  iops: min=1 max=128 avg=92.807693
  bw(KiB/s): min=255 max=16384 avg=11939.115234

Disk stats (read/write):
  sdb: ios=3196/2989 merge=0/110 ticks=2261590/2133004 in_queue=4391493, util=99.056862%
  -  OK
hj@cloudshell:~/kubestr (hj-int-20200908)$
```

## 2.EKS on AWS

### Storageclasss(es) List&#x20;

```bash
[cloudshell-user@ip-10-0-191-210 ~]$ k get storageclasses
NAME            PROVISIONER             RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
gp2 (default)   kubernetes.io/aws-ebs   Delete          WaitForFirstConsumer   false                  23d
```

### FIO thru gp2&#x20;

```bash
[cloudshell-user@ip-10-0-191-210 ~]$ kubestr fio -s gp2
PVC created kubestr-fio-pvc-xkzrw
Pod created kubestr-fio-pod-7qvhp
Running FIO test (default-fio) on StorageClass (gp2) with a PVC of Size (100Gi)
Elapsed time- 54.508805719s
FIO test results:
  
FIO version - fio-3.20
Global options - ioengine=libaio verify=0 direct=1 gtod_reduce=1

JobName: read_iops
  blocksize=4K filesize=2G iodepth=64 rw=randread
read:
  IOPS=378.102875 BW(KiB/s)=1529
  iops: min=98 max=714 avg=391.034485
  bw(KiB/s): min=392 max=2856 avg=1564.206909

JobName: write_iops
  blocksize=4K filesize=2G iodepth=64 rw=randwrite
write:
  IOPS=394.698486 BW(KiB/s)=1595
  iops: min=152 max=820 avg=409.620697
  bw(KiB/s): min=608 max=3280 avg=1638.655151

JobName: read_bw
  blocksize=128K filesize=2G iodepth=64 rw=randread
read:
  IOPS=390.869324 BW(KiB/s)=50565
  iops: min=126 max=614 avg=406.310333
  bw(KiB/s): min=16128 max=78592 avg=52011.550781

JobName: write_bw
  blocksize=128k filesize=2G iodepth=64 rw=randwrite
write:
  IOPS=555.474426 BW(KiB/s)=71636
  iops: min=331 max=1054 avg=557.666687
  bw(KiB/s): min=42411 max=134912 avg=71383.632812

Disk stats (read/write):
  nvme1n1: ios=13983/17066 merge=0/317 ticks=339584/463920 in_queue=789216, util=99.599434%
  -  OK
[cloudshell-user@ip-10-0-191-210 ~]$ 
```

### FIO thru azurefile&#x20;





## 3.AKS on AZURE

### Storageclassses List&#x20;

```bash
jo@Azure:~$ k get storageclasses.storage.k8s.io
NAME                PROVISIONER                RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
azurefile           kubernetes.io/azure-file   Delete          Immediate              true                   9m11s
azurefile-premium   kubernetes.io/azure-file   Delete          Immediate              true                   9m11s
default (default)   kubernetes.io/azure-disk   Delete          WaitForFirstConsumer   true                   9m11s
managed-premium     kubernetes.io/azure-disk   Delete          WaitForFirstConsumer   true                   9m11s
```

### FIO thru azurefile&#x20;

```bash
jo@Azure:~$ ./kubestr fio -s azurefile
PVC created kubestr-fio-pvc-frlfh
Pod created kubestr-fio-pod-hb6hm
Running FIO test (default-fio) on StorageClass (azurefile) with a PVC of Size (100Gi)
Elapsed time- 1m17.307862037s
FIO test results:

FIO version - fio-3.20
Global options - ioengine=libaio verify=0 direct=1 gtod_reduce=1

JobName: read_iops
  blocksize=4K filesize=2G iodepth=64 rw=randread
read:
  IOPS=206.786713 BW(KiB/s)=843
  iops: min=134 max=348 avg=262.458344
  bw(KiB/s): min=536 max=1392 avg=1049.875000

JobName: write_iops
  blocksize=4K filesize=2G iodepth=64 rw=randwrite
write:
  IOPS=216.112442 BW(KiB/s)=880
  iops: min=10 max=392 avg=272.666656
  bw(KiB/s): min=40 max=1568 avg=1090.958374

JobName: read_bw
  blocksize=128K filesize=2G iodepth=64 rw=randread
read:
  IOPS=196.055527 BW(KiB/s)=25618
  iops: min=106 max=326 avg=249.375000
  bw(KiB/s): min=13568 max=41728 avg=31926.708984

JobName: write_bw
  blocksize=128k filesize=2G iodepth=64 rw=randwrite
write:
  IOPS=181.752960 BW(KiB/s)=23790
  iops: min=114 max=324 avg=241.347824
  bw(KiB/s): min=14592 max=41472 avg=30898.869141

Disk stats (read/write):
  -  OK
```



### FIO thru azurefile-premium &#x20;

```bash
jo@Azure:~$ ./kubestr fio -s azurefile-premium
PVC created kubestr-fio-pvc-s88n8
Pod created kubestr-fio-pod-k5xhg
Running FIO test (default-fio) on StorageClass (azurefile-premium) with a PVC of Size (100Gi)
Elapsed time- 49.697466788s
FIO test results:

FIO version - fio-3.20
Global options - ioengine=libaio verify=0 direct=1 gtod_reduce=1

JobName: read_iops
  blocksize=4K filesize=2G iodepth=64 rw=randread
read:
  IOPS=204.523422 BW(KiB/s)=834
  iops: min=135 max=256 avg=207.266663
  bw(KiB/s): min=542 max=1024 avg=829.566650

JobName: write_iops
  blocksize=4K filesize=2G iodepth=64 rw=randwrite
write:
  IOPS=94.690720 BW(KiB/s)=393
  iops: min=49 max=146 avg=107.129028
  bw(KiB/s): min=196 max=584 avg=429.064514

JobName: read_bw
  blocksize=128K filesize=2G iodepth=64 rw=randread
read:
  IOPS=202.328308 BW(KiB/s)=26422
  iops: min=128 max=256 avg=204.466660
  bw(KiB/s): min=16384 max=32768 avg=26186.166016

JobName: write_bw
  blocksize=128k filesize=2G iodepth=64 rw=randwrite
write:
  IOPS=95.577408 BW(KiB/s)=12708
  iops: min=60 max=134 avg=104.548386
  bw(KiB/s): min=7680 max=17152 avg=13398.483398

Disk stats (read/write):
  -  OK
```



### FIO thru default &#x20;

```bash
```



### FIO thru managed-premium &#x20;

```bash
```



## 4.Native k8s on Local VM

### Sources

{% embed url="https://github.com/sysnet4admin/_Lecture_k8s.starterkit" %}

{% embed url="https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner" %}



### Storageclasss(es) List&#x20;

```bash
[root@m-k8s ~]# k get storageclasses.storage.k8s.io 
NAME         PROVISIONER                                     RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
nfs-client   cluster.local/nfs-subdir-external-provisioner   Delete          Immediate           true                   7s
```

### FIO thru nfs-client on MAC Book&#x20;

```bash
root@m-k8s kubestr]# kubestr fio -s nfs-client
PVC created kubestr-fio-pvc-wp9bm
Pod created kubestr-fio-pod-74rrp
Running FIO test (default-fio) on StorageClass (nfs-client) with a PVC of Size (100Gi)
Elapsed time- 1m21.911294684s
FIO test results:
  
FIO version - fio-3.20
Global options - ioengine=libaio verify=0 direct=1 gtod_reduce=1

JobName: read_iops
  blocksize=4K filesize=2G iodepth=64 rw=randread
read:
  IOPS=207.079086 BW(KiB/s)=844
  iops: min=47 max=271 avg=209.225800
  bw(KiB/s): min=190 max=1085 avg=837.258057

JobName: write_iops
  blocksize=4K filesize=2G iodepth=64 rw=randwrite
write:
  IOPS=206.293930 BW(KiB/s)=841
  iops: min=5 max=270 avg=212.699997
  bw(KiB/s): min=23 max=1080 avg=851.566650

JobName: read_bw
  blocksize=128K filesize=2G iodepth=64 rw=randread
read:
  IOPS=207.283630 BW(KiB/s)=27046
  iops: min=67 max=256 avg=208.935486
  bw(KiB/s): min=8634 max=32827 avg=26787.033203

JobName: write_bw
  blocksize=128k filesize=2G iodepth=64 rw=randwrite
write:
  IOPS=206.222961 BW(KiB/s)=26909
  iops: min=51 max=254 avg=208.677414
  bw(KiB/s): min=6551 max=32512 avg=26732.515625

Disk stats (read/write):
  -  OK
[root@m-k8s kubestr]# 
```

## 5.Unknow k8s on On-Prem&#x20;

### FIO thru local bare-metal (Local NVMe 4ea / Total 800G)

```bash
[spkr@erdia22 ~ (scluster:default)]$ kubestr fio -s high
PVC created kubestr-fio-pvc-lbzdq
Pod created kubestr-fio-pod-72jqs
Running FIO test (default-fio) on StorageClass (high) with a PVC of Size (100Gi)
Elapsed time- 25.4277098s
FIO test results:
FIO version - fio-3.20
Global options - ioengine=libaio verify=0 direct=1 gtod_reduce=1

JobName: read_iops
  blocksize=4K filesize=2G iodepth=64 rw=randread
read:
  IOPS=8064.111816 BW(KiB/s)=32273
  iops: min=5824 max=12104 avg=8010.931152
  bw(KiB/s): min=23296 max=48416 avg=32044.310547

JobName: write_iops
  blocksize=4K filesize=2G iodepth=64 rw=randwrite
write:
  IOPS=8853.468750 BW(KiB/s)=35430
  iops: min=6480 max=13036 avg=8812.930664
  bw(KiB/s): min=25920 max=52144 avg=35252.449219

JobName: read_bw
  blocksize=128K filesize=2G iodepth=64 rw=randread
read:
  IOPS=7149.720215 BW(KiB/s)=915701
  iops: min=5365 max=9722 avg=7165.344727
  bw(KiB/s): min=686754 max=1244416 avg=917192.062500

JobName: write_bw
  blocksize=128k filesize=2G iodepth=64 rw=randwrite
write:
  IOPS=7646.137207 BW(KiB/s)=979243
  iops: min=4700 max=9476 avg=7609.344727
  bw(KiB/s): min=601600 max=1212928 avg=974033.750000
  Disk stats (read/write):

nvme10n1: ios=257463/276086 merge=0/2247 ticks=1792211/1852018 in_queue=3660131, util=99.734482%
- OK
```



## 11.Reference&#x20;

### Perf Source

{% embed url="https://github.com/kastenhq/kubestr/releases/tag/v0.4.16" %}

### Script file &#x20;

#### But do not use for this looping test due to too long....consuming time.&#x20;

```bash
; fio-rand-RW.job for fiotest

[global]
name=fio-rand-RW
filename=fio-rand-RW
rw=randrw
rwmixread=60
rwmixwrite=40
bs=4K
direct=0
numjobs=4
time_based
runtime=900

[file1]
size=10G
ioengine=libaio
iodepth=16
```
