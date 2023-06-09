---
description: >-
  https://www.cncf.io/blog/2023/03/29/distributed-tracing-in-kubernetes-apps-what-you-need-to-know/
cover: ../../../.gitbook/assets/CNCF-Ambassador.jpg
coverY: 0
---

# 쿠버네티스 앱의 분산 추적에 관해서 지금 알면 좋은 사항(2023.03.29)

> **한줄 요약:** Zipkin, Jaeger 그리고 Tempo와 같은 앱의 분산 추적이 왜 필요한지에 대해서 설명하고 있습니다.
>
> **관련 프로젝트:** \
> &#x20;\- [https://zipkin.io/](https://zipkin.io/)\
> &#x20;\- [https://www.jaegertracing.io/](https://www.jaegertracing.io/)\
> &#x20;\- [https://grafana.com/oss/tempo](https://grafana.com/oss/tempo)

{% hint style="info" %}
이 문서는 기계 번역 후에 일부 내용만 다듬었으므로, 보다 정확한 이해를 위해서는 원문을 보는 것이 좋습니다.
{% endhint %}

_게시일: 2023년 3월 29일  (_[_Link_](https://www.cncf.io/blog/2023/03/29/distributed-tracing-in-kubernetes-apps-what-you-need-to-know/)_)_

이 게스트 포스트는 원래 Grafana Labs 블로그에 게시되었습니다.



Kubernetes는 기업이 소프트웨어 배포를 자동화하고 클라우드에서 대규모로 애플리케이션을 관리하기 쉽게 해줍니다. 그러나 클라우드 네이티브 앱을 배포해 본 적이 있다면, 이를 건강하고 예측 가능하게 유지하는 것이 얼마나 어려운지 잘 알고 계실 것입니다.

DevOps 팀과 SRE는 애플리케이션 상태와 성능에 대해 알아보는 데 필요한 인사이트를 얻기 위해 분산 추적을 사용하는 경우가 많습니다. 추적을 사용하면 서비스에서 추출한 신호에 식별자 집합을 사용하여 트랜잭션을 재구성하는 데 활용할 수 있습니다. 이를 통해 사용자의 문제 해결 여정에 정보를 제공하고 속도를 높일 수 있습니다.

이 문서에서는 Kubernetes 워크로드 모니터링을 위한 솔루션으로서 분산 추적에 대해 중점적으로 설명합니다. 분산 추적의 중요성과 분산 추적의 작동 방식에 대해 자세히 알아보고 Kubernetes 프로젝트에서 이를 구현할 수 있습니다.



## Kubernetes에서 분산 추적이란 무엇인가요?&#x20;

분산 추적은 Google이 2010년에 [Dapper 백서](https://research.google/pubs/pub36356/)에 발표한 개념을 기반으로 합니다. 이 개념은 각 서비스가 수행한 작업에 대한 정보를 방출하도록 하여 서비스 집합이 서로 상호 작용하는 방식을 암시적으로 관찰하고 공통 식별자를 사용하여 이러한 신호를 보강하는 것입니다.

이러한 단일 신호를 "스팬"(Span)이라고 부르며 일반적으로 설명적인 이름과 고유 식별자를 갖습니다. 단일 스팬은 n개의 스팬으로 구성된 '추적'의 일부입니다. 모든 스팬은 무제한의 자식을 가질 수 있습니다.

이 데이터 모델은 추적 ID가 보존되고 새 스팬이 이 ID를 갖는 한 추적을 계속할 수 있도록 보장합니다. 이 메커니즘이 작동하려면 모든 서비스가 이 상관관계 정보를 다음 다운스트림 서비스로 전파해야 합니다.

Kubernetes의 맥락에서, 각 애플리케이션 서비스 또는 플랫폼의 구성 요소를 이러한 스팬의 가능한 생성자로 생각하면 됩니다. 그런 다음 애플리케이션의 성능을 이해하려면 해당 구성 요소에 의해 생성된 스팬의 추적을 따라가며 기간 또는 Kubernetes 관련 속성과 같은 메타데이터를 살펴보세요.



\>>> Dapper 백서 추가 ([Link](https://research.google/pubs/pub36356/))

**대규모 분산 시스템 추적 인프라, Dapper**

최신 인터넷 서비스는 복잡한 대규모 분산 시스템으로 구현되는 경우가 많습니다. 이러한 애플리케이션은 서로 다른 팀에서 서로 다른 프로그래밍 언어로 개발한 소프트웨어 모듈 모음으로 구성되며, 여러 물리적 시설에 걸쳐 수천 대의 컴퓨터에 걸쳐 있을 수 있습니다. 이러한 환경에서는 시스템 동작을 이해하고 성능 문제를 추론하는 데 도움이 되는 도구가 매우 중요합니다.

이 글에서는 Google의 프로덕션 분산 시스템 추적 인프라인 Dapper의 설계를 소개하고, 낮은 오버헤드, 애플리케이션 수준의 투명성, 대규모 시스템에서의 유비쿼터스 배포라는 설계 목표를 어떻게 달성했는지 설명합니다. Dapper는 다른 추적 시스템, 특히 Magpie \[3] 및 X-Trace \[12]와 개념적 유사성을 공유하지만, 샘플링을 사용하고 계측을 다소 적은 수의 공통 라이브러리로 제한하는 등 우리 환경에서 성공의 핵심이 된 특정 설계 선택이 이루어졌습니다.

이 백서의 주요 목표는 2년 넘게 시스템을 구축, 배포 및 사용한 경험을 보고하는 것입니다. Dapper의 가장 큰 성공 척도는 개발자 및 운영 팀에 대한 유용성이었기 때문입니다. Dapper는 독립형 추적 도구로 시작했지만, 설계자가 예상하지 못했던 다양한 도구를 만들 수 있는 모니터링 플랫폼으로 발전했습니다. 이 글에서는 Dapper를 사용하여 구축된 몇 가지 분석 도구에 대해 설명하고, Google 내 사용 통계를 공유하며, 몇 가지 사용 사례를 제시하고, 지금까지 얻은 교훈에 대해 논의합니다.

\
_분산 추적과 로깅  (_Distributed tracing vs. logging)
----------------------------------------------

_오늘날에도 Kubernetes 메트릭, 로그, 추적의 차이점에 대해 혼란스러워하는 사람들이 있습니다. 이는 클러스터의 통합 가시성 전략의 기본적인 부분이기 때문에 이해할 수 있습니다._

_간단히 말해서, Kubernetes 로그는 마이크로서비스 내에서 발생한 이벤트에 대한 인사이트를 제공합니다. 이 정보는 문제 해결 중에 특히 유용합니다. 분산 추적은 마이크로서비스가 서로 상호 작용할 때 마이크로서비스 간에 발생하는 일과 나머지 분산 시스템과의 관계에 대한 귀중한 데이터를 제공함으로써 다른 각도에서 다룹니다._

_기본적으로 로그와 추적은 서로를 보완하여 데브옵스 팀에게 Kubernetes 에코시스템에서 실행 중인 요청, 서비스, 프로세스 간의 상호 작용에 대한 통합 가시성을 한눈에 파악할 수 있는 인사이트를 제공합니다._



_>>> 역자 추가: Kiali 대시보드에서 제공하는 Jaeger 트레이싱_&#x20;

<figure><img src="../../../.gitbook/assets/Screen Shot 2023-04-03 at 10.58.32 AM.png" alt=""><figcaption><p>Kiali 대시보드의 Traces 메뉴</p></figcaption></figure>

## 고유 요청을 식별하는 방법

고유 식별자(추적 ID)를 사용하여 특정 요청을 모니터링하면 수백 개의 다른 서비스와 상호 작용하는 동안 해당 요청이 어떻게 수행되는지 알 수 있습니다. 따라서 분산 추적은 클러스터 전체에 대한 Kubernetes 통합 가시성 전략의 중요한 부분입니다. 분산 추적은 이미 복잡한 클라우드 인프라에서 애플리케이션을 구축하는 느슨하게 결합된 서비스 모음에 대한 인사이트를 얻을 수 있는 가장 신뢰할 수 있는 방법입니다.

분산 추적은 단순해 보일 수 있지만 실제로는 Kubernetes 클러스터에 대한 귀중한 정보를 제공합니다. 다음은 잠재적인 이점에 대한 몇 가지 예입니다.

* **엔드투엔드 가시성.** 개발자와 운영자는 분산 추적 도구를 사용하여 사용자 요청과 다른 마이크로서비스의 상호 작용을 추적합니다. GPS 내비게이션과 좌표를 활용하여 DevOps 팀은 지도의 모든 지점을 되돌아봄으로써 성능, 처리량, 기능 및 인프라 병목 현상을 쉽게 추적할 수 있습니다.&#x20;
* **서비스 종속성.** 분산 추적을 사용하여 서로 다른 서비스 간의 종속성 및 상호 작용을 발견해야 합니다.&#x20;
* **이상 징후 탐지.** 정상 추적을 참조로 사용하여 애플리케이션 성능에 영향을 미치는 이상 징후를 쉽게 감지할 수 있습니다.&#x20;
* **향상된 애플리케이션 디버깅.** 분산 추적 도구를 클러스터 로그 및 메트릭과 함께 사용하여 애플리케이션 디버깅 및 문제 해결에 활용하세요.



## 분산 추적이 작동하는 방식&#x20;

마이크로서비스 기반 Kubernetes 애플리케이션 내에서 무슨 일이 일어나고 있는지 이해하려면 먼저 해당 애플리케이션 내의 컨테이너 상호 작용에 대한 컨텍스트가 필요합니다. 포드 내의 컨테이너는

* 서로 상호 작용
* 같은 노드 내 또는 다른 노드에 있는 다른 파드와 통신하기
* 외부 세계와 대화하거나 외부 리소스(예: 스토리지)에 도달할 수 있습니다.
* 멀티클라우드 배포에서 한 클러스터에서 다른 클러스터로 앱 요청 전달

분산 추적은 복잡한 상호 작용 시스템을 통해 요청의 경로를 따라갑니다. 컨테이너, 포드, 노드, 심지어 클러스터 내부 또는 외부에서 트리거된 마이크로서비스 또는 작업의 순서와 타이밍을 제공합니다. 아래의 원하는 결과를 보면 어떻게 수행되는지 알 수 있습니다:

<figure><img src="https://grafana.com/media/blog/Kubernetes-traces/K8s-Traces-diagram.jpg" alt=""><figcaption></figcaption></figure>

위 이미지는 분산 시스템을 단순화한 것으로, 각 마이크로서비스에는 API가 있고, 각 마이크로서비스는 자체 포드에 있으며, 하나의 데이터베이스는 외부에 있습니다. 각 선은 상호 작용을 나타냅니다. 여기서 사용되는 다른 개념으로는 스팬과 추적 컨텍스트(예: 추적에는 무엇이 포함되나요?)가 있습니다. 다음은 스팬과 추적 컨텍스트에 대한 몇 가지 추가 배경 정보입니다:

**스팬.** 스팬은 고유한 이름과 타임스탬프가 있는 작업 또는 상호 작용과 같은 단일 작업 단위로 생각하면 됩니다. 추적은 하나 또는 여러 개의 스팬으로 구성될 수 있습니다. 위에 표시된 예에서 상위 스팬은 원래 사용자 요청을 나타내며, 각 후속 스팬은 분산 시스템 내에서 고유한 작업입니다.&#x20;

**컨텍스트 추적.** W3C(월드와이드웹 컨소시엄)에 따르면 추적 컨텍스트는 "서비스 간에 컨텍스트 정보를 전송하고 수정하는 방법을 표준화하는" 사양입니다. 이 사양은 각 개별 요청에 고유 식별자를 제공하므로 동일한 트랜잭션에 다른 추적 도구가 사용되더라도 공급업체별 메타데이터를 추적할 수 있습니다. 개발자와 관리자는 분산 추적 도구를 사용하여 각 추적을 추적 컨텍스트 사양을 사용하여 전파가 기록되는 범위로 나누어 사용자 요청이 다른 서비스와 상호 작용할 때의 성능에 대한 인사이트를 얻을 수 있습니다. 이 접근 방식은 간단하지만 구현에는 다양한 구성 요소가 필요하며, 이에 대해서는 아래에서 설명합니다.



## 분산 추적 툴킷의 주요 구성 요소

분산 추적 도구는 유사하게 작동하며 일반적으로 동일한 구성 요소를 사용합니다. 그러나 구현 방식, 아키텍처, 심지어 사용되는 추적 프로토콜에도 차이가 있을 수 있습니다. 이 가이드에서는 Grafana Tempo를 예로 들어 설명하겠습니다.

<figure><img src="https://grafana.com/media/blog/Kubernetes-traces/Tempo-work.png" alt=""><figcaption></figcaption></figure>

대부분의 분산 추적 에코시스템과 마찬가지로, 그라파나 템포는 네 가지 구성 요소에 의존하여 유용하게 사용할 수 있습니다:

1. 계측. 간단히 말해, 이 구성 요소는 나중에 수집할 텔레메트리 데이터(이 경우 추적)를 제공하지만 다른 도구에서 사용하는 메트릭과 로그도 생성할 수 있습니다. 애플리케이션과 서비스에 대한 원격 분석 데이터를 수집하는 데는 여러 가지 표준이 있으며, 이에 대해서는 나중에 다룰 예정입니다. Grafana Tempo는 텔레메트리 데이터 수집을 위해 OpenTelemetry(OTLP를 통해), Zipkin, Jaeger 프로토콜을 지원합니다. 개발자가 애플리케이션 코드에서 추적을 생성할 수 있는 라이브러리와 프레임워크가 있지만, v1.22부터 Kubernetes 시스템 구성 요소도 OpenTelemetry 프로토콜을 사용하여 추적을 전송할 수 있습니다.
2. 파이프라인. 분산 추적 도구는 일반적으로 여러 소스에서 동시에 텔레메트리 데이터를 수신하므로 애플리케이션 확장에 따라 도구에서 사용하는 리소스를 최적화해야 합니다. 이를 위해 추적 파이프라인은 데이터 버퍼링, 추적 일괄 처리, 스토리지 백엔드로의 후속 라우팅과 같은 기능을 수행합니다. 이 중요한 작업을 위해 Grafana Tempo는 Grafana 에코시스템에서 메트릭과 로그를 스크래핑하는 데 사용되는 것과 동일한 텔레메트리 수집기인 Grafana 에이전트를 사용할 수 있습니다.
3. 스토리지 백엔드. 추적이 수집된 후에는 나중에 시각화 및 분석을 위해 저장해야 합니다. 이 과정에서 너무 많은 리소스를 소비하지 않고 애플리케이션과 함께 확장할 수 있는 스토리지 백엔드가 필요합니다. 그라파나 템포는 혁신적인 접근 방식을 취합니다: 리소스를 많이 사용하는 인메모리 데이터베이스에 의존하는 대신 Amazon S3, Google Cloud Storage, Microsoft Azure Blob Storage와 같은 널리 사용되는 객체 스토리지 솔루션을 추적 스토리지 백엔드로 사용합니다. 필요한 경우, 멤캐시드나 레디스를 사용해 쿼리 성능을 최적화할 수 있습니다.
4. 시각화 레이어. Tempo는 시각화를 위해 Grafana와 결합하여 Kubernetes에서 메트릭, 로그, 추적을 상호 연관시키는 동적 대시보드를 생성할 수 있습니다.



## _오픈 소스 분산 추적 표준_

_대부분의 도구가 곧 단일 분산 추적 표준을 사용할 가능성이 높지만, 오픈 소스 추적 프로토콜의 현재 상태를 알아두는 것이 좋습니다:_

* _**OpenTracing.** 오래되었지만 여전히 사용 가능한_ [_OpenTracing_](https://opentracing.io/)_은 분산 추적을 위한 벤더 중립적인 계측 API입니다. Zipkin에서 영감을 얻은 OpenTracing API는 Go, JavaScript, Java, Python, Ruby, PHP와 같은 언어로 제공되는 클라이언트 라이브러리 덕분에 플랫폼 간 호환성을 제공합니다. 이 표준을 사용하는 일부 도구로는 Zipkin, Jaeger, Datadog 등이 있습니다._
* _**OpenCensus.** 문서에 따르면,_ [_OpenCensus_](https://opencensus.io/)_는 "서비스에서 추적 및 지표를 자동으로 캡처하는 데 사용되는 Census라는 라이브러리 집합을 사용하는 Google에서 시작되었습니다."라고 설명합니다. OpenTracing과 유사하게, OpenCensus는 Go, Java, C#, Node.js, C++, Ruby, Erlang/Elixir, Python, Scala, PHP 등 여러 언어로 된 라이브러리를 보유하고 있습니다. OpenCensus를 지원하는 백엔드에는 Azure Monitor, Datadog, Instana, Jaeger, New Relic, SignalFx, Google Cloud Monitoring, Zipkin 등이 있습니다. OpenCensus와 OpenTracing은 2019년에 OpenTelemetry로 통합되었지만, 사용자 지정 모니터링 도구가 있는 회사에서는 여전히 별도로 사용되고 있습니다._
* _**OpenTelemetry.**_ [_OpenTelemetry_](https://opentelemetry.io/)_는 OpenTracing과 OpenCensus의 최고의 기능을 융합한 도구, API 및 SDK의 모음이라고 생각하면 됩니다. OpenTelemetry 코드 계측은 11개 이상의 프로그래밍 언어로 지원됩니다. 그러나 특히 눈에 띄는 장점은 Java, Node.js, Python으로 개발된 애플리케이션을 쉽게 계측할 수 있는 Kubernetes용 OpenTelemetry Operator입니다. 사실상의 표준으로서 미래가 유망한 OpenTelemetry는 AWS, Microsoft, Elastic, Honeycomb, Grafana Labs와 같은 IT 업계의 저명한 벤더들의 지지를 받고 있습니다._

_OpenTracing과 OpenCensus의 기능이 겹치긴 하지만, OpenTelemetry를 지원하는 도구가 다른 도구보다 우위에 있다고 말할 수 있습니다. Grafana Tempo는 집킨과 예거가 사용하는 오픈텔레메트리와 계측 프로토콜을 모두 기본적으로 지원하는 독보적인 위치에 있습니다. 즉, 애플리케이션 코드를 수정할 필요 없이 조직은 Tempo와 함께 Zipkin 또는 Jaeger 계측을 사용할 수 있습니다._



## 분산 추적 도구

사용 가능한 도구가 너무 많기 때문에, 가장 인기 있는 오픈 소스 분산 추적 솔루션에 대해 자세히 알아볼 필요가 있습니다. 모두 비슷하게 작동하므로 이들 간의 개략적인 비교는 매우 간단합니다.

[Zipkin](https://zipkin.io/)은 분산 추적 솔루션 개발의 선구자 중 하나입니다. 트위터에서 처음 개발하여 Java로 작성된 이 솔루션은 스토리지 백엔드로서 카산드라(Cassandra) 및 엘라스틱서치(Elasticsearch)를 지원할 뿐만 아니라 오픈트레이싱 표준을 지원하는 것이 특징입니다.

[Jaeger](https://www.jaegertracing.io/)는 Zipkin보다 다소 젊은 프로젝트입니다. 처음에는 Uber Technologies에서 만들었으나 나중에 Cloud Native Computing Foundation(CNCF)의 인큐베이터 프로젝트가 되었습니다. Golang으로 작성되었으며, Zipkin과 마찬가지로 스토리지 백엔드로 Cassandra와 Elasticsearch를 지원합니다. 한 가지 주목할 만한 차이점은 오픈트레이싱 표준과 완벽하게 호환된다는 점입니다.

[Grafana Tempo](https://grafana.com/oss/tempo/?pg=blog\&plcmt=body-txt)는 이 목록의 가장 최신 프로젝트입니다. Grafana Labs에서 개발한 Tempo는 처음부터 확장성이 뛰어나고 비용 효율적이며 유연하도록 구축되었습니다. 또한 추적 저장을 위한 오브젝트 스토리지 제공업체를 지원하며 Zipkin, Jaeger, OpenTelemetry 프로토콜과 호환됩니다.

관리형 모니터링 솔루션에 관심이 있으시다면, 넉넉한 무료 티어의 사용자를 포함하여 모든 Grafana Cloud 사용자가 사용할 수 있는 Grafana Cloud의 Kubernetes Monitoring을 확인해 보세요. 아직 Grafana Cloud 계정이 없는 경우, 지금 바로 무료 계정에 가입할 수 있습니다!
