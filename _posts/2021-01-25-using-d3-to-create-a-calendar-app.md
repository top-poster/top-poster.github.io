---
layout: post
title: "D3 데이터 시각화를 사용하여 캘린더 앱 만들기
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/d3-calendar.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/d3-calendar.png?fit=730%2C487&ssl=1)

잘 알려진 응용 프로그램 또는 사이트의 복제본을 구축하는 것은 새로운 기술을 배우거나 이미 가지고있는 지식을 레벨 업할 수있는 좋은 방법입니다.
 따라서 D3가 캘린더 앱을 구축하기 위해 접근 할 수있는 첫 번째 도구가 아닐 수도 있지만이를 사용하여 Google 캘린더 복제본을 생성함으로써 라이브러리에 대해 많은 것을 배울 수 있습니다.
 

### D3 데모 프로젝트 설정
 

간단하게 디렉터리를 만든 다음 Snowpack을 사용하여 앱을 스캐 폴딩합니다.
 명령 줄에서 다음을 실행합니다 (원하는 경우 `yarn`에 해당하는 항목을 실행할 수도 있습니다).
 

```undefined
mkdir calendar-clone
cd calendar-clone
npm init -y
npm install --save-dev snowpack
npm install --save d3
```

실행하기 전에 먼저`calendar-clone` 디렉토리의 루트에 다음 내용으로 HTML 파일 (`index.html`)을 만들어야합니다.
 

```xml
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="description" content="Starter Snowpack App" />
    <title>D3 Calendar App</title>
  </head>
  <body>
    <h1>D3 Calendar</h1>
    <script type="module" src="/index.js"></script>
  </body>
</html>
```

또한 동일한 디렉토리 (나머지 작업을 수행 할 위치)에`index.js` 파일을 만듭니다.
 

안전을 위해 콘솔 로그 문을 추가하여 올바르게 실행되고 있는지 확인합니다.
 

```coffeescript
console.log("Let's build a calendar app!");
```

여기에서 개발 서버를 시작하고 터미널에서 다음 명령을 실행할 수 있습니다.
 

```undefined
npx snowpack dev
```

그러면 개발 서버가 시작되고 실행중인 브라우저 창이 열립니다.
 

브라우저 devtools에서 콘솔을 열고 위에서 추가 한 로그를 찾으면 앱 빌드를 시작할 준비가 된 것입니다!
 

## 데모 : D3로 Google 캘린더 앱 복제본 빌드
 

D3는 데이터 가져 오기, 구문 분석 및 시각화에 가장 자주 사용됩니다.
 이 자습서에서는 자체 JSON 개체를 만들어 대부분의 데이터 시각화 프로젝트에서 발생하는 데이터 조작을 무시합니다.
 

### Import 문 및 데이터 선언
 

앱 빌드를 시작하기위한 첫 번째 단계는`index.js` 파일에 추가 한 이전 테스트 코드를 지우는 것입니다.
 `index.js` 파일이 비어있는 상태에서 이제 D3를 가져 와서 캘린더 이벤트를 선언하고 나중에 사용할`dates` 배열을 만듭니다.
 

```js
import * as d3 from 'd3';
const calendarEvents = [
  {
    timeFrom: '2020-11-11T05:00:00.000Z',
    timeTo: '2020-11-11T12:00:00.000Z',
    title: 'Sleep',
    background: '#616161'
  },
  {
    timeFrom: '2020-11-11T16:00:00.000Z',
    timeTo: '2020-11-11T17:30:00.000Z',
    title: 'Business meeting',
    background: '#33B779'
  },
  {
    timeFrom: '2020-11-12T00:00:00.000Z',
    timeTo: '2020-11-12T05:00:00.000Z',
    title: 'Wind down time',
    background: '#616161'
  }
];
// Make an array of dates to use for our yScale later on
const dates = [
  ...calendarEvents.map(d => new Date(d.timeFrom)),
  ...calendarEvents.map(d => new Date(d.timeTo))
];
```

### 표준 D3 변수 추가
 

나중에 더 디자인 할 것이지만 시작하려면 일반적으로 D3 프로젝트의 일부인 몇 가지 변수 (`margin`,`height`,`width`)와 우리 프로젝트에 특정한 몇 가지 변수 (`barWidth`,
 `nowColor`) :
 

```undefined
const margin = { top: 30, right: 30, bottom: 30, left: 50 }; // Gives space for axes and other margins
const height = 1500;
const width = 900;
const barWidth = 600;
const nowColor = '#EA4335';
const barStyle = {
  background: '#616161',
  textColor: 'white',
  width: barWidth,
  startPadding: 2,
  endPadding: 3,
  radius: 3
};
```

