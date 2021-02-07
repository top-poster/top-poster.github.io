---
layout: post
title: "반응과 함께 D3.js v6 사용"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/using-d3-js-v6-react.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/using-d3-js-v6-react.png?fit=730%2C487&ssl=1)

리액트는 세계에서 가장 인기 있는 프런트 엔드 웹 프레임워크이며, D3.js는 스크린에서 그래픽을 조작하는 가장 인기 있는 자바스크립트 라이브러리 중 하나이다.

버전 6은 D3.js의 최신 릴리스이며, 이 문서에서는 React 앱에서 D3.js v6를 사용하는 방법에 대해 알아보겠습니다.

## 시작 중

먼저 다음과 같은 애플리케이션 생성을 사용하여 Retact 프로젝트를 만들 것입니다.

```undefined
npx create-react-app d3-app
cd d3-app
npm start
```

그런 다음 D3.js 패키지를 설치합니다.

```coffeescript
npm i d3
```

이제 Relect 앱에 D3를 추가하여 그래픽을 추가할 수 있습니다.

## React 앱에서 D3.js v6 사용

앱에서 D3 코드를 사용효과 후크 콜백에 넣으면 D3를 사용할 수 있습니다. DOM으로 요소를 선택한 다음 D3로 변경하기 때문입니다.

예를 들어 다음과 같이 쓸 수 있습니다.

```js
import React, { useEffect } from "react";
import * as d3 from "d3";

export default function App() {
  useEffect(() => {
    d3.select(".target").style("stroke-width", 5);
  }, []);
  return (
    <div className="App">
      <svg>
        <circle
          class="target"
          style={ fill: "green" }
          stroke="black"
          cx={50}
          cy={50}
          r={40}
        ></circle>
      </svg>
    </div>
  );
}
```

우리는 대상 클래스와 함께 SVG를 얻은 다음 그것을 선택한 후에 스트로크 폭을 변경합니다.

우리는 또한 D3 코드를 함수에 넣고 우리가 사용하고 싶을 때 호출할 수 있습니다. 예를 들어 다음과 같이 쓸 수 있습니다.

```js
import React from "react";
import * as d3 from "d3";

export default function App() {
  const changeStroke = () => {
    d3.select(".target").style("stroke-width", 5);
  };

  return (
    <div className="App">
      <button onClick={changeStroke}>change stroke</button>
      <svg>
        <circle
          class="target"
          style={ fill: "green" }
          stroke="black"
          cx={50}
          cy={50}
          r={40}
        ></circle>
      </svg>
    </div>
  );
}
```

Change Stroke(변경 스트로크) 기능에서 원의 스트로크를 변경하도록 D3 코드를 설정하고 버튼 클릭으로 호출합니다.

우리는 또한 다른 `svg` 요소에 모든 것을 추가할 수 있다. 예를 들어, 다음 코드를 작성하여 3개의 원을 추가할 수 있습니다.

```js
import React, { useEffect } from "react";
import * as d3 from "d3";

export default function App() {
  useEffect(() => {
    const svg = d3.select("#area");
    svg
      .append("circle")
      .attr("cx", 50)
      .attr("cy", 50)
      .attr("r", 40)
      .style("fill", "blue");
    svg
      .append("circle")
      .attr("cx", 140)
      .attr("cy", 70)
      .attr("r", 40)
      .style("fill", "red");
    svg
      .append("circle")
      .attr("cx", 300)
      .attr("cy", 100)
      .attr("r", 40)
      .style("fill", "green");
  }, []);

  return (
    <div className="App">
      <svg id="area" height={200} width={450}></svg>
    </div>
  );
}
```

분해해 봅시다.

- 원(circle) 논리가 있는 첨부 방식은 원(circle)을 추가한다.
- atttr 메서드 호출은 원의 속성을 추가합니다.
- `cx`는 원의 중심에 있는 x-분위수이다.
- cy는 원의 중심에서 y를 뺀 것이다.
- r은 반지름이다.
- `style`에는 원의 `style` 속성에 넣으려는 속성과 값이 포함됩니다.

## 그래픽 스케일링

우리는 `스케일 리니어` 방법을 사용하여 리액트 앱에서 D3.js로 그래픽을 확장할 수 있다. 예를 들어, 다음 코드를 작성하여 그래프에 x축을 추가할 수 있습니다.

```js
import React, { useEffect } from "react";
import * as d3 from "d3";

export default function App() {
  useEffect(() => {
    const margin = { top: 10, right: 40, bottom: 30, left: 30 },
      width = 450 - margin.left - margin.right,
      height = 400 - margin.top - margin.bottom;

    const svg = d3
      .select("#area")
      .append("svg")
      .attr("width", width + margin.left + margin.right)
      .attr("height", height + margin.top + margin.bottom)
      .append("g")
      .attr("transform", `translate(${margin.left}, ${margin.top})`);

    const x = d3.scaleLinear().domain([0, 100]).range([0, width]);
    svg
      .append("g")
      .attr("transform", `translate(0, ${height})`)
      .call(d3.axisBottom(x));

    const y = d3.scaleLinear().domain([0, 100]).range([height, 0]);
    svg.append("g").call(d3.axisLeft(y));
  }, []);

  return (
    <div className="App">
      <svg id="area" height={400} width={500}></svg>
    </div>
  );
}
```

