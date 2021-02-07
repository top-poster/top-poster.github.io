---
layout: post
title: "Web Workers를 통한 인터페이스 응답성 향상 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/improve-interface-responsiveness-web-workers-api-nocdn.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/improve-interface-responsiveness-web-workers-api-nocdn.png?fit=730%2C487&ssl=1)

JavaScript는 단일 스레드이므로 실행되는 모든 JavaScript도 웹 페이지의 응답을 차단합니다. 사용자가 UI를 효과적으로 인식할 수 없을 정도로 코드가 빠르게 실행되기 때문에 이는 많은 경우에 문제가 되지 않습니다.

그러나 코드가 계산 비용이 많이 들거나 사용자의 하드웨어에 전력이 부족한 경우에는 심각한 문제가 될 수 있다.

## 웹 작업자

이 문제를 완화하는 한 가지 방법은 백그라운드 스레드에 작업을 오프로드하여 주 스레드에 많은 작업을 수행하지 않는 것입니다. 안드로이드나 iOS와 같은 다른 플랫폼들은 메인 스레드가 가능한 한 적은 수의 UI로 동작하도록 하는 것의 중요성을 강조한다.

Web Workers API는 Android 및 iOS 백그라운드 스레드에 해당하는 웹입니다. 브라우저의 97% 이상이 근로자를 지원하고 있다.

## 데모

문제 및 해결 방법을 시연할 데모를 만들어 보겠습니다. 또한 GitHub에서 최종 결과와 소스 코드를 볼 수 있습니다. 맨 뼈인 index.html부터 시작하겠습니다.

```xml
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Web Worker Demo</title>
    <script src="./index.js" async></script>
  </head>
  <body>
    <p>The current time is: <span id="time"></span></p>
  </body>
</html>
```

다음으로 `index.js`를 추가하여 지속적으로 시간을 업데이트하여 `21:45:08.345`와 같이 표시하겠습니다.

```js
// So that the hour, minute, and second are always two digits each
function padTime(number) {
  return number < 10 ? "0" + number : number;
}

function getTime() {
  const now = new Date();
  return (
    padTime(now.getHours()) +
    ":" +
    padTime(now.getMinutes()) +
    ":" +
    padTime(now.getSeconds()) +
    "." +
    now.getMilliseconds()
  );
}

setInterval(function () {
  document.getElementById("time").innerText = getTime();
}, 50);
```

간격을 50밀리초 값으로 설정하면 시간이 매우 빠르게 업데이트됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/s_2B1D39163F68BF012F7E2E562D0C8D85EE12DB93AC548B1B538FD80767F90560_1599436104511_clock-1.gif?resize=730%2C110&ssl=1)

### 서버 설정

그런 다음 npm init 또는 yarn init으로 Node.js 프로젝트를 시작하고 Parcl을 설치합니다. 우리가 택배를 사용하고자 하는 첫 번째 이유는 크롬에서 작업자는 로컬 파일에서 로드되는 것이 아니라 서비스를 받아야 하기 때문입니다.

따라서 나중에 작업자를 추가할 때 Chrome을 사용하는 경우 index.html을 열 수 없습니다. 두 번째 이유는 Parcel이 데모 구성이 필요 없는 Web Workers API를 기본으로 지원하기 때문입니다. 웹 팩과 같은 다른 번들러는 더 많은 설정이 필요합니다.

패키지에 시작 명령을 추가할 것을 제안합니다.json:

```undefined
{
  "scripts": {
    "start": "parcel serve index.html --open"    
  }
}
```

이렇게 하면 `npm start` 또는 `yarn start`를 실행하여 파일을 작성하고, 서버를 시작하고, 브라우저에서 페이지를 열고, 원본 파일을 변경할 때 페이지를 자동으로 업데이트할 수 있습니다.

### 이미지큐

이제 계산적으로 비용이 많이 드는 것을 추가해 보겠습니다.

이미지-q를 설치하겠습니다. 이미지-q는 지정된 이미지의 주 색상을 계산하여 이미지로부터 색상 팔레트를 만듭니다.

예는 다음과 같습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/crab.png?resize=730%2C665&ssl=1)

본문을 업데이트하겠습니다.

```xml
<body>  
  <div class="center">
    <p>The current time is: <span id="time"></span></p>

    <form id="image-url-form">
      <label for="image-url">Direct image URL</label>
      <input
        type="url"
        name="url"
        value="https://upload.wikimedia.org/wikipedia/commons/1/1f/Grapsus_grapsus_Galapagos_Islands.jpg"
      />
      <input type="submit" value="Generate Color Palette" />
      <p id="error-message"></p>
    </form>
  </div>

  <div id="loader-wrapper" class="center">
    <div id="loader"></div>
  </div>

  <div id="colors-wrapper" class="center">
    <div id="color-0" class="color"></div>
    <div id="color-1" class="color"></div>
    <div id="color-2" class="color"></div>
    <div id="color-3" class="color"></div>
  </div>

  <a class="center" id="image-link" target="_blank">
    <img id="image" crossorigin="anonymous" />
  </a>
</body>
```

