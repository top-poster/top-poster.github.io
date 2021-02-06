---
layout: post
title: "Visx 소개"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/visx-logo.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/visx-logo.png?fit=730%2C487&ssl=1)

Visx는 에어비앤비가 개발한 리액트 기반 데이터 시각화 도구 모음이다. 시각화 구성요소를 의미하는 Visx는 시각화 라이브러리가 아니라 프로젝트의 요구 사항에 따라 사용자 지정 시각화 라이브러리를 만들기 위해 함께 사용할 수 있는 기본 요소 또는 구성 요소의 모음입니다.

널리 사용되는 D3 시각화 라이브러리를 기반으로 구축되었지만 Visx는 DOM 조작 작업을 React 라이브러리에 위임합니다. 한편, D3는 계산에 주로 사용됩니다. 이것의 장점은 두 개의 라이브러리가 DOM을 제어하기 위해 고군분투할 때 슬그머니 들어올 수 있는 버그를 완화시킨다는 것이다.

Visx는 의견이 맞지 않으므로 아키텍처에 관계없이 모든 Retact 앱에 적합할 수 있습니다. 그것은 또한 배우기가 쉽다는 것을 의미하는 순수한 반응이다. 또한 Visx 도구 상자에서 필요한 항목을 항상 가져올 수 있으므로 코드베이스를 작고 안정적으로 유지할 수 있습니다.

Visx는 매우 안정적이고 신뢰할 수 있다. 3년여 전 Airbnb에서 시각화 스택을 전사적으로 통합한다는 유일한 목표로 개발이 시작되었습니다. 에어비앤비에서 2년 넘게 내부적으로 사용 중입니다.

## 다른 시각화 라이브러리와의 비교

에어비앤비 엔지니어링 팀에 따르면, Visx의 주요 목표는 성능, 학습 능력, 표현력이라고 합니다. 이것은 매우 드문 조합으로, 이 기사를 작성할 때 다른 반응 기반 프런트 엔드 시각화 라이브러리에 존재하지 않는다.

React에서 표현적 시각화를 구축하는 데 있어 D3는 수의사이지만 학습 곡선이 매우 가파르다. 리액트 개발자는 리액트에서의 이전 경험을 바탕으로 리액트 개발자를 가동하고 실행할 수 없습니다. React-vis와 같은 학습 가능성 문제를 해결하는 다른 시각화 도구도 있지만, 이는 표현력과 때로는 성능을 희생시키는 결과를 초래합니다. 따라서, Visx는 Airbnb의 목표 없는 엔지니어링 능력을 보여주는 것이 아닙니다. 시각화 응용 프로그램을 작성하는 React 엔지니어에게 실제로 큰 문제를 해결합니다.

### Visx 시작

Visx를 시작하는 것은 매우 쉽습니다. Visx 구성 요소 및 해당 속성을 숙지해야 합니다. 이러한 구성 요소는 일반 반응 구성 요소입니다.

이 예에서는 파리, 런던, 버라인 등 유럽 3개 도시의 일평균 기온을 14일 동안 보여주는 수직 적층 막대 차트를 만들 것입니다. 코드는 Github에서 사용할 수 있다.

## 설치

설치 프로세스의 첫 번째 단계는 react-app 보일러판을 설치하는 것입니다. npx create-react-app visx-chart를 실행하여 시작하십시오.

다음 단계는 Visx 패키지를 설치하는 것입니다. Visx는 Primitive의 집합체이기 때문에 오늘 우리가 만들 차트의 구성 요소 역할을 할 꽤 많은 패키지가 설치될 것입니다.

프로젝트의 루트 디렉터리로 이동하여 아래 명령을 실행하여 패키지를 설치하십시오. 애플리케이션 패키지를 관리하기 위해 npm을 사용하는 경우 yarn add를 npm 설치로 교체하는 것을 항상 기억하십시오.

```undefined
yarn add @visx/shape @visx/group @visx/grid @visx/axis @visx/scale @visx/tooltip @visx/legend @visx/responsive
```

다음은 설치된 각 패키지의 용도를 요약한 것입니다.

@visx/shape는 데이터 시각화에 사용되는 다양한 형태의 도형을 포함한다. 이 예제 프로젝트의 경우 쌓인 세로 막대는 이 패키지에서 가져옵니다.

@visx/group은 다른 SVG 객체를 그룹화하는 데 사용되는 SVG <g/> 요소의 간단한 구현이다.

@visx/grid에는 차트의 격자선을 만드는 데 사용되는 구성 요소가 포함되어 있습니다.

