---
layout: post
title: "반응에서 끌어서 놓기를 사용하는 방법"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2019/09/react-drag-and-drop.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/09/react-drag-and-drop.png?fit=730%2C487&ssl=1)

편집자 참고: 이 반응 드래그 앤 드롭 튜토리얼은 2021년 1월 4일에 마지막으로 업데이트되었습니다.

이 튜토리얼에서는 Retact 끌어서 놓기 도구 및 사용 사례에 대해 중점적으로 설명합니다. 반응에서 끌어서 놓기의 작동 방식을 시연하기 위해 이 기본 예제를 기반으로 간단한 응용 프로그램을 만듭니다.

다음 사항에 대해 자세히 설명합니다.

- 반응에서 끌어서 놓기란 무엇입니까?
- 대응에서 드래그 앤 드롭을 구현하는 방법
- 끌어서 놓기를 사용하여 파일 업로드
- 끌어서 놓기 라이브러리 대응
- `아름다운`
- ➡드드드드드
- 반응-그리드
- react-dnd 사용(예와 함께)
- 기본 드래그 앤 드롭 대응

## 반응에서 끌어서 놓기란 무엇입니까?

드래그 앤 드롭 API는 드래그 게이블 요소를 HTML로 가져와 개발자가 한 장소에서 다른 장소로 끌어다 놓을 수 있는 풍부한 UI 요소를 포함하는 애플리케이션을 구축할 수 있게 한다.

드래그 앤 드롭 API는 대부분의 최신 애플리케이션에서 필수적인 부분입니다. UX를 구성하지 않고 UI를 풍부하게 제공합니다.

반응에서 끌어서 놓기의 가장 일반적인 사용 사례는 다음과 같습니다.

- 파일을 업로드하는 중입니다. 이것은 Gmail, WordPress, Invision 등과 같은 제품의 핵심 기능입니다.
- 여러 목록 간에 항목을 이동합니다. Tello와 Asana와 같은 도구를 생각해 보십시오.
- 이미지와 자산을 재배치합니다. 대부분의 비디오 편집기에는 이 기능이 있으며, Invision과 같은 제품은 이 기능을 사용하여 섹션 간에 설계 자산을 재배치합니다.

## 대응에서 드래그 앤 드롭을 구현하는 방법

대응 드래그 앤 드롭 예제를 보여드리기 위해 사용자가 다음을 수행할 수 있는 간단한 애플리케이션을 구축하겠습니다.

- 이미지 파일을 브라우저에 삭제하여 업로드
- 이미지를 그리드로 미리 보기 표시
- 반응에서 끌어서 놓기로 이미지 순서 변경

먼저 다음과 같이 `생성-반응-앱`을 사용하여 반응 앱을 부팅하는 것으로 시작해 보겠습니다.

```undefined
npx create-react-app logrocket-drag-and-drop
cd logrocket-drag-and-drop
yarn start
```

## 끌어서 놓기를 사용하여 파일 업로드

우리는 스스로 모든 논리와 구성 요소를 만들어냄으로써 바퀴를 재창조하지는 않을 것입니다. 대신 가장 일반적인 표준 반응 끌어서 놓기 라이브러리를 사용합니다.

드래그 앤 드롭 업로드 기능을 위해 React: react-drop zone에서 가장 유명한 라이브러리 중 하나를 사용합니다. react-dropzone은 React에서 사용자 지정 구성 요소를 생성할 수 있도록 도와주는 매우 강력한 라이브러리입니다. 깃허브에는 거의 8,000개의 별이 있으며, 리액트 훅스의 지원을 받고 있다.

`react-drop zone`을 설치하려면:

```undefined
yarn add react-dropzone
```

그런 다음 `Dropzone.js`라는 새 파일을 만듭니다. 이 구성 요소는 간단한 내용 영역을 파일을 삭제할 수 있는 드롭 영역 영역으로 만드는 역할을 합니다.

`react-drop zone`의 작동 방식은 다음과 같습니다.

