---
description: https://istio.io/latest/blog/2022/introducing-ambient-mesh/
---

# 이스티오(Istio)의 앰비언트 메시 소개

2022년 9월 7일  \
작성자: John Howard - Google, Ethan J. Jackson - Google, Yuval Kohavi - Solo.io, Idit Levine - Solo.io, Justin Pettit - Google, Lin Sun - Solo.io \


{% hint style="info" %}
이 문서는 일부 의역이 들어가 있음으로 보다 정확한 이해를 위해서는 원문을 보는 것도 좋습니다.
{% endhint %}



오늘 우리는 기존 모드보다 단순하게 동작하며, 더 광범위한 애플리케이션 호환성 및 인프라 비용 절감을 위해 설계된 새로운 이스티오(Istio) 데이터 플레인 모드인 "앰비언트 메시(Ambient mesh)"를 소개하게 되어 기쁩니다. 앰비언트 메시는 제로 트러스트(zero-trust) 보안, 텔레메트리(telemetry / 예: jaeger) 그리고 트래픽 관리라는 Istio의 핵심 기능을 유지하면서 인프라에 통합된 서비스 메시 데이터 플레인의 사이드카(Sidecar / 예: Envoy) 프록시를  더이상 사용하지 않을 수 있는 옵션을 사용자에게 제공합니다. 우리는 앞으로 몇 달 안에 실 환경에서 적용할 수 있는 수준으로 만들기 위해 이스티오 커뮤니티에 앰비언트 메시의 개발(Preview) 버전을 공유하고 있습니다.

## 이스티오와 사이드카(Istio and sidecars)

이스티오의 아키택처는 처음부터 애플리케이션 컨테이너가 배포되어지면, 그에 따라 프로그래밍 가능한 프록시인 _사이드카_ (Side car)를 함께 배포되도록 설계되었습니다.  사이드카를 사용하면 실제 애플리케이션에는 아무런 영향을 주지 않고, 이스티오에서 제공하는 다양한 이점을 얻을 수 있습니다.&#x20;

<div align="center">

<figure><img src="../../../../.gitbook/assets/image (6).png" alt=""><figcaption><p>전통적인 이스티오 모델은 엔보이(Envoy) 프록시를 사이드카 형태로 파드 내부에 추가 배포함</p></figcaption></figure>

</div>

사이드카는 애플리케이션을 다시 설계(Refactoring, 리팩토링)하는 것보다 상당한 이점들이 있지만, 애플리케이션과 이스티오 데이터 플레인을 완벽하게 분리하는 기능을 제공하지는 못합니다. 이러한 이유로 사이드카를 사용하는 구성은 다음과 같은 제한 사항들이 발생합니다.&#x20;

* **침입성** - 사이드카는 파드의 스펙 명세를 수정하고 파드의 트래픽을 리디렉션하기 위해 애플리케이션에 "주입(Injected)"되어야 합니다. 결과적으로 사이드카를 설치하거나 업그레이드하려면 파드를 다시 시작해야 하며, 그에 따라 워크로드가 사이드카를 포함해서 새로 구성되는 동안은 원할한 동작을 하기 어려움을 의미합니다. &#x20;
* **리소스 활용률이 떨어짐** - 사이드카 프록시는 워크로드에 직접적으로 종속되어 있기 때문에, CPU와 메모리 리소스는 각 파드의 가장 많은 사용량에 대비하여 설계 및 배포되어 있어야 합니다. 이와 같이 쿠버네티스 클러스터에 많은 예약(Reservation) 리소스를 할당하는 경우 리소스의 실제 활용률은 매우 감소하게 됩니다.&#x20;
* **의도하지 않은 트래픽 중단** - 일반적으로 이스티오 사이드카에서 수행되는 트래픽 캡처와 HTTP 처리는 계산 비용이 많이 들고, 이스티오에서 처리된 HTTP의 경우 일부 애플리케이션에서는 이를 비규격으로 판단하여 트래픽이 정상적으로 처리되지 않을 가능성이 있습니다.&#x20;

위와 같은 이유에도 불구하고 사이드카를 통한 이스티오 구성은 이미 보편화 되었지만(자세한 내용은 나중에 설명) 우리는 좀 더 덜 침입하는 구성과 사용하기 쉬운 옵션을 통해 많은 서비스 메시 사용자들이 이를 원할하게 사용하기를 원합니다. &#x20;

## 레이어를 나누기(Slicing the layers)

