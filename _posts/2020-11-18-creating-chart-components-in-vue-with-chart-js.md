---
layout: post
title: "Chart.js를 사용하여 Vue에서 차트 구성 요소 생성"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/vue-chart.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/vue-chart.png?fit=730%2C487&ssl=1)

프런트 엔드 개발자가 차트를 가지고 작업해야 하는 것은 거의 불가피하다. 대시보드의 데이터 분석을 보여주는 것부터 웹 사이트의 간단한 정보에 사용하는 것까지, 시각적으로 만족스러운 형식으로 데이터를 표시해야 하는 시나리오가 발생할 가능성이 높습니다.

Chart.js는 개발자들이 간단하고 유연한 차트를 만들 수 있도록 도와주는 자바스크립트 라이브러리이다.

이 짧은 튜토리얼에서는 Chart.js 라이브러리를 사용하여 Vue 앱의 차트 구성 요소를 생성하는 방법을 보여 드리겠습니다.

### 시작하기

막대 및 선 차트를 렌더링할 수 있는 차트 구성 요소를 만듭니다.

> 참고: 시작하기 전에 시스템에 Node.js, Vue 및 Vue CLI가 설치되어 있어야 합니다. 여기서 Node.js를 설치하고 Vue CLI 설치 지침에 따라 설치할 수 있습니다.

먼저 새로운 Vue 프로젝트를 만들어 보겠습니다.

```undefined
vue create vue-chart
```

사전 설정을 선택하거나 자신에게 맞는 옵션을 선택할 수 있습니다.
다음 단계는 프로젝트에 Charts.js를 추가하는 것입니다.

```undefined
cd vue-chart
npm install chart.js --save
```

프로젝트를 실행하려면 다음 명령을 사용하십시오.

```coffeescript
npm run serve
```

이 작업을 마친 후에는 src/components 내부의 기본 HelloWorld.vue 구성 요소와 앱에서 이 구성 요소와 관련된 모든 사항을 삭제합니다.vue 성분

## 차트 구성 요소 생성

차트 작성을 시작하려면 구성 요소 폴더에 구성 요소를 생성해 보겠습니다. 구성 요소의 이름을 "Chart.vue"로 지정할 수 있습니다.
Chart.js를 사용하기 위해 템플릿에 필요한 것은 캔버스 요소입니다.

```undefined
<template>
   <div>
     <canvas></canvas>
   </div>
</template>
```

Chart.js를 스크립트 태그에 넣어 구성 요소로 가져옵니다.

```coffeescript
import Chart from "chart.js";
```

다음으로 차트에 대한 `건설자` 방법을 만들겠습니다. 이 함수는 템플릿에서 캔버스 요소를 가져온 다음 `차트` 클래스의 새 인스턴스를 만듭니다. 그런 다음 캔버스(canvas) 요소를 첫 번째 주장으로, 개체(object) 요소를 두 번째 주장으로 통과시킨다. 잠시 후에 그 물체에 도착하겠습니다.

```js
chartConstructor(chartType, chartData, chartOptions) {
    const chartElement = document.querySelector("canvas");
    const chart = new Chart(chartElement, {
    type: chartType,
    data: chartData,
    options: chartOptions,
  });
},
```

두 번째 인수로 전달하는 객체는 세 가지 속성인 `type`, `data`, `options`가 있는데, 이 속성들의 값은 함수의 매개 변수입니다.

Chart.js에서는 개체에 필요한 3가지 주요 속성이 위에 나와 있습니다.

`유형`은 단순히 어떤 종류의 차트를 만들고 싶은지 알려주는 문자열입니다. 예를 들어 "라인", "도넛", "바" 등이 있습니다.

데이터(Data)는 차트 값의 레이블 배열인 두 가지 속성 또는 레이블을 사용하는 개체입니다.

다른 하나는 각 데이터 세트에 대해 개체를 가져오는 배열인 `데이터 세트`입니다. 문서에서 취할 수 있는 일부 속성을 확인할 수 있습니다.

다음은 꺽은선형 차트에서 `data` 객체가 어떻게 보일 수 있는지 보여주는 예입니다.