@visx/axis`에는 차트용 축 그리기 작업에 사용되는 구성 요소가 포함되어 있습니다. 이 제품은 매우 유연하므로 사용자 정의할 수 있습니다.

Visx 설명서에 따르면 "@visx/scale"은 "그래프에 필요한 실제 픽셀 크기에 데이터 값을 매핑하는 데 도움이 되는 함수"의 모음이다.

@visx/tooltip은 시각화에 툴팁을 추가하는 것을 단순화하는 데 필요한 후크, 구성 요소 및 기타 유틸리티를 포함한다.

시각화에 다양한 유형의 범례를 추가하는 데 사용되는 구성 요소 집합 @visx/components.

패키지 설치 과정의 마지막 단계는 yarn added-time-format을 실행하여 데이터의 날짜와 시간을 원하는 형식으로 변환하는 데 도움이 되는 d3-time-format 패키지를 설치하는 것이다.

@visx/응답성`은 시각화를 응답성 있게 만든다.

## 차트 구성 요소

차트 구성 요소에는 마지막으로 수직으로 쌓인 막대 차트를 생성하기 위해 조합된 모든 로직 및 구성 요소가 포함됩니다. 응용 프로그램의 `src` 디렉토리에 `TemperatureBarStack.js`라는 파일을 만들고 아래 코드를 붙여 넣습니다.