전통적으로 이스티오는 기본적인 암호화 기능부터 고급 L7 정책 기능까지 모든 데이터 플레인의 기능들을 사이드카라는 단일 아키텍처 구성으로 구현하였습니다. 사실상 이와 같은 단일 아키택처 구성 상에서는 사이드카를 통해서 모두 구현하거나, 전혀 구현하지 않거나 하는 선택지 만을 제공할 수 밖에 없습니다. 예를 들어 만약 워크로드에 단순히 전송 보안(예: L4 by OSI L7 layer)만 필요한 경우라고 해도, 관리자는 여전히 사이드카를 배포하고 이를 관리해야 하는 부담을 지게 됩니다. 사이드카들은 워크로드마다 고정적인 운영 비용이 발생하기 때문에 복잡한 유스케이스(use case)에 따라 적절하게 변경하여 조절하기가 어렵습니다.

그에 반해 앰비언트 메시는 다른 접근 방식을 취합니다. 앰비언트 메시 모드에서는 이스티오의 데이터 플레인 기능을 2가지 레이어로 구분하여 나누었습니다. 기본적으로 앰베언트 메시에서는 트래픽에 대한 라우팅과 제로 트러스트 보안을 담당하는 Secure Overlay Layer가 있습니다.  만약 사용자들이 이스티오의 전체 기능이 필요한 경우에는 L7 Processing Layer를 활성화하여 사용할 수 있습니다. L7 처리(Processing) 모드는 Secure Overylay 모드 보다 무겁지만, 여전히 앰비언트 구성요소로서 동작하므로 전통적인 이스티오의 사이드카 형태와 마찬가지로 애플리케이션 파드의 수정이 필요하지 않습니다.  &#x20;

<figure><img src="../../../../.gitbook/assets/image (13).png" alt=""><figcaption><p>앰비언트 메시의 레이어(Layers)</p></figcaption></figure>

이 레이어로 구분된 구성은 사용자들로 하여금 (필요에 따라) 네임스페이스 별로 이스티오를 사용하지 않는 환경으로 부터 Secure Oveylay 환경 그리고 L7 Processing 환경으로 단계를 거쳐 원할하게 전환할 수 있도록 해줍니다. 더 나아가 앰비언트 메시와 사이드카가 혼합되어 동작하는 워크로드의 경우에도 원활하게 상호 운용되므로 사용자들은 시간에 따라 요구되는 다양한 요구들은 잘 섞어서 구성할 수 있습니다.  &#x20;

## 앰비언트 메시 구축

앰비언트 메시는 쿠버네티스 클러스터의 각 노드에 배포되어 실행되는 공유 에이전트(데몬셋 / DaemonSet)를 사용합니다. 이 에이전트는 제로 트러스트 터널(또는 _**ztunnel**_ )이며, 이 것이 하는 일은 앰비언트 메시 내의 요소들을 인증하고 안전하게 연결하는 것입니다. 노드의 네트워킹 스택은 설정된 모든 워크로드의 트래픽을 로컬 ztunnel 에이전트를 통해 리다이렉션합니다. 이와 같이 완전히 분리된 구성은 이스티오의 데이터 플레인에서 발생할 수 있는 문제가 애플리케이션에 영향을 주지 않는 형태가 되어, 궁극적으로 데이터 플레인의 활성화, 비활성화, 확장 그리고 업그레이드 상황에서도 애플리케이션이 영향을 받지 않는 구성을 지원할 수 있게 됩니다. ztunnel은 워크로드 트래픽에 대해 L7 Processing을 수행하지 않으므로 사이드카보다 훨씬 가볍게 동작할 수 있습니다. 이와 같은 구성을 통해서 감소한 복잡도와 리소스 비용을 본다면 현재 엠비언트 메시 구성 형태인 공유 에이전트 구성을 선택하는데 이의를 제기할 일은 없을 것입니다.&#x20;

Ztunnels은 서비스 메시의 핵심 기능인 제로 트러스트를 활성화합니다. Secure overlay는 네임스페이스에 앰비언트 설정이 활성화 되면 생성됩니다. Secure overlay는 HTTP를 중단하거나 구문 분석(Parsing)하지 않고, 워크로드의 mTLS, 텔레메트리, 인증(authentication) 그리고  L4 인가(authorization)가 포함된 워크로드를 제공합니다.

<figure><img src="../../../../.gitbook/assets/image (3).png" alt=""><figcaption><p>앰비언트 메시는 노드 별로 구성된 ztunnel을 사용하여 제로 트러스트 Secure Overlay를 제공함</p></figcaption></figure>

