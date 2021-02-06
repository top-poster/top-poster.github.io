---
layout: post
title: "RoughViz를 사용하여 Vue.js에서 스케치된 차트 시각화"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/roughViz-sketch.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/roughViz-sketch.png?fit=730%2C487&ssl=1)

## 도입

차트는 데이터 세트를 더 쉽게 읽을 수 있고 데이터 세트의 부분을 더 쉽게 구별하는 데 사용되는 데이터의 그래픽 표현이다. 대부분의 사용자는 간결하고 공식화된 차트를 보는 데 익숙하지만 일부 사용자는 손으로 그리거나 스케치한 차트를 선호합니다. 거기가 바로 거친 비즈가 나오는 곳이야.

RoughViz는 D3.js와 Rough.js 위에 구축된 자바스크립트 라이브러리이다. 라이브러리는 아래 예제와 같이 스케치하거나 손으로 그린 것처럼 보이는 차트를 쉽게 작성할 수 있도록 설계되었습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/skteched-graphs.gif?resize=537%2C288&ssl=1)

이 가이드에서는 vue-roughviz를 사용하여 Vue.js 응용 프로그램에서 스케치 같은 차트를 표시하는 방법과 vue-cli를 사용하여 Vue 응용 프로그램을 구성하는 방법에 대해 알아봅니다.

### 전제조건

이 튜토리얼에서는 다음과 같은 전제 조건을 가정합니다.

- Vue.js에 대한 기본 친숙도
- Node.js에 대한 로컬 개발 환경 및 Node 패키지 관리자(npm)에 대한 친숙함
- Visual Studio Code 또는 Atom과 같은 텍스트 편집기

## 프로젝트 설정

vue-cli를 아직 설치하지 않은 경우 다음 명령을 실행하여 최신 버전을 설치하십시오.

```coffeescript
npm install -g @vue/cli
# OR
yarn global add @vue/cli
```

이제 새 Vue 애플리케이션을 생성합니다.

```undefined
vue create my-app
```

참고: 이 프로세스는 몇 분 정도 걸릴 수 있습니다. 작업이 완료되면 새 애플리케이션 루트 디렉터리로 이동할 수 있습니다.

```bash
cd my-app
```

위에서 자세히 설명한 프로세스는 새 Vue.js 애플리케이션을 생성합니다. 모든 것이 잘 설정되었는지 확인하려면 npm run serve를 실행한다. http://localhost:8080을 방문하면 브라우저에 "Welcome to Your Vue.js 앱 페이지"가 표시됩니다.

### vue-rough viz 추가

vue-roughviz는 roughViz.js의 Vue.js 래퍼이다. 이렇게 하면 Vue.js 기반 프로젝트에서 원활한 재사용이 가능한 구성 요소로 라이브러리에 액세스할 수 있습니다.

프로젝트에 vue-roughviz를 포함하려면 다음을 실행하십시오.

```coffeescript
npm install vue-roughviz
```

### Viz 구성 요소

vue-roughviz는 다음과 같은 모든 Viz의 차트 스타일을 위한 구성요소를 제공합니다.

- `rough Bar` – 대략적인 Viz 막대 차트 구성 요소
- 러프 바H` – 대략적인 Viz 수평 막대 차트 구성 요소
- 러프 도넛 – 러프 비즈 도넛 차트 구성 요소
- 거친 파이 – 거친 비즈 파이 차트
- `roughLine` – 대략적인 Viz 라인 차트 구성 요소
- "RoughScatter" – 대략적인 Viz 산포 차트 구성 요소
- "Rough Stacked Bar" – 대략적인 Viz 스택형 막대 차트 구성 요소

### 사용법

프로젝트에 `Vue-rough viz`를 추가한 후 다음 단계는 원하는 텍스트 편집기에서 프로젝트 폴더를 여는 것입니다.

`src/App.vue` 파일을 열 때 초기 내용은 아래 이미지와 유사해야 합니다.

보기 내용이 위와 같으면 내용을 모두 삭제하고 다음 코드로 바꾸십시오.

```xml
<template>
  <div id="app">
    <rough-bar
      :data="{
        labels: ['North', 'South', 'East', 'West'],
        values: [10, 5, 8, 3],
      }"
      title="Regions"
      roughness="8"
      :colors="['red', 'orange', 'blue', 'skyblue']"
      stroke="black"
      stroke-width="3"
      fill-style="cross-hatch"
      fill-weight="3.5"
      class="d-inline"
    />
  </div>
</template>

