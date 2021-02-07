---
layout: post
title: "CSS 참조 가이드: '위치'"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/lr-css-reference-guide-position-nocdn.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/lr-css-reference-guide-position-nocdn.png?fit=730%2C487&ssl=1)

CSS `position` 속성은 문서에서 요소의 위치를 조작하거나 조정하는 데 사용됩니다. 요소에 사용할 위치 유형을 지정하는 데도 유용합니다.

#### 앞으로 이동:

- 데모
- 가치
정적인
➡➡➡➡➡➡➡➡➡➡ ➡
➡➡➡➡➡➡➡➡➡➡ ➡
`고정`
➡➡➡➡➡➡➡➡➡➡ ➡
- 정적인
- ➡➡➡➡➡➡➡➡➡➡ ➡
- ➡➡➡➡➡➡➡➡➡➡ ➡
- `고정`
- ➡➡➡➡➡➡➡➡➡➡ ➡

## 데모

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="450" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/kaperskyguru/embed/gOMrvoE?height=450&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=kaperskyguru&amp;slug-hash=gOMrvoE&amp;pen-title=CSS%20Position%20Example&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="CSS Position Example" loading="lazy" id="cp_embed_gOMrvoE"></iframe></div>

## 가치

위치 속성에 제공할 수 있는 다섯 가지 주요 위치 또는 값이 있습니다. 아래에서 각 항목을 자세히 살펴봅니다.

### 정적인

`static`은 CSS `position` 속성의 기본값입니다. 문서의 정상적인 흐름에서 요소를 수정하면 상단, 하단, 왼쪽, 오른쪽 값이 제공되더라도 정적으로 배치된 요소에는 영향을 미치지 않는다.

```undefined
div {
  border: 2px solid red;
  position: static;
}
```

이렇게 하면 div 태그가 정상과 동일한 위치에 있게 됩니다.

### ➡➡➡➡➡➡➡➡➡➡ ➡

정적(static)처럼 요소의 위치를 상대(relative)로 설정하면 해당 요소가 문서에서 해당 위치를 유지합니다. 그러나 상단, 하단, 왼쪽, 오른쪽 등의 속성은 요소에 영향을 미치므로 문서 내에서 요소의 위치를 이동하는 데 사용할 수 있다.

```undefined
div {
  border: 2px solid red;
  position: relative
  top: 20%
  bottom: 0
  right: 0
  left: 20%
}
```

이로 인해 div 요소는 상단에서 20%, 왼쪽에서 20% 이동하게 된다. 또한 상단, 하단, 왼쪽, 오른쪽 속성에 주어진 값이 다른 요소에 영향을 미치지 않는다는 점도 주목할 필요가 있다.

### ➡➡➡➡➡➡➡➡➡➡ ➡

절대 위치에 있는 요소는 `위치`가 `절대`로 설정됩니다. 구성요소는 문서의 정상적인 흐름에서 제거되고, 다른 구성요소는 작성된 공간을 채우며, 완전히 배치된 구성요소가 없는 것처럼 작동합니다.

절대 위치 요소도 상대적인 값을 갖는 요소처럼 상단, 하단, 왼쪽, 오른쪽 속성을 사용하여 제어할 수 있다.

또한 스크롤에 의해 절대 위치 요소가 영향을 받는다는 점을 유념해야 합니다.

```undefined
div {
  border: 2px solid red;
  position: absolute
  top: 30%
  bottom: 0
  right: 0
  left: 40%
}
```

### '고정'

고정 요소는 문서의 정상적인 흐름에서 요소를 제거하고 상단, 하단, 왼쪽, 오른쪽을 사용하여 위치에 영향을 줄 수 있는 절대 위치 요소와 동일하게 동작합니다.

차이점은 `고정` 요소는 절대 위치 요소에서처럼 가장 가까운 위치 선조가 아닌 뷰포트에 상대적이기 때문에 스크롤의 영향을 받지 않는다는 것이다.

```undefined
div {
  border: 2px solid red;
  position: fixed
  top: 50%
  bottom: 0
  right: 0
  left: 10%
}
```

### ➡➡➡➡➡➡➡➡➡➡ ➡

"sticky" 위치 요소는 선언된 임계값에 도달할 때까지 스크롤하는 동안 상대적으로 위치된 요소처럼 동작하며, 이때 요소는 "fixed" 요소처럼 동작하며, 요소는 그 지점에 고정됩니다.

```undefined
div {
  border: 2px solid red;
  position: sticky;
  top: 50%
}
```

위의 예에서 `div` 요소는 뷰포트의 50%에 도달할 때까지 계속 스크롤되며, 이때 스크롤이 중지되고 고정됩니다.