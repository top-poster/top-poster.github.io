---
layout: post
title: "웹 구성 요소를 사용하여 막대 차트 라이브러리 구축
 "
author: 'Code Tower'
thumbnail: url("https://blog.logrocket.com/wp-content/uploads/2021/01/webcomponents-bar-graph-library.png")
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/webcomponents-bar-graph-library.png?fit=730%2C487&ssl=1)

막대 차트는 막대가 범주의 직접 매핑이고 크기 (세로 막대의 높이)가 나타내는 값에 비례하는 범주 형 데이터 세트의 시각적 표현입니다.
 

한 축에 막대 크기와 일치하는 선형 눈금이있는 경우 다른 축 (카테고리)에 대한 막대의 위치는 일반적으로 그다지 중요하지 않으며 단순히 공간을 균등하게 차지합니다.
 

이 기사에서는 웹 구성 요소를 사용하여 막대 차트 라이브러리를 작성하는 방법을 다룹니다.
 

## 막대 차트 라이브러리에서 세그먼트 단위 만들기
 

먼저 막대의 비율을 계산하려면 표시하려는 가능한 값의 영역을 나타내는 하나의 단위 세그먼트에 대해 값을 투영하는 간단한 함수가 필요합니다.
 

```coffeescript
const createScale = ({domainMin, domainMax}) => (value) => (value - domainMin) / (domainMax - domainMin);
```

예를 들어, 한 단위의 세그먼트가 0에서 100으로 이동하는 경우 값 50은 세그먼트의 중간에있는 반면 25는 분기에 있습니다.
 

```cpp
const scale = createScale({domainMin: 0, domainMax: 100});

scale(50) // > 0.5

scale(25) // > 0.25
```

세그먼트의 단위가 물리적으로 원하는 것은 사용자에게 달려 있습니다 (900px, 4cm 등).
 또한 도메인에서 정의한 범위를 벗어난 값 (예 : 세그먼트에 맞지 않는 값)도 처리해야합니다.
 

일반적으로 값이 더 높으면 세그먼트 끝에서 맨 위에있는 반면, 더 낮 으면 상대적 비율은 단순히 null이됩니다.
 

```js
// an utility to compose functions together
const compose = (...fns) => (arg) => fns.reduceRight((acc, cfn) => cfn(acc), arg);

const greaterOrEqual = (min) => (value) => Math.max(min, value);

const lowerOrEqual = (max) => (value) => Math.min(max, value);

const createProjection = ({domainMin, domainMax}) => compose(
    lowerOrEqual(1),
    greaterOrEqual(0),
    createScale({
        domainMin,
        domainMax
    })
);

// example
const project = createProjection({domainMin: 0, domainMax: 100});

project(50); // > 0.5 "unit"

project(120); // > 1 "unit"

project(-40); // > 0 "unit
```

## 웹 구성 요소 란 무엇입니까?
 

웹 구성 요소는 개발자에게 공유 가능한 UI 컨트롤을 일반 DOM 요소로 만들 수있는 기능을 제공하는 세 가지 기술 집합입니다.
 

- Custom Elements는 저수준 API를 제공하여 새로운 HTML 요소를 생성
 
- Shadow DOM을 사용하면 개인 DOM 하위 트리를 캡슐화하고 문서의 나머지 부분에서 숨길 수 있습니다.
 
- HTML 템플릿 (`<template>`및`<slot>`)은 하위 트리의 디자인과 다른 DOM 트리에 맞는 방식을 지원합니다.
 

웹 구성 요소를 만들기 위해 모두 함께 사용할 필요는 없습니다.
 사람들은 종종 웹 구성 요소를 Shadow DOM과 혼동하지만 Shadow DOM이 전혀없는 사용자 지정 요소를 만들 수 있습니다.
 

## 사용자 정의 요소로 바 구성 요소 만들기
 

맞춤 요소의 장점은 HTML을 통해 선언적 방식으로 또는 HTML 요소 (속성, 이벤트, 선택기 등)와 동일한 API를 사용하여 프로그래밍 방식으로 사용할 수있는 유효한 HTML 요소라는 사실에 있습니다.
 

사용자 정의 요소를 만들려면 HTML 요소 기본 클래스를 확장하는 클래스가 필요합니다.
 그런 다음 일부 수명주기 및 후크 메서드에 액세스 할 수 있습니다.
 