<script>
import { RoughBar } from "vue-roughviz";
export default {
  name: "App",
  components: {
    RoughBar,
  },
};
</script>
```

#### 코드설명

- 수입...` – 이 줄은 앞에서 설치한 vue-rough viz에서 roughBar 구성요소를 가져오기 위한 것입니다.
- `export default {} - 이 블록은 이전에 가져온 구성 요소(roughBar)를 앱에서 사용할 수 있도록 하기 위한 것입니다.
- `<brack-bar:data="[...]" />` – 여기에서 외부 roughBar 구성 요소를 호출합니다. 이러한 구성 요소에 지정된 속성은 필수 항목입니다. 단일 파일 구성 요소를 잘 모르는 경우 이 가이드를 참조할 수 있습니다.

#### 부에 장식된 소품

필요한 소품은 차트를 구성할 데이터를 가리키는 데이터뿐이다. 문자열 또는 개체일 수 있습니다.

개체를 선택하는 경우 개체에는 레이블 및 값 키가 포함되어야 합니다. 대신 문자열을 사용하는 경우 문자열은 csv 또는 tsv 파일의 URL이어야 합니다. 또한 이 파일 내에서 각 열을 나타내는 별도의 속성으로 레이블과 값을 지정해야 합니다.

기타 유용한 소품은 다음과 같습니다.

- `➡` – 차트 제목을 지정합니다.
- `경직성` – 차트의 거칠기 수준
- 막대 스트로크의 색
- `스트로크 폭` – 막대 스트로크의 크기
- `채우기 가중치` – 내부 경로 색상의 가중치를 지정합니다.
- `채우기 스타일` – 막대 채우기 스타일(다음 중 하나일 수 있음)
➡➡➡➡➡➡➡➡➡➡ ➡
실속 있는
지그재그 선
십자수
`해처`
지그재그
- ➡➡➡➡➡➡➡➡➡➡ ➡
- 실속 있는
- 지그재그 선
- 십자수
- `해처`
- 지그재그

지원되는 모든 소품 목록을 보려면 여기를 방문하십시오.

### 뛰다

앱을 미리 보려면 `npm run serve`를 실행하십시오. 위의 단계를 올바르게 따른 경우 http://localhost:8080을 방문하면 브라우저에 표시되는 차트를 볼 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/sketched-bar-graph.gif?resize=600%2C369&ssl=1)

## 외부 API에서 데이터 로드

대신 지난 열흘간 비트코인의 가격 이력을 차트에 표시하는 작은 실험을 해보자. 이 실험을 위해 우리는 코인체코 API를 활용할 것입니다.

왜 코인체코야? 코인체코는 다른 암호화폐 API와 달리 무료이며 시작하기 위해 API 키가 필요하지 않아 우리의 실험에 이상적이다.

계속해서 `src/App.vue`를 아래 코드로 교체합니다.

```xml
<template>
  <div id="app">
    <div class="d-inline-block">
      <rough-bar
        v-if="chartValue.length > 0"
        :data="{
          labels: chartLabel,
          values: chartValue,
        }"
        title="BTC - 10 Days"
        roughness="3"
        color="#ccc"
        stroke="black"
        stroke-width="1"
        fill-style="zig-zag"
        fill-weight="2"
      />
    </div>
  </div>
</template>

<script>
import { RoughBar } from "vue-roughviz";
export default {
  name: "App",
  components: {
    RoughBar,
  },
  data() {
    return {
      chartLabel: [],
      chartValue: [],
    };
  },
  methods: {
    async loadData() {
      await fetch(
        "https://api.coingecko.com/api/v3/coins/bitcoin/market_chart?vs_currency=usd&days=10&interval=daily"
      )
        .then((res) => res.json())
        .then((rawData) => {
          console.table(rawData.prices);
          rawData.prices.map((data) => {
            let date = new Date(data[0]).toDateString();
            let rPrice = data[1];
            console.log(`Price of 1btc on ${date} is ${rPrice}`);
            this.chartLabel.push(date);
            this.chartValue.push(rPrice);
          });
        })
        .catch((err) => console.error("Fetch error -> ", err));
    },
  },
  beforeMount() {
    this.loadData();
  },
};
</script>
```

새로운 변화는 coingecko API에서 비트코인 가격 이력을 가져와 반환된 데이터를 순환하는 비동기식 방법인 loadData()를 만들고 반환된 날짜를 차트 레이블로, 가격을 차트 값으로 사용하여 가격에서 날짜를 분리한 것입니다. 그리고 앱이 뷰에 마운트되기 전에 beforeMount() 즉, 이전에 생성한 load Data() 기능을 호출했습니다.

앱을 실행하면 다음과 같이 차트의 새로운 변경 사항을 확인할 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/external-API-chart.gif?resize=516%2C530&ssl=1)

## 결론

이 자료에서는 차트와 그 용도를 설명하고, vue-cli를 사용하여 Vue 앱을 만드는 과정을 안내했으며, vue-roughviz를 사용하여 Vue.js 응용 프로그램에서 스케치 같은 차트를 표시하는 방법을 보여 주었다.

## 추가 탐사

만약 여러분이 거친 비즈에 대해 더 많이 배우거나 기여하는 것에 관심이 있다면, GitHub에 있는 거친 비즈나 뷰-러비즈 프로젝트를 방문하세요. 이 튜토리얼 소스 코드는 GitHub에서도 사용할 수 있습니다.