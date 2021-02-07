---
layout: post
title: "React를 사용하여 아바타 기능을 만드는 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/create-avatar-feature-react.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/create-avatar-feature-react.png?fit=730%2C487&ssl=1)

수년 동안 아바타는 특히 사람 사이의 상호작용을 수반하는 애플리케이션 사이에서 사용자의 시각적 표현을 제공하는 인기 있는 방법이 되었다. 소셜 미디어 응용 프로그램, 채팅 응용 프로그램, 깃허브 등에 대한 아바타가 있습니다. 이러한 맥락에서 아바타는 사용자를 나타내며 다른 사용자가 쉽게 인식할 수 있도록 하는 이미지(대부분 원형)이다.

사용자가 React 응용 프로그램에서 아바타 사진을 처음부터 업로드할 수 있는 방법을 구현하려고 했다면 사용자 사진을 수락하거나 사진을 자르고 크기를 조정하는 등 여러 가지 걱정을 해야 하며, 다운로드하거나 서버로 전송하기 위해 base64와 같은 형식으로 인코딩해야 할 수도 있습니다.

이 작업은 시간이 많이 걸릴 수 있으며, 이와 같은 대부분의 경우처럼 휠을 재설계하지 않는 것이 가장 좋습니다. 기존 라이브러리를 사용하는 것이 좋습니다. 그러한 도서관 중 하나는 리액트 아바타이다.

이 기사에서는 이 라이브러리에서 어떤 기능을 하는지 살펴보고, 기사의 마지막 부분까지 간단한 리액션 앱을 만들어 사용자가 컴퓨터에서 이미지를 가져와 아바타를 만들고 아바타를 컴퓨터에 다운로드할 수 있도록 하겠습니다.

### 전제조건

이 기사를 완벽하게 따르려면 React와 JavaScript를 기본적으로 이해해야 합니다.

## 리액트 아바타 도서관

리액트 아바타는 아바타 생성을 구현할 수 있는 리액트 기반 라이브러리로, 사용자가 원하는 이미지를 미리 보고 잘라 아바타를 만들 수 있습니다. 또한 사용자 정의가 가능하므로 프로그램의 나머지 부분에 맞게 반응 구성 요소를 스타일링할 수 있습니다. 이제 최소한의 사항만 다루었으니 코드를 살펴보도록 하죠.

### 아바타 구성품

이 도서관의 유일한 구성 요소는 아바타의 이미지를 선택하고 자르기 위해 사용하는 아바타 구성품이다. 다음은 사용 중인 `아바타` 구성 요소의 기본 예입니다.

```php
function App() {
  const [preview, setPreview] = useState(null);
  function onClose() {
    setPreview(null);
  }
  function onCrop(pv) {
    setPreview(pv);
  }
  function onBeforeFileLoad(elem) {
    if (elem.target.files[0].size > 71680) {
      alert("File is too big!");
      elem.target.value = "";
    }
  }
  return (
    <div>
      <Avatar
        width={300}
        height={300}
        onCrop={onCrop}
        onClose={onClose}
        onBeforeFileLoad={onBeforeFileLoad}
        src={null}
      />
      {preview && <img src={preview} alt="Preview" />}
    </div>
  );
}
export default App;
```

위의 코드에서 브라우저의 결과는 다음과 같습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/avatar-preview-box.jpeg?resize=632%2C340&ssl=1)

이제 상자를 클릭하여 미리 보기 상자에 전화를 걸어 아바타에 사용할 이미지를 선택할 수 있습니다. 그만큼 쉽죠!

다음으로 넘어가기 전에 아바타 부품의 여러 가지 소품들에 대해 설명하겠습니다.

### 높이와 너비

이러한 소품은 미리보기 상자의 높이와 너비를 정의합니다.

### 작물에 관하여

onCrop은 아바타 이미지가 잘릴 때마다 트리거되는 이벤트 리스너라고 생각할 수 있다. `onCrop`은 함수이므로 인수를 사용할 수 있으며, 이 경우 인수는 base64 형식으로 인코딩된 잘린 이미지이다.

### 닫힘 시'

이미지를 선택하면 미리보기 상자의 왼쪽 상단 모서리에 취소 아이콘이 나타납니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/cancel-icon-onclose-event.jpeg?resize=600%2C300&ssl=1)

이 아이콘을 클릭하면 `onClose` 이벤트 수신기가 트리거되고 `onClose` prop에 전달된 기능이 실행됩니다. 위의 코드에서는 미리보기 상자가 닫힐 때마다 미리보기 상태 변수를 null로 설정합니다.

### '파일 로드 전'

이면에는 미리보기 상자에는 사용자의 컴퓨터에서 파일을 가져와 처리할 수 있는 = 입력 유형="파일" 요소가 포함되어 있습니다.