```undefined
export class Bar extends HTMLElement {

    static get observedAttributes() {
        return ['size'];
    }

    get size() {
        return Number(this.getAttribute('size'));
    }

    set size(value) {
        this.setAttribute('size', value);
    }

    // the absolute value mapped to the bar
    get value() {
        return Number(this.getAttribute('value'));
    }

    set value(val) {
        this.setAttribute('value', val);
    }

    attributeChangedCallback() {
        this.style.setProperty('--bar-size', `${this.size}%`);
    }
}

customElements.define('app-bar', Bar);
```

일반적으로 getter 및 setter를 통한 프로그래밍 방식 액세스와 함께 HTML 속성 (이 경우 `size`)을 통해 선언적 API를 정의합니다.
 커스텀 엘리먼트는 정적 getter`observedAttributes`와 반응 콜백`attributeChangedCallback`을 통해 관찰 가능한 속성을 노출함으로써 (일반적인 UI 자바 스크립트 프레임 워크에서 찾을 수 있듯이) 일종의 반응 바인딩을 제공합니다.
 

우리의 경우`size` 속성이 변경 될 때마다 바 비율을 설정하는 데 사용할 수있는 CSS 변수 인 구성 요소 스타일 속성`--bar-size`를 업데이트합니다.
 

이상적으로 접근자는 속성을 반영해야하므로 소비자가 구성 요소 (속성, 프로그래밍 방식 등)를 사용하는 방법을 모르기 때문에 간단한 데이터 유형 (문자열, 숫자, 부울) 만 사용해야합니다.
 

마지막으로 브라우저가 DOM에서 찾은 새 HTML 요소를 처리하는 방법을 알 수 있도록 사용자 정의 요소를 전역 레지스트리에 등록해야합니다.
 

이제 HTML 문서에`app-bar` 태그를 놓을 수 있습니다.
 HTML 요소로 CSS 스타일 시트와 스타일을 연결할 수 있습니다.
 예를 들어 우리의 경우 반응 형 CSS 변수`--bar-size`를 활용하여 막대의 높이를 관리 할 수 있습니다.
 

다음 코드 펜 또는 stackblitz로 실행중인 예제를 찾을 수 있습니다 (보다 체계적인 샘플 용).
 막대의 높이 외에도 몇 가지 애니메이션과 개선 사항을 추가하여 우리의 요점을 증명했습니다.
 사용자 정의 요소는 모든 HTML 요소 앞에 있으므로 CSS 및 HTML과 같은 표준 웹 기술로 표현력이 뛰어납니다.
 

## 막대 차트 영역 만들기
 

이전 섹션에서는 간단한 웹 구성 요소와 스타일 시트 덕분에 실제 막대 차트에 가까운 것을 만들 수있었습니다.
 그러나 적용된 스타일 중 일부가 사용자 정의 된 경우 많은 부분이 막대 차트의 기능 요구 사항의 일부입니다.
 

- 막대 높이의 비율
 
- 범주 막대가 공간을 차지하는 방식 (시각적 편견을 피하기 위해 균등하게)
 

따라서 우리는 그 부분을 구성 요소에 캡슐화하여 소비자가 사용하는 것을 덜 지루하고 반복적으로 만들 필요가 있습니다.
 Shadow DOM을 입력하십시오.
 

Shadow DOM을 사용하면 웹 구성 요소가 문서의 나머지 부분과 분리 된 자체 DOM 트리를 만들 수 있습니다.
 블랙 박스처럼 다른 요소가 알지 못해도 내부 구조를 설정할 수 있다는 뜻입니다.
 

같은 방식으로 내부 부분에 특정한 개인 및 범위 스타일 규칙을 정의 할 수 있습니다.
 다음 예에서 어떻게 진행되는지 살펴 보겠습니다.
 

```js
import {createProjection} from './util.js';

const template = document.createElement('template');

/// language=css
const style = `
:host{
    display: grid;
    width:100%;
    height: 100%;
}

:host([hidden]){
    display:none;
}

#bar-area{
    align-items: flex-end;
    display:flex;
    justify-content: space-around;
}

::slotted(app-bar){
    flex-grow: 1;
    height: var(--bar-size, 0%);
    background: salmon; // default color which can be overwritten by the consumer
}
`;

template.innerHTML = `
<style>${style}</style>
<div id="bar-area">
    <slot></slot>
