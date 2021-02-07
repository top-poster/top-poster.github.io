---
layout: post
title: "존재하는지 몰랐던 5개의 CSS 유사 요소"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/css-hacks.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/css-hacks.png?fit=730%2C487&ssl=1)

이 게시물에서는 여러분이 알지 못하거나 이전에 사용하지 않았던 유사 요소에 대해 여러분의 관심을 끌겠습니다. 주요 목표는 당신이 CSS로 쉽게 얻을 수 있는 어떤 것에 대한 불필요한 자바스크립트 코드 라인을 쓰는 것을 피하는 것이다.

## 의사 요소란 무엇인가?

MDN에 따르면 CSS 의사 요소는 선택한 요소의 특정 부분을 스타일링할 수 있는 선택기에 추가된 키워드입니다. 예를 들어 `::first-line`을 사용하여 문단의 첫 줄에 스타일을 적용할 수 있습니다.

다음은 의사 요소의 일반 구문입니다.

```css
selector::pseudo-element {  
    property:  value;  
    }
```

위의 설명과 함께, 우리는 이제 우리의 주제로 돌아갈 수 있습니다. 저는 여러분이 존재하는지 전혀 몰랐던 사이비 요소들을 나열하고 여러분의 관심을 끌 것입니다.

## 1. ':첫글자' 유사 요소

:`:첫 글자의 유사 요소는 행에 다른 내용(이미지 또는 인라인 테이블 등)이 선행되지 않는 경우 텍스트의 첫 번째 글자에 특수 스타일을 추가하는 데 사용됩니다. 이것은 블로그 작성자들이 사용하는 매우 일반적인 스타일이다. 블록 레벨 요소에만 적용할 수 있습니다.

예를 들어, 다른 문자와 다른 색과 크기를 가지도록 문서의 첫 번째 문자를 스타일링한다고 가정합니다. 이것은 특정 요소의 첫 문자를 얻기 위해 자바스크립트를 사용하는 것보다 `::first-letter` 의사 요소를 사용하는 것이 더 쉬울 것이다.

```css
p:first-letter{
       font-size: 300%;
       color: red;
       float: left;
     }
```

다음 질문으로 떠오를 수 있는 것은, `:첫 번째 문자` 유사 요소를 사용하여 어떤 속성을 스타일링할 수 있는가 하는 것입니다.

다음 속성은 `:first-letter` 의사 요소에 적용됩니다.

- 글꼴 속성
- 배경 속성
- ➡➡의 성질.
- ➡➡의 성질.
- 국경의 재산.
- 텍스트 장식
- ➡-align (➡이 ➡인 경우에만 ➡-align)
- `문자 표시`
- `선 높이`
- 색채
- ➡➡➡➡➡➡➡➡➡➡ ➡
- 등등의

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/olawanlejoel/embed/BaKXjeY?height=500&amp;theme-id=dark&amp;default-tab=html%2Cresult&amp;user=olawanlejoel&amp;slug-hash=BaKXjeY&amp;pen-title=First-letter%20Pseudo-element&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="First-letter Pseudo-element" loading="lazy" id="cp_embed_BaKXjeY"></iframe></div>

## 2. ':일선' 유사 요소

첫 번째 줄 유사 요소는 텍스트 또는 블록 수준 요소의 첫 줄에 특수 스타일을 추가하는 데 사용됩니다. 첫 번째 줄의 길이는 요소의 너비, 문서의 너비, 텍스트의 글꼴 크기 등 매우 많은 요인에 의해 결정됩니다.

예를 들어, 뉴스 블로그 기사의 첫 줄을 스타일링하려면 자바스크립트를 사용하여 첫 줄을 얻는 것보다 `::first-line` 의사 요소를 사용하는 것이 더 쉬울 것이다.

```css
p::first-line { 
     background-color: #fff;
     color: #000; 
     word-spacing: 10px;
    }
```

다음 속성은 `:first-line` 의사 요소에 적용됩니다.

- 글꼴 속성
- 배경 속성
- 말씨름
- `편지 발송인
- 텍스트 장식
- 얼라인먼트
- `문자 표시`
- `선 높이`
- 색채
- `문자 표시`
- 등등의

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_2" scrolling="no" src="https://codepen.io/olawanlejoel/embed/ExKqPBx?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=olawanlejoel&amp;slug-hash=ExKqPBx&amp;pen-title=First-line%20Pseudo-element&amp;name=cp_embed_2" style="width: 100%; overflow:hidden; display:block;" title="First-line Pseudo-element" loading="lazy" id="cp_embed_ExKqPBx"></iframe></div>

## 3. ':선택' 유사 요소

