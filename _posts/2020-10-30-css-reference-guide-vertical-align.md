---
layout: post
title: "CSS 참조 가이드: '수직 정렬'"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/lr-css-reference-guide-vertical-align-nocdn.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/lr-css-reference-guide-vertical-align-nocdn.png?fit=730%2C487&ssl=1)

CSS `vertical-align` 속성은 인라인 블록으로 표시된 요소 또는 요소의 수직 정렬을 설정합니다. 수직 정렬 속성은 테이블 셀 요소에서도 작동합니다.

#### 앞으로 이동:

- 데모
- 가치
키워드
길이 및 백분율 값
전역 값
- 키워드
- 길이 및 백분율 값
- 전역 값

구문은 다음과 같다.

```undefined
element {
  vertical-align: baseline|top|bottom|middle|text-top|text-bottom|sub|super|length units;
}
```

## 데모

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="450" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/kaperskyguru/embed/BazKYEy?height=450&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=kaperskyguru&amp;slug-hash=BazKYEy&amp;pen-title=CSS%20Vertical%20Align%20Example&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="CSS Vertical Align Example" loading="lazy" id="cp_embed_BazKYEy"></iframe></div>

## 가치

### 키워드

#### ➡➡➡➡➡➡➡➡➡➡ ➡

이 값은 `수직 정렬`의 기본값입니다. 이름에서 알 수 있듯이 요소를 상위 요소의 기준선에 맞춥니다.

```undefined
img {
  vertical-align: baseline
}
```

#### 꼭대기

요소를 선에서 가장 높은 요소의 높이에 맞춥니다.

```undefined
img {
  vertical-align: top
}
```

#### 밑바닥

요소를 아래 가장 긴 요소와 동일한 수준으로 맨 아래로 정렬합니다.

```undefined
img {
  vertical-align: bottom
}
```

#### 중간의

요소를 상위 요소의 가운데에 맞춥니다.

```undefined
img {
  vertical-align: middle
}
```

#### 텍스트톱

상위 요소 줄에서 가장 높은 문자 맨 위를 사용하여 요소를 정렬합니다.

```undefined
img {
  vertical-align: text-top
}
```

#### 텍스트 하단

부모 요소의 줄에서 가장 긴 문자 하단을 사용하여 요소를 정렬합니다.

```undefined
img {
  vertical-align: text-bottom
}
```

#### 서브

요소를 상위 요소의 기준선 아래에 정렬합니다. 그것은 마치 `sub` 태그처럼 행동한다.

```undefined
img {
  vertical-align: sub
}
```

#### 슈퍼

요소를 상위 요소의 기준선 위에 정렬합니다. 그것은 `sup` 꼬리표처럼 행동한다.

```undefined
img {
  vertical-align: super
}
```

### 길이 및 백분율 값

요소를 지정된 단위로 정렬합니다. 양의 숫자는 요소를 기준선 위에 정렬하고 음의 값은 요소를 기준선 아래에 정렬합니다.

px, em, % 등의 모든 길이 단위를 사용할 수 있습니다.

```undefined
img {
  vertical-align: 10px
}
img {
  vertical-align: 50%
}
img {
  vertical-align: 3em
}
```

### 전역 값

#### '초기'

요소의 정렬을 기본값인 `기준선`으로 설정합니다.

```undefined
img {
  vertical-align: initial
}
```

#### ➡➡➡➡➡➡➡➡➡➡ ➡

요소의 정렬을 부모 요소의 값으로 설정합니다.

```undefined
img {
  vertical-align: inherit
}
```