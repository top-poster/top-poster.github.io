---
layout: post
title: "역동 성과 CSS 계산
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/Dynamism-CSS-Calculations.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Dynamism-CSS-Calculations.png?fit=730%2C480&ssl=1)

## 소개
 

CSS의 `min`, `max`, `clamp`함수에 대한 최근 기사에서 CSS 계산과 일반적으로 예상하지 못한 곳에서 어떻게 사용할 수 있는지에 대해 조금 다뤘습니다.
 예를 들어, HSL 색상을 동적으로 설정합니다.
 그러나 CSS 계산에는 명백한 단점이 있으며 이는 계산에 입력 할 수있는 동적 값이 없다는 것입니다.
 

예를 들어`hsl` 함수 내에서`min` 및`max` 함수를 사용하는 것을 보여주었습니다.
 

```undefined
hsl(min( 180, 190, 150), max(75%, 50%; 100%), 50%)
```

그 후, 나는 이것이 우리가 단순히 머리 속으로 계산을하고 글을 쓰는 것만으로는 얻을 수없는 것을 우리에게주지 않는다는 것을 지적했습니다.
 

```undefined
hsl(150, 100%, 50%)
```

이미 현실적으로 알 수없는 값을 가져올 방법을 찾지 않는 한 이러한 함수를 사용하면 불필요한 복잡성이 추가됩니다.
 물론 이것이 글꼴 크기 또는 너비 속성과 같은 경우에 사용되는 이유입니다.
 

여기에는 스타일을 작성할 때 알지 못했던 값 (백분율 또는 화면 크기와 관련된 값 또는 브라우저가 필요할 때만 결정되는 특성에 따라 측정 단위로 정의 됨)을 지정하는 다양한 방법이 있습니다.
 (예를 들어 em과 같은 단위).
 

그러나 실제로는 쓰기 시간이 아닌 렌더링 시간에 동적으로 값을 결정하는 여러 가지 방법이 있습니다.
 이 기사에서는 이러한 방법 중 몇 가지에 대한 개요를 제공하고 다른 방법으로는 사용할 수 없다고 생각할 수있는 곳에서 계산을 수행하는 데이 방법을 사용하는 방법을 보여줍니다.
 

## CSS 변수를 통한 역 동성
 

일반적으로 CSS 변수는 CSS 함수에 동적 값을 제공하는 데 가장 일반적으로 사용되는 방법이므로 대부분의 기사에서 이에 초점을 맞출 것입니다.
 CSS 변수의 값은 다양한 방식으로 변경 될 수 있습니다.
 원본 기사에서 나는 미디어 쿼리에서 변경되는 것을 보여주었습니다.
 

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/eYZKvqp?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=eYZKvqp&amp;pen-title=Min%2CMax%20with%20HSL%20media%20query&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="Min,Max with HSL media query" loading="lazy" id="cp_embed_eYZKvqp"></iframe></div>

나중에 그것에 대해 자세히 알아보십시오.
 

또한 JavaScript에 의해 변경되는 것을 보여주었습니다.
 

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_2" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/abNKWOp?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=abNKWOp&amp;pen-title=Min%2CMax%20with%20HSL%20JavaScript%20updating&amp;name=cp_embed_2" style="width: 100%; overflow:hidden; display:block;" title="Min,Max with HSL JavaScript updating" loading="lazy" id="cp_embed_abNKWOp"></iframe></div>

그러나 CSS 변수의 값을 변경할 수있는 다른 방법이 많이 있습니다.
 이 예제의 나머지 부분에서는 적어도 세 가지 브라우저에서 지원하는 방법에 중점을 둘 것입니다.
 

변수 값을 변경하는 가장 확실한 방법은 다른 선택기를 사용하여 다른 값을 설정하는 것입니다. 즉, 일반적으로 속성 값을 설정하는 것과 동일한 방식으로 변수 값을 설정할 수 있습니다.
 

## 변수 전환
 