OnBeforeFileLoad는 사용자가 아바타에 사용할 이미지를 선택할 때마다 트리거되며, 여기에 전달된 인수는 파일 선택 이벤트의 이벤트 개체입니다. 이 객체에서 우리는 크기와 같은 선택된 파일에 대한 정보를 얻을 수 있으며, 그 정보를 사용하여 아바타에 사용할 이미지의 크기를 제한하는 것과 같은 다른 작업을 수행할 수 있다.

또한 파일 크기가 지정된 크기보다 클 경우 대상 요소의 `값` 속성을 빈 문자열로 설정합니다. 라이브러리가 값 속성을 확인하고 빈 문자열일 경우 아바타 생성 프로세스(우리가 원하는 것)를 중단하기 때문이다.

### "On FileLoad"

onFileLoad는 onFileLoad 기능이 실행되면 바로 호출되는 또 다른 이벤트 수신기이다. 또한 이벤트 객체를 포함하므로 여기서는 `BeforeFileLoad`에서 원하는 모든 작업을 수행할 수 있습니다. 유일한 차이점은 라이브러리가 `onFileLoad` 함수를 호출하기 전에 이벤트 개체의 `value` 속성이 비어 있지 않도록 한다는 것입니다.

### 'label' 및 'labelStyle'

`label`은 이미지를 선택하기 전에 미리보기 상자에 표시되는 텍스트를 정의하고, `labelStyle`은 해당 텍스트의 스타일을 정의합니다.

이제 대부분의 소품을 다루었으므로 간단한 앱을 구축하여 사용자가 아바타를 만들어 기기에 다운로드할 수 있습니다.

## 대응 프로젝트 설정

우선, 우리는 새로운 리액트 프로젝트를 세워야 합니다. 이 작업은 생성-응답-앱을 통해 쉽게 수행할 수 있습니다.

```undefined
npx create-react-app avatar-project
```

Retact 프로젝트를 만들고 나면 Retact-avata 라이브러리를 종속성으로 설치해야 합니다. 터미널에 다음과 같이 설치합니다.

```coffeescript
npm install react-avatar-edit
```

이제 프로젝트가 설정되었으니 코드 작업을 시작하겠습니다. 위의 첫 번째 블록의 코드를 출발점으로 사용할 수 있으므로 코드 편집기에 복사하십시오.

이제 우리는 우리의 이미지에서 아바타를 만들 방법이 생겼기 때문에, 우리는 아바타를 우리의 장치에 다운로드하는 방법을 찾을 필요가 있습니다. 다운로드된 아바타의 이름(원하는 대로 될 수 있음)으로 설정된 다운로드 속성과 미리보기 상태 변수의 값인 기본 64 인코딩 형식으로 설정된 href 속성의 앵커 태그를 쉽게 사용할 수 있다.

간단한 앱의 코드는 다음과 같습니다.

```js
import React from "react";
import Avatar from "react-avatar-edit";
import { useState } from "react";

function App() {
  const [preview, setPreview] = useState(null);
  function onClose() {
    setPreview(null);
  }
  function onCrop(pv) {
    setPreview(pv);
  }
  function onBeforeFileLoad(elem) {
    if (elem.target.files[0].size > 2000000) {
      alert("File is too big!");
      elem.target.value = "";
    }
  }
  return (
    <div>
      <Avatar
        width={600}
        height={300}
        onCrop={onCrop}
        onClose={onClose}
        onBeforeFileLoad={onBeforeFileLoad}
        src={null}
      />
      <br/>
      {preview && (
        <>
          <img src={preview} alt="Preview" />
          <a href={preview} download="avatar">
            Download image
          </a>
        </>
      )}
    </div>
  );
}
export default App;
```

이제 사용자는 선호하는 이미지에서 아바타를 만들어 해당 아바타를 장치에 다운로드할 수 있어야 한다.

## 커스터마이징

프로그램 테마에 맞게 미리보기 상자를 사용자 정의하거나 프로그램을 더 흥미롭게 만들 수 있습니다. 앞에서 말했듯이, 이미지를 선택하기 전에 미리보기 상자 중앙에 표시되는 텍스트를 사용자 정의하기 위해 `label`과 `labelStyle`을 사용할 수 있습니다.

또한 미리보기 상자의 테두리 스타일을 사용자 지정할 수 있는 borderstyle, 이미지를 선택할 때 미리보기 상자의 배경색을 설정할 수 있는 background color, 잘라낼 이미지의 부분 색상을 사용자 지정할 수 있는 shading color 등의 사용자 지정 소품도 있습니다.

## 결론

이걸로 끝이야! 이제 Retact 응용 프로그램에 아바타 생성 기능을 구현할 수 있습니다. 이 라이브러리에 대해 자세히 알아보려면 여기에서 GitHub의 리포지토리를 확인하십시오.