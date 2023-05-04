---
description: https://www.cncf.io/blog/2023/04/06/introduction-to-the-linkerd-service-mesh/
cover: ../../../.gitbook/assets/CNCF-Ambassador.jpg
coverY: 0
---

# 링커드(Linkerd) 서비스 메시(Service Mesh) 소개(2023.04.06)

> **한줄 요약:** 복잡도가 높은 애플리케이션들의 통신을 매우 구체적으로 살펴 볼 수 있게 해주는 링커드(Linkerd) 서비스 메시의 구성 방법과 데모를 설명하고 있습니다.
>
> **관련 프로젝트:** \
> &#x20;\- [https://linkerd.io/](https://linkerd.io/)

{% hint style="info" %}
이 문서는 기계 번역 후에 일부 내용만 다듬었으므로, 보다 정확한 이해를 위해서는 원문을 보는 것이 좋습니다.
{% endhint %}

마이클 레반 4월 6일, 2023&#x20;

이 게스트 포스트는 원래 Michael Levan이 [_Bouyant_](https://buoyant.io/blog/introduction-to-the-linkerd-service-mesh) 블로그에 게시한 글입니다.

파드를 배포할 때 애플리케이션이 원하는 방식으로 실행되고 있는지 알고 계신가요? 트래픽이 암호화되어 있나요? 애플리케이션이 예상대로 작동하고 있나요? 계속 시간 초과가 발생하나요? 계속 재시도가 발생하나요? 이러한 질문은 컨테이너화 여부와 관계없이 프로덕션 환경에서 애플리케이션을 실행할 때 스스로에게 물어봐야 할 질문입니다.

이 블로그 게시물에서는 네트워크 보안과 통합 가시성의 사실상 표준이 되고 있는 인기 있는 프레임워크/플랫폼인 [서비스 메시](https://buoyant.io/service-mesh-manifesto)(Service Mesh)를 사용하여 이러한 질문에 답하는 방법을 알아보세요.



## 왜 서비스 메시를 사용할까요?

서비스 메시를 사용하면 컨테이너화된 애플리케이션이 네트워크 수준에서 어떤 작업을 수행하는지 파악할 수 있습니다.

좀 더 자세히 살펴보겠습니다.

쿠버네티스는 컨테이너 스케줄링이라는 한 가지 주요 작업을 수행합니다. 자가 복구, 자동 확장 및 기타 여러 기능이 있지만, 그 핵심은 Kubernetes가 파드(하나 이상의 컨테이너로 구성)를 스케줄링하고 이를 실행하는 데 사용할 수 있는 적절한 리소스가 있는 노드에 배치하는 것입니다.

Kubernetes는 기본적으로 보안, 네트워크 가시성 또는 기타 기능을 제공하기 위한 것이 아닙니다. 기본적으로 "나만의 모험을 선택하세요"라고 말하는 플랫폼을 제공합니다. Kubernetes는 오픈 소스이고 엔지니어링 영역에서 가시성이 높기 때문에 많은 벤더가 다양한 플러그인, 프레임워크 및 타사 도구를 구축하기 시작했으며, 이를 통해 사용자가 원하는 Kubernetes의 모습을 직접 선택할 수 있을 뿐만 아니라 프로덕션 지원 플랫폼을 커스터마이징할 수 있는 기능을 제공할 수 있게 되었습니다.

서비스 메시는 네트워크 가시성 및 관리를 위한 이러한 타사 "도구"(또는 분류 방법에 따라 서비스) 중 하나입니다.

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/04/6419eff9a22b7c767c8fe2b2_Screenshot-2023-03-21-at-1.56.43-PM.png" alt=""><figcaption></figcaption></figure>

서비스 메시가 하는 3가지 중요 내용이 있습니다.

```
    1. 보안 (Security)
    2. 신뢰성 (Reliability)
    3. 관찰 가능성 (Observability)
```

### 서비스 메시의 보안

보안은 모든 환경에서 중요하지만, 특히 포드 네트워킹에서는 항상 주요 관심사입니다. 기본적으로 모든 파드와 서비스는 다른 파드와 서비스 간에 완전히 암호화되지 않은 패킷을 전송합니다(일반적으로 이스트-웨스트 트래픽이라고 함). 서비스 메시는 mTLS로 이 문제를 해결합니다. 어떻게 할까요?

서비스 메시 컨테이너는 컨테이너화된 애플리케이션과 함께 각 파드에 주입되어 모든 트래픽이 포드 네트워크 내에서 암호화되도록 보장합니다. 이 구현은 일반적으로 매우 평평한 네트워크를 볼 수 있기 때문에 훌륭합니다. 네트워크 외부와 내부로 나가는 트래픽(송신 및 수신)을 위한 방화벽이 있지만, 일단 네트워크에 들어가면 일반적으로 모든 파드는 다른 파드와 직접 통신할 수 있습니다. 서비스 메시를 사용하면 방화벽이 외부 트래픽을 보호하는 동안 포드 네트워크 내부를 보호할 수 있습니다. 모든 계층에서 보안 윈윈입니다!

보안, 특히 mTLS/암호화 부분은 엔지니어가 애초에 서비스 메시를 선택하는 가장 큰 이유입니다. 서비스 메시에는 다른 많은 이점이 있지만, 패킷/네트워크 암호화 부분이 처음에 채택을 유도하는 경우가 많습니다.

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/04/6419f069cd10e7f3a4ffd42f_Screenshot-2023-03-21-at-1.58.56-PM.png" alt=""><figcaption></figcaption></figure>

### 서비스 메시의 신뢰성

두 번째 이점은 컨테이너화된 애플리케이션이 신뢰할 수 있는지 여부를 파악하는 것입니다. 신뢰성 측면에서는 일반적으로 비즈니스 비용/편익 분석과 추가 신뢰성 측정이 애플리케이션 성능에 미치는 영향을 모두 고려해야 합니다. 파드가 배포되면 그 안에서 실행 중인 애플리케이션이 예상대로 작동할 수도 있고 그렇지 않을 수도 있습니다. 예를 들어, 시간 초과가 발생할 때까지 계속 재시도가 발생할 수 있습니다. 애플리케이션이 예상대로 실행되지 않으면 사용자가 애플리케이션에 액세스할 수 없고, 엔지니어는 하던 일을 중단하고 애플리케이션 문제를 해결해야 하며, 비즈니스 전체가 타격을 입게 되므로 이는 분명 엔지니어링 문제이지만 비즈니스 문제이기도 합니다. 서비스 메시 로드 밸런서는 정상 인스턴스(서비스)로 트래픽을 라우팅하여 비정상 인스턴스를 방지하고 궁극적으로 애플리케이션 안정성을 향상시킵니다.

### 서비스 메시의 관찰 가능성

컨테이너화된 애플리케이션이 예상대로 작동하는지 확인하려면 쉽게 확인할 수 있는 메트릭이 필요합니다. 실행 중인 앱의 성공률, 사용자 요청의 지연 시간, 전체 트래픽 양을 이해하는 것은 애플리케이션의 현재 성능과 향후 성능을 이해하는 데 핵심적인 요소입니다. 즉, 성능이 좋지 않은 경우 그 이유를 이해할 수 있습니다.

지금까지 살펴본 것처럼 서비스 메시는 강력한 기능을 제공합니다. 다음 섹션에서는 특히 링커드(Linkerd)가 훌륭한 옵션인 이유에 대해 설명하겠습니다.



## 특히 링커드가 좋은 이유는?

이전 섹션에서는 서비스 메시를 사용해야 하는 이유에 대해 설명했습니다. 엔지니어들과 이야기를 나누다 보면 보통 2일차 작업이 끝날 때 서비스 메시를 도입할 것을 권장하며, 그렇게 해야 하는 강력한 이유가 필요하다고 말합니다. 이는 서비스 메시가 복잡하다는 평판이 있기 때문인데, 특히 Kubernetes와 서비스 메시 개념을 처음 접하는 엔지니어의 눈에는 복잡하다는 평판이 있어 많은 엔지니어가 가능한 한 오랫동안 서비스 메시를 피하려고 하기 때문입니다.

하지만 링커드는 이러한 운영 복잡성을 최소화하여 첫날 운영 시작부터 서비스 메시를 활용할 수 있도록 하는 것을 주요 목표로 삼고 있습니다. 그 방법을 살펴보겠습니다.

{% embed url="https://youtu.be/up3fKwXdEgc" %}

Linkerd 서비스 메시 배포 이 섹션에서는 다음을 수행합니다:

```
    1. 애플리케이션 배포하기
    2. Linkerd 배포
    3. Linkerd의 시각화 도구를 사용하여 애플리케이션 보기
```

이 과정을 따라하기 위해서는 Minikube 또는 Kind와 같은 Kubernetes 클러스터 또는 프로덕션 지원 클러스터가 준비되어 있어야 합니다.

애플리케이션 설치 먼저 애플리케이션을 설치합니다. 단순한 "이 Nginx 포드를 설치하세요"라는 데모가 되지 않기를 바라므로, 널리 사용되는 마이크로서비스 기반 데모 애플리케이션을 사용해 보겠습니다. Sock Shop 앱은 많은 데모에 사용되며 Linkerd와 같은 서비스 메시가 어떻게 작동하는지 보여주는 데 이상적입니다.

우선 깃허브 저장소를 복제(clone)합니다.

```
 git clone https://github.com/microservices-demo/microservices-demo 
```

그 다음으로는 마이크로서비스 데모를 보여줄 수 있는 디렉터리(`microservices-demo`)로 이동합니다.

```
 cd microservices-demo 
```

실습을 위해서 한 번 더 디렉터리를 `deploy/kubernetes`로 변경합니다.

```
 cd deploy/kubernetes 
```

이제 shop shop 애플리케이션을 배포하기 위한, 쿠버네티스 네임스페이스를 만듭니다.

```
 kubectl create namespace sock-shop 
```

애플리케이션(`complete-demo`)을 배포합니다.

```
 kubectl apply -f complete-demo.yaml 
```

다음의 스크린샷과 같은 결과가 터미널에 표시될 것입니다.

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/04/641a0639d62b2611eb57f15f_Screenshot-2023-03-21-at-3.31.56-PM.png" alt=""><figcaption></figcaption></figure>

몇 분간 기다렸다가 다음 명령을 실행하여 모든 Kubernetes Sock Shop 리소스가 성공적으로 생성되었는지 확인합니다.

```
 kubectl get all -n sock-shop 
```

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/04/641a06780dce5c55c25217e1_Screenshot-2023-03-21-at-3.33.02-PM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/04/641a06a6c501d8adcd602dab_Screenshot-2023-03-21-at-3.33.48-PM.png" alt=""><figcaption></figcaption></figure>

### Linkerd CLI 바이너리 설치

다음으로, Linkerd CLI를 설치합니다. 이를 통해 Kubernetes 클러스터가 Linkerd를 사용할 준비가 되었는지 확인하고 대시보드를 배포하는 등의 작업을 수행할 수 있습니다(나중에 설명할 예정).

다음을 실행하여 Linkerd CLI를 설치합니다.

```
 curl --proto '=https' --tlsv1.2 -sSfL https://run.linkerd.io/install | sh 
```

설치가 완료되면, 다음 명령을 실행하여 설치가 완료되었는지 확인합니다.

```
 linkerd version 
```

### Linkerd (역자: 링커드 컨트롤 플레인) 설치

먼저 Linkerd를 설치해야 합니다. 두 가지 방법이 있습니다:

* CLI&#x20;
* 헬름(Helm)

여기서는 헬름을 사용하여 데모할 것인데, 헬름을 사용하면 Linkerd 배포를 버전 관리하고 업그레이드하기가 더 쉬워 프로덕션 사용에 다소 더 적합하기 때문입니다.

먼저, 쿠버네티스 클러스터가 링커드를 실행할 준비가 되었는지 확인합니다.

```
 linkerd check --pre 
```

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/04/641a07287f6c818887cb7e2c_Screenshot-2023-03-21-at-3.35.58-PM.png" alt=""><figcaption></figcaption></figure>

다음으로 링커드 헬름 차트 저장소를 추가합니다.

```
 helm repo add linkerd https://helm.linkerd.io/stable 
```

링커드 커스텀 리소스 정의(CRD)를 설치합니다.

```
 helm install linkerd-crds linkerd/linkerd-crds \
-n linkerd --create-namespace 
```

헬름으로 링커드 컨트롤 플레인을 설치하는 경우 2가지 옵션이 있습니다.

&#x20;       1\. 고가용성 (HA, High availability)\
&#x20;       2\. 단일 노드 (Single node)

고가용성 모드(HA)로 설치하고자 한다면, 다음의 헬름 차트와 옵션을 수행합니다.

```
 helm install linkerd-control-plane \
--set-file identityTrustAnchorsPEM=ca.crt \ 
--set-file identity.issuer.tls.crtPEM=issuer.crt \ 
--set-file identity.issuer.tls.keyPEM=issuer.key \ 
-f linkerd-control-plane/values-ha.yaml \ 
linkerd/linkerd-control-plane
 
```

만약 고가용성 모드를 원하지 않는다면(예를 들면 데모 환경), 다음의 헬름 차트와 옵션을 수행합니다.

```
 helm install linkerd-control-plane \
  -n linkerd \
  --set-file identityTrustAnchorsPEM=ca.crt \
  --set-file identity.issuer.tls.crtPEM=issuer.crt \
  --set-file identity.issuer.tls.keyPEM=issuer.key \
  linkerd/linkerd-control-plane
 
```

링커드가 설치되었는지 확인하려면, 다음 명령을 실행하여 쿠버네티스 리소스를 살펴봅니다.

```
 kubectl get all -n linkerd 
```

아래 스크린샷과 유사한 출력이 표시되어야 합니다.

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/04/641a07df9d93b5365665536f_Screenshot-2023-03-21-at-3.38.58-PM.png" alt=""><figcaption></figcaption></figure>

배포가 완료되면 링커드 사이드카를 Sock Shop 네임스페이스에 가져올 방법이 필요합니다. 이 경우 가장 쉬운 방법은 `inject`(인젝트) 명령을 통해 모든 컨테이너화된 디플로이먼트에 사이드카를 주입하는 것입니다.

```
 kubectl get deploy -o yaml -n sock-shop | linkerd inject - | kubectl apply -f - 
```

그 후에 Sock Shop 파드를 재시작해야 합니다:

```
 kubectl rollout restart -n sock-shop deployments 
```

### 링커드로 앱 확인하기

마지막 단계는 링커드 UI(User Interface)의 애플리케이션 설치 섹션에서 배포한 마이크로서비스를 보는 것입니다. 먼저, Viz 대시보드를 설치합니다.

그런 다음, 로컬에서 Viz 대시보드를 실행합니다.

```
 linkerd viz dashboard & 
```

Viz 대비보드는 자동으로 웹브라우저 상에 표시됩니다.

네임스페이스를 Sock Shop 네임스페이스로 변경하면, 이제 Sock Shop 네임스페이스에서 실행되는 컨테이너화된 애플리케이션이 링커드로 보호(Secured)되는 것을 볼 수 있습니다.

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/04/641a08ba96d64d2b31dfdb09_Screenshot-2023-03-21-at-3.42.39-PM.png" alt=""><figcaption></figcaption></figure>

이 글을 따라가셨다면 링커드 설치가 얼마나 쉬운지 알게 되셨을 것입니다. Kubernetes를 처음 사용하더라도 서비스 메시가 제공하는 보안, 안정성 및 통합 가시성 기능을 놓칠 이유가 없습니다. 링커드의 제로 구성 접근 방식은 서비스 메시를 매우 쉽게 채택할 수 있게 해줍니다. 그리고 [링커드 슬랙](https://slack.linkerd.io/)(Slack)에 가입하는 것을 잊지 마세요. 커뮤니티 멤버와 유지 관리자가 어떤 질문에도 기꺼이 답변해 드립니다.
