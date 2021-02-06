---
layout: post
title: "호환되지 않는 5개의 CSS 속성 및 값"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/CSS-Incompatible.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/CSS-Incompatible.png?fit=730%2C485&ssl=1)

"제가 디브를 오른쪽에 놓았는데, 왜 아직도 왼쪽에 나타나죠?"

"정말요? 브라우저가 더 이상 flex를 지원하지 않는 이유는 무엇입니까?

CSS는 때때로 재미있고, 다른 때는 좀 재미있고, 대부분의 경우 좌절감을 줍니다. 제 경험상, 대부분의 좌절은 웹 페이지의 결과가 우리가 예상했던 것과 정확히 일치하지 않을 때 발생합니다.

대부분의 경우 문제는 맞춤법이나 선택기가 아니라 호환되는 속성 및 값을 사용하지 않는다는 사실을 알게 되었습니다. 그렇기 때문에, 때때로, 우리가 몇 가지를 바꿀 때, 갑자기, 그리고 겉으로 보기에는 무작위로 보이는 것이 효과가 있는 것입니다.

향후 이러한 문제를 방지하기 위해 이 글에서 CSS에서 함께 작동하지 않는 5개의 CSS 속성 및 값을 다루겠습니다(우리가 얼마나 원하는지에 관계없이).

## CSS 검토: 위치 및 비위치 값

이 글의 내용을 완전히 이해하기 위해 우선 위치 및 위치 미지정 값을 검토하겠습니다.

우선 절대값, 고정값, 상대값, 끈적임값을 들 수 있다. `절대` 위치 요소는 문서의 흐름에서 그 위치를 잃지만 상대 위치(그렇지 않으면 뷰포트)인 경우 상위 요소에 의해 제한된다. 값이 0인 top 속성은 요소의 경계 맨 위에 배치합니다.

절대와 달리 고정 위치 요소는 뷰포트로 제한되므로 값이 `0`인 상위 속성이 뷰포트의 맨 위에 요소를 배치합니다. 상대적인 위치 요소는 자기 자신에 상대적이며 상단, 왼쪽, 하단, 오른쪽을 받아들인다. 다음 절에서는 이러한 특성이 상대적 위치 요소에 어떤 영향을 미치는지 보여줍니다.

마지막으로, `sticky` 위치 요소는 고정 요소와 상대 요소의 조합이다. `top` 위치 속성을 사용하는 동안 상위 항목이 더 이상 표시되지 않을 때까지 상위 뷰포트 내에서 이러한 요소를 제한할 수 있습니다. 여기서 더 많이 읽을 수 있어요.

다음 두 절에서 주목해야 할 점은 `정적` 위치가 `상대적` 위치 요소와 유사하지만 위치가 `정적`인 요소는 위치가 정해진 것으로 간주되지 않는다는 점이다. 대신 정적(static)은 문서의 모든 선언된 요소의 기본 위치입니다.

## 1. position: static과 z-index: n

정적 요소는 문서의 정상적인 흐름과 관련하여 배치되고 해당 흐름에서 제거할 수 없으므로 절대 하위 항목이 상위 요소의 위치를 존중하려면 먼저 부모에 상대적인 값이 있어야 합니다. 혼합에 z-인덱스를 추가하면 문제가 된다.

z-index는 z축에서 요소(다른 요소 위 또는 아래)를 배치하는 데 사용되는 속성으로, 이 속성도 위치 요소만 존중합니다. 정적(static)은 기본 위치이므로 그 자체로 간주되지 않는다는 점을 기억하십시오. 아래에서는 이 문제가 어디서 발생하는지 확인할 수 있습니다.

### 예:

`static`의 경우 요소는 정규 흐름을 따라야 합니다.

```js
<div class='block1'></div>
<div class='block2'></div>
```

이제 `z-index`를 추가해 보십시오.

```css
.block1,
.block2 {
  width: 300px;
  height: 300px;
}

.block1 {
  background-color: yellow;
  z-index: 1;
}

.block2 {
  position: absolute;
  background-color: red;
  margin-top: -60px;
  margin-left: 10px;
}
```

출력은 다음과 같습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Z-index-static-position-output.png?resize=730%2C565&ssl=1)

