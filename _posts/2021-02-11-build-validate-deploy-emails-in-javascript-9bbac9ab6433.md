---
layout: post
title: "빌드. 검증. JavaScript에서 전자 메일을 배포합니다."
author: "Logger"
thumbnail: "undefined"
tags: 
---


![Image for post](https://miro.medium.com/max/12000/0*zD7bqRDNXGGQENL3)

GDPR 요건을 충족시키기 위한 노력의 일환으로, 앱에 이메일 통합을 추가하는 것을 더 이상 미룰 수 없다는 것이 분명했다. 이메일 마케팅 웹 사이트는 "아름답고 수상 경력이 있는 템플릿"을 제공했지만, 부족한 점은 GitHub를 통해 동일한 검토 및 배포 프로세스를 수행할 수 있도록 변경 사항을 추적한 것과 같은 방식으로 이메일을 유지 관리하는 것이었습니다.

# 환경

설정이 내 설정을 정확하게 미러링할 필요는 없지만 일부 컨텍스트를 제공하는 데 사용할 수 있습니다.

# 백엔드 통합

지속적으로 배포 가능한 전자 메일을 만드는 것은 단순히 배치를 쉽게 하는 것이 아니라 개발을 쉽게 하는 것입니다. 의사 결정 경로를 안내하는 것은 백엔드 서비스에서 이메일 템플릿을 사용하는 방법을 설명하는 것을 의미합니다.

## 로컬 파일 시스템에서 전자 메일 만들기

메일러 보내기를 사용하여 트랜잭션 이메일을 보냈습니다. 제목 줄, 텍스트 형식, HTML 내용, 보낸 사람 및 수신인에 대한 전자 메일 API 필수 필드입니다. 첫 번째 접근법에서, 나는 동등한 HTML 콘텐츠를 설계하고 작성해서 서버 런타임에 메모리에 로드되는 파일에 저장했다. HTML 컨텐츠에서 제목 줄(<title> 태그)과 동등한 텍스트 컨텐츠(npm 라이브러리 html-to-text 사용)를 추출할 수 있었습니다.

이메일용 HTML 콘텐츠 작성이 어렵기로 악명이 높았던 것으로 밝혀졌다. 이 표준(현재 20년 이상 된)은 현대적이고 모바일 친화적인 개발보다 앞선 시대에 만들어졌다. 기존 이메일 템플릿을 복사한 다음 필요에 맞게 수정하여 프로세스를 전복시킬 수 있다고 생각했지만, 결코 제가 바라는 만큼 결과가 좋은 것은 아니었습니다.

고맙게도 Mailjet Markup Language(또는 줄여서 MJML)라는 형식을 찾았습니다. MJML은 동일한 HTML로 컴파일할 수 있는 단순하고 강력한 HTML 유사 태그를 몇 개 제공했습니다. 개발 효율성을 개선할 수 있는 솔루션을 찾고 싶었지만, 런타임 효율성의 비용을 부담했습니다. 디스크에서 MJML 파일을 로드하고, HTML로 변환하고, 텍스트와 제목 콘텐츠를 추출하는 데 비용이 들었다. 트랜잭션 이메일이 더 필요하게 되면 런타임 비용이 만만치 않을 수 있다는 우려가 들었습니다.

런타임 비용이 한몫 했지만, MJML로 전환되면서 코드 유효성 검사가 성가시게 되었습니다. 파일을 브라우저에서 보려면 먼저 HTML로 변환해야 했습니다. 개발 피드백 루프를 단축하기 위해 서버측 렌더러가 필요했습니다. 이 서버를 마음대로 사용하면서 백엔드 서버가 이 서비스를 사용하여 전자 메일 API 데이터를 검색할 수 있는지 생각해 보았습니다.

## "이메일 서비스" 호스팅

이 접근법에는 몇 가지 이점이 있었다. 첫째, 백엔드 서버는 더 이상 파일 변환 작업을 수행할 필요가 없었습니다. 이 "이메일 서비스"는 JSON 문서로 단일 엔드포인트에서 필요한 콘텐츠를 제공할 수 있습니다. 또 다른 점은 이메일 내용을 변경해도 백엔드 서버를 변경할 필요가 없다는 것입니다. 그 결과, 전자 메일 템플리트는 백엔드 서버와 분리될 수 있으므로, 전자 메일 템플리트를 자체 전용 패키지로 분리해야 한다는 개념이 강화되었습니다.

호스트된 전자 메일 서버가 자체 위험 집합을 사용하여 왔습니다. 먼저, 나는 내 모든 이메일 템플릿을 인터넷에 노출시켜야 할 것이다. 결국 사용자가 수행한 행동에 따라 콘텐츠를 이용할 수 있게 될 것이기 때문에 그 자체로 큰 문제는 아니었지만, 다소 허술하게 느껴지기도 했다.

또 다른 위험은 가용성이었다. 파일 시스템 또는 메모리에 있는 항목이 로컬일 때는 항상 액세스할 수 있습니다. 엔드포인트에서 다운로드하면 다음과 같은 질문이 해결되어야 합니다.

그리고 호환성에 대한 문제가 있었다. MailerSend를 사용하면 변수 보간에 대한 자리 표시자를 정의할 수 있습니다. 예를 들어, 사용자가 데이터 사본을 다운로드하도록 요청할 수 있는 기능이 있습니다. 백엔드는 데이터 다운로드 요청을 받으면 사용자가 이메일로 받을 임시 링크에서 액세스할 수 있는 아카이브로 사용자 데이터를 업로드합니다. 이 링크는 정적 정보가 아니므로, 플레이스홀더가 템플릿에 설명되어 전자 메일을 보낼 준비가 되었을 때 URL로 대체됩니다.

이 문제는 변수가 추가되거나 변경될 때 발생합니다. 백엔드에서 모든 변수에 대한 값을 제공하지 않으면 손상된 것처럼 보이고 잘못된 전자 메일을 보낼 수 있습니다. 솔루션은 동일한 전자 메일의 여러 버전을 지원해야 한다는 것을 의미하지만, 백엔드가 이러한 전자 메일의 유일한 소비자이기 때문에 비효율적이고 불필요하게 느껴졌습니다.

## 컴파일된 이메일 템플릿

파일 시스템 이메일이 불필요한 런타임 오버헤드를 생성하고 호스팅된 이메일이 가용성과 호환성에 대한 질문으로 이어지면서 궁극적으로 해결한 세 번째 옵션은 이러한 MJML 이메일 파일을 백엔드와 호환되는 코드 라이브러리로 컴파일하는 것이었습니다. 이렇게 하면 백엔드에서 사용 중인 버전을 항상 알 수 있기 때문에 호환성 문제가 해결됩니다. 그리고 필요한 정보가 컴파일 시간에 계산됨에 따라 가용성과 런타임 문제가 완화된다.

백엔드는 JavaScript로 작성되므로, 이 컴파일의 JavaScript 구현에 초점을 맞출 것입니다. 그러나, 이 생각은 여전히 다른 어떤 언어 선택에도 적용된다.

나는 웹팩을 이용하여 나만의 MJML 웹팩 로더를 작성했다. 각 전자 메일 템플릿을 제목, 텍스트 및 HTML 콘텐츠 필드가 포함된 Javascript 개체로 변환했습니다. 코드는 다음과 같습니다.

그런 다음 백엔드 서버에 코드를 통합했습니다. 모노레포 내의 별도의 라이브러리로서, 나는 로컬에서 관리되는 npm 라이브러리와 관련된 불친절함을 완화하기 위해 npm 워크스페이스를 사용했다. 백엔드는 Firebase Functions에 의해 제공되었기 때문에, 이 통합은 자체적인 스파이크 피트와 스윙 블레이드와 함께 제공됩니다. 그리고 위로하는 것은 재미있지만, 다른 토론을 위해 남겨두는 것이 좋을 것이다.

# 개발 서버

이메일 템플릿을 호스팅하는 데 더 이상 이메일 서비스가 필요하지 않음에도 불구하고, 개발 과정에서 제가 구축하던 것이 제가 원하는 대로 보이도록 하는 것이 여전히 도움이 되었습니다. 이메일 패키지에는 `views/inctv`라는 디렉터리가 있습니다. 이 경로에는 라이브러리에 내장되어 백엔드 서버에서 사용할 모든 전자 메일 템플릿이 포함되어 있습니다.

그런 다음 Express를 설정합니다.MJML 파일을 HTML 및 브라우저에 렌더링할 일반 텍스트 문서로 변환하기 위한 사용자 지정 엔진이 있는 JS 서버. 각 MJML 파일은 확장자가 .mjml인 루트 파일 이름, 확장자가 .html인 루트 파일 이름, 확장자가 .txt인 루트 파일 이름의 세 가지 서버 끝점에 대응된다. 확장명이 없는 확장과 .html 확장자는 동일한 HTML 출력을 반환하고, 확장명이 .txt인 확장자는 일반 텍스트 출력을 반환했습니다.

## 가변 보간법

앞에서 언급했듯이, 백엔드 기능 중 하나를 통해 사용자는 다운로드 가능한 데이터 아카이브를 요청할 수 있습니다. 결과 액션은 정보에 대한 사용자 지정 링크가 포함된 전자 메일을 사용자에게 보냈습니다. 결과 내보내기 전자 메일에 해당 링크를 삽입하기 위해 변수 보간이 사용되었습니다. 그로 인해 변동성이 생겼기 때문에, 저는 결과적인 이메일에 대해 예상된 행동을 보장할 수 있는 방법이 필요했습니다. 그래서 저는 서버를 업데이트하여 쿼리 매개 변수를 HTML과 일반 텍스트 형식으로 해석하고 변환할 수 있는 보간 값으로 지정했습니다. 코드는 다음과 같습니다.

## 자산 관리

자산에는 회사 이름, 앱 로고 URL, 개인 정보 보호 정책 링크, 서비스 약관 및 가입 옵션 등이 포함됩니다. HTML 출력이 생성되기도 전에 이 정보가 이메일에 수동으로 주입될 수 있지만, 이 정보를 단일 엔드포인트로 통합하면 이메일뿐만 아니라 앱 전체에 도움이 될 것이라고 생각합니다.

이전에 서버 엔드포인트에서 전자 메일 검색을 반대했지만 엔드포인트에서 회사 구성을 호스팅하면 다음과 같은 이점이 있습니다.

엔드포인트에서 구성을 검색하는 옵션이 있으면 이제 해당 데이터를 사용하여 전자 메일 템플릿을 제공할 수 있습니다. 전자 메일 서버에 미들웨어를 추가하여 다음과 같이 해당 구성을 쿼리 매개 변수와 병합했습니다.

# 연속 배포

브라우저에서 이메일을 관찰하여 JavaScript 객체로 이메일을 작성하고 템플릿의 유효성을 보장할 수 있습니다. 마지막으로, 이 모든 기능을 배치에 통합해야 합니다. 중요한 요소는 전자 메일 라이브러리가 항상 백엔드 서버와 동기화되어야 한다는 것입니다. GitHub Actions를 사용하여 배포를 관리하므로 백엔드 서비스 워크플로우에 다음과 같은 변경 사항을 적용해야 했습니다.

저는 원래 e-메일이 다양한 종류의 웹 앱을 사용할 때 사용자가 상호작용하는 많은 것임을 깨닫기 전까지는 e-메일이 일반적으로 애플리케이션에 접하는 것처럼 보인다고 생각했습니다. 전자 메일에 대한 검사 동작이 나머지 코드 기반과 일치하도록 하는 데 큰 가치가 있으며, 이 접근 방식은 약간의 노력과 몇 개의 작은 코드 조각으로 이를 가능하게 하는 방법을 보여준다.

읽어주셔서 감사합니다!