CSS 변수를 사용하는 강력한 기술 중 하나는 전환 변수를 제공하는 것입니다. Ana Tudor는 "CSS 변수를 사용한 DRY 전환 : 하나의 선언의 차이"및 "CSS 변수를 사용한 논리 연산"기사에서 다룹니다.
 

기본적으로이 방법은 일종의 스위치 역할을하는 1 또는 0의 두 가지 가능한 값을 가진 변수를 사용하여 작동합니다 (예 : 전등 스위치).
 1은 스위치가 켜져 있음을 나타내고 0은 꺼져 있음을 나타냅니다.
 

수학 연산에서 이러한 변수를 사용하여 작동합니다.
 예를 들어 요소를 30도 회전하거나 회전하지 않으려면`calc (i * 30deg)`라고합니다.
 `calc`가 0이면 아무 일도 일어나지 않습니다.
 1이면 요소가 30도 회전합니다.
 

물론 이러한 변수 이름을 사용할 수있게되면 JavaScript 없이도 CSS 내에서 직접 수행 할 수있는 강력한 논리 연산을 만들 수 있습니다.
 

## 선택기를 사용하여 얻은 역 동성
 

특정 선택자 내에서 변수 값을 설정할 수 있다는 것은 분명하지만이 명백한 사실은 의미를 고려하지 않고는 분명하지 않은 복잡한 일을 많이 가능하게합니다. 당연히 "명백한"이라는 단어를 사용하게 될 것 같습니다.
 이 기사에서 많이.
 

### Facebook 재 설계
 

Facebook은 최근에 클래스 내에서 변수를 변경하는이 방법을 사용하는 CSS 작성 및 구조화 방법을 변경했습니다.
 기사에서 인용 :
 

“CSS 변수는 클래스 아래에 정의되며 해당 클래스가 DOM 요소에 적용되면 해당 값이 DOM 하위 트리 내의 스타일에 적용됩니다.
 이를 통해 테마를 단일 스타일 시트로 결합 할 수 있습니다. 즉, 다른 테마를 전환 할 때 페이지를 새로 고침 할 필요가없고, 다른 페이지는 추가 CSS를 다운로드하지 않고도 다른 테마를 가질 수 있으며, 다른 제품은 같은 페이지에서 다른 테마를 나란히 사용할 수 있습니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Facebook-CSS-Theme-Redesign.png?resize=730%2C235&ssl=1)

“이로 인해 구성 요소 라이브러리의 크기 나 복잡성이 아니라 색상 팔레트의 크기에 비례하여 테마의 성능에 영향을 미쳤습니다.
 단일 원자 CSS 번들에는 다크 모드 구현도 포함됩니다.”
 

그러나 클래스와 ID의 명백한 사용 이외에 많은 잠재적 선택자가 있습니다.
 이것들을 자세히 살펴 보겠습니다.
 

### 의사 클래스를 사용한 역 동성
 

클래스를 사용하여 변수를 변경하고 CSS 상속 규칙을 사용하여 입력을 변경할 수 있다면 유사 클래스를 사용하여 동일한 작업을 수행 할 수 있습니다.
 

다음은 `hover`상태를 사용하여 변수 값을 변경하는 간단한 예입니다.
 

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_3" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/MWjevWW?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=MWjevWW&amp;pen-title=hover%20variable%20change&amp;name=cp_embed_3" style="width: 100%; overflow:hidden; display:block;" title="hover variable change" loading="lazy" id="cp_embed_MWjevWW"></iframe></div>

불행히도 우리가하는 모든 변경은 변경된 요소의 컨텍스트에서 이루어져야하므로 하나의 하위 트리에서 변수를 변경하여 DOM의 별도 부분에 영향을주는 것은 불가능합니다. 예를 들면 다음과 같습니다.
 

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_4" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/ZEpOjYM?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=ZEpOjYM&amp;pen-title=hover%20variable%20change%202&amp;name=cp_embed_4" style="width: 100%; overflow:hidden; display:block;" title="hover variable change 2" loading="lazy" id="cp_embed_ZEpOjYM"></iframe></div>

