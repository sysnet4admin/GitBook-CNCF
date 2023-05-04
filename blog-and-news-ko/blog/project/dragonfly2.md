---
description: >-
  https://www.cncf.io/blog/2023/04/13/volcano-engine-distributed-image-acceleration-practice-based-on-dragonfly/
cover: ../../../.gitbook/assets/CNCF-Ambassador.jpg
coverY: 0
---

# Volcano Engine: Dragonfly를 통한 효과적인 이미지 배포 가속 방법(2023.04.13)

> **한줄 요약:** Dragonfly와 Nydus를 이용하면, 단순히 이미지를 내려받는 것 보다 효과적으로 이미지를 빠르게 내려받고 적용할 수 있음을 안내하고 있습니다.&#x20;
>
> **Dragonfly:** [https://github.com/dragonflyoss/Dragonfly2](https://github.com/dragonflyoss/Dragonfly2)
>
> **kraken:** [https://github.com/uber/kraken](https://github.com/uber/kraken)
>
> **Nydus:** [https://nydus.dev/](https://nydus.dev/)

{% hint style="info" %}
이 문서는 기계 번역 후에 일부 내용만 다듬었으므로, 보다 정확한 이해를 위해서는 원문을 보는 것이 좋습니다.
{% endhint %}

2023년 4월 13일에 게시됨 ([Link](https://www.cncf.io/blog/2023/04/13/volcano-engine-distributed-image-acceleration-practice-based-on-dragonfly/))

프로젝트 게시물 작성자 Gaius (Dragonfly 메인테이너)

### **용어 및 정의**&#x20;

| 용어                 | 정의                                                                                                                                                                                                                                                               |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| OCI                | 오픈 컨테이너 이니셔티브(OCI, Open Container Initiative)는 운영 체제 수준의 가상화(사실 가장 중요한 건 리눅스 컨테이너)를 위한 개방형 표준을 설계하기 위해 2015년 6월에 Docker가 시작한 리눅스 재단 프로젝트입니다.                                                                                                                     |
| OCI Artifact       | OCI 이미지 스펙을 따르는 제품.                                                                                                                                                                                                                                              |
| Image              | 이 문서의 이미지는 OCI 아티팩트(Artifact)를 참조합니다.                                                                                                                                                                                                                            |
| Image Distribution | 이미지 분배를 위한 제품은 OCI 분배 스펙에 따라 구현되어 있습니다.                                                                                                                                                                                                                          |
| ECS                | CPU, 메모리, 클라우드 드라이브로 구성된 리소스의 집합으로, 각 리소스는 논리적으로 데이터센터 인프라의 컴퓨팅 하드웨어 개체에 해당합니다.                                                                                                                                                                                  |
| CR                 | Volcano Engine 이미지 분배 서비스.                                                                                                                                                                                                                                       |
| VKE                | Volcano Engine은 차세대 클라우드 네이티브 기술을 깊이 통합하여 컨테이너를 핵심으로 하는 고성능 쿠버네티스 컨테이너 클러스터 관리 서비스를 제공하여 사용자가 컨테이너화된 애플리케이션을 빠르게 구축할 수 있도록 지원합니다.                                                                                                                                |
| VCI                | Volcano는 서버리스 및 컨테이너화된 컴퓨팅 서비스입니다. 현재 VCI는 컨테이너 서비스 VKE와 원활하게 통합되어 쿠버네티스 오케스트레이션 기능을 제공하며, VCI를 사용하면 기본 서비스형 클라우드와 같은 인프라를 구매하고 관리할 필요 없이 앱 자체 구축에 집중할 수 있고 컨테이너가 실제로 실행하는 데 소비되는 리소스에 대해서만 비용을 지불하면 됩니다. 또한 VCI는 세컨드 스타트 업, 높은 동시 생성, 샌드박스 컨테이너 보안 격리 등을 지원합니다. |
| TOS                | Volcano Engine은 안전하고 저렴하며 사용하기 쉽고 안정적이며 가용성이 높은 대규모 분산형 클라우드 스토리지 서비스를 제공합니다.                                                                                                                                                                                    |
| Private Zone       | 전용 네트워크 VPC(가상 프라이빗 클라우드) 환경을 기반으로 하는 프라이빗 DNS 서비스입니다. 이 서비스를 통해 프라이빗 도메인 이름을 하나 이상의 사용자 지정 VPC의 IP 주소에 매핑할 수 있습니다.                                                                                                                                              |
| P2P                | P2P 기술은 P2P 네트워크의 피어가 서버에서 데이터를 다운로드할 때 다른 피어가 데이터를 다운로드한 후 다운로드할 수 있는 서버 레벨로도 사용할 수 있습니다. 다수의 노드가 동시에 다운로드하는 경우, 이후 다운로드된 데이터를 서버 측에서 다운로드할 필요가 없도록 보장할 수 있습니다. 따라서 서버 측의 부담을 줄일 수 있습니다.                                                                        |
| Dragonfly          | Dragonfly는 P2P 기술을 기반으로 한 파일 배포 및 이미지 가속 시스템으로, 클라우드 네이티브 아키텍처에서 이미지 가속 분야의 표준 솔루션이자 모범 사례입니다. 현재 클라우드 네이티브 컴퓨팅 재단(CNCF)의 인큐베이션 프로젝트로 호스팅되고 있습니다.                                                                                                                |
| Nydus              | Nydus 가속 프레임워크는 지연 로딩을 통해 컨테이너 이미지 시작을 가속화할 수 있는 콘텐츠 주소 지정이 가능한 파일 시스템을 구현합니다. 매일 수백만 개의 가속화된 이미지 컨테이너 생성을 지원하고 있으며, 리눅스 커널의 erofs 및 fscache와 긴밀하게 통합되어 이미지 가속을 커널 내에서 지원할 수 있게 해줍니다. (역: Dragonfly Container Image Service을 의미)                                 |



## 배경 정보

Volcano Engine 이미지 리포지토리 CR은 컨테이너 이미지를 저장하기 위해 TOS를 사용합니다. 현재 대규모 동시 이미지 풀링의 수요를 어느 정도 충족시킬 수 있습니다. 그러나 풀링의 최종 동시성은 TOS의 대역폭과 QPS에 의해 제한됩니다.

다음은 현재 대규모 이미지 풀링에서 발생하는 두 가지 시나리오에 대해 간략히 소개합니다:

1. 클라이언트 수가 증가하고 이미지가 점점 더 커지고 있습니다. 결국 TOS의 대역폭이 부족해질 것입니다.
2. 클라이언트가 이미지 형식을 변환하기 위해 Nydus를 사용하면 TOS에 대한 요청량이 엄청나게 증가합니다. TOS API의 QPS 제한으로 인해 수요를 충족할 수 없습니다.

이미지 리포지토리 서비스 자체든 기반 스토리지든 결국에는 대역폭과 QPS 제한이 있을 수밖에 없습니다. 서버가 제공하는 대역폭과 QPS에만 의존하면 수요를 충족하지 못하기 쉽습니다. 따라서 서버의 부담을 줄이고 대규모 동시 이미지 풀링 수요를 충족하기 위해서는 P2P를 도입해야 합니다.

**P2P 기술 기반 이미지 배포 시스템 조사**

오픈소스 커뮤니티에는 여러 P2P 프로젝트가 있습니다. 다음은 이러한 프로젝트에 대한 간략한 소개입니다.

### [Dragonfly](https://github.com/dragonflyoss/Dragonfly2)

#### **아키텍처**

<figure><img src="https://lh5.googleusercontent.com/OdfxvRTnv4CMjRUF7Ubqex0P0q0P7gQZk6rfA4bmJj3UctMqjGBqpVhoR7s9ZCi1heKk7nKfASiBQpC9orF-zqAzLFuVTPeaA8WQQcmzyJdFjCxvZ_zVO01WqLA82iiF3cdOdSUDNgtjgMCgpMi6lA" alt=""><figcaption></figcaption></figure>

#### Manager

* 시드 피어 클러스터, 스케줄러 클러스터 및 dfdaemon에서 사용할 동적 구성을 저장합니다.&#x20;
* 시드 피어 클러스터와 스케줄러 클러스터 간의 관계를 유지합니다.&#x20;
* 하버와 결합된 이미지 예열을 위한 비동기 작업 관리 기능을 제공합니다.&#x20;
* 스케줄러 인스턴스와 시드 피어 인스턴스로 킵얼라이브.&#x20;
* dfdaemon을 위한 최적의 스케줄러 클러스터를 필터링합니다.&#x20;
* 사용자가 P2P 클러스터를 관리하는 데 유용한 시각적 콘솔을 제공합니다.

#### Scheduler

* 다양한 기능을 갖춘 지능형 스케줄링 시스템을 기반으로 최적의 부모 피어를 선택합니다.&#x20;
* P2P 클러스터에 대한 스케줄링 방향 비순환 그래프를 구축합니다.&#x20;
* 피어 다중 기능 평가 결과를 기반으로 비정상 피어를 제거합니다.&#x20;
* 스케줄링 실패 시 피어 백투소스 다운로드 알림.

#### Dfdaemon

* 다운로드 기능이 있는 dfget용 gRPC를 제공하고, 다양한 소스 프로토콜에 대한 적응 기능을 제공합니다.&#x20;
* 시드 피어로 사용할 수 있습니다. 시드 피어 모드를 켜면 P2P 클러스터에서 전체 클러스터의 다운로드용 루트 피어인 백투소스 다운로드 피어로 사용할 수 있습니다.&#x20;
* 컨테이너 레지스트리 미러 및 기타 http 백엔드에 대한 프록시를 제공합니다.&#x20;
* http, https 및 기타 사용자 정의 프로토콜을 통해 오브젝트를 다운로드합니다.



### [**Kraken**](https://github.com/uber/kraken)

#### **아키텍처**

<figure><img src="https://lh5.googleusercontent.com/rvrEtK2sUItn0J3EBEUo33s81jhB1jd_1TSQp8SUX-4WkN9LEBMb3CfWb3Uk8As9Ub2LZWaUulA_mAWPs0U7KdDePwj6MZ_XZ8t0dY_weKSs9mwDroWQpfg_W4Qdq4z1jbr1N9KKxlzDAS-xi8Lb4A" alt=""><figcaption></figcaption></figure>

#### Agent

* P2P 네트워크의 피어 노드이며 각 노드에 배포해야 합니다.
* 도커 레지스트리 인터페이스 구현
* 트래커에게 자신이 소유한 데이터를 알립니다.
* 다른 에이전트의 데이터를 다운로드합니다(트래커는 이 데이터를 다운로드할 에이전트를 에이전트에게 알려줍니다).

#### Origin

* 시딩을 위해 스토리지에서 데이터 읽기 담당
* 다양한 스토리지 지원
* 해시 링 형태의 고가용성

#### Tracker

* P2P 네트워크의 코디네이터로, 누가 피어이고 누가 시더인지 추적합니다.
* 피어가 소유한 데이터 추적
* 피어가 데이터를 다운로드할 수 있도록 정렬된 피어 노드 제공
* 해시 링 형태로 고가용성 제공

#### Proxy

* 도커 레지스트리 인터페이스 구현
* 이미지 레이어를 Origin 구성 요소에 전달
* 빌드 인덱스 구성 요소에 태그 전달

#### Build-Index

* 태그와 다이제스트 매핑, 에이전트가 해당 태그 데이터를 다운로드할 때, 빌드-인덱스에서 해당 다이제스트 값을 가져옵니다.
* 클러스터 간 이미지 복제
* 스토리지에 태그 데이터 저장
* 해시 링 형태의 고가용성



### **Dragonfly vs Kraken**

|                | Dragonfly                    | Kraken                             |
| -------------- | ---------------------------- | ---------------------------------- |
| 고가용성           | Scheduler 일관된 해시 링으로 고가용성 지원 | Tracker 일관된 해시 링, 여러 복제본으로 고가용성 보장 |
| containerD 지원  | 지원                           | 지원                                 |
| HTTPS 이미지 저장소  | 지원                           | 지원                                 |
| 커뮤니티 참여 수준     | 활동 중                         | 비 활동 중                             |
| 유저수            | 좀 더 많음                       | 좀 더 적음                             |
| 성숙도            | 높음                           | 높음                                 |
| Nydus와의 최적화 여부 | 최적화되어짐                       | 최적화되어 지지 않음                        |
| 아키택처 복잡도       | 중간 수준                        | 중간 수준                              |

#### 요약

프로젝트의 전반적인 성숙도, 커뮤니티 활성화 수준, 사용자 수, 아키텍처 복잡성, Nydus에 최적화되었는지 여부, 향후 개발 동향 및 기타 요인을 고려할 때 Dragonfly는 P2P 프로젝트에서 최고의 선택입니다.

#### 제안 사항

Volcano 엔진의 경우, VKE와 VCI가 CR을 통해 이미지를 가져온다는 점이 주요 고려 사항입니다.

* VKE의 제품 특징은 ECS를 기반으로 배포된 K8이므로 각 노드에 dfdaemon을 배포하고 각 노드의 대역폭을 최대한 활용하고 P2P의 기능을 충분히 활용하는 데 매우 적합합니다.
* VCI의 제품 특징은 하위 계층에 풍부한 리소스를 가진 일부 가상 노드가 있다는 것입니다. 상위 레이어 서비스는 캐리어로 POD를 기반으로 하기 때문에 VKE처럼 각 노드에 dfdaemon을 배포하는 것이 불가능하므로 캐시 기능을 사용하여 여러 개의 dfdaemon을 캐시로 배포하는 배포 형태입니다.
* VKE 또는 VCI 클라이언트는 Nydus 포맷으로 변환된 이미지를 가져옵니다. 이 시나리오에서는 dfdaemon을 캐시로 사용해야 하며, 스케줄러에 너무 많은 스케줄링 압력을 가하지 않도록 너무 많은 노드를 사용하지 않아야 합니다.

Volcano Engine의 위 제품들에 대한 수요와 Dragonfly의 특성을 바탕으로 여러 요소에 적합한 배포 체계를 설계해야 합니다. Dragonfly의 배포 체계는 다음과 같이 설계되었습니다.



\---

#### 역자 임의 추가 (크라켄 입장도 들어보는게 중요할 것 같아서)

{% embed url="https://uber-kraken.readthedocs.io/en/latest/" %}

\
**Alibaba의 Dragonfly Dragonfly**&#x20;

클러스터에는 클러스터의 모든 4MB 데이터 청크의 전송을 조정하는 하나 또는 몇 개의 "슈퍼노드"가 있습니다.

슈퍼노드가 최적의 결정을 내릴 수 있지만, 전체 클러스터의 처리량은 하나 또는 몇 개의 호스트의 처리 능력에 의해 제한되며, 블롭 크기 또는 클러스터 크기가 증가함에 따라 성능이 선형적으로 저하될 수 있습니다.

크라켄의 트래커는 연결 그래프를 조율하는 데만 도움을 주고 실제 데이터 전송 협상은 개별 피어에게 맡기기 때문에 크라켄은 큰 블롭(blobs)에 더 잘 확장됩니다. 또한, 크라켄은 안정적인 하이브리드 클라우드 설정에 필요한 고가용성(HA) 및 교차 클러스터 복제를 지원합니다.

**BitTorrent**&#x20;

크라켄은 처음에 비트토렌트 드라이버로 구축되었지만, 스토리지 솔루션과의 긴밀한 통합과 성능 최적화에 대한 더 많은 제어를 위해 비트토렌트 프로토콜을 기반으로 하는 자체 P2P 드라이버를 구현하게 되었습니다.

크라켄의 문제 공간은 비트토렌트가 설계한 것과는 약간 다릅니다. 크라켄의 목표는 안정적인 환경에서 글로벌 최대 다운로드 시간과 통신 오버헤드를 줄이는 것이지만, 비트토렌트는 예측할 수 없는 적대적인 환경을 위해 설계되었기 때문에 희소한 데이터의 사본을 더 많이 보존하고 악의적이거나 나쁜 행동을 하는 피어를 방어해야 합니다.

이러한 차이점에도 불구하고 저희는 크라켄의 프로토콜을 수시로 재검토하고 있으며, 가능하다면 비트토렌트와 다시 호환되도록 만들 수 있기를 희망합니다.



**제한 사항**&#x20;

도커 레지스트리 처리량이 배포 워크플로우의 병목 현상이 아닌 경우, 크라켄으로 전환한다고 해서 도커 풀의 속도가 마법처럼 빨라지지는 않습니다. 실제로 도커 풀의 속도를 높이려면 \*Makisu로 전환하여 빌드 시 레이어 재사용성을 개선하거나, 도커 풀이 데이터 압축 해제에 대부분의 시간을 소비하므로 압축 비율을 조정하는 것을 고려하세요. 태그 변경(예: 최신 태그 업데이트)은 허용되지만 몇 가지 기능이 작동하지 않습니다. 태그 조회 직후에는 Nginx 캐싱으로 인해 여전히 이전 값이 반환되고 복제가 트리거되지 않을 수 있습니다. 이 기능을 더 잘 지원하기 위해 노력 중입니다. 지금 당장 태그 변경 지원이 필요한 경우 빌드 인덱스 구성 요소의 캐시 간격을 줄이시기 바랍니다. 멀티클러스터 설정에서 복제가 필요한 경우 다른 도커 레지스트리를 크라켄의 백엔드로 설정하는 것을 고려하시기 바랍니다. 이론적으로 크라켄은 성능 저하 없이 모든 크기의 블롭을 배포해야 하지만, Uber에서는 20G 제한을 적용하고 있으며 초대형 블롭(예: 100G 이상)의 프로덕션 사용을 보증할 수 없습니다. 피어는 블롭별로 연결 제한을 적용하며, 비교적 빠른 시일 내에 시더가 되는 피어가 없을 경우 새로운 피어는 연결이 부족해질 수 있습니다. 배포하려는 초대형 블롭이 있는 경우, 먼저 10G 미만의 청크로 분할하는 것이 좋습니다.\
\*Uber’s infrastructure (we even built our own open source Docker image builder, Makisu!)

\---



#### 아키텍처

<figure><img src="https://lh6.googleusercontent.com/lZ44V6Bv8FuZy3WE-18s-pQ2Iwr4AQXp-D_t6UtRLabvGA4p9bU3N3a2ujLP5Xgl46MxbqdnYfQitqh8cghKfkcapwM_sgwVXD8gKbOfjzggsFo678iAO3wMMNzwiCysRiHfmQs23ZAS0GsZQ5orTA" alt=""><figcaption></figcaption></figure>

* Volcano Engine 리소스는 메인 계정, P2P 제어 구성 요소에 속하며, 메인 계정 수준 격리, P2P 제어 구성 요소 세트 아래의 각 마스터 계정에 속합니다. P2PManager 컨트롤러의 서버 수준 구현, 컨트롤러를 통해 모든 P2P cgroup 부분의 제어 플레인을 제어합니다.
* P2P 제어 구성 요소는 사용자 클러스터에 노출된 LB를 통해 CR 데이터 플레인 VPC에 배포됩니다.
* VKE 클러스터에서는 각 노드에 하나의 Dfdaemon이 배포된 DaemonSet으로 배포됩니다.
* VCI에서는 Dfdaemon이 배포로 배포됩니다.
* ECS의 컨테이너는 127.0.0.1:65001을 통해 이 노드의 Dfdaemon에 액세스합니다.
* 사용자 클러스터의 컨트롤러 구성 요소를 통해 클러스터드 배포 , PrivateZone 기능에 따라 사용자 클러스터에서 .p2p.volces.com 도메인 이름을 생성하면 컨트롤러는 특정 규칙에 따라 특정 노드 (VKE , VCI 포함 )의 Dfdaemon 파드를 선택하고 A 레코드 형식으로 위의 도메인 이름으로 해석합니다.
* \<clusterid>.p2p.volumes.com 도메인 이름으로 Nydusd의 ECS가 Dfdaemon에 액세스합니다.
* VCI의 이미지 서비스 클라이언트와 Nydusd는 \<clusterid>.p2p.volces.com 도메인 이름을 통해 Dfdaemon에 액세스합니다.

#### 벤치마크

**구성 환경**

&#x20;   컨테이너 리포지토리: 대역폭 10Gbit/s

&#x20;   Dragonfly Scheduler: 2개 레플리카，요청 1C2G，제한 4C8G, 대역폭 6Gbit/s

&#x20;   Dragonfly Manager: 2개 복제본，요청 1C2G，제한 4C8G, 대역폭 6Gbit/s

&#x20;   Dragonfly Peer : 제한 2C6G, 대역폭 6Gbit/s, SSD

**이미지**

&#x20;   Nginx(500M)

&#x20;   TensorFlow(3G)

**컴포넌트 버전**

&#x20;   Dragonfly v2.0.8

**파드 생성에서 컨테이너 시작까지**

&#x20;       Nginx 파드는 50, 100, 200, 500의 모든 파드에 대해 생성에서 시작까지를 동시에 진행

<figure><img src="https://lh6.googleusercontent.com/G1tcxxxeVob2HWQR6kRX6yxPJatuzt9l5PB_8FAKZmyypvAFKxQIS-IRbQZOytpngExnupZaKBFaTPw_7ex2v6ccxRleNXygkGZAZhoJwYfs3fiT2sNtj1FhflmZ_mT_gnaoVGjEi96bb6Q3mQFfZQ" alt=""><figcaption></figcaption></figure>

TensorFlow 파드는 50, 100, 200, 500개의 모든 파드에 대해 생성에서 시작까지를 동시에 진행

<figure><img src="https://lh3.googleusercontent.com/P9QnoFqYvp7eeMWE7-uIeAEyT0LroPP7iaR9nEe-PIGTrLf5GXAyCMZnvV9xM1QM4tYmWQo3LwbMDP1yXMhSueO0D-Tt4b9QmDwz9PTe_96uVV9QrnWYuK_ojtvXAKDaeHoO67-BGCnEVXzgnmgjBw" alt=""><figcaption></figcaption></figure>

대규모 이미지 시나리오에서 Dragonfly와 Dragonfly & Nydus 시나리오를 사용하면 OCIv1 시나리오에 비해 컨테이너 시작 시간을 90% 이상 절약할 수 있습니다. Nydus를 사용한 후 시작 시간이 단축된 것은 메타데이터 파드의 일부만 가져와서 시작하면 되는 지연 로드 기능 덕분입니다.

**컨테이너 레지스트리의 역방향 최고점 대역폭**

각각 50, 100, 200, 500의 Nginx 파드 동시 스토리지 최고점 트래픽

<figure><img src="https://lh4.googleusercontent.com/Sf2bG7h58UUoQYIEs987PH8Gx3MXFLSJs3quTutcWj6IL45sxWfyT3_JWM2DbJmtbtXIXtXTCOVhOnsjuDe3B7pug95o1Pq59bw0NlAhrFNV3c8kT6_DRw8QqSOZgDDs2VadsIxYNl14jTOvOoGrLg" alt=""><figcaption></figcaption></figure>

각각 50, 100, 200, 500의 TensorFlow 파드 동시 스토리지 최고점 트래픽

<figure><img src="https://lh5.googleusercontent.com/M9OJYJG__Y02W9YSYFb2XHmaNKcNj_ztNHzz6edg4-VNYCdTt6I2rv7AUJymhPcZ_Jai88gBF9IpOyDKCI1dMzGE64QdDNgCYS6qX3tN5EqNkKrrhL81akroCR15NLbk51sLLLcwC0HDzN1QVnwdbg" alt=""><figcaption></figcaption></figure>

**컨테이너 레지스트리의 역방향 소스 트래픽**

소스 트래픽으로 각각 50, 100, 200, 500개의 Nginx 파드 병렬 동시 처리

<figure><img src="https://lh4.googleusercontent.com/qCT-_C9BZI_Ny9ayUoFIgC5BFMZGIOVeJq-RGJJXL0nUDka9GaPP7HKT6bQJsh1AMQgaHte0T5hTt6Wl4KKy5cfmf1htnHbcIIS0mXn4JTArRMugrQmdIlBjT4hsa9iO2rKGg-lY8TK6Si5ABksNfw" alt=""><figcaption></figcaption></figure>

소스 트래픽으로 각각 50, 100, 200, 500개의 TensorFlow 파드 동시 접속이 가능\


<figure><img src="https://lh6.googleusercontent.com/qqvWXYedbFyduQshHmXyeLp61NOh--QYJp6ksjSXjguVVT82-xYeuBKSRexIhDyFVrXGz46sbm-bMq_QBv0FFoWxwDgoTrBhZ25_Dj2JjazevFC_5Cq8r9Qw6rjzMoXb5uJSAr5T1vOIm4NOujFgGA" alt=""><figcaption></figcaption></figure>

대규모 시나리오에서 Dragonfly를 사용하여 소스로 역방향 전송하면 소수의 이미지만 가져오고 OCIv1 시나리오에서는 모든 이미지가 소스로 역방향 전송되어야 하므로 Dragonfly를 사용하여 소스로 역방향 전송하면 최고점과 소스로 역방향 전송하는 트래픽이 OCIv1보다 훨씬 적습니다. 그리고 Dragonfly를 사용한 후 동시성 수가 증가해도 최고점과 소스로 역방향 전송하는 트래픽이 크게 증가하지 않습니다.

### 참고 자료

Volcano Engine: [_https://www.volcengine.com/_](https://www.volcengine.com/)

Volcano Engine VKE: [_https://www.volcengine.com/product/vke_](https://www.volcengine.com/product/vke)

Volcano Engine CR: [_https://www.volcengine.com/product/cr_](https://www.volcengine.com/product/cr)

Dragonfly: [_https://d7y.io/_](https://d7y.io/)

Dragonfly Github Repo: [_https://github.com/dragonflyoss/Dragonfly2_](https://github.com/dragonflyoss/Dragonfly2)

Nydus: [_https://nydus.dev/_](https://nydus.dev/)

Nydus Gihtub Repo: [_https://github.com/dragonflyoss/image-service_](https://github.com/dragonflyoss/image-service)
