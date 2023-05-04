---
description: >-
  Scanner for vulnerabilities in container images, file systems, and Git
  repositories, as well as for configuration issues
---

# trivy

## trivy image&#x20;

### official nginx

```
[root@m-k8s ~]# trivy image nginx
2021-08-28T07:41:59.246+0900    INFO    Need to update DB
2021-08-28T07:41:59.246+0900    INFO    Downloading DB...
23.08 MiB / 23.08 MiB [-------------------------------------------------------------------------------------------------------------------------------------------------------------------] 100.00% 4.83 MiB p/s 5s
2021-08-28T07:42:19.101+0900    INFO    Detected OS: debian
2021-08-28T07:42:19.102+0900    INFO    Detecting Debian vulnerabilities...
2021-08-28T07:42:19.282+0900    INFO    Number of language-specific files: 1

nginx (debian 10.10)
====================
Total: 181 (UNKNOWN: 1, LOW: 129, MEDIUM: 17, HIGH: 30, CRITICAL: 4)

+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
|     LIBRARY      |  VULNERABILITY ID   | SEVERITY |     INSTALLED VERSION     |  FIXED VERSION   |                            TITLE                             |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| apt              | CVE-2011-3374       | LOW      | 1.8.2.3                   |                  | It was found that apt-key in apt,                            |
|                  |                     |          |                           |                  | all versions, do not correctly...                            |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2011-3374                         |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| bash             | CVE-2019-18276      |          | 5.0-4                     |                  | bash: when effective UID is not                              |
|                  |                     |          |                           |                  | equal to its real UID the...                                 |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-18276                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | TEMP-0841856-B18BAF |          |                           |                  | -->security-tracker.debian.org/tracker/TEMP-0841856-B18BAF   |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| bsdutils         | CVE-2021-37600      |          | 2.33.1-0.1                |                  | util-linux: integer overflow                                 |
|                  |                     |          |                           |                  | can lead to buffer overflow                                  |
|                  |                     |          |                           |                  | in get_sem_elements() in                                     |
|                  |                     |          |                           |                  | sys-utils/ipcutils.c...                                      |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-37600                        |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| coreutils        | CVE-2016-2781       |          | 8.30-3                    |                  | coreutils: Non-privileged                                    |
|                  |                     |          |                           |                  | session can escape to the                                    |
|                  |                     |          |                           |                  | parent session in chroot                                     |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2016-2781                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2017-18018      |          |                           |                  | coreutils: race condition                                    |
|                  |                     |          |                           |                  | vulnerability in chown and chgrp                             |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2017-18018                        |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| curl             | CVE-2021-22924      | HIGH     | 7.64.0-4+deb10u2          |                  | curl: Bad connection reuse                                   |
|                  |                     |          |                           |                  | due to flawed path name checks                               |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-22924                        |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2021-22898      | LOW      |                           |                  | curl: TELNET stack                                           |
|                  |                     |          |                           |                  | contents disclosure                                          |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-22898                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2021-22922      |          |                           |                  | curl: Content not matching hash                              |
|                  |                     |          |                           |                  | in Metalink is not being discarded                           |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-22922                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2021-22923      |          |                           |                  | curl: Metalink download                                      |
|                  |                     |          |                           |                  | sends credentials                                            |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-22923                        |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| fdisk            | CVE-2021-37600      |          | 2.33.1-0.1                |                  | util-linux: integer overflow                                 |
|                  |                     |          |                           |                  | can lead to buffer overflow                                  |
|                  |                     |          |                           |                  | in get_sem_elements() in                                     |
|                  |                     |          |                           |                  | sys-utils/ipcutils.c...                                      |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-37600                        |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| gcc-8-base       | CVE-2018-12886      | HIGH     | 8.3.0-6                   |                  | gcc: spilling of stack                                       |
|                  |                     |          |                           |                  | protection address in cfgexpand.c                            |
|                  |                     |          |                           |                  | and function.c leads to...                                   |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2018-12886                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-15847      |          |                           |                  | gcc: POWER9 "DARN" RNG intrinsic                             |
|                  |                     |          |                           |                  | produces repeated output                                     |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-15847                        |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| gpgv             | CVE-2019-14855      | LOW      | 2.2.12-1+deb10u1          |                  | gnupg2: OpenPGP Key Certification                            |
|                  |                     |          |                           |                  | Forgeries with SHA-1                                         |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-14855                        |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| libapt-pkg5.0    | CVE-2011-3374       |          | 1.8.2.3                   |                  | It was found that apt-key in apt,                            |
|                  |                     |          |                           |                  | all versions, do not correctly...                            |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2011-3374                         |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| libblkid1        | CVE-2021-37600      |          | 2.33.1-0.1                |                  | util-linux: integer overflow                                 |
|                  |                     |          |                           |                  | can lead to buffer overflow                                  |
|                  |                     |          |                           |                  | in get_sem_elements() in                                     |
|                  |                     |          |                           |                  | sys-utils/ipcutils.c...                                      |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-37600                        |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| libc-bin         | CVE-2021-33574      | CRITICAL | 2.28-10                   |                  | glibc: mq_notify does                                        |
|                  |                     |          |                           |                  | not handle separately                                        |
|                  |                     |          |                           |                  | allocated thread attributes                                  |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-33574                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2021-35942      |          |                           |                  | glibc: Arbitrary read in wordexp()                           |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-35942                        |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2020-1751       | HIGH     |                           |                  | glibc: array overflow in                                     |
|                  |                     |          |                           |                  | backtrace functions for powerpc                              |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2020-1751                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2020-1752       |          |                           |                  | glibc: use-after-free in glob()                              |
|                  |                     |          |                           |                  | function when expanding ~user                                |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2020-1752                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2021-3326       |          |                           |                  | glibc: Assertion failure in                                  |
|                  |                     |          |                           |                  | ISO-2022-JP-3 gconv module                                   |
|                  |                     |          |                           |                  | related to combining characters                              |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-3326                         |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-25013      | MEDIUM   |                           |                  | glibc: buffer over-read in                                   |
|                  |                     |          |                           |                  | iconv when processing invalid                                |
|                  |                     |          |                           |                  | multi-byte input sequences in...                             |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-25013                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2020-10029      |          |                           |                  | glibc: stack corruption                                      |
|                  |                     |          |                           |                  | from crafted input in cosl,                                  |
|                  |                     |          |                           |                  | sinl, sincosl, and tanl...                                   |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2020-10029                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2020-27618      |          |                           |                  | glibc: iconv when processing                                 |
|                  |                     |          |                           |                  | invalid multi-byte input                                     |
|                  |                     |          |                           |                  | sequences fails to advance the...                            |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2020-27618                        |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2010-4051       | LOW      |                           |                  | CVE-2010-4052 glibc: De-recursivise                          |
|                  |                     |          |                           |                  | regular expression engine                                    |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2010-4051                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2010-4052       |          |                           |                  | CVE-2010-4051 CVE-2010-4052                                  |
|                  |                     |          |                           |                  | glibc: De-recursivise                                        |
|                  |                     |          |                           |                  | regular expression engine                                    |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2010-4052                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2010-4756       |          |                           |                  | glibc: glob implementation                                   |
|                  |                     |          |                           |                  | can cause excessive CPU and                                  |
|                  |                     |          |                           |                  | memory consumption due to...                                 |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2010-4756                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2016-10228      |          |                           |                  | glibc: iconv program can hang                                |
|                  |                     |          |                           |                  | when invoked with the -c option                              |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2016-10228                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2018-20796      |          |                           |                  | glibc: uncontrolled recursion in                             |
|                  |                     |          |                           |                  | function check_dst_limits_calc_pos_1                         |
|                  |                     |          |                           |                  | in posix/regexec.c                                           |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2018-20796                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-1010022    |          |                           |                  | glibc: stack guard protection bypass                         |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-1010022                      |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-1010023    |          |                           |                  | glibc: running ldd on malicious ELF                          |
|                  |                     |          |                           |                  | leads to code execution because of...                        |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-1010023                      |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-1010024    |          |                           |                  | glibc: ASLR bypass using                                     |
|                  |                     |          |                           |                  | cache of thread stack and heap                               |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-1010024                      |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-1010025    |          |                           |                  | glibc: information disclosure of heap                        |
|                  |                     |          |                           |                  | addresses of pthread_created thread                          |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-1010025                      |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-19126      |          |                           |                  | glibc: LD_PREFER_MAP_32BIT_EXEC                              |
|                  |                     |          |                           |                  | not ignored in setuid binaries                               |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-19126                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-9192       |          |                           |                  | glibc: uncontrolled recursion in                             |
|                  |                     |          |                           |                  | function check_dst_limits_calc_pos_1                         |
|                  |                     |          |                           |                  | in posix/regexec.c                                           |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-9192                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2020-6096       |          |                           |                  | glibc: signed comparison                                     |
|                  |                     |          |                           |                  | vulnerability in the                                         |
|                  |                     |          |                           |                  | ARMv7 memcpy function                                        |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2020-6096                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2021-27645      |          |                           |                  | glibc: Use-after-free in                                     |
|                  |                     |          |                           |                  | addgetnetgrentX function                                     |
|                  |                     |          |                           |                  | in netgroupcache.c                                           |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-27645                        |
+------------------+---------------------+----------+                           +------------------+--------------------------------------------------------------+
| libc6            | CVE-2021-33574      | CRITICAL |                           |                  | glibc: mq_notify does                                        |
|                  |                     |          |                           |                  | not handle separately                                        |
|                  |                     |          |                           |                  | allocated thread attributes                                  |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-33574                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2021-35942      |          |                           |                  | glibc: Arbitrary read in wordexp()                           |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-35942                        |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2020-1751       | HIGH     |                           |                  | glibc: array overflow in                                     |
|                  |                     |          |                           |                  | backtrace functions for powerpc                              |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2020-1751                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2020-1752       |          |                           |                  | glibc: use-after-free in glob()                              |
|                  |                     |          |                           |                  | function when expanding ~user                                |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2020-1752                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2021-3326       |          |                           |                  | glibc: Assertion failure in                                  |
|                  |                     |          |                           |                  | ISO-2022-JP-3 gconv module                                   |
|                  |                     |          |                           |                  | related to combining characters                              |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-3326                         |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-25013      | MEDIUM   |                           |                  | glibc: buffer over-read in                                   |
|                  |                     |          |                           |                  | iconv when processing invalid                                |
|                  |                     |          |                           |                  | multi-byte input sequences in...                             |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-25013                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2020-10029      |          |                           |                  | glibc: stack corruption                                      |
|                  |                     |          |                           |                  | from crafted input in cosl,                                  |
|                  |                     |          |                           |                  | sinl, sincosl, and tanl...                                   |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2020-10029                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2020-27618      |          |                           |                  | glibc: iconv when processing                                 |
|                  |                     |          |                           |                  | invalid multi-byte input                                     |
|                  |                     |          |                           |                  | sequences fails to advance the...                            |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2020-27618                        |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2010-4051       | LOW      |                           |                  | CVE-2010-4052 glibc: De-recursivise                          |
|                  |                     |          |                           |                  | regular expression engine                                    |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2010-4051                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2010-4052       |          |                           |                  | CVE-2010-4051 CVE-2010-4052                                  |
|                  |                     |          |                           |                  | glibc: De-recursivise                                        |
|                  |                     |          |                           |                  | regular expression engine                                    |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2010-4052                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2010-4756       |          |                           |                  | glibc: glob implementation                                   |
|                  |                     |          |                           |                  | can cause excessive CPU and                                  |
|                  |                     |          |                           |                  | memory consumption due to...                                 |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2010-4756                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2016-10228      |          |                           |                  | glibc: iconv program can hang                                |
|                  |                     |          |                           |                  | when invoked with the -c option                              |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2016-10228                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2018-20796      |          |                           |                  | glibc: uncontrolled recursion in                             |
|                  |                     |          |                           |                  | function check_dst_limits_calc_pos_1                         |
|                  |                     |          |                           |                  | in posix/regexec.c                                           |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2018-20796                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-1010022    |          |                           |                  | glibc: stack guard protection bypass                         |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-1010022                      |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-1010023    |          |                           |                  | glibc: running ldd on malicious ELF                          |
|                  |                     |          |                           |                  | leads to code execution because of...                        |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-1010023                      |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-1010024    |          |                           |                  | glibc: ASLR bypass using                                     |
|                  |                     |          |                           |                  | cache of thread stack and heap                               |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-1010024                      |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-1010025    |          |                           |                  | glibc: information disclosure of heap                        |
|                  |                     |          |                           |                  | addresses of pthread_created thread                          |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-1010025                      |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-19126      |          |                           |                  | glibc: LD_PREFER_MAP_32BIT_EXEC                              |
|                  |                     |          |                           |                  | not ignored in setuid binaries                               |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-19126                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-9192       |          |                           |                  | glibc: uncontrolled recursion in                             |
|                  |                     |          |                           |                  | function check_dst_limits_calc_pos_1                         |
|                  |                     |          |                           |                  | in posix/regexec.c                                           |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-9192                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2020-6096       |          |                           |                  | glibc: signed comparison                                     |
|                  |                     |          |                           |                  | vulnerability in the                                         |
|                  |                     |          |                           |                  | ARMv7 memcpy function                                        |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2020-6096                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2021-27645      |          |                           |                  | glibc: Use-after-free in                                     |
|                  |                     |          |                           |                  | addgetnetgrentX function                                     |
|                  |                     |          |                           |                  | in netgroupcache.c                                           |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-27645                        |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| libcurl4         | CVE-2021-22924      | HIGH     | 7.64.0-4+deb10u2          |                  | curl: Bad connection reuse                                   |
|                  |                     |          |                           |                  | due to flawed path name checks                               |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-22924                        |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2021-22898      | LOW      |                           |                  | curl: TELNET stack                                           |
|                  |                     |          |                           |                  | contents disclosure                                          |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-22898                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2021-22922      |          |                           |                  | curl: Content not matching hash                              |
|                  |                     |          |                           |                  | in Metalink is not being discarded                           |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-22922                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2021-22923      |          |                           |                  | curl: Metalink download                                      |
|                  |                     |          |                           |                  | sends credentials                                            |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-22923                        |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| libexpat1        | CVE-2013-0340       |          | 2.2.6-2+deb10u1           |                  | expat: internal entity expansion                             |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2013-0340                         |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| libfdisk1        | CVE-2021-37600      |          | 2.33.1-0.1                |                  | util-linux: integer overflow                                 |
|                  |                     |          |                           |                  | can lead to buffer overflow                                  |
|                  |                     |          |                           |                  | in get_sem_elements() in                                     |
|                  |                     |          |                           |                  | sys-utils/ipcutils.c...                                      |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-37600                        |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| libgcc1          | CVE-2018-12886      | HIGH     | 8.3.0-6                   |                  | gcc: spilling of stack                                       |
|                  |                     |          |                           |                  | protection address in cfgexpand.c                            |
|                  |                     |          |                           |                  | and function.c leads to...                                   |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2018-12886                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-15847      |          |                           |                  | gcc: POWER9 "DARN" RNG intrinsic                             |
|                  |                     |          |                           |                  | produces repeated output                                     |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-15847                        |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| libgcrypt20      | CVE-2019-13627      | MEDIUM   | 1.8.4-5+deb10u1           |                  | libgcrypt: ECDSA timing attack                               |
|                  |                     |          |                           |                  | allowing private key leak                                    |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-13627                        |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2018-6829       | LOW      |                           |                  | libgcrypt: ElGamal implementation                            |
|                  |                     |          |                           |                  | doesn't have semantic security due                           |
|                  |                     |          |                           |                  | to incorrectly encoded plaintexts...                         |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2018-6829                         |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| libgd3           | CVE-2017-6363       | HIGH     | 2.2.5-5.2                 |                  | ** DISPUTED ** In the                                        |
|                  |                     |          |                           |                  | GD Graphics Library (aka                                     |
|                  |                     |          |                           |                  | LibGD) through 2.2.5,...                                     |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2017-6363                         |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2021-38115      | MEDIUM   |                           |                  | read_header_tga in gd_tga.c                                  |
|                  |                     |          |                           |                  | in the GD Graphics Library                                   |
|                  |                     |          |                           |                  | (aka LibGD) through 2.3.2...                                 |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-38115                        |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2018-14553      | LOW      |                           |                  | gd: NULL pointer                                             |
|                  |                     |          |                           |                  | dereference in gdImageClone                                  |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2018-14553                        |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2021-40145      | UNKNOWN  |                           |                  | ** DISPUTED ** gdImageGd2Ptr                                 |
|                  |                     |          |                           |                  | in gd_gd2.c in the GD                                        |
|                  |                     |          |                           |                  | Graphics Library (aka...                                     |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-40145                        |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| libgnutls30      | CVE-2011-3389       | LOW      | 3.6.7-4+deb10u7           |                  | HTTPS: block-wise chosen-plaintext                           |
|                  |                     |          |                           |                  | attack against SSL/TLS (BEAST)                               |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2011-3389                         |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| libgssapi-krb5-2 | CVE-2021-36222      | HIGH     | 1.17-3+deb10u1            | 1.17-3+deb10u2   | krb5: sending a request containing                           |
|                  |                     |          |                           |                  | a PA-ENCRYPTED-CHALLENGE padata                              |
|                  |                     |          |                           |                  | element without using FAST...                                |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-36222                        |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2021-37750      | MEDIUM   |                           |                  | krb5: NULL pointer dereference                               |
|                  |                     |          |                           |                  | in process_tgs_req() in                                      |
|                  |                     |          |                           |                  | kdc/do_tgs_req.c via a FAST inner...                         |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-37750                        |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2004-0971       | LOW      |                           |                  | security flaw                                                |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2004-0971                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2018-5709       |          |                           |                  | krb5: integer overflow                                       |
|                  |                     |          |                           |                  | in dbentry->n_key_data                                       |
|                  |                     |          |                           |                  | in kadmin/dbutil/dump.c                                      |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2018-5709                         |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| libicu63         | CVE-2021-30535      | HIGH     | 63.1-6+deb10u1            |                  | Double free in ICU in Google Chrome                          |
|                  |                     |          |                           |                  | prior to 91.0.4472.77 allowed a...                           |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-30535                        |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| libidn2-0        | CVE-2019-12290      |          | 2.0.5-1+deb10u1           |                  | GNU libidn2 before 2.2.0                                     |
|                  |                     |          |                           |                  | fails to perform the roundtrip                               |
|                  |                     |          |                           |                  | checks specified in...                                       |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-12290                        |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| libjbig0         | CVE-2017-9937       | LOW      | 2.1-3.1                   |                  | libtiff: memory malloc failure                               |
|                  |                     |          |                           |                  | in tif_jbig.c could cause DOS.                               |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2017-9937                         |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| libjpeg62-turbo  | CVE-2017-15232      |          | 1:1.5.2-2+deb10u1         |                  | libjpeg-turbo: NULL                                          |
|                  |                     |          |                           |                  | pointer dereference in                                       |
|                  |                     |          |                           |                  | jdpostct.c and jquant1.c                                     |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2017-15232                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2018-11813      |          |                           |                  | libjpeg: "cjpeg" utility                                     |
|                  |                     |          |                           |                  | large loop because read_pixel                                |
|                  |                     |          |                           |                  | in rdtarga.c mishandles EOF                                  |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2018-11813                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2020-17541      |          |                           |                  | libjpeg-turbo: Stack-based buffer                            |
|                  |                     |          |                           |                  | overflow in the "transform" component                        |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2020-17541                        |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| libk5crypto3     | CVE-2021-36222      | HIGH     | 1.17-3+deb10u1            | 1.17-3+deb10u2   | krb5: sending a request containing                           |
|                  |                     |          |                           |                  | a PA-ENCRYPTED-CHALLENGE padata                              |
|                  |                     |          |                           |                  | element without using FAST...                                |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-36222                        |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2021-37750      | MEDIUM   |                           |                  | krb5: NULL pointer dereference                               |
|                  |                     |          |                           |                  | in process_tgs_req() in                                      |
|                  |                     |          |                           |                  | kdc/do_tgs_req.c via a FAST inner...                         |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-37750                        |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2004-0971       | LOW      |                           |                  | security flaw                                                |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2004-0971                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2018-5709       |          |                           |                  | krb5: integer overflow                                       |
|                  |                     |          |                           |                  | in dbentry->n_key_data                                       |
|                  |                     |          |                           |                  | in kadmin/dbutil/dump.c                                      |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2018-5709                         |
+------------------+---------------------+----------+                           +------------------+--------------------------------------------------------------+
| libkrb5-3        | CVE-2021-36222      | HIGH     |                           | 1.17-3+deb10u2   | krb5: sending a request containing                           |
|                  |                     |          |                           |                  | a PA-ENCRYPTED-CHALLENGE padata                              |
|                  |                     |          |                           |                  | element without using FAST...                                |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-36222                        |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2021-37750      | MEDIUM   |                           |                  | krb5: NULL pointer dereference                               |
|                  |                     |          |                           |                  | in process_tgs_req() in                                      |
|                  |                     |          |                           |                  | kdc/do_tgs_req.c via a FAST inner...                         |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-37750                        |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2004-0971       | LOW      |                           |                  | security flaw                                                |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2004-0971                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2018-5709       |          |                           |                  | krb5: integer overflow                                       |
|                  |                     |          |                           |                  | in dbentry->n_key_data                                       |
|                  |                     |          |                           |                  | in kadmin/dbutil/dump.c                                      |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2018-5709                         |
+------------------+---------------------+----------+                           +------------------+--------------------------------------------------------------+
| libkrb5support0  | CVE-2021-36222      | HIGH     |                           | 1.17-3+deb10u2   | krb5: sending a request containing                           |
|                  |                     |          |                           |                  | a PA-ENCRYPTED-CHALLENGE padata                              |
|                  |                     |          |                           |                  | element without using FAST...                                |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-36222                        |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2021-37750      | MEDIUM   |                           |                  | krb5: NULL pointer dereference                               |
|                  |                     |          |                           |                  | in process_tgs_req() in                                      |
|                  |                     |          |                           |                  | kdc/do_tgs_req.c via a FAST inner...                         |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-37750                        |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2004-0971       | LOW      |                           |                  | security flaw                                                |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2004-0971                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2018-5709       |          |                           |                  | krb5: integer overflow                                       |
|                  |                     |          |                           |                  | in dbentry->n_key_data                                       |
|                  |                     |          |                           |                  | in kadmin/dbutil/dump.c                                      |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2018-5709                         |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| libldap-2.4-2    | CVE-2015-3276       |          | 2.4.47+dfsg-3+deb10u6     |                  | openldap: incorrect multi-keyword                            |
|                  |                     |          |                           |                  | mode cipherstring parsing                                    |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2015-3276                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2017-14159      |          |                           |                  | openldap: Privilege escalation                               |
|                  |                     |          |                           |                  | via PID file manipulation                                    |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2017-14159                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2017-17740      |          |                           |                  | openldap:                                                    |
|                  |                     |          |                           |                  | contrib/slapd-modules/nops/nops.c                            |
|                  |                     |          |                           |                  | attempts to free stack buffer                                |
|                  |                     |          |                           |                  | allowing remote attackers to cause...                        |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2017-17740                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2020-15719      |          |                           |                  | openldap: Certificate                                        |
|                  |                     |          |                           |                  | validation incorrectly                                       |
|                  |                     |          |                           |                  | matches name against CN-ID                                   |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2020-15719                        |
+------------------+---------------------+          +                           +------------------+--------------------------------------------------------------+
| libldap-common   | CVE-2015-3276       |          |                           |                  | openldap: incorrect multi-keyword                            |
|                  |                     |          |                           |                  | mode cipherstring parsing                                    |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2015-3276                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2017-14159      |          |                           |                  | openldap: Privilege escalation                               |
|                  |                     |          |                           |                  | via PID file manipulation                                    |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2017-14159                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2017-17740      |          |                           |                  | openldap:                                                    |
|                  |                     |          |                           |                  | contrib/slapd-modules/nops/nops.c                            |
|                  |                     |          |                           |                  | attempts to free stack buffer                                |
|                  |                     |          |                           |                  | allowing remote attackers to cause...                        |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2017-17740                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2020-15719      |          |                           |                  | openldap: Certificate                                        |
|                  |                     |          |                           |                  | validation incorrectly                                       |
|                  |                     |          |                           |                  | matches name against CN-ID                                   |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2020-15719                        |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| liblz4-1         | CVE-2019-17543      |          | 1.8.3-1+deb10u1           |                  | lz4: heap-based buffer                                       |
|                  |                     |          |                           |                  | overflow in LZ4_write32                                      |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-17543                        |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| libmount1        | CVE-2021-37600      |          | 2.33.1-0.1                |                  | util-linux: integer overflow                                 |
|                  |                     |          |                           |                  | can lead to buffer overflow                                  |
|                  |                     |          |                           |                  | in get_sem_elements() in                                     |
|                  |                     |          |                           |                  | sys-utils/ipcutils.c...                                      |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-37600                        |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| libnghttp2-14    | TEMP-0000000-A4EF31 |          | 1.36.0-2+deb10u1          |                  | -->security-tracker.debian.org/tracker/TEMP-0000000-A4EF31   |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| libpcre3         | CVE-2020-14155      | MEDIUM   | 2:8.39-12                 |                  | pcre: integer overflow in libpcre                            |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2020-14155                        |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2017-11164      | LOW      |                           |                  | pcre: OP_KETRMAX feature in the                              |
|                  |                     |          |                           |                  | match function in pcre_exec.c                                |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2017-11164                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2017-16231      |          |                           |                  | pcre: self-recursive call                                    |
|                  |                     |          |                           |                  | in match() in pcre_exec.c                                    |
|                  |                     |          |                           |                  | leads to denial of service...                                |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2017-16231                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2017-7245       |          |                           |                  | pcre: stack-based buffer overflow                            |
|                  |                     |          |                           |                  | write in pcre32_copy_substring                               |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2017-7245                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2017-7246       |          |                           |                  | pcre: stack-based buffer overflow                            |
|                  |                     |          |                           |                  | write in pcre32_copy_substring                               |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2017-7246                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-20838      |          |                           |                  | pcre: buffer over-read in                                    |
|                  |                     |          |                           |                  | JIT when UTF is disabled                                     |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-20838                        |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| libpng16-16      | CVE-2018-14048      |          | 1.6.36-6                  |                  | libpng: Segmentation fault in                                |
|                  |                     |          |                           |                  | png.c:png_free_data function                                 |
|                  |                     |          |                           |                  | causing denial of service                                    |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2018-14048                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2018-14550      |          |                           |                  | libpng: Stack-based buffer overflow in                       |
|                  |                     |          |                           |                  | contrib/pngminus/pnm2png.c:get_token()                       |
|                  |                     |          |                           |                  | potentially leading to                                       |
|                  |                     |          |                           |                  | arbitrary code execution...                                  |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2018-14550                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-6129       |          |                           |                  | libpng: memory leak of                                       |
|                  |                     |          |                           |                  | png_info struct in pngcp.c                                   |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-6129                         |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| libseccomp2      | CVE-2019-9893       |          | 2.3.3-4                   |                  | libseccomp: incorrect generation                             |
|                  |                     |          |                           |                  | of syscall filters in libseccomp                             |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-9893                         |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| libsepol1        | CVE-2021-36084      |          | 2.8-1                     |                  | libsepol: use-after-free in                                  |
|                  |                     |          |                           |                  | __cil_verify_classperms()                                    |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-36084                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2021-36085      |          |                           |                  | libsepol: use-after-free in                                  |
|                  |                     |          |                           |                  | __cil_verify_classperms()                                    |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-36085                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2021-36086      |          |                           |                  | libsepol: use-after-free in                                  |
|                  |                     |          |                           |                  | cil_reset_classpermission()                                  |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-36086                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2021-36087      |          |                           |                  | libsepol: heap-based buffer                                  |
|                  |                     |          |                           |                  | overflow in ebitmap_match_any()                              |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-36087                        |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| libsmartcols1    | CVE-2021-37600      |          | 2.33.1-0.1                |                  | util-linux: integer overflow                                 |
|                  |                     |          |                           |                  | can lead to buffer overflow                                  |
|                  |                     |          |                           |                  | in get_sem_elements() in                                     |
|                  |                     |          |                           |                  | sys-utils/ipcutils.c...                                      |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-37600                        |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| libssh2-1        | CVE-2019-13115      | HIGH     | 1.8.0-2.1                 |                  | libssh2: integer overflow in                                 |
|                  |                     |          |                           |                  | kex_method_diffie_hellman_group_exchange_sha256_key_exchange |
|                  |                     |          |                           |                  | in kex.c leads to out-of-bounds write                        |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-13115                        |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-17498      | LOW      |                           |                  | libssh2: integer overflow in                                 |
|                  |                     |          |                           |                  | SSH_MSG_DISCONNECT logic in packet.c                         |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-17498                        |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| libssl1.1        | CVE-2021-3711       | HIGH     | 1.1.1d-0+deb10u6          | 1.1.1d-0+deb10u7 | openssl: SM2 Decryption                                      |
|                  |                     |          |                           |                  | Buffer Overflow                                              |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-3711                         |
+                  +---------------------+----------+                           +                  +--------------------------------------------------------------+
|                  | CVE-2021-3712       | MEDIUM   |                           |                  | openssl: Read buffer overruns                                |
|                  |                     |          |                           |                  | processing ASN.1 strings                                     |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-3712                         |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2007-6755       | LOW      |                           |                  | Dual_EC_DRBG: weak pseudo                                    |
|                  |                     |          |                           |                  | random number generator                                      |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2007-6755                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2010-0928       |          |                           |                  | openssl: RSA authentication weakness                         |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2010-0928                         |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| libstdc++6       | CVE-2018-12886      | HIGH     | 8.3.0-6                   |                  | gcc: spilling of stack                                       |
|                  |                     |          |                           |                  | protection address in cfgexpand.c                            |
|                  |                     |          |                           |                  | and function.c leads to...                                   |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2018-12886                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-15847      |          |                           |                  | gcc: POWER9 "DARN" RNG intrinsic                             |
|                  |                     |          |                           |                  | produces repeated output                                     |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-15847                        |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| libsystemd0      | CVE-2019-3843       |          | 241-7~deb10u8             |                  | systemd: services with DynamicUser                           |
|                  |                     |          |                           |                  | can create SUID/SGID binaries                                |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-3843                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-3844       |          |                           |                  | systemd: services with DynamicUser                           |
|                  |                     |          |                           |                  | can get new privileges and                                   |
|                  |                     |          |                           |                  | create SGID binaries...                                      |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-3844                         |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2013-4392       | LOW      |                           |                  | systemd: TOCTOU race condition                               |
|                  |                     |          |                           |                  | when updating file permissions                               |
|                  |                     |          |                           |                  | and SELinux security contexts...                             |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2013-4392                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-20386      |          |                           |                  | systemd: memory leak in button_open()                        |
|                  |                     |          |                           |                  | in login/logind-button.c when                                |
|                  |                     |          |                           |                  | udev events are received...                                  |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-20386                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2020-13529      |          |                           |                  | systemd: DHCP FORCERENEW                                     |
|                  |                     |          |                           |                  | authentication not implemented                               |
|                  |                     |          |                           |                  | can cause a system running the...                            |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2020-13529                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2020-13776      |          |                           |                  | systemd: Mishandles numerical                                |
|                  |                     |          |                           |                  | usernames beginning with decimal                             |
|                  |                     |          |                           |                  | digits or 0x followed by...                                  |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2020-13776                        |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| libtasn1-6       | CVE-2018-1000654    |          | 4.13-3                    |                  | libtasn1: Infinite loop in                                   |
|                  |                     |          |                           |                  | _asn1_expand_object_id(ptree)                                |
|                  |                     |          |                           |                  | leads to memory exhaustion                                   |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2018-1000654                      |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| libtiff5         | CVE-2014-8130       |          | 4.1.0+git191117-2~deb10u2 |                  | libtiff: divide by zero                                      |
|                  |                     |          |                           |                  | in the tiffdither tool                                       |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2014-8130                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2017-16232      |          |                           |                  | libtiff: Memory leaks in                                     |
|                  |                     |          |                           |                  | tif_open.c, tif_lzw.c, and tif_aux.c                         |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2017-16232                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2017-17973      |          |                           |                  | libtiff: heap-based use after                                |
|                  |                     |          |                           |                  | free in tiff2pdf.c:t2p_writeproc                             |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2017-17973                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2017-5563       |          |                           |                  | libtiff: Heap-buffer overflow                                |
|                  |                     |          |                           |                  | in LZWEncode tif_lzw.c                                       |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2017-5563                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2017-9117       |          |                           |                  | libtiff: Heap-based buffer                                   |
|                  |                     |          |                           |                  | over-read in bmp2tiff                                        |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2017-9117                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2018-10126      |          |                           |                  | libtiff: NULL pointer dereference                            |
|                  |                     |          |                           |                  | in the jpeg_fdct_16x16                                       |
|                  |                     |          |                           |                  | function in jfdctint.c                                       |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2018-10126                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2020-35521      |          |                           |                  | libtiff: Memory allocation                                   |
|                  |                     |          |                           |                  | failure in tiff2rgba                                         |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2020-35521                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2020-35522      |          |                           |                  | libtiff: Memory allocation                                   |
|                  |                     |          |                           |                  | failure in tiff2rgba                                         |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2020-35522                        |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| libudev1         | CVE-2019-3843       | HIGH     | 241-7~deb10u8             |                  | systemd: services with DynamicUser                           |
|                  |                     |          |                           |                  | can create SUID/SGID binaries                                |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-3843                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-3844       |          |                           |                  | systemd: services with DynamicUser                           |
|                  |                     |          |                           |                  | can get new privileges and                                   |
|                  |                     |          |                           |                  | create SGID binaries...                                      |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-3844                         |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2013-4392       | LOW      |                           |                  | systemd: TOCTOU race condition                               |
|                  |                     |          |                           |                  | when updating file permissions                               |
|                  |                     |          |                           |                  | and SELinux security contexts...                             |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2013-4392                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-20386      |          |                           |                  | systemd: memory leak in button_open()                        |
|                  |                     |          |                           |                  | in login/logind-button.c when                                |
|                  |                     |          |                           |                  | udev events are received...                                  |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-20386                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2020-13529      |          |                           |                  | systemd: DHCP FORCERENEW                                     |
|                  |                     |          |                           |                  | authentication not implemented                               |
|                  |                     |          |                           |                  | can cause a system running the...                            |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2020-13529                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2020-13776      |          |                           |                  | systemd: Mishandles numerical                                |
|                  |                     |          |                           |                  | usernames beginning with decimal                             |
|                  |                     |          |                           |                  | digits or 0x followed by...                                  |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2020-13776                        |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| libuuid1         | CVE-2021-37600      |          | 2.33.1-0.1                |                  | util-linux: integer overflow                                 |
|                  |                     |          |                           |                  | can lead to buffer overflow                                  |
|                  |                     |          |                           |                  | in get_sem_elements() in                                     |
|                  |                     |          |                           |                  | sys-utils/ipcutils.c...                                      |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-37600                        |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| libwebp6         | CVE-2016-9085       |          | 0.6.1-2+deb10u1           |                  | libwebp: Several integer overflows                           |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2016-9085                         |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| libxml2          | CVE-2017-16932      | HIGH     | 2.9.4+dfsg1-7+deb10u2     |                  | libxml2: Infinite recursion                                  |
|                  |                     |          |                           |                  | in parameter entities                                        |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2017-16932                        |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2016-9318       | MEDIUM   |                           |                  | libxml2: XML External                                        |
|                  |                     |          |                           |                  | Entity vulnerability                                         |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2016-9318                         |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| libxslt1.1       | CVE-2015-9019       | LOW      | 1.1.32-2.2~deb10u1        |                  | libxslt: math.random() in                                    |
|                  |                     |          |                           |                  | xslt uses unseeded randomness                                |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2015-9019                         |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| login            | CVE-2007-5686       |          | 1:4.5-1.1                 |                  | initscripts in rPath Linux 1                                 |
|                  |                     |          |                           |                  | sets insecure permissions for                                |
|                  |                     |          |                           |                  | the /var/log/btmp file,...                                   |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2007-5686                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2013-4235       |          |                           |                  | shadow-utils: TOCTOU race                                    |
|                  |                     |          |                           |                  | conditions by copying and                                    |
|                  |                     |          |                           |                  | removing directory trees                                     |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2013-4235                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2018-7169       |          |                           |                  | shadow-utils: newgidmap                                      |
|                  |                     |          |                           |                  | allows unprivileged user to                                  |
|                  |                     |          |                           |                  | drop supplementary groups                                    |
|                  |                     |          |                           |                  | potentially allowing privilege...                            |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2018-7169                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-19882      |          |                           |                  | shadow-utils: local users can                                |
|                  |                     |          |                           |                  | obtain root access because setuid                            |
|                  |                     |          |                           |                  | programs are misconfigured...                                |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-19882                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | TEMP-0628843-DBAD28 |          |                           |                  | -->security-tracker.debian.org/tracker/TEMP-0628843-DBAD28   |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| mount            | CVE-2021-37600      |          | 2.33.1-0.1                |                  | util-linux: integer overflow                                 |
|                  |                     |          |                           |                  | can lead to buffer overflow                                  |
|                  |                     |          |                           |                  | in get_sem_elements() in                                     |
|                  |                     |          |                           |                  | sys-utils/ipcutils.c...                                      |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-37600                        |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| nginx            | CVE-2021-3618       | HIGH     | 1.21.1-1~buster           |                  | ALPACA: Application Layer                                    |
|                  |                     |          |                           |                  | Protocol Confusion - Analyzing                               |
|                  |                     |          |                           |                  | and Mitigating Cracks in TLS...                              |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-3618                         |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2020-36309      | MEDIUM   |                           |                  | ngx_http_lua_module (aka                                     |
|                  |                     |          |                           |                  | lua-nginx-module) before                                     |
|                  |                     |          |                           |                  | 0.10.16 in OpenResty allows                                  |
|                  |                     |          |                           |                  | unsafe characters in an...                                   |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2020-36309                        |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2009-4487       | LOW      |                           |                  | nginx: Absent sanitation of                                  |
|                  |                     |          |                           |                  | escape sequences in web server log                           |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2009-4487                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2013-0337       |          |                           |                  | The default configuration of nginx,                          |
|                  |                     |          |                           |                  | possibly 1.3.13 and earlier, uses                            |
|                  |                     |          |                           |                  | world-readable permissions...                                |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2013-0337                         |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+
| openssl          | CVE-2021-3711       | HIGH     | 1.1.1d-0+deb10u6          | 1.1.1d-0+deb10u7 | openssl: SM2 Decryption                                      |
|                  |                     |          |                           |                  | Buffer Overflow                                              |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-3711                         |
+                  +---------------------+----------+                           +                  +--------------------------------------------------------------+
|                  | CVE-2021-3712       | MEDIUM   |                           |                  | openssl: Read buffer overruns                                |
|                  |                     |          |                           |                  | processing ASN.1 strings                                     |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-3712                         |
+                  +---------------------+----------+                           +------------------+--------------------------------------------------------------+
|                  | CVE-2007-6755       | LOW      |                           |                  | Dual_EC_DRBG: weak pseudo                                    |
|                  |                     |          |                           |                  | random number generator                                      |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2007-6755                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2010-0928       |          |                           |                  | openssl: RSA authentication weakness                         |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2010-0928                         |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| passwd           | CVE-2007-5686       |          | 1:4.5-1.1                 |                  | initscripts in rPath Linux 1                                 |
|                  |                     |          |                           |                  | sets insecure permissions for                                |
|                  |                     |          |                           |                  | the /var/log/btmp file,...                                   |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2007-5686                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2013-4235       |          |                           |                  | shadow-utils: TOCTOU race                                    |
|                  |                     |          |                           |                  | conditions by copying and                                    |
|                  |                     |          |                           |                  | removing directory trees                                     |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2013-4235                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2018-7169       |          |                           |                  | shadow-utils: newgidmap                                      |
|                  |                     |          |                           |                  | allows unprivileged user to                                  |
|                  |                     |          |                           |                  | drop supplementary groups                                    |
|                  |                     |          |                           |                  | potentially allowing privilege...                            |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2018-7169                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-19882      |          |                           |                  | shadow-utils: local users can                                |
|                  |                     |          |                           |                  | obtain root access because setuid                            |
|                  |                     |          |                           |                  | programs are misconfigured...                                |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-19882                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | TEMP-0628843-DBAD28 |          |                           |                  | -->security-tracker.debian.org/tracker/TEMP-0628843-DBAD28   |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| perl-base        | CVE-2011-4116       |          | 5.28.1-6+deb10u1          |                  | perl: File::Temp insecure                                    |
|                  |                     |          |                           |                  | temporary file handling                                      |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2011-4116                         |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| sysvinit-utils   | TEMP-0517018-A83CE6 |          | 2.93-8                    |                  | -->security-tracker.debian.org/tracker/TEMP-0517018-A83CE6   |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| tar              | CVE-2005-2541       |          | 1.30+dfsg-6               |                  | tar: does not properly warn the user                         |
|                  |                     |          |                           |                  | when extracting setuid or setgid...                          |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2005-2541                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2019-9923       |          |                           |                  | tar: null-pointer dereference                                |
|                  |                     |          |                           |                  | in pax_decode_header in sparse.c                             |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2019-9923                         |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | CVE-2021-20193      |          |                           |                  | tar: Memory leak in                                          |
|                  |                     |          |                           |                  | read_header() in list.c                                      |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-20193                        |
+                  +---------------------+          +                           +------------------+--------------------------------------------------------------+
|                  | TEMP-0290435-0B57B5 |          |                           |                  | -->security-tracker.debian.org/tracker/TEMP-0290435-0B57B5   |
+------------------+---------------------+          +---------------------------+------------------+--------------------------------------------------------------+
| util-linux       | CVE-2021-37600      |          | 2.33.1-0.1                |                  | util-linux: integer overflow                                 |
|                  |                     |          |                           |                  | can lead to buffer overflow                                  |
|                  |                     |          |                           |                  | in get_sem_elements() in                                     |
|                  |                     |          |                           |                  | sys-utils/ipcutils.c...                                      |
|                  |                     |          |                           |                  | -->avd.aquasec.com/nvd/cve-2021-37600                        |
+------------------+---------------------+----------+---------------------------+------------------+--------------------------------------------------------------+

```