여기서`hover` 이벤트는`--box-background` 변수를 firebrick red로 설정합니다.
 

```css
.example:hover {
--size: 3rem;
--box-background: hsl(0, 100%, 59%);
}
```

그러나`.box` 클래스가있는 요소는`.example` 클래스 내에 있지 않기 때문에 :
 

```undefined
<div class="example">Example</div>
<div class="box">Box</div>
```

`.box` 클래스 내의 실제 요소는 검은 색 배경으로 유지됩니다.
 그러나 다음과 같이`.example`의 후손이되도록 변경하면 :
 

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_5" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/oNzLMqL?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=oNzLMqL&amp;pen-title=hover%20variable%20change%203&amp;name=cp_embed_5" style="width: 100%; overflow:hidden; display:block;" title="hover variable change 3" loading="lazy" id="cp_embed_oNzLMqL"></iframe></div>

```undefined
<div class="example">Example
<div class="box">Box&lt;/div>
</div>
```

그런 다음`.example`을 가리키면 배경이 빨간색으로 바뀝니다.
 

여러 유형의 의사 클래스가 있지만 CSS 변수로 더 흥미로운 작업을 수행 할 수있는 의사 클래스에만 집중할 것입니다.
 

#### 언어 의사 클래스
 

이를 통해 언어 또는 스크립트 방향에 따라 선택할 수 있습니다.
 언어와 관련하여 이것은`lang` 속성과`meta` 요소의 조합에 의해 결정될 수 있습니다.
 

이론적으로 언어는`Content-Language` HTTP 헤더에 의해 결정될 수도 있지만 누군가 우리 페이지를 로컬에 저장하면 스타일이 변경됩니다.
 최상의 결과를 얻으려면 일반적으로 언어를`lang` 속성으로 설정해야합니다.
 

#### 위치 의사 클래스
 

이름에서 알 수 있듯이 이들은 위치와 관련된 의사 클래스입니다.
 일반적으로 이러한 요소는 위치에 연결되는 요소와 관련이 있습니다. 예를 들어 `호버`상태인지 `활성`상태인지 또는 링크가 이미 방문되었는지 여부에 따라 다른 스타일을 가질 수있는 요소입니다.
 

하지만 위치 카테고리에는 자주 사용되지 않는 의사 클래스가 있습니다. `target`의사 클래스로 현재 활성 타겟 인 페이지의 일부를 스타일링 할 수 있습니다.
 즉, 페이지의 일부로 연결되는 링크가있는 경우 링크를 클릭 할 때 페이지의 해당 부분에 스타일을 지정할 수 있습니다.
 

아래 예를 참조하십시오.
 

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_6" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/WNGxgMV?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=WNGxgMV&amp;pen-title=target%20pseudo%20selector&amp;name=cp_embed_6" style="width: 100%; overflow:hidden; display:block;" title="target pseudo selector" loading="lazy" id="cp_embed_WNGxgMV"></iframe></div>

#### 사용자 작업 의사 클래스
 

대부분의 사람들이 `hover`, `active`및 `focus`클래스를 사용하는 가장 널리 사용되는 의사 클래스 일 것입니다.
 

`focus`와 관련하여 변수 설정과 관련하여 매우 흥미로운 또 다른 의사 클래스가 있습니다. 바로 `focus-within`의사 클래스입니다.
 

다음은 이전`target` 의사 클래스가`focus-within`으로 변경된 것입니다.
 

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_7" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/wvzgOmW?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=wvzgOmW&amp;pen-title=focus-within%20pseudo-selector&amp;name=cp_embed_7" style="width: 100%; overflow:hidden; display:block;" title="focus-within pseudo-selector" loading="lazy" id="cp_embed_wvzgOmW"></iframe></div>

따라서 사용자가 링크를 클릭하여 포커스를 주면 상자 배경이 녹색으로 바뀌고 텍스트가 검은 색으로 변경됩니다.
 

