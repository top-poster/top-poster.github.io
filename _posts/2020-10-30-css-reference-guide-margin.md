---
layout: post
title: "CSS 참조 가이드: '마진'"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/lr-css-reference-guide-margin-nocdn.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/lr-css-reference-guide-margin-nocdn.png?fit=730%2C487&ssl=1)

CSS `마진` 단축 속성은 정의된 테두리 밖의 요소 주위에 공간을 만드는 데 사용됩니다. 상자 모델의 가장 바깥쪽 부분을 정의합니다.

#### 앞으로 이동:

- 데모
- 구문
- 가치
한 값
두 값
세 가지 값
네 개의 값
중심 요소
음수 값
- 한 값
- 두 값
- 세 가지 값
- 네 개의 값
- 중심 요소
- 음수 값

여백 속성은 길이, 백분율, 키워드(예: auto)를 사용하여 지정할 수 있습니다. 또한 음수 값을 사용할 수도 있습니다.

## 데모

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/kaperskyguru/embed/XWKdVBb?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=kaperskyguru&amp;slug-hash=XWKdVBb&amp;pen-title=CSS%20Margin%20Example&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="CSS Margin Example" loading="lazy" id="cp_embed_XWKdVBb"></iframe></div>

## 구문

```css
div {
  margin: <length> | <percentage> | auto
}
```

margin은 margin-top, margin-right, margin-left, margin-bottom에 대해 최대 4개의 값을 정의하는 데 사용되는 CSS 속기 속성이다.

```undefined
div {
  margin: 2px 3px 4px 5px;
}
```

등가 `마진`은 다음과 같이 정의할 수 있다.

```undefined
div {
  margin-top: 2px;
  margin-right: 3px;
  margin-left: 5px;
  margin-bottom: 4px;
}
```

## 가치

마진부동산은 1~4개의 값을 수용할 수 있다.

### 한 값

한 값이 마진 속기에 공급되면 네 면 모두에 동일한 마진 값을 적용한다.

```undefined
div {
  margin: 5px;
}
 // SAME AS
div {
  margin-top: 5px;
  margin-right: 5px;
  margin-bottom: 5px;
  margin-left: 5px;
}
```

### 두 값

두 값이 `마진` 속성에 공급되면 첫 번째 값이 위쪽과 아래쪽 여백에 적용되고 나머지 값은 왼쪽과 오른쪽 여백에 적용됩니다.

```undefined
div {
  margin: 5px 3px;
}
 // SAME AS
div {
  margin-top: 5px;
  margin-right: 3px;
  margin-left: 3px;
  margin-bottom: 5px;
}
```

### 세 가지 값

세 개의 값이 제공되면 첫 번째 값이 위쪽 여백에 적용되고 두 번째 값이 왼쪽과 오른쪽 여백에 적용되고 세 번째 값이 아래쪽 여백에 적용됩니다.

```undefined
div {
  margin: 5px 3px 1px;
}
 // SAME AS
div {
  margin-top: 5px;
  margin-right: 3px;
  margin-left: 3px;
  margin-bottom: 1px;
}
```

### 네 개의 값

4개의 값이 제공되면 값이 시계 방향으로 적용됩니다. 즉, 첫 번째 값이 위쪽 여백에 적용되고 두 번째 값이 오른쪽 여백에 적용되며 세 번째 값이 아래쪽 여백에 적용되고 네 번째 값이 왼쪽에 적용됩니다.

```undefined
div {
  margin: 5px 3px 1px 6px;
}
 // SAME AS
div {
  margin-top: 5px;
  margin-right: 3px;
  margin-left: 6px;
  margin-bottom: 1px;
}
```

### 중심 요소

auto 키워드를 사용하면 여백을 사용하여 컨테이너에 요소를 배치하기가 매우 쉽습니다.

```undefined
div {
  margin: 2em auto;  /* top and bottom: 2em margin   */
                    /* Box is horizontally centered */
}

div {
  margin: auto;    /* top and bottom: 0 margin     */
                  /* Box is horizontally centered */
}
```

### 음수 값

음수 값이 `여백`에 공급되면 요소를 밀어넣기보다는 특정 방향으로 끌어당깁니다.

예를 들어 아래 조각은 요소를 위쪽 방향으로 5배, 오른쪽을 향해 3배, 왼쪽을 향해 6px, 아래쪽을 향해 1px 당깁니다.

```undefined
div {
  margin: -5px -3px -1px -6px;
}
```