</div>
`;

export class BarChart extends HTMLElement {

    static get observedAttributes() {
        return ['domainmin', 'domainmax'];
    }

    get domainMin() {
        return this.hasAttribute('domainmin') ?
            Number(this.getAttribute('domainmin')) :
            Math.min(...[...this.querySelectorAll('app-bar')].map(b => b.value));
    }

    set domainMin(val) {
        this.setAttribute('domainmin', val);
    }

    get domainMax() {
        return this.hasAttribute('domainmax') ?
            Number(this.getAttribute('domainmax')) :
            Math.max(...[...this.querySelectorAll('app-bar')].map(b => b.value));
    }

    set domainMax(val) {
        this.setAttribute('domainmax', val);
    }

    attributeChangedCallback(...args) {
        this.update();
    }

    constructor() {
        super();
        this.attachShadow({mode: 'open'});
        this.shadowRoot.appendChild(template.content.cloneNode(true));
    }

    update() {
        const project = createProjection({domainMin: this.domainMin, domainMax: this.domainMax});
        const bars = this.querySelectorAll('app-bar');

        for (const bar of bars) {
            bar.size = project(bar.value);
        }
    }

    connectedCallback() {
        this.shadowRoot.querySelector('slot')
            .addEventListener('slotchange', () => this.update());
    }
}

customElements.define('app-bar-chart', BarChart);
```

여기에는 새로운 일이 거의 없습니다.
 먼저 DOM 트리가있는`template` 요소를 생성합니다.이 요소는 첨부 된 shadow DOM (cf 생성자) 덕분에 문서의 개인 트리로 사용됩니다.
 

이 템플릿에는 슬롯 요소가 있으며 이는 본질적으로 구성 요소의 소비자가 다른 HTML 요소로 채울 수있는 구멍입니다.
 이 경우 해당 요소는 웹 구성 요소의 Shadow DOM에 속하지 않으며 상위 범위에 남아 있습니다.
 그러나 그들은 Shadow DOM 레이아웃에 정의 된대로 위치를 잡을 것입니다.
 

우리는 또한`connectedCallback`이라는 새로운 라이프 사이클 메서드를 사용합니다.
 이 함수는 구성 요소가 문서에 마운트 될 때마다 실행됩니다.
 슬롯 콘텐츠 (바)가 변경 될 때마다 컴포넌트를 다시 렌더링하도록 요청하는 이벤트 리스너를 등록합니다.
 

막대 차트의 기능적 요구 사항을 구현하고 캡슐화 할 수있는 범위 스타일이 있습니다 (이전에 글로벌 스타일 시트를 통해 달성 된 것).
 가상 요소`: host`는 웹 구성 요소 루트 노드를 나타내는 반면`:: slotted`는 구성 요소가 "수신 된"요소 (이 경우 막대)에 일부 기본 스타일을 정의 할 수 있도록합니다.
 

맞춤 요소에는 기본적으로`display` 속성이`inline`으로 설정되어 있습니다.
 여기서는 기본값을`grid`로 덮어 씁니다.
 하지만 CSS 특이성 규칙 때문에 컴포넌트에 `hidden`속성이있는 경우를 처리해야합니다.
 

같은 방식으로 투영 된 높이의 계산이 이제 구성 요소 내부의 일부입니다.
 이전과 마찬가지로 구성 요소에는 반응 속성 / 속성이 있으므로 정의 된 도메인 범위가 변경 될 때마다 막대 비율도 변경됩니다.
 

이제 두 개의 웹 구성 요소를 결합하여 HTML로 막대 차트를 만들 수 있습니다.
 광범위하게 사용자 정의 할 수 있지만 소비자는 더 이상 막대의 높이 계산이나 렌더링을 처리 할 부담이 없습니다.
 

두 구성 요소간에 암시 적 계약이 있음을 알 수 있습니다.`app-bar`의`size` 속성은`app-bar-chart` 구성 요소에 의해 관리됩니다.
 

기술적으로 소비자는 css 변수`--bar-size` (캡슐화 누수)를 방해하는 동작을 깨뜨릴 수 있지만이 절충안은 동시에 우리에게 큰 유연성을 제공합니다.
 

```xml
<app-bar-chart>
    <app-bar value="7"></app-bar>
    <app-bar value="2.5"></app-bar>
    <app-bar value="3.3"></app-bar>
    <app-bar value="2.2"></app-bar>
    <app-bar value="4"></app-bar>
    <app-bar value="8.3"></app-bar>
    <app-bar value="3.1"></app-bar>
    <app-bar value="7.6"></app-bar>
 <app-bar-chart>
```

다음 코드 펜 (Stackblitz)에서 철근 방향을 정의 할 수있는 고급 예제를 찾을 수 있습니다.
 

## 막대 차트 축 정의
 

지금까지이 구성 요소를 통해 독자는 범주의 상대적 비율을 빠르게 파악할 수 있습니다.
 