```coffeescript
import React from "react";
import { BarStack } from "@visx/shape";
import { Group } from "@visx/group";
import { Grid } from "@visx/grid";
import { AxisBottom } from "@visx/axis";
import { scaleBand, scaleLinear, scaleOrdinal } from "@visx/scale";
import { timeFormat, timeParse } from "d3-time-format";
import { useTooltip, useTooltipInPortal, defaultStyles } from "@visx/tooltip";
import { LegendOrdinal } from "@visx/legend";
import "./index.css";

const purple1 = "#6c5efb";
const purple2 = "#c998ff";
const purple3 = "#a44afe";
const background = "#eaedff";
const defaultMargin = { top: 40, right: 0, bottom: 0, left: 0 };
const tooltipStyles = {
  ...defaultStyles,
  minWidth: 60,
  backgroundColor: "rgba(0,0,0,0.9)",
  color: "white"
};

const data = [
  {
    date: "2020-10-01",
    London: "60",
    Paris: "68",
    Berlin: "61"
  },
  {
    date: "2020-10-02",
    London: "57",
    Paris: "58",
    Berlin: "62"
  },
  {
    date: "2020-10-03",
    London: "59",
    Paris: "57",
    Berlin: "72"
  },
  {
    date: "2020-10-04",
    London: "52",
    Paris: "59",
    Berlin: "68"
  },
  {
    date: "2020-10-05",
    London: "63",
    Paris: "57",
    Berlin: "63"
  },
  {
    date: "2020-10-06",
    London: "61",
    Paris: "62",
    Berlin: "61"
  },
  {
    date: "2020-10-07",
    London: "61",
    Paris: "64",
    Berlin: "61"
  },
  {
    date: "2020-10-08",
    London: "64",
    Paris: "66",
    Berlin: "60"
  },
  {
    date: "2020-10-09",
    London: "58",
    Paris: "62",
    Berlin: "60"
  },
  {
    date: "2020-10-10",
    London: "56",
    Paris: "59",
    Berlin: "55"
  },
  {
    date: "2020-10-11",
    London: "57",
    Paris: "58",
    Berlin: "52"
  },
  {
    date: "2020-10-12",
    London: "56",
    Paris: "58",
    Berlin: "54"
  },
  {
    date: "2020-10-13",
    London: "52",
    Paris: "56",
    Berlin: "55"
  },
  {
    date: "2020-10-14",
    London: "58",
    Paris: "57",
    Berlin: "51"
  }
];
const keys = ["London", "Paris", "Berlin"];

const temperatureTotals = data.reduce((allTotals, currentDate) => {
  const totalTemperature = keys.reduce((dailyTotal, k) => {
    dailyTotal += Number(currentDate[k]);
    return dailyTotal;
  }, 0);

  allTotals.push(totalTemperature);
  return allTotals;
}, []);

const parseDate = timeParse("%Y-%m-%d");
const format = timeFormat("%b %d");
const formatDate = (date) => format(parseDate(date));

const getDate = (d) => d.date;

const dateScale = scaleBand({ domain: data.map(getDate), padding: 0.2 });
const temparatureScale = scaleLinear({
  domain: [0, Math.max(...temperatureTotals)],
  nice: true
});
const colorScale = scaleOrdinal({
  domain: keys,
  range: [purple1, purple2, purple3]
});

let tooltipTimeout;

export default function TemperatureBarStack({
  width,
  height,
  event = false,
  margin = defaultMargin
}) {
  const {
    tooltipOpen,
    tooltipTop,
    tooltipLeft,
    hideTooltip,
    showTooltip,
    tooltipData
  } = useTooltip();

  const { containerRef, TooltipInPortal } = useTooltipInPortal();

  if (width < 10) return null;

  const xMax = width;
  const yMax = height - margin.top - 100;

  dateScale.rangeRound([0, xMax]);
  temparatureScale.range([yMax, 0]);

  return width < 10 ? null : (
    <div style={ position: "relative" }>
      <svg ref={containerRef} width={width} height={height}>
        <rect
          x={0}
          y={0}
          width={width}
          height={height}
          fill={background}
          rx={14}
        />
        <Grid
          top={margin.top}
          left={margin.left}
          xScale={dateScale}
          yScale={temparatureScale}
          width={xMax}
          height={yMax}
          stroke="black"
          strokeOpacity={0.1}
          xOffset={dateScale.bandwidth() / 2}
        />
        <Group top={margin.top}>
          <BarStack
            data={data}
            keys={keys}
            x={getDate}
            xScale={dateScale}
            yScale={temparatureScale}
            color={colorScale}
          >
            {(barStacks) =>
              barStacks.map((barStack) =>
                barStack.bars.map((bar) => (
                  <rect
                    key={`bar-stack-${barStack.index}-${bar.index}`}
                    x={bar.x}
                    y={bar.y}
                    height={bar.height}
                    width={bar.width}
                    fill={bar.color}
                    onClick={() => {
                      if (event) alert(`Clicked: ${JSON.stringify(bar)}`);
                    }
                    onMouseLeave={() => {
                      tooltipTimeout = window.setTimeout(() => {
                        hideTooltip();
                      }, 300);
                    }
                    onMouseMove={(event) => {
                      if (tooltipTimeout) clearTimeout(tooltipTimeout);
                      const top = event.clientY - margin.top - bar.height;
                      const left = bar.x + bar.width / 2;
                      showTooltip({
                        tooltipData: bar,
                        tooltipTop: top,
                        tooltipLeft: left
                      });
                    }
                  />
                ))
              )
            }
          </BarStack>
        </Group>
        <AxisBottom
          top={yMax + margin.top}
          scale={dateScale}
          tickFormat={formatDate}
          stroke={purple3}
          tickStroke={purple3}
          tickLabelProps={() => ({
            fill: purple3,
            fontSize: 11,
            textAnchor: "middle"
          })}
        />
      </svg>
      <div
        style={
          position: "absolute",
          top: margin.top / 2 - 10,
          width: "100%",
          display: "flex",
          justifyContent: "center",
          fontSize: 14
        }
      >
        <LegendOrdinal
          scale={colorScale}
          direction="row"
          labelMargin="0 15px 0 0"
        />
      </div>
      {tooltipOpen && tooltipData && (
        <TooltipInPortal
          key={Math.random()}
          top={tooltipTop}
          left={tooltipLeft}
          style={tooltipStyles}
        >
          <div style={ color: colorScale(tooltipData.key) }>
            <strong>{tooltipData.key}</strong>
          </div>
          <div>{tooltipData.bar.data[tooltipData.key]}℉</div>
          <div>
            <small>{formatDate(getDate(tooltipData.bar.data))}</small>
          </div>
        </TooltipInPortal>
      )}
    </div>
  );
}
```

위의 코드에 대한 설명은 다음과 같습니다.

미리 설치된 @visx 패키지와 시각화를 위한 CSS 스타일을 가져왔습니다.

```coffeescript
import React from "react";
import { BarStack } from "@visx/shape";
import { Group } from "@visx/group";
import { Grid } from "@visx/grid";
import { AxisBottom } from "@visx/axis";
import { scaleBand, scaleLinear, scaleOrdinal } from "@visx/scale";
import { timeFormat, timeParse } from "d3-time-format";
import { useTooltip, useTooltipInPortal, defaultStyles } from "@visx/tooltip";
import { LegendOrdinal } from "@visx/legend";
import "./styles.css";
```

여기서 시각화에 대한 중요한 스타일 속성이 정의됩니다.

또한 시각화할 데이터(즉, `데이터`)는 각 온도 값에 대한 레이블을 포함하는 `키` 배열과 함께 제공된다. 이러한 키는 스택형 차트의 적절한 세그먼트에 매핑되며 차트의 범례를 만드는 데도 사용됩니다.

