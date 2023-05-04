---
description: https://www.cncf.io/blog/2023/04/24/how-to-use-weave-gitops-as-your-flux-ui/
cover: ../../../.gitbook/assets/CNCF-Ambassador.jpg
coverY: 0
---

# Weave GitOps를 Flux UI로 구현하는 법(2023.04.24)



> **한줄 요약:** Flux의 일부 약점이었던 UI의 기능 개선이 이루어지고, 이를 통해서 GitOps를 구현할 수 있습니다.&#x20;
>
> **FluxCD:** [https://github.com/fluxcd/website/](https://github.com/fluxcd/website/)
>
> **Flux Ecosystem:** [https://fluxcd.io/ecosystem/](https://fluxcd.io/ecosystem/)
>
> **GitOps Run:** [https://fluxcd.io/ecosystem/](https://fluxcd.io/ecosystem/)

{% hint style="info" %}
이 문서는 기계 번역 후에 일부 내용만 다듬었으므로, 보다 정확한 이해를 위해서는 원문을 보는 것이 좋습니다.
{% endhint %}

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/04/Single-Template-4-1180x620.png" alt=""><figcaption></figcaption></figure>

2023년 4월 24일 게시됨 ([Link](https://www.cncf.io/blog/2023/04/24/how-to-use-weave-gitops-as-your-flux-ui/))&#x20;

2023 Daniel Holbach가 [Flux 블로그](https://fluxcd.io/blog/2023/04/how-to-use-weave-gitops-as-your-flux-ui/)에 게시한 게스트 게시물입니다.

[생태계 카테고리](https://fluxcd.io/tags/ecosystem/)의 최신 블로그 포스팅을 소개합니다. Flux를 재작성하게 된 주요 이유 중 하나는 이전의 모놀리스 솔루션을 기능의 각 부분을 제공하는 별도의 컨트롤러로 분리했기 때문입니다. 이를 통해 사용자는 필요한 부분만 선택할 수 있고, 통합자는 Flux의 API를 기반으로 매우 쉽게 빌드할 수 있습니다. 현재 [Flux 생태계](https://fluxcd.io/ecosystem/)는 매우 활성화되어 있으며, 저희는 이를 매우 환영하며 성공의 지표로 보고 있습니다.

## Weave GitOps 소개&#x20;

오늘은 Weave GitOps에 대해 이야기하고자 합니다. 이 기능은 약 1년 동안 공개적으로 구축되어 왔으며, 무엇보다도 Flux에 가장 많이 요청된 기능 중 하나인 UI를 제공합니다.

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/04/wego2-1800x1238.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/04/wego-featured-1800x1238.png" alt=""><figcaption></figcaption></figure>

Weave GitOps를 사용하면

* 애플리케이션을 모두 한 곳에서 관리 및 확인
* 지속적인 배포와 GitOps를 통해 생성되는 내용을 쉽게 확인할 수 있음
* UI에서 직접 최신 Git 커밋을 동기화할 수 있음
* 대시보드에서 권한을 제어하기 위해 Kubernetes RBAC 활용
* 조정 배포 런타임의 상태를 빠르게 확인할 수 있음

Weave GitOps 팀은 Flux 커뮤니티와 매우 긴밀하게 협력하며, 두 팀의 많은 엔지니어가 실제로 동료이기도 합니다.

UI 외에도 Weave GitOps는 GitOps 경험을 빠르게 익힐 수 있는 방법을 제공합니다: [GitOps Run](https://docs.gitops.weave.works/docs/gitops-run/overview/). 시작하는 데 필요한 것은 클러스터와 Weave GitOps CLI뿐입니다. Flux와 Weave GitOps 대시보드를 포함한 다른 모든 것이 자동으로 설정됩니다.

GitOps Run은 실제로 설정 이상의 기능을 제공합니다. 모든 것이 PR 프로세스를 거치는 일반적인 루프 대신 거의 실시간으로 변경 사항이 동기화되므로 GitOps 패턴을 유지하면서 매우 빠르게 반복할 수 있습니다. 변경 사항에 만족하는 순간 평소처럼 PR을 생성하면 됩니다. 두 가지 장점을 모두 누릴 수 있습니다.

이 짧은 동영상을 통해 사용의 편리함과 아름다움을 확인해보세요.

{% embed url="https://youtu.be/2TJz7RhDtAc" %}

테라폼 사용자라면 테라폼 컨트롤러가 기본적으로 통합되어 있고 테라폼 리소스도 대시보드에 표시된다는 점이 마음에 드실 것입니다.

## 시작하기&#x20;

위 동영상에 표시된 대로 GitOps Run을 사용하는 것이 가장 쉽게 설정할 수 있는 방법입니다.&#x20;

다음은 대시보드를 포함하여 GitOps(Flux에서 제공)를 사용하여 앱 배포를 설정하는 방법의 예입니다.

1. `brew install fluxcd/tap/flux`
2. [podinfo](https://github.com/stefanprodan/podinfo)로 이동하여 podinfo-gitops-run이라는 이름의 포크를 생성합니다.

<figure><img src="https://raw.githubusercontent.com/stefanprodan/podinfo/gh-pages/screens/podinfo-ui-v3.png" alt=""><figcaption><p>podinfo의 Web UI</p></figcaption></figure>

3. 로컬로 복제하고 디렉토리로 변경\
   `export GITHUB_USER==<`your github username`>` \
   \# 이미 리포지토리를 생성하고 \
   \# 복제했다면 이 두 명령은 무시해도 됩니다. `git@github.com:$GITHUB_USER/podinfo-gitops-run.git cd podinfo-gitops-run`
4. 이제 직접 모드에서 사용하려는 단일 사용자 클러스터이므로 `--no-session`으로 `gitops` 명령을 실행합니다. 포트 포워드는 나중에 생성할 `podinfo` 파드를 가리킵니다 \
   `./podinfo --no-session \ --port-forward namespace=dev,resource=svc/backend,port=9898:9898` \
   다른 인수는 매니페스트가 저장될 디렉터리를 나타내며, 설치하려는 애플리케이션에 포트 포워딩을 설정합니다.
5. 설치 프로세스 중에 Flux가 설치되어 있지 않은 경우 설치되며 이제 [GitOps 대시보드](https://docs.gitops.weave.works/docs/getting-started/intro/)를 설치할 것인지 묻는 메시지가 표시됩니다. **예**라고 대답하고 비밀번호를 설정하세요.\
   참고: 비밀번호를 설정하지 않으면 GitOps UI에 로그인할 수 없습니다 😱.잠시 후 대시보드를 열 수 있습니다. 사용자 이름은 **admin**이고 비밀번호는 위에서 **설정한 비밀번호**입니다.
6. `podinfo` 디렉터리의 내용을 확인하면 `kustomization.yam`l 파일을 찾을 수 있습니다. 리소스 요소를 편집하여 "`../deploy/overlays/dev`"도 나열합니다. \
   해당 내용은 아래와 같아야 합니다\
   `---` \
   `apiVersion: kustomize.config.k8s.io/v1beta1` \
   `kind: Kustomization` \
   `name: dev-podinfo` \
   `resources: [` \
   &#x20; `"../deploy/overlays/dev"` \
   `]`\


파일을 저장하면 `podinfo`가 배포되고 [http://localhost:9898](http://localhost:9898) 에서 액세스할 수 있습니다.

터미널에서 실행 중인 "gitops" 프로세스를 **Ctrl-C**하면 배포를 "GitOps 모드"로 변경할 것인지 묻는 메시지가 표시되며, 이는 클러스터 정의 및 대시보드에 대한 매니페스트도 추가되어 GitHub로 푸시된다는 의미입니다.

보시다시피, Weave GitOps는 반복적인 작업과 무거운 작업을 많이 처리합니다. 설정하기만 하면 Flux가 뒤에서 모든 것을 알아서 처리해준다는 것을 알 수 있는 멋진 방법입니다.



## 더 필요한 것이 있을까요?&#x20;

위에서 언급한 모든 사항에 대한 전문적인 지원이 필요한 경우, 엔터프라이즈 버전의 Weave GitOps도 이용할 수 있습니다. 그 외에도 애플리케이션 팀을 위한 셀프 서비스를 만들 수 있는 템플릿 및 GitOpsSet과 같은 고급 기능을 사용할 수 있습니다.

Weave GitOps 팀은 매우 친절하며 항상 기꺼이 도와드리고 피드백을 받습니다. [Weave 사용자 슬랙](https://slack.weave.works/)의 `#weave-gitops` 채널에 참여하세요.



## 더 나은 변화를 위해 우리에게 피드백을 주세요.

이 이야기에 대한 피드백이 있으면 Slack이나 소셜 미디어를 통해 알려주시고, 직접 들려줄 이야기가 있다면 [fluxcd.io 웹사이트 저장소](https://github.com/fluxcd/website/)에도 문의해 주세요. 저희는 생태계의 더 많은 이야기와 Flux 성공 사례를 보고 싶습니다. 연락해 주셔서 미리 감사드립니다!

