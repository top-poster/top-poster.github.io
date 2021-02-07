---
layout: post
title: "반응에서 스벨트로 전환해야 합니까?"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/12/svelte-v-react-copy.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/svelte-v-react-copy.png?fit=730%2C487&ssl=1)

## 도입

리액트.js와 스벨트는 개발자 세계에서 흔히 볼 수 있는 두 개의 자바스크립트 프런트엔드 프레임워크이다. 두 도구 모두 개발자에게 서로 다른 웹 애플리케이션의 개발에 대한 생산적인 접근 방식을 제공합니다. 이 기사에서는 스벨트가 가져오는 강점과 그것이 왜 당신이 고려할 가치가 있는지 알아보겠습니다.

### 가상 DOM 대 컴파일러

React는 런타임 중에 응용 프로그램 코드를 해석하기 위해 가상 DOM을 사용합니다. 일정량의 오버헤드 코드가 번들로 제공되며, 이 코드는 브라우저의 JavaScript 엔진에서 실행되어 DOM을 모니터링하고 업데이트합니다. 가상 DOM은 모든 변경 사항을 감시하고 실제 DOM을 변경하기 위한 최선의 방법을 계산합니다.

스벨트는 프로덕션 응용 프로그램을 생성할 때 응용 프로그램을 JavaScript 코드로 변환하는 컴파일러입니다. 이렇게 하면 프로그램이 DOM에서 실행 중일 때 브라우저에 오버헤드 프레임워크 코드가 주입되지 않습니다. 빌드 시 Svelte가 실행되어 구성 요소를 DOM을 수술적으로 업데이트하는 매우 효율적인 명령 코드로 변환합니다. 이렇게 하면 고성능 코드를 작성할 수 있습니다.

### 사용의 용이성

가장 기본적인 애플리케이션을 구축하기 위해 JSX나 CSS-in-JS와 같은 것들을 배워야 할 때 리액트 학습은 부담스러울 수 있다.

리액트(React)와 비교했을 때, 스벨트는 플레인 자바스크립트, HTML, CSS가 주요 부분이기 때문에 이해하고 시작하는 것이 더 간단하다. HTML, CSS, 자바스크립트의 고전적인 웹 개발 모델에 가깝게 붙어 있다. 이것은 리액트보다 스벨트를 배우는 것을 훨씬 더 쉽게 만든다. HTML과 JavaScript에 대한 몇 가지 확장 기능만 제공하므로 React를 통해 배울 필요가 없는 개념을 더 적게 배울 수 있습니다.

이것은 `.svelte` 구성 요소의 예입니다.

기본 HTML, CSS 및 JavaScript만 제공됩니다.

```xml
<style>
  h1 {
    color: green;
  }
</style>

<script>
  let name = 'Nefe';
</script>

<h1>Hello, {name}!</h1>
```

### 더 나은 개발자 경험

코드가 적다고 해서 항상 더 나은 기능 코드를 의미하는 것은 아니지만, Svelte의 장점은 React에 비해 코드를 덜 써야 한다는 것입니다. 다음은 Svelte로 만든 기본 응용 프로그램의 예입니다.

```xml
<script>
  let a = 1;
  let b = 2;
</script>

<input type="number" bind:value={a}>
<input type="number" bind:value={b}>

<p>{a} + {b} = {a + b}</p>
```

아래는 React로 빌드된 동일한 응용 프로그램의 코드 조각입니다. 스벨트는 코드 9줄, 리액트는 코드 21줄이다. 그것은 중요한 차이점이다. 스벨트 앱의 아름다운 점은 그것이 결코 덜 기능적이라는 것이다.

```undefined
import React, { useState } from 'react';

export default () => {
  const [a, setA] = useState(1);
  const [b, setB] = useState(2);

  function handleChangeA(event) {
    setA(+event.target.value);
  }
  function handleChangeB(event) {
    setB(+event.target.value);
  }

  return (
    <div>
      <input type="number" value={a} onChange={handleChangeA}/>
      <input type="number" value={b} onChange={handleChangeB}/>
      <p>{a} + {b} = {a + b}</p>
    </div>
  );
};
```

