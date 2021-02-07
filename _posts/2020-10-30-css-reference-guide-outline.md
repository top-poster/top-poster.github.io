---
layout: post
title: "CSS 참조 가이드: '개요'"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/lr-css-reference-guide-outline-nocdn.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/lr-css-reference-guide-outline-nocdn.png?fit=730%2C487&ssl=1)

CSS "개요" 속기 속성은 요소의 외부 주위에 선을 그리는 데 사용됩니다. 링크나 다른 요소에 더 중점을 두는 것은 특히 `a:focus` 선택기와 함께 유용하다.

#### 앞으로 이동:

- 데모
- 구문
- 구성 요소 속성
엷은 빛깔
`스파이브 스타일`
`폭폭`
- 엷은 빛깔
- `스파이브 스타일`
- `폭폭`

처음에는 개요가 국경과 비슷해 보일 수 있다. 그러나 다른 점은 `개요`가 전체 요소 주위에 선을 그어 선을 그릴 면을 지정할 수 없다는 점입니다. 또 경계선과는 달리 외곽에 선을 긋기 때문에 윤곽선은 상자 모델의 일부가 아니다.

## 데모

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/kaperskyguru/embed/LYZNevy?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=kaperskyguru&amp;slug-hash=LYZNevy&amp;pen-title=CSS%20Outline%20Example&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="CSS Outline Example" loading="lazy" id="cp_embed_LYZNevy"></iframe></div>

## 구문

```undefined
outline: color | style | width
```

개요 속기 속성은 1, 2 또는 3 값으로 선언할 수 있다. 예를 들어:

```undefined
outline: solid; // This specifies only the STYLE value

outline: #eee dashed; // This specifies the COLOR value and STYLE value

outline: inset thick; // This specifies the STYLE value and WIDTH value

outline: green solid 2px; // This specifies all 3 values
```

하나 또는 두 개의 값만 지정하면 다른 값이 기본값으로 확인됩니다. 개요는 작업하기 위해 `스타일` 값만 설정하면 됩니다.

## 구성 요소 속성

아웃라인 속성은 아웃라인의 색상, 너비 및 스타일을 정의하기 위해 세 개의 개별 속성으로 구성된 단축키입니다. 아래 각 항목을 살펴봅니다.

### 엷은 빛깔

아웃라인의 텍스트 부분과 장식 부분의 색상을 설정합니다. 키워드, 16진수 값, RGB/RGBA 값, HSL/HSLA 값을 통해 지정할 수 있다.

브라우저가 지원하는 경우 기본값은 `반전`이고, 그렇지 않은 경우 기본값은 `current Color`입니다.

```undefined
outline-color: currentColor | red | #eee | rgb(255, 255, 255) | hsl(0,0,0)
```

### '스파이브 스타일'

그릴 선의 유형을 지정합니다. 값은 다음 키워드 중 하나가 될 수 있습니다.

- auto
- ➡➡➡➡➡➡➡➡➡➡ ➡
- ➡➡➡➡➡➡➡➡➡➡ ➡
- 실속 있는
- 이중으로
- ➡➡➡➡➡➡➡➡➡➡ ➡
- 릿지
- inset
- ➡➡➡➡➡➡➡➡➡➡ ➡
- 없음(개요 없음)

기본값은 `없음`입니다.

```undefined
outline-style: auto | dotted | dashed | solid | double | groove | ridge | inset | outset
```

### '폭폭'

그릴 선의 두께를 지정합니다. 이 값은 길이 값 또는 다음 키워드 중 하나가 될 수 있습니다.

- 얇다
- 중형
- ➡➡➡➡➡➡➡➡➡➡ ➡

기본값은 `medium`입니다.

```undefined
outline-width: 2(px, em, rem) | thin | medium | thick
```