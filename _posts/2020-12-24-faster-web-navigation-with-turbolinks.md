---
layout: post
title: "더 빠른 웹 탐색을 위한 터보 링크"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/turbolinks.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/turbolinks.png?fit=730%2C487&ssl=1)

레일즈 개발자인 경우 터볼링크를 알고 있을 가능성이 높습니다. Turbolinks는 웹 페이지를 통한 탐색 속도를 높이기 위한 유연하고 가벼운 JavaScript 라이브러리입니다. Turbolinks는 일반적인 전체 페이지 로드를 다중 페이지 애플리케이션의 부분 로드에 대체하여 웹 페이지 성능을 향상시킵니다.

하지만 어떻게 정확히 이것을 성취할 수 있을까요? Turbolinks는 자동으로 페이지를 가져오고, DOM의 본문을 교환하고, `헤드`와 혼합하여 작동합니다. 따라서 라이브러리는 전체 로드를 추론하지 않고도 페이지의 변경된 내용을 이해할 수 있습니다.

Turbolinks는 일반적으로 애플리케이션 내의 페이지로 사용자를 유도하는 모든 `<a ref> 링크를 가로채 AJAX를 통해 요청으로 전송하므로 본문을 수신 콘텐츠로 교체할 수 있다.

## 특징 및 이점

Turbolinks는 자동 탐색 최적화에 중점을 두므로 너무 많은 변경 없이 페이지가 자동으로 빨라집니다. Turbolinks는 사용자가 방문하는 모든 페이지를 내부 캐시 내에 저장하므로 동일한 페이지에 액세스해야 할 때 Turbolinks가 캐시에서 페이지를 검색하여 성능을 지속적으로 향상시킬 수 있습니다. 또한 하이브리드 애플리케이션의 iOS 및 Android에 맞게 페이지를 조정하여 내비게이션 컨트롤의 후속 작업을 수행할 수 있습니다.

Turbolinks 라이브러리는 HTML 페이지, Ruby on Rails, Node.js, Laravel, 부분 조각 또는 JSON과 통합될 수 있다. 또한 브라우저의 적응력도 강조합니다. Turbolinks는 클릭을 차단하여 백그라운드에서 동작을 변경하는 동시에 브라우저가 History API를 통해 앞뒤로 이동하는 방법을 이해하는 데 도움이 됩니다.

터볼링크는 또한 한 페이지에서 다른 페이지로 이동할 때 유명한 "깜빡" 효과를 피할 수 있는 즉각적인 혜택을 가져다 준다. 또한 Turbolinks는 HTML5 History API에 의존하기 때문에 모든 최신 브라우저에서 지원되며, 부작용 없이 이전 브라우저에서 정상 탐색으로 저하될 수 있다.

이 기사에서는 Turbolinks 라이브러리에 대해 좀 더 자세히 살펴보고 업사이드, 다운사이드 및 애플리케이션이 어떻게 이점을 얻을 수 있는지 알아보겠습니다. 안으로 들어가자!

## 설정 및 사용

일반 HTML 및 JavaScript 애플리케이션을 사용할 경우 `헤드` 태그 내에 있는 JavaScript 파일을 가져오기만 하면 됩니다.

라이브러리는 처음에 Rails 애플리케이션을 대상으로 했기 때문에 가장 널리 사용되는 언어입니다.

Ruby on Rails 응용 프로그램에 설치하려면 `Gemfile`에 해당 보석을 추가하십시오.

```undefined
gem 'turbolinks', '~> 5.2.0'
```

bundle install 명령을 실행하면 Rails가 gem 다운로드 및 설치 작업을 수행합니다.

또한 JavaScript 매니페스트 파일에 다음 설명을 추가해야 합니다.

```coffeescript
//= require turbolinks
```

노드 프로젝트와 함께 사용할 계획이라면 설치가 훨씬 더 간단합니다.

```coffeescript
npm i turbolinks
```

JavaScript 번들 파일에 다음 코드 조각을 포함해야 합니다.

```js
    var Turbolinks = require("turbolinks");
    Turbolinks.start();
