---
description: >-
  https://www.cncf.io/blog/2023/04/11/announcing-a-white-paper-on-platforms-for-cloud-native-computing/
cover: ../../../.gitbook/assets/CNCF-Ambassador.jpg
coverY: 0
---

# 클라우드 네이티브 컴퓨팅을 위한 플랫폼 백서(White Paper) 소개(2023.04.11)

> **한줄 요약:** 2023년 10대 전략 기술로 선정된 플랫폼 엔지니어 관점에서 구분한 CNCF 프로젝트들입니다.
>
> **관련 설명 문서:** \
> &#x20;\- Web ([EN](https://appdelivery.cncf.io/whitepapers/platforms/), KO)\
> &#x20;\- PDF ([EN](https://github.com/cncf/tag-app-delivery/raw/main/platforms-whitepaper/v1/assets/platforms-def-v1.0.pdf), KO)

{% hint style="info" %}
이 문서는 기계 번역 후에 많은 의역을 통해 변경되었으므로, 보다 정확한 이해를 위해서는 원문을 보는 것이 좋습니다.
{% endhint %}

_게시일: 2023년 4월 11일  (_[_Link_](https://www.cncf.io/blog/2023/04/11/announcing-a-white-paper-on-platforms-for-cloud-native-computing/)_)_

커뮤니티 게시글 작성자: CNCF의 플랫폼 WG의 Josh Gavant와 Abby Bangser

CNCF의 플랫폼 워킹 그룹(WG, Working Group)은 클라우드 네이티브 컴퓨팅을 위한 플랫폼의 특성과 이점에 대한 지침과 명확성을 제공하는 백서의 첫 번째 릴리스를 발표하게 되어 기쁘게 생각합니다. 지금 PDF([EN](https://github.com/cncf/tag-app-delivery/raw/main/platforms-whitepaper/v1/assets/platforms-def-v1.0.pdf), KO)로 다운로드하거나 웹사이트([EN](https://tag-app-delivery.cncf.io/whitepapers/platforms/), KO)에서 확인하세요.

지속적인 의견과 인사이트를 제공해 주신 아래 나열된 많은 기여자에게 감사드립니다!

이 백서를 준비한 이유는 플랫폼을 통해 조직이 클라우드 컴퓨팅의 가치를 완전히 실현할 수 있다는 사실을 알게 되었기 때문입니다. 플랫폼은 인프라와 애플리케이션 구성 요소를 신속하게 통합하여 애플리케이션 및 서비스 제공을 가속화합니다. 플랫폼은 엔터프라이즈 IT의 지속적인 발전의 한 단계로서, 조직 전체에 데브옵스 스타일 핵심 요소인 효율성과 자율성을 일관되게 제공합니다.

이 백서의 목적은 내부 플랫폼이 제공하는 가치, 플랫폼이 해결하는 문제, 플랫폼의 성공적인 사례를 도입하는 방법, 플랫폼에 필요한 속성 및 기능들을 설명함으로써 조직의 리더와 플랫폼 구축 담당자를 교육하고 도움을 주는 것입니다. 또한 오늘날의 CNCF 프로젝트가 어떻게 완전한 플랫폼에 대한 선도적인 위치로 자리매김했는지를 설명하고자 합니다. 마지막으로 플랫폼 팀이 성공할 수 있도록 지원하는 방법, 진행 상황을 측정하는 방법, 준비해야 할 몇 가지 과제에 대한 지침을 제공합니다.

WG Platforms과 [TAG App Delivery](https://github.com/cncf/tag-app-delivery)는 클라우드 애플리케이션 빌더와 CNCF 프로젝트 관리자에게 더 많은 지침을 제공하고 복잡성을 줄이기 위해 지금 소개하고 있는 구성 위에 구축되고 있습니다. 플랫폼 팀에 제품 사고방식을 통합하고 표준 거버넌스 정책을 적용하는 것과 같은 관행에 대한 지침을 확대하고, 완전한 플랫폼의 잠재적 구성 요소인 비밀(Secrets) 관리, 아티팩트 저장소, 웹 포털 및 API 프레임워크와 같은 기능에 대한 규칙을 추구하고 있습니다.

이와 같은 다양한 정책을 만드는 것에 관심이 있다면 리소스에 나와 있는 링크를 통해 저희와 함께 하실 수 있습니다.

<figure><img src="https://lh5.googleusercontent.com/ReVL0KXEgkVxrLJpTIaIqLox9URS6ubm7_YEPvgf8PH_ULqUPqfZBd0kaag40zHBI9cDt4UgBXyNJdGwykdLxjhLi3Xse7FFKKAtcPIZjbCgTW4HbifZJRAtkCO2cG_brIfJp2VkVGi47pJBI6OE9cY" alt=""><figcaption></figcaption></figure>

마지막으로, 이번 백서 버전이 마지막이 아닙니다! 설문조사(곧 공개될 예정입니다!)에 응답하고 CNCF 그룹과 밋업에서 여러분의 플랫폼 이야기를 공유하여 다음 버전에 대한 정보를 제공해 주세요. 곧 여러분과 만나 뵙기를 기대합니다!

## 기여자(Contributor) 여러분들께 감사드립니다!&#x20;

이 이정표에 도달하면서 특히 다음과 같은 분들의 [모든 기여](https://github.com/cncf/tag-app-delivery/commits/main/platforms-whitepaper)와 피드백에 대해 CNCF의 WG 플랫폼 멤버들에게 감사의 말씀을 전합니다:

* Abby Bangser
* Abhinav Mishra
* Abi Noda
* Alex Chesser
* Brad Bazemore
* Chris Aniszczyk
* Colin Griffin
* Dash Copeland
* Gopal Ramachandran
* Henrik Blixt
* Johannes Kleinlercher
* Josh Gavant
* Justin Abrahms
* Lian Li
* Mark Fussell
* Mauricio Salatino
* Pascal Fenkam
* Raffaele Spazzoli
* Roberth Strand
* Saim Safdar
* Scott Nasello
* Taras Mankovski
* Thomas Vitale
* Viktor Nagy

## 리소스&#x20;

* Slack: [https://cloud-native.slack.com/archives/C020RHD43BP](https://cloud-native.slack.com/archives/C020RHD43BP)
* Work item tracker: [https://github.com/cncf/tag-app-delivery/issues/new/choose](https://github.com/cncf/tag-app-delivery/issues/new/choose)
* Mailing list: [https://lists.cncf.io/g/cncf-tag-app-delivery](https://lists.cncf.io/g/cncf-tag-app-delivery)
