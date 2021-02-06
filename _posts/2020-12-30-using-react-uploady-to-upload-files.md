---
layout: post
title: "React Uploady를 사용하여 파일 업로드"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/react-uploady-upload-files.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/react-uploady-upload-files.png?fit=730%2C487&ssl=1)

응용 프로그램을 만드는 동안 파일을 업로드할 수 있는 것은 필수적인 요구 사항입니다. 파일 업로드란 클라이언트측 사용자가 서버에 파일을 업로드할 수 있어야 함을 의미합니다. 각 파일 시스템에는 고유의 구현이 있기 때문에 이를 달성하기 위한 여러 가지 방법이 있습니다.

이 기사에서는 파일 업로드 시스템을 구축하여 Retact with Return Uploady에서 파일 업로드를 살펴봅니다.

## 리액션 업로디란?

공식 문서에 따르면, React-Uploady는 개발자들이 단 몇 줄의 코드로 클라이언트측 파일 업로드 기능을 구축할 수 있게 해주는 경량 라이브러리이다.

기본적으로, React 애플리케이션을 위한 최신 파일 업로드 구성 요소와 후크에 초점을 맞춘 라이브러리입니다. 또한 파일 업로드 진행률, 업로드 버튼 및 업로드를 위한 Enhancer와 같은 모든 파일 업로드를 위한 올인원 숍입니다.

## 전제조건

이 문서의 경우 다음이 필요합니다.

- 시스템에 설치된 Node.js 및 NPM
- JavaScript의 기본 지식 및 React.js의 작동 방식

먼저 공식 React 애플리케이션 비계 툴인 `create-react-app`을 설치합니다.

```undefined
npm install -g create-react-app
```

`create-react-app`을 실행하여 설치를 확인할 수 있습니다. 디렉토리 이름을 지정하라는 메시지가 나타납니다.

## React Uploady는 어떻게 작동합니까?

Reactuploady의 이면에 있는 아이디어는 사용하기 쉽고 사용자 정의도 가능해야 한다는 것입니다. 이를 위해 React Uploady는 이벤트 및 기능에 대한 구성 요소와 후크를 제공합니다. 이를 활용하여 간단한 업로드 시스템을 구축하겠습니다.

## React Uploady 시작

우리는 프로젝트를 부트스트랩하기 위해 `생성-반응-앱`을 사용할 것이다. `크리에이트-리액트-앱`은 개발자들이 단시간에 `리액트` 프로젝트를 시작할 수 있도록 페이스북과 커뮤니티가 관리하는 오픈소스 도구다.

`create-react-app` 상용판을 사용하여 새 프로젝트를 생성하려면 원하는 터미널에서 다음 명령을 실행하십시오.

```undefined
create-react-app react-uploady 
```

이 프로젝트의 프로젝트 이름에는 `react-Uploady`라는 이름이 사용되지만, 원하는 이름으로 대체될 수 있습니다.

그런 다음 프로젝트 디렉터리로 전환하고 다음을 실행하여 개발 서버를 시작하십시오.

```bash
cd react-uploady && npm start 
```

이 명령은 기본 보일러 플레이트 애플리케이션을 렌더링하는 브라우저 탭을 엽니다.

## 업로드 시스템 구축

React Uploady에는 업로드 버튼을 처리하는 UploadButton과 파일 업로드를 미리 볼 수 있는 Upload Preview와 같은 후크와 구성 요소가 포함되어 있습니다.

시스템을 시작하려면 먼저 터미널을 사용하여 다음 패키지를 설치하십시오.

```coffeescript
npm i @rpldy/upload-button @rpldy/uploady rc-progress
```

위의 코드에서 우리는 다음을 설치했다.

- `@rpldy/https-button`: 업로드 버튼에 대한 Return Uploady의 구성 요소입니다.
- `@rpldy/dify`: 응답 업로드 라이브러리의 기본 패키지입니다.
- `rc-progress`: 파일 업로드 진행률을 표시하는 패키지입니다.

다음으로, 우리 시스템을 위한 `헤더` 구성 요소를 만들어 봅시다. 이렇게 하려면 `src` 폴더에 `Header.js`라는 새 파일을 만드십시오. 파일 이름은 원하는 대로 지정할 수 있습니다.

다음으로, 우리는 아래의 것과 같은 기능적인 구성요소를 만듭니다.

```js
import React from "react";

export default function App() {
  return (
    <div>
      <h1>File upload with React Uploady</h1>
    </div>
  );
}
```

여기서 우리는 "Retact Uploady로 파일 업로드"라고 적힌 `h1` 태그를 가진 기능 구성요소를 만들었습니다.

다음으로, 아래의 앱 구성 요소에서 `헤더` 구성 요소를 렌더링해 보겠습니다.

```js
import React from "react";

export default function App() {
  return (
      <div className="App">
        <Header />
      </div>
      );
  }     
```

우리는 앱 컴포넌트에 헤더 컴포넌트를 렌더링했고, 우리는 아래 이미지와 유사한 결과를 브라우저에서 보아야 한다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/file-upload-react-uploady-example.png?resize=622%2C327&ssl=1)

다음으로, 우리의 `App.js` 컴포넌트에 우리의 업로드 시스템을 구축하고 유사한 간단한 업로드를 만들어 봅시다. 그러기 위해서는 패키지를 가져와 `업로드 버튼` 패키지에서 업로드 버튼을 렌더링하고 앱을 `업로드` 컴포넌트로 포장해야 합니다.

