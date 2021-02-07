---
layout: post
title: "GPU.js로 JavaScript 성능 향상"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/intro-to-gpujs.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/intro-to-gpujs.png?fit=730%2C487&ssl=1)

복잡한 계산이나 계산을 실행하여 시간이 오래 걸리고 프로세스가 느려진다는 사실을 알아본 적이 있습니까?

웹 작업자 또는 배경 스레드를 사용하는 등 이 문제를 해결하는 방법은 여러 가지가 있습니다. GPU는 CPU의 처리 로드를 제거하여 CPU에 다른 프로세스를 위한 공간을 더 많이 제공합니다. 한편, 웹 작업자는 여전히 CPU에서 실행되지만 다른 스레드에서 실행됩니다.

이 초보자 가이드에서는 GPU.js를 사용하여 복잡한 수학 계산을 수행하고 JavaScript 앱의 성능을 향상시키는 방법을 시연한다.

## GPU.js란?

GPU.js는 그래픽 처리 장치(GPGPU)의 범용 프로그래밍을 위해 웹과 Node.js용으로 제작된 자바스크립트 가속 라이브러리이다. CPU가 아닌 GPU로 복잡하고 시간이 많이 소요되는 계산을 넘겨 더 빠른 계산 및 작업을 수행할 수 있습니다. 또한 Fallback 옵션도 있습니다. GPU가 시스템에 없는 상황에서는 기능이 일반 JavaScript 엔진에서 계속 실행됩니다.

시스템에서 수행해야 하는 복잡한 계산이 있을 경우 기본적으로 이러한 부담을 CPU 대신 시스템의 GPU로 전달하여 처리 속도와 시간을 높일 수 있습니다.

고성능 컴퓨팅은 GPU.js를 사용하는 주요 장점 중 하나이다. WebGL에 대한 사전 지식 없이 브라우저에서 병렬 계산을 시작하려는 경우 GPU.js가 라이브러리입니다.

## GPU.js를 사용해야 하는 이유

GPU를 사용하여 복잡한 계산을 수행해야 하는 이유는 셀 수 없이 많습니다. 한 게시물에서 탐색하기에는 너무 많습니다. 다음은 GPU 사용의 가장 주목할 만한 이점 중 몇 가지이다.

- GPU는 대규모 병렬 GPGPU 계산을 수행하는 데 사용될 수 있다. 비동기식으로 수행해야 하는 계산 유형입니다.
- 시스템에서 GPU를 사용할 수 없는 경우, 다시 JavaScript로 전환됩니다.
- GPU는 현재 브라우저와 Node.js에서 실행되며, 계산량이 많은 웹 사이트의 속도를 높이는 데 이상적입니다.
- GPU.js는 JavaScript를 염두에 두고 작성되므로 함수는 합법적인 JavaScript 구문을 사용합니다.

프로세서가 작업을 수행할 수 있고 GPU.js가 필요하지 않다고 생각되는 경우 이 GPU 및 CPU를 사용하여 계산을 실행한 다음 결과를 살펴보십시오.

보시다시피 GPU는 CPU보다 22.97배 더 빠릅니다.

## GPU.js의 작동 방식

그 정도의 속도를 감안하면 자바스크립트 생태계에 로켓이 주어진 셈이다. GPU는 웹 사이트, 특히 첫 페이지에서 복잡한 계산을 수행해야 하는 사이트를 훨씬 빠르게 로드하도록 지원합니다. GPU는 일반 CPU보다 22.97배 빠른 속도로 계산을 실행할 수 있으므로 더 이상 백그라운드 스레드 및 로더 사용에 대해 걱정할 필요가 없습니다.

gpu.createKernel 메서드는 JavaScript 함수에서 전송된 GPU 가속 커널을 생성합니다.

커널 기능을 GPU와 병렬로 실행하면 하드웨어에 따라 계산 속도가 1-15배 빨라집니다.

## GPU.js를 시작하는 중

GPU.js를 사용하여 복잡한 계산을 더 빨리 계산하는 방법을 보여주기 위해 빠르고 실용적인 데모를 회전시켜 보자.

설치:

```undefined
sudo apt install mesa-common-dev libxi-dev  // using Linux
```

npm:

```undefined
npm install gpu.js --save

// OR

yarn add gpu.js
```

노드 프로젝트에 GPU.js가 필요합니다.

```js
import { GPU } from ('gpu.js')

// OR
const { GPU } = require('gpu.js')

const gpu = new GPU();
```

## 곱셈 데모

아래 예에서 계산은 GPU에서 병렬로 수행됩니다.

먼저 대규모 데이터 세트를 생성합니다.

```js
  const getArrayValues = () => {

    //Create 2D arrary here
    const values = [[], []]

    // Insert Values into first array
    for (let y = 0; y < 600; y++){
      values[0].push([])
      values[1].push([])

      // Insert values into second array
      for (let x = 0; x < 600; x++){
        values\[0\][y].push(Math.random())
        values\[1\][y].push(Math.random())
      }
    }

    //Return filled array
    return values
  }
```

커널(GPU에서 실행되는 함수의 다른 단어)을 생성합니다.

```js
  const gpu = new GPU();

  // Using `createKernel()` method to multiply the array
  const multiplyLargeValues = gpu.createKernel(function(a, b) {
    let sum = 0;
    for (let i = 0; i < 600; i++) {
      sum += a\[this.thread.y\][i] * b\[i\][this.thread.x];
    }
    return sum;
  }).setOutput([600, 600])
```

행렬이 있는 커널을 매개 변수로 호출합니다.

```undefined
  const largeArray = getArrayValues()
  const out = multiplyLargeValues(largeArray[0], largeArray[1])
```

출력:

```undefined
console.log(out\[y\][x]) // Logs the element at the xth row and the yth column of the array
console.log(out\[10\][12]) // Logs the element at the 10th row and the 12th column of the output array
```

## GPU 벤치마크 실행

GitHub에 지정된 단계를 따라 벤치마크를 실행할 수 있습니다.

```undefined
npm install @gpujs/benchmark

const benchmark = require('@gpujs/benchmark')

const benchmarks = benchmark.benchmark(options);
```

옵션 개체에는 벤치마크에 전달할 수 있는 다양한 구성이 포함되어 있습니다.

공식 GPU.js 웹 사이트로 이동하여 계산의 전체 벤치마크를 확인합니다. 이렇게 하면 복잡한 계산을 위해 GPU.js를 사용하여 얼마나 많은 속도를 얻을 수 있는지 이해하는 데 도움이 됩니다.

## 결론

이 튜토리얼에서 우리는 GPU.js를 자세히 탐색하고, GPU의 작동 방식을 세분화하고, 병렬 계산을 수행하는 방법을 시연했다. 또한 Node.js 응용 프로그램에서 GPU.js를 설정하는 방법을 보여드렸습니다.

계속 코딩하세요!