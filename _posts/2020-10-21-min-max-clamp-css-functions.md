---
layout: post
title: "최소 'max() 및 clamp() CSS 함수에 대한 가이드"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/min-max-clamp-css.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/min-max-clamp-css.png?fit=730%2C487&ssl=1)

최근에 사이트를 다시 설계할 기회가 생겨서 이제 유용하게 사용할 수 있을 만큼 널리 구현되기 시작한 최소 최대 및 클램프(clamp) CSS 기능을 이용할 수 있을 것으로 생각했습니다.

내 추정에 따르면, 이러한 함수는 웹 레이아웃을 혁신할 수 있는 잠재력을 가지고 있지만, CSS를 그들의 잠재력을 최대한 활용한다면 훨씬 더 추론하기 어렵게 만들 수 있다.

## 이 기능들은 무엇입니까?

MDN에 대한 정의를 참조하겠습니다.

- `min()`: 쉼표로 구분된 식 목록에서 가장 작은(가장 음수) 값을 CSS 속성 값의 값으로 설정할 수 있습니다.
- `max()`: 쉼표로 구분된 식 목록에서 가장 큰(가장 양수) 값을 CSS 속성 값의 값으로 설정할 수 있습니다.
- `clamp()`: 상한과 하한 사이의 값을 클램프합니다. `값`은 정의된 최소값과 최대값 사이의 값 범위 내에서 중간값을 선택할 수 있습니다. 최소값, 기본값 및 최대 허용값의 세 가지 매개 변수가 필요합니다.

또 다른 흥미로운 점은 calc를 사용하지 않고도 함수 안에서 수학을 할 수 있다는 것이다. 따라서 `min`(calc(5vw + 5px), 50px) 대신 `min`(5vw + 5px, 50px)만 수행할 수 있습니다. 물론 변수 내에서 계산을 수행하고 해당 변수를 사용할 수도 있습니다.

한 가지 헷갈릴 수 있는 것은 이름이다. 최소값은 값 리스트에서 최소값을 반환하고 최대값은 예상대로 반환합니다. 그러나 다음과 같은 작업을 수행한다면 어떻게 될까요?(예의 목적을 위한 정말 말도 안 되는 사용법):

```undefined
width: min(1px,200px,300px);
```

물론 1px는 반환되며, 이는 최소 함수의 출력이 해당 너비의 최대값을 1px로 설정한다는 것을 의미한다.

CSS 값 및 유닛 모듈 레벨 4에 대한 W3C 사양은 다음과 같습니다.

> min()/max()를 사용할 때 가끔 혼동되는 점은 max()를 사용하여 최소값을 부과하고, min()과 같은 속성을 사용하여 최대값을 부과하는 경우 max()를 사용하여 최대값을 부과하는 경우이며, 반대 함수에 도달하기 쉽고 min()을 사용하여 최소값을 추가하려고 시도하는 경우입니다. clamp()를 사용하면 값이 최소값과 최대값 사이에 중첩되므로 코드를 보다 자연스럽게 읽을 수 있다.

다음은 3div의 예로서, 2div는 `min() 값 출력을 표시하고 1div는 `max()` 값 출력을 표시하여 너비를 설정하는 예입니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="450" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/xxVMbpp?height=450&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=xxVMbpp&amp;pen-title=example%20min%2C%20max&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="example min, max" loading="lazy" id="cp_embed_xxVMbpp"></iframe></div>

각 div는 그 폭을 설정하는 데 사용되는 실제 min() 또는 max() 함수를 텍스트 콘텐츠로 가지고 있으므로 쉽게 효과를 볼 수 있다.

이러한 각 기능은 길이, 주파수, 각도, 시간, 백분율, 숫자 또는 정수를 사용할 수 있는 모든 곳에서 사용할 수 있습니다. 이는 이러한 유형의 값에 사용할 수 있는 모든 단위를 입력으로 사용할 수 있음을 의미합니다. 예를 들어 다음과 같은 기능을 사용할 수 있습니다.

```css
transform: rotate(min(45deg, .15turn));
```

## 이러한 기능을 예기치 않은 방법으로 사용

물론 이러한 기능을 `폭`과 같은 속성이나 `칼크()`와 같은 기능(또는 사용법이 당장 분명하지 않다면 적어도 그 기능에서 사용할 수 있다는 것은 즉시 명백하다. 그러나 그것들이 사용될 수 있는 많은 다른 방법들은 쉽게 명백하지 않을 수 있다.

이 절에서 배워야 할 주요 교훈은 이러한 함수는 단위에서 작동하므로 `calc()`와 같이 a를 반환하는 함수를 입력으로 사용하거나 단위를 기대하는 함수 내부에서 사용할 수 있다는 것이다. 첫 번째 예로서, 아주 분명하지 않은 HSL 함수에 그것들을 사용합시다.

### HSL 기능과 함께 사용

HSL은 색조, 채도, 가벼움을 의미하며, CSS 3에 추가된 기능이다. HSL에 대한 리프레셔는 이 게시물을 참조할 수 있다.

다음 예제는 처음에는 특별히 유용하지 않지만 HSL 함수 내에서 `min`과 `max`를 사용할 수 있는 방법을 살펴 보십시오.

```undefined
hsl(min( 180, 190, 150), max(75%, 50%; 100%), 50%)
```

이는 단순히 머릿속에서 계산을 하고 글을 쓰는 것만으로 얻을 수 없는 것을 주지 않습니다.

```undefined
hsl(150, 100%, 50%)
```

이 경우 모든 값이 어떻게 되는지 알 수 있습니다. CSS 변수는 이런 경우에 이러한 함수들을 훨씬 더 유용하게 만든다.

### CSS 변수와 함께 산술 및 HSL 사용

CSS 변수는 미디어 쿼리에 의해 덮어쓰거나, 사용자가 사이트에 있는 컨텍스트에서 특정 CSS를 로드하거나, JavaScript에 의해 변수 값을 변경할 수 있습니다.

따라서 다음과 같은 작업을 수행할 수 있습니다.

```undefined
hsl(
  min(
    var(--extreme-hue),
    var(--base-hue)
  ), 
  max(
    var(--base-saturation),
    var(--region-saturation)
  ),
  50%);