### 브라우저 개발 도구로 DOM 업데이트 확인
 

계속하기 전에 모든 것이 작동하는지 확인할 수 있도록 페이지에 표시 할 내용이 표시됩니다.
 

```js
// Create the SVG element
const svg = d3
  .create('svg')
  .attr('width', width)
  .attr('height', height);
// All further code additions will go just below this line

// Actually add the element to the page
document.body.append(svg.node());
// This part ^ always goes at the end of our index.js
```

브라우저를 열면 처음에는 아무것도 눈치 채지 못할 수 있지만 페이지에 큰 SVG 요소가 있으므로 안심하십시오.
 브라우저 devtools를 열고 페이지 요소를 검사하여이를 확인할 수 있습니다.
 

코드가 페이지에서 SVG 요소와 해당 속성을 변경하므로 D3 문제를 디버깅하는 좋은 방법입니다.
 

### 스케일 기능 추가
 

다음으로, 종종`x` 또는`xScale` 및`y` 또는`yScale`이라고하는 스케일 함수를 추가합니다.
 이러한 함수는 데이터의 포인트를 시각화의 픽셀 값에 매핑하는 데 사용됩니다.
 

예를 들어, y 축을 따라 매핑 된 1부터 100까지의 데이터 포인트가있는 경우`y `함수는 82를 SVG 개체의 위쪽으로 매핑합니다 (정확히 82 % 위쪽).
 

대부분의 경우 시간은 x 스케일에 매핑되지만 우리의 목적을 위해 시간을 y 스케일에 수직으로 표시합니다.
 더 간단하게하기 위해 우리는 하루 달력 만 만들 것이므로 x-scale을 전혀 포함 할 필요가 없습니다.
 

```cpp
const yScale = d3
  .scaleTime()
  .domain([d3.min(dates), d3.max(dates)])
  .range([margin.top, height - margin.bottom]);
```

데이터 범위는 `도메인`으로, 픽셀 범위는 `범위`로 전달합니다.
 또한 `range`함수의 경우 먼저 하단이 아닌 `margin.top`을 전달합니다.
 이는 SVG가 위에서 아래로 그려 지므로 0 y 좌표가 맨 위에 있기 때문입니다.
 

### Y 축 그리기
 

나중에 방금 만든 배율 매핑을 사용하지만 먼저 실제 y 축 자체를 그립니다.
 

```js
const yAxis = d3
  .axisLeft()
  .ticks(24)
  .scale(yScale);
// We'll be using this svg variable throughout to append other elements to it
svg
  .append('g')
  .attr('transform', `translate(${margin.left},0)`)
  .attr('opacity', 0.5)
  .call(yAxis);
```

### 커스텀 틱 스타일링
 

그런 다음 지금은 하루 만 표시하므로 나중에 앱의 다른 위치에 실제 날짜를 표시하기 위해 자정 (12AM)으로 표시되도록 첫 번째 및 마지막 틱 스타일을 지정할 수 있습니다.
 

```coffeescript
svg
  .selectAll('g.tick')
  .filter((d, i, ticks) => i === 0 || i === ticks.length - 1)
  .select('text')
  .text('12 AM');
```

### 체크인 중…
 

지금까지 축을 만들고 차트의 왼쪽에 있도록 지정하고 축에 24 개의 눈금 표시를 원하도록 지정하고 배율을 적용했습니다.
 그런 다음`append` 함수를 사용하여 실제로 축을`svg` 요소 내의`g` 요소에 넣었습니다.
 

이것이 모두 올바르게 설정되었는지 확인하려면 지금까지 브라우저 devtools에있는 요소를 검사하십시오.
 앱 자체는 다음과 같습니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/D3-basic-calendar.png?resize=694%2C775&ssl=1)

### 그리드 선 추가
 

이것은 결국 달력이므로 디자인을위한 격자 선을 추가 할 것입니다.
 

이전 단계에서 우리는`yAxis`를 설정하고 틱을 24 시간으로 설정했습니다.
 그리드 선을 설정하기 위해 `axisRight`를 사용하여 틱이 오른쪽에 표시되도록 동일한 작업을 수행합니다.
 

```js
const gridLines = d3
  .axisRight()
  .ticks(24)
  .tickSize(barStyle.width) // even though they're "ticks" we've set them to be full-width
  .tickFormat('')
  .scale(yScale);

svg
  .append('g')
  .attr('transform', `translate(${margin.left},0)`)
  .attr('opacity', 0.3)
  .call(gridLines);
```

이전 접근 방식과 마찬가지로 축을`g` 요소에 배치하고`svg` 요소에 추가했습니다.
 DOM 구조를 보려면 브라우저 devtools를 확인하십시오.
 