`::selection` 의사 요소는 문서에서 강조 표시된 부분 또는 사용자가 작성한 요소입니다. 사용자가 강조 표시한 문서 부분에 스타일을 적용하는 데 사용됩니다(예: 사용자가 마우스를 클릭하여 텍스트 위로 끌 때).

기본 텍스트 선택 배경색은 파란색이며, 이 속성은 기본 색을 변경하는 데 사용됩니다.

구문:

```undefined
::selection { 
        css declarations; 
    }
```

예를 들어:

배경색을 기본 색상(파란색)에서 녹색으로 변경한다고 가정합니다. 이것은 `:선택` 유사 요소를 사용하여 쉽게 달성할 수 있다.

```css
::selection {  
    color:  red;  
    background:  yellow;  
    }
```

위의 스타일링은 일반적으로 해당 페이지에 적용되지만 개별 요소 또는 CSS 선택기를 대상으로 결정할 수도 있습니다.

```css
p::selection {  
 color:  red;  
 background:  yellow;  
 }
```

다음 세 가지 속성 중 `:selection`을 사용할 수 있는 속성은 다음과 같습니다.

- 색채
- background 속성(background-color, background-image 등)
- `문자 표시`

## 4. ':backdrop' 사이비 요소

`::backdrop` CSS 의사 요소는 전체 화면 모드에서 표시되는 모든 요소 바로 아래에 렌더링되는 뷰포트의 상자 크기입니다. 이것은 전체 화면 API의 일부이며 예술적이거나 창조적인 것을 원한다면 몇 가지 스와그 포인트를 얻을 수 있습니다.

::backdrop 의사 요소는 상위 계층에서 맨 위에 있을 때 요소 아래에 있는 모든 것을 모호하게 하거나 스타일링하거나 완전히 숨길 수 있게 한다.

브라우저에서 전체 화면 모드로 들어갈 때마다 대부분의 브라우저는 검은색 배경(배경)을 표시하는 경향이 있습니다. `:backdrop` 유사 요소를 사용하면 검은색 배경을 원하는 색상으로 변경할 수 있습니다!

구문:

```undefined
::backdrop{ 
   css declarations; 
  }
```

예를 들어, 비디오를 전체 화면 모드로 전환할 때 사용되는 스타일을 대부분의 브라우저에서 기본값으로 사용하는 검정색이 아닌 회색-파란색으로 변경하려면 다음을 수행합니다.

```css
video::backdrop {
  background-color: #448;
}
```

MDN의 배경 의사 요소에 대해 자세히 알아보십시오. 이 링크에서 예를 확인할 수도 있습니다.

## 5. ': 자리 표시자' 유사 요소

`::placeholder` 의사 요소는 자리 표시자 텍스트가 있는 폼 요소를 선택하고 자리 표시자 텍스트를 스타일링할 수 있습니다.

> 참고: 대부분의 브라우저에서 자리 표시자 텍스트의 기본 색상은 연한 회색입니다.

예를 들어, 제 웹사이트가 핑크와 화이트의 두 가지 색상만 사용한다고 가정해 보겠습니다. 자리 표시자 텍스트를 분홍색으로 하려면 `:: 자리 표시자` 유사 요소를 사용하여 쉽게 얻을 수 있습니다.

```xml
<!-- HTML -->
< input  type="text"  placeholder="Enter your full name">

/* CSS */
::placeholder {  
color:  pink;  
}
```

위의 코드는 이 스타일링에 연결된 모든 자리 표시자 텍스트의 색상을 변경합니다. 하지만 특정 필드나 선택된 필드에만 적용되도록 하려면 어떻게 해야 할까요? 이 경우 선택기를 추가해야 합니다.

```xml
<!-- HTML -->
< input  type="text" class="name"  placeholder="Enter your name">

/* CSS */
.name::placeholder {  
color:  pink;  
}
```

우리는 실제로 텍스트를 스타일링하기 때문에 여러분이 사용할 주요 CSS 속성은 컬러, 글꼴 크기, 글꼴 가중치 등과 같은 텍스트 스타일링과 관련된 것입니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="450" width="100%" name="cp_embed_3" scrolling="no" src="https://codepen.io/olawanlejoel/embed/MWyNKMx?height=450&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=olawanlejoel&amp;slug-hash=MWyNKMx&amp;pen-title=Placeholder%20Pseudo-element&amp;name=cp_embed_3" style="width: 100%; overflow:hidden; display:block;" title="Placeholder Pseudo-element" loading="lazy" id="cp_embed_MWyNKMx"></iframe></div>

## 결론

이는 많은 개발자들이 사용하는 것을 잊거나 심지어 알지 못하는 5가지 유사 요소이다. 이 게시물이 명확한 예를 들어 그들을 제대로 설명할 수 있었기를 바랍니다.

읽어주셔서 감사합니다!