---
layout: post
title: "이 React 구성 요소로 SVG를 반응 형으로 만드십시오.
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/makeanysvgresponsivewiththisreactcomponent.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/makeanysvgresponsivewiththisreactcomponent.png?fit=730%2C487&ssl=1)

디지털 콘텐츠를 월드 와이드 웹에 게시하여 작품을 홍보하는 경우 청중이 모바일 장치에서 볼 가능성이 높습니다.
 텍스트와 이미지의 경우 최신 CSS에서는 큰 문제가 아닙니다.
 그러나 콘텐츠가 SVG 요소를 사용하는 데이터 시각화 인 경우 문제가 될 수 있습니다.
 

처음에는 내가 만들고있는 일부 그래프로 모바일 장치에서 문제가 발생했습니다.
 많은 뒤죽박죽 끝에 나는 이러한 고통으로부터 나를 보호 할 수있는 몇 가지 재사용 가능한 부품을 만들었다.
 

## SVG를 확장하려면 얼마나 많은 공간이 필요합니까?
 

SVG 문서의 크기를 조정하는 데 사용할 수있는 공간을 계산할 때 고려해야 할 몇 가지 추상화가 있습니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/viewport2.png?resize=1067%2C944&ssl=1)

### 브라우저 뷰포트
 

브라우저 뷰포트는 웹 페이지의 가시 영역입니다.
 

### SVG 뷰포트
 

SVG 뷰포트는 브라우저의 뷰포트와 유사하지만 SVG 문서의 표시 영역 일뿐입니다.
 SVG 문서는 원하는만큼 높고 넓을 수 있지만 한 번에 문서의 일부만 볼 수 있습니다.
 

아래 예와 같이 하드 코딩 된 너비 및 높이 속성은이 문서를 볼 수있는 여러 장치에 적용 할 수 없기 때문에 피하고자합니다.
 

```coffeescript
export const App: React.FC = () => (
  <svg width="1000" height="1000">
    <rect
      x="20%"
      y="20%"
      width="1000"
      height="1000"
      rx="20"
      style={ fill: '#ff0000', stroke: '#000000', strokeWidth: '2px' }
    />

    <rect
      x="30%"
      y="30%"
      width="1000"
      height="1000"
      rx="40"
      style={ fill: '#0000ff', stroke: '#000000', strokeWidth: '2px', fillOpacity: 0.7 }
    />
  </svg>
);
```

위의 구성 요소는이 문서를 렌더링합니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/viewport4.png?resize=835%2C657&ssl=1)

2 개의 <rect /> 요소로 구성된 위의 SVG 문서는 SVG 뷰포트보다 크고 이로 인해 일부만 표시됩니다.
 

viewBox라는 마법의 속성은 SVG 반응 요구에 대한 답입니다.
 

## viewBox 및 좌표계
 

`viewBox` 속성에 대한 mdn의 정의는 다음과 같습니다.
 

> viewBox 속성은 사용자 공간에서 SVG 뷰포트의 위치와 크기를 정의합니다.
 

나는 당신에 대해 모르지만 처음 읽었을 때 대답보다 질문이 더 많았습니다.
 도대체 사용자 공간이란 무엇입니까?
 

어떤 수준 으로든 수학을했다면 고전적인`x` 및`y` 축이있는 유클리드 공간을 접하게 될 것입니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/euclidean-2d.png?resize=237%2C212&ssl=1)

점 (0, 0)이 중심이 아니라 왼쪽 상단에 있기 때문에 SVG 좌표 공간이 다릅니다.
 

앞서 생성 한 SVG에`viewBox` 속성을 추가하면 SVG 뷰포트의 크기를 다시 제어 할 수 있습니다.
 

```coffeescript
export const App: React.FC = () => (
  <svg preserveAspectRatio="xMaxYMid meet" viewBox="0 0 529 470">
    <rect
      x="20%"
      y="20%"
      width="1000"
      height="1000"
      rx="20"
      style={ fill: '#ff0000', stroke: '#000000', strokeWidth: '2px' }
    />

    <rect
      x="30%"
      y="30%"
      width="1000"
      height="1000"
      rx="40"
      style={ fill: '#0000ff', stroke: '#000000', strokeWidth: '2px', fillOpacity: 0.7 }
    />
  </svg>
);
```

이 마법 속성 덕분에 두 개의 <rect /> 요소는 이제 SVG 뷰포트보다 작습니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/viewBox2.png?resize=768%2C631&ssl=1)

## viewBox 수학
 

다음은`viewBox` 속성의 예입니다.
 

```undefined
viewBox="-200 -100 3000 400"
```

속성에있는 마법의 네 숫자는 요소를 축소하거나 확장하고 위치를 변형 할 수 있습니다.
 하지만 어떻게?
 

viewBox는 다음과 같이 아주 적은 비용으로 많은 작업을 수행합니다.
 

- 이미지의 종횡비를 정의합니다.
 
- SVG 내부에서 사용되는 모든 길이와 좌표를 사용 가능한 공간에 맞게 조정하는 방법을 정의합니다.
 
- 새 좌표계의 원점을 정의합니다.
 

다음과 같은 초기 4 개의 값을 기억하고 싶습니다.
 

```undefined
viewBox="minX minY width height"
```