- `drop-drop zone`은 파일 입력을 숨기고 아름다운 사용자 지정 드롭 영역 영역을 표시합니다.
- 파일을 드롭할 때 `react-dropzone`은 HTML onDrag 이벤트를 사용하고 파일을 드롭존 영역 내에 드롭하는지 여부를 기준으로 이벤트에서 파일을 캡처합니다.
- 영역을 클릭하면 React-dropzone 라이브러리가 React `ref`를 사용하여 숨겨진 입력을 통해 파일 선택 대화 상자를 시작하고 파일을 선택하여 업로드할 수 있습니다.

`Dropzone`이라는 구성 요소를 생성해 보겠습니다.

```js
/* 
  filename: Dropzone.js 
*/

import React from "react";
// Import the useDropzone hooks from react-dropzone
import { useDropzone } from "react-dropzone";

const Dropzone = ({ onDrop, accept }) => {
  // Initializing useDropzone hooks with options
  const { getRootProps, getInputProps, isDragActive } = useDropzone({
    onDrop,
    accept
  });

  /* 
    useDropzone hooks exposes two functions called getRootProps and getInputProps
    and also exposes isDragActive boolean
  */

  return (
    <div {...getRootProps()}>
      <input className="dropzone-input" {...getInputProps()} />
      <div className="text-center">
        {isDragActive ? (
          <p className="dropzone-content">Release to drop the files here</p>
        ) : (
          <p className="dropzone-content">
            Drag 'n' drop some files here, or click to select files
          </p>
        )}
      </div>
    </div>
  );
};

export default Dropzone;
```

부품은 간단하지만 코드를 좀 더 자세히 살펴보겠습니다.

useDropzone은 사용자 지정 드롭존 영역을 만들기 위한 여러 가지 방법과 변수를 제공합니다. 우리 프로젝트의 경우, 우리는 주로 다음 세 가지 속성에 관심이 있습니다.

- getRootProps는 드롭존 영역의 상위 요소를 기반으로 설정됩니다. 이 요소는 드롭 영역 영역의 폭과 높이를 결정합니다.
- getInputProps는 입력 요소에 전달되는 소품입니다. 파일을 가져오는 드래그 이벤트와 함께 클릭 이벤트를 지원할 수 있습니다.
- useDropzone에 전달하는 파일과 관련된 모든 옵션은 이 입력 요소로 설정되어 있습니다. 예를 들어, 단일 파일만 지원하려는 경우 `복수: false`를 전달할 수 있습니다. 하나의 파일만 허용하려면 자동으로 `drop zone`이 필요합니다.
- 파일을 드롭 영역 위로 끌면 `isDragActive`가 설정됩니다. 이 변수를 기준으로 스타일링하는 데 매우 유용합니다.

다음은 `isDragActive` 값을 기준으로 스타일/클래스 이름을 설정하는 방법의 예입니다.

```js
const getClassName = (className, isActive) => {
  if (!isActive) return className;
  return `${className} ${className}-active`;
};

...
<div className={getClassName("dropzone", isDragActive)} {...getRootProps()}>
...
```

드래그 앤 드롭 예에서는 두 가지 소품만 사용했습니다. 이 라이브러리는 사용자의 필요에 따라 드롭존 영역을 사용자 정의할 수 있는 많은 소품을 지원합니다.

우리는 이미지 파일만 허용하기 위해 `수락` 소품을 사용했습니다. 우리의 App.js는 다음과 같아야 한다.

```js
/*
filename: App.js 
*/

import React, { useCallback } from "react";
// Import the dropzone component
import Dropzone from "./Dropzone";

import "./App.css";

function App() {
  // onDrop function  
  const onDrop = useCallback(acceptedFiles => {
    // this callback will be called after files get dropped, we will get the acceptedFiles. If you want, you can even access the rejected files too
    console.log(acceptedFiles);
  }, []);

  // We pass onDrop function and accept prop to the component. It will be used as initial params for useDropzone hook
  return (
    <main className="App">
      <h1 className="text-center">Drag and Drop Example</h1>
      <Dropzone onDrop={onDrop} accept={"image/*"} />
    </main>
  );
}

export default App;
```

