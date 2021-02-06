---
layout: post
title: "React-Uploady 및 Ant Design으로 파일 업로드 구성 요소 구축"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/buildingafileuploadcomponentwithreactuploady.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/buildingafileuploadcomponentwithreactuploady.png?fit=730%2C487&ssl=1)

웹 사이트의 사용에 있어서는 사용자 입력이 중요하며, 오늘날 많은 사이트에서는 사용자가 웹 사이트에 파일을 업로드하는 데 이런저런 이유가 있을 수 있습니다. 새 사용자 프로필 이미지부터 도움말 포럼에서 사용자의 문제 해결에 필요한 진단 파일에 이르기까지 모든 작업을 수행할 수 있습니다. 따라서 사용자가 파일 업로드 프로세스를 쉽고 원활하게 수행하는 것이 중요합니다. 사용자가 파일 선택기에 빠르게 액세스하고 파일을 선택하여 업로드하고 업로드 상태에 대한 피드백을 받을 수 있도록 해야 합니다. 여기서 하루를 살리기 위한 리액트업(react upday)이 시작됩니다.

리액트업로드 라이브러리는 간단하면서도 사용자 정의가 가능하도록 구축되었으며 업로더의 보기를 원하는 대로 만들고 사용자 정의할 수 있습니다. 라이브러리를 사용하는 개발자는 단순성을 유지하고 플러그 앤 플레이만 사용하거나 전체 업로드 흐름을 사용자 정의할 수도 있습니다. 이 기사에서는 잘 구축되고 사용자 정의가 가능한 React UI 구성 요소의 라이브러리를 제공하는 Ant Design을 사용하여 파일 탐색기를 열고 업로드할 파일을 선택할 수 있는 단순하면서도 매력적인 업로더 구성 요소를 만들 것입니다.

## 왜 반응 업로드를 사용하는가?

개발자가 다른 파일 업로더 라이브러리보다 리액트 업로드를 사용하는 이유가 궁금할 수 있습니다. 리액트 업로드는 다른 라이브러리에서 얻을 수 없는 몇 가지 이점을 살펴보도록 하겠습니다.

- 사용자 지정이 가능합니다. 개발자는 업로드 프로세스를 원하는 대로 조정할 수 있도록 패키지로 업로더를 설정하는 방법에 대한 여러 가지 옵션을 제공하고, 있는 그대로 사용하거나 사용자 지정 UI를 만들 수 있으며, 다운로드 진행률 및 미리 보기를 다른 옵션 중에서 표시할 수 있습니다.
- 도서관은 다양한 조각이 분리되어 불필요한 패키지 설치를 줄일 수 있어 매우 가볍다. 예를 들어, 개발자가 업로드 미리 보기 또는 진행 상황을 모니터링 및 표시하려는 경우 각 부품과 관련된 패키지를 별도로 설치하고 필요하지 않은 패키지는 생략하여 번들 크기가 훨씬 작아질 수 있습니다.

## 업로더를 만드는 중

### 응용 프로그램 설정

먼저 create-react-app으로 애플리케이션을 부트스트랩합니다. 아직 설치되어 있지 않은 경우 다음을 실행하여 애플리케이션을 부트스트랩할 수 있습니다.

```undefined
npm install -g create-react-app
```

이제 다음을 실행하여 애플리케이션을 생성할 수 있습니다.

```undefined
create-react-app react-uploady-demo
```

애플리케이션을 설치하고 다음 명령을 실행하는 데 필요한 몇 가지 종속성이 있습니다.

```undefined
yarn add @rpldy/uploady @rpldy/sender antd
```

다음 패키지는 다음과 같습니다.

- @rpldy/Uploady – 리액션 업로드 메인 패키지에는 업로더의 기능 및 기타 고급 기능 및 클라이언트 앱용 데이터를 노출하는 후크뿐 아니라 업로드 컨텍스트 제공자가 포함되어 있습니다.
- @rpldy/sender – 응답 업로드를 위한 기본 XHR 요청 전송기로, 실제 서버 요청을 설정하는 대신 보낸 요청을 조롱하기 위해 사용하고 있습니다. 우리는 모의 강화제를 만들어서 이것을 하고 있는데, 이 기사 뒷부분에서 다룰 것이다.
- Antd – Ant Design Component 라이브러리

## 'Uploader' 구성 요소 생성

아래와 같이 `Uploader.js` 파일을 생성하고 `Uploader` 기능 구성 요소를 생성하십시오.

```js
//Uploader.js
import React, { useState, useCallback, useContext } from "react";
import "./App.css";
import Uploady, { useItemProgressListener, UploadyContext } from "@rpldy/uploady";
import { createMockSender } from "@rpldy/sender";
import { Button, Progress } from "antd";

const Uploader = () => {
  return (
    <Uploady
      destination={ url: "http://mock-server.com" }
      enhancer={mockEnhancer}
    >
      <div className="Uploader">
        <CustomButton />
        <br />
        <br />
        <UploadProgress />
      </div>
    </Uploady>
  );
}
const mockEnhancer = uploader => {
  const mockSender = createMockSender({ delay: 1000 });
  uploader.update({ send: mockSender.send });
  return uploader;
};

export default Uploader;
```

위의 코드에서 우리는 리액션 업로드를 통해 컨텍스트 제공자 `업로드` 구성 요소를 가져온다. 이를 통해 추가하고자 하는 모든 개선 프로그램뿐만 아니라 대상과 같은 업로드 옵션을 구성할 수 있습니다.

