---
description: Cloud Native Runtime Security
---

# falco (under contruction)

## run falco&#x20;

### install kernel-devel which is same as current kernel.&#x20;

```
[root@w1-k8s ~]# yum -y install kernel-devel-$(uname -r)
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.navercorp.com
 * epel: nrt.edge.kernel.org
 * extras: mirror.navercorp.com
 * updates: mirror.navercorp.com
Resolving Dependencies
--> Running transaction check
---> Package kernel-devel.x86_64 0:3.10.0-1160.36.2.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===================================================================================================================================================================================================================
 Package                                            Arch                                         Version                                                       Repository                                     Size
===================================================================================================================================================================================================================
Installing:
 kernel-devel                                       x86_64                                       3.10.0-1160.36.2.el7                                          updates                                        18 M

Transaction Summary
===================================================================================================================================================================================================================
Install  1 Package

Total download size: 18 M
Installed size: 38 M
Downloading packages:
Delta RPMs disabled because /usr/bin/applydeltarpm not installed.
kernel-devel-3.10.0-1160.36.2.el7.x86_64.rpm                                                                                                                                                |  18 MB  00:00:01
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : kernel-devel-3.10.0-1160.36.2.el7.x86_64                                                                                                                                                        1/1
  Verifying  : kernel-devel-3.10.0-1160.36.2.el7.x86_64                                                                                                                                                        1/1

Installed:
  kernel-devel.x86_64 0:3.10.0-1160.36.2.el7

Complete!
[root@w1-k8s ~]# yum -y install falco
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.navercorp.com
 * epel: nrt.edge.kernel.org
 * extras: mirror.navercorp.com
 * updates: mirror.navercorp.com
Resolving Dependencies
--> Running transaction check
---> Package falco.x86_64 0:0.29.1-1 will be installed
--> Processing Dependency: dkms for package: falco-0.29.1-1.x86_64
--> Running transaction check
---> Package dkms.noarch 0:2.8.4-1.el7 will be installed
--> Processing Dependency: elfutils-libelf-devel for package: dkms-2.8.4-1.el7.noarch
--> Running transaction check
---> Package elfutils-libelf-devel.x86_64 0:0.176-5.el7 will be installed
--> Processing Dependency: elfutils-libelf(x86-64) = 0.176-5.el7 for package: elfutils-libelf-devel-0.176-5.el7.x86_64
--> Processing Dependency: pkgconfig(zlib) for package: elfutils-libelf-devel-0.176-5.el7.x86_64
--> Running transaction check
---> Package elfutils-libelf.x86_64 0:0.176-4.el7 will be updated
--> Processing Dependency: elfutils-libelf(x86-64) = 0.176-4.el7 for package: elfutils-libs-0.176-4.el7.x86_64
---> Package elfutils-libelf.x86_64 0:0.176-5.el7 will be an update
---> Package zlib-devel.x86_64 0:1.2.7-19.el7_9 will be installed
--> Processing Dependency: zlib = 1.2.7-19.el7_9 for package: zlib-devel-1.2.7-19.el7_9.x86_64
--> Running transaction check
---> Package elfutils-libs.x86_64 0:0.176-4.el7 will be updated
---> Package elfutils-libs.x86_64 0:0.176-5.el7 will be an update
---> Package zlib.x86_64 0:1.2.7-18.el7 will be updated
---> Package zlib.x86_64 0:1.2.7-19.el7_9 will be an update
--> Finished Dependency Resolution

Dependencies Resolved

===================================================================================================================================================================================================================
 Package                                                  Arch                                      Version                                             Repository                                            Size
===================================================================================================================================================================================================================
Installing:
 falco                                                    x86_64                                    0.29.1-1                                            falcosecurity-rpm                                    4.6 M
Installing for dependencies:
 dkms                                                     noarch                                    2.8.4-1.el7                                         epel                                                  78 k
 elfutils-libelf-devel                                    x86_64                                    0.176-5.el7                                         base                                                  40 k
 zlib-devel                                               x86_64                                    1.2.7-19.el7_9                                      updates                                               50 k
Updating for dependencies:
 elfutils-libelf                                          x86_64                                    0.176-5.el7                                         base                                                 195 k
 elfutils-libs                                            x86_64                                    0.176-5.el7                                         base                                                 291 k
 zlib                                                     x86_64                                    1.2.7-19.el7_9                                      updates                                               90 k

Transaction Summary
===================================================================================================================================================================================================================
Install  1 Package  (+3 Dependent packages)
Upgrade             ( 3 Dependent packages)

Total download size: 5.3 M
Downloading packages:
Delta RPMs disabled because /usr/bin/applydeltarpm not installed.
(1/7): elfutils-libelf-devel-0.176-5.el7.x86_64.rpm                                                                                                                                         |  40 kB  00:00:00
(2/7): elfutils-libelf-0.176-5.el7.x86_64.rpm                                                                                                                                               | 195 kB  00:00:00
(3/7): elfutils-libs-0.176-5.el7.x86_64.rpm                                                                                                                                                 | 291 kB  00:00:00
(4/7): zlib-devel-1.2.7-19.el7_9.x86_64.rpm                                                                                                                                                 |  50 kB  00:00:00
warning: /var/cache/yum/x86_64/7/epel/packages/dkms-2.8.4-1.el7.noarch.rpm: Header V4 RSA/SHA256 Signature, key ID 352c64e5: NOKEY
Public key for dkms-2.8.4-1.el7.noarch.rpm is not installed
(5/7): dkms-2.8.4-1.el7.noarch.rpm                                                                                                                                                          |  78 kB  00:00:01
(6/7): zlib-1.2.7-19.el7_9.x86_64.rpm                                                                                                                                                       |  90 kB  00:00:00
(7/7): falco-0.29.1-x86_64.rpm                                                                                                                                                              | 4.6 MB  00:00:01
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                                                              2.5 MB/s | 5.3 MB  00:00:02
Retrieving key from file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
Importing GPG key 0x352C64E5:
 Userid     : "Fedora EPEL (7) <epel@fedoraproject.org>"
 Fingerprint: 91e9 7d7c 4a5e 96f1 7f3e 888f 6a2f aea2 352c 64e5
 Package    : epel-release-7-11.noarch (@extras)
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Updating   : zlib-1.2.7-19.el7_9.x86_64                                                                                                                                                                     1/10
  Updating   : elfutils-libelf-0.176-5.el7.x86_64                                                                                                                                                             2/10
  Installing : zlib-devel-1.2.7-19.el7_9.x86_64                                                                                                                                                               3/10
  Installing : elfutils-libelf-devel-0.176-5.el7.x86_64                                                                                                                                                       4/10
  Installing : dkms-2.8.4-1.el7.noarch                                                                                                                                                                        5/10
  Installing : falco-0.29.1-1.x86_64                                                                                                                                                                          6/10

Creating symlink /var/lib/dkms/falco/17f5df52a7d9ed6bb12d3b1768460def8439936d/source ->
                 /usr/src/falco-17f5df52a7d9ed6bb12d3b1768460def8439936d

DKMS: add completed.

Kernel preparation unnecessary for this kernel.  Skipping...

Building module:
cleaning build area...
make -j1 KERNELRELEASE=3.10.0-1160.36.2.el7.x86_64 -C /lib/modules/3.10.0-1160.36.2.el7.x86_64/build M=/var/lib/dkms/falco/17f5df52a7d9ed6bb12d3b1768460def8439936d/build.........
cleaning build area...

DKMS: build completed.

falco.ko.xz:
Running module version sanity check.
 - Original module
   - No original module exists within this kernel
 - Installation
   - Installing to /lib/modules/3.10.0-1160.36.2.el7.x86_64/extra/
Adding any weak-modules

depmod.....

DKMS: install completed.
  Updating   : elfutils-libs-0.176-5.el7.x86_64                                                                                                                                                               7/10
  Cleanup    : elfutils-libs-0.176-4.el7.x86_64                                                                                                                                                               8/10
  Cleanup    : elfutils-libelf-0.176-4.el7.x86_64                                                                                                                                                             9/10
  Cleanup    : zlib-1.2.7-18.el7.x86_64                                                                                                                                                                      10/10
  Verifying  : dkms-2.8.4-1.el7.noarch                                                                                                                                                                        1/10
  Verifying  : zlib-1.2.7-19.el7_9.x86_64                                                                                                                                                                     2/10
  Verifying  : zlib-devel-1.2.7-19.el7_9.x86_64                                                                                                                                                               3/10
  Verifying  : falco-0.29.1-1.x86_64                                                                                                                                                                          4/10
  Verifying  : elfutils-libelf-0.176-5.el7.x86_64                                                                                                                                                             5/10
  Verifying  : elfutils-libs-0.176-5.el7.x86_64                                                                                                                                                               6/10
  Verifying  : elfutils-libelf-devel-0.176-5.el7.x86_64                                                                                                                                                       7/10
  Verifying  : zlib-1.2.7-18.el7.x86_64                                                                                                                                                                       8/10
  Verifying  : elfutils-libelf-0.176-4.el7.x86_64                                                                                                                                                             9/10
  Verifying  : elfutils-libs-0.176-4.el7.x86_64                                                                                                                                                              10/10

Installed:
  falco.x86_64 0:0.29.1-1

Dependency Installed:
  dkms.noarch 0:2.8.4-1.el7                                    elfutils-libelf-devel.x86_64 0:0.176-5.el7                                    zlib-devel.x86_64 0:1.2.7-19.el7_9

Dependency Updated:
  elfutils-libelf.x86_64 0:0.176-5.el7                                     elfutils-libs.x86_64 0:0.176-5.el7                                     zlib.x86_64 0:1.2.7-19.el7_9

Complete!
```