우리는 간단히 `d3`라고 부른다.x축을 추가하는 x 함수를 가진 축 하단. 그러면 `도메인` 함수에 전달된 최소값과 최대값 사이에 값이 추가됩니다.

## 그래프에 y축 추가

D3로 그래프에 Y축을 추가할 수 있다는 것은 두말할 나위도 없습니다.

위의 코드에서는 그래프의 여백을 보다 쉽게 설정할 수 있도록 `여백` 개체를 추가했습니다. 그런 다음 다음과 같은 코드를 사용하여 페이지 본문에 `svh` 개체를 추가합니다.

```js
const svg = d3
  .select("#area")
  .append("svg")
  .attr("width", width + margin.left + margin.right)
  .attr("height", height + margin.top + margin.bottom)
  .append("g")
  .attr("transform", `translate(${margin.left}, ${margin.top})`);
```

폭과 높이를 설정한 다음 원하는 위치로 물체를 `번역`합니다.

다음으로, 다음과 같이 기록하여 x축과 척도를 추가합니다.

```js
const x = d3.scaleLinear().domain([0, 100]).range([0, width]);
svg
  .append("g")
  .attr("transform", `translate(0, ${height})`)
  .call(d3.axisBottom(x));
```

우리는 이전처럼 도메인과 범위라고 부르고 d3라고 부른다.axisBottom` 메서드를 사용하여 x축을 추가합니다.

그런 다음 y축을 추가하기 위해 다음과 같이 씁니다.

```cpp
const y = d3.scaleLinear().domain([0, 100]).range([height, 0]);
svg.append("g").call(d3.axisLeft(y));
```

우리는 y축에 대한 최소값과 최대값을 만들기 위해 도메인과 범위라고 불렀고, y축을 추가하기 위해 d3.axis Left라고 불렀다.

## 산점도 생성

그래프에 점/점을 추가하여 산점도를 작성하려면 전과 같이 원을 추가할 수 있습니다. 예를 들어, 다음을 작성하여 전체 산점도 그래프를 만들 수 있습니다.

```js
import React, { useEffect } from "react";
import * as d3 from "d3";

const DATA = [
  { x: 10, y: 20 },
  { x: 20, y: 50 },
  { x: 80, y: 90 }
];

export default function App() {
  useEffect(() => {
    const margin = { top: 10, right: 40, bottom: 30, left: 30 },
      width = 450 - margin.left - margin.right,
      height = 400 - margin.top - margin.bottom;

    const svg = d3
      .select("#area")
      .append("svg")
      .attr("width", width + margin.left + margin.right)
      .attr("height", height + margin.top + margin.bottom)
      .append("g")
      .attr("transform", `translate(${margin.left}, ${margin.top})`);

    const x = d3.scaleLinear().domain([0, 100]).range([0, width]);
    svg
      .append("g")
      .attr("transform", `translate(0, ${height})`)
      .call(d3.axisBottom(x));

    const y = d3.scaleLinear().domain([0, 100]).range([height, 0]);
    svg.append("g").call(d3.axisLeft(y));

    svg
      .selectAll("whatever")
      .data(DATA)
      .enter()
      .append("circle")
      .attr("cx", (d) => x(d.x))
      .attr("cy", (d) => y(d.y))
      .attr("r", 7);
  }, []);

  return (
    <div className="App">
      <svg id="area" height={400} width={500}></svg>
    </div>
  );
}
```

조금 분해해 봅시다. 산점도의 점을 렌더링하기 위해 `사용 효과` 콜백에서 이 코드를 추가했습니다.

```coffeescript
svg
  .selectAll("whatever")
  .data(DATA)
  .enter()
  .append("circle")
  .attr("cx", (d) => x(d.x))
  .attr("cy", (d) => y(d.y))
  .attr("r", 7);
