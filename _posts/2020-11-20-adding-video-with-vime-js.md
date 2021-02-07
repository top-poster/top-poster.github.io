---
layout: post
title: "Vime.js를 사용하여 비디오 추가"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/videowithvime.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/videowithvime.png?fit=730%2C487&ssl=1)

## 도입

Vime.js는 최신 프레임워크에 구애받지 않는 미디어 플레이어이다. 유튜브, Vimeo, Dailymotion, HTML5 등과 같은 비디오 호스팅 서비스를 지원합니다. HTML MediaElement를 사용하여 미디어 파일을 로드, 재생 및 제어할 수 있습니다. 따라서 로컬 비디오 파일 추가를 지원합니다.

자체 호스팅된 비디오에는 성능 문제, 스토리지 문제 등 여러 가지 단점이 있습니다. 또한 웹 비디오에 대한 단일 파일 형식 표준은 없으며, 따라서 서로 다른 주요 웹 브라우저가 서로 다른 형식을 지원한다.

파이어폭스는 Ogg나 WebM 동영상을 지원하지만 H.264는 지원하지 않는다. 사파리는 H.264(MP4) 동영상을 지원하지만 웹M이나 오그 등은 지원하지 않는다. 다행히 크롬은 모든 주요 비디오 포맷을 지원한다. 이러한 기능은 YouTube 또는 Vimeo와 같은 비디오 공유 플랫폼을 사용하여 조정할 수 있습니다. 그러나 이 방법에는 한계도 있다.

여러 플랫폼의 비디오를 원하는 시나리오에서는 플랫폼마다 다른 로직을 작성해야 할 수도 있습니다. 결과적으로, 우리의 코드는 건조하지 않다. Vime.js는 이러한 문제를 해결합니다.

다양한 비디오 호스팅 플랫폼과 상호 작용하는 번거로움을 없애고 사용하기 쉬운 API를 제공합니다. 이것은 우리가 사용하고 싶은 비디오 호스팅 플랫폼의 수에 관계없이, 우리는 Vime.js만 배우면 된다는 것을 의미한다.

또한 Vime.js는 크로스 브라우저 호환성 문제를 처리합니다. 또한 훨씬 더 확장 가능하고, 사용자 지정이 가능하며, 액세스할 수 있습니다.

우리는 Vime.js의 중요성과 왜 우리가 그것을 즉시 채택해야 하는지 알게 되었다. 흥미롭게도, 우리는 그것이 할 수 있는 일의 표면을 거의 긁지 않았다. 그 기능은 다음과 같습니다.

- 모든 플랫폼에 대한 API(하나의 API를 학습하여 모든 플랫폼에 사용)
- 다중 비디오 호스팅 플랫폼(Youtube, Vimeo, HTML5, Dailymotion 등) 지원
- 교차 브라우저 호환성 문제 없음(브라우저 차이 해결)
- 모바일 및 데스크톱 플랫폼 모두에 대한 지원 제공
- 가볍고 성능이 뛰어난 제품
- 확장 가능(Vime 구축 및 확장)
- CSS 모듈(테밍 및 스타일을 사용자 지정할 수 있음)
- TypeScript를 사용하여 빌드됨
- React, Vue, Svelte, Stencil 및 Angular에 대한 프레임워크별 바인딩

## Vime.js 시작하기

### 설치

우선 Vime.js는 프레임워크에 구애받지 않는 라이브러리이다. 반응, 각도, 뷰 및 스벨트 프로젝트에 연결할 수 있습니다. 이는 또한 각 프레임워크마다 설치가 다르다는 것을 의미합니다.

설치는 npm이 담당하기 때문에 문제가 되지 않는다. 그러나 각 npm 스크립트의 뉘앙스를 알아채는 것이 급선무다.

이 게시물에서는 React와 HTML5의 두 가지 사용 사례에 초점을 맞추겠습니다. 그러나 필요한 링크는 다른 프레임워크의 문서에 맡기겠습니다.

## CDN

서로 다른 `npm` 설치 스크립트의 nicrety를 처리하지 않으려면 Vime.js를 사용하는 것이 가장 간단합니다. 최상의 성능을 위해 `CDN` jsDelivr을 사용하는 것이 좋습니다.

HTML 5 문서의 `<head> 태그 안에 이 코드를 넣기만 하면 됩니다.

```xml
<!-- Default theme. ~960B -->
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/@vime/core@^4/themes/default.css"
/>