```

보다 의미 있고 강력한 계산을 할 수 있는 길을 열어줍니다. (사이트나 응용 프로그램의 맥락에서 변수 이름은 사용을 더 잘 반영할 수 있습니다. 여기서는 단지 방법론을 나타내기 위한 것입니다.)

다음은 예입니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="450" width="100%" name="cp_embed_2" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/YzqvXKe?height=450&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=YzqvXKe&amp;pen-title=Min%2CMax%20with%20HSL&amp;name=cp_embed_2" style="width: 100%; overflow:hidden; display:block;" title="Min,Max with HSL" loading="lazy" id="cp_embed_YzqvXKe"></iframe></div>

다음은 미디어 쿼리 내에서 CSS 변수 값을 재정의하는 예입니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="450" width="100%" name="cp_embed_3" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/eYZKvqp?height=450&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=eYZKvqp&amp;pen-title=Min%2CMax%20with%20HSL%20media%20query&amp;name=cp_embed_3" style="width: 100%; overflow:hidden; display:block;" title="Min,Max with HSL media query" loading="lazy" id="cp_embed_eYZKvqp"></iframe></div>

말했듯이, 당신은 또한 자바스크립트로 CSS 변수를 재정의할 수 있습니다. 다음은 변수를 2초 간격으로 랜덤하게 변경하는 어리석은 예입니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="450" width="100%" name="cp_embed_4" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/abNKWOp?height=450&amp;theme-id=dark&amp;default-tab=js%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=abNKWOp&amp;pen-title=Min%2CMax%20with%20HSL%20JavaScript%20updating&amp;name=cp_embed_4" style="width: 100%; overflow:hidden; display:block;" title="Min,Max with HSL JavaScript updating" loading="lazy" id="cp_embed_abNKWOp"></iframe></div>

### min()sph'max()max()max()clamp()를 사용할 수 있는 함수

내가 증명했듯이, CSS 변수가 존재하기 때문에 `min() ipt` 또는 `clamp()`에서 이득을 얻을 것 같지 않다고 생각할 수 있는 함수들도 여전히 그것들을 사용할 수 있다.

따라서 일반적으로 예상하지 못할 수도 있지만 연산 함수를 사용할 수 있는 함수 목록이 여기에 있습니다(예: `calc()ait`와 같은 함수를 사용할 것으로 예상할 수 있음). 이 목록은 이해하기 쉬운 기능의 유형으로 구분됩니다.

#### 색 함수

이러한 기능은 필터를 출력하거나 이미지의 색 구성표에서 작동합니다.

- 낙하산
- ➡➡➡와 ➡➡➡➡
- ➡-➡
다음은 `방사선 그라데이션`의 예입니다. 일부 그라데이션 기능은 브라우저 전체에서 지원되지 않지만 다른 그라데이션 기능은 동일한 방식으로 작동합니다.
- 다음은 `방사선 그라데이션`의 예입니다. 일부 그라데이션 기능은 브라우저 전체에서 지원되지 않지만 다른 그라데이션 기능은 동일한 방식으로 작동합니다.
- `➡➡➡➡➡➡➡➡
- 밝기
- ➡스케일➡
- ➡-➡
- `➡➡➡➡➡➡➡➡
- ➡➡➡➡➡.
- ➡➡➡➡
- `➡➡➡➡➡➡➡➡

#### 형상함수

이러한 기능은 형상(이미지)을 만들거나 변환을 출력합니다.

