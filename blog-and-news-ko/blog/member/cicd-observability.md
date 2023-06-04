---
description: >-
  https://www.cncf.io/blog/2023/05/05/how-to-gain-observability-into-your-ci-cd-pipeline/
---

# CI/CD 파이프라인에 관측 가능성(observability)을 확보하는 방법(2023.05.05)

> **한줄 요약:** 원할한 cicd를 위해 가장 중요한 요소 중에 하나인 관측 가능성에 대해서 설명하고 있습니다.
>
> **관련 프로젝트:** \
> &#x20;\- [https://opentelemetry.io/](https://opentelemetry.io/)\
> &#x20;\- [https://github.com/open-telemetry/oteps](https://github.com/open-telemetry/oteps)&#x20;

{% hint style="info" %}
이 문서는 기계 번역 후에 일부 내용만 다듬었으므로, 보다 정확한 이해를 위해서는 원문을 보는 것이 좋습니다.
{% endhint %}

By 도탄 호로비츠&#x20;

게시일: May 5, 2023 ([Link](https://www.cncf.io/blog/2023/05/05/how-to-gain-observability-into-your-ci-cd-pipeline/))

원래 Logz.io의 블로그에 게시된 게스트&#x20;

게시물 작성자: Dotan Horovits

<figure><img src="https://dytvr9ot2sszz.cloudfront.net/wp-content/uploads/2022/05/image7.png" alt=""><figcaption><p>Kibana(또는 OpenSearch) 대시보드로 시각화하기</p></figcaption></figure>

통합 가시성이 운영 중인 운영 체제에 반드시 필요하다는 것은 누구나 알고 있습니다. 그러나 우리는 종종 소프트웨어 릴리스 프로세스라는 정작 우리 자신의 안쪽을 소홀히 하는 경우가 많습니다.

Logz.io에서도 이러한 실수를 저지른 적이 있습니다. CI/CD 파이프라인에서 장애를 처리하는 데 시간과 에너지를 낭비하고 있었고, 개발자 근무(DoD, Developer-on-Duty) 교대 근무를 지루하게 만들었습니다. 그렇기 때문에 관측 가능성을 [CI/CD 파이프라인](https://logz.io/learn/cicd-observability-jenkins/)에 통합하는 것이 중요합니다.

일부 CI/CD 도구는 기본적으로 일부 관측 가능성 기능을 제공합니다. Logz.io에서는 Jenkins를 사용하며 해당 영역의 기능과 플러그인을 살펴봤습니다. Jenkins를 사용하면 개별 실행을 입력하고 해당 실행이 어떻게 진행되었는지 확인할 수 있습니다.

그러나 패턴을 실제로 이해하기 위해 자체 필터와 시간 범위를 사용하여 모든 지점 및 머신에 걸쳐 모든 파이프라인의 실행에서 집계된 정보를 모니터링하려는 경우에는 이것만으로는 충분하지 않을 때가 많습니다.

다음과 같은 기본적인 집계 질문은 대답하기 까다롭거나 번거롭다는 것을 알게 되었습니다:

* 모든 실행이 동일한 단계에서 실패했나요?&#x20;
* 모든 실행이 같은 이유로 실패했나요?&#x20;
* 특정 지점에서만 장애가 발생했는가?&#x20;
* 특정 머신에서 장애가 발생했는가?&#x20;
* 가장 많이 실패한 항목은 무엇인가요?&#x20;
* 이상값을 식별하는 데 걸리는 일반적인 실행 시간은 얼마인가요?

<figure><img src="https://dytvr9ot2sszz.cloudfront.net/wp-content/uploads/2022/05/image12-1024x50.png" alt=""><figcaption><p>관측 가능성을 위한 4단계 프로세스. 출처: CI/CD 통합 가시성 <a href="https://logz.io/learn/cicd-observability-jenkins/"><code>가이드</code></a></p></figcaption></figure>

CI/CD 도구의 기본 제공 관측 가능성 기능도 모두 사용했다면, 이제 프로덕션 환경과 마찬가지로 전용 모니터링 및 관측 가능성 설정을 통해 적절한 관측 가능성을 설정해야 할 때입니다.

4단계로 CI/CD 파이프라인에 관측 가능성을 확보할 수 있습니다. 이 긴 형식의 가이드인 '[느리고 불규칙한 CI/CD 파이프라인의 생성 작업은 관측 가능성으로부터 시작](https://logz.io/learn/cicd-observability-jenkins/)' 이라는 부분은 많은 사람들이 알고 있는 인기 있는 오픈 소스 프로젝트이자 저희 회사에서도 광범위하게 사용하고 있는 Jenkins를 참조 도구로 사용했습니다.

하지만 다른 도구를 사용 중이시더라도 많은 부분을 적용할 수 있습니다. CI/CD 파이프라인에 대한 관측 가능성을 확보하려면 다음을 수행해야 합니다:

1. CI/CD 파이프라인 실행에 대한 데이터 수집
2. 빠른 쿼리 및 검색을 위해 데이터 색인 및 저장
3. 사용자 정의 대시보드로 데이터 시각화
4. 데이터에 대한 보고서 작성 및 알림 규칙 설정

우수한 CI/CD 통합 가시성에 투자하면 [변경 리드 타임](https://logz.io/blog/dora-metrics-improving-devops-performance/)(`역자: 링크 제목은 'DORA(DevOps Research and Assessment) 메트릭을 통한 데브옵스 성능 개선'`)이 크게 개선되어 커밋이 프로덕션에 도달하는 데 걸리는 주기를 효과적으로 단축할 수 있습니다.

CI/CD 관측 가능성을 표준화할 수 있나요? 실제로 CNCF의 [OpenTelemetry](https://opentelemetry.io/) 프로젝트는 관측 가능성 데이터 수집을 위한 통합 개방형 플랫폼이기 때문에 완벽하게 적용할 수 있습니다. 이것이 바로 제가 제안한 [OpenTelemetry 확장 제안(OTEP, OpenTelemetry extension proposal)](https://github.com/open-telemetry/oteps/pull/223)의 배경이 되는 아이디어입니다. CNCF GitHub에서 PR을 확인하시고 언제든지 의견을 보내주시기 바랍니다.
