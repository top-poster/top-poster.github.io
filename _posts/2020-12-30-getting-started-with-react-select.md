---
layout: post
title: "반응 선택: 소개서"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2019/06/pexels-nao-triponez-792199.jpg
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/06/pexels-nao-triponez-792199.jpg?fit=730%2C487&ssl=1)

편집자 참고: 이 반응 선택 튜토리얼은 2021년 1월 12일에 업데이트되었습니다.

## 도입

3, 4년 전 웹 프로젝트에서 작업할 때 선택 요소를 구축하는 것이 가장 쉬운 작업 중 하나였습니다. 지금은 특히 UI/UX가 우선 순위가 높을 때 선택 요소를 구축하는 데 많은 부분이 있습니다.

온포커스, 스타일링 선택 요소, 원격 소스에서 데이터 가져오기 등의 기능을 고려해야 하며, 목록은 계속된다. 여러분은 아마 리액트 프로젝트를 할 때 이런 생각을 했을 것입니다. 그리고 다중우주의 어딘가에 재사용 가능한 부품이 존재하기를 바랬을 것입니다.

## 반응 선택이란?

글쎄요, 다행히도, 제드 왓슨은 씽크밀과 아틀라시안의 자금 지원을 받아 리액션 셀렉트라고 불리는 오픈 소스 프로젝트를 시작했습니다.

v2를 탄생시킨 리액션 셀렉트 버전 1에는 몇 가지 제한이 있었다. 이 기사에서는 react-select v2에 포함된 멋진 기능과 함께 react-select v2를 시작하는 방법을 소개하는 론칭 패드에 대해 살펴보겠습니다.

## 리액션 셀렉트의 설치 및 기본 사용

전제조건

- 실/npm 설치
- 설치된 react app CLI 도구 생성
- HTML, JavaScript(ES6) 및 CSS에 대한 기본적인 이해
- React JS에 대한 기본 이해 및 Create React 앱 사용
- 명령줄 터미널에 대한 기본 이해

### 리액션 선택 설치

이러한 모든 요구 사항이 제거되었으므로 이제 기존의 React 애플리케이션에 react-select 패키지를 추가해야 합니다. 이 튜토리얼에서는 react-app CLI 생성 도구를 사용합니다. 기존 프로젝트가 없는 경우 다음과 같이 프로비저닝할 수 있습니다.

```shell
$ yarn create react-app react
```

작업이 완료되면 npm에서 react-select 패키지를 설치합니다.

```undefined
$ yarn add react-select
```

이제 대응 프로그램에서 대응 선택 패키지를 가져오고 사용하는 방법을 살펴보겠습니다.

### 반응 선택 기본 사용법

`App.js` 파일에서는 파일 상단에 있는 두 개의 항목, 즉 반응 및 반응 선택 패키지를 각각 다음과 같이 가져옵니다.

```js
//App.js

import React from 'react';
import Select from 'react-select';

...
```

이 두 패키지를 가져오면 리액트 선택(<선택/>)에 액세스할 수 있고 리액트도 확장할 수 있습니다.구성 요소의 클래스입니다. 기존 HTML에서 < 태그에는 여러 옵션과 값이 포함되어 있습니다. 우리의 반응 선택 구성 요소는 동일한 규칙을 따르지만 옵션과 값은 소품으로 전달된다.

```js
//App.js

//Import React and Select 

const options = [
  { value: 'blues', label: 'Blues' },
  { value: 'rock', label: 'Rock' },
  { value: 'jazz', label: 'Jazz' },
  { value: 'orchestra' label: 'Orchestra' } 
];

class App extends React.Component {
  render(){
    return (
      <Select options = {options} />
    );
  }
}

export default App;
```

위의 코드 조각에서, 우리는 소품으로 선택된 컴포넌트에 전달되는 음악 장르로서 우리의 선택 옵션을 가지고 있다. 클래스 "App"은 DOM의 "App" 구성 요소에 렌더링될 수 있도록 내보냅니다. 이 응용 프로그램을 실행할 때 한 쪽 끝에서 다른 쪽 끝까지 화면에 걸쳐 있는 선택 요소를 볼 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2019/06/image_preview-1.gif?resize=600%2C155&ssl=1)

