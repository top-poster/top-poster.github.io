---
layout: post
title: "2021년 JavaScript에서 쿼리 문자열 값 가져오기"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![Code](https://miro.medium.com/max/11520/0*vCZhrMG6Jwvds8PZ)

과거에는 일반 JavaScript에서 URL의 쿼리 문자열에서 키-값 쌍을 검색하는 간단한 문제에 대한 간단한 솔루션이 없었습니다.

다음 URL을 살펴보겠습니다. http://example.com/search?someParam=someValue

이러한 일반적인 작업에는 기본 제공 솔루션이 필요했습니다. 이 솔루션의 이름은 `UrlSearch Params`입니다. 대부분의 최신 브라우저와 호환됩니다. Internet Explorer(인터넷 익스플로러)를 지원해야 하는 경우에도 폴리필을 제공하여 Internet Explorer(인터넷 익스플로러)를 사용해야 합니다.

이 기능이 필요할 때 대개 다음 상황 중 하나에 해당됩니다.

# 현재 웹 사이트의 쿼리 문자열 값 가져오기

먼저 현재 URL의 검색 부분이 필요합니다. window.location을 통해 얻을 수 있습니다.수색하다

참고: window.location은 사용할 수 없습니다.그 이유는 다음 절에서 설명될 것이다.

출력:

```undefined
일부 매개 변수 값
otherParamotherValue
```

# 모든 URL의 쿼리 문자열 값 가져오기

여기서 한 가지 주의할 점은 전체 URL을 구문 분석하려면 먼저 검색 매개 변수 부분(`?` 이후 모든 항목)을 분리해야 한다는 것입니다. 예제 URL의 경우 이 부분은 `?someParam=someValue`입니다.

`?`가 해당 문자열의 일부인지 여부는 중요하지 않다는 점에 유의하십시오. UrlSearch Params의 건설사가 그것을 다룰 것이다.

출력:

```undefined
일부 매개 변수 값
otherParamotherValue
```

# 결론

복잡한 홈메이드 정규 표현으로 저글링하던 시대는 끝났다. 이제 URL 검색 파람스를 안전하게 사용할 수 있습니다. 오래된 브라우저를 지원해야 하는 사람들에게 좋은 폴리필이 있습니다.

# 참고문헌