```

우리는 `데이터` 방식으로 데이터를 읽습니다. DATA 어레이는 다음과 같습니다.

```cpp
const DATA = [
  { x: 10, y: 20 },
  { x: 20, y: 50 },
  { x: 80, y: 90 }
];
```

그런 다음 도메인에 상대적인 x-좌표와 y-좌표를 얻기 위해 점으로 x와 y-좌표를 사용한다. 이 값은 0에서 100 사이이며 `폭`과 `높이` 값에 비례하여 자동으로 크기가 조정됩니다.

따라서 x가 10이면 cx가 10 * (450-margin.left-margin.right)이고, y가 20이면 cy가 20 * (400-margin.top-margin)이 된다.맨 아래)의

## 데이터에서 그래프 만들기

D3의 가장 일반적인 사용 사례 중 하나는 데이터에서 그래프를 만드는 것입니다. 이를 위해 CSV에서 데이터를 읽을 수 있습니다.

예를 들어 `src` 폴더의 `data.csv` 파일에서 데이터를 읽을 수 있습니다.

```css
date,close
01-May-20,58.13
30-Apr-20,53.98
27-Apr-20,67.00
26-Apr-20,89.70
25-Apr-20,99.00
24-Apr-20,130.28
23-Apr-20,166.70
20-Apr-20,234.98
19-Apr-20,345.44
18-Apr-20,443.34
17-Apr-20,543.70
16-Apr-20,580.13
13-Apr-20,605.23
12-Apr-20,622.77
11-Apr-20,626.20
10-Apr-20,628.44
09-Apr-20,636.23
05-Apr-20,633.68
04-Apr-20,624.31
03-Apr-20,629.32
02-Apr-20,618.63
```

App.js에서는 다음과 같이 씁니다.

```js
import React, { useEffect } from "react";
import * as d3 from "d3";
import "d3-time-format";
const parseTime = d3.timeParse("%d-%b-%y");

const createGraph = async () => {
  const margin = { top: 20, right: 20, bottom: 50, left: 70 },
    width = 960 - margin.left - margin.right,
    height = 500 - margin.top - margin.bottom;
  const x = d3.scaleTime().range([0, width]);
  const y = d3.scaleLinear().range([height, 0]);

  const valueLine = d3.line()
    .x((d) => { return x(d.date); })
    .y((d) => { return y(d.close); });

  const svg = d3.select("body").append("svg")
    .attr("width", width + margin.left + margin.right)
    .attr("height", height + margin.top + margin.bottom)
    .append("g")
    .attr("transform", `translate(${margin.left}, ${margin.top})`);

  let data = await d3.csv(require("./data.csv"))

  data.forEach((d) => {
    d.date = parseTime(d.date);
    d.close = +d.close;
  });

  data = data.sort((a, b) => +a.date - +b.date)

  x.domain(d3.extent(data, (d) => { return d.date; }));
  y.domain([0, d3.max(data, (d) => { return d.close; })]);

  svg.append("path")
    .data([data])
    .attr("class", "line")
    .attr("d", valueLine);

  svg.append("g")
    .attr("transform", `translate(0, ${height})`)
    .call(d3.axisBottom(x));

  svg.append("g")
    .call(d3.axisLeft(y));
}

export default function App() {
  useEffect(() => {
    createGraph();
  }, []);

  return (
    <div>
      <style>{
        `
          .line {
            fill: none;
            stroke: steelblue;
            stroke-width: 2px;
          }
      `}
      </style>
    </div>
  );
}
```

그래프 생성 코드를 `graph 생성` 함수로 이동시킨다. 또한 npm id3-time-format을 실행하여 d3-time-format 라이브러리를 추가했습니다. 그러면 이 파일에 있는 것처럼 가져올 수 있습니다.

우리는 앞의 예와 비슷한 방법으로 축을 만들었습니다. "x"와 "y" 함수를 이와 같이 만들어 나중에 축을 만들 수 있도록 합니다.

```cpp
const x = d3.scaleTime().range([0, width]);
const y = d3.scaleLinear().range([height, 0]);
```

그래프 컨테이너는 다음을 사용하여 생성됩니다.

```js
const svg = d3.select("body").append("svg")
  .attr("width", width + margin.left + margin.right)
  .attr("height", height + margin.top + margin.bottom)
  .append("g")
  .attr("transform", `translate(${margin.left}, ${margin.top})`);
```

그런 다음 CSV 데이터를 다음과 같이 읽습니다.

```js
let data = await d3.csv(require("./data.csv"))
```

시간을 구문 분석하고 다음을 사용하여 `닫기` 속성을 숫자로 변경합니다.

```coffeescript
data.forEach((d) => {
  d.date = parseTime(d.date);
  d.close = +d.close;
});
```

그런 다음 데이터를 날짜별로 다음과 같이 정렬합니다.

```coffeescript
data = data.sort((a, b) => +a.date - +b.date)
```

다음 코드를 사용하여 라인을 추가합니다.

```css
svg.append("path")
  .data([data])
  .attr("class", "line")
  .attr("d", valueLine);
```

그리고 각각 x축과 y축을 추가합니다.

```js
svg.append("g")
  .attr("transform", `translate(0, ${height})`)
  .call(d3.axisBottom(x));
```

```css
svg.append("g")
  .call(d3.axisLeft(y));
```

마지막으로, `App` 구성 요소에는 다음과 같이 씁니다.

```xml
<style>{
  `
    .line {
      fill: none;
      stroke: steelblue;
      stroke-width: 2px;
    }
`}
```

이걸로 채워진 모양 대신 선을 볼 수 있습니다.

## 결론

방금 보신 것처럼, 우리는 D3.js로 쉽게 그래픽을 만들 수 있습니다. 리액트 앱에서 잘 작동하며 별도의 작업 없이 통합할 수 있습니다.