---
layout: post
title: "웹 작업자를 사용하여 미디어 로드 최적화"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/optimized-media-loading-web-workers.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/optimized-media-loading-web-workers.png?fit=730%2C487&ssl=1)

지금은 2020년이고, 우리는 올해 우리의 상당한 문제들 이상을 가지고 있습니다. 그러나 개발자들이 수십 년 동안 겪어온 일관된 문제는 미디어를 웹 애플리케이션에 효율적으로 로드하는 방법이다.

게으른 로딩, 압축, 대역폭을 기반으로 한 동적 미디어 선택 등과 같은 다양한 기술을 사용하여 이러한 문제를 해결하기 위한 몇 가지 실험과 학습이 있었지만, 여전히 앱 성능과 사용자 경험에 심각한 손상을 줄 수 있는 경우가 몇 가지 있다.

이 기사에서는 약 1,000개의 이미지(유효한 이미지와 유효하지 않은 이미지 모두)의 콜라주를 구축하는 기술에 대해 논의하고, 그 과정에서 다양한 접근 방식의 문제, 일부 해결책 및 장단점에 대해 논의한다.

다음 기본 설정을 고려해 보겠습니다. `index.html`은 클릭 시 이미지 로드를 시작할 수 있는 버튼과 브라우저가 정지되었을 때 성능을 보여주는 타이머(`setInterval` 포함)가 있는 간단한 웹 페이지입니다.

```js
//index.html

<html>
    <head>
        <title>Optimize media loading with Web workers | LogRocket</title>
        <link rel="stylesheet" href="styles.css">
    </head>
    <body>
        <div>
            <div class="box">
                <button id="start" onclick="start()">Start</button>
                <div id="count"></div>
            </div>
            <div id="collage"></div>
        </div>
    </body>
    <script>
        setInterval(() => {
            const count = document.getElementById("count")
            const today = new Date();
            const time = today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();
            count.innerHTML = time.toString();
        }, 100)
        </script>
</html>
```

images.js는 로드할 이미지의 URL 배열입니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/images-js-array.png?resize=730%2C487&ssl=1)

이 문제에 대한 세 가지 접근 방식을 살펴보겠습니다. 즉, DOM에 이미지 추가, 약속 사용, Web Workers 사용입니다.

## DOM에 이미지 추가

이러한 모든 이미지를 추가하는 간단한 방법 중 하나는 URL 배열을 통해 반복하고 모든 URL에 대해 새 DOM 요소를 만든 다음 DOM에 추가하는 것입니다. 이 접근 방식은 주 스레드를 차단하고 나쁜 사용자 환경도 만듭니다. 즉, 빈번한 DOM 변경으로 인한 성능 문제는 언급하지 않습니다.

다음은 코드와 작동 방법의 예입니다.

```js
// Function to append images into the DOM
const start = () => {
        const container = document.getElementById("collage")
        images.forEach(url => {
            const image = document.createElement("img");
            image.src = url;
            container.appendChild(image)
        });
    }
```

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/adding-images-dom.gif?resize=730%2C399&ssl=1)

보시다시피 위의 접근 방식에서 유효한 이미지 URL과 유효하지 않은 이미지 URL이 모두 DOM에 추가되어 성능에 영향을 미칩니다(타이머 지연에 주의). 문서 조각 만들기 기능을 이용하면 조금 더 나아질 수 있지만 크게 달라지지는 않는다.

이것은 매우 나쁜 접근법임이 입증되었고 우리에게 더 나은 것을 찾도록 강요했다. 즉, 약속들이 접근한다.

## 약속 사용

이러한 상황을 처리하기 위한 더 나은 해결책은 이러한 이미지를 비동기식으로 로드하여 DOM에 한 번에 삽입하는 것이다. 우리는 약속을 사용하여 `이미지()` API로 이미지를 비동기식으로 로드할 수 있다. 이미지() 생성자에 대한 자세한 내용은 여기에서 확인할 수 있습니다.