- `➡➡➡➡➡➡➡➡
다음은 `원`과 이전 방사형 그라데이션 함수를 사용한 예입니다. 폴리곤, 엘립스 등과 같은 다른 형상 함수들도 같은 방식으로 수학 함수를 사용할 수 있다. 효과를 보려면 렌더링 영역 크기 조정
- 다음은 `원`과 이전 방사형 그라데이션 함수를 사용한 예입니다. 폴리곤, 엘립스 등과 같은 다른 형상 함수들도 같은 방식으로 수학 함수를 사용할 수 있다. 효과를 보려면 렌더링 영역 크기 조정
- 애니메이션 기능 및 기타 애니메이션 기능
- `➡➡➡➡➡➡➡➡
- `매트릭스
- `매트릭스 3d²`
- 원근법
- rotate() (또한 rotate3d() rotateX() 등)
- scale() (scale 3d()scale X()scale 등)
- skew()(skewX(skewY)(skewY)(skewX))
- 스텝스
- 번역() (3d()번역X()번역 등)

물론 다른 기능들도 있습니다. 하지만 이 기능들 중 일부는 제가 생각할 수 있는 어떤 방식으로도 사용할 수 없습니다. 여러분이 얼마나 창의적이 되려고 노력하든 간에 말입니다. 또는 이 글을 쓸 때, 그것들은 가장 인기 있는 플랫폼들에서는 지원되지 않기 때문에, 사용할 가치가 없을 것 같습니다.

## 이러한 기능이 작동하지 않는 예상치 못한 방법 - 아직

앞에서 언급한 바와 같이, 이러한 함수는 변수의 길이와 같은 숫자 또는 계산 가능한 값을 입력으로 받아들일 수 있습니다. 또한 변수와 같은 입력으로 사용할 수 있습니다. 여기서 계산 결과, 숫자 또는 다른 값을 저장했을 수 있습니다.

많은 다른 CSS 속성들은 결국 길이, 백분율, 숫자, 즉 이러한 함수들 중 하나가 잠재적으로 사용할 수 있는 값을 반환하는 특별한 키워드를 가지고 있다.

예를 들어, `width`에는 다음과 같은 특수 키워드가 있습니다.

- `최대 콘텐츠`: 고유 선호 높이 또는 너비
- `min-content`: 고유 최소 높이 또는 너비(예: 단일 문자가 있는 블록은 해당 문자의 고유 최소 높이 또는 너비가 있음)

W3C 작업 그룹에서 고유 크기 조정의 의미에 대해 인용하자면 다음과 같습니다.

> 각 축에 있는 상자의 최소 함량 크기는 해당 축에 auto 크기가 주어졌을 때(해당 축에는 최소 또는 최대 크기가 없음) 플로트일 경우 및 해당 축에 포함된 블록의 크기가 0인 경우 가질 수 있는 크기입니다. (즉, 크기가 "적합하게 축소"될 때 최소 크기입니다.)
각 축에 있는 상자의 최대 내용물 크기는 해당 축에 자동 크기가 주어졌을 때(해당 축에는 최소 또는 최대 크기가 없을 때) 플로트일 때 가질 수 있는 크기이며, 해당 축에 포함된 블록의 크기가 무한대인 경우이다. (즉, 크기가 "적합하게 축소"되었을 때의 최대 크기입니다.)

이러한 키워드는 특히 fit-content와 같은 함수가 정의에서 함수의 구현 방법을 설명하는 데 사용되는 것을 볼 때 `min`/`max` 함수에서 사용할 수 있다고 가정할 수 있다. 그러나 현재, 이것이 어떻게 달성되어야 하는지에 대한 사양은 없다.

CSS 워킹그룹에 사용 가능 여부에 대한 해명을 요청했지만, "지금은 안 된다"는 답변이 온 것 같습니다. 하지만, 그들은 그것들이 어느 시점에 사용 가능하게 되기를 원할 것이다.

## clamp()에 대해 약간

이 다음 섹션에서는 "min"()과 "max()"의 일부 사용에도 적용되지만 "clamp()" 기능을 사용하기 시작할 것이다. 따라서 계속하기 전에 "clamp()"와 세 가지 기능을 함께 사용할 때 발생할 수 있는 복잡성에 대해 확장해야 할 것이다.

먼저 clamp()는 최소값, 기본값 또는 기본값, 최대값의 세 가지 속성을 정확히 취한다는 점을 기억한다. 따라서 `clamp(최소값, 기본값, 최대값)`는 다음과 같습니다.

clamp()에 대한 추론의 용이성이 큰 장점이다. clamp()를 사용하면 개발자로서의 QA와 디자이너의 광기를 일부 줄일 수 있습니다. 제 말은, 물론, 픽셀 단위 완벽한 디자인의 광기입니다.

### clamp()가 픽셀 단위 디자인에 어떤 도움을 주는지

구글이나 덕덕고에서 `픽셀 퍼펙트 디자인의 문제`를 검색하면 이에 대한 반론도 충분히 찾을 수 있을 것이다. 하지만 개발자로서 여러분은 픽셀 단위 디자인은 실제로 존재하지 않는다는 것을 이미 깨달았을 것입니다. 고려 사항:

- 설계에는 응용프로그램이 가능한 모든 상태가 표시되지 않을 수 있습니다. 예를 들어, 설계를 구현하기에는 데이터가 너무 적으면 어떻게 됩니까?
- 특정한 모니터 치수와 해상도를 염두에 두고 설계된 것으로서, 세계에서 발견되는 다른 모든 모니터에는 맞지 않는다.

그러나 개발자로서 여러분은 디자인에 대한 충실도를 요구하며 작업을 제어하는 사람들을 경험해 본 적이 있을 것입니다. 일반적으로 이는 모니터 사양에 따라 설계가 만들어졌기 때문일 것입니다. 따라서 가장 넓은 범위의 모니터에서 멋지게 보이려면 균형 잡힌 설계를 백분율 집합으로 처리했을 가능성이 높기 때문에 설계가 올바르게 보이지 않는다는 것을 알 수 있습니다.

그러나 `clamp()`clamp()와 최소값, 기본값 및 최대값의 조합을 사용하면 설계 기대치와 일치하는 기본값 또는 최대값을 지정할 수 있습니다.

따라서, 컴퓨터에서 제대로 작동하는 설계를 필요로 하는 이해 관계자와 마주친 경우(버그가 없는 이유로 컴퓨터에서 작동한다는 사용자의 주장을 쉽게 받아들이지 않더라도), 그렇지 않으면 이 우스꽝스러운 목표를 가능하게 할 수 있는 도구가 있다는 사실을 기억하십시오.

화소완벽한 디자인에 반대하는 주장들을 그들에게 보내주시고, 그들을 수용하기 전에 그들이 원하는 것이 합리적이지 않다는 것을 이해하도록 노력하세요.

### clamp() 구현 방법

MDN은 clamp(MIN, VAL, MAX)는 max(MIN, min(VAL, MAX))로 해결된다고 주장한다.

함수가 텍스트로 작성된 단순 `min`/`max` div의 이전 예를 기억하면서, 여기 `clamp()`clamp()의 두 가지 예가 있는데, 그 중 하나는 `clamp()` 함수의 구현이 위에서 설명한 것처럼 동작함을 보여준다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="450" width="100%" name="cp_embed_5" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/BaKMyvy?height=450&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=BaKMyvy&amp;pen-title=example%20clamps&amp;name=cp_embed_5" style="width: 100%; overflow:hidden; display:block;" title="example clamps" loading="lazy" id="cp_embed_BaKMyvy"></iframe></div>

물론 이것은 클램핑 동작의 작은 변형을 만들 수 있을 만큼 충분히 쉽다는 것을 보여준다. 예를 들어 max(MIN, min, min(변수 1, MAX), min(변수 2, MAX) 등의 용도를 쉽게 가질 수 있으며 앞에서 언급한 바와 같이 min, max, clamp, calcal과 같은 방법으로 min, max, calcal을 사용할 수 있다.에스의

마지막으로, 물론 이미 보신 것처럼 측정 단위를 혼합하고 일치시킬 수 있습니다. 하지만 최소(200px; 50%; 50vw)를 가지고 있다고 가정해 봅시다. 이제 세 값의 최소값을 수행하는데, 그 중 하나는 절대값(`200px`), 하나는 부모값(`50%), 다른 하나는 창 크기에 따라 결정되는 값(`50vw`)입니다.