### `calendarEvents` 배열 사용
 

축과 격자 선이 제자리에 있으면 캘린더 이벤트에 추가하기 위해 만든`calendarEvents` 배열을 사용할 수 있습니다.
 

이벤트를 추가하려면`[join] (https://observablehq.com/@d3/selection-join)`메소드를 사용하여`svg` 요소에`g` 요소를 추가합니다.
 이를 위해 `barGroup`클래스를 가진 기존의 모든 `g`요소를 선택하고 (힌트 : 아직 존재하지 않음) 존재하는 각 `calendarEvents`항목에 대해 해당 클래스와 새 `g`요소를 결합합니다.
 

이를 위해 추가 할 코드는 다음과 같습니다.
 

```undefined
const barGroups = svg
  .selectAll('g.barGroup')
  .data(calendarEvents)
  .join('g')
    .attr('class', 'barGroup');
```

이제 브라우저 devtools를 보면`svg` 요소 내에 3 개의 빈`g` 항목 (이벤트 목록)이 있음을 확인할 수 있습니다.
 

### 항목에 요소 추가
 

`g` 항목이 생성되고 일정 이벤트에 바인딩되었으므로 이제 다른 요소를 추가 할 수 있습니다.
 

데모에서는`.append` 및`rect`를 사용하여 캘린더 이벤트에 대한 색상 사각형을 만듭니다.
 

```js
barGroups
  .append('rect')
  .attr('fill', d => d.background || barStyle.background)
  .attr('x', margin.left)
  .attr('y', d => yScale(new Date(d.timeFrom)) + barStyle.startPadding)
  .attr('height', d => {
    const startPoint = yScale(new Date(d.timeFrom));
    const endPoint = yScale(new Date(d.timeTo));
    return (
      endPoint - startPoint - barStyle.endPadding - barStyle.startPadding
    );
  })
  .attr('width', barStyle.width)
  .attr('rx', barStyle.radius);
```

위의 예에서 직사각형이 세로로 시작해야하는 위치와 높이가 얼마나되어야하는지 알아 내기 위해 몇 가지 수학을 수행했습니다.
 수학을 변경하면 앱에 미치는 영향을 확인할 수 있습니다.
 기억하세요. 수학을 변경했다면 다시 변경하는 것을 잊지 마세요.
 

### 현재 시간 추적
 

유사한 접근 방식을 사용하여 현재 시간을 추적하는 줄을 추가하여 사용자가 달력에서 "현재"위치를 볼 수 있도록 할 수 있습니다.
 

```js
// Since we've hardcoded all our events to be on November 11 of 2020, we'll do the same thing for the "now" date
const currentTimeDate = new Date(new Date(new Date().setDate(11)).setMonth(10)).setFullYear(2020);

barGroups
  .append('rect')
  .attr('fill', nowColor)
  .attr('x', margin.left)
  .attr('y', yScale(currentTimeDate) + barStyle.startPadding)
  .attr('height', 2)
  .attr('width', barStyle.width);
```

직사각형이 축과 겹치지 않도록 처음에 선언 한`margin` 변수를 다시 한 번 사용했습니다.
 

### 라벨 이벤트
 

마지막으로 달력에있는 이벤트에 대한 레이블을 추가합니다.
 다시`append` 메소드를 사용하지만 이번에는`barGroups` 변수에`text` 요소를 추가합니다.
 

```js
barGroups
  .append('text')
  .attr('font-family', 'Roboto')
  .attr('font-size', 12)
  .attr('font-weight', 500)
  .attr('text-anchor', 'start')
  .attr('fill', barStyle.textColor)
  .attr('x', margin.left + 10)
  .attr('y', d => yScale(new Date(d.timeFrom)) + 20)
  .text(d => d.title);
```

### D3 최종 제품
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/D3-designed-calendar.png?resize=665%2C713&ssl=1)

## 결론
 

이와 마찬가지로 D3.js를 사용하여 자체 캘린더 및 이벤트 앱을 만들었습니다!
 물론 이것은 D3 캘린더 실험의 시작일뿐입니다.
 여기에서 주별 또는 월별 캘린더로 확장하거나 동적 이벤트 생성을 허용하는 등 계속해서 개선 할 수 있습니다.
 

일반적으로 D3에서 캘린더 앱을 빌드하지 못할 수도 있지만이 연습을 통해 D3를 사용하여 배율, 축 및 모양을 그리는 방법에 대한 통찰력을 얻을 수 있습니다.
 향후 실험을 위해 브라우저 개발 도구를 사용하여 D3 코드의 변경 사항이 앱의 SVG 요소에 어떤 영향을 미치고, 변경되고, 수정되는지 확인하십시오.
 