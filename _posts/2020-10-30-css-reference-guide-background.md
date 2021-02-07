---
layout: post
title: "CSS 참조 가이드: '배경'"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/lr-css-reference-guide-background-nocdn.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/lr-css-reference-guide-background-nocdn.png?fit=730%2C487&ssl=1)

CSS `background` 속기 속성은 요소의 배경 효과를 설정합니다. 이것은 요소의 내용 아래에 그리는 것을 제어합니다.

#### 앞으로 이동:

- 데모
- 구성 요소 속성
배경 이미지
배경 위치
배경 크기
배경화면
`배경부착`
배경화면
배경 클립
배경색
- 배경 이미지
- 배경 위치
- 배경 크기
- 배경화면
- `배경부착`
- 배경화면
- 배경 클립
- 배경색

다음 예에서는 요소에 대한 배경 효과를 생성하기 위해 지정할 수 있는 속성의 전체 목록을 보여 줍니다.

```undefined
header {
  background: 
    url('folder/image.jpg') // image
    top center /  // position
    150px 150px   // size
    no-repeat     // repeat
    fixed         // attachment
    padding-box   // origin
    content-box   // clip
    blue;         // color
}
```

## 데모

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/kaperskyguru/embed/PozNbVY?height=500&amp;theme-id=dark&amp;default-tab=html%2Cresult&amp;user=kaperskyguru&amp;slug-hash=PozNbVY&amp;pen-title=CSS%20Background%20Example&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="CSS Background Example" loading="lazy" id="cp_embed_PozNbVY"></iframe></div>

## 구성 요소 속성

background(배경) 속기는 8가지 성분 속성으로 구성된다. 아래 각각에 대해 자세히 알아보겠습니다.

### 배경 이미지

background-image는 하나 이상의 이미지를 요소의 배경으로 설정하는 CSS 속성입니다. 여러 개의 이미지를 지정할 때 해당 이미지는 스택으로 표시되고 다른 이미지 위에 하나씩 표시됩니다.

#### 기본값

배경 이미지 기본값은 `없음`으로, 이는 단순히 이미지가 표시되지 않음을 의미합니다.

```undefined
header {
  background-image: none
}
```

#### 사용법

이 예에서는 단일 이미지만 지정합니다.

```undefined
header {
  background-image: url('folder/image.jpg');
}
```

이 예에서는 첫 번째 이미지를 두 번째 이미지 위에 표시합니다.

```undefined
header {
  background-image: 
    url('folder/first.jpg'), 
    url('folder/second.jpg')
}
```

이 예에서는 `두 번째` 이미지 위에 선형 그라디언트를 지정합니다.

```undefined
header {
  background-image: 
    linear-gradient(to bottom, rgba(255,255,0,0.5), rgba(0,0,255,0.5)),
    url('folder/second.jpg')
}
```

### 배경 위치

배경 위치 속성은 배경 이미지의 시작 위치를 설정합니다. 이 값은 이미지가 요소 상자의 가장자리를 기준으로 배치될 좌표를 지정합니다.