```js
//App.js

import 'bootstrap/dist/css/bootstrap.css';
//Import react and select 

return(
  <Select className="mt-4 col-md-8 col-offset-4"
    options = { options }
  />
);

...
```

더 나은 결과를 위해 루트 DOM 요소를 부트스트랩 컨테이너에 `index.html`로 묶겠습니다.

```xml
<!-- index.html -->
...

<div class="container">
    <div id="root"></div>
</div>

...
```

이를 통해 아래 이미지와 똑같이 보이는 요소를 선택할 수 있습니다.

```js
//App.js 
//Import react and select

const customStyles = {
  option: (provided, state) => ({
    ...provided,
    borderBottom: '2px dotted green',
    color: state.isSelected ? 'yellow' : 'black',
    backgroundColor: state.isSelected ? 'green' : 'white'
  }),
  control: (provided) => ({
    ...provided,
    marginTop: "5%",
  })
}

...

return(
  <Select className="col-md-8 col-offset-4"
    styles = { customStyles }
    options = { options }
  />
);

...
```

선택한 구성 요소의 모양과 느낌을 확장하고 사용자 지정하기 위해 조정한 두 가지 구성 요소 속성(옵션 및 컨트롤)이 있습니다. 리액션 셀렉트에 의해 제공되는 많은 특성들이 있어, 우리 소비자들, 우리의 요구와 취향에 맞게 구축할 수 있는 많은 공간을 제공한다. 이 문서의 뒷부분에서 사용자 정의 구성 요소에 대해 자세히 설명하겠습니다. 이 섹션을 위해 위에서 설명한 두 가지 사용자 지정 구성 요소에 대해 간략히 설명하겠습니다.

옵션: 옵션 표시를 담당하는 구성 요소입니다. 이 구성 요소를 타겟으로 하여 아래의 선택 요소를 얻을 수 있었습니다.

컨트롤: 이것은 `값 컨테이너`와 `지표 컨테이너`를 담당하는 부품으로, 기본 사용 섹션에서 선택한 구성 요소의 첫 번째 이미지와 반대로 전체 선택 구성 요소를 페이지 상단에서 멀어지는 `5%`의 `마진톱` 속성을 추가할 수 있었습니다.

## 온 체인지, 오토포커스, 옵션 소품 사용

이 섹션에서는 선택한 구성 요소의 기능을 사용자 정의하는 데 사용하는 몇 가지 주요 소품을 살펴봅니다. 아래는 이러한 소품 중 몇 가지가 어떻게 유용하게 사용되는지 보여주는 예입니다.

```js
//App.js

//Import react and select

state = {
  selectedOption: null,
}
handleChange = (selectedOption) => {
  this.setState({ selectedOption });
  console.log(`Option selected:`, selectedOption);
}
render(){
  const { selectedOption } = this.state;
}

return (
  <Select className="mt-4 col-md-6 col-offset-4"
  onChange={this.handleChange}
  options={options}
  autoFocus={true}
  />
)
```

위의 내용은 현재 선택된 항목에 대한 정보를 얻기 위해 사용하는 변경 시 국가 관리자 소지자입니다. 콘솔의 옵션으로 `락`을 선택한다면 `선택한 옵션: {value: "rock", 라벨: "Rock}`의 라인을 따라 우리가 선택한 구성 요소에서 얻은 데이터를 조작하고 싶을 때 유용하게 사용할 수 있을 것이다. 옵션과 오토포커스 소품도 눈에 띈다. 옵션 프로포트는 선택 옵션을 선택한 구성 요소에 전달하는 데 사용됩니다. 위의 코드 블록에서 사용되는 옵션은 다음과 같습니다.

```undefined
const options = [
  { value: 'blues', label: 'Blues' },
  { value: 'rock', label: 'Rock' },
  { value: 'jazz', label: 'Jazz' },
  { value: 'orchestra' label: 'Orchestra' } 
];
```