정상적인 흐름은 `.block1`에서 `.block2`로 표시됩니다. 그리고 코드 블록에 z-index 속성이 포함되더라도 z-index는 위치 요소만 존중하기 때문에 .block1의 스타일 선언에서는 무시된다.

수정하려면 .block1에 position ` relative`를 추가하여 다음을 확인합니다.

```css
.block1,
.block2 {
  width: 300px;
  height: 300px;
}

.block1 {
  position: relative;
  background-color: yellow;
  z-index: 1;
}

.block2 {
  background-color: red;
  margin-top: -60px;
  margin-left: 10px;
}
```

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Z-Index-Relative-Position-Output.png?resize=730%2C602&ssl=1)

position 속성을 명시적으로 선언하지 않으면 static이 기본값이 되고 z-index가 작동하지 않는다는 점을 기억하십시오.

## 2. '위치: 정적'과 '상단|왼쪽|오른쪽

짐작하셨겠지만, 이것은 위의 정적에 대한 설명과도 관련이 있다. 우리는 상단, 하단, 왼쪽, 오른쪽이 요소의 위치를 바꾸는 데 사용된다는 것을 알고 있기 때문에 이러한 속성을 사용하면 정적인 위치에 있는 요소들에 대해 문제를 일으킬 수 있다.

그러나 마진톱 등은 어떨까. 우리는 다음의 예에서 이들 `마진` 관계자들과 그들의 위치를 바꾸는 관계자들 사이의 차이점을 살펴볼 것이다.

### 예:

먼저 블록을 만듭니다.

```undefined
<div class="block block1">content</div>
<div class="block block2">content</div>
```

이제 `마진톱`을 추가해 보십시오.

```css
.block {
  width: 400px;
  height: 400px;
}
.block1 {
  margin-top: 100px;
  background-color: red;
}
.block2 {
  background-color: yellow;
}
```

출력은 다음과 같습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Z-Index-Margin-Top-Output.png?resize=730%2C498&ssl=1)

마진톱을 사용하면 빨간색 상자가 밀렸지만 위치가 바뀌지 않는다는 것을 알 수 있습니다. 빨간색 상자는 원래 위치(dev 도구에서 요소를 선택할 때 주황색 오버레이)에서 밀리게 되며, 따라서 빨간색 상자를 따르는 다른 모든 요소에 영향을 미칩니다. 이 경우 노란색 상자도 밀렸다는 것을 알 수 있습니다.

짐작하셨겠지만 이제 `톱`과 `좋아요`는 정적인 요소로는 작동하지 않을 것입니다. 그러나 이 예에서 우리는 정해진 위치의 top(상대형)을 사용하여 이 문제를 해결할 수 있다.

```css
.block {
  width: 400px;
  height: 400px;
}
.block1 {
  position: relative;
  top: 200px;
  background-color: red;
}
.block2 {
  background-color: yellow;
}
```

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Z-Index-Top-Relative-Output.png?resize=730%2C481&ssl=1)

보시는 바와 같이 원소의 위치가 바뀌었으며 주변 원소(노란색 상자)에도 영향을 주지 않습니다.

위의 예에서, 우리는 `마진` 위치가 요소들을 서로 다른 쪽으로만 밀어넣어 프로세스 내 주변 요소들에 영향을 미치는 반면, `톱`은 다른 요소들에 영향을 주지 않고 요소들을 재배치한다는 것을 알 수 있다. 또한 모든 요소를 밀어넣을 수 있지만 모든 요소(`정적` 참조)의 위치를 변경할 수 없다는 점도 알아챘을 수 있습니다.

이러한 속성을 사용할 때 발생할 수 있는 불상사를 방지하기 위해 여백은 모든 위치에 대해 동일하게 동작하지만 상단|왼쪽|오른쪽)은 서로 다른 위치(상대적, 절대적, 고정적, 끈적임)에 대해 다르게 동작한다는 점도 유용하다.

## 3. '위치: x'와 'transition'

애니메이션과 전환은 한 스타일 속성에서 다른 스타일 속성으로 원활하게 변환할 수 있는 CSS 속성이다.