```css
data: {
    labels: ["Jan1", "Jan2", "Jan3", "Jan4", "Jan5", "Jan6", "Jan7"],
    datasets: [
      {
        label: "This week",
        data:  [12, 19, 10, 17, 6, 3, 7],
        backgroundColor: "rgba(224, 248, 255, 0.4)",
        borderColor: "#5cddff",
        lineTension: 0,
        pointBackgroundColor: "#5cddff",
      },
      {
        label: "Last week",
        data:  [10, 25, 3, 25, 17, 4, 9],
        backgroundColor: "rgba(241, 225, 197, 0.4)",
        borderColor: "#ffc764",
        lineTension: 0,
        pointBackgroundColor: "#ffc764",
      },
    ],
  },
```

`옵션`은 애니메이션, 레이아웃, 범례 등과 같은 차트에 대한 구성을 설정하는 데 사용됩니다.

우리의 구성 요소에서는 구성 요소가 데이터를 수신할 소품을 만들어 차트 생성자에게 전달할 것입니다.

```css
props:{
  chartType:String,
  chartData:Object,
  chartOptions:Object
},
```

구성 요소가 마운트되면 차트 생성자에게 전화를 걸어 소품에서 데이터를 제공하겠습니다.

```undefined
mounted(){
  let {chartType,chartData,chartOptions} = this;
  this.chartConstructor(chartType, chartData, chartOptions);
}
```

이것으로 우리의 차트 컴포넌트는 완성되었다. 어디든 수입할 수 있고 필요한 자료를 소품에 넘길 수 있고, 우리 차트가 살아날 것입니다.

### 예

다음은 선 차트의 전체 예입니다.

차트 구성 요소를 가져오고 소품과 함께 다음과 같이 선언합니다.

```js
<template>
  <div class="chart">
    <Chart 
    :chartData="chartData" 
    :chartOptions="chartOptions" 
    :chartType="chartType" />
  </div>
</template>
```

데이터 내에서는 `차트 데이터`에 가치를 부여합니다.

```undefined
chartType: "line",
```

당사의 `차트 데이터`는 다음과 같은 방식으로 나타납니다.

```css
chartData: {
        labels: 
          ["Jan1", "Jan2", "Jan3", "Jan4", "Jan5", "Jan6",  "Jan7"],
        datasets: [
          {
            label: "This week",
            data: [12, 19, 10, 17, 6, 3, 7],
            backgroundColor: "rgba(224, 248, 255, 0.4)",
            borderColor: "#5cddff",
            lineTension: 0,
            pointBackgroundColor: "#5cddff",
          },
          {
            label: "Last week",
            data: [10, 25, 3, 25, 17, 4, 9],
            backgroundColor: "rgba(241, 225, 197, 0.4)",
            borderColor: "#ffc764",
            lineTension: 0,
            pointBackgroundColor: "#ffc764",
          },
        ],
      },
```

실제 애플리케이션에서 데이터셋 객체의 데이터는 API에서 나올 수 있습니다.

마지막으로 `차트 옵션`은 다음과 같습니다.

```undefined
chartOptions: {
        scales: {
          xAxes: [
            {
              stacked: true,
              gridLines: { display: false },
            },
          ],
          yAxes: [
            {
              ticks: {
                stepSize: 1,
                callback: function(value, index, values) {
                  if (value % Math.round(values[0] / 6) == 0) {
                    return value;
                  } else if (value === 0) {
                    return value;
                  }
                },
              },
              // stacked: true
            },
          ],
        },
        maintainAspectRatio: false,
        legend: {
          labels: {
            boxWidth: 10,
          },
          position: "top",
        },
        animation: {
          duration: 2000,
          easing: "easeInOutQuart",
        },
      },
```

> 참고: 여기 보시는 가치들은 제가 선호하는 것들입니다. 자유롭게 실험하고 다른 것들을 시도해 보세요.

## 결론

우리가 구축한 것은 단순한 차트 구성요소입니다. 더 많은 것을 추가하여 보다 유연하고 복잡해질 수 있으며, 이를 통해 더 많은 가치를 창출할 수 있습니다.

이것이 여러분의 프로젝트에 놀라운 차트를 추가하는 데 도움이 되기를 바랍니다.

모든 것을 보고 다른 예제를 자세히 보려면 이 보고서를 참조하십시오.