### sysnet4admin/chk-info

```bash
[root@m-k8s ~]# trivy image sysnet4admin/chk-info
2021-08-28T07:47:35.046+0900    INFO    Detected OS: debian
2021-08-28T07:47:35.051+0900    INFO    Detecting Debian vulnerabilities...
2021-08-28T07:47:35.161+0900    INFO    Number of language-specific files: 1

sysnet4admin/chk-info (debian 10.9)
===================================
Total: 209 (UNKNOWN: 1, LOW: 129, MEDIUM: 22, HIGH: 40, CRITICAL: 17)

+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
|     LIBRARY      |  VULNERABILITY ID   | SEVERITY |     INSTALLED VERSION     |     FIXED VERSION     |                            TITLE                             |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| apt              | CVE-2011-3374       | LOW      | 1.8.2.3                   |                       | It was found that apt-key in apt,                            |
|                  |                     |          |                           |                       | all versions, do not correctly...                            |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2011-3374                         |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| bash             | CVE-2019-18276      |          | 5.0-4                     |                       | bash: when effective UID is not                              |
|                  |                     |          |                           |                       | equal to its real UID the...                                 |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-18276                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | TEMP-0841856-B18BAF |          |                           |                       | -->security-tracker.debian.org/tracker/TEMP-0841856-B18BAF   |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| bsdutils         | CVE-2021-37600      |          | 2.33.1-0.1                |                       | util-linux: integer overflow                                 |
|                  |                     |          |                           |                       | can lead to buffer overflow                                  |
|                  |                     |          |                           |                       | in get_sem_elements() in                                     |
|                  |                     |          |                           |                       | sys-utils/ipcutils.c...                                      |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-37600                        |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| coreutils        | CVE-2016-2781       |          | 8.30-3                    |                       | coreutils: Non-privileged                                    |
|                  |                     |          |                           |                       | session can escape to the                                    |
|                  |                     |          |                           |                       | parent session in chroot                                     |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2016-2781                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2017-18018      |          |                           |                       | coreutils: race condition                                    |
|                  |                     |          |                           |                       | vulnerability in chown and chgrp                             |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2017-18018                        |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| curl             | CVE-2021-22924      | HIGH     | 7.64.0-4+deb10u2          |                       | curl: Bad connection reuse                                   |
|                  |                     |          |                           |                       | due to flawed path name checks                               |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-22924                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-22898      | LOW      |                           |                       | curl: TELNET stack                                           |
|                  |                     |          |                           |                       | contents disclosure                                          |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-22898                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-22922      |          |                           |                       | curl: Content not matching hash                              |
|                  |                     |          |                           |                       | in Metalink is not being discarded                           |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-22922                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-22923      |          |                           |                       | curl: Metalink download                                      |
|                  |                     |          |                           |                       | sends credentials                                            |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-22923                        |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| fdisk            | CVE-2021-37600      |          | 2.33.1-0.1                |                       | util-linux: integer overflow                                 |
|                  |                     |          |                           |                       | can lead to buffer overflow                                  |
|                  |                     |          |                           |                       | in get_sem_elements() in                                     |
|                  |                     |          |                           |                       | sys-utils/ipcutils.c...                                      |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-37600                        |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| gcc-8-base       | CVE-2018-12886      | HIGH     | 8.3.0-6                   |                       | gcc: spilling of stack                                       |
|                  |                     |          |                           |                       | protection address in cfgexpand.c                            |
|                  |                     |          |                           |                       | and function.c leads to...                                   |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-12886                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-15847      |          |                           |                       | gcc: POWER9 "DARN" RNG intrinsic                             |
|                  |                     |          |                           |                       | produces repeated output                                     |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-15847                        |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| gpgv             | CVE-2019-14855      | LOW      | 2.2.12-1+deb10u1          |                       | gnupg2: OpenPGP Key Certification                            |
|                  |                     |          |                           |                       | Forgeries with SHA-1                                         |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-14855                        |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| libapt-pkg5.0    | CVE-2011-3374       |          | 1.8.2.3                   |                       | It was found that apt-key in apt,                            |
|                  |                     |          |                           |                       | all versions, do not correctly...                            |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2011-3374                         |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| libblkid1        | CVE-2021-37600      |          | 2.33.1-0.1                |                       | util-linux: integer overflow                                 |
|                  |                     |          |                           |                       | can lead to buffer overflow                                  |
|                  |                     |          |                           |                       | in get_sem_elements() in                                     |
|                  |                     |          |                           |                       | sys-utils/ipcutils.c...                                      |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-37600                        |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| libc-bin         | CVE-2021-33574      | CRITICAL | 2.28-10                   |                       | glibc: mq_notify does                                        |
|                  |                     |          |                           |                       | not handle separately                                        |
|                  |                     |          |                           |                       | allocated thread attributes                                  |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-33574                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-35942      |          |                           |                       | glibc: Arbitrary read in wordexp()                           |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-35942                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2020-1751       | HIGH     |                           |                       | glibc: array overflow in                                     |
|                  |                     |          |                           |                       | backtrace functions for powerpc                              |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-1751                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2020-1752       |          |                           |                       | glibc: use-after-free in glob()                              |
|                  |                     |          |                           |                       | function when expanding ~user                                |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-1752                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-3326       |          |                           |                       | glibc: Assertion failure in                                  |
|                  |                     |          |                           |                       | ISO-2022-JP-3 gconv module                                   |
|                  |                     |          |                           |                       | related to combining characters                              |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-3326                         |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-25013      | MEDIUM   |                           |                       | glibc: buffer over-read in                                   |
|                  |                     |          |                           |                       | iconv when processing invalid                                |
|                  |                     |          |                           |                       | multi-byte input sequences in...                             |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-25013                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2020-10029      |          |                           |                       | glibc: stack corruption                                      |
|                  |                     |          |                           |                       | from crafted input in cosl,                                  |
|                  |                     |          |                           |                       | sinl, sincosl, and tanl...                                   |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-10029                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2020-27618      |          |                           |                       | glibc: iconv when processing                                 |
|                  |                     |          |                           |                       | invalid multi-byte input                                     |
|                  |                     |          |                           |                       | sequences fails to advance the...                            |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-27618                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2010-4051       | LOW      |                           |                       | CVE-2010-4052 glibc: De-recursivise                          |
|                  |                     |          |                           |                       | regular expression engine                                    |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2010-4051                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2010-4052       |          |                           |                       | CVE-2010-4051 CVE-2010-4052                                  |
|                  |                     |          |                           |                       | glibc: De-recursivise                                        |
|                  |                     |          |                           |                       | regular expression engine                                    |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2010-4052                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2010-4756       |          |                           |                       | glibc: glob implementation                                   |
|                  |                     |          |                           |                       | can cause excessive CPU and                                  |
|                  |                     |          |                           |                       | memory consumption due to...                                 |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2010-4756                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2016-10228      |          |                           |                       | glibc: iconv program can hang                                |
|                  |                     |          |                           |                       | when invoked with the -c option                              |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2016-10228                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2018-20796      |          |                           |                       | glibc: uncontrolled recursion in                             |
|                  |                     |          |                           |                       | function check_dst_limits_calc_pos_1                         |
|                  |                     |          |                           |                       | in posix/regexec.c                                           |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-20796                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-1010022    |          |                           |                       | glibc: stack guard protection bypass                         |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-1010022                      |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-1010023    |          |                           |                       | glibc: running ldd on malicious ELF                          |
|                  |                     |          |                           |                       | leads to code execution because of...                        |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-1010023                      |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-1010024    |          |                           |                       | glibc: ASLR bypass using                                     |
|                  |                     |          |                           |                       | cache of thread stack and heap                               |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-1010024                      |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-1010025    |          |                           |                       | glibc: information disclosure of heap                        |
|                  |                     |          |                           |                       | addresses of pthread_created thread                          |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-1010025                      |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-19126      |          |                           |                       | glibc: LD_PREFER_MAP_32BIT_EXEC                              |
|                  |                     |          |                           |                       | not ignored in setuid binaries                               |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-19126                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-9192       |          |                           |                       | glibc: uncontrolled recursion in                             |
|                  |                     |          |                           |                       | function check_dst_limits_calc_pos_1                         |
|                  |                     |          |                           |                       | in posix/regexec.c                                           |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-9192                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2020-6096       |          |                           |                       | glibc: signed comparison                                     |
|                  |                     |          |                           |                       | vulnerability in the                                         |
|                  |                     |          |                           |                       | ARMv7 memcpy function                                        |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-6096                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-27645      |          |                           |                       | glibc: Use-after-free in                                     |
|                  |                     |          |                           |                       | addgetnetgrentX function                                     |
|                  |                     |          |                           |                       | in netgroupcache.c                                           |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-27645                        |
+------------------+---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
| libc6            | CVE-2021-33574      | CRITICAL |                           |                       | glibc: mq_notify does                                        |
|                  |                     |          |                           |                       | not handle separately                                        |
|                  |                     |          |                           |                       | allocated thread attributes                                  |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-33574                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-35942      |          |                           |                       | glibc: Arbitrary read in wordexp()                           |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-35942                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2020-1751       | HIGH     |                           |                       | glibc: array overflow in                                     |
|                  |                     |          |                           |                       | backtrace functions for powerpc                              |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-1751                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2020-1752       |          |                           |                       | glibc: use-after-free in glob()                              |
|                  |                     |          |                           |                       | function when expanding ~user                                |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-1752                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-3326       |          |                           |                       | glibc: Assertion failure in                                  |
|                  |                     |          |                           |                       | ISO-2022-JP-3 gconv module                                   |
|                  |                     |          |                           |                       | related to combining characters                              |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-3326                         |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-25013      | MEDIUM   |                           |                       | glibc: buffer over-read in                                   |
|                  |                     |          |                           |                       | iconv when processing invalid                                |
|                  |                     |          |                           |                       | multi-byte input sequences in...                             |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-25013                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2020-10029      |          |                           |                       | glibc: stack corruption                                      |
|                  |                     |          |                           |                       | from crafted input in cosl,                                  |
|                  |                     |          |                           |                       | sinl, sincosl, and tanl...                                   |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-10029                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2020-27618      |          |                           |                       | glibc: iconv when processing                                 |
|                  |                     |          |                           |                       | invalid multi-byte input                                     |
|                  |                     |          |                           |                       | sequences fails to advance the...                            |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-27618                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2010-4051       | LOW      |                           |                       | CVE-2010-4052 glibc: De-recursivise                          |
|                  |                     |          |                           |                       | regular expression engine                                    |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2010-4051                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2010-4052       |          |                           |                       | CVE-2010-4051 CVE-2010-4052                                  |
|                  |                     |          |                           |                       | glibc: De-recursivise                                        |
|                  |                     |          |                           |                       | regular expression engine                                    |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2010-4052                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2010-4756       |          |                           |                       | glibc: glob implementation                                   |
|                  |                     |          |                           |                       | can cause excessive CPU and                                  |
|                  |                     |          |                           |                       | memory consumption due to...                                 |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2010-4756                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2016-10228      |          |                           |                       | glibc: iconv program can hang                                |
|                  |                     |          |                           |                       | when invoked with the -c option                              |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2016-10228                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2018-20796      |          |                           |                       | glibc: uncontrolled recursion in                             |
|                  |                     |          |                           |                       | function check_dst_limits_calc_pos_1                         |
|                  |                     |          |                           |                       | in posix/regexec.c                                           |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-20796                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-1010022    |          |                           |                       | glibc: stack guard protection bypass                         |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-1010022                      |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-1010023    |          |                           |                       | glibc: running ldd on malicious ELF                          |
|                  |                     |          |                           |                       | leads to code execution because of...                        |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-1010023                      |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-1010024    |          |                           |                       | glibc: ASLR bypass using                                     |
|                  |                     |          |                           |                       | cache of thread stack and heap                               |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-1010024                      |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-1010025    |          |                           |                       | glibc: information disclosure of heap                        |
|                  |                     |          |                           |                       | addresses of pthread_created thread                          |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-1010025                      |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-19126      |          |                           |                       | glibc: LD_PREFER_MAP_32BIT_EXEC                              |
|                  |                     |          |                           |                       | not ignored in setuid binaries                               |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-19126                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-9192       |          |                           |                       | glibc: uncontrolled recursion in                             |
|                  |                     |          |                           |                       | function check_dst_limits_calc_pos_1                         |
|                  |                     |          |                           |                       | in posix/regexec.c                                           |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-9192                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2020-6096       |          |                           |                       | glibc: signed comparison                                     |
|                  |                     |          |                           |                       | vulnerability in the                                         |
|                  |                     |          |                           |                       | ARMv7 memcpy function                                        |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-6096                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-27645      |          |                           |                       | glibc: Use-after-free in                                     |
|                  |                     |          |                           |                       | addgetnetgrentX function                                     |
|                  |                     |          |                           |                       | in netgroupcache.c                                           |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-27645                        |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| libcurl4         | CVE-2021-22924      | HIGH     | 7.64.0-4+deb10u2          |                       | curl: Bad connection reuse                                   |
|                  |                     |          |                           |                       | due to flawed path name checks                               |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-22924                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-22898      | LOW      |                           |                       | curl: TELNET stack                                           |
|                  |                     |          |                           |                       | contents disclosure                                          |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-22898                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-22922      |          |                           |                       | curl: Content not matching hash                              |
|                  |                     |          |                           |                       | in Metalink is not being discarded                           |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-22922                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-22923      |          |                           |                       | curl: Metalink download                                      |
|                  |                     |          |                           |                       | sends credentials                                            |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-22923                        |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| libexpat1        | CVE-2013-0340       |          | 2.2.6-2+deb10u1           |                       | expat: internal entity expansion                             |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2013-0340                         |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| libfdisk1        | CVE-2021-37600      |          | 2.33.1-0.1                |                       | util-linux: integer overflow                                 |
|                  |                     |          |                           |                       | can lead to buffer overflow                                  |
|                  |                     |          |                           |                       | in get_sem_elements() in                                     |
|                  |                     |          |                           |                       | sys-utils/ipcutils.c...                                      |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-37600                        |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| libgcc1          | CVE-2018-12886      | HIGH     | 8.3.0-6                   |                       | gcc: spilling of stack                                       |
|                  |                     |          |                           |                       | protection address in cfgexpand.c                            |
|                  |                     |          |                           |                       | and function.c leads to...                                   |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-12886                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-15847      |          |                           |                       | gcc: POWER9 "DARN" RNG intrinsic                             |
|                  |                     |          |                           |                       | produces repeated output                                     |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-15847                        |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| libgcrypt20      | CVE-2021-33560      |          | 1.8.4-5                   | 1.8.4-5+deb10u1       | libgcrypt: mishandles ElGamal                                |
|                  |                     |          |                           |                       | encryption because it lacks                                  |
|                  |                     |          |                           |                       | exponent blinding to address a...                            |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-33560                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-13627      | MEDIUM   |                           |                       | libgcrypt: ECDSA timing attack                               |
|                  |                     |          |                           |                       | allowing private key leak                                    |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-13627                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2018-6829       | LOW      |                           |                       | libgcrypt: ElGamal implementation                            |
|                  |                     |          |                           |                       | doesn't have semantic security due                           |
|                  |                     |          |                           |                       | to incorrectly encoded plaintexts...                         |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-6829                         |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| libgd3           | CVE-2017-6363       | HIGH     | 2.2.5-5.2                 |                       | ** DISPUTED ** In the                                        |
|                  |                     |          |                           |                       | GD Graphics Library (aka                                     |
|                  |                     |          |                           |                       | LibGD) through 2.2.5,...                                     |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2017-6363                         |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-38115      | MEDIUM   |                           |                       | read_header_tga in gd_tga.c                                  |
|                  |                     |          |                           |                       | in the GD Graphics Library                                   |
|                  |                     |          |                           |                       | (aka LibGD) through 2.3.2...                                 |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-38115                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2018-14553      | LOW      |                           |                       | gd: NULL pointer                                             |
|                  |                     |          |                           |                       | dereference in gdImageClone                                  |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-14553                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-40145      | UNKNOWN  |                           |                       | ** DISPUTED ** gdImageGd2Ptr                                 |
|                  |                     |          |                           |                       | in gd_gd2.c in the GD                                        |
|                  |                     |          |                           |                       | Graphics Library (aka...                                     |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-40145                        |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| libgnutls30      | CVE-2021-20231      | CRITICAL | 3.6.7-4+deb10u6           | 3.6.7-4+deb10u7       | gnutls: Use after free in                                    |
|                  |                     |          |                           |                       | client key_share extension                                   |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-20231                        |
+                  +---------------------+          +                           +                       +--------------------------------------------------------------+
|                  | CVE-2021-20232      |          |                           |                       | gnutls: Use after free                                       |
|                  |                     |          |                           |                       | in client_send_params in                                     |
|                  |                     |          |                           |                       | lib/ext/pre_shared_key.c                                     |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-20232                        |
+                  +---------------------+----------+                           +                       +--------------------------------------------------------------+
|                  | CVE-2020-24659      | HIGH     |                           |                       | gnutls: Heap buffer                                          |
|                  |                     |          |                           |                       | overflow in handshake with                                   |
|                  |                     |          |                           |                       | no_renegotiation alert sent                                  |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-24659                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2011-3389       | LOW      |                           |                       | HTTPS: block-wise chosen-plaintext                           |
|                  |                     |          |                           |                       | attack against SSL/TLS (BEAST)                               |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2011-3389                         |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| libgssapi-krb5-2 | CVE-2021-36222      | HIGH     | 1.17-3+deb10u1            | 1.17-3+deb10u2        | krb5: sending a request containing                           |
|                  |                     |          |                           |                       | a PA-ENCRYPTED-CHALLENGE padata                              |
|                  |                     |          |                           |                       | element without using FAST...                                |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-36222                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-37750      | MEDIUM   |                           |                       | krb5: NULL pointer dereference                               |
|                  |                     |          |                           |                       | in process_tgs_req() in                                      |
|                  |                     |          |                           |                       | kdc/do_tgs_req.c via a FAST inner...                         |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-37750                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2004-0971       | LOW      |                           |                       | security flaw                                                |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2004-0971                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2018-5709       |          |                           |                       | krb5: integer overflow                                       |
|                  |                     |          |                           |                       | in dbentry->n_key_data                                       |
|                  |                     |          |                           |                       | in kadmin/dbutil/dump.c                                      |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-5709                         |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| libhogweed4      | CVE-2021-20305      | HIGH     | 3.4.1-1                   | 3.4.1-1+deb10u1       | nettle: Out of bounds memory                                 |
|                  |                     |          |                           |                       | access in signature verification                             |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-20305                        |
+                  +---------------------+          +                           +                       +--------------------------------------------------------------+
|                  | CVE-2021-3580       |          |                           |                       | nettle: Remote crash                                         |
|                  |                     |          |                           |                       | in RSA decryption via                                        |
|                  |                     |          |                           |                       | manipulated ciphertext                                       |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-3580                         |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| libicu63         | CVE-2021-30535      |          | 63.1-6+deb10u1            |                       | Double free in ICU in Google Chrome                          |
|                  |                     |          |                           |                       | prior to 91.0.4472.77 allowed a...                           |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-30535                        |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| libidn2-0        | CVE-2019-12290      |          | 2.0.5-1+deb10u1           |                       | GNU libidn2 before 2.2.0                                     |
|                  |                     |          |                           |                       | fails to perform the roundtrip                               |
|                  |                     |          |                           |                       | checks specified in...                                       |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-12290                        |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| libjbig0         | CVE-2017-9937       | LOW      | 2.1-3.1                   |                       | libtiff: memory malloc failure                               |
|                  |                     |          |                           |                       | in tif_jbig.c could cause DOS.                               |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2017-9937                         |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| libjpeg62-turbo  | CVE-2017-15232      |          | 1:1.5.2-2+deb10u1         |                       | libjpeg-turbo: NULL                                          |
|                  |                     |          |                           |                       | pointer dereference in                                       |
|                  |                     |          |                           |                       | jdpostct.c and jquant1.c                                     |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2017-15232                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2018-11813      |          |                           |                       | libjpeg: "cjpeg" utility                                     |
|                  |                     |          |                           |                       | large loop because read_pixel                                |
|                  |                     |          |                           |                       | in rdtarga.c mishandles EOF                                  |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-11813                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2020-17541      |          |                           |                       | libjpeg-turbo: Stack-based buffer                            |
|                  |                     |          |                           |                       | overflow in the "transform" component                        |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-17541                        |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| libk5crypto3     | CVE-2021-36222      | HIGH     | 1.17-3+deb10u1            | 1.17-3+deb10u2        | krb5: sending a request containing                           |
|                  |                     |          |                           |                       | a PA-ENCRYPTED-CHALLENGE padata                              |
|                  |                     |          |                           |                       | element without using FAST...                                |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-36222                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-37750      | MEDIUM   |                           |                       | krb5: NULL pointer dereference                               |
|                  |                     |          |                           |                       | in process_tgs_req() in                                      |
|                  |                     |          |                           |                       | kdc/do_tgs_req.c via a FAST inner...                         |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-37750                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2004-0971       | LOW      |                           |                       | security flaw                                                |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2004-0971                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2018-5709       |          |                           |                       | krb5: integer overflow                                       |
|                  |                     |          |                           |                       | in dbentry->n_key_data                                       |
|                  |                     |          |                           |                       | in kadmin/dbutil/dump.c                                      |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-5709                         |
+------------------+---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
| libkrb5-3        | CVE-2021-36222      | HIGH     |                           | 1.17-3+deb10u2        | krb5: sending a request containing                           |
|                  |                     |          |                           |                       | a PA-ENCRYPTED-CHALLENGE padata                              |
|                  |                     |          |                           |                       | element without using FAST...                                |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-36222                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-37750      | MEDIUM   |                           |                       | krb5: NULL pointer dereference                               |
|                  |                     |          |                           |                       | in process_tgs_req() in                                      |
|                  |                     |          |                           |                       | kdc/do_tgs_req.c via a FAST inner...                         |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-37750                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2004-0971       | LOW      |                           |                       | security flaw                                                |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2004-0971                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2018-5709       |          |                           |                       | krb5: integer overflow                                       |
|                  |                     |          |                           |                       | in dbentry->n_key_data                                       |
|                  |                     |          |                           |                       | in kadmin/dbutil/dump.c                                      |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-5709                         |
+------------------+---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
| libkrb5support0  | CVE-2021-36222      | HIGH     |                           | 1.17-3+deb10u2        | krb5: sending a request containing                           |
|                  |                     |          |                           |                       | a PA-ENCRYPTED-CHALLENGE padata                              |
|                  |                     |          |                           |                       | element without using FAST...                                |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-36222                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-37750      | MEDIUM   |                           |                       | krb5: NULL pointer dereference                               |
|                  |                     |          |                           |                       | in process_tgs_req() in                                      |
|                  |                     |          |                           |                       | kdc/do_tgs_req.c via a FAST inner...                         |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-37750                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2004-0971       | LOW      |                           |                       | security flaw                                                |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2004-0971                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2018-5709       |          |                           |                       | krb5: integer overflow                                       |
|                  |                     |          |                           |                       | in dbentry->n_key_data                                       |
|                  |                     |          |                           |                       | in kadmin/dbutil/dump.c                                      |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-5709                         |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| libldap-2.4-2    | CVE-2015-3276       |          | 2.4.47+dfsg-3+deb10u6     |                       | openldap: incorrect multi-keyword                            |
|                  |                     |          |                           |                       | mode cipherstring parsing                                    |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2015-3276                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2017-14159      |          |                           |                       | openldap: Privilege escalation                               |
|                  |                     |          |                           |                       | via PID file manipulation                                    |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2017-14159                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2017-17740      |          |                           |                       | openldap:                                                    |
|                  |                     |          |                           |                       | contrib/slapd-modules/nops/nops.c                            |
|                  |                     |          |                           |                       | attempts to free stack buffer                                |
|                  |                     |          |                           |                       | allowing remote attackers to cause...                        |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2017-17740                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2020-15719      |          |                           |                       | openldap: Certificate                                        |
|                  |                     |          |                           |                       | validation incorrectly                                       |
|                  |                     |          |                           |                       | matches name against CN-ID                                   |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-15719                        |
+------------------+---------------------+          +                           +-----------------------+--------------------------------------------------------------+
| libldap-common   | CVE-2015-3276       |          |                           |                       | openldap: incorrect multi-keyword                            |
|                  |                     |          |                           |                       | mode cipherstring parsing                                    |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2015-3276                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2017-14159      |          |                           |                       | openldap: Privilege escalation                               |
|                  |                     |          |                           |                       | via PID file manipulation                                    |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2017-14159                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2017-17740      |          |                           |                       | openldap:                                                    |
|                  |                     |          |                           |                       | contrib/slapd-modules/nops/nops.c                            |
|                  |                     |          |                           |                       | attempts to free stack buffer                                |
|                  |                     |          |                           |                       | allowing remote attackers to cause...                        |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2017-17740                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2020-15719      |          |                           |                       | openldap: Certificate                                        |
|                  |                     |          |                           |                       | validation incorrectly                                       |
|                  |                     |          |                           |                       | matches name against CN-ID                                   |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-15719                        |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| liblz4-1         | CVE-2021-3520       | CRITICAL | 1.8.3-1                   | 1.8.3-1+deb10u1       | lz4: memory corruption                                       |
|                  |                     |          |                           |                       | due to an integer overflow                                   |
|                  |                     |          |                           |                       | bug caused by memmove...                                     |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-3520                         |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-17543      | LOW      |                           |                       | lz4: heap-based buffer                                       |
|                  |                     |          |                           |                       | overflow in LZ4_write32                                      |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-17543                        |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| libmount1        | CVE-2021-37600      |          | 2.33.1-0.1                |                       | util-linux: integer overflow                                 |
|                  |                     |          |                           |                       | can lead to buffer overflow                                  |
|                  |                     |          |                           |                       | in get_sem_elements() in                                     |
|                  |                     |          |                           |                       | sys-utils/ipcutils.c...                                      |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-37600                        |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| libnettle6       | CVE-2021-20305      | HIGH     | 3.4.1-1                   | 3.4.1-1+deb10u1       | nettle: Out of bounds memory                                 |
|                  |                     |          |                           |                       | access in signature verification                             |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-20305                        |
+                  +---------------------+          +                           +                       +--------------------------------------------------------------+
|                  | CVE-2021-3580       |          |                           |                       | nettle: Remote crash                                         |
|                  |                     |          |                           |                       | in RSA decryption via                                        |
|                  |                     |          |                           |                       | manipulated ciphertext                                       |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-3580                         |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| libnghttp2-14    | TEMP-0000000-A4EF31 | LOW      | 1.36.0-2+deb10u1          |                       | -->security-tracker.debian.org/tracker/TEMP-0000000-A4EF31   |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| libpcre3         | CVE-2020-14155      | MEDIUM   | 2:8.39-12                 |                       | pcre: integer overflow in libpcre                            |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-14155                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2017-11164      | LOW      |                           |                       | pcre: OP_KETRMAX feature in the                              |
|                  |                     |          |                           |                       | match function in pcre_exec.c                                |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2017-11164                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2017-16231      |          |                           |                       | pcre: self-recursive call                                    |
|                  |                     |          |                           |                       | in match() in pcre_exec.c                                    |
|                  |                     |          |                           |                       | leads to denial of service...                                |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2017-16231                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2017-7245       |          |                           |                       | pcre: stack-based buffer overflow                            |
|                  |                     |          |                           |                       | write in pcre32_copy_substring                               |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2017-7245                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2017-7246       |          |                           |                       | pcre: stack-based buffer overflow                            |
|                  |                     |          |                           |                       | write in pcre32_copy_substring                               |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2017-7246                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-20838      |          |                           |                       | pcre: buffer over-read in                                    |
|                  |                     |          |                           |                       | JIT when UTF is disabled                                     |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-20838                        |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| libpng16-16      | CVE-2018-14048      |          | 1.6.36-6                  |                       | libpng: Segmentation fault in                                |
|                  |                     |          |                           |                       | png.c:png_free_data function                                 |
|                  |                     |          |                           |                       | causing denial of service                                    |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-14048                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2018-14550      |          |                           |                       | libpng: Stack-based buffer overflow in                       |
|                  |                     |          |                           |                       | contrib/pngminus/pnm2png.c:get_token()                       |
|                  |                     |          |                           |                       | potentially leading to                                       |
|                  |                     |          |                           |                       | arbitrary code execution...                                  |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-14550                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-6129       |          |                           |                       | libpng: memory leak of                                       |
|                  |                     |          |                           |                       | png_info struct in pngcp.c                                   |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-6129                         |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| libseccomp2      | CVE-2019-9893       |          | 2.3.3-4                   |                       | libseccomp: incorrect generation                             |
|                  |                     |          |                           |                       | of syscall filters in libseccomp                             |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-9893                         |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| libsepol1        | CVE-2021-36084      |          | 2.8-1                     |                       | libsepol: use-after-free in                                  |
|                  |                     |          |                           |                       | __cil_verify_classperms()                                    |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-36084                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-36085      |          |                           |                       | libsepol: use-after-free in                                  |
|                  |                     |          |                           |                       | __cil_verify_classperms()                                    |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-36085                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-36086      |          |                           |                       | libsepol: use-after-free in                                  |
|                  |                     |          |                           |                       | cil_reset_classpermission()                                  |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-36086                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-36087      |          |                           |                       | libsepol: heap-based buffer                                  |
|                  |                     |          |                           |                       | overflow in ebitmap_match_any()                              |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-36087                        |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| libsmartcols1    | CVE-2021-37600      |          | 2.33.1-0.1                |                       | util-linux: integer overflow                                 |
|                  |                     |          |                           |                       | can lead to buffer overflow                                  |
|                  |                     |          |                           |                       | in get_sem_elements() in                                     |
|                  |                     |          |                           |                       | sys-utils/ipcutils.c...                                      |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-37600                        |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| libssh2-1        | CVE-2019-13115      | HIGH     | 1.8.0-2.1                 |                       | libssh2: integer overflow in                                 |
|                  |                     |          |                           |                       | kex_method_diffie_hellman_group_exchange_sha256_key_exchange |
|                  |                     |          |                           |                       | in kex.c leads to out-of-bounds write                        |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-13115                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-17498      | LOW      |                           |                       | libssh2: integer overflow in                                 |
|                  |                     |          |                           |                       | SSH_MSG_DISCONNECT logic in packet.c                         |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-17498                        |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| libssl1.1        | CVE-2021-3711       | HIGH     | 1.1.1d-0+deb10u6          | 1.1.1d-0+deb10u7      | openssl: SM2 Decryption                                      |
|                  |                     |          |                           |                       | Buffer Overflow                                              |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-3711                         |
+                  +---------------------+----------+                           +                       +--------------------------------------------------------------+
|                  | CVE-2021-3712       | MEDIUM   |                           |                       | openssl: Read buffer overruns                                |
|                  |                     |          |                           |                       | processing ASN.1 strings                                     |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-3712                         |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2007-6755       | LOW      |                           |                       | Dual_EC_DRBG: weak pseudo                                    |
|                  |                     |          |                           |                       | random number generator                                      |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2007-6755                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2010-0928       |          |                           |                       | openssl: RSA authentication weakness                         |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2010-0928                         |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| libstdc++6       | CVE-2018-12886      | HIGH     | 8.3.0-6                   |                       | gcc: spilling of stack                                       |
|                  |                     |          |                           |                       | protection address in cfgexpand.c                            |
|                  |                     |          |                           |                       | and function.c leads to...                                   |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-12886                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-15847      |          |                           |                       | gcc: POWER9 "DARN" RNG intrinsic                             |
|                  |                     |          |                           |                       | produces repeated output                                     |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-15847                        |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| libsystemd0      | CVE-2019-3843       |          | 241-7~deb10u7             |                       | systemd: services with DynamicUser                           |
|                  |                     |          |                           |                       | can create SUID/SGID binaries                                |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-3843                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-3844       |          |                           |                       | systemd: services with DynamicUser                           |
|                  |                     |          |                           |                       | can get new privileges and                                   |
|                  |                     |          |                           |                       | create SGID binaries...                                      |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-3844                         |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-33910      | MEDIUM   |                           | 241-7~deb10u8         | systemd: uncontrolled                                        |
|                  |                     |          |                           |                       | allocation on the stack in                                   |
|                  |                     |          |                           |                       | function unit_name_path_escape                               |
|                  |                     |          |                           |                       | leads to crash...                                            |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-33910                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2013-4392       | LOW      |                           |                       | systemd: TOCTOU race condition                               |
|                  |                     |          |                           |                       | when updating file permissions                               |
|                  |                     |          |                           |                       | and SELinux security contexts...                             |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2013-4392                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-20386      |          |                           |                       | systemd: memory leak in button_open()                        |
|                  |                     |          |                           |                       | in login/logind-button.c when                                |
|                  |                     |          |                           |                       | udev events are received...                                  |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-20386                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2020-13529      |          |                           |                       | systemd: DHCP FORCERENEW                                     |
|                  |                     |          |                           |                       | authentication not implemented                               |
|                  |                     |          |                           |                       | can cause a system running the...                            |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-13529                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2020-13776      |          |                           |                       | systemd: Mishandles numerical                                |
|                  |                     |          |                           |                       | usernames beginning with decimal                             |
|                  |                     |          |                           |                       | digits or 0x followed by...                                  |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-13776                        |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| libtasn1-6       | CVE-2018-1000654    |          | 4.13-3                    |                       | libtasn1: Infinite loop in                                   |
|                  |                     |          |                           |                       | _asn1_expand_object_id(ptree)                                |
|                  |                     |          |                           |                       | leads to memory exhaustion                                   |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-1000654                      |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| libtiff5         | CVE-2014-8130       |          | 4.1.0+git191117-2~deb10u2 |                       | libtiff: divide by zero                                      |
|                  |                     |          |                           |                       | in the tiffdither tool                                       |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2014-8130                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2017-16232      |          |                           |                       | libtiff: Memory leaks in                                     |
|                  |                     |          |                           |                       | tif_open.c, tif_lzw.c, and tif_aux.c                         |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2017-16232                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2017-17973      |          |                           |                       | libtiff: heap-based use after                                |
|                  |                     |          |                           |                       | free in tiff2pdf.c:t2p_writeproc                             |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2017-17973                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2017-5563       |          |                           |                       | libtiff: Heap-buffer overflow                                |
|                  |                     |          |                           |                       | in LZWEncode tif_lzw.c                                       |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2017-5563                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2017-9117       |          |                           |                       | libtiff: Heap-based buffer                                   |
|                  |                     |          |                           |                       | over-read in bmp2tiff                                        |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2017-9117                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2018-10126      |          |                           |                       | libtiff: NULL pointer dereference                            |
|                  |                     |          |                           |                       | in the jpeg_fdct_16x16                                       |
|                  |                     |          |                           |                       | function in jfdctint.c                                       |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-10126                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2020-35521      |          |                           |                       | libtiff: Memory allocation                                   |
|                  |                     |          |                           |                       | failure in tiff2rgba                                         |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-35521                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2020-35522      |          |                           |                       | libtiff: Memory allocation                                   |
|                  |                     |          |                           |                       | failure in tiff2rgba                                         |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-35522                        |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| libudev1         | CVE-2019-3843       | HIGH     | 241-7~deb10u7             |                       | systemd: services with DynamicUser                           |
|                  |                     |          |                           |                       | can create SUID/SGID binaries                                |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-3843                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-3844       |          |                           |                       | systemd: services with DynamicUser                           |
|                  |                     |          |                           |                       | can get new privileges and                                   |
|                  |                     |          |                           |                       | create SGID binaries...                                      |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-3844                         |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-33910      | MEDIUM   |                           | 241-7~deb10u8         | systemd: uncontrolled                                        |
|                  |                     |          |                           |                       | allocation on the stack in                                   |
|                  |                     |          |                           |                       | function unit_name_path_escape                               |
|                  |                     |          |                           |                       | leads to crash...                                            |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-33910                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2013-4392       | LOW      |                           |                       | systemd: TOCTOU race condition                               |
|                  |                     |          |                           |                       | when updating file permissions                               |
|                  |                     |          |                           |                       | and SELinux security contexts...                             |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2013-4392                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-20386      |          |                           |                       | systemd: memory leak in button_open()                        |
|                  |                     |          |                           |                       | in login/logind-button.c when                                |
|                  |                     |          |                           |                       | udev events are received...                                  |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-20386                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2020-13529      |          |                           |                       | systemd: DHCP FORCERENEW                                     |
|                  |                     |          |                           |                       | authentication not implemented                               |
|                  |                     |          |                           |                       | can cause a system running the...                            |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-13529                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2020-13776      |          |                           |                       | systemd: Mishandles numerical                                |
|                  |                     |          |                           |                       | usernames beginning with decimal                             |
|                  |                     |          |                           |                       | digits or 0x followed by...                                  |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-13776                        |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| libuuid1         | CVE-2021-37600      |          | 2.33.1-0.1                |                       | util-linux: integer overflow                                 |
|                  |                     |          |                           |                       | can lead to buffer overflow                                  |
|                  |                     |          |                           |                       | in get_sem_elements() in                                     |
|                  |                     |          |                           |                       | sys-utils/ipcutils.c...                                      |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-37600                        |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| libwebp6         | CVE-2018-25009      | CRITICAL | 0.6.1-2                   | 0.6.1-2+deb10u1       | libwebp: out-of-bounds read                                  |
|                  |                     |          |                           |                       | in WebPMuxCreateInternal                                     |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-25009                        |
+                  +---------------------+          +                           +                       +--------------------------------------------------------------+
|                  | CVE-2018-25010      |          |                           |                       | libwebp: out-of-bounds                                       |
|                  |                     |          |                           |                       | read in ApplyFilter()                                        |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-25010                        |
+                  +---------------------+          +                           +                       +--------------------------------------------------------------+
|                  | CVE-2018-25011      |          |                           |                       | libwebp: heap-based buffer                                   |
|                  |                     |          |                           |                       | overflow in PutLE16()                                        |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-25011                        |
+                  +---------------------+          +                           +                       +--------------------------------------------------------------+
|                  | CVE-2018-25012      |          |                           |                       | libwebp: out-of-bounds read                                  |
|                  |                     |          |                           |                       | in WebPMuxCreateInternal()                                   |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-25012                        |
+                  +---------------------+          +                           +                       +--------------------------------------------------------------+
|                  | CVE-2018-25013      |          |                           |                       | libwebp: out-of-bounds                                       |
|                  |                     |          |                           |                       | read in ShiftBytes()                                         |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-25013                        |
+                  +---------------------+          +                           +                       +--------------------------------------------------------------+
|                  | CVE-2018-25014      |          |                           |                       | libwebp: use of uninitialized                                |
|                  |                     |          |                           |                       | value in ReadSymbol()                                        |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-25014                        |
+                  +---------------------+          +                           +                       +--------------------------------------------------------------+
|                  | CVE-2020-36328      |          |                           |                       | libwebp: heap-based buffer overflow                          |
|                  |                     |          |                           |                       | in WebPDecode*Into functions                                 |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-36328                        |
+                  +---------------------+          +                           +                       +--------------------------------------------------------------+
|                  | CVE-2020-36329      |          |                           |                       | libwebp: use-after-free in                                   |
|                  |                     |          |                           |                       | EmitFancyRGB() in dec/io_dec.c                               |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-36329                        |
+                  +---------------------+          +                           +                       +--------------------------------------------------------------+
|                  | CVE-2020-36330      |          |                           |                       | libwebp: out-of-bounds read                                  |
|                  |                     |          |                           |                       | in ChunkVerifyAndAssign()                                    |
|                  |                     |          |                           |                       | in mux/muxread.c                                             |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-36330                        |
+                  +---------------------+          +                           +                       +--------------------------------------------------------------+
|                  | CVE-2020-36331      |          |                           |                       | libwebp: out-of-bounds                                       |
|                  |                     |          |                           |                       | read in ChunkAssignData()                                    |
|                  |                     |          |                           |                       | in mux/muxinternal.c                                         |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-36331                        |
+                  +---------------------+----------+                           +                       +--------------------------------------------------------------+
|                  | CVE-2020-36332      | HIGH     |                           |                       | libwebp: extreme memory                                      |
|                  |                     |          |                           |                       | allocation when reading a file                               |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-36332                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2016-9085       | LOW      |                           |                       | libwebp: Several integer overflows                           |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2016-9085                         |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| libxml2          | CVE-2017-16932      | HIGH     | 2.9.4+dfsg1-7+deb10u1     |                       | libxml2: Infinite recursion                                  |
|                  |                     |          |                           |                       | in parameter entities                                        |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2017-16932                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-3516       |          |                           | 2.9.4+dfsg1-7+deb10u2 | libxml2: Use-after-free in                                   |
|                  |                     |          |                           |                       | xmlEncodeEntitiesInternal()                                  |
|                  |                     |          |                           |                       | in entities.c                                                |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-3516                         |
+                  +---------------------+          +                           +                       +--------------------------------------------------------------+
|                  | CVE-2021-3517       |          |                           |                       | libxml2: Heap-based buffer overflow                          |
|                  |                     |          |                           |                       | in xmlEncodeEntitiesInternal()                               |
|                  |                     |          |                           |                       | in entities.c                                                |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-3517                         |
+                  +---------------------+          +                           +                       +--------------------------------------------------------------+
|                  | CVE-2021-3518       |          |                           |                       | libxml2: Use-after-free in                                   |
|                  |                     |          |                           |                       | xmlXIncludeDoProcess() in xinclude.c                         |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-3518                         |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2016-9318       | MEDIUM   |                           |                       | libxml2: XML External                                        |
|                  |                     |          |                           |                       | Entity vulnerability                                         |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2016-9318                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2020-24977      |          |                           | 2.9.4+dfsg1-7+deb10u2 | libxml2: Buffer overflow                                     |
|                  |                     |          |                           |                       | vulnerability in                                             |
|                  |                     |          |                           |                       | xmlEncodeEntitiesInternal()                                  |
|                  |                     |          |                           |                       | in entities.c                                                |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-24977                        |
+                  +---------------------+          +                           +                       +--------------------------------------------------------------+
|                  | CVE-2021-3537       |          |                           |                       | libxml2: NULL pointer dereference                            |
|                  |                     |          |                           |                       | when post-validating mixed                                   |
|                  |                     |          |                           |                       | content parsed in recovery mode...                           |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-3537                         |
+                  +---------------------+          +                           +                       +--------------------------------------------------------------+
|                  | CVE-2021-3541       |          |                           |                       | libxml2: Exponential entity                                  |
|                  |                     |          |                           |                       | expansion attack bypasses all                                |
|                  |                     |          |                           |                       | existing protection mechanisms                               |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-3541                         |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| libxslt1.1       | CVE-2015-9019       | LOW      | 1.1.32-2.2~deb10u1        |                       | libxslt: math.random() in                                    |
|                  |                     |          |                           |                       | xslt uses unseeded randomness                                |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2015-9019                         |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| login            | CVE-2007-5686       |          | 1:4.5-1.1                 |                       | initscripts in rPath Linux 1                                 |
|                  |                     |          |                           |                       | sets insecure permissions for                                |
|                  |                     |          |                           |                       | the /var/log/btmp file,...                                   |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2007-5686                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2013-4235       |          |                           |                       | shadow-utils: TOCTOU race                                    |
|                  |                     |          |                           |                       | conditions by copying and                                    |
|                  |                     |          |                           |                       | removing directory trees                                     |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2013-4235                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2018-7169       |          |                           |                       | shadow-utils: newgidmap                                      |
|                  |                     |          |                           |                       | allows unprivileged user to                                  |
|                  |                     |          |                           |                       | drop supplementary groups                                    |
|                  |                     |          |                           |                       | potentially allowing privilege...                            |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-7169                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-19882      |          |                           |                       | shadow-utils: local users can                                |
|                  |                     |          |                           |                       | obtain root access because setuid                            |
|                  |                     |          |                           |                       | programs are misconfigured...                                |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-19882                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | TEMP-0628843-DBAD28 |          |                           |                       | -->security-tracker.debian.org/tracker/TEMP-0628843-DBAD28   |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| mount            | CVE-2021-37600      |          | 2.33.1-0.1                |                       | util-linux: integer overflow                                 |
|                  |                     |          |                           |                       | can lead to buffer overflow                                  |
|                  |                     |          |                           |                       | in get_sem_elements() in                                     |
|                  |                     |          |                           |                       | sys-utils/ipcutils.c...                                      |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-37600                        |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| nginx            | CVE-2021-3618       | HIGH     | 1.20.1-1~buster           |                       | ALPACA: Application Layer                                    |
|                  |                     |          |                           |                       | Protocol Confusion - Analyzing                               |
|                  |                     |          |                           |                       | and Mitigating Cracks in TLS...                              |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-3618                         |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2020-36309      | MEDIUM   |                           |                       | ngx_http_lua_module (aka                                     |
|                  |                     |          |                           |                       | lua-nginx-module) before                                     |
|                  |                     |          |                           |                       | 0.10.16 in OpenResty allows                                  |
|                  |                     |          |                           |                       | unsafe characters in an...                                   |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2020-36309                        |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2009-4487       | LOW      |                           |                       | nginx: Absent sanitation of                                  |
|                  |                     |          |                           |                       | escape sequences in web server log                           |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2009-4487                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2013-0337       |          |                           |                       | The default configuration of nginx,                          |
|                  |                     |          |                           |                       | possibly 1.3.13 and earlier, uses                            |
|                  |                     |          |                           |                       | world-readable permissions...                                |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2013-0337                         |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+
| openssl          | CVE-2021-3711       | HIGH     | 1.1.1d-0+deb10u6          | 1.1.1d-0+deb10u7      | openssl: SM2 Decryption                                      |
|                  |                     |          |                           |                       | Buffer Overflow                                              |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-3711                         |
+                  +---------------------+----------+                           +                       +--------------------------------------------------------------+
|                  | CVE-2021-3712       | MEDIUM   |                           |                       | openssl: Read buffer overruns                                |
|                  |                     |          |                           |                       | processing ASN.1 strings                                     |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-3712                         |
+                  +---------------------+----------+                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2007-6755       | LOW      |                           |                       | Dual_EC_DRBG: weak pseudo                                    |
|                  |                     |          |                           |                       | random number generator                                      |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2007-6755                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2010-0928       |          |                           |                       | openssl: RSA authentication weakness                         |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2010-0928                         |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| passwd           | CVE-2007-5686       |          | 1:4.5-1.1                 |                       | initscripts in rPath Linux 1                                 |
|                  |                     |          |                           |                       | sets insecure permissions for                                |
|                  |                     |          |                           |                       | the /var/log/btmp file,...                                   |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2007-5686                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2013-4235       |          |                           |                       | shadow-utils: TOCTOU race                                    |
|                  |                     |          |                           |                       | conditions by copying and                                    |
|                  |                     |          |                           |                       | removing directory trees                                     |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2013-4235                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2018-7169       |          |                           |                       | shadow-utils: newgidmap                                      |
|                  |                     |          |                           |                       | allows unprivileged user to                                  |
|                  |                     |          |                           |                       | drop supplementary groups                                    |
|                  |                     |          |                           |                       | potentially allowing privilege...                            |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2018-7169                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-19882      |          |                           |                       | shadow-utils: local users can                                |
|                  |                     |          |                           |                       | obtain root access because setuid                            |
|                  |                     |          |                           |                       | programs are misconfigured...                                |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-19882                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | TEMP-0628843-DBAD28 |          |                           |                       | -->security-tracker.debian.org/tracker/TEMP-0628843-DBAD28   |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| perl-base        | CVE-2011-4116       |          | 5.28.1-6+deb10u1          |                       | perl: File::Temp insecure                                    |
|                  |                     |          |                           |                       | temporary file handling                                      |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2011-4116                         |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| sysvinit-utils   | TEMP-0517018-A83CE6 |          | 2.93-8                    |                       | -->security-tracker.debian.org/tracker/TEMP-0517018-A83CE6   |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| tar              | CVE-2005-2541       |          | 1.30+dfsg-6               |                       | tar: does not properly warn the user                         |
|                  |                     |          |                           |                       | when extracting setuid or setgid...                          |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2005-2541                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2019-9923       |          |                           |                       | tar: null-pointer dereference                                |
|                  |                     |          |                           |                       | in pax_decode_header in sparse.c                             |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2019-9923                         |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | CVE-2021-20193      |          |                           |                       | tar: Memory leak in                                          |
|                  |                     |          |                           |                       | read_header() in list.c                                      |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-20193                        |
+                  +---------------------+          +                           +-----------------------+--------------------------------------------------------------+
|                  | TEMP-0290435-0B57B5 |          |                           |                       | -->security-tracker.debian.org/tracker/TEMP-0290435-0B57B5   |
+------------------+---------------------+          +---------------------------+-----------------------+--------------------------------------------------------------+
| util-linux       | CVE-2021-37600      |          | 2.33.1-0.1                |                       | util-linux: integer overflow                                 |
|                  |                     |          |                           |                       | can lead to buffer overflow                                  |
|                  |                     |          |                           |                       | in get_sem_elements() in                                     |
|                  |                     |          |                           |                       | sys-utils/ipcutils.c...                                      |
|                  |                     |          |                           |                       | -->avd.aquasec.com/nvd/cve-2021-37600                        |
+------------------+---------------------+----------+---------------------------+-----------------------+--------------------------------------------------------------+

```

