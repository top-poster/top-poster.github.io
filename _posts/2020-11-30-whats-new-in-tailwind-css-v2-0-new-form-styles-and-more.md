---
layout: post
title: "Tailwind CSS v2.0의 새로운 기능: 새로운 양식 스타일 등"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/whats-new-tailwind-css-v2-0.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/whats-new-tailwind-css-v2-0.png?fit=730%2C487&ssl=1)

## 도입

공식 사이트에 따르면, Tailwind CSS는 "클래스로 포장된 유틸리티 우선 CSS 프레임워크로서 마크업에서 직접 어떤 디자인이라도 구축하도록 구성될 수 있다"고 한다. 이것은 낮은 수준이고, 기본적으로 무엇을 해야 하는지 알려주는 다른 CSS 프레임워크와 비교했을 때, 그것은 주장되지 않습니다. 이렇게 하면 매우 최소의 CSS로 선언적인 방식으로 UI 요소를 구성할 수 있습니다.

이 게시물은 Tailwind CSS 2.0의 몇 가지 새로운 기능과 이를 프로젝트에서 사용하는 방법을 다룹니다. 또한 몇 가지 변경 사항 및 마지막으로 버전 2.0으로의 마이그레이션 경로도 다룹니다. 시작하겠습니다!

## Tailwind CSS v2.0의 새로운 기능 및 변경 사항

### 다크 모드

최근에는 다크 모드가 매우 바람직한 기능이 되었다. Tailwind 앱에서 다크 모드 스타일을 추가하려면 다음과 같은 `dark:dark:` 접두사 클래스:

```undefined
<div class="bg-white dark:bg-gray-800">
  ...
</div>
```

다크 모드는 최종 번들 크기에 미치는 영향 때문에 기본적으로 사용할 수 없습니다. 다크 모드를 활성화하려면 `tailwind.config.css` 파일에 다음을 추가하십시오.

```java
// tailwind.config.js
module.exports = {
  darkMode: 'media',
  // ...
}
```

후드 아래에서는 선호 색상 체계 미디어 기능을 사용하여 어두운 변형을 전환한다. 따라서 사용자의 운영 체제에서 다크 모드가 활성화되면 `다크` 변형이 고정되지 않은 클래스보다 우선합니다.

운영 체제 기본 설정에 의존하지 않고 수동으로 다크 모드를 전환하는 방법도 있습니다. Tailwind 구성 파일에서 `media`를 `class`로 바꿉니다.

```java
// tailwind.config.js
module.exports = {
  darkMode: 'class',
  // ...
}
```

다크 모드 클래스는 클래스 `다크`가 루트 `<` 태그에 있을 때 적용됩니다.

```xml
<!-- Dark mode enabled -->
<html class="dark">
<body>
  <!-- Will be black -->
  <div class="bg-white dark:bg-black">
    <!-- ... -->
  </div>
</body>
</html>
```

### 새로운 색상 팔레트

테일윈드 CSS 2.0은 이전 버전의 90개 값보다 훨씬 많은 총 220개의 값을 가진 완전히 새로운 색상 팔레트를 제공한다. 각 색상의 색조는 현재 50~900가지이다. 이 팔레트는 또한 "블루 그레이"에서 "따뜻한 그레이"에 이르는 다섯 가지 회색 음영을 포함하고 있습니다.

모든 색상에 50이라는 초경량 차양이 새로 추가되었으며 다음과 같이 사용할 수 있습니다.

```undefined
<div class="bg-gray-50">This div has a gray background.</div>
```

### IE11 지원 중단

v2 릴리즈와 함께 Tailwind 팀은 IE11에 대한 지원을 중단하기로 결정했다. 이렇게 하면 최종 빌드에 폴리 필링이 번들로 제공되지 않기 때문에 빌드 크기가 작아집니다. 더 큰 가능성을 가능하게 하는 CSS 사용자 지정 속성 같은 새로운 기능으로 포커스가 이동될 것이다.

IE11에 대한 지원이 여전히 필요한 경우 Tailwind CSS v1.9를 계속 사용할 수 있습니다. 이 문서에 언급된 새로운 기능은 제공하지 않지만 그래도 좋습니다.

### 새 양식 스타일 및 '사용자 정의 양식' 플러그인

폼 요소는 이전 버전의 Tailwind CSS와 함께 매우 민낯으로 보였기 때문에 다소 점잖게 보이도록 많은 사용자 지정 스타일링이 작성되어야 했다.

v2.0 릴리스와 함께 @tailwindcs/custom-forms라는 새로운 공식 플러그인이 출시되었다. 폼 컨트롤을 내장 유틸리티 클래스로 쉽게 스타일링할 수 있는 상태로 재설정합니다.

```xml
<!-- This will be a nice rounded checkbox with an indigo focus ring and an indigo checked state -->
<input
  type="checkbox"
  class="h-4 w-4 rounded border-gray-300 focus:border-indigo-300 focus:ring-2 focus:ring-indigo-200 focus:ring-opacity-50 text-indigo-500"
/>
```