이 예제를 통해 좀 더 살펴 보겠습니다.
 

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_8" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/ExgZMOd?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=ExgZMOd&amp;pen-title=focus-within%20pseudo-selector%202&amp;name=cp_embed_8" style="width: 100%; overflow:hidden; display:block;" title="focus-within pseudo-selector 2" loading="lazy" id="cp_embed_ExgZMOd"></iframe></div>

실제 동작을 보려면 먼저 예제 링크를 클릭하십시오.
 링크에 포커스가 있으므로 두 상자의 색상이 변경되는 것을 볼 수 있습니다.
 그런 다음 탭 키를 누르면 다음 코드로 인해 포커스가 링크로 다시 이동해야합니다. 다시 두 개의 상자가 같은 녹색이됩니다.
 

```css
.example:focus-within {
--box-background: hsl(90, 100%, 49%);
--box-color: hsl(0, 0%, 2%);
}
```

그런 다음 Tab 키를 다시 누르고 포커스를 버튼으로 이동합니다. 이전과 동일한 동작입니다.
 

그런 다음 탭 키를 다시 누릅니다.
 이제`tabindex = "0"`이있는`focusableDiv` 클래스가있는 div로 탭했습니다.
 두 번째 상자는 여전히 위와 같이`focus-within` 변수가 설정되어 있기 때문에 검은 색 텍스트가있는 녹색 배경을 가지고 있지만,`focusableDiv`는 다음과 같이`focus `의사 클래스를 사용하기 때문에 검은 색 텍스트가있는 흰색입니다.
 

```css
.focusableDiv:focus {
--box-background: #fff;
--box-color: hsl(0, 0%, 2%);
}
```

대부분의 사람들이 충분히 잘 알고 있다고 생각하므로 여기서는 입력 의사 클래스 또는 트리 구조 의사 클래스를 살펴 보지 않을 것입니다.
 

## 미디어 쿼리를 사용한 역 동성
 

미디어 쿼리를 사용하여 변수를 변경할 때 변수의 범위를 고려해야합니다.
 변수를 전역 적으로 변경하려면`:: root` 선택기를 사용하는 것이 가장 좋습니다.
 

일반적으로 미디어 쿼리가 변수를 변경하는 데 사용되는 경우 쿼리는 화면 크기의 변화를 기반으로하므로 화면의 요소가 얼마나 커야하는지 또는 그 사이의 거리가 얼마나되는지 결정하는 데 주로 유용합니다.
 그들.
 일반적으로 화면 크기와 관련된 다른 미디어 쿼리 (예 : `화면비`및 `방향`쿼리)가 있습니다.
 

인쇄용인지 여부와 같이 미디어 자체와 관련된 쿼리가 있습니다.
 또한 장치 색상과 관련된 변수 값을 설정하는 데 사용할 수있는 다양한 미디어 쿼리가 있습니다.
 

그러나 일반적으로 이러한 색상 규칙은 CanIuse 페이지가 나타내는 것처럼 실제로 유용하지 않습니다.
 

### `@media (색상)`규칙
 

`color` 미디어 쿼리는 출력 장치의 색상 구성 요소 (빨간색, 녹색, 파란색) 당 비트 수 또는 사용 가능한 색상이 있는지 테스트하는 데 사용할 수 있습니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Media-Color-Rule-Output.png?resize=730%2C178&ssl=1)

### `@media (color-gamut)`규칙
 

`color-gamut`미디어 쿼리를 사용하여 사용자 에이전트 및 출력 장치에서 지원하는 대략적인 색상 범위를 테스트 할 수 있습니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Media-Color-Gamut-Rule-Output.png?resize=730%2C221&ssl=1)

Firefox에서는 작동하지 않습니다.
 

### `@media (흑백)`규칙
 

`모노크롬`미디어 쿼리는 출력 장치의 흑백 프레임 버퍼에서 픽셀 당 비트 수를 테스트하는 데 사용할 수 있습니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Media-Monochrome-Query-Output.png?resize=730%2C179&ssl=1)