```cpp
const purple1 = "#6c5efb";
const purple2 = "#c998ff";
const purple3 = "#a44afe";
const background = "#eaedff";
const defaultMargin = { top: 40, right: 0, bottom: 0, left: 0 };
const tooltipStyles = {
  ...defaultStyles,
  minWidth: 60,
  backgroundColor: "rgba(0,0,0,0.9)",
  color: "white"
};

const data = [...];
const keys = ["London", "Paris", "Berlin"];
```

여기 차트를 위해 데이터가 준비됩니다. 온도토탈스 어레이는 하루 평균 기온을 3개 도시의 평균 기온을 합한 값을 포함하고 있다. 세 도시를 가로지르는 일교량의 합이 하루 동안 쌓인 막대의 높이를 결정합니다.

형식 날짜() 기능은 날짜를 보다 읽기 쉬운 형식으로 변환하며, 이 시나리오에서는 `2020-10-14`를 10월 14일로 변경합니다.

```js
const temperatureTotals = data.reduce((allTotals, currentDate) => {
  const totalTemperature = keys.reduce((dailyTotal, k) => {
    dailyTotal += Number(currentDate[k]);
    return dailyTotal;
  }, 0);

  allTotals.push(totalTemperature);
  return allTotals;
}, []);

const parseDate = timeParse("%Y-%m-%d");
const format = timeFormat("%b %d");
const formatDate = (date) => format(parseDate(date));

const getDate = (d) => d.date;
```

@visx/scale은 d3-scale 패키지를 기반으로 한다. 여기에는 데이터를 가져와서 시각적 값을 반환하는 기능이 포함되어 있습니다. 아래 "스케일 밴드()"는 막대의 수평 특성, 즉 폭과 패딩을 관리합니다. 척도 선형()은 막대의 수직 속성을 처리합니다. 마지막으로, 스케일 Ordinal()은 개별 도시의 사전 정의된 색상에 대한 매핑을 처리한다. 여기서 d3의 척도에 대해 자세히 볼 수 있습니다.

```js
const dateScale = scaleBand({ domain: data.map(getDate), padding: 0.2 });
const temparatureScale = scaleLinear({
  domain: [0, Math.max(...temperatureTotals)],
  nice: true
});
const colorScale = scaleOrdinal({
  domain: keys,
  range: [purple1, purple2, purple3]
});

let tooltipTimeout;
```

이것은 수직으로 쌓인 막대 차트를 반환하는 반응 함수 구성 요소입니다. 가로, 높이, 높이, 이벤트, 여백 등 두 가지 소품이 부모사이즈(나중에 더 많이) 부품에서 전달될 예정이다.

또한 `use Tooltip`에는 툴팁의 물리적 특성 표시 설정에 도움이 되는 속성 및 기능이 포함되어 있다. 또한 포털에서 툴팁을 렌더링하는 useTooltipInPortal Hooks를 설정하였다.

```js
export default function TemperatureBarStack({
  width,
  height,
  event = false,
  margin = defaultMargin
}) {
  const {
    tooltipOpen,
    tooltipTop,
    tooltipLeft,
    hideTooltip,
    showTooltip,
    tooltipData
  } = useTooltip();

  const { containerRef, TooltipInPortal } = useTooltipInPortal();
  ...
}
```

여기서 너비가 최대 10px가 아닌 경우 구성 요소는 null을 반환하도록 설정됩니다.

또한:

```undefined
if (width < 10) return null;

  const xMax = width;
  const yMax = height - margin.top - 100;

  dateScale.rangeRound([0, xMax]);
  temparatureScale.range([yMax, 0]);
```

이것이 대부분의 원시 요소들이 사용되는 곳입니다. 시각화는 SVG를 기반으로 합니다. 따라서 차트에 상위 `SVG` 요소를 사용해야 합니다. 올바른 요소는 차트의 직사각형 배경을 관리합니다. 그리드 구성요소는 차트의 배경에 그려진 그리드 선을 담당합니다.

앞서 설명한 것처럼 그룹은 SVG 요소 집합 주위에 컨테이너를 만드는 데 도움이 됩니다. BarStack 구성 요소는 데이터가 수직으로 쌓인 막대 차트로 시각화되도록 렌더링합니다. 생성된 막대가 구독되는 이벤트도 관리합니다. AxisButton은 차트의 하단 축을 사용자 지정하는 데 사용됩니다.