배경 위치는 키워드(왼쪽, 위쪽, 아래쪽, 오른쪽, 가운데), 백분율(0%(25% 등), 길이(0cm, 3px 등)로 지정할 수 있다. 이러한 값은 함께 결합할 수 있습니다.

#### 기본값

배경 위치`의 기본값은 `0% 0%`로, 요소 상자의 왼쪽 상단 모서리를 나타냅니다.

```undefined
header {
  background-position: 0% 0% // top left
}
```

#### 사용법

```undefined
header {
  background-position: top // same as top center
}

header {
  background-position: left // same as left center
}

header {
  background-position: center // center
}

header {
  background-position: 20% 50% // 20% from left and 50% from top
}

header {
  background-position: bottom 20px left 50px // 20px from bottom and 50px from left
}

header {
  background-position: left 2cm bottom 5cm; // 2cm from left and 5cm from bottom
}
```

### 배경 크기

배경 크기 속성은 배경 이미지 크기를 지정합니다. 이미지를 늘리거나, 기본 크기로 표시하거나, 사용 가능한 전체 공간을 채울지 정의합니다.

현재 이미지 크기로 가려진 공간들이 배경색으로 채워질 것이라는 점도 주목할 필요가 있다.

`배경 크기`는 키워드, 백분율 또는 길이를 사용하여 지정할 수 있습니다.

#### 키워드

- 커버(cover): 이미지를 최대한 크게 확장하지만 이미지를 늘리지 않습니다. 또한 이미지를 수직 또는 수평으로 잘라 빈 공간을 덮을 수 있습니다.
- `포함`: 이미지를 최대한 크게 확장하지만 이미지를 자르거나 늘리지 않습니다.
- auto: 이미지가 원래 비율을 유지하도록 해당 방향으로 축척합니다.

#### 기본값

배경 크기의 기본값은 auto입니다.

```undefined
header {
  background-size: auto;
}
```

#### 사용법

```undefined
header {
  background-size: auto; // default value
}

// stretches to fill the element's box
header {
  background-size: cover; 
}

// scale the image without cropping/scretching. Image can repeat to fill element's box
header {
  background-size: contain; 
}

// scale image by 20% width & auto for height. Image can repeat to fill element's box
header {
  background-size: 20%|em|cm|auto|px;
}

// scale image by 3em width and 25% height. Image can repeat to fill element's box
header {
  background-size: 3em 25%;
}

// You can specify more values for multiple backgrounds
header {
  background-size: 50%, 25%, 60%;
}
```

### 배경화면

배경 반복 속성은 배경 이미지의 반복 여부 및/또는 반복 방법을 지정합니다. 이 속성은 배경 이미지를 수평으로 반복하도록 지정하거나 아예 반복할지 여부를 지정할 수 있습니다.

기본적으로 이미지가 요소의 크기에 맞게 잘리는 만큼 배경 반복 속성을 사용하여 요소의 상자에 맞거나 채우도록 크기를 조정할 수도 있습니다.

백그라운드 반복 속성은 키워드 값을 사용하여 지정됩니다.

#### 키워드

- ➡: 전체 배경을 덮기 위해 이미지를 반복합니다.
- `불가침`: 이미지를 반복하지 않음
- `공간`: 클리핑하지 않고 이미지를 반복하고 반복된 이미지 간에 공백을 균등하게 분배합니다.
- `둥근`: 이미지를 반복하고 반복되는 이미지 사이의 공백을 채웁니다.
- `repeat-x`: x축을 따라만 영상 반복
- `반복 y`: Y축을 따라 영상만 반복

#### 기본값

background-repeat(백그라운드 반복)의 기본값은 `repeat(반복)`으로, 전체 요소 배경을 채울 때까지 이미지를 반복합니다.

```undefined
header {
  background-repeat: repeat;
}
```

#### 사용법

```undefined
header {
  background-repeat: repeat-x;  // equivalent `repeat no-repeat`
}

header {
  background-repeat: repeat-y; // equivalent `no-repeat repeat`
}

header {
  background-repeat: repeat; // equivalent `repeat repeat`
}

header {
  background-repeat: space; // equivalent `space space`
}

header {
  background-repeat: round; // equivalent `round round`
}

header {
  background-repeat: no-repeat; // equivalent `no-repeat no-repeat`
}
```

### '배경부착'

배경 첨부 파일 속성은 배경 이미지를 스크롤할 수 있는지 또는 뷰포트에 고정할지 여부를 지정합니다.

백그라운드 첨부 파일 속성은 키워드로만 지정됩니다.

#### 키워드

- 스크롤: 배경 이미지를 요소에 고정합니다. 배경 이미지를 내용과 함께 스크롤하지 않음
- `fixed`: 배경 이미지를 뷰포트에 고정합니다. 내용이 스크롤 가능하더라도 배경 이미지는 스크롤되지 않습니다.
- `local`: 요소의 내용을 기준으로 배경 이미지를 수정하여 내용이 스크롤될 때 배경 이미지를 스크롤할 수 있도록 합니다.

#### 기본값

배경 첨부 파일의 기본값은 `스크롤`입니다.

```undefined
header {
  background-attachment: scroll;
}
```

#### 사용법

```undefined
header {
  background-attachment: scroll; // Fix image to element (can scrol only if the element is scrolling not the content of the element)
}

header {
  background-attachment: fixed; // Fix image to viewport
}

header {
  background-attachment: local; // Scrolls the image with content
}

// On multiple images we can specify double values
header {
  background-image: url("star1.gif"), url("star2.gif");
  background-attachment: fixed, scroll;
}
```

### 배경화면

background-origin 속성은 컨텐츠 박스 값을 사용하여 전체 요소를 가로질러 경계 상자 값을 사용하여 테두리 내에서 또는 패딩 값을 사용하여 패딩 내에서 배경 이미지를 그릴 위치를 지정합니다.

배경 첨부파일을 고정으로 설정하면 배경 원점이 무시되고 배경 원점도 클립이 아닌 배경 클립과 동일하다는 점에 유의한다.

백그라운드 오리진 속성은 키워드 값을 사용하여 지정됩니다.

#### 키워드

- `내용 상자`: 내용 상자를 기준으로 배경 위치 지정
- `경계선 상자`: 테두리 상자를 기준으로 배경 위치 지정
- `상자 상자`: 패딩 상자를 기준으로 배경 위치 지정

#### 기본값

background-origin의 기본값은 padding-box 키워드입니다.

```undefined
header {
  background-origin: padding-box;
}
```

#### 사용법

```undefined
header {
  background-origin: border-box;
}

header {
  background-origin: padding-box;
}

header {
  background-origin: content-box;
}

// We can specify double values for multiple images
header {
  background-image: url('logo.jpg'), url('main.png');
  background-origin: content-box, padding-box;
}
```

### 배경 클립

배경 클립 속성은 요소의 배경이 상자를 확장할지 여부를 결정합니다. 이 속성은 배경 이미지 또는 배경색이 지정된 경우에만 실제 효과를 나타내며, 그렇지 않은 경우 경계선에만 시각적 효과를 줍니다.

CSS `background-clip` 속성은 키워드 값을 사용하여 지정됩니다.

#### 키워드

- `경계선 상자`: 요소의 테두리를 넘어 배경을 펼칩니다.
- `내용 상자`: 요소의 내용에 배경을 클립으로 고정합니다.
- `상자 상자`: 요소의 패딩 너머로 배경을 확장합니다.
- 텍스트: 배경을 전경 텍스트로 클리핑하는 실험 값

#### 기본값

background-clip의 기본값은 border-box 키워드입니다.

```undefined
header {
  background-clip: border-box;
}
```

#### 사용법

```undefined
header {
  background-clip: border-box;
}

header {
  background-clip: padding-box;
}

header {
  background-clip: content-box;
}
```

### 배경색

CSS `background-color` 속성은 요소의 배경색을 설정하는 데 사용됩니다. 이미지가 로드되는 데 시간이 걸리는 경우 "background-image" 속성과 결합할 수 있습니다.

배경색 값은 색상 이름, 16진수 값, RGB/RGBA 값 또는 HSL/HSLA 값으로 지정됩니다.

#### 기본값

배경색 기본값은 투명 키워드입니다.

```undefined
header {
  background-color: transparent;
}
```

#### 사용법

```undefined
/* Keyword values */
header {
  background-color: red;
}

/* Hexadecimal value */
header {
  background-color: #bbff00;
}

/* RGB value */
header {
  background-color: rgb(255, 255, 128);        /* Fully opaque */
}
header {
  background-color: rgba(117, 190, 218, 0.5);  /* 50% transparent */
}

/* HSL value */
header {
  background-color: hsl(50, 33%, 25%);         /* Fully opaque */
}
header {
  background-color: hsla(50, 33%, 25%, 0.75);  /* 75% transparent */
}
```