### Deploy by helm&#x20;



### logs on daemonset&#x20;

```
[root@m-k8s ~]# k logs  falco-wbq9k
* Setting up /usr/src links from host
* Running falco-driver-loader for: falco version=0.29.1, driver version=17f5df52a7d9ed6bb12d3b1768460def8439936d
* Running falco-driver-loader with: driver=module, compile=yes, download=yes
* Unloading falco module, if present
* Trying to load a system falco module, if present
* Looking for a falco module locally (kernel 3.10.0-1160.36.2.el7.x86_64)
* Trying to download a prebuilt falco module from https://download.falco.org/driver/17f5df52a7d9ed6bb12d3b1768460def8439936d/falco_centos_3.10.0-1160.36.2.el7.x86_64_1.ko
* Download succeeded
* Success: falco module found and inserted
Fri Aug 27 23:39:22 2021: Falco version 0.29.1 (driver version 17f5df52a7d9ed6bb12d3b1768460def8439936d)
Fri Aug 27 23:39:22 2021: Falco initialized with configuration file /etc/falco/falco.yaml
Fri Aug 27 23:39:22 2021: Loading rules from file /etc/falco/falco_rules.yaml:
Fri Aug 27 23:39:22 2021: Loading rules from file /etc/falco/falco_rules.local.yaml:
Fri Aug 27 23:39:23 2021: Starting internal webserver, listening on port 8765
23:39:23.463002000: Notice Privileged container started (user=root user_loginuid=0 command=container:4fa90bbac999 k8s.ns=kube-system k8s.pod=calico-node-htwxj container=4fa90bbac999 image=k8s.gcr.io/pause:3.5) k8s.ns=kube-system k8s.pod=calico-node-htwxj container=4fa90bbac999
23:39:23.474550000: Notice Privileged container started (user=root user_loginuid=0 command=container:023256e9390a k8s.ns=kube-system k8s.pod=kube-proxy-txf86 container=023256e9390a image=k8s.gcr.io/pause:3.5) k8s.ns=kube-system k8s.pod=kube-proxy-txf86 container=023256e9390a
23:39:23.598757000: Notice Privileged container started (user=<NA> user_loginuid=0 command=container:09f2b3f2c3eb k8s.ns=default k8s.pod=falco-wbq9k container=09f2b3f2c3eb image=k8s.gcr.io/pause:3.5) k8s.ns=default k8s.pod=falco-wbq9k container=09f2b3f2c3eb
23:52:06.857477317: Error File below a monitored directory opened for writing (user=root user_loginuid=0 command=cp --reflink=auto /var/tmp/dracut.jvjLeS/initramfs.img /boot/initramfs-3.10.0-1127.19.1.el7.x86_64.tmp file=/boot/initramfs-3.10.0-1127.19.1.el7.x86_64.tmp parent=dracut pcmdline=dracut /sbin/dracut -f /boot/initramfs-3.10.0-1127.19.1.el7.x86_64.tmp 3.10.0-1127.19.1.el7.x86_64 gparent=weak-modules container_id=host image=<NA>) k8s.ns=<NA> k8s.pod=<NA> container=host k8s.ns=<NA> k8s.pod=<NA> container=host
23:53:26.979870470: Error File below a monitored directory opened for writing (user=root user_loginuid=0 command=cp --reflink=auto /var/tmp/dracut.iI7dvn/initramfs.img /boot/initramfs-3.10.0-1160.36.2.el7.x86_64.tmp file=/boot/initramfs-3.10.0-1160.36.2.el7.x86_64.tmp parent=dracut pcmdline=dracut /sbin/dracut -f /boot/initramfs-3.10.0-1160.36.2.el7.x86_64.tmp 3.10.0-1160.36.2.el7.x86_64 gparent=weak-modules container_id=host image=<NA>) k8s.ns=<NA> k8s.pod=<NA> container=host k8s.ns=<NA> k8s.pod=<NA> container=host
23:59:48.682346016: Notice Packet socket was created in a container (user=root user_loginuid=-1 command=speaker --port=7472 --config=metallb socket_info=domain=17(AF_PACKET) type=3 proto=0  container_id=e65ffbf9e87e container_name=k8s_speaker_metallb-speaker-wjjdp_metallb-system_814c2cf2-ffab-49c0-81a6-9ceb570a4ea8_7 image=quay.io/metallb/speaker:v0.10.2) k8s.ns=metallb-system k8s.pod=metallb-speaker-wjjdp container=e65ffbf9e87e k8s.ns=metallb-system k8s.pod=metallb-speaker-wjjdp container=e65ffbf9e87e
00:01:28.772124537: Notice Packet socket was created in a container (user=root user_loginuid=-1 command=speaker --port=7472 --config=metallb socket_info=domain=17(AF_PACKET) type=3 proto=0  container_id=e65ffbf9e87e container_name=k8s_speaker_metallb-speaker-wjjdp_metallb-system_814c2cf2-ffab-49c0-81a6-9ceb570a4ea8_7 image=quay.io/metallb/speaker:v0.10.2) k8s.ns=metallb-system k8s.pod=metallb-speaker-wjjdp container=e65ffbf9e87e k8s.ns=metallb-system k8s.pod=metallb-speaker-wjjdp container=e65ffbf9e87e
00:01:28.772995203: Notice Packet socket was created in a container (user=root user_loginuid=-1 command=speaker --port=7472 --config=metallb socket_info=domain=17(AF_PACKET) type=3 proto=0  container_id=e65ffbf9e87e container_name=k8s_speaker_metallb-speaker-wjjdp_metallb-system_814c2cf2-ffab-49c0-81a6-9ceb570a4ea8_7 image=quay.io/metallb/speaker:v0.10.2) k8s.ns=metallb-system k8s.pod=metallb-speaker-wjjdp container=e65ffbf9e87e k8s.ns=metallb-system k8s.pod=metallb-speaker-wjjdp container=e65ffbf9e87e
00:01:28.773654409: Notice Packet socket was created in a container (user=root user_loginuid=-1 command=speaker --port=7472 --config=metallb socket_info=domain=17(AF_PACKET) type=3 proto=0  container_id=e65ffbf9e87e container_name=k8s_speaker_metallb-speaker-wjjdp_metallb-system_814c2cf2-ffab-49c0-81a6-9ceb570a4ea8_7 image=quay.io/metallb/speaker:v0.10.2) k8s.ns=metallb-system k8s.pod=metallb-speaker-wjjdp container=e65ffbf9e87e k8s.ns=metallb-system k8s.pod=metallb-speaker-wjjdp container=e65ffbf9e87e
00:10:11.487220214: Error File below /etc opened for writing (user=root user_loginuid=-1 command=start_runit /sbin/start_runit parent=start_runit pcmdline=start_runit /sbin/start_runit file=/etc/envvars program=start_runit gparent=<NA> ggparent=<NA> gggparent=<NA> container_id=954874082b49 image=<NA>) k8s.ns=<NA> k8s.pod=<NA> container=954874082b49 k8s.ns=<NA> k8s.pod=<NA> container=954874082b49
00:10:18.184204191: Error File below /etc opened for writing (user=root user_loginuid=-1 command=cp --coreutils-prog-shebang=cp /usr/bin/cp -a /etc/service/available/felix /etc/service/enabled/ parent=start_runit pcmdline=start_runit /sbin/start_runit file=/etc/service/enabled/felix/run program=cp gparent=start_runit ggparent=<NA> gggparent=<NA> container_id=954874082b49 image=calico/node) k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49 k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49
00:10:18.198643983: Error File below /etc opened for writing (user=root user_loginuid=-1 command=cp --coreutils-prog-shebang=cp /usr/bin/cp -a /etc/service/available/felix /etc/service/enabled/ parent=start_runit pcmdline=start_runit /sbin/start_runit file=/etc/service/enabled/felix/log/run program=cp gparent=start_runit ggparent=<NA> gggparent=<NA> container_id=954874082b49 image=calico/node) k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49 k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49
00:10:18.235780168: Error File below /etc opened for writing (user=root user_loginuid=-1 command=cp --coreutils-prog-shebang=cp /usr/bin/cp -a /etc/service/available/monitor-addresses /etc/service/enabled/ parent=start_runit pcmdline=start_runit /sbin/start_runit file=/etc/service/enabled/monitor-addresses/run program=cp gparent=start_runit ggparent=<NA> gggparent=<NA> container_id=954874082b49 image=calico/node) k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49 k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49
00:10:18.237964494: Error File below /etc opened for writing (user=root user_loginuid=-1 command=cp --coreutils-prog-shebang=cp /usr/bin/cp -a /etc/service/available/monitor-addresses /etc/service/enabled/ parent=start_runit pcmdline=start_runit /sbin/start_runit file=/etc/service/enabled/monitor-addresses/log/run program=cp gparent=start_runit ggparent=<NA> gggparent=<NA> container_id=954874082b49 image=calico/node) k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49 k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49
00:10:18.323819764: Error File below /etc opened for writing (user=root user_loginuid=-1 command=cp --coreutils-prog-shebang=cp /usr/bin/cp -a /etc/service/available/allocate-tunnel-addrs /etc/service/enabled/ parent=start_runit pcmdline=start_runit /sbin/start_runit file=/etc/service/enabled/allocate-tunnel-addrs/run program=cp gparent=start_runit ggparent=<NA> gggparent=<NA> container_id=954874082b49 image=calico/node) k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49 k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49
00:10:18.325727146: Error File below /etc opened for writing (user=root user_loginuid=-1 command=cp --coreutils-prog-shebang=cp /usr/bin/cp -a /etc/service/available/allocate-tunnel-addrs /etc/service/enabled/ parent=start_runit pcmdline=start_runit /sbin/start_runit file=/etc/service/enabled/allocate-tunnel-addrs/log/run program=cp gparent=start_runit ggparent=<NA> gggparent=<NA> container_id=954874082b49 image=calico/node) k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49 k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49
00:10:18.481361275: Error File below /etc opened for writing (user=root user_loginuid=-1 command=cp --coreutils-prog-shebang=cp /usr/bin/cp -a /etc/service/available/bird /etc/service/enabled/ parent=start_runit pcmdline=start_runit /sbin/start_runit file=/etc/service/enabled/bird/run program=cp gparent=start_runit ggparent=<NA> gggparent=<NA> container_id=954874082b49 image=calico/node) k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49 k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49
00:10:18.585492034: Error File below /etc opened for writing (user=root user_loginuid=-1 command=cp --coreutils-prog-shebang=cp /usr/bin/cp -a /etc/service/available/bird /etc/service/enabled/ parent=start_runit pcmdline=start_runit /sbin/start_runit file=/etc/service/enabled/bird/log/run program=cp gparent=start_runit ggparent=<NA> gggparent=<NA> container_id=954874082b49 image=calico/node) k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49 k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49
00:10:18.670484493: Error File below /etc opened for writing (user=root user_loginuid=-1 command=cp --coreutils-prog-shebang=cp /usr/bin/cp -a /etc/service/available/bird6 /etc/service/enabled/ parent=start_runit pcmdline=start_runit /sbin/start_runit file=/etc/service/enabled/bird6/run program=cp gparent=start_runit ggparent=<NA> gggparent=<NA> container_id=954874082b49 image=calico/node) k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49 k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49
00:10:18.693771716: Error File below /etc opened for writing (user=root user_loginuid=-1 command=cp --coreutils-prog-shebang=cp /usr/bin/cp -a /etc/service/available/bird6 /etc/service/enabled/ parent=start_runit pcmdline=start_runit /sbin/start_runit file=/etc/service/enabled/bird6/log/run program=cp gparent=start_runit ggparent=<NA> gggparent=<NA> container_id=954874082b49 image=calico/node) k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49 k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49
00:10:18.759589445: Error File below /etc opened for writing (user=root user_loginuid=-1 command=cp --coreutils-prog-shebang=cp /usr/bin/cp -a /etc/service/available/confd /etc/service/enabled/ parent=start_runit pcmdline=start_runit /sbin/start_runit file=/etc/service/enabled/confd/run program=cp gparent=start_runit ggparent=<NA> gggparent=<NA> container_id=954874082b49 image=calico/node) k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49 k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49
00:10:18.765935932: Error File below /etc opened for writing (user=root user_loginuid=-1 command=cp --coreutils-prog-shebang=cp /usr/bin/cp -a /etc/service/available/confd /etc/service/enabled/ parent=start_runit pcmdline=start_runit /sbin/start_runit file=/etc/service/enabled/confd/log/run program=cp gparent=start_runit ggparent=<NA> gggparent=<NA> container_id=954874082b49 image=calico/node) k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49 k8s.ns=kube-system k8s.pod=calico-node-htwxj container=954874082b49
06:04:53.857482631: Notice A shell was spawned in a container with an attached terminal (user=root user_loginuid=-1 k8s.ns=default k8s.pod=deploy-w-pvc-84cf7f666c-8jn9z container=bb0d588bb97b shell=bash parent=runc cmdline=bash terminal=34816 container_id=bb0d588bb97b image=sysnet4admin/chk-log) k8s.ns=default k8s.pod=deploy-w-pvc-84cf7f666c-8jn9z container=bb0d588bb97b
06:05:10.104894869: Warning Shell history had been deleted or renamed (user=root user_loginuid=-1 type=openat command=bash fd.name=/root/.bash_history name=/root/.bash_history path=<NA> oldpath=<NA> k8s.ns=default k8s.pod=deploy-w-pvc-84cf7f666c-8jn9z container=bb0d588bb97b) k8s.ns=default k8s.pod=deploy-w-pvc-84cf7f666c-8jn9z container=bb0d588bb97b

```