예를 들어 불투명도 0 요소를 불투명도 1로 변환하는 경우를 가정해 보겠습니다. 원소는 0에서 시작하여 완전히 보인 다음 더 이상 볼 수 없을 때까지 점진적으로 변환하기 시작합니다. 더 간단한 예는 한 요소의 높이를 다른 높이로 변환하는 것입니다.

계속 따라오셨다면, 제가 다음에 질문할 것은 애니메이션과 전환이 어떻게 `위치`와 함께 작동하는가 하는 것입니다. 간단한 답이 있습니다. 다음 예에서 살펴보겠습니다.

### 예:

```js
<div class="container">
  <div class="elem"></div>
</div>
```

이 예에서는 `상대적` 위치를 사용합니다.

```css
.container {
  margin-top: 100px;
}

.elem {
  position: relative;
  width: 100px;
  height: 100px;
  background-color: black;
}
```

결과:

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Animation-Transition-Relative-Position-Output.png?resize=730%2C439&ssl=1)

그런 다음 우리는 다음과 같은 `포지션` 속성을 포함하는 (`허버`와 같은 상태에서의) 변환을 도입할 수 있다.

```css
.elem:hover {
  width: 100px;
  height: 100px;
  background-color: black;
  position: fixed;
  right: 0;
}
```

결과:

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Transformation-Output-Hover-Possition.png?resize=730%2C354&ssl=1)

얼핏 보기에는 값이 변경되었기 때문에 효과가 있었던 것으로 보입니다. 그러나 브라우저가 무엇을 애니메이션해야 할지 모르기 때문에 원활한 전환이 이루어지지 않습니다. 그 이유는 간단합니다: 애니메이션은 일반적으로 시간이 지남에 따라 변하는 가치를 수반합니다. 예를 들어, 0부터 1까지의 불투명도에 대한 애니메이션에는 불투명도 0, 0.1, 0.2 등이 포함됩니다. 그러나 position: static에서 position: fixed로 변경하는 경우에는 이러한 중간값이 통과되지 않는다.

따라서, 만약 여러분이 `직위` 속성에 대한 전환을 하려고 한다면, 여러분은 결코 성공할 수 없을 것입니다. 그리고 그것은 간단한 대답입니다. 이제 아시겠죠!

## 4. '디스플레이: 인라인'과 '높이|폭'

디스플레이 구성요소의 두 가지 주요 유형은 인라인과 블록이다. 블록 요소는 항상 새 라인에서 시작되므로 다음 즉시 요소도 새 라인에 나타나야 합니다. 블록 구성요소는 과도한 경우에도 접근할 수 있는 만큼의 수평 공간을 포함합니다. 블록 요소의 예로는 p, h1, div, form, pre 등이 있다.

반대로 인라인 요소는 필요한 공간만 차지하므로 문서의 흐름이 끊어지지 않습니다. 이름에서 알 수 있듯이 인라인 요소는 발견된 모든 콘텐츠의 인라인으로 표시됩니다. 인라인 요소의 예로는 스판, a, 버튼 등이 있다.

다음 예에서는 이러한 표시 요소가 작동하는 것을 볼 수 있습니다.

### 예:

```xml
<div>
  I love <a href='https://google.com'>Google</a>. The title of my story is
  <h1>Inline elements</h1>. I am <span class="highlight">highlighted</span> and
  <p>I love CSS</p>
</div>


.highlight {
  background-color: pink;
  padding: 30px;
}
```

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Inline-height-width-display-example.png?resize=730%2C533&ssl=1)

앵커 태그는 흐름을 방해하지 않지만 제목("Inline 요소")은 방해합니다. 표제 h1이 블록 요소이기 때문이다.

반면 span 요소("강조 표시")는 약간 다르게 처리된다. 네 귀퉁이에 ➡을 적용했지만 요소 주변 요소는 좌우 패딩만 존중한다. 다른 요소는 이동되지 않았습니다. 스팬이 인라인 요소이기 때문이다.

인라인 요소는 필요한 만큼만 공간을 차지하기 때문에 높이나 너비 스타일은 받아들이지 않는다. 아래에서는 높이나 폭이 제대로 작동하지 않을 경우 디스플레이를 항상 확인해야 하는 이유를 설명하겠습니다.

### 예:

인라인 요소에 높이와 너비 값을 더하면…