그러나 축이 없으면 이러한 비율을 절대 값에 매핑하고 지정된 막대에 레이블 또는 범주를 지정하는 것은 여전히 어렵습니다.
 

카테고리 축
앞에서 바의 위치는 그다지 의미가 없으며 공간을 균등하게 차지하면된다고 설명했습니다.
 카테고리 레이블은 동일한 논리를 따릅니다.
 

먼저 바 영역의 템플릿을 변경하여 축에 슬롯을 추가하고 레이아웃을 일관되게 유지하기 위해 스타일을 추가해야합니다.
 CSS`grid`는 다음을 쉽게 만듭니다.
 

```xml
// bar-chart.js
template.innerHTML = `
<style>
<!-- ...  -->

:host{
    /* ... */
    grid-template-areas:
    "bar-area"
    "axis-bottom";
    grid-template-rows: 1fr auto;
    grid-template-columns: auto 1fr;
}

#bar-area{
    /* ... */
    grid-area: bar-area;
}

#axis-bottom{
    display: flex;
    grid-area: axis-bottom;
}

</style>
<div id="bar-area">
    <slot name="bar-area"></slot>
</div>
<div id="axis-bottom">
    <slot name="axis-bottom"></slot>
</div>

```

이제 막대 차트에는 두 개의 고유 한 명명 된 슬롯이 있습니다.
 그런 다음 자식 요소가 삽입 될 슬롯을 지정해야합니다. 막대의 경우 `bar-area`섹션에 삽입합니다.
 값이 `bar-area`인 막대에 `slot`속성을 추가합니다.
 

이 동작을 bar 컴포넌트에 기본값으로 추가합니다.
 

```js
// bar.js
export class Bar extends HTMLElement {
    /* ... */
    connectedCallback() {
        if (!this.hasAttribute('slot')) {
            this.setAttribute('slot', 'bar-area');
        }
    }
}
```

`connectedCallback` 내에 앞서 언급 한 속성을 조건부로 추가합니다.
 기본 속성을 사용하는 경우 소비자가 구성 요소를 사용하거나 확장하는 방법을 모르기 때문에 사용자 지정 속성 (따라서 조건)에 우선 순위를 부여하는 것이 좋습니다.
 

이제 범주 축과 레이블 구성 요소를 만들어 보겠습니다.이 구성 요소는 레이아웃을 적용하는 기본 스타일을 사용하는 간단한 논리없는 구성 요소 쌍입니다.
 

```js
// label.js
const template = document.createElement('template');

/// language=css
const style = `
:host{
    display:flex;
}

:host([hidden]){
    display:none;
}

#label-text{
    flex-grow: 1;
    text-align: center;
}

:host(:last-child) #tick-after{
    display: none;
}

:host(:first-child) #tick-before{
    display: none;
}
`;

template.innerHTML = `
<style>${style}</style>
<div part="tick" id="tick-before"></div>
<div id="label-text"><slot></slot></div>
<div part="tick" id="tick-after"></div>
`;

export class Label extends HTMLElement {
    constructor() {
        super();
        this.attachShadow({mode: 'open'});
        this.shadowRoot.appendChild(template.content.cloneNode(true));
    }
}

customElements.define('app-label', Label);

// category-axis.js
const template = document.createElement('template');

/// language=css
const style = `
:host{
    display:flex;
    border-top: 1px solid gray;
}

:host([hidden]){
    display:none;
}

::slotted(app-label){
    flex-grow:1;
}

app-label::part(tick){
    width: 1px;
    height: 5px;
    background: gray;
}
`;

template.innerHTML = `
<style>${style}</style>
<slot></slot>
`;

export class CategoryAxis extends HTMLElement {
    constructor() {
        super();
        this.attachShadow({mode: 'open'});
        this.shadowRoot.appendChild(template.content.cloneNode(true));
    }

    connectedCallback() {
        if (!this.hasAttribute('slot')) {
            this.setAttribute('slot', 'axis-bottom');
        }
    }
}

customElements.define('app-category-axis', CategoryAxis);
```

이제 이러한 구성 요소를 HTML 문서에 추가 할 수 있습니다.
 

```xml
<app-bar-chart domainmin="0" domainmax="10">
    <app-bar value="2.5"></app-bar>
    <app-bar value="3.3"></app-bar>
    <app-bar value="8.3"></app-bar>
    <app-bar value="3.1"></app-bar>
    <app-bar value="7.6"></app-bar>
    <app-category-axis>
        <app-label>
            <!-- custom template if you want -->
            <span>cat-1</span>
        </app-label>
        <app-label>cat-2</app-label>
        <app-label>cat-3</app-label>
        <app-label>cat-4</app-label>
        <app-label>cat-5</app-label>
    </app-category-axis>