따라서 clamp(최소값, 기본값, 최대값)는 추론하기가 더 쉬울 수 있지만, 이러한 세 가지 값의 결정이 실제로 얼마나 복잡할 수 있는지, 코드를 추론하기 위해 개발자로서의 머릿속에 얼마나 많은 요소가 있어야 하는지를 고려할 때 이 이점은 즉시 실현될 수 있습니다.

이 모든 것은 CSS가 지금까지보다 더 복잡할 가능성을 내포하고 있으며 동시에 전력의 비교 증가를 암시하고 있다. 저는 어떤 제안도 부분적으로 누가 제안하든 선호하는 문제 처리 방법론에 기초할 것이라고 생각하기 때문에 이 문제를 해결할 방법을 제안하지는 않을 것입니다.

따라서, 나는 이러한 복잡한 상황에 대처하기 위한 최선의 방법이 예상되는 출력에 대한 추론을 더 단순하게 유지하기 위해 (아마도 보풀 규칙에 의해 시행되는) "클램프()" 함수가 내포된 함수나 계산을 포함할 수 없는 일종의 코딩 규율을 확립하는 것이라고 몇몇 사람들이 쉽게 상상할 수 있다.

다른 한편으로, 이러한 종류의 복잡성에 대처하는 내가 선호하는 방법은 CSS 코드가 무엇을 하고 있는지 분석하는 데 도움이 되는 도구를 쓰는 것이다. 그러나 다른 사람들은 계산된 스타일 결과에 대한 광범위한 테스트를 작성하기를 원할 수 있다.