기본적으로 포함되어 있지 않으므로 사용하려면 `plugins` 섹션의 `tailwind.config.js` 파일에 명시적으로 추가해야 합니다.

```java
// tailwind.config.js
module.exports = {
  // ...
  plugins: [require('@tailwindcss/forms')],
}
```

자세한 내용은 설명서를 참조하십시오.

### 새로운 아웃라인 링 유틸리티

테일윈드 팀은 요소에 상자 그림자를 만드는 데 사용할 수 있는 새로운 아웃라인 링 유틸리티를 추가했다. 이 기능은 버튼의 호버/포커스 상태를 표시하는 데 특히 유용합니다.

```undefined
<button class="... focus:ring-4 focus:ring-green-500 focus:ring-opacity-50">
  Button
</button>
```

링 유틸리티는 테두리가 아닌 상자 그림자를 사용하여 레이아웃 리플로우를 유발하고 사용자 환경을 왜곡시킨다.

### 확장된 간격, 타이포그래피 및 불투명도 척도

간격 유틸리티는 부분 값(`0.5`, `1.5` 등)을 포함하도록 확장되었습니다.

```undefined
<span class="ml-3.5">Just a little nudge.</span>
```

또한 더 높은 간격의 유틸리티도 확장되었습니다.

```undefined
<div class="p-96">This is too much padding.</div>
```

7xl, 8xl, 9xl 등 타이포그래피 스케일이 확대됐다.

```js
<h1 class="text-9xl">What is this, an Apple website?</h1>
```

불투명도 척도가 `0`에서 `100` 단계로 확장되었습니다.

```js
<figure class="opacity-20">
  <p>Can you see me?</p>
</figure>
```

### 새로운 '텍스트 오버플로' 유틸리티

오버플로우-엘리피시스(overflow-ellipsis)와 오버플로-클립(overflow-clip) 등 텍스트-오버플로우(text-overflow) 특성을 제어하는 새로운 오버플로 유틸리티가 추가됐다. 이렇게 하면 잘린 텍스트에 타원을 추가하거나 텍스트를 클리핑할 수 있습니다.

```undefined
<p class="overflow-ellipsis overflow-hidden">
  Look ma no whitespace-nowrap ipsum...
</p>
```

Tailwind CSS 2.0의 전체 변경 목록은 릴리스 노트를 참조하십시오.

## Tailwind CSS v2.0으로 마이그레이션

새 버전에 몇 가지 변경 사항(일부 변경 사항 포함)이 새로 변경되었으므로 마이그레이션 중에 몇 가지 단계를 수행해야 합니다. 팀에 따르면, 대부분의 프로젝트는 마이그레이션하는 데 30분도 걸리지 않을 것이라고 합니다. 몇 가지 주의해야 할 사항과 함께 Tailwind CSS를 업그레이드하는 방법에 대해 알아보겠습니다.

### Tailwind CSS v2.0 및 PostCS 8 설치

새 버전은 PostCSS 및 Autopfixer를 피어 종속성으로 요구하므로 Tailwind와 나란히 설치해야 합니다.

```coffeescript
npm install tailwindcss@latest postcss@latest autoprefixer@latest
```

문제가 발생하면 PostCSS 7 호환성 빌드를 실행해야 할 수도 있습니다.

### Node.js 12.13 이상으로 업그레이드

Tailwind CSS v2.0을 구축하려면 노드 §12.13이 필요합니다. 하위 버전의 노드를 실행 중인 경우 노드를 업그레이드해야 합니다.

그 밖의 몇 가지 변경 사항은 다음과 같습니다.

- `@tailwindcs/typography` 및 `@tailwindcs/custom-forms`(@tailwindcs/forms로)와 같은 타이포그래피 및 폼 플러그인 업데이트
- 향후 및 실험 구성 옵션 제거
module.dister = {
// 더 이상 필요하지 않습니다.
미래: {}
기본 선높이: 참,
정리 계층:기본값: true,
감산된 Gap Utilities 제거: true,
},
실험: {}
추가 중단점: 참,
extendedFontSizeScale: true,
확장 SpacingScale: true,
},
// ...
}
- 변경된 유틸리티 클래스 업데이트
- shadow-outline 및 shadow-xs를 링 유틸리티로 대체
- 색상표를 명시적으로 구성합니다.

업그레이드 가이드에 대한 자세한 내용은 Tailwind CSS 설명서를 참조하십시오.

## 결론

이 게시물에서, 우리는 Tailwind CSS v2.0에 도입된 새로운 변경사항의 일부를 다루었다. 또한 기존 애플리케이션을 새 버전으로 마이그레이션하는 방법과 특정 프로젝트에 영향을 미칠 수 있는 몇 가지 변경 사항도 살펴보았습니다. 프로젝트가 더 이상 지원되지 않는 일부 기능에 종속되어 있는 경우 v1.9를 계속 사용할 수 있습니다.