destination prop는 업로드된 파일을 보낼 엔드포인트를 구성하는 객체를 사용합니다. 우리의 경우, 우리는 단순히 URL을 제공하지만, 여기서 당신은 당신의 요청에 필요한 요청 매개 변수, 방법, 그리고 헤더를 설정할 수 있습니다.

반면, `enhancer` 소품은 업로더의 동작을 변경하는 데 사용할 수 있는 Enhancer 기능을 사용합니다. 예를 들어, 여기서 업로더에게 실패한 다운로드를 다시 시도하도록 지시하는 기능을 추가할 수 있습니다. 우리의 경우 1000밀리초 후 업로드가 성공적으로 전송되었음을 업로더에게 알려주는 `크리에이트 모크송더`를 사용하여 만든 `모크인핸서` 기능을 공급하고 있다. Enhancer에 대한 자세한 내용은 Enhancer에 대한 설명서를 참조하십시오.

또한 CustomButton과 UploadProgress라는 두 가지 구성 요소가 누락된 것을 알게 될 것입니다.

## 업로드 버튼 생성

당사의 업로드 버튼은 React의 useContext 후크를 사용하여 업로드 컨텍스트에 대한 액세스 권한을 부여받고 이를 사용하여 네이티브 파일 브라우저를 열어 서버로 보낼 파일을 선택하는 간단한 기능 구성요소입니다. 이 프로세스는 Ant Design에서 사용자 지정 버튼을 클릭하면 트리거되며, 이 버튼은 구성 요소에 정의된 `핸들 업로드` 기능을 호출합니다. 이를 위해 다음 코드를 `Uploader.js`에 추가합니다.

```js
const CustomButton = () => {
    const uploady = useContext(UploadyContext);
    const hanldeUpload = useCallback(()=> {
            uploady.showFileUpload();
        },[uploady]);
    return <Button onClick={hanldeUpload} type="primary">Custom Upload Button</Button>
}
```

## 업로드 진행률 모니터링 및 표시

사용자에게 업로드 프로세스가 어떻게 진행되고 있는지에 대한 피드백을 제공하는 것이 좋은 사용자 환경을 만드는 데 중요하며, 이를 위한 좋은 방법은 진행 표시줄이나 유사한 UI를 통해 다운로드 진행 상황을 표시하는 것입니다. 우리의 경우 앤트 디자인의 `프로그레스` 요소, 특히 원형 변형으로 진행할 것이며, 이렇게 진행할 수 있다.

다음 기능 구성 요소를 `Uploader.js`에 추가하십시오.

```js
const UploadProgress = () => {
  const [progress, setProgess] = useState(0);
  const progressData = useItemProgressListener();
  if (progressData && progressData.completed > progress) {
    setProgess(() => progressData.completed);
  }
  return (
    progressData && (
      <Progress
        type="circle"
        percent={progress}
      />
    )
  );
};
```

4개의 진행 UI는 actupload에서 useItemProgress Listener 후크를 사용하여 업로더로부터 현재 진행 상황을 받았습니다. 이 후크는 보낸 사람으로부터 각 다운로드의 진행률 정보를 수신하고 캡처한 다음, 우리는 이 진행률을 상태로 저장하고, 완료까지 업로드된 양을 보여주는 `진행률` 구성 요소에 공급한다. 물론 우리는 진행 중인 업로드가 없을 때 이 구성 요소를 숨깁니다.

## 스타일링 및 마무리

마지막 단계는 일부 CSS를 추가하여 우리의 구성 요소가 브라우저에 잘 배치되도록 한 다음 우리의 업로더 구성 요소를 메인 `App.js` 파일에 추가하여 표시될 수 있도록 하는 것이다. `App.css`에 다음 몇 줄을 추가하여 구성 요소를 중앙에 배치하고 공간을 약간 확보하십시오.

```css
.Uploader{
  padding: 5rem;
  margin: 0 auto;
}
```

이제 모든 create-react-app 보일러 플레이트 코드를 다음과 같은 몇 개의 줄로 교체하여 `App.js`를 업데이트하겠습니다.

```js
import './App.css';
import 'antd/dist/antd.css';
import Uploader from './Uploader'
function App() {
  return (
    <div className="App">
      <Uploader />
    </div>
  );
}
export default App;
```

바로 그거야! 우리는 갈 준비가 다 됐어요! yarn start로 응용 프로그램을 시작하고 마술을 보세요!

최종 신청서는 여기서 확인하실 수 있습니다.

## 결론 및 제안

이 정보를 사용하면 Retact 응용 프로그램에서 리액션 업로드를 사용하여 직관적인 사용자 업로드 구성 요소를 생성할 수 있습니다. 그러나 이 방법은 특정 URL에서 파일을 업로드할 뿐만 아니라 끌어서 놓기 입력을 지원하는 라이브러리와 함께 파일을 업로드하기 위한 하나의 방법입니다. 라이브러리는 또한 청크된 XHR 요청 및 TUS 프로토콜 사용과 같은 많은 송신자를 지원합니다. 위의 모든 방법을 단 한 번의 기사만으로 자세히 살펴보는 것은 거의 불가능하며, 리액션 업로드를 사용할 때 가이드와 문서를 확인하는 것을 추천합니다. 관리자들은 또한 친절하게도 다양한 복잡성의 몇 가지 예를 들어 스토리북을 만들 수 있었다.

물론, 다시 시작할 수 있는 다운로드를 만들고 토스트와 함께 사용자 피드백을 제공하는 등 업로더의 UX를 더욱 개선하기 위해 취할 수 있는 다른 단계가 있습니다. 바라건대, 이 기사에서 습득한 지식을 바탕으로 이러한 개선사항 등을 살펴볼 수 있기를 바랍니다.