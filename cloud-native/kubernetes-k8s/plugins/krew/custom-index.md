---
description: https://github.com/sysnet4admin/custom-index
---

# custom-index

## 0.Pre-requirement

**krew** installed on your system and make permanent `export` to avoid your bothering.&#x20;

{% embed url="https://krew.sigs.k8s.io/docs/user-guide/setup/install/" %}

If you have any kind of issue you would be destroy it. `uninstall` is best solution most of times.&#x20;

{% embed url="https://krew.sigs.k8s.io/docs/user-guide/setup/uninstall/" %}

**Note:** This document based on this document. So I recommend to read both.&#x20;

{% embed url="https://krew.sigs.k8s.io/docs/user-guide/custom-indexes/" %}

## 1.Build your own plugin files&#x20;

### 1-1.Executable file&#x20;

Here is [SAMPLE](https://github.com/sysnet4admin/kubectxon) file. it has this code as a bash.&#x20;

```bash
<snipped> <snipped> 

main() {
  # avoid chaos history 
  unset HISTFILE
  
  # create ctxon working directory & kubectxon as well 
  set_env

  # backup history forcely 
  \cp -rf $historyPATH $HOME/.ctxon/.backup_history	

  pass_kubectl
  ARGS=${@:1}

  if [[ "$#" -eq 0 ]]; then
    ARG1="default" 
    toggle_ctxon
    exit 0
  elif [[ "$1" == "-h" || "$1" == "--help" ]]; then
    usage
    exit 0
  elif [[ "$1" == "uninstall" ]]; then
    unset_env
    exit 0
  elif (( $1 >= 30 && $1 <= 37 || $1 >=40 && $1 <=47 )); then
    ARG1="$1"	  
    toggle_ctxon
    exit 0 
  else
    echo "$ARGS Option Not Supported"	 
    exit 1  
  fi

  # history recovery and up all 
  echo $historyPATH
  mv $HOME/.ctxon/.backup_history $historyPATH
  history -c; history -r $historyPATH
  set HISTFILE
}

main "$@"

```

###

### 1-2.LICENSE

Here is [SAMPLE](https://github.com/sysnet4admin/kubectxon/blob/main/LICENSE) file. it is MIT.



### 1-3.Achieving files&#x20;

Both tar.gz or zip are okay. so you could use as your preference.    &#x20;

```bash
$ tar cvfz kubectxon_v0.0.3_linux_arm64.tar.gz kubectxon LICENSE
```

####

### 1-4.Upload achieved file your repo's **releases**&#x20;

![](<../../../../.gitbook/assets/image (12).png>)



### 1-5.make manifest file&#x20;

Here is [SAMPLE](https://github.com/sysnet4admin/custom-index/blob/master/plugins/ctxon.yaml) `ctxon.yaml` file. it needs to create in **LOCAL** first&#x20;

```yaml
apiVersion: krew.googlecontainertools.github.com/v1alpha2
kind: Plugin
metadata:
  name: ctxon
spec:
  platforms:
  - sha256: 64e336dca2d0d152687b6d837ae5d1d05b59ca409064056f4cb2598ca00fe764
    uri: https://github.com/sysnet4admin/kubectxon/releases/download/v0.0.3/kubectxon_v0.0.3_linux_arm64.tar.gz
    bin: kubectxon
    files:
    - from: "kubectxon"
      to: "."
    - from: "LICENSE"
      to: "."
    selector:
      matchExpressions:
      - {key: os, operator: In, values: [darwin,linux]}
  version: "v0.0.3"
  homepage: https://github.com/sysnet4admin/kubectxon
  shortDescription: Easy to check active-context in kubernetes thru the prompt
```



### 1-6.Check Validation

It can check automatically (especially sha256), so you could verify and modify as this guideline.&#x20;

{% embed url="https://krew.sigs.k8s.io/docs/developer-guide/testing-locally/" %}

Here is some error on sha256, so I changed as this guideline. or you could make it before showing this error. [online-sha256](https://emn178.github.io/online-tools/sha256.html) link.&#x20;

```bash
$ kubectl krew install --manifest=ctxon.yaml --archive=kubectxon_v0.0.3_linux_arm64.tar.gz
Installing plugin: ctxon
W0512 14:12:58.127870   48774 install.go:164] failed to install plugin "ctxon": install failed: failed to unpack into staging dir: failed to unpack the plugin archive: checksum does not match, want: 48ff5e51920b76506f99e18ff927a6b79a354d306e3cb0063af6517b53c605cb, got 64e336dca2d0d152687b6d837ae5d1d05b59ca409064056f4cb2598ca00fe764
F0512 14:12:58.127999   48774 root.go:79] failed to install some plugins: [ctxon]: install failed: failed to unpack into staging dir: failed to unpack the plugin archive: checksum does not match, want: 48ff5e51920b76506f99e18ff927a6b79a354d306e3cb0063af6517b53c605cb, got 64e336dca2d0d152687b6d837ae5d1d05b59ca409064056f4cb2598ca00fe764
```



## 2.Publish your files

### 2-1.Create REPO like this tree structure.&#x20;

{% embed url="https://github.com/sysnet4admin/custom-index" %}

i.e. manifest files should locate in **plugins** folder from main.&#x20;

![](<../../../../.gitbook/assets/image (17).png>)

plugins folder have actual manifests. so upload your `manifest` in **plugins** folder&#x20;

![](<../../../../.gitbook/assets/image (8).png>)

it is same methodology from default ([krew-index](https://github.com/kubernetes-sigs/krew-index)) &#x20;



### 2-2.Check your upload files to your system :)&#x20;

Here is the procedure of installation. it should work if you have no issue until here.&#x20;

{% embed url="https://github.com/sysnet4admin/custom-index/tree/master/plugins" %}



See ya!!!