## 이러한 기능을 사용할 수 있는 대상

이러한 기능들은 단순히 많은 CSS 속성들이 입력의 형태로 사용되기 때문에 처음에는 사용하지 않을 수 있는 많은 곳에서 사용될 수 있다는 것을 증명했다고 생각합니다. 하지만 이제는 그들이 주요 사용 사례에서 어떻게 작동하는지 살펴봐야 할 때라고 생각합니다.

### 글꼴 크기

글꼴 크기부터 시작하겠습니다. 왜냐하면 글꼴 크기는 종종 사이트에서 시작하는 크기입니다. (적어도 그렇습니다. 설계에서 더 복잡한 블록으로 넘어가기 전에 제목과 텍스트의 메인 비트의 크기와 레이아웃을 제공하는 것이 좋습니다.)

클램프()와 몇 가지 계산만 하면 멋진 글꼴 크기 체험이 간단해진다.

예를 들어, 여기에는 제목과 부제 등 몇 가지 클래스와 크기에 사용하는 몇 가지 변수가 있습니다. 제목에 `clamp()`를 사용하면 제목에 가장 작은 글꼴 크기가 애플리케이션의 기본 글꼴 크기가 되도록 지정할 수 있습니다. 우리가 선호하는 크기는 4.5vw이지만, 애플리케이션의 가장 큰 글꼴 크기보다 커서는 안 됩니다.

우리는 부제목에도 같은 것을 명시하고 있으며, 크기를 줄이기 위해 단지 약간의 "칼크"를 사용한다.

제목과 부제목의 글자 간격에 클램프를 달았다. 같은 원리입니다: 작으면 글자 간격이 1.1rem이고, 크면 0.5rem이 됩니다. 우리가 중간에 1.5vw를 맞출 수 있다면, 그것은 사용될 것입니다. 서브타이틀의 글자 간격은 제목보다 모든 경우에서 약간 작다.

```css
.title {
  text-transform: uppercase;
  font-weight: 800;
  font-size: clamp(var(--default-font-s), 4.5vw, var(--biggest-font-s));
  letter-spacing: clamp(.1rem, 1.5vw, .5rem)
}

.subTitle {
  font-size: clamp( var(--default-font-s) - 2px, 4.5vw - 10px, var(--biggest-font-s) - 5px);
  letter-spacing: clamp(.1rem, 1vw, .4rem)
}
```

아래 예제를 보면 창을 더 크고 작게 만들수록 크기가 잘 조정되는 것을 볼 수 있습니다. (물론 이러한 항목에는 창을 멋지게 보이게 하기 위한 몇 가지 추가 속성이 있습니다.)

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="450" width="100%" name="cp_embed_6" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/JjXBGLY?height=450&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=JjXBGLY&amp;pen-title=min%2C%20max%2C%20clamp%20_%20titles&amp;name=cp_embed_6" style="width: 100%; overflow:hidden; display:block;" title="min, max, clamp _ titles" loading="lazy" id="cp_embed_JjXBGLY"></iframe></div>

이제 다음 예를 살펴보십시오.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="450" width="100%" name="cp_embed_7" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/rNerLqK?height=450&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=rNerLqK&amp;pen-title=min%2C%20max%2C%20clamp%20_%20with%20cards_simple%20content&amp;name=cp_embed_7" style="width: 100%; overflow:hidden; display:block;" title="min, max, clamp _ with cards_simple content" loading="lazy" id="cp_embed_rNerLqK"></iframe></div>

이렇게 하면 텍스트와 글꼴 크기, 문자 간격 설정이 포함된 블록이 두 개 더 추가됩니다. 일부 스타일은 수업 사이에 재사용하기 위해 옮겨갔지만 제목과 부제목의 스타일에는 실질적인 변화가 없고 단지 몇 개의 다른 섹션이 추가되었다. 우리는 기사의 다음 부분에서 이 부분들을 스타일링할 것이다.

### 여백 및 패딩

저는 이것이 실제로 이러한 기능들의 가장 좋은 사용 사례라고 생각합니다. 제가 작업하는 거의 모든 프로젝트에서 최소 또는 최대 여백(또는 패딩)을 `최소 폭` 또는 `최대 폭`으로 정의할 수 있는 지점이 있습니다.

다음 데모를 보십시오.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="450" width="100%" name="cp_embed_8" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/XWdBjXO?height=450&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=XWdBjXO&amp;pen-title=min%2C%20max%2C%20clamp%20_%20with%20cards_simple%20content&amp;name=cp_embed_8" style="width: 100%; overflow:hidden; display:block;" title="min, max, clamp _ with cards_simple content" loading="lazy" id="cp_embed_XWdBjXO"></iframe></div>

브라우저의 사용자 에이전트 스타일시트에 의해 설정된 카드 마진이 상당하다는 것을 알 수 있습니다. 예를 들어 브레이브 브라우저에서 사용자 에이전트 스타일시트는 다음과 같이 말합니다.