사용자에게 어떤 설정이 있는지 알려주기 위해 약간의 CodePen을 만들었습니다 ( `color-gamut`쿼리에 대한 Firefox에서는 작동하지 않음).
 

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_9" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/JjRWvqY?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=JjRWvqY&amp;pen-title=color%20media%20queries%20checks&amp;name=cp_embed_9" style="width: 100%; overflow:hidden; display:block;" title="color media queries checks" loading="lazy" id="cp_embed_JjRWvqY"></iframe></div>

따라서 Chrome의 이전 Macbook Air에서 다음과 같이 표시됩니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/CodePen-Monochrome-Chrome-Output-1.png?resize=703%2C311&ssl=1)

이것은 기본적으로 구형 Macbook Air에서 기대할 수있는 것입니다.
 불행히도 이것은 Touch Lux 4에서도 나에게 알려줍니다.
 

내가 테스트 한 흑백 기기 (친구의 Nook, Pocketbook Touch Lux 4, 오래된 Kindle)에서는 `흑백`미디어 쿼리가 작동하지 않았습니다.
 

작동하는 한 가지는`prefers-color-scheme` 미디어 쿼리입니다.
 

### `@media : prefers-color-scheme` 규칙
 

`prefers-color-scheme` 미디어 쿼리는 사용자가 시스템에 밝은 색상 또는 어두운 색상 테마를 사용하도록 요청했는지 감지하는 데 사용됩니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Media-Prefers-Color-Scheme-Rule-Output.png?resize=730%2C169&ssl=1)

다음은이를 사용한 예입니다.
 

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_10" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/RwGgQrG?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=RwGgQrG&amp;pen-title=Dark%20And%20Light%20Mode%20HSL&amp;name=cp_embed_10" style="width: 100%; overflow:hidden; display:block;" title="Dark And Light Mode HSL" loading="lazy" id="cp_embed_RwGgQrG"></iframe></div>

기본 밝은 색 구성표가 있으면 다음과 같은 것을 볼 수 있습니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Light-Color-Scheme-Default-Visual.png?resize=730%2C119&ssl=1)

어두운 색 구성표를 켜는 방법이 궁금한 경우 방법은 브라우저 및 OS마다 다르므로 특정 장치를 찾아야합니다.
 

예를 들어, Mac의 Chrome에서는 경로에 DevTools를 통과하는 것이 포함되어있는 반면 Android 태블릿의 Chrome에서는 설정-> 테마에서 수행 할 수 있습니다.
 Firefox에서는 아마도`about : config`를 열고`ui.systemUsesDarkTheme`를 입력 한 다음 1로 설정하는 것이 가장 확실 할 것입니다.
 

어떤 방법을 선택하든 나중에 브라우저를 다시 시작하거나 최소한 새 탭을 열어 변경 사항이 적용되는지 확인해야 할 수 있습니다.
 Firefox에서`ui.systemUsesDarkTheme` 메소드를 사용하고 있으며 위의 스타일이 내 모습을 어떻게 보여 주는지 다음과 같습니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Dark-Color-Scheme-Visual.png?resize=730%2C130&ssl=1)

## `@ supports` 쿼리를 사용한 역 동성
 

`@ supports` CSS 규칙을 사용하면 브라우저에서 CSS 사양의 기능에 대한 특정 지원을 기반으로 변수를 변경할 수 있습니다.
 

분명히`@ supports` 쿼리는 우리가 검사 할 속성에서 사용할 변수를 설정하는 데 더 유용합니다.
 미디어 쿼리와 유사하게 범위에 대한 동일한 조항이 여기에 적용됩니다. 따라서 분명히`:: root` 선택기에서 변수를 설정합니다.
 

## 고려할 수있는 문제
 

### 캐스케이드의 위험성
 

CSS 캐스케이드는 때때로 개발자가 제어하는 데 문제가 있습니다.
 개인적으로 그렇게 찾은 적은 없지만 충돌하는 스타일을 처리하는 데 많은 버그와 개발자 시간이 낭비된다는 사실을 인정해야합니다.
 