데이터 유형이 `boolean`인 `autoFocus` 프로포트는 페이지 로드 시 선택한 구성 요소에 autoFocus를 추가하는 데 사용됩니다. 사용할 수 있는 소품에 대한 자세한 내용은 소품 설명서를 참조하십시오.

## 사용자 지정 구성 요소

스타일과 상태에서는 선택 스타일링을 확장하는 데 사용한 두 가지 사용자 지정 구성 요소(옵션 및 제어)에 대해 논의했습니다. 이 섹션에서는 `Custom Single Value`라는 또 다른 사용자 지정 구성 요소를 살펴보겠습니다. 이 사용자 지정 구성 요소는 정기적으로 선택하는 구성 요소가 하는 기능을 수행하지만 몇 가지 세부 사항을 추가하고자 합니다. App.js 파일에서 다음과 같이 react와 react-select에서 각각 react와 Select 패키지를 가져올 것입니다.

```js
//App.js

import React, { type ElementConfig } from 'react';
import Select, { components } from 'react-select';
...
```

작업이 끝날 무렵에는 다음과 같은 모양의 완제품이 완성되었습니다.

```coffeescript
//App.js 
 
 const SingleValue = ({ children, ...props }) => (
   <components.SingleValue {...props}>
     {children}
   </components.SingleValue>
 );
 
 class App extends React.Component {
   state = {};
   state = {
     selectedOption: null,
   }
   handleChange = (selectedOption) => {
     this.setState({ selectedOption });
   }
   render() {
     return (
       <Select
           className="mt-4 col-md-6 col-offset-4"
           onChange={this.handleChange}
           styles={ singleValue: (base) => ({ ...base, padding: 5, borderRadius: 5, background: this.state.selectedOption.value, color: 'white', display: 'flex' }) }
           components={ SingleValue }
           options={colourOptions}
         />
     );
   }
 }
 export default App;
```

## 내장 애니메이션 구성 요소 사용

이 섹션에서는 반응 선택 구성 요소에 작은 애니메이션을 추가하는 방법을 살펴보겠습니다. 우리가 선택한 컴포넌트에 애니메이션을 추가하면 되는데, 이 경우 make animated라는 이름의 애니메이션 컴포넌트를 가져온 다음 make animated를 다음과 같이 컴포넌트의 소품에 참조하면 된다.

```js
//App.js

import React from 'react';
import Select, { components } from 'react-select';
import makeAnimated from 'react-select/lib/animated';
import 'bootstrap/dist/css/bootstrap.css';

const colourOptions = [] //our array of colours

class App extends React.Component {
  render(){
    return (
      <Select
        className="mt-4 col-md-6 col-offset-4"
        components={makeAnimated()}
        isMulti
        options={colourOptions}
        />
    );
  }
}

export default App;
```

isMulti` 소품은 아래 gif에서 볼 수 있듯이 한 번에 두 가지 이상의 옵션을 선택할 수 있도록 하기 위해 사용됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/06/image_preview-3.gif?resize=600%2C189&ssl=1)

또 다른 유용한 구성 요소는 고정 옵션 구성 요소입니다. 이 구성 요소로 제거할 수 없는 이미 선택한 값으로 고정 옵션을 사용할 수 있습니다.

## 결론

이 글의 과정 동안, 우리는 반응 선택 구성 요소의 몇 가지 일반적인 사용 사례, 시작하는 방법, 그리고 우리의 요구에 맞게 미리 정의된 구성 요소 중 일부를 확장하는 방법에 대해 배웠다. 리액션 선택 패키지에는 다양한 기능이 내장되어 있으며, 일부는 사용자의 요구에 적합하고 일부는 사용 사례에 맞게 사용자 정의해야 합니다. 여기 손을 더럽히기 위한 공식 문서 링크가 있습니다. 질문이 있거나 막히면 언제든지 의견을 남겨주세요. 제가 도와드리겠습니다.