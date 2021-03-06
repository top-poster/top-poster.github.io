---
layout: post
title: "웹 개발에서 반응하는 이미지를 처리하는 올바른 방법"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![six paintings displayed on easels](https://miro.medium.com/max/12196/0*UeWCxUo3cwHhrzXX)

# 사랑받는 밈

이미지를 웹 페이지에 포함시키는 가장 쉬운 방법은 고전적인 HTML 태그를 사용하는 것입니다.

![Image for post](https://miro.medium.com/max/2092/1*AqMJCTorCreb4lSltm6Ugg.png)

이거 되네. 그러나 한 가지 중요한 문제가 있습니다. 화면 크기에 상관없이 동일한 이미지를 로드합니다. 이것은 문제가 될 수 있습니다. 왜냐하면 당신은 작은 휴대폰과 8K 스크린에서 같은 거대한 4000 x 5120 사진을 사용하고 싶지 않기 때문입니다.

# Rescue로 설정된 소스

img 요소에는 srcset이라는 매우 유용한 속성이 있습니다.

![Image for post](https://miro.medium.com/max/2332/1*66iqYr3aiXTUlKv5gmtZNg.png)

srcset 속성을 사용하면 이미지에 대한 여러 소스를 정의할 수 있습니다. 레이아웃이 동적인 경우(다음의 두 번째 예) 고정 폭 이미지에 대한 장치 픽셀 밀도(다음의 첫 번째 예) 또는 미디어 쿼리에 따라 작동할 수 있습니다.

여기서 src는 브라우저가 단지 Internet Explorer인 srcset을 지원하지 않을 경우 폴백 역할을 한다.

## 예 1.

이미지가 항상 320 CSS 픽셀의 너비를 가지며 `width: 320px`를 통해 CSS로 설정될 수 있다는 것을 알면 다음과 같은 간단한 구문을 사용할 수 있습니다.

![Image for post](https://miro.medium.com/max/2972/1*Tjey9Lwqlqj0dS_CZ3uj0g.png)

이게 어떻게 작동하죠? 화면의 물리적 픽셀 값이 CSS 픽셀 값과 같으면 image-320w.jpg가 로드됩니다. 쉬워요.

그러나 현재 디스플레이는 해상도가 큰 경향이 있기 때문에 많은 픽셀이 1인치(인치당 픽셀 수)로 채워진다. 이 경우 모든 것이 확장됩니다(즉, 실제 물리적 픽셀보다 크게 표시됨). 스케일링에 따라 CSS 픽셀이 실제 픽셀과 같지 않을 수 있습니다.

위에서 우리 srcset을 보세요. 물리적 픽셀과 CSS 픽셀의 2배 차이에 대해서는 최종 사용자에게 더 많은 세부 정보를 제공하는 더 큰 이미지를 촬영하므로 이미지가 멋지고 선명하게 표시됩니다.

## 예 2.

이미지의 치수가 정적이 아닌 경우, `srcset` 속성과 `size` 속성의 조합을 사용하여 어떤 이미지를 어떤 뷰포트 크기로 로드할지도 브라우저에 명시적으로 지정할 수 있습니다.

![Image for post](https://miro.medium.com/max/1688/1*E9OTf3wGqkpJGXRJEuZKbA.png)

![vertical image of leaf taking up whole width of a smartphone screen](https://miro.medium.com/max/4000/1*iJm51tWilf178zAeyAvMzw.png)

이게 어떻게 작동하죠? 브라우저는 `size` 속성에서 모든 조건을 거칩니다. 일치하는 첫 번째 이미지는 기준 너비로 간주되며, 해당 너비에 가장 가까운 이미지는 `srcset`에서 가져온다.

위의 예에 따라 최대 40em(휴대전화 등)의 뷰포트 너비를 기준으로 브라우저는 100vw를 선택하고 이 값을 기준으로 srcset에서 이미지를 선택한다.

40em 이상에서는 브라우저가 뷰포트 폭(50vw)의 50%만 차지하며 이를 기반으로 이미지를 선택할 수 있습니다. 여러분은 "이것이 왜 유용한가?"라고 물어볼 수 있습니다.

![vertical image of a leaf taking up 50% of a smartphone screen](https://miro.medium.com/max/4000/1*M7jUb59qBuOR3t3ZBL5cqQ.png)

음, 다른 장치들의 다른 배치를 생각해보세요. 휴대폰에서는 전체 뷰포트를 포함하는 이미지를 표시하려고 하지만 태블릿과 대형 모니터에서는 50%만 표시하려고 합니다.

꽤 전형적인 사용 사례라고 말할 수 있겠습니다.

# <그림>

HTML 태그는 다른 대체 사진 소스를 제공하는 데 사용될 수 있으며, 일부 미디어 조건에 따라 가장 적합한 것을 선택할 수 있다.

![Image for post](https://miro.medium.com/max/2636/1*JrdzgBGEbtd4XLFZv_r3YA.png)

위의 예제는 뷰포트가 최소 300px인 경우 image1.jpg를 표시하고, 그렇지 않으면 image2.jpg를 표시합니다.

`<그림>` 태그는 예술 방향에 사용되며 화면 해상도/픽셀 밀도에 따라 이미지를 전환하는 데만 사용해서는 안 된다는 점에 유의해야 한다. 이를 위해 위에서 살펴본 바와 같이 srcset 속성을 가진 표준 img 태그가 있다.

![vertical image of a leaf taking up 50% of a smartphone screen](https://miro.medium.com/max/4000/1*M7jUb59qBuOR3t3ZBL5cqQ.png)

## 미술 방향

미술 방향이란 용어는 그림을 완전히 꺼내는 것을 말하며, 따라서 더 이상 같은 이미지를 보여주지 않는다.

애플은 아이패드 출시 당시 이 기술을 사용한 것으로 유명하다. 큰 화면에서는 아이패드의 표준 사진을 보여주지만 작은 화면에서는 다른 각도에서 찍은 아이패드의 사진을 보여주었습니다. 따라서 회전하는 모습을 보여주었습니다. 수직 화면 부동산을 덜 차지했습니다. 똑똑해!

![rotated vertical image of a leaf taking up 50% of an iPad screen](https://miro.medium.com/max/4000/1*YmHe2APXcuemhWHaewcShQ.png)

# WebP와 이미지 포맷의 미래

JPG, JPEG 또는 PNG 이미지를 사용하는 것이 일반적입니다. 현재 대부분의 이미지가 이러한 형식으로 저장됩니다. 그러나 파일 크기에 있어서는 더 잘 할 수 있으므로 페이지 로드 시간을 암묵적으로 향상시킬 수 있습니다.

WebP 형식을 예로 들어 보겠습니다. 구글이 개발한 이 제품은 손실 압축과 무손실 압축 기능을 모두 제공하므로 JPG(손실)와 PNG(무손실) 이미지를 대체할 수 있다.

장점? 속도. JPG 또는 PNG를 WebP로 변환하면 화질이 저하되지 않고도 파일 크기를 크게 줄일 수 있습니다(몇 가지 경우 최대 50%까지 가능하지만 보통 20%에서 30%까지).

즉, 웹 사이트가 훨씬 빠르게 로드되어 더 빠른 사용자 환경을 만들 수 있습니다.

문제는 브라우저 지원일 수 있습니다. 이 기사를 쓸 때, IE와 Safari를 제외한 거의 모든 브라우저가 WebP 형식을 지원합니다.

우리는 WebP와 PNG/JPG를 모두 지원하는 그림 태그와 type 속성을 사용하여 이 문제를 쉽게 해결할 수 있습니다.

코드를 봅시다.

위의 코드는 행동의 구체적인 예를 보여주고 지금까지 우리가 이야기한 모든 것을 다룬다.

브라우저의 WebP 지원(또는 웹P의 부족)에 따라 큰 장치에서는 뷰포트의 100% 폭 또는 50% 너비에 따라 크기가 다른 이미지를 렌더링합니다(`50em` 위의 두 열 레이아웃으로 이동).

이 데모의 소스코드는 GitHub에서 찾을 수 있으며, 여기 호스트된 웹사이트가 있으니 실제로 보실 수 있습니다.

# 키 테이크어웨이즈

이 기사를 끝까지 읽어주셔서 감사합니다.

3초 안에 로딩되지 않으면 사람들이(통계적으로 말하면) 웹 사이트를 포기한다는 것을 명심하십시오. 이미지 크기를 최적화하는 것이 좋은 시작입니다.