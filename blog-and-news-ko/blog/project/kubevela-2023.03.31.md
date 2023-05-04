---
description: >-
  https://www.cncf.io/blog/2023/03/31/kubevela-the-road-to-cloud-native-application-and-platform-engineering/
cover: ../../../.gitbook/assets/CNCF-Ambassador.jpg
coverY: 0
---

# KubeVela: 클라우드 네이티브 애플리케이션 및 플랫폼 엔지니어링으로 가는 길(2023.03.31)

> **한줄 요약:** 애플리케이션 개발자를 중심으로 한 인프라를 제공하는 것을 목표로 하는 OAM과 이를 구현한 Kubevela&#x20;
>
> **관련 프로젝트:** [https://github.com/kubevela/kubevela](https://github.com/kubevela/kubevela)
>
> **kubevela 페이지:** [https://kubevela.io/](https://kubevela.io/) \
> **OAM(Open Application Model) 페이지:** [https://oam.dev/](https://oam.dev/)
>
> **CUE(Configure Unify Execute) 페이지**: [https://cuelang.org/](https://cuelang.org/)

{% hint style="info" %}
이 문서는 기계 번역 후에 일부 내용만 다듬었으므로, 보다 정확한 이해를 위해서는 원문을 보는 것이 좋습니다.
{% endhint %}



게시됨 2023년 3월 31일 ([Link](https://www.cncf.io/blog/2023/03/31/kubevela-the-road-to-cloud-native-application-and-platform-engineering/))

Alibaba Cloud의 엔지니어이자 KubeVela의 유지 관리자인 Da Yin의 게스트 포스트

## 배경&#x20;

몇년 전인 2019년에는 인프라 배포 및 관리를 위한 사실상의 표준으로 Kubernetes가 점차 널리 채택되고 있었습니다. 그리고점점 더 많은 플랫폼 엔지니어들이 쿠버네티스를 기반으로 플랫폼을 구축하고 최종 사용자에게 서비스를 제공하기 시작했습니다. 컨테이너화된 배포와 선언적 구성은 복잡한 시스템 운영의 어려움을 크게 줄여주었습니다.

하지만 수천 개의 워크로드와 관련 리소스가 클러스터에 모여 있는 경우 운영자가 논리적 토폴로지를 파악하고 내부 관계에 따라 적절하고 정확한 관리를 하기란 어려운 일입니다.

이런 시기에 헬름이나 커스터마이즈와 같은 도구는 Kubernetes 리소스를 패키징하고 제공하기 위해 노력해 왔습니다. 현재 이러한 솔루션은 매우 효과적인 것으로 입증되었으며, 헬름 차트는 쿠버네티스 세계에서 빌드된 이미지와 함께 아티팩트를 게시하고 배포하는 가장 인기 있는 방법이 되었습니다.

다른 한편으로, 다양한 프로젝트에서 애플리케이션을 정의하는 여정을 시작했습니다. 이러한 시도는 단순히 리소스를 패키징하는 대신 보다 하향식 관점에서 솔루션을 모색합니다. 보다 편리한 사용을 위해 개별 객체를 그룹화하는 것과 달리, 애플리케이션은 앱 사용자와 기본 인프라 간의 인식 격차를 해소하도록 설계되었습니다. 개방형 애플리케이션 모델(OAM, Open Application Model)의 탄생과 이를 구현한 KubeVela는 바로 여기에서 시작됩니다.

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/03/image-1.png" alt=""><figcaption><p>앱 개발자와 기반 인프라 사용 간의 격차를 해소하기 위해 제안된 개방형 애플리케이션 모델(OAM)</p></figcaption></figure>



## 앱 개발자와 기반 인프라 사용 간의 격차를 해소하기 위해 제안된 개방형 애플리케이션 모델의 탄생&#x20;

Microsoft와 Alibaba Cloud의 지원을 받는 OAM(개방형 애플리케이션 모델)은 클라우드 네이티브 애플리케이션이 어떤 모습이어야 하는지에 대한 이론적 모델을 제시하기 위해 제안되었습니다. 처음에는 Kubernetes 구현과 결합되지 않도록 설계되었으며, 애플리케이션 운영을 위한 통합 인터페이스를 제공하는 경향이 있었습니다.

OAM은 애플리케이션 아키텍처를 추상화하기 위해 **컴포넌트(Component)** 및 **Trait(트레이트,위 참고)**의 개념을 높입니다. 이는 네이티브 쿠버네티스 사용자에게는 큰 변화입니다. 하지만 여기에는 이유가 있습니다.

쿠버네티스에서는 디플로이먼트나 서비스와 같은 리소스가 구현에 중점을 둡니다. 각 유형의 리소스는 특정 기능을 제공합니다. 컨테이너 실행과 같은 일부 낮은 수준의 기능은 파드와 같은 기본 단위로 이동합니다. 컨테이너 오케스트레이션, 배포 또는 스테이트풀셋과 같은 롤백 처리와 같은 일부 상위 레벨은 하위 레벨을 기반으로 구축됩니다. "플랫폼 빌더를 위한 플랫폼"이 큰 성공을 거둔 것은 의심할 여지가 없습니다. 그러나 앱 개발자의 경우, 커뮤니티는 앱 개발자가 일을 처리하기 위해 Kubernetes 전문가가 될 것을 강요하고 있습니다. 게다가 저수준 리소스에 대한 피상적인 이해는 실제로 잘못된 작업으로 인해 상당한 위험을 초래할 수 있습니다.

\>>> OAM 비교번역 추가: ([Link](https://oam.dev/#overview))

| TRADITIONAL WAY                                                                                                                                                                                                                                                                                               | THE OAM WAY                                                                                                                                                                                                                                                                                                     |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <h3>App Deployment is Hard</h3>                                                                                                                                                                                                                                                                               | <h3>An <em>App-centric</em> Approach</h3>                                                                                                                                                                                                                                                                       |
| <ol><li>개발자는 인그레스, 레이블, DNS 등 앱이 아닌 인프라 세부 정보에 시간을 할애하고 인프라가 어떻게 구현되는지 배우게 됩니다.</li></ol><ol start="2"><li>확장 불가능 - 상위 계층 플랫폼이 도입될 수 있지만, 앱의 요구 사항이 곧 해당 플랫폼의 기능을 능가할 것이 거의 확실합니다.</li></ol><ol start="3"><li>런타임 종속 - 앱 설명은 런타임 인프라와 긴밀하게 연결되어 있으며, 이는 하이브리드 환경에서 앱을 구성, 개발 및 운영하는 방식에 큰 영향을 미칩니다.</li></ol> | <ol><li>애플리케이션 우선 - 인프라 없이 운영 동작을 앱 정의의 일부로 포함하는 독립형 모델로 앱을 정의합니다.</li></ol><ol start="2"><li>명확성 및 확장성 - 플랫폼 기능을 재사용 가능한 조각으로 모듈화하여 필요에 따라 앱 배포로 조립하고 완전한 셀프 서비스를 제공하는 개방형 표준입니다.</li></ol><ol start="3"><li>런타임에 구애받지 않는 환경 - 온프레미스 클러스터, 클라우드 제공업체 또는 에지 디바이스 전반에서 앱을 배포하고 운영할 수 있는 일관된 환경을 제공합니다.</li></ol> |

### 컴포넌트&#x20;

OAM은 다른 시작점을 선택합니다. 애플리케이션을 실제로 구성하는 요소는 무엇일까요? 앱 개발자에게 필요한 것은 무엇일까요?

애플리케이션을 실행하는 무언가입니다. 이것은 애플리케이션의 절대적으로 핵심적인 부분입니다. 쿠버네티스 내부에서는 디플로이먼트, 파드 또는 다른 것일 수 있습니다. Kubernetes 외부에서는 Docker 컨테이너, 가상 머신 또는 클라우드 실행 엔진이 될 수 있습니다. 99.9%의 앱 개발자가 프로그램을 만들고 실행합니다. OAM은 앱을 실행하는 모든 것을 컴포넌트로 정의합니다. 단순한 리소스 그룹이 아닙니다. 컴포넌트가 없으면 애플리케이션의 용도를 알 수 없습니다.

```
apiVersion: core.oam.dev/v1beta1
kind: Application
metadata:
  name: webserver-demo
spec:
  components:
    - name: hello-world
      type: webserver               # claim to deploy webserver component definition
      properties:                   # setting parameter values
        image: crccheck/hello-world
        port: 8000                  # this port will be automatically exposed to public
        env:
        - name: "foo"
          value: "bar"
        cpu: "100m"
```

_OAM 애플리케이션 내부의 컴포넌트 예시_



### 특성

&#x20;애플리케이션을 제대로 실행하기 위해서는 다른 것들도 필요합니다. 예를 들어, 쿠버네티스에는 서비스, 컨피그맵, 퍼시스턴트볼륨클레임, 그리고 디플로이먼트가 사용할 수 있는 특정 기능을 제공하는 다른 많은 리소스들이 있습니다. 그러나 이러한 리소스는 일부 기술적 요구 사항을 충족하기 위해 만들어졌으며 목적 지향적이지 않습니다. 컨피그맵은 쿠버네티스 사용자가 구성 데이터를 저장할 수 있는 방법을 제공하지만, 컨테이너가 구성을 사용하도록 하려면 사용자가 디플로이먼트에 볼륨 섹션을 추가하고 컨테이너 인수 또는 환경 변수를 설정해야 한다.

다시 관점을 바꿔서 앱 개발자가 이러한 리소스를 사용할 때 실제로 무엇을 하고 싶은지 생각해 보면, 많은 리소스가 애플리케이션 컴포넌트의 추가 기능을 위해 제공된다는 결론에 도달할 수 있습니다. 서비스 또는 인그레스 객체는 애플리케이션에 대한 액세스를 제공하기 위해 생성됩니다. 배포의 퍼시스턴트볼륨클레임과 볼륨 섹션은 스토리지를 제공하기 위한 것입니다. Prometheus의 ServiceMonitor와 같은 서드파티 개체는 관찰 규칙을 제공하기 위한 것입니다. 기능을 제공하는 데 사용되는 이러한 보조 리소스 및 수정 사항은 OAM에서 Trait로 정의됩니다.

Trait은 독립적으로 작동하지 않습니다. 이러한 기능은 컴포넌트에 첨부되어 데코레이션으로 작동합니다. 이 기능이 없으면 애플리케이션의 주요 목적은 변경되지 않지만 불완전한 목적이 됩니다.

```
apiVersion: core.oam.dev/v1beta1
kind: Application
metadata:
  name: my-example-app
spec:
  components:
    - name: publicweb
      type: web-ui
      properties: # properties targeting component parameters.
        image: example/web-ui:v1.0.2@sha256:verytrustworthyhash
        param_1: "enabled" # param_1 is defined on the web-ui component
      traits:
        - type: ingress # ingress trait providing a public endpoint for the publicweb component of the application.
          properties: # properties are defined by the trait CRD spec. This example assumes path and port.
            path: /
            port: 8080
    - name: backend
      type: company/test-backend # test-backend is referenced from other namespace
      properties:
        debug: "true" # debug is a parameter defined in the test-backend component.
      traits:
        - type: scaler # scaler trait to specify the number of replicas for the backend component
          properties:
            replicas: 4
```

_특성이 첨부된 두 개의 컴포넌트가 포함된 OAM 애플리케이션의 예시_



### 그리고 더 나아가서

주로 기능적인 측면에서 애플리케이션을 설명하는 컴포넌트와 특성 외에도 애플리케이션을 다양한 수준에서 작동시키는 다른 요소들이 있습니다.

각 애플리케이션은 여러 개의 원자적 기능 단위, 즉 컴포넌트를 포함할 수 있으며 각 컴포넌트는 여러 개의 특성으로 장식될 수 있습니다. 실행 중인 컴포넌트와 명확한 일대일 관계가 없는 일부 애플리케이션 수준의 동작이나 전략을 정의하려면 애플리케이션 정의에 더 많은 것이 필요합니다.

아직까지 OAM에는 모델링의 이 부분에 대한 결정적인 솔루션이 없습니다. 범위, 정책, 워크플로우 등의 개념이 제안되었습니다. 범위는 애플리케이션의 사용 범위를 정의하기 위해 추가되었습니다. 정책은 공통 전략과 동작을 설명하기 위해 추가되었습니다. 워크플로우는 애플리케이션의 전달 측면에 중점을 둡니다. 이 중 일부는 실제 사례로 설계를 추진하는 데 도움이 될 수 있도록 OAM을 위한 Kubernetes 기반 구현 중 하나인 KubeVela에 채택되었습니다.

## 이론에서 실제까지&#x20;

오픈 애플리케이션 모델의 사양은 개념적으로 앱 개발자의 관점에서 애플리케이션을 모델링하고 플랫폼을 구축하고자 하는 개발자에게 가이드를 제공합니다. 지난 몇 년 동안  [KubeVela](https://kubevela.io/), [Crossplane](https://github.com/crossplane/oam-kubernetes-runtime), [Verrazzano](https://verrazzano.io/latest/) 그리고 [Intuit](https://thenewstack.io/how-intuits-platform-engineering-team-chose-an-app-definition/) 등 다양한 업체에서 다양한 시도를 해왔습니다. 부분적으로는 기술적인 한계로 인해 OAM 사양이 인프라에 구애받지 않는 것으로 제기되었지만, 대부분의 구현은 모두 쿠버네티스를 기반으로 하고 있습니다.

역사를 되돌아본 후 요약한 바와 같이, 애플리케이션 모델링은 쿠버네티스에서 특히 중요합니다. 쿠버네티스에 OAM 사양을 도입할 때, KubeVela는 다양한 세부 기술 문제를 해결하기 위해 많은 노력을 기울였습니다. 이러한 작업은 추상화, 배포 및 관리의 세 가지 트랙으로 분류할 수 있습니다.

### 추상화&#x20;

OAM의 컴포넌트는 애플리케이션의 실행 인스턴스를 정의합니다. 실행 중인 인스턴스 유형에 따라 다른 유형의 컴포넌트가 사용됩니다. 쿠버네티스에서는 디플로이먼트나 스테이트풀셋과 같은 다양한 워크로드 구현에 해당합니다. KubeVela는 이러한 타입을 컴포넌트정의로 선언하고 확장 가능한 솔루션을 위해 CUE를 도입했다. CUE는 쿠버네티스 리소스를 템플릿화하는 데 사용되며, 따라서 플랫폼 빌더는 임의의 커스터마이징을 할 수 있다.

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/03/image-5.png" alt=""><figcaption><p>KubeVela는 컴포넌트 및 특성 템플릿을 생성하기 위해 CUE를 활용</p></figcaption></figure>

Trait의 경우, 추상화 방법은 유사하며, TraitDefinition을 사용하여 다양한 유형을 선언합니다. 컴포넌트와의 주요 차이점 중 하나는 일부 유형의 Trait는 ComponentDefinition으로 추상화된 리소스를 수정해야 한다는 것입니다. 예를 들어 포트를 노출하려면 Service 객체를 추가하고 동시에 배포 사양에 포트를 노출해야 합니다. CUE 자체는 이를 위한 방법을 제공하지 않지만, KubeVela는 어떻게든 CUE를 확장하여 이를 달성했습니다.

정적 렌더링 외에도 KubeVela의 추상화 계층은 런타임을 인식합니다. 즉, 렌더링 로직이 현재 사용하고 있는 Kubernetes 버전과 같은 런타임 환경에 따라 변경될 수 있습니다. 이는 특히 클러스터 전반에서 통합 컨트롤 플레인으로 KubeVela를 사용할 때 유용합니다.

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/03/image-3.png" alt=""><figcaption><p>기존 시스템에서는 애플리케이션 개발자가 클러스터 전체에서 버전 업그레이드를 처리해야 함</p></figcaption></figure>

요약하자면, 헬름 차트에서 사용하는 Go 템플릿이나 Kustomization을 통해 스펙 오버레이를 만드는 것과 유사하게, KubeVela의 추상화 계층 구현은 큰 기술적 차이를 보이지 않는다. 반면, KubeVela는 이러한 추상화를 ComponentDefinitions와 TraitDefinitions에 근거를 두어 재사용을 개선했습니다. 이는 헬름 차트를 사용하여 전체 애플리케이션을 직접 템플릿화하는 것과 비교하여 플랫폼 빌더에게 더 세분화된 선택권을 제공합니다. 이를 통해 플랫폼 엔지니어(**X-Definitions**)와 앱 개발자(**Application)**의 책임이 더 명확해집니다.

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/03/image-6.png" alt=""><figcaption><p>추상화 구현은 플랫폼 엔지니어에게 맡기고 앱 개발자는 미리 정의된 유형의 컴포넌트 및 특성을 사용하여 애플리케이션을 구성할 수 있음</p></figcaption></figure>

### 배포

추상화 계층은 주로 애플리케이션을 빌드, 패키징 및 배포하는 문제를 해결합니다. 애플리케이션을 배포하고 통합된 애플리케이션으로 만들면 상황이 더 복잡해집니다. 애플리케이션 전체는 KubeVela에서 전송을 위한 기본 단위로 취급됩니다. 하나의 애플리케이션 내에서 서로 다른 컴포넌트 간의 내부 관계에 따라 이러한 컴포넌트가 어떻게 전달되어야 하는지가 결정될 수 있으며, 이는 의존성 필드에 표시됩니다.

리소스 디스패치 외에도 애플리케이션 딜리버리에는 프로덕션 환경으로의 위험한 딜리버리에 대한 수동 검토, 자동 딜리버리 완료에 대한 알림, 멀티 클러스터 간 차별화 딜리버리 등 더 많은 작업이 수반될 수 있습니다. 사용자가 배포 프로세스를 커스터마이징할 수 있도록 KubeVela는 애플리케이션 모델에 워크플로우를 추가하고, 워크플로우스텝정의는 CUE를 활용하여 원자 실행 단위를 정의합니다.

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/03/image-4.png" alt=""><figcaption><p>KubeVela는 일관되고 프로그래밍 가능한 선언적 워크플로우를 제공하여 앱 전송 프로세스를 오케스트레이션 진행함</p></figcaption></figure>

일부 사용자는 서로 연관된 여러 개의 애플리케이션을 전송하고자 하는 경우도 발생할 수 있으며, KubeVela는 이를 위해 파이프라인이라는 상위 수준의 솔루션까지 제공합니다.

또한 애플리케이션 모델을 전송과 함께 공동으로 설계해야 하는지에 대해서도 자주 논의되는 문제입니다. Tekton, Argo Workflow와 같은 독립적인 배포 도구는 수년 전부터 개발되어 왔습니다. Jenkins나 GitHub Actions와 같은 다른 CI 도구도 다양한 배포 시나리오를 지원하기 위해 스스로 진화하고 있습니다. KubeVela는 애플리케이션 모델과 배포 프로세스를 함께 결합하는 선구적인 발걸음을 내디뎠는데, 이는 이 방법이 애플리케이션 모델을 프로덕션에 적용하기 위한 실용적인 솔루션이라고 생각했기 때문입니다. 그러나 이론적 모델링에 대한 해답은 여전히 폭넓은 논의를 기다려야 하기 때문에 OAM 사양의 발전이 더디게 진행되고 있습니다.

### 관리&#x20;

애플리케이션 모델은 배포만 위한 것이 아닙니다. 선언된 상태는 항상 원합니다. 따라서 실제로 애플리케이션 모델은 KubeVela의 일관된 조정에도 사용되고 있습니다. KubeVela는 딜리버리 프로세스가 성공적으로 완료된 후에도 원하는 상태에서 벗어나지 않도록 보장하며, 이는 최종 상태 지향적인 Kubernetes의 철학과 일치합니다.

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/03/image-2.png" alt=""><figcaption><p>KubeVela는 제공된 애플리케이션에 대한 구성 변동이 없도록 지속적으로 보장</p></figcaption></figure>

또한, 버전 관리, 권한 제어, 리소스 공유, 가비지 수집 등 다양한 관리 작업도 내장되어 있습니다. 이러한 기능들은 현재로서는 OAM 사양에 포함되어 있지 않지만 향후 OAM 사양의 일부가 될 수 있는 일반적인 기능으로 간주됩니다.

일반적으로 쿠버네티스에서 애플리케이션 사용에 대한 모든 종류의 요구를 충족시키기 위해, KubeVela는 OAM을 넘어 애플리케이션을 운영할 수 있는 더 많은 방법을 모색하기 위해 지속적으로 시도해 왔습니다.

더 자세한 기술적인 내용이 궁금하시다면 [이 블로그](https://kubevela.net/blog/2022/11/29/retro-2022)를 참고하시기 바랍니다.

## 미래

많은 분들이 최근 OAM이 왜 진화를 멈춘 것 같은지 의문을 제기했습니다. 프로덕션 용도에 주로 관심이 있는 분들을 위한 대답은 OAM 구현인 KubeVela는 항상 진화해 왔다는 것입니다. 사용자 수가 증가하고 있으며 이제 막 CNCF 인큐베이션 프로젝트가 되었습니다.

이론적 모델에 관해서는, KubeVela는 워크플로우나 정책과 같은 추가 개념을 OAM 사양에 제안하기 전에 여전히 산업 사용으로부터 더 많은 피드백과 증거를 기다리고 있습니다. 점점 더 많은 사람들이 CNCF의 도움을 받아 커뮤니티에 참여하여 오늘날의 하이브리드 멀티클라우드 환경에서 애플리케이션을 더 쉽고, 빠르고, 안정적으로 배포하고 운영할 수 있기를 기대합니다.
