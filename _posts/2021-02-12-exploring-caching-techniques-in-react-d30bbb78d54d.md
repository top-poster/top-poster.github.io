---
layout: post
title: "반응형 캐싱 기술 탐색"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![Components and their information](https://miro.medium.com/max/2278/1*LT0jhp8UwZRahePKZ9bEpg.png)

반응에서 데이터를 가져오는 것도 한 가지입니다. 이 데이터를 저장하고 캐슁하는 것은 또 다른 사례입니다. 가능성은 무궁무진해 보이고 차이점도 미묘하기 일쑤여서 올바른 기술을 선택하는 것이 때로는 난관에 봉착하기도 한다.

오늘은 다양한 기술을 살펴보고 세부 정보와 세부 정보를 살펴보겠습니다. memo를 사용할까요, memo를 사용할까요? `useState`와 컨텍스트를 사용하여 데이터를 저장해야 합니까? 작업을 마치면 데이터 캐슁과 관련하여 정보에 입각한 선택을 편안하게 하실 수 있습니다. 당신은 모든 안팎에 대해 배우게 될 것입니다.

그리고 많은 애니메이션 GIF가 있습니다. 또 무엇을 바랄 수 있겠는가?

시작해 봅시다!

# 당사의 데이터

코드를 자세히 살펴보기 전에 구성 요소 대부분에서 가져올 데이터를 간단히 살펴볼 수 있습니다. API 역할을 하는 파일은 다음과 같습니다.

이 코드는 프로젝트의 경로 `/api/people`에 대한 요청을 할 때 실행됩니다. 보다시피 두 가지 속성을 가진 개체를 반환합니다.

`랜덤 번호` 속성은 캐시된 데이터를 프런트 엔드에서 렌더링하는지 여부를 시각화하는 데 도움이 됩니다. 그냥 계속 읽으세요. 곧 말이 될 거예요.

setTimeout을 사용하여 네트워크 지연을 시뮬레이션합니다.

## 피플 컴포함

API에서 데이터를 렌더링하면 PeopleEnder라는 컴포넌트로 전달하겠습니다. 다음과 같습니다.

![PeopleRenderer component.](https://miro.medium.com/max/1108/1*pCw1cUWOuSuowUfpaBbI9g.png)

![Two components displaying the same (cached) data.](https://miro.medium.com/max/1108/1*CGi7Zu8F3rxfdfVMT-E6Pw.png)

![Two components rendering different (non-cached) data.](https://miro.medium.com/max/1110/1*iO-NeG3BNNx5brRx3kEjsg.png)

지금 말하고 있는 모든 것은, 첫 번째 기술을 살펴보도록 하자.

# 효과 사용

구성 요소 내부에서는 `효과` 후크를 사용하여 데이터를 가져올 수 있습니다. 그런 다음 `useState`를 사용하여 로컬(구성 요소 내부)에 저장할 수 있습니다.

빈 어레이를 두 번째 매개 변수로 전달하면(11번 라인 참조), `use Effect` 후크는 구성 요소가 DOM에 마운트될 때 실행되며, 그 후에만 실행됩니다. 구성 요소가 다시 렌더링되면 다시 실행되지 않습니다. `한 번 실행`한 후크야

이러한 방식으로 `use Effect`를 사용하면 DOM에 구성 요소의 인스턴스가 여러 개 있을 때 모두 개별적으로 데이터를 가져올 수 있습니다(장착된 경우).

![Animation: The components fetch data individually.](https://miro.medium.com/max/1190/1*sIjJj7wyvEDMVZJPf48Fcw.gif)

이 기술에는 아무런 문제가 없다. 때때로, 이것이 우리가 원하는 것이다. 그러나 데이터를 한 번 가져와 다른 모든 인스턴스에서 캐슁하여 재사용하는 경우도 있습니다. 우리는 그것을 이루기 위해 몇 가지 기술을 사용할 수 있다.

# 메모화

메모는 매우 간단한 기술을 위한 멋진 단어이다. 함수를 만들고 함수를 호출할 때마다 함수 호출 결과를 캐시에 저장한 후 반환한다는 의미입니다.

이 메모된 함수를 처음 호출하면 결과가 계산되거나 함수 본문 내에서 수행 중인 작업이 계산됩니다. 결과를 반환하기 전에 다음 입력 매개 변수로 생성된 키의 캐시에 저장합니다.

이 보일러 플레이트 코드를 만드는 것은 빠르게 번거로워질 수 있으므로, Lodash, Underscore와 같은 인기 있는 라이브러리는 기억 기능을 쉽게 만드는 데 사용할 수 있는 유틸리티 기능을 제공합니다.

## 데이터 가져오기를 위한 메모 활용

데이터를 가져올 때 이 기술을 활용할 수 있습니다. 우리는 `getData` 함수를 만들어 가져오기 요청이 완료되면 해결된 `약속`을 반환합니다. 이 `약속`을 메모합니다.

이 예에서는 오류를 처리하지 않았습니다. 그것은 특히 우리가 메모(거부된 약속도 메모될 수 있어 문제가 될 수 있다)를 사용할 때 자체적인 기사를 쓸 만하다.

이제 `이용 효과` 후크를 다음과 같은 다른 후크로 대체할 수 있습니다.

getData(데이터 가져오기)의 결과가 메모되어 있으므로, 당사 구성 요소는 모두 마운트될 때 동일한 데이터를 수신합니다.

![Animation: Our components use the same memoized Promise.](https://miro.medium.com/max/1190/1*OqOTRCkSvFgaSjH1V1fQvw.gif)

또한 구성 요소의 첫 번째 인스턴스를 마운트하기 전에 `memoize.tsx` 페이지를 열 때 데이터가 이미 사전 추출되어 있음을 언급할 필요가 있습니다. 왜냐하면 우리는 페이지 상단에 포함된 별도의 파일에 getData 기능을 정의했고 그 파일이 로드될 때 약속이 생성되기 때문입니다.

또한 다음과 같이 메모된 기능의 캐시 속성에 새로운 캐시(Cache)를 할당하여 메모된 기능의 캐시를 무효화(비우기)할 수 있습니다.

```undefined
getData.cache = 새 memoize를 입력합니다.캐시(;
```

또는 기존 캐시(`맵` 인스턴스)를 지울 수 있습니다.

```undefined
getData.cache.clear(;)
```

하지만 이것은 Lodash 고유의 기능입니다. 다른 라이브러리에는 다른 솔루션이 필요합니다. 여기서는 실행 중인 캐시의 무효화를 확인할 수 있습니다.

![Animation: Resetting the cache of the memoized getData function.](https://miro.medium.com/max/1190/1*Vnf6nV7_FS1eqen0RSd7hw.gif)

# 대응 컨텍스트

또 다른 인기 있고 잘 논의되었지만 종종 오해를 받는 도구는 React Context이다. 참고로 Redux와 같은 툴은 다시 한 번 대체하지 않습니다. 그것은 국가 관리 도구가 아니다. 마크 에릭슨은 온라인에서 힘든 싸움을 하고 있고 그 이유를 계속 설명하고 있다. 나는 그 주제에 대한 그의 최근 기사를 읽는 것을 강력히 추천한다.

그리고 정말 관심이 있으시다면 제 관련 기사도 읽어보시길 바랍니다.

그렇다면 컨텍스트란 무엇일까요? 구성 요소 트리에 데이터를 주입하는 메커니즘입니다. 일부 데이터가 있는 경우 구성 요소 계층에서 높은 위치에 있는 구성 요소 내부에 `useState` 후크를 사용하여 이 데이터를 저장할 수 있습니다. 그런 다음 컨텍스트 `공급자`를 사용하여 트리로 데이터를 주입한 후 아래 구성 요소의 데이터를 읽거나 사용할 수 있습니다.

예를 들면 이해하기가 더 쉽다. 먼저 새 컨텍스트를 만듭니다.

그런 다음 사용자 구성요소를 렌더링하는 구성요소를 컨텍스트 "공급자"로 포장합니다.

12번 라인에서는 원하는 건 뭐든지 드릴 수 있습니다. 트리 아래 아래쪽에 있는 `피플` 구성 요소를 렌더링할 것입니다.

우리는 컨텍스트 후크를 사용하여 공급자로부터 가치를 소비할 수 있습니다. 결과는 다음과 같습니다.

![Animation: Consuming data from a Context.](https://miro.medium.com/max/1190/1*Dxd_0uv_1J9KysLM3xJXCA.gif)

한 가지 중요한 차이점에 주목하세요! 위의 애니메이션이 끝나면 "새 시드 설정" 버튼을 누릅니다. 이를 통해 컨텍스트 `공급자`에 저장된 데이터를 다시 가져올 수 있습니다. 작업이 완료되면(750ms 이후) 새로 가져온 데이터는 공급자(Provider)의 새로운 값이 되고 피플(People) 구성요소는 다시 렌더링됩니다. 보시다시피, 그들은 모두 같은 데이터를 공유합니다.

이것은 우리가 위에서 살펴본 메모화 예와 큰 차이입니다. 이 경우 각 구성요소는 `useState`를 사용하여 메모된 데이터의 복사본을 저장하였다. 이 경우 컨텍스트를 사용하고 사용하므로 복사본을 저장하지 않고 동일한 개체에 대한 참조만 사용합니다. 이것이 우리가 `공급자`의 값을 업데이트할 때 모든 구성요소가 동일한 데이터로 업데이트되는 이유입니다.

# 메모 사용

마지막으로 use Memo를 잠깐 살펴본다. 이 후크는 로컬 수준의 캐싱 형태일 뿐이라는 점에서 우리가 살펴본 다른 기술과는 다릅니다. 구성 요소의 한 인스턴스 안에서 말입니다. prop-drilling 또는 종속성 주입(예: React Context)과 같은 해결 방법이 없으면 `useMemo`를 사용하여 여러 구성 요소 간에 데이터를 공유할 수 없습니다.

useMemo는 최적화 도구이다. 이 옵션을 사용하여 구성 요소의 모든 리렌더에 대한 값이 재계산되지 않도록 할 수 있습니다. 설명서에 나와 있는 것보다 더 잘 설명되어 있습니다. 예를 들어 보겠습니다.

마지막으로 7번 라인에 use Memo를 사용합니다. 함수 호출 결과를 메모하고 power라는 변수를 저장한다. 우리의 함수는 2의 힘으로 `나이`를 합한 숫자에 난수를 더한 숫자를 반환한다. 연령 변수에 의존성이 있어 사용메모 통화의 의존성 주장에 전가하는 것이다.

이 기능은 연령의 값이 변경될 때만 실행됩니다. 만약 우리의 컴포넌트가 리렌더되고 `나이`의 값이 변하지 않았다면, `use Memo`는 단순히 메모한 결과를 반환할 것이다.

이 예에서 `power`의 계산은 그리 복잡하지 않지만, 우리의 기능이 더 무겁고 구성 요소를 자주 다시 렌더링해야 할 때 이러한 계산을 통해 얻을 수 있는 이점을 상상할 수 있다.

마지막 두 애니메이션은 어떤 일이 일어나는지 보여줄 것이다. 먼저 임의 번호를 업데이트하고 연령 값에 영향을 주지 않습니다. 따라서 useMemo가 동작(구성 요소를 다시 렌더링해도 power의 값은 변경되지 않음)으로 표시됩니다. 버튼을 클릭할 때마다 구성 요소가 다시 렌더링됩니다.

![Animation: Many re-renders, but the value of pow is memoized with useMemo.](https://miro.medium.com/max/1190/1*9RRMNqyBrwKQxLVDFwLguQ.gif)

하지만 나이 값을 바꾸면 use Memo 호출이 enge 값에 의존하기 때문에 power도 다시 렌더링된다.

![Animation: Our memoized value gets updated when we update the dependency.](https://miro.medium.com/max/1190/1*lIHfc1smxTpCulGsPtOXvw.gif)

# 결론

자바스크립트에는 데이터를 캐싱하는 많은 기술과 유틸리티가 있다. 이 글은 겉만 긁적거리지만, 개발 과정에서 여러분이 가지고 갈 수 있는 지식을 제공하기를 바랍니다.

이 문서에 사용된 모든 코드는 GitLab의 내 리포지토리에서 찾을 수 있습니다.

시간 내주셔서 감사합니다!