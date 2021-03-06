---
layout: post
title: "반응 기능 구성 요소를 사용하여 D3.js를 렌더링하는 5단계"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![Overlapping orange and blue circles](https://miro.medium.com/max/1600/1*FRaslVFg_cZnEJUuEndI_w.png)

데이터 시각화는 정보와 데이터의 그래픽 표현이다. 시각적 요소를 사용하여 데이터의 추세, 특이치 및 패턴을 표시합니다. 특히 시계열과 같이 데이터가 많을 때 효율적이며, 데이터 포인트는 시간 순서로 인덱싱됩니다.

D3.js는 웹 표준을 사용하여 데이터를 시각화하기 위한 자바스크립트 라이브러리이다. D3는 "데이터 기반 문서"를 나타냅니다. 임의 데이터를 DOM(Document Object Model)에 바인딩한 다음 데이터 기반 변환을 문서에 적용합니다. 우리는 D3를 사용하여 숫자 배열에서 HTML 테이블을 생성할 수 있습니다. 또는 동일한 데이터를 사용하여 부드러운 전환과 상호 작용을 가진 대화형 SVG(Scalable Vector Graphics) 막대 차트를 생성할 수 있습니다.

이전 기사에서는 C3이 D3의 모든 복잡성을 알지 않고도 차트를 빠르게 구축할 수 있다는 점에 주목했다. 차트를 만들기 위해 C3을 사용하려면 몇 줄의 코드만 필요합니다. 그러나 X축과 Y축, 각 데이터 점의 모양, 크기 및 위치 등을 고려해야 하므로 D3를 사용하여 동일한 차트를 구현하는 데 많은 노력이 필요합니다.

D3의 복잡성과 진부한 세부 사항에 대해 자세히 알아보고자 하는 이유는 무엇입니까?

D3는 그래픽 표현을 구성할 수 있는 풍부한 기능을 제공하기 때문입니다. 도표의 경우 D3는 축, 도형, 툴팁, 텍스트, 색상 및 애니메이션의 모든 세부 정보를 사용자 정의할 수 있습니다.

이 기사에서는 위의 버블 이미지를 예로 들겠습니다. 우리는 작업 환경에 React를 사용하고 D3가 관리하는 오렌지색과 블루색 원을 유치하기 위해 `svg` 태그를 만들 것이다.

# 반응에서 D3 렌더링을 위한 5단계

React는 사용자 인터페이스를 구축하기 위한 선언적이고 효율적이며 유연한 JavaScript 라이브러리이다. D3와 React는 모두 DOM을 조작하는 주장된 라이브러리이다. 작업 환경에 React를 사용하므로 D3는 데이터 시각화 영역에서만 DOM을 조작합니다. 이러한 D3 관리 영역의 경우, 우리는 Retact가 성능과 효율성 측면에서 제공하는 것을 포기합니다.

반응에서 D3를 렌더링하려면 다섯 단계가 필요합니다.

# 1. 제어되지 않는 구성 요소 구축

우리는 이 글에서 통제되고 통제되지 않는 요소들에 대해 토론했다. 제어되는 구성 요소는 하위의 데이터를 관리하는 반면, 제어되지 않는 구성 요소는 하위의 자체 데이터를 관리하도록 합니다.

D3 관리 영역은 통제되지 않는 구성요소로 구성되어야 한다. 예에서 제어되지 않는 `svg` 요소의 경우, `useRef`는 SVG 구성 요소에 액세스하기 위해 생성된다.

```undefined
constant containerRef = useRef(null);
```

이 `containerRef`의 `current`는 리액트 코드의 SVG 요소에 할당됩니다.

```undefined
=svgid="content" 폭="100%" 높이="350" ref={containerRef} }
<g 변환="0, 100" / >
</svg>
```

이 React `ref`를 사용하면 `containerRef.current.clientWidth`를 사용하여 SVG 요소의 너비를 얻을 수 있습니다.

# 2. D3 코드 생성

우리는 원의 반지름을 지정하는 숫자의 배열인 `prop`을 가진 `Circle`이라는 리액트 성분을 생성한다. 홀수 색인 원은 파란색이고 짝수 색인 원은 주황색입니다.

다음은 이러한 원을 배열하는 D3 코드입니다.

원을 그리려면 네 단계가 필요합니다.

## D3 데이터 바인딩(라인 1)

D3 데이터를 바인딩하는 데 사용되는 기능은 다음과 같습니다.

라인 1은 g(그룹) 태그를 선택하고 그룹 내의 모든 circle(원) 태그를 선택하여 데이터 어레이에 연결합니다.

## 새 데이터 입력(3-10줄)

데이터를 입력하는 데 사용되는 기능은 다음과 같습니다.

3-10 행은 아직 바인딩되지 않은 새 데이터를 추가합니다. 새 데이터는 반지름(라인 6), 원 중심 X 값(라인 7), 원 중심 Y 값(라인 8), 원 획 색상(라인 9) 및 원 채우기 색상(라인 10)별로 구성됩니다.

## 기존 데이터 업데이트(12-17)

d3.attr 기능을 사용하여 반지름(13라인), 원 중심 X 값(14라인), 원 중심 Y 값(15라인), 원 획 색상(16라인), 원 채움 색상(17라인)을 기존 요소로 구성한다.

## 나가는 데이터 제거(19호선)

다음 기능은 종료 데이터를 제거하는 데 사용됩니다.

라인 19는 바인딩할 데이터가 없는 DOM 요소를 제거합니다.

2단계와 3단계(동일한 기능을 사용하여 원을 구성)가 중복되지 않습니까?

예. 새 요소와 기존 요소가 서로 다르게 표시되거나 애니메이션화될 수 있도록 코딩되어 있습니다. 그러나 동일한 동작으로 코딩되므로 `d3.merge`를 사용하여 입력 데이터와 기존 데이터를 모두 처리할 수 있다.

다음은 단순화된 D3 코드입니다.

6번 라인에서 병합 기능을 사용하면 D3 코드가 단축됩니다.

# 3. D3 코드 '레이아웃 효과 사용'

use layout effect란?

use Layout Effect의 시그니처는 use Effect와 동일하지만 모든 DOM 돌연변이가 발생한 후 동시에 발생합니다. 이는 `use Effect`가 모든 DOM 돌연변이와 함께 비동기식으로 발생함을 의미한다.

다음과 같은 상황이 발생합니다.

항상 그렇듯이 Create React App을 사용하여 React `use Effect`와 `use Layout Effect`를 탐색합니다. 다음은 수정된 `src/App.js`입니다.

데이터 업데이트 버튼을 클릭하면 콘솔에서는 0으로 설정되고 1과 2로 설정된다. 디스플레이에 최종 값 `2`가 표시됩니다. use Layout Effect가 use Effect보다 먼저 실행된다는 것을 알 수 있다.

![What happens in the console when Update Data button is clicked](https://miro.medium.com/max/914/1*BqCANXm5tb7RT3R3hN44lg.png)

11-14줄에 댓글을 달고 데이터 업데이트 버튼을 빠르고 반복적으로 클릭하면 0과 2가 휙휙 지나가는 것을 볼 수 있다.

6-9줄에 주석을 달고 데이터 업데이트 버튼을 빠르고 반복적으로 클릭하면 값(`1`)이 튕기지 않고 표시되는 것을 알 수 있다.

대부분의 경우, 우리는 `이용 효과`를 사용해야 한다. 그러나 코드가 DOM을 변이시키는 경우 `Layout Effect`를 사용하여 DOM의 변이 작업이 완료될 때까지 디스플레이를 차단해야 합니다. 이렇게 하면 디스플레이가 깜박이는 것을 피할 수 있습니다.

D3 코드가 DOM을 변이시키므로 `Layout Effect 사용`에 넣어야 합니다.

라인 1은 `레이아웃 효과 사용`을 사용하여 D3 코드를 포장합니다.

라인 2는 데이터가 배열임을 보장합니다.

# 4. 이벤트 청취자 설정

우리가 언급했듯이, 우리는 `svg` 요소에 대한 반응의 통제를 D3에 포기한다. 따라서, 우리는 사이즈 조절과 같은 몇 가지 추가 사항들을 처리해야 할 수도 있습니다.

우리는 시야의 폭을 기준으로 원을 고르게 그렸습니다. 데이터 소품이 바뀌지 않는 한 제어되지 않는 SVG 부품은 재도장을 하지 않기 때문에 디스플레이 크기가 조정되면 서클이 재배열되지 않는다. 크기 조정 이벤트를 처리하려면 이벤트 수신기를 사용해야 합니다. 이 경우 크기 조정은 use effect가 좋은 선택이다.

라인 6은 `크기 조정` 이벤트 핸들러를 추가합니다.

라인 7은 효과 정리 중에 `크기 조정` 이벤트 핸들러를 제거합니다.

이 기능은 작동하지만 크기 조정 중에 디스플레이가 깜박입니다. 사실 크기 조정의 모든 단계를 표시할 필요는 없습니다. 디바운스는 크기가 안정될 때까지 변화를 지연시키는 데 도움이 될 수 있다. Create React App의 기본 제공 `디바운스`를 사용하여 다음과 같은 목표를 달성할 수 있습니다.

5번 라인에서는 크기 조정 이벤트 핸들러에 디바운스가 추가된다.

이제 우리는 반응의 D3의 완전한 예를 위한 모든 조각을 가지고 있습니다. 이 파일은 세 개의 파일로 구성됩니다.

```undefined
"종속성": {
"d3": "^6.3.1",
...
}
```

우리의 예는 데이터 업데이트 버튼(17번 라인)과 Circles(18번 라인)의 두 가지 요소로 구성되어 있다.

버튼을 클릭하면 원이 업데이트됩니다. 클릭 핸들러는 라인 6-13에 구현됩니다. 표시할 원의 수와 각 원의 반지름을 계산합니다.

다음은 이 예제의 비디오 클립입니다.

# 5. 반응과 D3 코드 간의 균형

앞의 예에서 데이터 시각화는 `svg`와 `g` 태그 외에도 여러 기능을 사용하여 SVG를 조작하는 D3에 의해 처리된다.

실제로 React를 사용하여 SVG를 직접 관리할 수 있습니다. 다음은 동일한 예를 수행하기 위한 코드입니다.

그럼 왜 D3가 필요한 거죠?

일부 작업의 경우 D3는 매우 간단한 방법으로 특정 효과를 달성할 수 있는 유리한 방법을 제공한다. 예를 들어 `d3.scaleLinear`는 입력과 출력 사이의 선형 관계를 갖는 척도를 구성한다. 데이터를 차트에 배치하는 데 드는 많은 노력을 절약할 수 있습니다.

우리는 DOM을 관리할 두 개의 강력한 라이브러리를 가지고 있다. 그들은 어떤 경우에 비슷한 것을 성취할 수 있다. 반응 방식은 반응 환경에서 기본이지만 D3의 접근 방식은 데이터 시각화를 조작하는 데 더 우수하다. 우리는 대부분 D3의 한 케이스와 React의 다른 케이스만 보여주었습니다. 그것들은 다른 비율과 혼합될 수 있다. 중요한 부분은 코드 관리 용이성과 효율성을 위한 균형점을 찾는 것이다.

# 결론

D3는 데이터 시각화에 탁월하고 React는 일반 사용자 인터페이스에 탁월합니다. 둘 다 DOM을 조작하는 의견이 있는 라이브러리이다. 적절하게 결혼하면 아름다운 사용자 인터페이스가 만들어질 수 있습니다.

다음 기사에서는 React에 D3 차트를 만들 예정입니다.

읽어주셔서 감사합니다. 이게 도움이 됐으면 좋겠어요. 여기 제 다른 매체 출판물을 보실 수 있습니다.