그래서 우리는 이미지에 직접 링크를 걸 수 있는 양식을 추가하고 있습니다. 그런 다음, 처리 중에 회전 애니메이션을 표시하는 로더가 있습니다. 우리는 그것을 구현하기 위해 이 코드펜을 조정할 것이다. 색상 팔레트를 표시하는 데 사용할 4개의 디브도 있습니다. 마지막으로 이미지 자체를 표시합니다.

일부 인라인 스타일을 `헤드`에 추가합니다. 여기에는 회전 로더를 위한 CSS 애니메이션이 포함됩니다.

```xml
<style type="text/css">
  .center {
    display: block;
    margin: 0 auto;
    max-width: max-content;
  }

  form {
    margin-top: 25px;
    margin-bottom: 25px;
  }

  input[type="url"] {
    display: block;
    padding: 5px;
    width: 320px;
  }

  form * {
    margin-top: 5px;
  }

  #error-message {
    display: none;
    background-color: #f5e4e4;
    color: #b22222;
    border-radius: 5px;
    margin-top: 10px;
    padding: 10px;
  }

  .color {
    width: 80px;
    height: 80px;
    display: inline-block;
  }

  img {
    max-width: 90vw;
    max-height: 500px;
    margin-top: 25px;
  }

  #image-link {
    display: none;
  }

  #loader-wrapper {
    display: none;
  }

  #loader {
    width: 50px;
    height: 50px;
    border: 3px solid #d3d3d3;
    border-radius: 50%;
    border-top-color: green;
    animation: spin 1s ease-in-out infinite;
    -webkit-animation: spin 1s ease-in-out infinite;
  }

  @keyframes spin {
    to {
      -webkit-transform: rotate(360deg);
    }
  }
  @-webkit-keyframes spin {
    to {
      -webkit-transform: rotate(360deg);
    }
  }

  #error-message {
    display: none;
    background-color: #f5e4e4;
    color: #b22222;
    border-radius: 5px;
    margin-top: 10px;
    padding: 10px;
  }
</style>
```

`index.js` 업데이트:

```js
import * as iq from "image-q";

// Previous code for updating the time

function setPalette(points) {
  points.forEach(function (point, index) {
    document.getElementById("color-" + index).style.backgroundColor =
      "rgb(" + point.r + "," + point.g + "," + point.b + ")";
  });

  document.getElementById("loader-wrapper").style.display = "none";
  document.getElementById("colors-wrapper").style.display = "block";
  document.getElementById("image-link").style.display = "block";
}

function handleError(message) {
  const errorMessage = document.getElementById("error-message");
  errorMessage.innerText = message;
  errorMessage.style.display = "block";
  document.getElementById("loader-wrapper").style.display = "none";
  document.getElementById("image-link").style.display = "none";
}

document
  .getElementById("image-url-form")
  .addEventListener("submit", function (event) {
    event.preventDefault();

    const url = event.target.elements.url.value;
    const image = document.getElementById("image");

    image.onload = function () {
      document.getElementById("image-link").href = url;

      const canvas = document.createElement("canvas");
      canvas.width = image.naturalWidth;
      canvas.height = image.naturalHeight;
      const context = canvas.getContext("2d");
      context.drawImage(image, 0, 0);
      const imageData = context.getImageData(
        0,
        0,
        image.naturalWidth,
        image.naturalHeight
      );

      const pointContainer = iq.utils.PointContainer.fromImageData(imageData);
      const palette = iq.buildPaletteSync([pointContainer], { colors: 4 });
      const points = palette._pointArray;
      setPalette(points);
    };

    image.onerror = function () {
      handleError("The image failed to load. Please double check the URL.");
    };

    document.getElementById("error-message").style.display = "none";
    document.getElementById("loader-wrapper").style.display = "block";
    document.getElementById("colors-wrapper").style.display = "none";
    document.getElementById("image-link").style.display = "none";

    image.src = url;
  });
```

설정 팔레트는 색상 디브의 배경색을 설정하여 팔레트를 표시합니다. 또한 이미지가 로드되지 않을 경우 `handle Error` 기능도 있습니다.

그리고 나서, 우리는 양식 제출에 귀를 기울입니다. 새로운 제출을 받을 때마다 이미지 요소의 `온로드` 기능을 설정하여 이미지 데이터를 `image-q`에 적합한 형식으로 추출한다.