```undefined
margin-block-start: 0.83em;
margin-block-end: 0.83em;
```

다른 브라우저 설정을 보면 유사한 선언이 표시됩니다. 당신은 또한 그것이 `em` 단위를 사용하도록 설정되어 있기 때문에 그 크기는 우리의 지역 `font-size`의 영향을 받을 것이라는 것을 알게 될 것이다.

이제 이 데모를 보십시오.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="450" width="100%" name="cp_embed_9" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/PoNdmOa?height=450&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=PoNdmOa&amp;pen-title=min%2C%20max%2C%20clamp%20_%20negative%20margins&amp;name=cp_embed_9" style="width: 100%; overflow:hidden; display:block;" title="min, max, clamp _ negative margins" loading="lazy" id="cp_embed_PoNdmOa"></iframe></div>

내 `resolvedSectionsNav` 클래스에서 다음 항목이 있습니다.

```undefined
margin-top: min(-10px, -3vh );
```

이것은 우리가 긍정적일 수 있는 한 쉽게 이러한 기능에서 음수를 사용할 수 있다는 것을 지적하는 교육학적인 목적을 위한 약간 나쁜 코드이다.

#### 번호 테스트 중

`GetComputed Value`라는 버튼도 있습니다. 클릭하면 `마진톱`에 사용된 현재 계산된 값이 표시됩니다.

창 높이를 줄이면 카드타이틀의 em 크기가 줄어들기 때문에 여백도 줄어든다(기억해두세요. 글꼴 크기와 관련된 양의 argin-block-start와 argin-block-end가 있다).

그러나 우리의 카드를 분리하는 것은 그것이 -10px가 될 때까지 점점 더 (마진톱을 줄임으로써) 올라간다는 것에 주목하라.

이제 재미있는 예제가 사라졌으므로 더 심각한 예제는 다음과 같습니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="450" width="100%" name="cp_embed_10" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/jOqvGBd?height=450&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=jOqvGBd&amp;pen-title=min%2C%20max%2C%20clamp%20_%20margins&amp;name=cp_embed_10" style="width: 100%; overflow:hidden; display:block;" title="min, max, clamp _ margins" loading="lazy" id="cp_embed_jOqvGBd"></iframe></div>

카드타이틀 글꼴 크기를 좀 더 보기 좋게 변경했고, 제목에 중심을 맞췄다. 하지만 여기서 주목해야 할 주요 사항은 여백과 패딩입니다.

클램프()를 사용하여 뷰포트를 기준으로 "마진 하단"을 설정합니다.

```css
margin-bottom: clamp( 4px, 3.5vh, 1.5rem)
```

화면 높이를 줄이면 최소 4px의 간격에 도달할 때까지 행 사이의 여백이 줄어드는 반응성 동작을 수행할 수 있습니다.

코드펜의 작은 크기로는 차이를 알 수 없다는 것을 잘 알고 있습니다(뷰포트가 그렇게 크지 않고 뷰포트 높이와 비교 확인 중임). 따라서 이것이 상황을 얼마나 부드럽게 만들어 주는지 더 잘 보기 위해 축소한 다음 `마진-바텀`을 다음과 같이 변경할 수 있습니다.

```css
margin-bottom: clamp( 4px, 6.5vh, 5.5rem)
```

그런 다음 ".info" 텍스트에 "padding-left"와 "padding-right"를 추가하고 "text-indent"를 추가합니다.

여기서는 각 변수에 대해 동일한 변수를 다시 사용했습니다.

```css
.info {
  padding-left: var(--card-h-pad);
  padding-right: var(--card-h-pad);
  text-indent: calc(var(--card-h-pad) - 2px);
}
```

그리고 `카드-h-pad` 변수는 무엇을 포함하고 있는가? `클램프` 함수:

```css
--card-h-pad: clamp( 15px, 10%, 1.5rem);
```

### 폭 및 높이

min()과 max() 기능을 코드 크기를 줄이는 방법으로 보기 때문에 처음 보면 흥분하는 사람이 많다. 즉, 다음과 같은 것이 있는 대신 다음과 같은 것이 있습니다.

```css
.roundedCard {
  min-height: 75px;
  max-height: 125px;
  height: 25vh;  
}
```

그들은 훨씬 덜 장황하게 말할 수 있었다.

```css
.roundedCard {
  height: clamp(75px, 25vh, 125px);  
}
```

그건 사실입니다. 그들은 할 수 있습니다. 그러나 최소 높이, 최대 높이, 그리고 물론 최소 폭과 최대 폭의 속성이 여전히 존재하고 있으며, 오랫동안 (만약 그렇다면) 아무데도 가지 않을 것이라고 생각해 보자. 높이와 폭에서 이 기능을 사용할 수 있듯이 최소 높이, 최소 폭, 최대 높이, 최대 폭에서도 사용할 수 있기 때문에 결코 사라지지 않을 것이라고 개인적으로 믿는다.