```css
.highlight {
  background-color: pink;
  padding: 30px;
  width: 40px;
  height: 100px;
}
```

… 결과는 이전의 예와 동일합니다!

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Inline-height-width-display-example.png?resize=730%2C533&ssl=1)

블록 요소에 대해 `인라인` 디스플레이를 명시적으로 지정할 수도 있고 그 반대도 마찬가지입니다.

```css
.highlight {
  background-color: pink;
  padding: 30px;
  width: 40px;
  height: 100px;
  display: block;
}
h1 {
  display: inline;
}
```

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Specified-Inline-Block-Example.png?resize=730%2C401&ssl=1)

고전적인 트위스트에서 h1은 이제 필요한 만큼만 공간을 사용하고 span 요소는 이제 높이나 폭을 수용할 수 있다.

요약하면 높이 또는 폭이 작동하지 않는 경우 대상 요소의 `표시`를 확인해야 합니다. 그것은 아마 인라인과 관련이 있을 것이다.

인라인 블록이라는 표시값이 있다는 점도 눈여겨볼 필요가 있다. 인라인 요소처럼 동작하지만 차단 요소가 허용하는 속성을 수락합니다.

## 5. '디스플레이: 플렉스'와 '셀프'

예를 들어, 컨테이너의 왼쪽 가장자리에 처음 두 개의 요소와 오른쪽 가장자리에 마지막 요소의 세 가지 요소를 표시하려고 했습니다.

저와 같은 일부 사람들은 즉시 다음과 같은 생각을 할 것입니다.

```undefined
<div class="container">
  <div class="block block1"></div>
  <div class="block block2"></div>
  <div class="block block3"></div>
</div>


.container {
  display: flex;
}
.block {
  width: 100px;
  height: 100px;
  margin: 20px;
  background-color: red;
}
.block3 {
  justify-self: end;
}
```

결국 정당화-자신은 마지막 블록을 (수평인) 본축의 끝으로 `정당화`하는 것이 되는 것이다. 알고 보니 별로였어요.

몇 번의 시행착오를 거쳐 나는 정당화-셀프(prightify-self)가 블록 수준, 절대 위치 요소, 그리드-레이아웃(grid-layout)과 함께 작동하지만 플렉스 컨테이너에서는 작동하지 않는다는 것을 알게 되었다.

그 이유는 주축에서 플렉스 컨테이너가 콘텐츠를 그룹으로 취급하기 때문입니다. 이 그룹의 경우 콘텐츠 정당화 속성에 따라 각 자식마다 충분한 공간과 동일한 공간을 할당하려고 합니다. 따라서 각 개별 요소에 사용할 수 있는 공간이 제한됩니다.

반면 얼라인셀프(align-self)를 사용하면 개별 품목을 배치할 수 있는 크로스 축의 공간이 더 유연해진다.

다음 예에서, 저는 애초에 제가 하려고 했던 것을 성취하기 위해 제가 하게 된 것을 보여드리겠습니다.

### 예:

```css
.block3 {
margin-left: auto;
}
```

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Align-Self-Output-.png?resize=730%2C395&ssl=1)

이 방식을 통해.block3는 주축의 모든 가용 공간에 접근할 수 있기 때문에 auto의 margin-left가 이를 오른쪽으로 밀어낸다. margin-right: auto도 추가되면 요소가 사용 가능한 공간의 중앙에 배치됩니다.

## 결론

CSS 고투의 일부(또는 대부분의 경우 누구에게 요청하느냐에 따라)는 CSS 속성 및 값의 호환되지 않는 조합의 결과입니다. 이 기사에서는 다음과 같은 다섯 가지 조합을 살펴봤습니다.

- position: static 및 z-index: n
- position: static과 top|왼쪽|오른쪽
- position: x와 transition
- `디스플레이: 인라인` 및 `높이|폭`
- 디스플레이: 플렉스(flex)와 셀프(self

CSS를 너무 좌절하게 만드는 속성이나 가치의 조합은 아직 더 많을 것이라고 확신하지만, 이 다섯 가지 팁으로 CSS를 쓰는 것이 전보다 조금 더 쉬웠으면 좋겠다.

행운을 빕니다.