<!-- Optional light theme (extends default). ~400B -->
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/@vime/core@^4/themes/light.css"
/>

<!-- Library and all of its components are lazy loaded, so nothing to sweat about here. ~3kB -->
<script
  type="module"
  src="https://cdn.jsdelivr.net/npm/@vime/core@^4/dist/vime/vime.esm.js"
></script>
```

## 반응하다

먼저 CSS를 `root` 구성 요소에 추가하여 로드합니다.

```java
// Default theme. ~960B
import '@vime/core/themes/default.css';

// Optional light theme (extends default). ~400B
import '@vime/core/themes/light.css';
```

실행:

```coffeescript
npm i @vime/react
```

다른 프레임워크에 대한 링크는 다음과 같습니다.

- 부에
- 앵글
- 스벨트
- Stencil(여기에서 Stencil에 대해 자세히 알아보기)

## Vime.js 코어

Vime.js에는 두 가지 핵심 구성 요소가 있으며, 이 구성 요소는 플레이어, 공급자 및 UI 구성 요소입니다.

각자 알아서 처리합시다.

### 플레이어 구성 요소

오토플레이와 쇼 컨트롤 등 플레이어의 상태를 그대로 유지하는 구성 요소다. 현재 시간이나 기간 등 미디어 재생의 상태도 갖고 있다.

루트 구성 요소로 자리 잡고 UI 및 공급자 구성 요소를 래핑합니다. 또한 플레이어의 속성을 설정하고, 이벤트를 수신하고, 통화 방법을 호출하기 위해 인터페이스할 기본 구성 요소입니다.

자세한 정보, 이벤트 및 방법을 보려면 여기를 클릭하십시오.

다음은 간단한 플레이어 구성요소 구현입니다.

```xml
<vime-player>
    <!-- Provider component is placed here. -->
    <!-- UI components are placed here. -->
</vime-player>
```

### 제공자 구성 요소

플레이어 구성요소가 직접 상호작용하는 유일한 구성요소입니다. 미디어 공급자 어댑터 인터페이스를 사용하여 이 작업을 수행합니다. 공급자는 플레이어/매체를 로드하고 제어할 책임이 있습니다.

예를 들어 유튜브 프로바이더는 유튜브 플레이어를 내장해 동영상을 로딩한다. 그런 다음 플레이어 구성 요소를 통해 이 비디오를 제어할 수 있습니다.

아래의 `Youtube provider`에 대한 코드 샘플을 고려하십시오.

```xml
<vime-player controls>
  <vime-youtube video-id="DyTCOwB0DVw"></vime-youtube>
  <!-- ... -->
</vime-player>
```

위의 예에서, 우리는 "vime-youtube" 제공자가 "video-id"라는 속성을 가지고 있음을 알 수 있다. 이 속성은 `리소스`를 보유합니다.로드할 비디오의 "ID"입니다.

VIM-Youtube에 대해 자세히 알아보십시오.

### UI 구성 요소

이러한 구성 요소는 상호 작용할 수 있는 미디어 플레이어에서 볼 수 있는 구성 요소입니다. 좋은 예는 스피너이다.

아래 코드를 고려하십시오.

```xml
<vime-player>
  <!-- ... -->
  <vime-ui>
    <!-- ... -->
    <vime-spinner></vime-spinner>
  </vime-ui>
</vime-player>
```

위의 코드 샘플은 UI에 스핀너를 던진다. 이것은 사용자 경험을 향상시키는 데 사용되는 편리한 유틸리티입니다.

아래 이미지에는 동영상이 버퍼링될 때 로드 표시기로 사용되는 스피너가 나와 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/spinnerloadingindicator.jpg?resize=730%2C356&ssl=1)

## Vime.js 작업

이 섹션에서는 Vime.js를 사용하여 비디오를 로드하고 재생하며 style.css를 사용자 지정합니다.

비디오 플레이어에 대한 전체 인터페이스를 설정하는 것은 매우 어려운 일이므로 간단한 React.js 설정부터 시작하겠습니다.

앞에서 언급한 것처럼 Vime은 프레임워크에 구애받지 않으므로 이 섹션에서는 React.js와 함께 작업하겠습니다. Vime.js API는 프레임워크 전반에 걸쳐 일관적이기 때문에 우리는 한 번 배우고 어디에서나 사용할 수 있다. 또한 각 프레임워크의 구현 간 유사성은 매우 작기 때문에 번거로움 없이 다른 프레임워크로 쉽게 전환할 수 있습니다.

## Vime.js 사용

아래 코드를 고려하십시오.

```js
import "./styles.css";
import React from "react";
import { VimePlayer, VimeYoutube } from "@vime/react";
export default function APP() {
  return (
    <VimePlayer controls>
      <VimeYoutube videoId="1GFbKYQhDMU" />
      {/* ... */}
    </VimePlayer>
  );
}
```

위의 조작된 예는 `Vime`을 사용하여 LogRocket의 YouTube 채널에서 비디오를 로드합니다.유튜브 제공 업체. 여기서 데모를 볼 수 있습니다.

좀 더 재미있는 일을 해봅시다.

```undefined
import React from 'react';
import { VimePlayer, VimeVideo } from '@vime/react';