그래서 우리는 이미지 데이터 개체를 검색할 수 있도록 캔버스에 이미지를 그립니다.

우리는 이 개체를 image-q에 전달하고 계산 비용이 많이 드는 부분인 iq.buildPaletteSync라고 부른다. 세트 팔레트에게 전달한 네 가지 색상을 반환합니다.

또한 적절하게 요소를 숨기거나 숨깁니다.

### 문제

색상표를 생성해 보십시오. image-q가 처리되는 동안 시간이 업데이트를 중지합니다. URL 입력을 클릭하려고 하면 UI도 응답하지 않습니다. 그러나 회전 애니메이션은 여전히 작동할 수 있습니다. CSS 애니메이션을 별도의 컴포지터 스레드로 대신 처리할 수 있다는 설명이다.

Firefox에서는 브라우저에 다음과 같은 경고가 표시됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/warning.png?resize=730%2C85&ssl=1)

빠른 컴퓨터를 사용하는 경우 CPU가 작업을 빠르게 처리할 수 있기 때문에 문제가 명확하지 않을 수 있습니다. 느린 디바이스를 시뮬레이션하기 위해 개발자 도구 설정이 있는 Chrome을 사용하여 CPU를 조정할 수 있습니다.

성능 탭을 연 다음 해당 설정을 열어 옵션을 표시합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/4-x-slowdown.png?resize=730%2C90&ssl=1)

### 작업자 추가

응답하지 않는 UI를 수정하려면 작업자를 사용합니다. 먼저 양식에 작업자를 사용할지 여부를 나타내는 확인란을 추가합니다. 제출 입력 전에 이 HTML을 추가하십시오.

```xml
<input type="checkbox" name="worker" />
<label for="worker"> Use worker</label>
<br />
```

다음으로 작업자를 `index.js`로 설정합니다. 작업자에 대한 브라우저 지원이 널리 보급되어 있지만 `if(창)`를 사용하여 기능 탐지 검사를 추가해 보겠습니다.근로자) `혹시나`

```js
let worker;
if (window.Worker) {
  worker = new Worker("worker.js");
  worker.onmessage = function (message) {
    setPalette(message.data.points);
  };
}
```

온메시지 방법은 작업자로부터 데이터를 받는 방법입니다.

그런 다음 확인란이 선택되어 있을 때 작업자를 사용하도록 이미지 `온로드` 핸들러를 변경합니다.

```undefined
// From before
const imageData = context.getImageData(
    0,
    0
    image.naturalWidth,
    image.naturalHeight
);

if (event.target.elements.worker.checked) {
    if (worker) {
        worker.postMessage({ imageData });
    } else {
        handleError("Your browser doesn't support Web Workers.");
    }
    return;
}

// From before
const pointContainer = iq.utils.PointContainer.fromImageData(imageData);
```

작업자의 postMessage 방법은 작업자에게 데이터를 보내는 방법입니다.

마지막으로 worker.js에서 worker 자체를 생성해야 합니다.

```js
import * as iq from "image-q";

onmessage = function (e) {
  const pointContainer = iq.utils.PointContainer.fromImageData(
    e.data.imageData
  );
  const palette = iq.buildPaletteSync([pointContainer], { colors: 4 });
  postMessage({ points: palette._pointArray });
};
```

현재도 온메시지와 포스트메시지를 사용하고 있지만 온메시지는 index.js에서 메시지를 수신하고 포스트메시지는 index.js로 메시지를 전송합니다.

작업자와 팔레트를 생성해 보십시오. 처리 중에 시간이 계속 업데이트됩니다. 그 형태는 또한 얼지 않고 상호 작용한다.

## 결론

Web Workers API는 특히 웹 사이트가 대부분 정적 데이터의 표시라기보다는 응용 프로그램에 가깝다고 생각할 때 웹 사이트의 반응을 더 좋게 만드는 효과적인 방법이다. 앞서 살펴본 바와 같이 작업자를 설정하는 작업은 매우 간단할 수 있으므로 CPU 집약적인 코드를 식별하여 작업자로 이동하는 것은 쉬운 일이 될 수 있습니다.

노동자들은 주로 DOM에 접근할 수 없는 제약 조건을 가지고 있습니다. 일반적인 사고방식은 비용이 많이 드는 작업을 근로자에게 이전하면서 DOM 업데이트를 포함하여 주요 스레드가 UI에 최대한 집중하도록 하는 것이어야 한다. 이 방법이 적절할 때 이렇게 하면 사용자에게 정지하지 않고 지속적으로 사용하기 좋은 인터페이스를 제공할 수 있습니다.