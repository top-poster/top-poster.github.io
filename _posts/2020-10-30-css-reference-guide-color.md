---
layout: post
title: "CSS 참조 가이드: 'color'"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/lr-css-reference-guide-color-nocdn.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/lr-css-reference-guide-color-nocdn.png?fit=730%2C487&ssl=1)

CSS `color` 속성은 텍스트 요소 또는 텍스트 장식의 전경색을 설정합니다.

#### 앞으로 이동:

- 데모
- 구문
- 가치
키워드
명명된 값
16진수 값
RGB 및 RGBA 값
HSL 및 HSLA 값
- 키워드
- 명명된 값
- 16진수 값
- RGB 및 RGBA 값
- HSL 및 HSLA 값

## 데모

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="480" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/kaperskyguru/embed/jOrqYYX?height=480&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=kaperskyguru&amp;slug-hash=jOrqYYX&amp;pen-title=CSS%20Color%20Example&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="CSS Color Example" loading="lazy" id="cp_embed_jOrqYYX"></iframe></div>

## 구문

```undefined
p.text {
  color: <rgb()> | <rgba()> | <hsl()> | <hsla()> | <hex-color> | <named-color> | currentcolor
}
```

## 가치

CSS `color` 속성의 값은 다양한 방법으로 지정할 수 있습니다. 우리는 아래에서 그것들을 자세히 검토한다.

### 키워드

CSS `color` 속성은 키워드를 값으로 사용할 수 있습니다. 기본값은 "color" 속성의 상속된 값에서 값을 가져오는 키워드 "current color"입니다.

```undefined
p.text {
  color: currentcolor;
}
```

우리는 또한 불투명하지 않고 완전히 투명하고 투명한 색상 값을 가지고 있다.

```undefined
p.text {
  color: currentcolor | transparent
}
```

### 명명된 값

명명된 값을 명명된 색상이라고도 합니다. 이것들은 16진수 값이 붙어 있는 빨간색, 녹색 등과 같은 이름 붙여진 색깔들이다.

```undefined
p.text {
  color: red | green | aqua | tan | rebeccapurple | etc
}
```

### 16진수 값

색상의 16진수 값은 `#` 문자 앞에 6자의 영숫자 문자열로 표시됩니다.

16진수 값의 첫 번째 영숫자 쌍은 빨간색 값을, 두 번째 쌍은 녹색 값을, 세 번째 쌍은 RGB 스펙트럼의 파란색 값을 나타낸다.

CSS는 또한 3자 약어를 사용하여 16진수 값을 지정할 수 있다. 이 값은 항상 00~FF(6자 문자열) 또는 0~F(3자 문자열)의 범위여야 합니다.

```undefined
p.text {
  color: #00FFFF // 00 => RED, FF => GREEN and FF => BLUE
}

div.text {
  color #eee // hex value can also be 3 values only. E => RED, E => GREEN and E => BLUE
}
```

### RGB 및 RGBA 값

RGB 색상은 쉼표로 구분된 세 가지 값 목록으로 표시되며, 이 값은 빨간색, 녹색, 파란색 순으로 표시됩니다. 이 세 가지 값은 0에서 2 5까지 또는 0에서 100%까지이다. 기본적으로 모든 RGB 색상은 불투명합니다.

```undefined
p.text {
 color : rgb(0, 255, 255);
}

p.text {
  color : rgb(0%, 100%, 100%);
}
```

RGBA 색상 값을 사용하면 색상의 불투명도(또는 알파) 값을 지정할 수 있습니다. 여기서 `0.0`은 완전 투명도를 나타내고 `1`은 완전 불투명도를 나타냅니다. 이러한 방법으로 RGB 색상의 불투명도를 조정할 수 있습니다.

```undefined
p.text {
 color : rgb(0, 255, 255);
}

p.text {
  color : rgba(0%, 100%, 100%, 0.8);
}
```

### HSL 및 HSLA 값

HSL 색상은 색조(0~360), 채도(0%100%), 밝기(0%100%)를 나타내는 3가지 값으로 표시된다. 기본적으로 모든 HSL 색상은 불투명합니다.

```undefined
p.text {
  color: hsl(180, 100%, 50%);
}
```

HSLA 값을 사용하면 색상의 불투명도(또는 알파) 값을 지정할 수 있습니다. 여기서 `0.0`은 완전 투명도를 나타내고 `1`은 완전 불투명도를 나타냅니다. 이 방법으로 HSL 색상의 불투명도를 조정할 수 있습니다.

```undefined
p.text {
  color: hsla(180, 100%, 50%, 0.5);
}
```