![image](https://blog.logrocket.com/wp-content/uploads/2019/09/dndex-nocdn.png)

우리는 메인 페이지에 드롭존 구성 요소를 추가했습니다. 이제 파일을 놓으면 삭제된 이미지 파일이 콘솔로 표시됩니다.

- `acceptedFiles`는 `File` 값의 배열입니다. 파일을 읽거나 서버로 전송하여 업로드할 수 있습니다. 원하는 프로세스를 모두 수행할 수 있습니다.
- 영역을 클릭하고 업로드해도 동일한 `온 드롭` 콜백이 호출됩니다.
- 수락 소품은 마임 유형을 수락합니다. 모든 표준 MIME 유형 및 일치 패턴을 지원합니다. 예를 들어 PDF만 허용하려는 경우 `accept={`application/pdf`}. 이미지 유형과 PDF를 모두 원하는 경우 `accept={`application/pdf, image/*`}`를 지원합니다.
- onDrop 기능은 useCallback에 포함되어 있습니다. 현재로서는 컴퓨팅 작업량이 많거나 파일을 서버로 전송하지 않았습니다. 우리는 단지 `수락된 파일`을 위로할 뿐이다. 그러나 나중에 파일을 읽고 브라우저에 이미지를 표시하는 상태로 설정할 것입니다. 비싼 기능은 콜백을 사용하고 불필요한 리렌더는 피하는 것이 좋다. 이 예에서는 완전히 선택 사항입니다.

이미지 파일을 읽고 `App.js`의 상태에 추가하겠습니다.

```js
/*
filename: App.js
*/
import React, { useCallback, useState } from "react";
// cuid is a simple library to generate unique IDs
import cuid from "cuid";

function App() {
  // Create a state called images using useState hooks and pass the initial value as empty array
  const [images, setImages] = useState([]);

  const onDrop = useCallback(acceptedFiles => {
    // Loop through accepted files
    acceptedFiles.map(file => {
      // Initialize FileReader browser API
      const reader = new FileReader();
      // onload callback gets called after the reader reads the file data
      reader.onload = function(e) {
        // add the image into the state. Since FileReader reading process is asynchronous, its better to get the latest snapshot state (i.e., prevState) and update it. 
        setImages(prevState => [
          ...prevState,
          { id: cuid(), src: e.target.result }
        ]);
      };
      // Read the file as Data URL (since we accept only images)
      reader.readAsDataURL(file);
      return file;
    });
  }, []);

  ...
}
```

우리의 `이미지` 상태의 데이터 구조는 다음과 같습니다.

```undefined
const images = [
  {
    id: 'abcd123',
    src: 'data:image/png;dkjds...',
  },
  {
    id: 'zxy123456',
    src: 'data:image/png;sldklskd...',
  }
]
```

그리드 레이아웃으로 이미지 미리 보기를 보여드리겠습니다. 이를 위해 이미지리스트라는 또 다른 구성 요소를 만들 예정입니다.

```coffeescript
import React from "react";

// Rendering individual images
const Image = ({ image }) => {
  return (
    <div className="file-item">
      <img alt={`img - ${image.id}`} src={image.src} className="file-img" />
    </div>
  );
};

// ImageList Component
const ImageList = ({ images }) => {
  
  // render each image by calling Image component
  const renderImage = (image, index) => {
    return (
      <Image
        image={image}
        key={`${image.id}-image`}
      />
    );
  };

  // Return the list of files
  return <section className="file-list">{images.map(renderImage)}</section>;
};

export default ImageList;
```

이제, 우리는 이 `ImageList` 구성 요소를 `App.js`에 추가하고 이미지 미리 보기를 표시할 수 있습니다.

```php
function App() {
  ...

  // Pass the images state to the ImageList component and the component will render the images
  return (
    <main className="App">
      <h1 className="text-center">Drag and Drop Example</h1>
      <Dropzone onDrop={onDrop} accept={"image/*"} />
      <ImageList images={images} />
    </main>
  );
}
```

우리는 지원서의 절반을 성공적으로 마쳤다. 이제 끌어서 놓기를 사용하여 파일을 업로드하고 이미지 미리 보기를 볼 수 있습니다.

![image](https://blog.logrocket.com/wp-content/uploads/2019/09/dnd-nocdn.png)

다음으로 끌어서 놓기 기능을 사용하여 미리 보기된 이미지의 순서를 변경할 수 있습니다. 먼저 이 기능을 구현하는 데 사용된 가장 인기 있는 라이브러리를 빠르게 검토하고 프로젝트의 필요에 따라 가장 적합한 라이브러리를 선택하는 방법에 대해 살펴보겠습니다.

## 끌어서 놓기 라이브러리 대응

가장 인기 있는 React 드래그 앤 드롭 패키지는 다음과 같습니다.

- `아름다운`
- ➡드드드드드
- ➡-grid-contract

모두 리액트 개발자들 사이에서 인기가 많고 적극적인 기여자들이 있다. 각 도서관을 빠르게 확대하고 그 장단점을 분석해보자.

### '아름다운'

`아름다운-dnd`는 목록을 위해 특별히 만들어진 상위 단계의 추상화이다. 자연스럽고 아름다우며 접근성이 뛰어난 React 드래그 앤 드롭 환경을 제공하도록 설계되었습니다.

찬성:

- `아름다운-dnd`는 수평 또는 수직 이동이 필요한 1차원 레이아웃(예: 목록)과 드래그 앤 드롭 기능에 매우 잘 어울린다. 예를 들어, Trello와 같은 레이아웃은 `react-beautiful-dnd`와 함께 박스 밖으로 나올 수 있다.
- react-beautiful-dnd API는 산들바람이다. 이 팀은 코드베이스에 복잡성을 가중시키지 않고 정말로 즐거운 개발자 경험을 만들어 낼 수 있었습니다.

반대:

- α-뷰티풀-dnd는 모든 방향으로 요소를 이동시켜 x축과 y축의 위치를 동시에 계산할 수 없기 때문에 그리드에서 작동하지 않는다. 그리드에서 요소를 끄는 동안 요소를 놓을 때까지 컨텐츠가 랜덤하게 이동됩니다.

### ➡드드드드드

`react-dnd`는 구성 요소를 분리하지 않고 고급 드래그 앤 드롭 인터페이스를 구축할 수 있도록 설계된 일련의 Retact 유틸리티입니다. Tello 및 Storify와 같은 앱과 유사한 기능을 지원하며, 드래그 앤 드롭 이벤트에 대응하여 앱의 다양한 부분 간에 데이터가 전송되고 구성 요소가 모양과 애플리케이션 상태를 변경합니다.

찬성:

- React DND는 거의 모든 사용 사례(그리드, 1차원 목록 등)에 대해 작동합니다.
- React 끌어서 놓기에서 사용자 지정 작업을 수행할 수 있는 매우 강력한 API를 갖추고 있습니다.

반대:

- API는 작은 예에서도 쉽게 시작할 수 있습니다. 애플리케이션에 맞춤화된 기능이 필요한 경우 이를 달성하는 것이 매우 까다로워집니다. react-beautiful-dnd보다 학습곡선이 더 높고 복잡하다.
- 웹 및 터치 장치를 모두 지원하려면 일부 해킹이 필요합니다.

우리의 사용 사례에 대해서는 ract-dnd를 사용하기로 했습니다. 레이아웃에 항목 리스트만 포함되었으면 `react-beautiful-dnd`를 선택했을 텐데, 저희 예에서는 이미지 그리드가 있습니다. 따라서 드래그 앤 드롭을 달성하기 위한 가장 쉬운 API는 react-dnd이다.

### 반응-그리드

React-Grid-Layout은 React 전용 그리드 레이아웃 시스템입니다.

패커리와 그리드스터와 같은 유사한 시스템과 달리, 리액트 그리드 레이아웃은 응답성이 뛰어나고 사용자가 제공하거나 자동 생성할 수 있는 중단점 레이아웃을 지원합니다. jQuery가 필요하지 않습니다.

찬성:

- 반응 그리드 레이아웃은 그리드에서 작동합니다. 그리드 자체는 모든 것을 포괄하므로 기술적으로는 1차원 이동에도 사용할 수 있습니다.
- React-Grid-Layout은 완전한 사용자 정의 및 크기를 가진 대시보드(예: Looker, 데이터 시각화 제품 등)와 같이 드래그 앤 드롭이 필요한 복잡한 그리드 레이아웃에 적합합니다.
- 대규모 애플리케이션 요구에 대해 복잡할 만한 가치가 있습니다.

반대:

- React-Grid-Layout에는 자체적으로 많은 계산을 수행해야 하는 악성 API가 있습니다.
- 레이아웃 구조는 React-Grid-Layout의 구성 요소 API를 통해 UI에서 정의되어야 합니다. 이 API는 동적 요소를 즉시 생성할 때 복잡성이 더 커집니다.

## react-dnd 사용(예와 함께)

드래그 앤 드롭 코드에 들어가기 전에 먼저 react-dnd가 어떻게 작동하는지 이해해야 한다. "ddnd"는 어떤 요소도 끌 수 있게 만들 수 있고 어떤 요소도 떨어뜨릴 수 있게 만들 수 있다. 이를 위해 react-dnd는 다음과 같은 몇 가지 가정을 가지고 있다.

- ddnd는 모든 드롭 가능한 항목의 참조가 있어야 합니다.
- ddnd는 끌어다 놓을 수 있는 모든 항목의 참조가 있어야 합니다.
- 드래그할 수 있고 드롭할 수 있는 모든 요소는 내부 상태를 초기화하고 관리하는 데 사용되는 react-dnd의 컨텍스트 제공자 안에 포함시켜야 합니다.

ract-dnd는 상태를 어떻게 관리하는지 너무 걱정할 필요가 없다. react-dnd는 그러한 상태를 노출시킬 수 있는 멋지고 쉬운 API를 가지고 있기 때문에 우리는 우리의 지역 상태를 계산하고 업데이트할 수 있다.

react-dnd를 설치하려면:

```undefined
yarn add react-dnd
```

먼저 다음과 같은 DND 컨텍스트 제공자에 `ImageList` 구성 요소를 동봉합니다.

```js
/* 
  filename: App.js 
*/

import { DndProvider } from "react-dnd";
import HTML5Backend from "react-dnd-html5-backend";

function App() {
  ...
  return (
    <main className="App">
      ...
      <DndProvider backend={HTML5Backend}>
        <ImageList images={images} onUpdate={onUpdate} />
      </DndProvider>
    </main>
  );
}
```

간단합니다. "DND 공급자"를 가져와 "백엔드" 소품으로 초기화하기만 하면 됩니다. 앞서 말씀드린 바와 같이 드래그 앤 드롭에 사용할 API를 선택하는 데 도움이 되는 변수입니다. 다음을 지원합니다.

- HTML5 드래그 앤 드롭 API(터치 장치가 아닌 웹에서만 지원)
- 터치 드래그 앤 드롭 API(터치 장치에서 지원됨)

현재 HTML5 API를 사용하여 시작하고 있습니다. 기능이 완료되면 터치 장치에 대한 기본 지원 기능을 제공하는 간단한 유틸리티를 작성할 것입니다.

이제 드래그 가능 및 드롭 가능 항목을 추가해야 합니다. 우리의 응용 프로그램에서는 드래그 가능 품목과 드롭 가능 품목이 모두 동일합니다. 이미지 구성 요소를 끌어다 다른 이미지 구성 요소로 떨어뜨려 작업이 조금 더 쉬워집니다.

이를 구현하려면:

```js
import React, { useRef } from "react";
// import useDrag and useDrop hooks from react-dnd
import { useDrag, useDrop } from "react-dnd";

const type = "Image"; // Need to pass which type element can be draggable, its a simple string or Symbol. This is like an Unique ID so that the library know what type of element is dragged or dropped on.

const Image = ({ image, index }) => {
  const ref = useRef(null); // Initialize the reference
  
  // useDrop hook is responsible for handling whether any item gets hovered or dropped on the element
  const [, drop] = useDrop({
    // Accept will make sure only these element type can be droppable on this element
    accept: type,
    hover(item) {
      ...
    }
  });

  // useDrag will be responsible for making an element draggable. It also expose, isDragging method to add any styles while dragging
  const [{ isDragging }, drag] = useDrag({
    // item denotes the element type, unique identifier (id) and the index (position)
    item: { type, id: image.id, index },
    // collect method is like an event listener, it monitors whether the element is dragged and expose that information
    collect: monitor => ({
      isDragging: monitor.isDragging()
    })
  });
  
  /* 
    Initialize drag and drop into the element using its reference.
    Here we initialize both drag and drop on the same element (i.e., Image component)
  */
  drag(drop(ref));

  // Add the reference to the element
  return (
    <div
      ref={ref}
      style={ opacity: isDragging ? 0 : 1 }
      className="file-item"
    >
      <img alt={`img - ${image.id}`} src={image.src} className="file-img" />
    </div>
  );
};

const ImageList = ({ images }) => {
  ...
};

export default ImageList;
```

자, 우리의 이미지는 이미 끌 수 있습니다. 하지만 만약 우리가 그것을 떨어뜨린다면, 다시 한 번, 이미지는 원래 위치로 갈 것입니다. 왜냐하면 use Drag과 use Drop은 우리가 그것을 떨어뜨릴 때까지 그것을 처리할 것이기 때문이다. 우리가 우리의 지역 국가를 바꾸지 않는 한, 그것은 다시 원래 위치로 돌아갈 것이다.

로컬 상태를 업데이트하려면 다음을 알아야 합니다.

- 질질 끄는 요소
- 홀딩된 요소(끌린 요소가 홀딩된 요소)

사용드래그는 후버(hover) 방식으로 정보를 노출한다.

```undefined
const [, drop] = useDrop({
    accept: type,
    // This method is called when we hover over an element while dragging
    hover(item) { // item is the dragged element
      if (!ref.current) {
        return;
      }
      const dragIndex = item.index;
      // current element where the dragged element is hovered on
      const hoverIndex = index;
      // If the dragged element is hovered in the same place, then do nothing
      if (dragIndex === hoverIndex) { 
        return;
      }
      // If it is dragged around other elements, then move the image and set the state with position changes
      moveImage(dragIndex, hoverIndex);
      /*
        Update the index for dragged item directly to avoid flickering
        when the image was half dragged into the next
      */
      item.index = hoverIndex;
    }
});
```

후버 방법은 요소가 끌려가 이 요소 위로 맴돌 때마다 트리거됩니다. 이러한 방식으로 요소를 끌기 시작하면 해당 요소의 인덱스와 현재 대기 중인 요소의 인덱스를 얻게 됩니다. 이미지 상태를 업데이트하기 위해 이 `드래그 인덱스`와 `허버 인덱스`를 넘기겠습니다.

이 시점에서 다음과 같은 의문이 들 수 있습니다.

- 대기하면서 상태를 업데이트해야 하는 이유는 무엇입니까?
- 삭제하는 동안 업데이트하지 마십시오.

삭제하면서 상태 업데이트만 가능합니다. 그리고 끌어서 놓기 및 위치를 재배열하지만 사용자 경험 좋은 일은 아니야 일할 것이다.

예를 들어 한 이미지를 다른 이미지 위로 끌어다 놓으면 해당 위치가 즉시 변경되면 해당 이미지를 끌어오는 사용자에게 좋은 피드백을 제공할 수 있습니다. 그렇지 않으면 이미지를 특정 위치에 놓을 때까지 끌어서 놓기 기능이 작동하는지 여부를 모를 수 있습니다.

그것이 우리가 모든 호버의 상태를 업데이트하는 이유입니다. 다른 이미지 위를 맴돌면서 상태를 설정하고 위치를 변경합니다. 사용자는 멋진 애니메이션을 볼 수 있습니다. 데모 페이지에서 확인하실 수 있습니다.

지금까지는 상태를 `moveImage`로 업데이트하기 위한 코드를 보여줍니다. 구현 내용을 살펴보겠습니다.

```js
/*
  filename: App.js
*/

import update from "immutability-helper";

const moveImage = (dragIndex, hoverIndex) => {
    // Get the dragged element
    const draggedImage = images[dragIndex];
    /*
      - copy the dragged image before hovered element (i.e., [hoverIndex, 0, draggedImage])
      - remove the previous reference of dragged element (i.e., [dragIndex, 1])
      - here we are using this update helper method from immutability-helper package
    */
    setImages(
      update(images, {
        $splice: [[dragIndex, 1], [hoverIndex, 0, draggedImage]]
      })
    );
};

// We will pass this function to ImageList and then to Image -> Quiet a bit of props drilling, the code can be refactored and place all the state management in ImageList itself to avoid props drilling. It's an exercise for you :)
```

현재 저희 앱은 HTML5 `onDrag` 이벤트 지원 기기에서 완벽하게 작동합니다. 하지만 불행하게도, 그것은 터치 장치에서는 작동하지 않을 것이다.

앞서 말씀드렸듯이, 유틸리티 기능뿐만 아니라 터치 장치도 지원할 수 있습니다. 최선의 해결책은 아니지만 여전히 효과가 있습니다. 하지만 터치 장치에서는 드래그 경험이 그리 좋지 않을 것이다. 간단히 업데이트되지만 끌리는 느낌은 들지 않습니다. 깨끗하게 만드는 것도 가능합니다.

```js
import HTML5Backend from "react-dnd-html5-backend";
import TouchBackend from "react-dnd-touch-backend";

// simple way to check whether the device support touch (it doesn't check all fallback, it supports only modern browsers)
const isTouchDevice = () => {
  if ("ontouchstart" in window) {
    return true;
  }
  return false;
};

// Assigning backend based on touch support on the device
const backendForDND = isTouchDevice() ? TouchBackend : HTML5Backend;

...
return (
  ...
  <DndProvider backend={backendForDND}>
    <ImageList images={images} moveImage={moveImage} />
  </DndProvider>
)
...
```

## 기본 드래그 앤 드롭 대응

React Native에서 드래그 앤 드롭을 구현하는 방법에 대한 자세한 내용은 아래 비디오 튜토리얼을 참조하십시오.

## 결론

그게 다야. 파일을 드래그 앤 드롭하고, 파일을 업로드하고, 파일 순서를 변경할 수 있는 작고 강력한 데모를 성공적으로 구축했습니다. 데모는 여기서 확인하실 수 있습니다.

프로젝트의 코드베이스가 여기에 있습니다. 또한 레포의 분기를 통해 애플리케이션을 어떻게 구축했는지 단계별로 확인할 수 있습니다.

우리는 드래그 앤 드롭 기능 측면에서 React가 할 수 있는 것의 표면을 긁어냈다. 드래그 앤 드롭 라이브러리를 사용하여 매우 포괄적인 기능을 구축할 수 있습니다. 우리는 그 사업에서 가장 좋은 도서관 몇 군데에 대해 토론했다. Drag-and-drop 기능을 빠르고 안정적으로 구축하는 데 도움이 되었으면 합니다.

다른 라이브러리도 확인하시고, 이 라이브러리로 무엇을 만들었는지 댓글로 보여 주세요 😎