다음 예에서 변수의 값은 `counter`를 통해 `content`속성에 기록되며 해당 값은 값을 설정하는 선택기와 동일한 컨텍스트를 갖는 여러 선택기에 의해 설정됩니다.
 

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_11" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/xxEdQEL?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=xxEdQEL&amp;pen-title=cascade%20and%20switch%20variables&amp;name=cp_embed_11" style="width: 100%; overflow:hidden; display:block;" title="cascade and switch variables" loading="lazy" id="cp_embed_xxEdQEL"></iframe></div>

`content`에 쓰여진 값은 CodePen CSS 패널 중간에있는이 특정 선택기를 기반으로 4입니다 (교육 목적으로 두려운 `! important`사용).
 

```css
div > div {
--pos: 4 !important;
}
```

다음 사항도 고려하십시오.
 

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_12" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/mdrmQvY?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=mdrmQvY&amp;pen-title=cascade%20and%20switch%20variables%202&amp;name=cp_embed_12" style="width: 100%; overflow:hidden; display:block;" title="cascade and switch variables 2" loading="lazy" id="cp_embed_mdrmQvY"></iframe></div>

여기서 `content`의 값은 3입니다.
 

마크 업은 다음과 같습니다.
 

```js
<div class="example">
<div class="switcher _pos4 _pos3 _pos2"></div>
</div>
```

CSS 설정 변수 값은 다음과 같습니다.
 

```css
._pos2 {
--pos: 2;
}
```

```css
._pos4 {
--pos: 4;
}
```

```css
._pos3 {
--pos: 3;
}
```

CSS 캐스케이드의 규칙이 주어지면`pos` 변수는 스타일 시트의 마지막 클래스에 정의 된 값을 취하므로 숫자 3을 출력합니다. 클래스`_pos3`이
 `class` 속성.
 

물론 규칙이 더 구체적인 경우 다음과 같이 우선 적용됩니다.
 

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_13" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/yLabGop?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=yLabGop&amp;pen-title=cascade%20and%20switch%20variables%203&amp;name=cp_embed_13" style="width: 100%; overflow:hidden; display:block;" title="cascade and switch variables 3" loading="lazy" id="cp_embed_yLabGop"></iframe></div>

`content`에 출력되는 값은 다음과 같이 변경하여 2입니다.
 

```css
._pos2 {
--pos: 2;
}
```

더 구체적으로 말하면 :
 

```css
.example ._pos2 {
--pos: 2;
}
```

개발자가 대규모 CSS 코드베이스에서 캐스케이드를 제어하는 데 종종 문제가 있다는 점을 고려하여 CSS 구성을위한 다양한 지침, 특히 OOCSS, BEM 및 SMACSS가 만들어졌습니다.
 

개발자가 CSS 변수를 설정하고 재정의하는 클래스를 유지하고 사용하도록 돕는 목적으로 개발되지 않았다는 점을 지적하는 것 외에는 이러한 다양한 방법론에 대해 더 이상 논의하지 않겠습니다.
 따라서 이전에 논의한 변수 전환 기술 또는 Facebook 재 설계와 같이 유사하게 변수가 많은 솔루션에서 발생할 수있는 충돌을 처리하는 데 도움이되지 않습니다.
 

마지막으로,`clamp`,`min`,`max` 또는`hsl`과 같은 다른`color` 함수와 같이 CSS에서 사용할 수있는 일부 함수는 매우 원자 적 변수를 사용하도록 유혹 할 수 있습니다.
 여기).
 

특정 선택자에 의해 설정된 경우 하위 요소가 예상하지 못한 방식으로 해당 변수를 사용하는 경우 원자 변수를 디버그하기가 특히 어려울 수 있습니다.
 고급 계산을 수행하기위한 원자 변수와 결합하면 요소의 실제 속성을 설정하는 데 사용하는 것보다 캐스케이드가 훨씬 더 문제가 될 수 있습니다.
 

### 변수 변경의 위험
 

