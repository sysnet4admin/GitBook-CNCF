---
description: >-
  https://www.cncf.io/blog/2023/06/06/a-chatgpt-powered-code-reviewer-bot-for-open-source-projects/
---

# 오픈소스 프로젝트를 위한 ChatGPT 기반 코드 리뷰어 봇(Bot) (2023.06.06)

> **한줄 요약:** WasmEdge 코드 리뷰 봇을 사용해서 생산적인 코드 리뷰하는 방법 제시&#x20;
>
> **관련 프로젝트:** [https://github.com/WasmEdge/WasmEdge](https://github.com/WasmEdge/WasmEdge)&#x20;
>
> **WasmEdge runtime 페이지:** [https://wasmedge.org/](https://wasmedge.org/) \
> **flows.network 페이지:** [https://flows.network/](https://flows.network/)

{% hint style="info" %}
이 문서는 기계 번역 후에 일부 내용만 다듬었으므로, 보다 정확한 이해를 위해서는 원문을 보는 것이 좋습니다.
{% endhint %}



2023년 6월 6일 ([Link](https://www.cncf.io/blog/2023/06/06/a-chatgpt-powered-code-reviewer-bot-for-open-source-projects/))

Miley Fu, CNCF 홍보대사, CNCF WasmEdge 프로젝트 개발자

코드 리뷰는 최신 소프트웨어 개발에서 매우 중요한 부분입니다. GitHub 워크플로에서 코드 리뷰는 풀 리퀘스트(PR)가 생성될 때 시작되며, PR이 승인되고 병합 또는 거부되면 종료됩니다. 검토자는 일반적으로 선임 개발자 또는 아키텍트입니다. 이들은 리포지토리에 제출된 코드가 정확하고, 유지 관리가 가능하며, 확장 가능하고, 안전한지 확인하는 데 도움을 줍니다. 이는 커뮤니티로부터 많은 코드 기여를 받을 수 있는 오픈 소스 프로젝트에서 특히 중요합니다.

그러나 PR의 코드 리뷰는 소프트웨어 개발에서 가장 큰 분쟁의 소지가 되기도 합니다.

* 시니어 개발자는 매우 바쁘고 비용도 많이 듭니다. 코드 리뷰에 할애할 수 있는 시간이 가장 적습니다.&#x20;
* 하지만 코드 리뷰 없이는 개발 프로세스를 진행할 수 없습니다(예: PR 병합). 개발자는 종종 검토를 기다리며 무료하게 시간을 보내기도 합니다. 오픈소스 커뮤니티 개발자의 경우, 시기적절하지 않은 코드 리뷰는 추가 기여를 방해합니다.&#x20;
* 관리자는 종종 선임 개발자에게 PR과 관련된 주요 변경 사항 및 위험 요소를 보고하고 설명하도록 요청하여 프로세스를 더욱 지연시킵니다.&#x20;

_26,000명의 개발자가 70만 개 이상의 PR을 검토한 Linear b의 설문조사에 따르면, PR을 검토하는 데 평균 4일 이상이 걸리는 것으로 나타났습니다. 개발자는 PR을 제출할 때마다 이틀의 유휴 시간을 낭비하는 셈입니다. 이는 엄청난 생산성 낭비입니다._

이 블로그 게시물에서는 CNCF의 WasmEdge 커뮤니티에서 만든 GitHub PR 코드 리뷰 봇에 대해 설명합니다. 이 봇은 오픈 소스 [WasmEdge 런타임](https://wasmedge.org/)에서 실행되며 ChatGPT/GPT4를 사용하여 코드 검토 작업을 수행합니다. 모든 PR을 자동으로 검토하기 위해 이미 WasmEdge 리포지토리에 배포되어 있습니다. 급한 분들을 위해 5분 이내에 GitHub에서 코드 검토 봇을 직접 생성하고 배포할 수 있습니다!



## 실제 사례

하지만 ChatGPT/4가 코드 리뷰를 수행할 만큼 똑똑할까요? 이것은 시니어 개발자가 해야 할 일 아닌가요? 더 이상 고민하지 않고 예시를 살펴보겠습니다. 아래 그림은 WasmEdge 오픈 소스 리포지토리 중 하나에 제출된 PR을 보여줍니다. 입력된 숫자가 소수인지 확인하기 위해 check\_prime() 함수를 추가합니다. 구현은 꽤 표준적으로 보입니다. 2에서 n의 제곱근까지 반복하고 모든 정수에 대해 분할 가능성을 확인합니다.

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/06/image1.jpg" alt=""><figcaption><p>n이 소수인지 확인하는 이 코드에서 문제를 발견하셨나요? ChatGPT가 찾아냈습니다! (아래에 있음)</p></figcaption></figure>

봇은 [다음과 같은 코드 리뷰 코멘트](https://github.com/second-state/wasmedge-quickjs/pull/82#issuecomment-1498299630)를 제공합니다. 정말 대단하다고 말하지 않을 수 없습니다!



<figure><img src="https://www.cncf.io/wp-content/uploads/2023/06/image2.jpg" alt=""><figcaption><p>ChatGPT의 코드 리뷰</p></figcaption></figure>

대화를 계속 진행하면 [ChatGPT/4를 통해 코드를 더욱 최적화](https://github.com/second-state/chat-with-chatgpt/issues/250)하여 반복문에서 이미 발견된 소수의 배수를 모두 건너뛰는 솔루션을 찾을 수 있게 도와주는 것을 확인할 수 있습니다.

관리자/메인테이너로서 [코드 리뷰 봇이 작성한 기술 요약](https://github.com/WasmEdge/WasmEdge/pull/2394#issuecomment-1497819842)도 매우 유용했습니다.

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/06/image8.jpg" alt=""><figcaption><p>풀 리퀘스트로 인한 코드 변경 사항 요약</p></figcaption></figure>



## 어떻게 작동할까요?

코드 리뷰어 봇은 서버리스 함수(즉, flow function)로 Rust(그리고 곧 JavaScript!)로 작성되며, Wasm으로 컴파일되고 [flows.network](https://flows.network/)에서 호스팅하는 WasmEdge 런타임에서 실행됩니다.

Flows.network는 WasmEdge 함수를 실행하고 외부 API(예: GitHub)에 연결하기 위한 UI와 호스팅된 서비스를 제공하는 PaaS입니다. 충분한 무료 티어가 있습니다. 물론 원하는 경우 자체 WasmEdge 클라우드 서비스를 실행할 수도 있습니다.

연결된 GitHub 저장소에 PR이 생성되면 flow function(또는 🤖)이 트리거됩니다. 플로우 함수는 PR의 패치와 파일을 수집하여 ChatGPT/4에 검토 및 요약하도록 요청합니다. 그런 다음 그 결과를 다시 PR에 댓글로 게시합니다.

봇은 PR에 새로운 커밋과 업데이트가 있는지 지속적으로 모니터링합니다. 필요에 따라 PR의 코드 리뷰 코멘트를 업데이트(덮어쓰기)합니다.

봇은 PR의 댓글 섹션에 있는 마법 문구로 트리거될 수도 있습니다. 예를 들어, 리뷰어가 봇이 요약을 업데이트하도록 하려면 " flow summarize"라고 댓글을 달면 됩니다.



## 나만의 봇 만들기

나만의 코드 검토 봇을 만들고 배포하려면 다음의 간단한 3단계를 따르세요. 5분도 채 걸리지 않습니다!

두 가지 봇 템플릿 중에서 선택할 수 있습니다. [하나](https://github.com/flows-network/github-pr-summary)는 PR의 각 커밋을 요약하는 것입니다(이를 통해 봇 생성). [다른 하나](https://github.com/flows-network/github-pr-review)는 PR에서 변경된 각 파일을 검토하는 것입니다(이 파일로 봇 만들기). 다음은 전자의 단계를 보여줍니다.

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/06/image6.jpg" alt=""><figcaption><p>템플릿으로 봇 만들기</p></figcaption></figure>

* [**flows.network에서 코드 검토 봇 템플릿을 로드합니다.**](https://flows.network/flow/createByTemplate/Summarize-Pull-Request) **(로그인 필요)** 템플릿에는 봇 자체의 소스 코드가 포함되어 있습니다. 나중에 수정하고 사용자 지정할 수 있도록 소스 코드를 사용자 고유의 GitHub 계정에 복제합니다. 만들기 및 배포를 클릭합니다.&#x20;
* **봇에 OpenAI API 키를 제공합니다.** 과거에 API 키를 저장한 적이 있다면 이 단계를 건너뛰고 해당 키를 재사용할 수 있습니다.&#x20;
* **봇의 GitHub 액세스를 승인합니다.** github\_owner 및 github\_repo는 봇이 PR을 검토할 대상 GitHub 저장소를 가리킵니다. 권한 부여를 클릭하여 봇에 GitHub에서 필요한 권한을 부여합니다.&#x20;

아래 그림은 위의 2단계와 3단계를 보여줍니다.

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/06/image3.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/06/image10.jpg" alt=""><figcaption></figcaption></figure>

봇이 GitHub에서 제공하는 OAuth UI를 사용하여 WasmEdge/wasmedge-db-examples GitHub 저장소에 액세스할 수 있도록 권한을 부여합니다.

그게 다입니다. github\_owner/github\_repo 저장소에 새 풀 리퀘스트를 생성하고 봇이 마법처럼 작동하는 것을 확인하세요!



## 사용자 정의 형태의 봇 설정하기

위의 프로세스에서는 먼저 [템플릿에서 봇 소스 코드](https://github.com/flows-network/github-pr-summary)를 자신의 GitHub 계정(예: your\_id/summarize-github-pull-requests 저장소)에 복제했습니다. 그런 다음 이 소스 코드에서 봇을 만듭니다. 자신의 계정에서 봇 소스 코드를 변경하여 봇 동작을 사용자 지정하거나 수정합니다.

봇 소스 코드에 대한 변경 사항을 GitHub에 푸시해야 flows.network가 해당 변경 사항을 반영하여 봇(즉, 플로우 함수)을 다시 빌드할 수 있습니다.

다음은 봇을 사용자 정의하기 위해 수행할 수 있는 몇 가지 간단한 코드 변경 사항입니다. 복제된 저장소의 src/github-pr-summary.rs 소스 코드 파일을 다음과 같이 변경하면 됩니다. flows.network가 변경 사항을 반영할 수 있도록 변경 사항을 GitHub에 푸시하는 것을 잊지 마세요.

1. 다른 모델을 선택하세요. 봇은 기본적으로 GPT 3.5 모델을 사용합니다. 상위 모델인 GPT4에 액세스할 수 있는 경우 다음 소스 코드에서 "GPT35Turbo"를 "GPT4"로 변경하세요. GPT4는 더 나은 코드 검토 기능을 제공하지만 비용이 더 비쌉니다.

```rust
static MODEL : ChatModel = ChatModel::GPT35Turbo;
// static MODEL : ChatModel = ChatModel::GPT4;
```

2. 엔지니어 ChatGPT 프롬프트. 예를 들어 ChatGPT가 숙련된 Java 개발자가 되어 Java 소스 코드 파일을 검토하도록 할 수 있습니다. 사용자 지정 프롬프트를 사용하여 봇이 코드의 특정 측면에 집중하도록 할 수 있습니다(예: 보안 문제 또는 성능에 집중). 제안된 변경 사항에 대한 코드 스니펫을 제공하거나 보안 문제에 대한 글머리 기호를 제공하는 등 특정 유형의 검토 의견을 제공하도록 봇에 메시지를 표시할 수도 있습니다. 다음 코드는 템플릿의 프롬프트입니다. 여기에서 힌트를 얻을 수 있는 많은 프롬프트 라이브러리가 있습니다.

{% code overflow="wrap" fullWidth="false" %}
```rust
let chat_id = format!("PR#{pull_number}");
    let system = &format!("You are an experienced software developer. You will act as a reviewer for a GitHub Pull Request titled \"{}\".", title);
    let mut reviews: Vec<String> = Vec::new();
    let mut reviews_text = String::new();
    for (_i, commit) in commits.iter().enumerate() {
        let commit_hash = &commit[5..45];
        let co = ChatOptions {
            model: MODEL,
            restart: true,
            system_prompt: Some(system),
            retry_times: 3,
        };
        let question = "The following is a GitHub patch. Please summarize the key changes and identify potential problems. Start with the most important findings.\n\n".to_string() + truncate(commit, CHAR_SOFT_LIMIT);
```
{% endcode %}

3. 봇을 더 친근감 있게 만드세요. 다음 소스 코드에서 "안녕하세요, 저는 flows.network의 코드 리뷰 봇입니다"로 시작하는 문장을 변경하여 봇의 풀 리퀘스트 댓글의 콘텐츠와 스타일을 변경할 수 있습니다. 예를 들어 커뮤니티 멤버에게 사용자 지정 인사말을 추가할 수 있습니다.

```rust
stlet mut resp = String::new();
    resp.push_str("Hello, I am a [code review bot](https://github.com/flows-network/github-pr-summary/) on [flows.network](https://flows.network/). Here are my reviews of code commits in this PR.\n\n------\n\n");
    if reviews.len() > 1 {
        let co = ChatOptions {
            model: MODEL,
            restart: true,
            system_prompt: Some(system),
            retry_times: 3,
        };
```

4. 리뷰 전략을 맞춤 설정하세요. 기본적으로 봇은 풀 리퀘스트의 모든 변경된 파일과 모든 커밋을 검토합니다. 특정 파일만 검토하거나 특정 개발자의 변경 사항만 검토하도록 소스 코드를 편집할 수 있습니다.



## 다수의 저장소에서 봇 사용

하나의 저장소에서 봇을 성공적으로 실행한 후에는 모든 저장소에 대해 코드 검토를 하고 싶을 것입니다! 물론 각 저장소에 대해 템플릿에서 별도의 봇을 배포할 수 있습니다. 그러나 이는 각 봇마다 관리해야 할 소스 코드가 따로 있다는 의미이며, 관리가 어려워질 수 있습니다. 동일한 봇 소스 코드를 사용하여 여러 봇을 만들 수 있습니다! flows.network에서는 각 봇을 " flow"라고 부릅니다.

먼저 " Create a flow(플로우 만들기)"를 클릭하고 플로우에 대한 봇 소스 코드를 가져올 수 있습니다. 봇 소스 코드는 템플릿에서 복제된 GitHub 저장소에 있습니다. PR 검토를 위해 봇을 배포하려는 저장소와 혼동하지 마세요!

다음으로 "고급" 섹션에서 봇이 PR을 검토할 대상 GitHub 저장소를 가리키도록 github\_owner 및 github\_repo 설정을 추가할 수 있습니다.

아래 그림은 "템플릿에서 복제한 기존 봇 소스 코드 저장소에서 새 봇(플로우)을 만들기"의 단계를 보여줍니다.

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/06/image9.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/06/image4.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/06/image7.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="https://www.cncf.io/wp-content/uploads/2023/06/image5.jpg" alt=""><figcaption></figcaption></figure>

마지막으로, 봇( flow)에 OpenAI API 키와 봇을 배포할 대상 GitHub 리포지토리에 대한 액세스 권한을 부여하는 프로세스를 거치게 됩니다.



## 그 다음 단계는 어떻게 되나요?

AI 지원 코드 리뷰는 빠르게 진화하는 분야입니다. CNCF의 [WasmEdge](https://github.com/WasmEdge/WasmEdge)는 코드 리뷰 봇 애플리케이션을 위한 효율적인 런타임을 제공합니다. 커뮤니티는 봇 템플릿을 개선하기 위해 여러 가지 새로운 아이디어를 실험하고 있습니다. 다음은 단기적으로 기대할 수 있는 몇 가지 개선 사항입니다!

Claude, PaLM 등과 같이 코딩 작업에 대해 학습된 추가 LLM을 지원합니다. CVE 데이터베이스에서 학습된 Llama 모델과 같이 미세 조정된 모델을 지원합니다. 이슈 트래커 및 프로젝트 관리 도구와 같은 다른 R\&D 관리 도구와 통합. GitHub 이외의 코드 호스팅 서비스를 지원합니다. 지금 바로 오픈 소스 소프트웨어 리포지토리의 코드 품질과 개발자 생산성을 높이세요!