min(max)과 clamp()가 자주 등장하는 또 다른 흥미는 미디어 질의에 덜 복잡한 코드를 쓸 수 있다는 점이다. 다음을 고려하십시오.

```css
.example {
  min-width: 100px;
  max-width: 200px;
  width: 80vw;
}

@media only screen and (min-width: 700px) {
  .example {
    max-width: 350px;
    width: 40vw;
  }
} 

@media only screen and (min-width: 1250px) {
  .example {
    max-width: unset;
    width: 500px;
  }
}
```

따라서 크기가 작은 화면에서 `.예`는 폭이 100px보다 작지 않고 200px보다 크지는 않지만 기본적으로 화면 크기의 80vw로 설정됩니다.

예를 들어 iPhone과 같이 화면 크기가 장치 폭에 따라 결정된다고 가정하면 실제로 이 예제의 폭은 200px일 것입니다. 어떤 개발자가 앉아서 무언가를 테스트하기 위해 화면을 아주 작게 만드는 것이 원인이라고 가정한다면, 우리는 아마도 우리의 최소 너비를 맞출 것입니다.

여기 중간 화면 크기는 700px입니다. 우리 화면이 700px일 때 너비는 `700 *.40`으로 280px를 의미한다. 물론 화면 크기가 커질수록 화면 크기가 875px가 될 때까지 .example의 너비가 늘어나는데, 이때 중간 화면 크기에 대한 최대 너비는 350px가 되어야 한다.

화면 크기가 1250px 이상인 경우, ".예" 너비는 정확히 500px입니다.

아래 예에서는 `overflow-wrap: break-word`를 추가하여 동작을 보다 쉽게 표시했습니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="450" width="100%" name="cp_embed_11" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/eYZbxmV?height=450&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=eYZbxmV&amp;pen-title=example%20min-widths&amp;name=cp_embed_11" style="width: 100%; overflow:hidden; display:block;" title="example min-widths" loading="lazy" id="cp_embed_eYZbxmV"></iframe></div>

당연히 우리는 다음과 같이 표현해야 합니다.

```css
.example {
  width: clamp(100px, 80vw, 200px);
}

@media only screen and (min-width: 700px) {
  .example {
    width: clamp(280px, 40vw, 350px);
  }
} 

@media only screen and (min-width: 1250px) {
  .example {
    width: 500px;
  }
}
```

여기서 우리의 단점은 폭 요구사항 때문에 700px의 새로운 min() 사이즈를 700px의 min() 사이즈로 써야 한다는 것인데, 이것은 우리가 이미 그 최소 폭보다 컸기 때문에 문제가 되지 않았다. 하지만 이것 외에도 명료성에는 합리적인 이득이 있다고 생각합니다.

`clamp()`의 예:

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="400" width="100%" name="cp_embed_12" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/LYNMqVa?height=400&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=LYNMqVa&amp;pen-title=example%20clamps%202&amp;name=cp_embed_12" style="width: 100%; overflow:hidden; display:block;" title="example clamps 2" loading="lazy" id="cp_embed_LYNMqVa"></iframe></div>

그러나 카드 레이아웃으로 돌아가면 라운드 카드 클래스에 다음과 같은 `폭`을 추가한다고 가정해 보겠습니다.

```undefined
width: clamp(200px, 25vw, 300px);
```

여기서 보실 수 있습니다. HTML 디스플레이 영역의 크기가 매우 작은 것을 보면 다음과 같은 것을 볼 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/display-area-small.png?resize=218%2C1094&ssl=1)

따라서 이 경우 다음과 같이 클램프 기능과 별도로 최소 너비:200px를 설정하면 다음과 같은 이점을 얻을 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/min-width-200px.png?resize=332%2C1188&ssl=1)

그러나 명심하십시오. 우리는 또한 유연한 컨테이너 안에 있습니다. 이 컨테이너는 사물이 어떻게 크기에 맞게 조정되는지를 스스로 제어할 수 있습니다. 이렇게 설정하면 어떻게 됩니까?

```undefined
min-width: clamp(200px, 25vw, 300px);
```

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/max-value-flex-container.png?resize=730%2C295&ssl=1)

첫 번째 카드는 최대값을 가져갑니다. 이 값은 나머지 카드의 최대값을 저장할 공간이 충분하지 않은 경우 기본값이 클램프됩니다.

clamp() 함수를 flex 레이아웃과 함께 사용하면 발생하는 또 다른 결과를 추론하기 어렵다. 하지만 물론 작은 콘텐츠 분야에서도 좋은 모습을 보이고 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/smaller-content-area.png?resize=382%2C774&ssl=1)

## 알아야 할 것 또는 다른 생각을 해야 할 것

### 만약 당신이 전처리를 한다면...