이 접근법에서, 우리는 URL 배열을 반복하고 `이미지` API에 로드된 각 URL로 약속을 만든다. 그런 다음 각각 영상과 null로 분해되는 onload 및 onerror 기능을 노출한다. 코드는 다음과 같습니다.

```js
  const imagesPromiseArray = urlArray.map(url => {
    return new Promise((resolve, reject) => {
      const img = new Image();
      img.onload = () => {
        resolve(img);
      };
      img.onerror = () => {
        resolve(null);
      };
      img.src = url;
    });
  });
```

일단 우리가 일련의 이미지 약속을 갖게 되면, 우리는 이제 그것들을 `약속`으로 해결할 수 있다.모두 약속으로 돌려주다 여기서는 잘못된 이미지에 대한 `null`로 이미지 약속을 해결하기 때문에 유효한 이미지만 필터링 및 반환하고 잘못된 이미지는 무시한다.

```js
return new Promise((resolve, reject) => {
    Promise.all(imagesPromiseArray).then(images => {
      resolve(images.filter(Boolean));
    });
  });
```

모든 것을 종합하면:

```js
//resolve-images.js

const resolveImages = urlArray => {
  const imagesPromiseArray = urlArray.map(url => {
    return new Promise((resolve, reject) => {
      const img = new Image();
      img.onload = () => {
        resolve(img);
      };
      img.onerror = () => {
        resolve(null);
      };
      img.src = url;
    });
  });

  return new Promise((resolve, reject) => {
    Promise.all(imagesPromiseArray).then(images => {
      resolve(images.filter(Boolean));
    });
  });

};
```

시작 기능에서는 이미지를 하나씩 추가하는 대신 이 약속을 사용하고 모든 유효한 이미지를 DOM에 한 번에 추가해야 한다. 시작 기능은 다음과 같습니다.

```coffeescript
const start = () => {
      const imageFragment = document.createDocumentFragment();
      const container = document.getElementById("collage")
       resolveImages(images).then((imgs) => {
          imgs.forEach((img) => {
              imageFragment.appendChild(img)
          });
          container.appendChild(imageFragment)
      }, () => {})
}
```

동작의 변경 사항:

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/adding-images-promises.gif?resize=480%2C384&ssl=1)

위의 gif에서 보시면 성능과 사용자 경험이 훨씬 더 좋습니다. 이제 사용자가 Start(시작) 버튼을 클릭하면 이미지 로딩이 백그라운드에서 시작되고 잠시 후 모든 유효한 이미지가 화면에 로드됩니다.

그러나 한 가지 문제가 눈에 띈다. 시작 버튼을 클릭하자마자 6시 14분 4초에 상당 시간 카운터가 정지한다. 거대한 이미지 목록을 한꺼번에 처리해야 하는 등 브라우저가 정지돼 있기 때문이다. 실제 응용 프로그램에서는 응용 프로그램의 다른 부분들도 주 스레드와 결합되기 때문에 훨씬 더 심각할 것입니다.

따라서, 이러한 접근 방식은 더 나아 보일 수 있지만, 여전히 충분하지 않습니다. 이것은 Web Workers API로 이어집니다.

## 웹 작업자 사용

자바스크립트는 단일 스레드 언어이므로 데이터 집약적인 작업이 수행될 때 위의 예에서 버튼 클릭 후처럼 브라우저를 정지시킨다.

그러나 우리는 Web Workers API를 사용하여 멀티스레딩의 이점을 활용하여 메인 스레드를 망치지 않도록 할 수 있습니다. 그것이 바로 우리의 경우에서 그 문제를 해결하기 위해 우리가 할 것입니다. Web Workers API에 대한 자세한 내용은 여기에서 확인할 수 있습니다.

단계는 다음과 같이 간단합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/web-worker-process.png?resize=730%2C378&ssl=1)