### sysnet4admin/net-tools

```bash
[root@m-k8s ~]# trivy image sysnet4admin/net-tools
2021-08-28T07:49:48.840+0900    INFO    Detected OS: alpine
2021-08-28T07:49:48.841+0900    INFO    Detecting Alpine vulnerabilities...
2021-08-28T07:49:48.860+0900    INFO    Number of language-specific files: 0

sysnet4admin/net-tools (alpine 3.13.5)
======================================
Total: 8 (UNKNOWN: 0, LOW: 0, MEDIUM: 2, HIGH: 5, CRITICAL: 1)

+--------------+------------------+----------+-------------------+---------------+---------------------------------------+
|   LIBRARY    | VULNERABILITY ID | SEVERITY | INSTALLED VERSION | FIXED VERSION |                 TITLE                 |
+--------------+------------------+----------+-------------------+---------------+---------------------------------------+
| apk-tools    | CVE-2021-36159   | CRITICAL | 2.12.5-r0         | 2.12.6-r0     | libfetch before 2021-07-26, as        |
|              |                  |          |                   |               | used in apk-tools, xbps, and          |
|              |                  |          |                   |               | other products, mishandles...         |
|              |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2021-36159 |
+--------------+------------------+----------+-------------------+---------------+---------------------------------------+
| bind-libs    | CVE-2021-25218   | HIGH     | 9.16.15-r1        | 9.16.20-r0    | bind: Too strict assertion            |
|              |                  |          |                   |               | check could be triggered              |
|              |                  |          |                   |               | when responses require UDP...         |
|              |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2021-25218 |
+--------------+                  +          +                   +               +                                       +
| bind-tools   |                  |          |                   |               |                                       |
|              |                  |          |                   |               |                                       |
|              |                  |          |                   |               |                                       |
|              |                  |          |                   |               |                                       |
+--------------+------------------+          +-------------------+---------------+---------------------------------------+
| krb5-libs    | CVE-2021-36222   |          | 1.18.3-r1         | 1.18.4-r0     | krb5: sending a request containing    |
|              |                  |          |                   |               | a PA-ENCRYPTED-CHALLENGE padata       |
|              |                  |          |                   |               | element without using FAST...         |
|              |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2021-36222 |
+--------------+------------------+          +-------------------+---------------+---------------------------------------+
| libcrypto1.1 | CVE-2021-3711    |          | 1.1.1k-r0         | 1.1.1l-r0     | openssl: SM2 Decryption               |
|              |                  |          |                   |               | Buffer Overflow                       |
|              |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2021-3711  |
+              +------------------+----------+                   +               +---------------------------------------+
|              | CVE-2021-3712    | MEDIUM   |                   |               | openssl: Read buffer overruns         |
|              |                  |          |                   |               | processing ASN.1 strings              |
|              |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2021-3712  |
+--------------+------------------+----------+                   +               +---------------------------------------+
| libssl1.1    | CVE-2021-3711    | HIGH     |                   |               | openssl: SM2 Decryption               |
|              |                  |          |                   |               | Buffer Overflow                       |
|              |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2021-3711  |
+              +------------------+----------+                   +               +---------------------------------------+
|              | CVE-2021-3712    | MEDIUM   |                   |               | openssl: Read buffer overruns         |
|              |                  |          |                   |               | processing ASN.1 strings              |
|              |                  |          |                   |               | -->avd.aquasec.com/nvd/cve-2021-3712  |
+--------------+------------------+----------+-------------------+---------------+---------------------------------------+

```

{% embed url="https://github.com/aquasecurity/trivy" %}

{% embed url="https://aquasecurity.github.io/trivy/v0.19.2/getting-started/installation/" %}