function Player() {
  return (
    <VimePlayer controls>
      <VimeVideo crossOrigin="" poster="https://media.vimejs.com/poster.png">
        <source
          data-src="https://media.vimejs.com/720p.mp4"
          type="video/mp4"
        />
        <track
          default
          kind="subtitles"
          src="https://media.vimejs.com/subs/english.vtt"
          srcLang="en"
          label="English"
        />
      </VimeVideo>

      {/* ... */}
    </VimePlayer>
  );
}
```

위의 예에서는 VimeVideo 구성 요소를 사용하여 이미지를 로드하고 있습니다. 이 구성 요소를 사용하면 HTML 5 비디오 요소를 사용하여 비디오를 로드, 재생 및 제어할 수 있습니다. 우리의 예에서 "크로스 오리진"과 "포스터"와 같은 몇 가지 속성을 필요로 한다. 이러한 속성은 기본 HTML 5 요소에 전달됩니다.

위의 예에서 VimeVideo 구성 요소에는 두 개의 하위 HTML 5 미디어 요소가 있습니다. 이 요소는 `source` 요소와 `track` 요소입니다. HTML 5 소스 요소에서 "data-src"를 속성으로 전달하여 "게을 로딩"을 허용하고, 일반 "src"도 사용할 수 있다.

트랙 요소는 비디오의 자막을 처리하는 데 사용됩니다.

또한 이 구성 요소에 제어 속성을 전달했습니다. 이 속성은 재생, 볼륨, 전체 화면 등과 같은 플레이어 `컨트롤`을 켭니다. 여기서 코드를 확인할 수 있습니다.

## 스타일링 및 테마링

박스 밖으로 나온 Vime.js는 밝고 어두운 테마와 함께 나온다. 테마 속성을 플레이어 구성 요소에 전달하면 이러한 속성을 전환할 수 있습니다.

```xml
<!-- Default is 'dark'. -->
<vime-player theme="light">
  <!-- ... -->
</vime-player>
```

스타일링을 위해 Vime.js는 구성 요소에 사용자 지정 CSS 속성을 사용합니다. 다음은 사용 가능한 `선택기` 목록입니다.

```css
vime-player.mobile {
  /* Add styles here for when the player is loaded on a mobile device. */
}

vime-player.live {
  /* Add styles here for when the media is a live stream. */
}

vime-player.audio {
  /* Add styles here for when the media is of type `audio`. */
}

vime-player.video {
  /* Add styles here for when the media is of type `video`. */
}

/* You can replace 'light' with 'dark' or any custom theme name you'd like. */
vime-player[theme="light"] {
  /* Add styles here for when the theme is set to `light`.  */
}
```

스타일링을 위한 더 많은 구성 요소별 속성을 여기서 찾을 수 있습니다.

또한 사용자 정의 클래스 이름 또는 ID를 추가하여 아래 이미지에 표시된 것처럼 스타일을 지정할 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/stylescssforvime.png?resize=730%2C363&ssl=1)

여기서 코드를 사용합니다.

## 결론

의심할 여지 없이, Vime.js는 놀라운 라이브러리입니다. 그리고 우리가 논의했던 것보다 훨씬 더 많은 것을 할 수 있다. 그러나 이 게시물을 사용하면 번거로움 없이 Vime.js를 사용할 수 있습니다. 좀 더 고급스러운 것을 원하시면, 공식 문서를 확인해 보시기를 권하고 싶습니다.