코드로 구현해 봅시다. 첫 번째 단계는 `image-worker.js`라는 새 파일을 만드는 것이다.

```js
self.addEventListener(
  "message",
  async function(e) {
    const urls = e.data;
    const images = await Promise.all(
      urls.map(async url => {
        try {
          const response = await fetch(url);
          const fileBlob = await response.blob();
          if (fileBlob.type === "image/jpeg")
            return URL.createObjectURL(fileBlob);
        } catch (e) {
          return null;
        }
      })
    );
    self.postMessage(images);
  },
  false
);
```

여기서, 우리는 URL 배열을 반복하고, URL을 가져와 블럽으로 변환하고, 유효한 이미지 블럽 배열을 반환한다. image() API는 img 요소로 변환되며, 웹 작업자가 DOM 액세스를 지원하거나 허용하지 않기 때문에 사용할 수 없습니다.

다음 단계는 아래와 같이 `이미지 확인` 기능에서 웹 작업자를 사용하는 것입니다.

```cpp
const worker = new Worker("image-worker.js");
```

주 스레드와 웹 작업자는 포스트 메시지 기능을 사용하여 통신합니다. 따라서, 우리는 `postMessage`를 통해 일련의 이미지 URL을 웹 작업자에게 전달할 것입니다.

```css
worker.postMessage(urlArray);
```

작업자가 URL을 처리하고 이미지 블럽을 주 스레드로 다시 전송한 후에는 아래와 같이 이벤트 수신기가 필요합니다.

```js
worker.addEventListener(
      "message",
      async function(event) {
        const imagePromises = event.data.map(async url => {
          if (url) {
            return await createImage(url);
          }
        });
        const imageElements = await Promise.all(imagePromises);
        resolve(imageElements.filter(Boolean));
      },
      false
    );
```

여기서는 이미지 블롭을 얻은 후 `createImage` 함수에서 `Image() API를 사용하여 이미지 구성 요소를 구축하고 이전 접근 방식과 동일한 단계를 반복한다.

```js
const createImage = url => {
    return new Promise((resolve, reject) => {
      const img = new Image();
      img.onload = () => {
        resolve(img);
      };
      img.onerror = () => {
        resolve(null);
      };
      img.src = url;
    });
  };
```

이 모든 것을 종합하면, `resolveImages.js`는 다음과 같이 보인다.

```js
const resolveImages = urlArray => {
  const createImage = url => {
    return new Promise((resolve, reject) => {
      const img = new Image();
      img.onload = () => {
        resolve(img);
      };
      img.onerror = () => {
        resolve(null);
      };
      img.src = url;
    });
  };
  return new Promise((resolve, reject) => {
    const worker = new Worker("image-worker.js");
    worker.postMessage(urlArray);
    worker.addEventListener(
      "message",
      async function(event) {
        const imagePromises = event.data.map(async url => {
          if (url) {
            return await createImage(url);
          }
        });
        const imageElements = await Promise.all(imagePromises);
        resolve(imageElements.filter(Boolean));
      },
      false
    );
  });
};
```

이 접근 방식은 약속 기반 접근 방식의 모든 이점을 제공하며 모든 작업을 주 스레드에서 웹 작업자로 이동시켰기 때문에 브라우저가 정지되는 것을 방지한다. 아래 gif에서는 이미지가 부드럽게 로드되는 것을 볼 수 있으며 타이머가 전혀 멈추거나 지연되지 않습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/adding-images-web-workers.gif?resize=480%2C140&ssl=1)

## 결론

따라서 Web Workers API의 도움으로 미디어 로드를 성공적으로 최적화했습니다. 우리는 노동자들의 힘을 활용하여 웹 개발의 세계에 존재하는 많은 문제들을 해결할 수 있습니다. 그리고 이것은 그것을 위한 하나의 활용 사례입니다. 이에 대한 더 나은 접근법이나 아이디어를 찾을 수 있는지 의견을 제시합니다.