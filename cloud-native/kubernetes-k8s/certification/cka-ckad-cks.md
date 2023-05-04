# CKA, CKAD, CKS - Jan 03 2022

### **CKA & CKAD Environment**

* Each task on this exam must be completed on a designated cluster/configuration context.
* To minimize switching, the tasks are grouped so that all questions on a given cluster appear consecutively.
* There are six clusters (CKA) / four clusters (CKAD) that comprise the exam environment, made up of varying numbers of containers, as follows:

|             |                       | **CKA Clusters** |                                                |
| ----------- | --------------------- | ---------------- | ---------------------------------------------- |
| **Cluster** | **Members**           | **CNI**          | **Description**                                |
| k8s         | 1 master, 2 worker    | flannel          | k8s cluster                                    |
| hk8s        | 1 master, 2 worker    | calico           | k8s cluster                                    |
| bk8s        | 1 master, 1 worker    | flannel          | k8s cluster                                    |
| wk8s        | 1 master, 2 worker    | flannel          | k8s cluster                                    |
| ek8s        | 1 master, 2 worker    | flannel          | k8s cluster                                    |
| ik8s        | 1 master, 1 base node | loopback         | <p>k8s cluster − missing worker</p><p>node</p> |



|             |                    | **CKAD  Clusters** |                 |
| ----------- | ------------------ | ------------------ | --------------- |
| **Cluster** | **Members**        | **CNI**            | **Description** |
| k8s         | 1 master, 2 worker | flannel            | k8s cluster     |
| dk8s        | 1 master, 1 worker | flannel            | k8s cluster     |
| nk8s        | 1 master, 2 worker | calico             | k8s cluster     |
| sk8s        | 1 master, 1 worker | flannel            | k8s cluster     |

{% embed url="https://docs.linuxfoundation.org/tc-docs/certification/tips-cka-and-ckad" %}

### **CKS Environment**

* Each task on this exam must be completed on a designated cluster/configuration context.
* **Sixteen clusters comprise** the exam environment, one for each task. Each cluster is made up of one master node and one worker node.

{% embed url="https://docs.linuxfoundation.org/tc-docs/certification/important-instructions-cks" %}

### Common Enviroment&#x20;

For your convenience, all environments, in other words, the base system and the cluster nodes, have the following additional command-line tools pre-installed and pre-configured:

* `kubectl` with `k`alias and Bash autocompletion   << wow good&#x20;
* `jq` for YAML/JSON processing
* `tmux` for terminal multiplexing
* `curl` and `wget` for testing web services
* `man` and man pages for further documentation

The CKA & CKAD environments are currently running Kubernetes v1.22.\
_The CKA, CKS and CKAD exam environment will be aligned with the most recent K8s minor version within approximately 4 to 8 weeks of the K8s release date._

_>>> i.e. Latest version_&#x20;

### _CKA & CKAD_ **Resources allowd during exam**

During the exam, candidates may:

