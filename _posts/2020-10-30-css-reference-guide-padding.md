---
layout: post
title: "CSS 참조 가이드: '패딩'"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/lr-css-reference-guide-padding-nocdn.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/lr-css-reference-guide-padding-nocdn.png?fit=730%2C487&ssl=1)

CSS `패딩` 속기 속성은 요소 내에 공간을 만드는 데 사용됩니다. 상자 모델의 내용 부분을 정의합니다.

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

패딩 속성은 길이, 백분율, 키워드(예: auto)를 사용하여 지정할 수 있습니다. 또한 음수 값을 사용할 수도 있습니다.

## 데모

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/kaperskyguru/embed/JjKXpKy?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=kaperskyguru&amp;slug-hash=JjKXpKy&amp;pen-title=CSS%20Padding%20Example&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="CSS Padding Example" loading="lazy" id="cp_embed_JjKXpKy"></iframe></div>

## 구문

```undefined
div {
  padding: <length> | <percentage> | auto
}
```

CSS 패딩(padding) 속기 속성은 패딩톱, 패딩우측, 패딩좌측, 패딩바텀에 대해 최대 4개의 값을 정의하는 데 사용된다.

```undefined
div {
  padding: 2px 3px 4px 5px;
}
```

동등한 `패딩`은 다음과 같이 정의할 수 있습니다.

```undefined
div {
 padding-top: 2px;
 padding-right: 3px;
 padding-left: 5px;
 padding-bottom: 4px;
}
```

## 가치

패딩 속성은 1~4개의 값을 허용할 수 있습니다.

### 한 값

한 값이 CSS 패딩(padding) 속기 속성에 공급되면 4면 모두에 동일한 패딩(padding) 값을 적용한다.

```undefined
div {
  padding: 5px;
}
 // SAME AS
div {
 padding-top: 5px;
 padding-right: 5px;
 padding-left: 5px;
 padding-bottom: 5px;
}
```

### 두 값

두 값이 `패딩` 속성에 지정되면 첫 번째 값은 상단과 하단 패딩에, 두 번째 값은 왼쪽과 오른쪽에 적용됩니다.

```undefined
div {
  padding: 5px 3px;
}
 // SAME AS
div {
 padding-top: 5px;
 padding-right: 3px;
 padding-left: 3px;
 padding-bottom: 5px;
}
```

### 세 가지 값

세 개의 값이 제공되면 첫 번째 값이 상단 패딩에 적용되고, 두 번째 값이 왼쪽 및 오른쪽 패딩에 적용되고, 세 번째 값이 하단 패딩에 적용됩니다.

```undefined
div {
  padding: 5px 3px 1px;
}
 // SAME AS
div {
  padding-top: 5px;
  padding-right: 3px;
  padding-left: 3px;
  padding-bottom: 1px;
}
```

### 네 개의 값

4개의 값이 제공되면 값이 시계 방향으로 적용됩니다. 즉, 첫 번째 값은 상단 패딩에 적용되고, 두 번째 값은 오른쪽 패딩에 적용되며, 세 번째 값은 하단에 적용되며, 네 번째 값은 왼쪽 패딩에 적용됩니다.

```undefined
div {
  padding: 5px 3px 1px 6px;
}
 // SAME AS
div {
  padding-top: 5px;
  padding-right: 3px;
  padding-left: 6px;
  padding-bottom: 1px;
}
```

### 중심 요소

auto 키워드를 사용하면 패딩 속성을 사용하여 컨테이너에 요소를 배치하기가 매우 쉽습니다.

```undefined
div {
  padding: 2em auto;  /* top and bottom: 2em padding   */
                    /* Box is horizontally centered */
}

div {
  padding: auto;    /* top and bottom: 0 padding     */
                  /* Box is horizontally centered */
}
```

### 음수 값

패딩에 음수 값이 공급되면 요소를 밀어넣기보다는 특정 방향으로 끌어당깁니다.

예를 들어 아래 조각은 요소를 위쪽 방향으로 5px, 오른쪽에서 3px, 왼쪽에서 6px, 아래쪽을 1px로 당깁니다.

```undefined
div {
  padding: -5px -3px -1px -6px;
}
```