</app-bar-chart>
```

여기에는 한 점을 제외하고는 새로운 것이 없습니다. 라벨 템플릿에는`part` 속성이있는 두 개의 요소가 있습니다.
 이렇게하면 일반적으로 구성 요소 외부에서 액세스 할 수 없지만 Shadow DOM의 특정 부분을 사용자 지정할 수 있습니다.
 

다음 코드 펜 (Stackblitz)에서 실제로 작동하는 것을 볼 수 있습니다.
 

선형 스케일 축
선형 축의 경우 지금까지 본 기술을 대부분 혼합하여 사용하지만 새로운 개념 인 사용자 지정 이벤트도 소개합니다.
 

이전에 막대 차트 구성 요소에 대해했던 것처럼 선형 축 구성 요소는 도메인 범위 값과 두 연속 틱 사이의 간격을 정의하는 선언적 API를 노출합니다.
 

실제로이 구성 요소가 도메인 범위를 제어하도록하는 것이 이치에 맞지만 동시에 막대와 축 사이에 커플 링을 추가하고 싶지 않습니다.
 

대신, 축이 도메인 변경을 볼 때마다 막대를 다시 렌더링하도록 막대 차트에 알릴 수 있도록 상위 막대 차트 구성 요소를 이들 사이의 중재자로 사용합니다.
 

사용자 지정 이벤트로이 패턴을 얻을 수 있습니다.
 

```java
// linear-axis.js

// ...

export class LinearAxis extends HTMLElement {

   static get observedAttributes() {
      return ['domainmin', 'domainmax', 'gap'];
   }

   // ...

   attributeChangedCallback() {
      const {domainMin, domainMax, gap} = this;
      if (domainMin !== void 0 && domainMax !== void 0 && gap) {
         this.update();
         this.dispatchEvent(new CustomEvent('domain', {
            bubbles: true,
            composed:true,
            detail: {
               domainMax,
               domainMin,
               gap
            }
         }));
      }
   }
}
```

업데이트를 요청하는 것 외에도 구성 요소는 도메인 값 세부 정보를 전달하는 CustomEvent를 내 보냅니다.
 이벤트가 트리 계층 구조에서 올라가고 그림자 트리 경계를 벗어날 수 있도록 두 개의 플래그 `bubbles`및 `composed`를 전달합니다.
 

그런 다음 막대 차트 구성 요소에서 :
 

```java
// bar-chart.js

// ...

class BarChar extends HTMLElement {

   // ... 

   connectedCallback() {
      this.addEventListener('domain', ev => {
         const {detail} = ev;
         const {domainMin, domainMax} = detail;
         // the setters will trigger the update of the bars
         this.domainMin = domainMin;  
         this.domainMax = domainMax;
         ev.stopPropagation();
      });
   }

}
```

이전과 같이 속성 setter를 사용하여 막대에 대한 업데이트 호출을 사용자 지정 이벤트에 등록하기 만하면됩니다.
 이 경우 이벤트를 중재자 패턴 구현에만 사용하기 때문에 이벤트 전파를 중지하기로 결정했습니다.
 

평소처럼 세부 사항에 관심이 있다면 codepen 또는 stackblitz를 살펴볼 수 있습니다.
 

## 결론
 

이제 선언적인 방식으로 막대 차트를 작성하기위한 모든 기본 구성 요소가 있습니다.
 그러나 코드를 작성할 때 데이터를 사용할 수있는 경우가 많지 않고 나중에 동적으로로드됩니다.
 이것은별로 중요하지 않습니다. 핵심은 데이터를 해당 DOM 트리로 변환하는 것입니다.
 

React, Vue.js 등의 라이브러리를 사용하면 매우 간단하게 진행할 수 있습니다.
 웹 구성 요소를 웹 응용 프로그램에 통합하는 것은 무엇보다 일반적인 HTML 요소이므로 간단합니다.
 

웹 구성 요소 사용의 또 다른 이점은 차트를 사용자 지정하고 적은 양의 코드로 다양한 사용 사례를 처리 할 수 있다는 것입니다.
 

차트 라이브러리는 일반적으로 방대하고 유연성을 제공하기 위해 많은 구성을 노출해야하지만 웹 구성 요소를 사용하면 약간의 CSS 및 Javascript를 사용하여 막대 차트 라이브러리를 만들 수 있습니다.
 

읽어 주셔서 감사합니다!
 verified_user