`LegendOrdinal`은 차트 범례를 설정하는 데 사용되며 마지막으로 `TooltipInPortal` 구성 요소가 툴팁의 표시를 관리합니다.

```coffeescript
return width < 10 ? null : (
    <div style={ position: "relative" }>
      <svg ref={containerRef} width={width} height={height}>
        <rect
          x={0}
          y={0}
          width={width}
          height={height}
          fill={background}
          rx={14}
        />
        <Grid
          top={margin.top}
          left={margin.left}
          xScale={dateScale}
          yScale={temparatureScale}
          width={xMax}
          height={yMax}
          stroke="black"
          strokeOpacity={0.1}
          xOffset={dateScale.bandwidth() / 2}
        />
        <Group top={margin.top}>
          <BarStack
            data={data}
            keys={keys}
            x={getDate}
            xScale={dateScale}
            yScale={temparatureScale}
            color={colorScale}
          >
            {(barStacks) =>
              barStacks.map((barStack) =>
                barStack.bars.map((bar) => (
                  <rect
                    key={`bar-stack-${barStack.index}-${bar.index}`}
                    x={bar.x}
                    y={bar.y}
                    height={bar.height}
                    width={bar.width}
                    fill={bar.color}
                    onClick={() => {
                      if (event) alert(`Clicked: ${JSON.stringify(bar)}`);
                    }
                    onMouseLeave={() => {
                      tooltipTimeout = window.setTimeout(() => {
                        hideTooltip();
                      }, 300);
                    }
                    onMouseMove={(event) => {
                      if (tooltipTimeout) clearTimeout(tooltipTimeout);
                      const top = event.clientY - margin.top - bar.height;
                      const left = bar.x + bar.width / 2;
                      showTooltip({
                        tooltipData: bar,
                        tooltipTop: top,
                        tooltipLeft: left
                      });
                    }
                  />
                ))
              )
            }
          </BarStack>
        </Group>
        <AxisBottom
          top={yMax + margin.top}
          scale={dateScale}
          tickFormat={formatDate}
          stroke={purple3}
          tickStroke={purple3}
          tickLabelProps={() => ({
            fill: purple3,
            fontSize: 11,
            textAnchor: "middle"
          })}
        />
      </svg>
      <div
        style={
          position: "absolute",
          top: margin.top / 2 - 10,
          width: "100%",
          display: "flex",
          justifyContent: "center",
          fontSize: 14
        }
      >
        <LegendOrdinal
          scale={colorScale}
          direction="row"
          labelMargin="0 15px 0 0"
        />
      </div>
      {tooltipOpen && tooltipData && (
        <TooltipInPortal
          key={Math.random()}
          top={tooltipTop}
          left={tooltipLeft}
          style={tooltipStyles}
        >
          <div style={ color: colorScale(tooltipData.key) }>
            <strong>{tooltipData.key}</strong>
          </div>
          <div>{tooltipData.bar.data[tooltipData.key]}℉</div>
          <div>
            <small>{formatDate(getDate(tooltipData.bar.data))}</small>
          </div>
        </TooltipInPortal>
      )}
    </div>
  );
```

### 스타일링

`index.css` 파일의 CSS 코드를 아래 코드로 교체합니다.

```css
html,
body,
#root {
  height: 100%;
  line-height: 2em;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
    Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
}
```

### 대응성

Visx에는 시각화의 응답성을 높이는 유틸리티가 있습니다. 부모사이즈도 그중 하나다. 부모 구성 요소의 폭과 높이를 랩핑된 구성 요소로 전달합니다. 이 프로젝트를 구현하려면 `index.js` 파일의 내용을 아래 코드로 바꾸십시오.

```coffeescript
import React from "react";
import ReactDOM from "react-dom";
import ParentSize from "@visx/responsive/lib/components/ParentSize";

import TemperatureBarStack from "./TemperatureBarStack";

ReactDOM.render(
  
    {({ width, height }) => (
      
    )}
  ,
  document.getElementById("root")
);
```

yarn start를 사용하여 React 애플리케이션을 실행하는 것을 잊지 마십시오.

## 결론

Visx는 학습 능력과 표현력 때문에 데이터 시각화에 있어 React 개발자들에게 확실히 큰 변화를 가져다 줍니다. 그러나, 리액트와 함께 그것을 만들기로 결정한 것은 리액트가 에어비앤비의 주요 프런트 엔드 라이브러리라는 사실에 영향을 받은 것이 분명하기 때문에, 유사한 시각화 패키지가 개발되는 것을 보는 데는 영원히 걸릴 수 있다.