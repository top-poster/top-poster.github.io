---
layout: post
title: "CSS 참조 가이드: '텍스트 장식'"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/lr-css-reference-guide-text-decoration-nocdn.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/lr-css-reference-guide-text-decoration-nocdn.png?fit=730%2C487&ssl=1)

CSS `text-decoration` 속기 속성을 사용하면 텍스트에 장식 라인을 추가하고 스타일을 지정할 수 있습니다.

#### 앞으로 이동:

- 데모
- 구성 요소 속성
텍스트 장식 줄
텍스트 장식 스타일
텍스트 데코레이션 컬러
텍스트 데코레이션
- 텍스트 장식 줄
- 텍스트 장식 스타일
- 텍스트 데코레이션 컬러
- 텍스트 데코레이션

속성의 구문은 다음과 같습니다.

```undefined
text-decoration: line | style | color | thickness
```

## 데모

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="450" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/kaperskyguru/embed/MWeyQzp?height=450&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=kaperskyguru&amp;slug-hash=MWeyQzp&amp;pen-title=CSS%20Text%20Decoration%20Example&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="CSS Text Decoration Example" loading="lazy" id="cp_embed_MWeyQzp"></iframe></div>

## 구성 요소 속성

CSS 텍스트 데코레이션(text-decoration) 속기는 네 가지 구성 요소 속성으로 구성된다. 다음 예를 고려하십시오.

```cpp
p {
  text-decoration: underline solid red 0.1em;
}

// explodes to:

p {
  text-decoration-line: underline;
  text-decoration-style: solid;
  text-decoration-color: red;
  text-decoration-thickness: 0.1em
}
```

아래 각 구성 요소 속성을 자세히 살펴봅니다.

### 텍스트 장식 줄

텍스트 장식의 줄 스타일을 설정합니다.

#### 가치

- ➡: 텍스트 아래에 선을 표시합니다.
- 라인스루: 텍스트에 선을 긋는다.
- `개요`: 텍스트 위에 선을 표시합니다.
- ➡: 텍스트가 계속 표시되는 텍스트와 보이지 않는 텍스트를 번갈아 표시합니다. 즉, 깜박입니다. 이 값은 CSS 애니메이션을 위해 더 이상 사용되지 않습니다.
- `없음`: 라인 생성 안 함

#### 사용법

```undefined
p {
  text-decoration-line: underline | line-through | overline | blink | none
}
```

### 텍스트 장식 스타일

텍스트에 표시할 줄의 스타일을 설정합니다.

#### 가치

- solid: 실선을 그립니다.
- `double`double`: 이중 선을 그립니다.
- 점: 점선을 그립니다.
- `dashed`: 파선을 그립니다.
- 웨이브: 웨이브 라인을 그립니다.

#### 사용법

```undefined
p { 
  text-decoration-style: solid | double | dotted | dashed | wavy;
}
```

### 텍스트 데코레이션 컬러

텍스트에 표시할 선의 색상을 설정합니다.

#### 가치

모든 CSS 색상과 마찬가지로, 값은 키워드, 16진수 코드, RGB/RGBA 값 또는 HSL/HSLA 값으로 제공할 수 있다.

#### 사용법

```undefined
p {
  text-decoration-color: currentcolor | red | #00ff00 | rgba(255, 128, 128, 0.5) | transparent;
}
```

### 텍스트 데코레이션

텍스트에 그려진 선의 스트로크 두께를 설정합니다.

> N.B.는 현재 파이어폭스와 사파리 최신 버전에서만 지원되고 있다.

#### 가치

- `auto`: 브라우저가 텍스트 장식 라인에 적합한 너비를 선택할 수 있습니다.
- `from-font`: 텍스트의 글꼴 파일로 선의 두께를 결정할 수 있습니다. 글꼴 파일에 두께 정보가 제공되지 않는 경우 기본값은 `자동`입니다.
- `길이`: 개발자가 임의의 길이 값(예: `1px`, `2em` 등)을 지정할 수 있습니다.
- `백분율`: 개발자가 선 두께에 대한 백분율 값을 지정할 수 있습니다.

#### 사용법

```undefined
p {
  text-decoration-thickness: auto | from-font | 0.2em | 3px | 10%;
}
```