```

#### 서버에서 리디렉션

Turbolink는 예를 들어, 이러한 환경에서 비롯되었기 때문에 Node 애플리케이션보다 Rails 환경 내에서 더 원활하게 작동합니다.

앞서 언급한 자동 리디렉션을 사용합니다. `redirect_to`를 통해 레일즈 내의 새 페이지로 리디렉션하면 Turblinks Rails는 요청에 `Turbolinks-Location`이라는 헤더를 자동으로 추가합니다.

이 헤더는 서버로부터 아무런 도움도 받지 못한 리디렉션 흐름에서 지정된 요청이 오는 경우를 식별하는 Turbolinks에 필수적입니다.

반면, 노드 기반 애플리케이션을 처리할 때 Turbolinks가 이 헤더를 받는 것 외에 이러한 유형의 작업을 식별할 수 있는 방법은 없으므로 클라이언트가 이 요청을 수동으로 처리해야 합니다.

### 자바스크립트: '머리' vs '몸' 접근법

일반적으로 성능을 위해 자바스크립트 파일을 로드하고 처리하기 전에 전체 페이지를 렌더링할 수 있도록 `본문` 요소의 하단에 `스크립트` 태그를 항상 포함시켜야 한다.

그러나 Turbolinks는 정반대를 요구하며 스크립트를 `헤드` 태그 안에 배치하도록 권장합니다.

터볼링크가 헤드를 무시한 채 페이지 본문을 가져오고 파싱하기 때문이다. 스크립트를 본문 내에 그대로 두면 탐색할 때마다 동일한 스크립트가 계속해서 로드됩니다.

페이지 로드를 차단하지 않고 첫 페이지 로드를 더 빠르게 수행하려면 스크립트의 가져오기를 비동기식으로 선택할 수 있습니다.

```xml
<script async src="..."></script>
```

또는 대부분의 최신 웹 브라우저에서 허용되는 지연 속성을 추가하여 페이지가 로드된 후에만 스크립트가 처리되도록 할 수 있습니다.

스크립트가 자주 변경되는 경우 스크립트 태그에 추가 `데이터 터볼링크-트랙` 특성을 제공하여 스크립트 번들이 변경된 경우에만 전체 페이지를 다시 로드하도록 하는 것이 좋습니다.

### 분석 라이브러리

Google Analytics(GA)와 같은 분석 라이브러리의 경우 다양한 요소에 주의를 기울이는 것이 중요합니다.

```xml
<!-- Google Analytics -->
<script>
window.ga=window.ga||function(){(ga.q=ga.q||[]).push(arguments)};ga.l=+new Date;
ga('create', 'UA-XXXXX-Y', 'auto');
ga('send', 'pageview');
</script>
<script async src='https://www.google-analytics.com/analytics.js'></script>
<!-- End Google Analytics -->
```

GA는 일반적으로 사용자가 `script` 요소 다음에 `analytics.js` 자바스크립트 파일을 로드하도록 요구한다. 항상 전자는 몸 안에서, 후자는 머리 안에서 수입하도록 하세요. 이렇게 하면 각 탐색 이벤트가 분석 도구에 적합한 변수를 다시 제공합니다.

### 기본 진행률 표시줄

좋습니다. 그러나 요청 중인 링크가 반환되는 데 너무 오래 걸리면 어떻게 됩니까? 이는 여러 가지 이유로 인해 발생할 수 있는 일반적인 사용 사례이며, 예기치 않은 네트워크 지연이 더 일반적입니다.

Turbolinks는 기본 진행률 표시줄을 활성화하여 이 문제를 해결합니다. 페이지가 처리되는 데 500ms 이상 걸리는 경우 Turbolinks는 사용자가 원하는 대로 덮어쓸 수 있는 일부 기본 제공 CSS 스타일로 진행 표시줄을 표시합니다. 지연은 API를 통해 사용자 정의할 수도 있습니다.

결국 div 요소는 `turbolinks-progress-bar`라는 CSS 클래스로 생성된다.

### 자동 캐싱

응용 프로그램을 탐색하고 있으며 몇 번의 클릭으로 동일한 페이지에 액세스하려고 합니다. 이렇게 하려면 브라우저 기록으로 이동하여 이전 페이지의 링크를 클릭합니다. 일반적으로 앱은 서버에서 전체 페이지를 다시 삽입하여 명령에 따릅니다.

백엔드에서 모든 것을 다시 요청하지 않는 것이 좋지 않을까요? 페이지가 캐시된 경우?

동일한 페이지에 표시되는 데이터가 빠르게 변경된다고 상상해 보십시오. 캐시 데이터가 이미 오래되었을 수 있으므로 기록으로 돌아가서 캐시에서 페이지를 검색하는 것은 작동하지 않습니다.

두 시나리오 모두에서 Turbolinks는 캐시 버전을 직접 선택하여 표시하거나, 요청이 완료되면 백엔드로부터 새 복사본을 병렬로 가져와 화면에서 대체할 수 있는 좋은 방법을 제공합니다.

이는 모두 자동 캐싱 메커니즘으로 인해 가능합니다.

그러나 페이지가 캐시되기 전에 일부 코드를 실행하려면 다음을 통해 이 작업을 수행할 수 있습니다.

```js
document.addEventListener("turbolinks:before-cache", function() {
  // your code here
});
```

기능을 사용하지 않으려면 `메타` 태그에 `캐시 없음` 지시문을 추가하십시오.

```xml
<meta name="turbolinks-cache-control" content="no-cache">
```

### 요청 사용자 정의

Turbolinks의 또 다른 멋진 기능은 요청을 제출하기 전에 가로채고 사용자 지정할 수 있는 기능입니다.

예를 들어 Turbolinks에서 트리거된 모든 요청에 대해 임의 ID를 전송하려고 한다고 가정해 보겠습니다. 이를 위해 다음 이벤트 수신기를 매핑하기만 하면 됩니다.

```js
document.addEventListener("turbolinks:request-start", function(event) {
  event.data.xhr.setRequestHeader("X-My-Custom-Request-Id", "MyId");
});
```

깔끔하죠? 사용자 지정 가능한 자세한 내용은 API Reference를 참조하십시오.

## 결론

터볼링크는 꽤 오랫동안 사용되어 왔고 상당한 수준의 성숙도를 달성했다. 이를 지원하는 훌륭한 커뮤니티를 통해 빠르게 발전했으며 많은 웹 개발자들이 애플리케이션을 위한 더 나은 성능을 달성할 수 있도록 지원했습니다.

API Reference는 반드시 읽어야 하는 것이므로, 이 작고 강력한 라이브러리의 잠재력과 탐색 시간 및 애플리케이션 성능을 향상시킬 수 있는 기능에 대해 자세히 이해하기 위해 반드시 검토해야 합니다.