```undefined
import React, { useState } from "react";
import Header from "./Header";
import "./App.css";

import { Line } from "rc-progress";
import Uploady, { useItemProgressListener } from "@rpldy/uploady";
import UploadButton from "@rpldy/upload-button";
import { createMockSender } from "@rpldy/sender";
```

위의 코드에서, 우리는 `rc-progress` 패키지에서 `Line` 스타일의 파일 업로드를 가져왔습니다. 다음으로 업로드 및 진행률 수신기를 `@rpldy/Uploady`에서 가져온 다음 `@rpldy/Upload-button`에서 업로드 버튼을 가져왔습니다.

우리는 또한 테스트 목적으로 실제 보낸 사람을 대체하는 데 사용되는 `@rpldy/sender`에서 모의 보낸 사람을 수입했다.

다음으로, 먼저 파일 상태를 만들고 앱에서 파일 업로드 진행 상황을 구성 요소로 만들어 보겠습니다.

```js
const UploadProgress = () => {
  const [progress, setProgess] = useState(0);

  const progressData = useItemProgressListener();

  if (progressData && progressData.completed > progress) {
    setProgess(() => progressData.completed);
  }
```

위의 코드에서 업로드를 진행한다는 함수를 만들었습니다. 그런 다음 "setState" 후크를 사용하여 "progress"를 초기 상태로 초기화하고 이를 "0"으로 설정했다.

그 후, 우리는 리액트 업로디에서 얻은 progressData 변수를 use ItemProgress Listener로 설정했습니다. 여섯 번째 줄에서는 if라는 문구를 사용하여 진행률 데이터가 진행률보다 크면 완료로 설정한다고 했다.

파일 업로드에 대한 진행률 표시줄을 완료하려면 진행률 표시줄의 줄을 반환하고 다음을 스타일링합니다.

```js
 return (
    progressData && (
      <Line
        style={ height: "10px", marginTop: "20px" }
        strokeWidth={2}
        strokeColor={progress === 100 ? "#00a626" : "#2db7f5"}
        percent={progress}
      />
    )
  );
};
```

여기서는 "rc-progress"에서 가져온 "Line" 구성 요소를 사용하고 있습니다. 높이와 여백 상단을 추가하고 스트로크 폭도 2로 추가했습니다.

스위치 오퍼레이터를 사용하여 진행 표시줄이 로드되는 시점과 완료 시점에 대한 색상을 추가했습니다. 마지막으로 진행률 표시줄의 비율을 설정합니다.

앱을 완성하려면 앱에서 이 모든 것을 렌더링하고 브라우저에서 어떻게 표시되는지 알아보겠습니다.

```js
export default function App() {
  return (
    <Uploady
      destination={ url: "http://server.com" }
      enhancer={mockEnhancer}
    >
      <div className="App">
        <Header />
        <UploadButton />
        <UploadProgress />
      </div>
    </Uploady>
  );
}
```

위의 코드에서, 우리는 `Uploady` 컴포넌트를 사용하여 `App` 컴포넌트를 포장하고 파일 업로드 대상을 설정했습니다. 테스트용이기 때문에 더미 서버를 대상으로 설정할 수 있습니다.

그런 다음 "Header" 구성 요소인 "UploadButton"과 "Upload Progress"를 렌더링하여 이전에 기능으로 초기화했습니다. 우리의 어플리케이션은 아래 이미지와 같아야 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/react-uploady-no-styling.png?resize=683%2C216&ssl=1)

우리의 지원서는 거의 완성되었다.

이 작업을 완료하기 위해 업로드 시스템에 스타일과 개선제를 추가해 보겠습니다. Enhancer는 업로더 인스턴스를 향상시킬 수 있는 기능입니다. 이러한 기능을 사용하여 파일 업로드에 사용자 지정 기능을 추가할 수 있습니다. 지금 해보겠습니다.

```js
const mockEnhancer = (uploader) => {
  const mockSender = createMockSender({ delay: 1500 });
  uploader.update({ send: mockSender.send });
  return uploader;
};
```

위의 코드에서 업로더를 메소드로 하는 mockEnhancer 기능을 만들었습니다. 그 안에서, 우리는 `@rpldy/sender`에서 가져온 mocksender 방법을 초기화했다. 그리고 1.5초 지연을 추가해서 업로더를 반송했습니다. 이것은 실제 전송 및 업로드를 모방하기 위해 수행되었습니다.

아래 스타일을 추가하고 지원서의 마지막 면을 살펴보겠습니다.

```css
.App {
  font-family: arial, sans-serif;
  text-align: center;
  margin-top: 20px;
}
body {
  background: #5c6e91;
  color: #eeeded;
}
button {
  height: 60px;
  width: 200px;
  font-size: 22px;
  background-color: #8f384d;
  color: #eeeded;
  text-transform: uppercase;
  cursor: pointer;
}
```

아래는 React Uploady가 포함된 최종 애플리케이션 파일 업로드 시스템의 비디오입니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/file-upload-react-uploady.gif?resize=600%2C338&ssl=1)

## 결론

이 게시물에서는 React Uploady에 대해 알아보고 이를 사용하여 간단한 파일 업로드 시스템을 구축하는 방법에 대해 알아봤습니다. 또한 ES6 JavaScript를 사용하여 파일 업로드를 위한 로직을 작성하는 방법도 확인했습니다.

여기 LogRocket에서 React 기사를 더 읽고 React Uploady에 대한 공식 문서를 확인할 수 있습니다. 앱의 코드와 작동 버전은 여기에서 확인할 수 있습니다.