### custom rule&#x20;

{% code title="/etc/falco/falco_rules.local.yaml" %}
```
- list: systemd_procs
  items: [systemd, '"(systemd)"']

- rule: Any Open Activity by Systemd
  desc: Detects all open events by systemd.
  condition: evt.type=open and proc.name in (systemd_procs)
  output: "File opened by systemd (user=%user.name command=%proc.cmdline file=%fd.name)"
  priority: WARNING
```
{% endcode %}

```
[root@w1-k8s ~]# falco
Sat Aug 28 18:55:14 2021: Falco version 0.29.1 (driver version 17f5df52a7d9ed6bb12d3b1768460def8439936d)
Sat Aug 28 18:55:14 2021: Falco initialized with configuration file /etc/falco/falco.yaml
Sat Aug 28 18:55:14 2021: Loading rules from file /etc/falco/falco_rules.yaml:
Sat Aug 28 18:55:15 2021: Loading rules from file /etc/falco/falco_rules.local.yaml:
Sat Aug 28 18:55:15 2021: Loading rules from file /etc/falco/k8s_audit_rules.yaml:
Sat Aug 28 18:55:17 2021: Starting internal webserver, listening on port 8765
18:55:16.931435000: Notice Privileged container started (user=root user_loginuid=0 command=container:4fa90bbac999 k8s_POD_calico-node-htwxj_kube-system_0928961b-6afd-48b7-abe6-f89231a11edc_4 (id=4fa90bbac999) image=k8s.gcr.io/pause:3.5)
18:55:16.946395000: Notice Privileged container started (user=root user_loginuid=0 command=container:023256e9390a k8s_POD_kube-proxy-txf86_kube-system_5f39afb9-27a2-4d2e-b0ff-829176f5d8f6_4 (id=023256e9390a) image=k8s.gcr.io/pause:3.5)
18:55:17.090611000: Notice Privileged container started (user=root user_loginuid=0 command=container:09f2b3f2c3eb k8s_POD_falco-wbq9k_default_5a545e02-37f4-4ca0-838e-3d6ffe0ef46b_1 (id=09f2b3f2c3eb) image=k8s.gcr.io/pause:3.5)


18:55:21.968572527: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:21.970375233: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/proc/self/mountinfo)
18:55:21.976484179: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:21.976493140: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/mount/utab)
18:55:21.978080600: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:21.978110708: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/sys/devices/virtual/block/dm-0/uevent)
18:55:21.978188115: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:21.978197705: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/udev/data/b253:0)
18:55:21.978331418: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:21.978343502: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/sys/devices/pci0000:00/0000:00:0d.0/ata3/host2/target2:0:0/2:0:0:0/block/sda/sda1/uevent)
18:55:21.978392596: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:21.978397484: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/udev/data/b8:1)
18:55:21.982728339: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:21.982744082: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/proc/self/mountinfo)
18:55:21.984461001: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:21.984470413: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/mount/utab)
18:55:21.984687961: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:21.984700565: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/sys/devices/virtual/block/dm-0/uevent)
18:55:21.984826936: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:21.984838248: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/udev/data/b253:0)
18:55:21.985004757: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:21.985020582: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/sys/devices/pci0000:00/0000:00:0d.0/ata3/host2/target2:0:0/2:0:0:0/block/sda/sda1/uevent)
18:55:21.985089077: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:21.985096903: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/udev/data/b8:1)
18:55:21.999085938: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:21.999959564: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/proc/471/cgroup)
18:55:24.938200936: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.938213808: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/proc/self/mountinfo)
18:55:24.939619508: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.939625783: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/mount/utab)
18:55:24.939787816: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.939798370: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/sys/devices/virtual/block/dm-0/uevent)
18:55:24.939850448: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.939856867: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/udev/data/b253:0)
18:55:24.939972653: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.939984560: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/sys/devices/pci0000:00/0000:00:0d.0/ata3/host2/target2:0:0/2:0:0:0/block/sda/sda1/uevent)
18:55:24.940032823: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.940213469: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/udev/data/b8:1)
18:55:24.940818941: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.940831152: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/sys/devices/virtual/block/dm-0/uevent)
18:55:24.940878878: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.940883989: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/udev/data/b253:0)
18:55:24.941012317: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.941122137: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/etc/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e-runc.l2wgXc.mount)
18:55:24.941134237: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.941140051: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e-runc.l2wgXc.mount)
18:55:24.941147652: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.941151693: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e-runc.l2wgXc.mount)
18:55:24.941159034: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.941524741: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/local/lib/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e-runc.l2wgXc.mount)
18:55:24.941534137: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.941545525: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/lib/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e-runc.l2wgXc.mount)
18:55:24.941553164: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.941558429: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator.late/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e-runc.l2wgXc.mount)
18:55:24.941904536: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.941907743: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/etc/systemd/system/run.mount)
18:55:24.941911778: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.941914465: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/system/run.mount)
18:55:24.941918153: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.941920748: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator/run.mount)
18:55:24.941924168: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.941927263: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/local/lib/systemd/system/run.mount)
18:55:24.941930831: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.941932879: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/lib/systemd/system/run.mount)
18:55:24.941936228: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.941938332: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator.late/run.mount)
18:55:24.941947057: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.941949385: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/etc/systemd/system/run-docker.mount)
18:55:24.941953066: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.941955610: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/system/run-docker.mount)
18:55:24.941959152: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.941961864: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator/run-docker.mount)
18:55:24.941965662: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.941967441: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/local/lib/systemd/system/run-docker.mount)
18:55:24.941971107: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.941973238: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/lib/systemd/system/run-docker.mount)
18:55:24.941976935: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.941979862: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator.late/run-docker.mount)
18:55:24.941985937: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.941989229: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/etc/systemd/system/run-docker-runtime\x2drunc.mount)
18:55:24.944035479: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.944048557: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/system/run-docker-runtime\x2drunc.mount)
18:55:24.944059448: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.944067919: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator/run-docker-runtime\x2drunc.mount)
18:55:24.944074213: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.944079597: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/local/lib/systemd/system/run-docker-runtime\x2drunc.mount)
18:55:24.944086053: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.944091442: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/lib/systemd/system/run-docker-runtime\x2drunc.mount)
18:55:24.944097373: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.944102719: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator.late/run-docker-runtime\x2drunc.mount)
18:55:24.944117444: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.944122298: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/etc/systemd/system/run-docker-runtime\x2drunc-moby.mount)
18:55:24.944127998: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.944132509: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/system/run-docker-runtime\x2drunc-moby.mount)
18:55:24.944138386: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.944141911: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator/run-docker-runtime\x2drunc-moby.mount)
18:55:24.944148637: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.944151126: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/local/lib/systemd/system/run-docker-runtime\x2drunc-moby.mount)
18:55:24.944156751: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.944159938: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/lib/systemd/system/run-docker-runtime\x2drunc-moby.mount)
18:55:24.944166054: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.944169793: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator.late/run-docker-runtime\x2drunc-moby.mount)
18:55:24.944185187: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.944189929: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/etc/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e.mount)
18:55:24.944197365: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.944201027: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e.mount)
18:55:24.944207660: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.944209984: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e.mount)
18:55:24.944216648: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.944218347: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/local/lib/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e.mount)
18:55:24.944224783: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.944226939: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/lib/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e.mount)
18:55:24.944233746: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.944236284: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator.late/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e.mount)
18:55:24.945187994: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.945200692: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/proc/self/mountinfo)
18:55:24.948455870: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.948462480: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/mount/utab)
18:55:24.948624897: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.948636223: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/sys/devices/virtual/block/dm-0/uevent)
18:55:24.948689110: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.948696578: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/udev/data/b253:0)
18:55:24.948810540: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.948911153: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/sys/devices/pci0000:00/0000:00:0d.0/ata3/host2/target2:0:0/2:0:0:0/block/sda/sda1/uevent)
18:55:24.948970864: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:24.948976258: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/udev/data/b8:1)
18:55:31.899677626: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.899686206: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/proc/self/mountinfo)
18:55:31.900987512: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.900991842: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/mount/utab)
18:55:31.901145483: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.901156605: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/sys/devices/virtual/block/dm-0/uevent)
18:55:31.901208664: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.901215907: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/udev/data/b253:0)
18:55:31.901329514: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.901340898: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/sys/devices/pci0000:00/0000:00:0d.0/ata3/host2/target2:0:0/2:0:0:0/block/sda/sda1/uevent)
18:55:31.901389133: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.901394251: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/udev/data/b8:1)
18:55:31.902138807: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.902149502: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/sys/devices/virtual/block/dm-0/uevent)
18:55:31.902203750: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.902209398: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/udev/data/b253:0)
18:55:31.902340085: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.902357420: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/etc/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e-runc.Tf5bCt.mount)
18:55:31.902366902: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.902372033: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e-runc.Tf5bCt.mount)
18:55:31.902379567: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.902383709: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e-runc.Tf5bCt.mount)
18:55:31.902391059: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.902396055: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/local/lib/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e-runc.Tf5bCt.mount)
18:55:31.902403298: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.902412130: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/lib/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e-runc.Tf5bCt.mount)
18:55:31.902419458: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.902423346: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator.late/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e-runc.Tf5bCt.mount)
18:55:31.902734637: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.902737220: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/etc/systemd/system/run.mount)
18:55:31.902741181: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.902744141: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/system/run.mount)
18:55:31.902747579: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.902750576: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator/run.mount)
18:55:31.902754051: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.902755578: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/local/lib/systemd/system/run.mount)
18:55:31.902758927: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.902760971: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/lib/systemd/system/run.mount)
18:55:31.902764280: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.902766816: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator.late/run.mount)
18:55:31.902774718: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.902777433: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/etc/systemd/system/run-docker.mount)
18:55:31.902781386: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.907753919: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/system/run-docker.mount)
18:55:31.907778892: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.907804183: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator/run-docker.mount)
18:55:31.907810954: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.907816698: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/local/lib/systemd/system/run-docker.mount)
18:55:31.907821368: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.907825774: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/lib/systemd/system/run-docker.mount)
18:55:31.907829752: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.907833627: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator.late/run-docker.mount)
18:55:31.907846346: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.907851183: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/etc/systemd/system/run-docker-runtime\x2drunc.mount)
18:55:31.907855636: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.907860442: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/system/run-docker-runtime\x2drunc.mount)
18:55:31.907864743: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.907867791: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator/run-docker-runtime\x2drunc.mount)
18:55:31.907872061: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.907873675: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/local/lib/systemd/system/run-docker-runtime\x2drunc.mount)
18:55:31.907877860: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.907880093: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/lib/systemd/system/run-docker-runtime\x2drunc.mount)
18:55:31.907884344: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.907886813: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator.late/run-docker-runtime\x2drunc.mount)
18:55:31.907893457: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.907895324: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/etc/systemd/system/run-docker-runtime\x2drunc-moby.mount)
18:55:31.907899610: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.907901892: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/system/run-docker-runtime\x2drunc-moby.mount)
18:55:31.907906141: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.907908767: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator/run-docker-runtime\x2drunc-moby.mount)
18:55:31.907914203: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.907919318: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/local/lib/systemd/system/run-docker-runtime\x2drunc-moby.mount)
18:55:31.907926901: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.907933310: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/lib/systemd/system/run-docker-runtime\x2drunc-moby.mount)
18:55:31.907939721: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.907943540: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator.late/run-docker-runtime\x2drunc-moby.mount)
18:55:31.907953150: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.907956568: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/etc/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e.mount)
18:55:31.907963522: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.907966911: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e.mount)
18:55:31.907973705: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.907976309: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e.mount)
18:55:31.907982964: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.907984889: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/local/lib/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e.mount)
18:55:31.907991431: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.907993994: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/lib/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e.mount)
18:55:31.908000466: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.908003110: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator.late/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e.mount)
18:55:31.910484124: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.910498195: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/proc/self/mountinfo)
18:55:31.914861556: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.914865902: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/mount/utab)
18:55:31.915024186: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.915035612: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/sys/devices/virtual/block/dm-0/uevent)
18:55:31.915087837: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.915095323: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/udev/data/b253:0)
18:55:31.915212100: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.915224062: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/sys/devices/pci0000:00/0000:00:0d.0/ata3/host2/target2:0:0/2:0:0:0/block/sda/sda1/uevent)
18:55:31.915271932: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.915277563: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/udev/data/b8:1)
18:55:31.921915421: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:31.921931164: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/proc/645/cgroup)
18:55:35.008819611: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.008827559: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/proc/self/mountinfo)
18:55:35.010087630: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.010092173: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/mount/utab)
18:55:35.010243070: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.010253916: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/sys/devices/virtual/block/dm-0/uevent)
18:55:35.010305475: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.010312089: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/udev/data/b253:0)
18:55:35.010577773: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.010590738: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/sys/devices/pci0000:00/0000:00:0d.0/ata3/host2/target2:0:0/2:0:0:0/block/sda/sda1/uevent)
18:55:35.010645471: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.010651803: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/udev/data/b8:1)
18:55:35.011229334: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.011239607: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/sys/devices/virtual/block/dm-0/uevent)
18:55:35.011286684: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.011291764: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/udev/data/b253:0)
18:55:35.011508685: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.011526031: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/etc/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e-runc.13Sj2z.mount)
18:55:35.011535010: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.011540333: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e-runc.13Sj2z.mount)
18:55:35.011547539: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.011551662: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e-runc.13Sj2z.mount)
18:55:35.011559135: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.011563844: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/local/lib/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e-runc.13Sj2z.mount)
18:55:35.011571128: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.011579088: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/lib/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e-runc.13Sj2z.mount)
18:55:35.011586140: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.011589701: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator.late/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e-runc.13Sj2z.mount)
18:55:35.011908377: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.011911544: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/etc/systemd/system/run.mount)
18:55:35.011915556: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.011918416: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/system/run.mount)
18:55:35.011921916: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.011924530: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator/run.mount)
18:55:35.011928060: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.011929899: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/local/lib/systemd/system/run.mount)
18:55:35.011933258: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.011935942: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/lib/systemd/system/run.mount)
18:55:35.011939271: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.011941675: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator.late/run.mount)
18:55:35.011949689: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.011951488: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/etc/systemd/system/run-docker.mount)
18:55:35.011955015: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.011957216: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/system/run-docker.mount)
18:55:35.011960688: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.011963071: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator/run-docker.mount)
18:55:35.011966909: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.011968713: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/local/lib/systemd/system/run-docker.mount)
18:55:35.011972571: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.011974750: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/lib/systemd/system/run-docker.mount)
18:55:35.011978398: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.011980779: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator.late/run-docker.mount)
18:55:35.011986455: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.011989537: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/etc/systemd/system/run-docker-runtime\x2drunc.mount)
18:55:35.011993734: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.011996227: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/system/run-docker-runtime\x2drunc.mount)
18:55:35.012000493: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.012003361: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator/run-docker-runtime\x2drunc.mount)
18:55:35.012007635: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.012009149: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/local/lib/systemd/system/run-docker-runtime\x2drunc.mount)
18:55:35.012013353: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.012015577: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/lib/systemd/system/run-docker-runtime\x2drunc.mount)
18:55:35.012019785: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.012022161: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator.late/run-docker-runtime\x2drunc.mount)
18:55:35.012027669: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.012029888: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/etc/systemd/system/run-docker-runtime\x2drunc-moby.mount)
18:55:35.012034138: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.012036637: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/system/run-docker-runtime\x2drunc-moby.mount)
18:55:35.012040971: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.012044010: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator/run-docker-runtime\x2drunc-moby.mount)
18:55:35.012048495: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.012049983: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/local/lib/systemd/system/run-docker-runtime\x2drunc-moby.mount)
18:55:35.012054212: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.012056470: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/lib/systemd/system/run-docker-runtime\x2drunc-moby.mount)
18:55:35.012060773: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.012062876: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator.late/run-docker-runtime\x2drunc-moby.mount)
18:55:35.012070435: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.012072710: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/etc/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e.mount)
18:55:35.012079445: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.012082017: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e.mount)
18:55:35.012088454: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.012091286: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e.mount)
18:55:35.012097835: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.012099460: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/local/lib/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e.mount)
18:55:35.012105804: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.012108360: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/usr/lib/systemd/system/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e.mount)
18:55:35.012114755: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.012117892: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/systemd/generator.late/run-docker-runtime\x2drunc-moby-954874082b4930f1e1b2b664003443d1565adf7cce086d32404cbc3e087d401e.mount)
18:55:35.040018494: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.040033436: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/proc/self/mountinfo)
18:55:35.041459253: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.041466154: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/mount/utab)
18:55:35.041791660: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.041807214: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/sys/devices/virtual/block/dm-0/uevent)
18:55:35.041887114: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.041898105: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/udev/data/b253:0)
18:55:35.042066636: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.042081213: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/sys/devices/pci0000:00/0000:00:0d.0/ata3/host2/target2:0:0/2:0:0:0/block/sda/sda1/uevent)
18:55:35.042150449: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.042158128: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/udev/data/b8:1)
18:55:35.043024448: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.043037910: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/sys/devices/virtual/block/dm-0/uevent)
18:55:35.043104054: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.043111503: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/udev/data/b253:0)
18:55:35.047203722: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.047220240: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/proc/self/mountinfo)
18:55:35.056180490: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.056186944: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/mount/utab)
18:55:35.058029567: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.058041705: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/sys/devices/virtual/block/dm-0/uevent)
18:55:35.058096299: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.058102964: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/udev/data/b253:0)
18:55:35.059053019: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.059070756: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/sys/devices/pci0000:00/0000:00:0d.0/ata3/host2/target2:0:0/2:0:0:0/block/sda/sda1/uevent)
18:55:35.059253150: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=<NA>)
18:55:35.059260949: Warning File opened by systemd (user=root command=systemd --switched-root --system --deserialize 22 file=/run/udev/data/b8:1)
^CSat Aug 28 18:55:39 2021: SIGINT received, exiting...
Events detected: 347
Rule counts by severity:
   INFO: 3
   WARNING: 344
Triggered rules by rule name:
   Any Open Activity by Systemd: 344
   Launch Privileged Container: 3
Syscall event drop monitoring:
   - event drop detected: 0 occurrences
   - num times actions taken: 0

```



{% embed url="https://github.com/falcosecurity/falco" %}

{% embed url="https://falco.org/docs/getting-started/installation/" %}