캐스케이드에서 발생할 수있는 위험 외에도 변수 값을 변경할 때 발생할 수있는 다른 문제가 있습니다.
 문제는 변수 변경이 속성 변경과 같은 방식으로 작동하지 않는다는 것입니다.
 

다음과 같은 CSS가 있다고 가정하겠습니다.
 

```css
.example {
background: black;
background: "howdy";
}
```

빌드 도구가 어떤 이유로이를 통과했다고 가정하면`.example` 클래스의 배경색은 무엇입니까?
 분명히, 당신의 브라우저가 검은 색으로 사용하는 것은` "howdy"`가 유효하지 않은 값이기 때문에 폐기 될 것입니다.
 

속성에 유효한 값이 무엇인지 알고 있기 때문에 유효하지 않은 값은 속성에 대해 버릴 수 있습니다.
 대부분의 CSS 속성은 강력하게 형식화되어 있습니다 (제가 잊은 경우를 대비하여 "most"라고 말했지만 콘텐츠조차도 문자열 만 사용하고`10px`를` "10px"`로 변환하지 않으므로이 문제에 대해 꽤 확신합니다).
 

그러나 CSS 변수는 모든 속성에서 사용할 수 있기 때문에 약한 유형이며 변수를 색상에서 숫자로, 상식이 아닌 문자열로 변경하지 못하도록하는 본질적인 방법이 없습니다.
 

예를 들어 다음 CodePen을 살펴보십시오.
 

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_14" scrolling="no" src="https://codepen.io/bryanrasmussen/embed/poEWxXZ?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=bryanrasmussen&amp;slug-hash=poEWxXZ&amp;pen-title=attr()%20variable%20change&amp;name=cp_embed_14" style="width: 100%; overflow:hidden; display:block;" title="attr() variable change" loading="lazy" id="cp_embed_poEWxXZ"></iframe></div>

`: hover` 선택자가`attribute` 선택자 앞에 사용된다는 점에서 계단식 문제가 있음을 알 수 있습니다. 즉,`.example` 요소를 가리키면`--box-background` 변수가
 초기화.
 

그러나 다음 코드로 `--box-background`유형도 변경합니다.
 

```css
.example[data-color] {
background: navy;
background: attr(data-color color);
--box-background: attr(data-color color);
}
```

HSL 색상에서 현재 거의 모든 브라우저의`content` 속성에 사용할 문자열 만 반환하는`attr ()`함수를 사용하여 배경 값으로 지원되지 않는 색상까지 :
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Attr-Funcrion-Returns-String.png?resize=730%2C342&ssl=1)

그렇다면`--box-variable`이 지원되지 않는 경우`.box` 요소는 어떤 배경색입니까?
 아무것도.
 

이제 배경을 다음과 같이 설정 한`.example` 요소는 배경색입니다.
 

```undefined
background: navy;
background: attr(data-color color);
```

해군입니다.
 

물론,`attr ()`함수가`content`뿐만 아니라 모든 속성에 대해 지원되면 위에 표시된 코드를 실행할 수 있으며`.box` 요소의 배경색을 녹색으로 할 수 있습니다.
 따라서 앞으로이 기사를 읽는다면 축하합니다. 모든 속성 및 데이터 유형에 대해`attr ()`함수 사용을 지원하는 브라우저를 사용하고 있습니다.
 

## 이러한 문제를 피하는 방법
 

불행히도 기사는 이미 상당히 커지고 있으므로 이러한 문제를 최소화하기 위해 변수를 구성하는 방법에 대해서는 여기서 제안하지 않을 것입니다.
 그러나 더 많은 가변 조작으로 개발을 시작하면 문제를 고려해야합니다.
 이러한 문제를 정확히 피하고 수정하기위한 향후 기사에서 가능한 전략에 대해 더 자세히 알아 보려고합니다.
 

## 결론
 

이것이 일반적으로 고려할 수있는 메서드 외부의 속성 값을 제어하고 변경할 수있는 몇 가지 방법에 대한 유용한 개요가 되었기를 바랍니다.
 