* review the Exam content instructions that are presented in the command line terminal
* review Documents installed by the distribution (i.e. /usr/share and its subdirectories)
* use their Chrome or Chromium browser to open one additional tab in order to access assets at: [https://helm.sh/docs/](https://helm.sh/docs/), [https://kubernetes.io/docs/](https://kubernetes.io/docs/), [https://github.com/kubernetes/](https://github.com/kubernetes/),  [https://kubernetes.io/blog/](https://kubernetes.io/blog/) and their subdomains. This includes all available language translations of these pages (e.g. [https://kubernetes.io/zh/docs/)](https://kubernetes.io/zh/docs/home/)

No other tabs may be opened and no other sites may be navigated to   (including [https://discuss.kubernetes.io/](https://discuss.kubernetes.io/)).&#x20;

_The allowed sites above may contain links that point to external sites. It is the responsibility of the candidate not to click on any links that cause them to navigate to a domain that is not allowed._



### _CKS_ **Resources allowed during exam**

During the exam, candidates may:

* review the Exam content instructions that are presented in the command line terminal.
* review Documents installed by the distribution (i.e. /usr/share and its subdirectories)
* use their Chrome or Chromium browser to open one additional tab in order to access&#x20;
  *   **Kubernetes Documentation:**&#x20;

      * [https://kubernetes.io/docs/](https://kubernetes.io/docs/) and their subdomains
      * [https://github.com/kubernetes/](https://github.com/kubernetes/) and their subdomains
      * [https://kubernetes.io/blog/](https://kubernetes.io/blog/) and their subdomains

      This includes all available language translations of these pages (e.g. [https://kubernetes.io/zh/docs/](https://kubernetes.io/zh/docs/home/))
  *   **Tools:**

      * Trivy documentation [https://aquasecurity.github.io/trivy/](https://aquasecurity.github.io/trivy/)
      * Sysdig documentation [https://docs.sysdig.com/](https://docs.sysdig.com/)
      * Falco documentation [https://falco.org/docs/](https://falco.org/docs/)

      This includes all available language translations of these pages (e.g. [https://falco.org/zh/docs/](https://falco.org/zh/docs/))
  *   **App Armor:**

      * Documentation [https://gitlab.com/apparmor/apparmor/-/wikis/Documentation](https://gitlab.com/apparmor/apparmor/-/wikis/Documentation)

      \
      _The allowed sites above may contain links that point to external sites. It is the responsibility of the candidate not to click any links to navigate to a domain that is not allowed_

## OS Version

* CKA: Ubuntu 18.04
* CKAD: Ubuntu 18.04
* CKS: Ubuntu 18.04

{% embed url="https://docs.linuxfoundation.org/tc-docs/certification/lf-candidate-handbook/exam-preparation-checklist" %}

## USER Interface&#x20;

![](<../../../.gitbook/assets/image (18).png>)

{% embed url="https://docs.linuxfoundation.org/tc-docs/certification/lf-candidate-handbook/exam-user-interface" %}

## Exam Duration&#x20;

* CKA 2 hour
* CKAD 2 hour
* CKS 2 hour

## Pass or No Pass

![Pass format](<../../../.gitbook/assets/image (2).png>)

{% embed url="https://docs.linuxfoundation.org/tc-docs/certification/lf-candidate-handbook/exam-scoring-and-notification/exam-results-pass" %}



![No Pass](<../../../.gitbook/assets/image (22).png>)

{% embed url="https://docs.linuxfoundation.org/tc-docs/certification/lf-candidate-handbook/exam-scoring-and-notification/exam-results-no-pass" %}

## Certification expire

Certifications expire 36\*  months from the date that a certificant successfully passes their certification exam, unless revoked earlier for cause or certificant successfully completes certification renewal requirements.  \
\* **NOTE:** CKS Certifications expire 24 months from the date that certificant successfully passes their certification exam, unless revoked earlier for cause or certificant successfully completes certification renewal requirements

{% embed url="https://docs.linuxfoundation.org/tc-docs/certification/lf-candidate-handbook/certificates-and-certification" %}

Certifications expire 36\*\* months from the date that the Program Certification requirements are met by a candidate.  &#x20;

**\*\*NOTE THE SPECIFIC EXPIRATION POLICY FOR CKS, LFCS, LFCE AND CFCD CERTIFICATIONS \*\***

### **CKS**

CKS Certifications will expire 24 months from the date that the Program certification requirements are met, unless revoked earlier for cause of Certificant successfully completes Certification renewal

## **Certification Renewal Requirements**

Certification **Renewal** requirements must be completed prior to the expiration of the Certification.

Candidates may keep their Certification valid  by retaking and passing the same exam, prior to the expiration of their certification.  \
The Certification will become valid for 3 years (2 years for CKS) starting on the date the exam is retaken and passed.

{% embed url="https://docs.linuxfoundation.org/tc-docs/certification/lf-candidate-handbook/certificates-and-certification#certification-expiration" %}



## Certification Verification Tool

The Linux Foundation provides a [verification tool](https://training.linuxfoundation.org/certification/verify-certifications) that allows users (e.g. potential employers or clients) to validate the status of an individual’s certification.

{% embed url="https://docs.linuxfoundation.org/tc-docs/certification/lf-candidate-handbook/certificates-and-certification/certification-verification-tool" %}