.svlte 구성 요소를 자동으로 내보냅니다. Svelte를 사용하면 기본적으로 내보내지고 다른 구성 요소에서 가져올 준비가 되었으므로 내보낼 필요가 없습니다.

스벨트는 효과와 애니메이션이 내장되어 있습니다. 대응에서와 같이 타사 라이브러리를 사용하여 애니메이션을 만들 필요는 없습니다.

`svelte/motion`, `svelt/transition`, `svelte/animate`, `svelt/temporate`는 우리에게 강력한 모듈을 제공하여 놀라운 애니메이션을 만들어낸다.

스벨트를 사용하면 독특한 수업이나 스타일이 구성 요소에서 새어나오는 것에 대해 걱정할 필요가 없습니다. 스벨트에서 스타일은 스타일 태그의 구성요소 범위이며, 이를 통해 유연한 스타일링이 가능합니다.

이는 개발자인 귀하에게 덜 골치 아픈 일이라는 것을 의미하며, 여러분의 일을 더 쉽게 만들어줍니다. 또한 스타일 구성 요소 및 감정과 같은 스타일링 라이브러리를 사용하는 방법을 배울 필요가 없습니다. 따라서 기술 오버헤드가 적기 때문에 코드를 더 빨리 작성하고 전송할 수 있습니다.

## 성과

스벨트는 컴파일 단계에서 작동합니다. Svelte 앱의 프로덕션 빌드를 실행하면 Svelte는 고도로 최적화된 바닐라 JavaScript로 코드를 컴파일합니다. 이는 런타임에 프레임워크 코드가 추가되지 않음을 의미합니다. 이렇게 하면 브라우저의 작업량이 줄어들기 때문에 응용 프로그램의 전반적인 성능이 향상됩니다.

스벨트는 중간 관리자나 복잡한 조정 기술에 의존하지 않고 DOM을 수술적으로 업데이트합니다. 스벨트의 컴파일러는 변수의 변경 내용을 추적하고 그에 따라 HTML을 업데이트합니다.

코드를 살펴보고 변수에 따라 달라지는 성분을 확인하고 변수가 변경되면 해당 성분을 업데이트합니다. 이러한 방식으로 Svelte는 타사 API에 의존하지 않고도 사후 대응적입니다. 반면, this.state에 전화를 걸거나 use State Hook을 사용하여 React에게 수정 사항을 지켜보라고 지시해야 한다.

```xml
<script>
  let count = 0;
  $: doubled = count * 2;
  function handleClick() {
    count += 1;
  }
</script>

<button on:click="{handleClick}">Click me!</button>

<p>{count} doubled is {doubled}</p>
```

여기서 변수 `카운트`와 또 다른 변수 `더블`이 있습니다. α는 그 가치에 대한 카운트에 따라 달라집니다. React에서 "doubled"를 업데이트하면 "set doubled(count)"로 표시됩니다.

그러나 자바스크립트 식별자 $:는 svelte에게 곱셈은 count에 따라 달라지는 변수이며, svelte는 count가 바뀔 때마다 곱셈을 자동으로 업데이트한다는 것을 알고 있다. 반응성에 대한 좀 더 심층적인 정보는 Ovie Okeh의 기사를 참조하십시오.

스벨트의 `bundle.js` 파일은 리액트 파일보다 훨씬 작은 공간을 남긴다. 따라서 특히 저전력 장치나 CPU 집약적인 애플리케이션에서 성능 면에서 큰 차이를 보입니다. 스벨트는 또한 지퍼를 채웠을 때 더 작은 번들 크기를 가지고 있다.

## 결론

스벨트 생태계는 빠르게 발전하고 있다. 스벨트의 Next.js 버전인 Sapper와 네이티브 모바일 애플리케이션 구축을 위한 Svelte Native의 도입으로 스벨트는 프런트엔드 기술 분야에서 더 강력한 경쟁자로 성장하고 있다. 매우 빠른 Svelte 애플리케이션을 확인하고, 실험하고, 구축하십시오.