\
앰비언트 메시가 활성화되고 보안 오버레이가 생성되면, 네임스페이스에 L7 기능들을 사용할 수 있도록 설정할 수 있습니다. 이와 같은 설정을 통해서 네임스페이스는  [가상 서비스(Virtual Service) API](https://istio.io/latest/docs/reference/config/networking/virtual-service/) , [L7 텔레메트리](https://istio.io/latest/docs/reference/config/telemetry/) 그리고 [L7 인증 정책(Authorization Policy)](https://istio.io/latest/docs/reference/config/security/authorization-policy/) 을 포함한 이스티오의 모든 사용 가능한 옵션들을 사용할 있게 됩니다. 이 모드에서 동작하는 네임스페이스는 워크로드의 L7 처리(Processing)을 위해 하나 이상의 엔보이(Envoy) 기반의 _웨이포인트 프록시(waypoint proxies)_를 사용합니다.  이스티오의 컨트롤 플레인(control plane)은 웨이포인트 프록시를 통해 L7 기반의 처리가 필요한 모든 트래픽을 보내기 위해서 ztunnels을 구성합니다. 여기서 중요한 것은 쿠버네티스 관점에서 웨이포인트 프록시는 특수한 형태가 아니라,  디플로이먼트 형태로 배포되어 다른 일반 파드와 마찬가지로 단지 자동으로 확장되는 기능만을 가지고 있다는 점입니다. 웨이포인트 프록시는 상상 그 이상의 부하를 생성해 운영자를 괴롭히는게 아니라 실시간으로 요청되는 트래픽의 요청에 따라 적절하게 자동으로 확장하여 운영되기 때문에 상당한 리소스를 절약할 수 있을 것으로 기대하고 있습니다.&#x20;

<figure><img src="../../../../.gitbook/assets/image (19).png" alt=""><figcaption><p>추가 기능이 필요한 경우 앰비언트 메시는 기능에 필요한 정책 적용을 위해 ztunnel을 통해 연결된 웨이포인트 프록시를 배포함</p></figcaption></figure>



앰비언트 메시는 mTLS를 이용한 HTTP 연결(CONNECT)를 이용하여 보안 터널(secure tunnel)을 구현하고, 경로에 웨이포인트 포록시를 삽입합니다. 이런 구성 방식을 HBONE(HTTP 기반 오버레이 네트워크 환경)이라고 합니다. HBONE은 일반적인 로드 밸런서 인프라와의 상호 운용성을 가능하게 하면서 자체적으로 TLS보다 깔끔한 트래픽 캡슐화(encapsulation)를 제공합니다. FIPS(Federal Information Processing Standards, 연방 정보 처리 표준)를 준수하는 빌드는 기본 설정만으로 컴프라이언스(complliance, 통상 법규준수/준법감시/내부통제)를 충족합니다. HBONE에 대한 좀 더 자세한 내용 및 표준을 기반으로 하는 접근 방법 그리고 UDP와 TCP가 아닌 프로토콜에 대해서와 같은 여러가지 주제에 대해서는 향후 블로그를 통해서 제공될 예정입니다.&#x20;

단일 서비스 메시에서 사이드카들과 앰비언트 모드를 혼합해도 사용하는 경우에도 특별한 기능이나 보안적인 제한은 없습니다.  이스티오 컨트롤 플레인은 선택한 배포 모델에 관계없이 적절하게 정책이 적용되도록 합니다. 앰비언트 메시는 운영자가 더 쉽게 일할 수 있게 하고, 더 많은 유연성을 제공하도록 것과 같은 사용자의 편의성을 높이는 것을 집중하여 설계되었습니다. [\
](https://istio.io/latest/blog/2022/introducing-ambient-mesh/ambient-waypoint.png)

## 로컬 노드에서 L7 처리(Processing)를 하지 않는 이유는? <a href="#why-no-l7-processing-on-the-local-node" id="why-no-l7-processing-on-the-local-node"></a>

앰비언트 메시는 노드마다 구성되어 있는 ztunnel 에이전트를 사용하여 서비스 메시의 제로 트러스트를 구현하는 반면, L7 처리는 별도로 배포된 웨이포인트 프록시 파드에서 진행하도록 구현되어 있습니다. 왜? 번거롭게 이와 같이 간접적으로 처리하고 있을까요? 왜? 노드 전체에 공유된 L7 프록시를 사용하지 않을까요?  이에 대한 몇 가지 이유가 있습니다.

* 엔보이(Envoy)는 본질적으로 다중 테넌트(multi-tenant) 구조가 아닙니다. 그렇기 때문에 공유 인스턴스에서 제약이 없는 여러 테넌트의 L7 트래픽에 대한 복잡한 처리 규칙들을 혼합하게 되는 경우 보안적인 문제를 야기합니다.  L4 처리는 엄격하게 제한함으로써 취약점이 발생할 지점을 매우 크게 줄입 수 있습니다.&#x20;
* ztunnel에서 제공하는 mTLS 및 L4 기능은 웨이포인트 프록시에서 필요로 하는 L7 처리와 비교할 때 훨씬 작은 CPU와 메모리 공간을 필요로 합니다.  웨이포인트 프록시를 공유 네임스페이스 리소스에서 실행하게 되면, 해당 네임스페이스의 요구 사항에 따라 독립적으로 확장할 수 있으며, 사용하지 않는 테넌트에 배포되어 쓸데 없는 비용이 발생하는 것도 방지할 수 있습니다. &#x20;
* ztunnel의 범위를 줄임으로써 잘 정의된 상호 운용성 수준을 충족하는 다른 보안 터널로 구현된 것으로도 대체 할 수 있습니다.&#x20;



## 그렇다면 추가적인 경로(원문은 hop)가 발생하는 부분은?

앰비언트 메시를 사용하면 웨이포인트 프록시가 서비스를 제공하는 워크로드와 동일한 노드에 반드시 있어야 하는 것은 아닙니다. 언뜻 보기에 이는 성능 문제가 발생할 수 있을 것 같지만, 궁극적으로 이스티오의 현재 사이드카를 통한 구현 형태와 비슷한 지연 시간(latency)을 보일 것입니다.  이후에 성능과 관련한 블로그 게시물에서 더 자세히 논의하겠지만, 지금은 두 가지 사항으로 요약하겠습니다.

* 이스티오 네트워크 지연 시간의 대부분은 사실 네트워크 자체에서 발생하지 않습니다( [최신 클라우드 서비스 제공업체는 매우 빠른 네트워크를 제공하고 있습니다)](https://www.clockwork.io/there-is-no-upside-to-vm-colocation/) . 대신 가장 큰 원인은 이스티오가 구현해야 하는 매우 수준 높은 기능들을 제공해야 하는 L7 처리 부분에 있습니다. 각 연결을 위해 두 개의 L7 처리(각 사이드카당 하나씩)를 필요로 하는 사이드카와 달리, 앰비언트 메시는 이 두 개의 L7처리를 한 번으로 줄였습니다. 대부분의 경우에 추가된 네트워크 홉(hop)에 대한 비용은 L7 처리를 줄인 비용 수준 내에서 처리될 것으로 예상됩니다.&#x20;
* 사용자는 첫 번째 단계로 제로 트러스트 보안 태세를 활성화하기 위해 서비스 메시를 배포한 다음 필요에 따라 선택적으로 L7 기능을 활성화합니다. 앰비언트 메시를 사용하면 필요하지 않을 때 L7 처리 비용을 완전히 우회할 수 있습니다.

\
리소스 오버헤드(overhead)
------------------

전반적으로 우리는 앰비언트 메시가 대부분의 사용자에게 더 적고 더 예측 가능한 리소스 요구 사항을 가질 것으로 기대합니다. ztunnel의 줄어든 요구사항(limted responsibilities)으로 인해, ztunnel을 노드의 공유 리소스로 배포할 수 있습니다. 이와 같은 구성은 기존에 대부분의 사용자에게 필요했던 워크로드당 예약되어지는 사이드카와 관련된 리소스를 크게 줄일 수 있습니다.  게다가 웨이포인트 프록시는 일반 쿠버네티스 파드이므로, 서비스하는 워크로드의 실시간 트래픽 수요에 따라 동적으로 배포하고 확장할 수 있습니다.

반면에 사이드카는 각 워크로드에 대해 최악의 경우를 대비하여 CPU와 메모리를 예약해야 합니다. 이러한 계산은 복잡하기 때문에 실제로 관리자는 과도하게 프로비저닝(over-provision)하는 경향이 있습니다. 이와 같은 높은 리소스 예약(다른 워크로드로 스케줄되지 못하도록 함)으로 인해 사용률이 낮은 노드가 발생합니다. 앰비언트 메시의 낮은 고정 노드당 오버헤드와 동적으로 확장되는 웨이포인트 프록시는 전체적으로 훨씬 적은 리소스 예약을 필요로 하므로 클러스터를 보다 효율적으로 사용할 수 있습니다.



## 보안은 어떨까요?

근본적으로 새로운 아키텍처에는 자연스럽게 보안에 대한 의문들이 따라옵니다. [엠비언트 보안 블로그](https://istio.io/latest/blog/2022/ambient-security/) 에서 자세히  분석 하겠지만  일단 여기서는 요약하여 진행하겠습니다.&#x20;

워크로드와 같은 위치에서 제공되는 사이드카는 결과적으로 하나의 취약성이 다른 하나에 영향을 미칩니다. 앰비언트 메시 모델에서는, 애플리케이션이 손상되더라도 ztunnel과 웨이포인트 프록시는 여전히 손상된 애플리케이션의 트래픽에 대해 엄격한 보안 정책을 시행할 수 있습니다. 뿐만 아니라 엔보이(Envoy)는 세계 최대의 네트워크 사업자가 사용하는 성숙한 품질 테스트(battle-tested)를 거친 소프트웨어라는 점을 감안할 때, 함께 배포되는 애플리케이션보다 덜 취약할 가능성이 높습니다.

ztunnel은 공유 리소스이지만, 현재 실행 중인 노드에 워크로드의 키로만 접근할 수 있습니다. 따라서 영향을 받는 범위(blast radius)는 노드마다 존재하는 키(key)를 이용하여 암호화하는 CNI(Container Network Interface) 방식보다 나쁘지 않습니다. 또한 ztunnel의 L4로 한정된 공격 범위와 엔보이(Envoy)의 앞서 언급한 성숙한 소프트웨어라는 점을 고려할 때 이러한 리스크는 제한적으로 발생할 것이고 허용 가능한 수준이라고 생각합니다.

마지막으로 웨이포인트 프록시는 공유 리소스이지만, 하나의 서비스 계정으로만 제공하도록 제한되어 있습니다. 이것은 오늘날의 사이드카보다 나쁜 상황을 만들지 않습니다. 만약에 1개의 웨이포인트 프록시가 손상된다면, 웨이포인트 프록시와 연관된 자격 증명은 더이상 사용할 수 없겠지만, 그 외에 다른 일들은 발생하지 않을 것입니다.&#x20;



## 사이드카는 이제 끝인가요?

확실히 아니라고 말할 수 있습니다. 우리는 앰비언트 메시가 앞으로 많은 서비스 메시 사용자에게 최고의 옵션이 될 것이라고 믿지만, 사이드카는 컴플라이언스 또는 성능 튜닝(Performance turnning)과 같은 전용 데이터 플레인 리소스가 필요한 사용자에게는 꾸준히 좋은 선택지가 될 것입니다. 이스티오는 계속 사이드카를 지원할 것이며, 중요한 것은 사이드카가 앰비언트 메시와 원활하게 상호 운용될 수 있도록 한다는 것입니다. 실제로 오늘 공개하는 앰비언트 메시 코드는 이미 사이드카 기반 이스티오와의 상호 운용성을 지원합니다. &#x20;



## 더 알아보기&#x20;

Christian이 이스티오 앰비언트 메시 구성 요소들을 설명하고 몇 가지 기능을 설명하는 짧은 동영상 데모를 보세요.

{% embed url="https://youtu.be/nupRBh9Iypo" %}

## 참여 정보 &#x20;

오늘 출시한 것은 이스티오의 앰비언트 메시 초기 버전이며, 아직 활발하게 개발 중입니다. 우리는 더 넓은 커뮤니티와 공유하게 되어 기쁘고, 2023년에 프로덕션(production)에 사용할 수 있도록 준비하기 위해 더 많은 사람이 참여하게 되기를 기대합니다.&#x20;

우리는 솔루션을 구성하는 데 도움이 되는 피드백을 매우 환영합니다. 앰비언트 메시를 지원하는 이스티오 빌드는 [Istio Experimental repo](https://github.com/istio/istio/tree/experimental-ambient)에서 내려받아서 사용해 볼 수 있고, 이와 관련한 [문서는 여기](https://istio.io/latest/blog/2022/get-started-ambient/)에 있습니다. 누락된 기능 및 작업 항목 목록은 [README](https://github.com/istio/istio/blob/experimental-ambient/README.md) 에서 확인할 수 있습니다 . 사용해 보시고 의견을 [알려주세요!](https://slack.istio.io/)

_앰비언트 메시 출시에 기여한 팀에 감사드립니다!_

* _Google: Craig Box, John Howard, Ethan J. Jackson, Abhi Joglekar, Steven Landow, Oliver Liu, Justin Pettit, Doug Reid, Louis Ryan, Kuat Yessenov, Francis Zhou_
* _Solo.io: Aaron Birkland, Kevin Dorosh, Greg Hanson, Daniel Hawton, Denis Jannot, Yuval Kohavi, Idit Levine, Yossi Mesika, Neeraj Poddar, Nina Polshakova, Christian Posta, Lin Sun, Eitan Yarmush_

\