많은 사람들이 Sass, 또는 Less를 사용합니다. 제 개인적인 생각으로는, 이제 더 이상 css next와 PostCSs를 가지고 있지 않습니다. 이 도서관들은 자체 내장형(비강력)인 min()과 max() 기능을 가지고 있다. 따라서 이러한 사전 프로세서 중 하나에서 둘 중 하나를 사용하는 경우 실제 CSS `min() 또는 `max()를 사용 중임을 알리는 방법이 필요합니다.

그렇지 않으면 두 가지 중 하나가 발생합니다.

- 이 함수는 컴파일 시 평가되며, 이는 여러분이 원하지 않는 것일 수 있습니다.
- 예를 들어, 전처리가 이해하지 못할 vw 또는 vh 단위를 사용하는 경우와 같이 계산에서 값을 사용하기 때문에 모든 것이 실패합니다.

따라서 CSS `min() 및 `max()를 이스케이프해야 합니다. Sass에서는 할당 취소 기능을 사용하여 이 작업을 수행합니다. 따라서 width:min(200px, 50%, 50vw) 대신 width: unquote("min(200px, 50%, 50vw")라고 씁니다. Less에서 `e` 함수를 사용하여 이 작업을 수행합니다. 동일한 사항: e("min(200px, 50%, 50vw");`

### SVG와 관련하여…

SVG 2가 구현되면 이전에 SVG 요소에 직접 설정되어야 했던 많은 속성이 CSS 속성으로 설정될 수 있으며 이러한 속성 중 많은 부분이 길이, 백분율 등을 사용합니다. 따라서 최소, 최대, 클램프 등의 고급 CSS 기능과 calc 및 CSS 변수를 사용하여 인라인 SVG의 이러한 기능을 더 많이 조작할 수 있습니다.

길이 또는 백분율이 걸리는 `스트로크 너비`와 같이 CSS에서 설정할 수 있는 SVG 속성이 이미 있으며, 이러한 기능에서 작동할 것이다. 예는 다음과 같습니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="450" width="100%" name="cp_embed_13" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/ZEWZXMX?height=450&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=ZEWZXMX&amp;pen-title=svg%20with%20max&amp;name=cp_embed_13" style="width: 100%; overflow:hidden; display:block;" title="svg with max" loading="lazy" id="cp_embed_ZEWZXMX"></iframe></div>

원의 변경사항을 보려면 렌더링 영역의 크기를 조정합니다. 또한 완전성과 비교를 위해 max() 대신 clamp()를 사용하는 동그라미가 있습니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="450" width="100%" name="cp_embed_14" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/ExKJwzp?height=450&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=ExKJwzp&amp;pen-title=svg%20with%20clamp&amp;name=cp_embed_14" style="width: 100%; overflow:hidden; display:block;" title="svg with clamp" loading="lazy" id="cp_embed_ExKJwzp"></iframe></div>

분명히 여기서 min()은 비교해도 별로 흥미롭지 않을 것이다. 20px의 스트로크 너비를 얻을 수 있기 때문이다.

### 구현 테스트 중...

clamp()의 구현 방법 절의 마지막에 언급했듯이, 사람들이 이러한 기능에 대한 추론의 복잡성을 완화하는 방법 중 하나는 테스트인데, 어떤 경우에는 훌륭한 아이디어라고 생각한다.

예를 들어 창 크기에 따라 동적으로 변화하는 복잡한 크기를 작성할 때 요소의 `computedStyle`을 예상과 비교하여 테스트하는 것이 유용할 수 있다. 위의 `computedStyle` 예제를 이미 작성했지만 특정 GUI 테스트 솔루션에 대해 작성된 테스트에서 `computedStyle`을 사용하여 통합하는 것은 사용자의 몫입니다.

## 어떤 종류의 결론.

저는 우리가 당연한 결론에 도달했다고 느껴서가 아니라 기사가 압도적인 기장에 도달하기 시작했기 때문에 여기서 결론짓는 것입니다. 이것은 이해할 수 있다; 이러한 기능들은 겉으로 보기에는 간단하지만, 현대 CSS의 거의 모든 측면에 영향을 미칠 수 있는 많은 시사점을 가지고 있다.

특히 calc()와 변수와 결합할 경우 CSS의 이해력과 검정력 모두에 영향을 미친다. 응답성이 높은 설계가 구현되는 방식을 변경할 수 있으며 향후 Zeplin, Invision 및 Sketch와 같은 도구 설계에 영향을 미칠 것으로 예상됩니다.

그리고 그것은 우리가 그들이 영향을 미치는 것을 우리가 예상하는 바로 그 분야입니다. 분명 똑똑한 사람이 그것을 사용하여 색이나 애니메이션 효과를 구현한 많은 시나리오가 있을 것입니다. 여러분이 기대하지 않았을 것입니다. 그래서 우리는 이러한 기능들이 예상치 못한 방식으로 사용될 수 있다는 것을 명심해야 합니다. 혹은 자바스크립트가 어디에 있는지 궁금해하며 몇 시간을 보낼 준비를 해야 합니다.