viewBox를 추가하면 기존 SVG 뷰포트 좌표에서 새로운 좌표 세트가 복사됩니다.
 이 새로운 좌표 세트를 `사용자 공간`이라고합니다.
 mdn의 비밀스러운 설명은 다음과 같이 언급했습니다.
 

> viewBox 속성은 사용자 공간에서 SVG 뷰포트의 위치와 크기를 정의합니다.
 

viewBox의 처음 두 값은 -200과 -100이며,이 값은 이미지를 왼쪽 상단 원점에서 오른쪽 아래로 이동합니다.
 

```xml
<svg
  width="500px"
  height="100px"
  viewBox="-200 -100 3000 400"
>
```

마지막 두 값인 3000과 400을 사용하면 SVG 이미지를 확대하거나 축소 할 수 있습니다.
 

SVG 요소의 너비는 500px이고 높이는 100px입니다.
 viewBox 속성이 추가됨에 따라 3000 단위 및 수직으로 4 억 단위의 새로운 사용자 좌표계를 사용할 수 있습니다.
 

새 사용자 공간은 뷰포트 좌표계에 매핑되며 새 공간의 1 단위는 다음 계산과 같습니다.
 

```undefined
height = SVG viewport height ÷ viewBox height vertically
width = SVG viewport width ÷ viewBox width horizontally
```

viewBox 자체만으로는 모든 반응 요구 사항을 해결할 수 없습니다.
 실제 세계에서는 하드 코딩 된 값을 사용할 수 없습니다.
 컴포넌트가 마운트되거나 크기가 조정될 때 새 값을 가져와야합니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/newheightnewweight.png?resize=1026%2C224&ssl=1)

## 반응 형 SVG
 

다음은 내 자신의 @ cutting / svg 패키지의`ResponsiveSVG` 구성 요소입니다.
 

```js
export function ResponsiveSVG<E extends HTMLElement = HTMLElement>({
  elementRef,
  children,
  preserveAspectRatio = 'xMinYMin slice',
  innerRef,
  className,
  options = {},
}: PropsWithChildren<ParentsizeSVGProps<E>>): JSX.Element | null {
  const { width, height } = useParentSize(elementRef, options);

  const aspect = width / height;

  const adjustedHeight = Math.ceil(width / aspect);

  return (
    <div
      data-selector="cutting-svg-container"
      style={
        position: 'relative',
        overflow: 'visible',
        height: '1px',
      }
    >
      <svg
        style={ overflow: 'visible' }
        className={className}
        preserveAspectRatio={preserveAspectRatio}
        viewBox={`0 0 ${width} ${adjustedHeight}`}
        ref={innerRef}
      >
        {children}
      </svg>
    </div>
  );
}
```

위의 구성 요소는 ResizeObserver를 사용하여 컨테이너`<div />`요소의 변경 사항을 감시하는 useParentSize 후크를 사용합니다.
 타겟`<div />`에서 크기가 변경 될 때마다 크기 조정 이벤트가 새 너비 및 높이 값을 발생시키고 구성 요소는 이러한 새 값에 따라 다시 렌더링됩니다.
 

SVG 요소의 모양이 다른 크기로 유지되도록하려면 너비를 기준으로 높이를 계산해야합니다.
 이 비례 관계를 종횡비라고합니다.
 

## 종횡비
 verified_user

종횡비는 화면에서 이미지의 너비와 높이의 비율입니다.
 

정사각형 이미지의 가로 세로 비율은 `1 : 1`입니다.
 너비의 두 배인 이미지는 가로 세로 비율이 `1 : 2`입니다.
 

조정 된 높이에 대한 계산은 다음과 같습니다.
 

```cpp
const aspect = width / height;
const adjustedHeight = Math.ceil(width / aspect);
```

이 값은`viewBox`를 완성하는 데 도움이됩니다.
 

```xml
<svg
  style={ overflow: 'visible' }
  className={className}
  preserveAspectRatio={preserveAspectRatio}
  viewBox={`0 0 ${width} ${adjustedHeight}`}
  ref={innerRef}
>
```

## `preserveAspectRatio` 속성
 

`viewBox` 속성에는`preserveAspectRatio`라는 사이드킥이 있습니다.
 viewBox가 없으면 아무런 효과가 없습니다.
 `preserveAspectRatio`는`viewBox`의 종횡비가 viewPort의 종횡비와 일치하지 않는 경우 SVG 문서의 크기를 조정하는 방법을 설명합니다.
 대부분의 경우 다음과 같은 기본 동작이 작동합니다.
 

```undefined
preserveAspectRatio="xMidYMid meet"
```

`xMidYMid meet`는 CSS 규칙`background-size : contain;`과 약간 비슷합니다.
 `slice` 값은 더 넉넉한 크기에 맞게 이미지의 크기를 조정하고 여분을 잘라냅니다.
 

```undefined
preserveAspectRatio="xMidYMid slice"
```

`slice` 값은`overflow : hidden` CSS 규칙과 유사합니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/preserveAspectRatio.png?resize=538%2C132&ssl=1)

## 발문
 

내`ResponsiveSVG` 구성 요소를 사용하면 사용자가 내게 던질 수있는 모든 장치에 대한 준비가되어 있습니다. 아래는 브라우저 크기를 최소한으로 조정할 때의 모습입니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/sizer-1.gif?resize=730%2C709&ssl=1)