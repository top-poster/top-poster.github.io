---
layout: post
title: "반응과 함께 부트스트랩 사용"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2018/05/using-bootstrap-with-react.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2018/05/using-bootstrap-with-react.png?fit=730%2C487&ssl=1)

지난 몇 년 동안 단일 페이지 애플리케이션의 인기가 높아지면서 Angular, React, Vue.js, Ember를 포함한 자바스크립트 프레임워크가 유입되었다. 따라서 더 이상 웹 앱을 빌드하기 위해 jQuery와 같은 DOM 라이브러리를 사용할 필요가 없습니다.

이것은 개발자들이 반응하는 웹 앱을 만들도록 고안된 CSS 프레임워크의 출현과 동시에 일어났다. Frontend 개발자인 경우, Bootboot, Foundation 및 Bulma에 대해 거의 확실히 알고 있거나 들어본 적이 있을 것입니다. 강력한 기능과 내장 유틸리티를 갖춘 모든 응답성(모바일 우선) CSS 프레임워크입니다. 리액트가 웹 애플리케이션을 구축하는 데 가장 많이 사용되는 자바스크립트 프레임워크라면, 부트스트랩은 가장 인기 있는 CSS 프레임워크로 인터넷에서 수백만 개의 웹 사이트에 전원을 공급한다.

이 튜토리얼에서는 반응 및 부트스트랩 사용 방법, 반응 앱에 부트스트랩을 추가하는 방법, 예를 들어 `반응 부트스트랩`과 같은 반응 부트스트랩 패키지를 설치하는 방법, 마지막으로 부트스트랩을 사용하여 반응 앱을 만드는 방법에 대해 설명합니다.

이 프레임워크들을 지금 막 시작하고 있다면, 저는 공식 반응 및 부트스트랩 문서를 훑어보는 것을 제안합니다. 아래 비디오 튜토리얼을 보고 좀 더 자세히 살펴보시기 바랍니다.

## 대응할 부트스트랩 추가

Retact 앱에 부트스트랩을 추가하는 가장 일반적인 세 가지 방법은 다음과 같습니다.

- 부트스트랩 사용CDN
- 종속성으로 대응에서 부트스트랩 가져오기
- React 부트스트랩 패키지 설치(예: `bootstrap-react` 또는 `reactstrap`)

### 1. 부트스트랩 사용CDN

부트스트랩CDN은 대응 앱에 부트스트랩을 추가하는 가장 쉬운 방법입니다. 설치 또는 다운로드가 필요하지 않습니다. 다음 조각과 같이 앱의 `<헤드` 섹션에 `<링크>`를 넣기만 하면 됩니다.

```undefined
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.0/css/bootstrap.min.css" 
integrity="sha384-9gVQ4dYFwwWSjIDZnLEWnxCjeSWFphJiwGPXr1jddIhOegiu1FwO5qRGvFXOdJZ4" 
crossorigin="anonymous">
```

부트스트랩과 함께 제공되는 JavaScript 구성 요소를 사용하려는 경우 다음 <script> 태그를 닫는 </body> 태그 바로 앞 페이지 끝에 배치해야 사용할 수 있습니다.

```undefined
<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" 
integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" 
crossorigin="anonymous"></script>

<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.0/umd/popper.min.js" 
integrity="sha384-cs/chFZiN24E4KMATLdqdvsezGxaGsi4hLGOzlXwp5UZB1LY//20VyM2taTB4QvJ" 
crossorigin="anonymous"></script>

<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.0/js/bootstrap.min.js" 
integrity="sha384-uefMccjFJAIv6A+rW+L4AHf99KvxDjWSu1z9VI8SKNVmz4sk7buKt/6v9KI65qnm" 
crossorigin="anonymous"></script>
```

보시는 것처럼 부트스트랩 4에는 JavaScript 구성 요소에 jQuery 및 Popper.js가 필요합니다. 위의 코드 조각에서, 우리는 jQuery의 슬림 버전을 사용했지만, 당신은 풀 버전을 사용할 수 있습니다.

반응 앱의 경우 일반적으로 이러한 코드 조각이 앱의 인덱스 페이지에 추가됩니다. `create-react-app`을 사용하여 앱을 만든 경우 `public/index.html` 페이지는 다음 조각처럼 보입니다.

```xml
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <meta name="theme-color" content="#000000">
    <!--
      manifest.json provides metadata used when your web app is added to the
      homescreen on Android. See https://developers.google.com/web/fundamentals/engage-and-retain/web-app-manifest/
    -->
    <link rel="manifest" href="%PUBLIC_URL%/manifest.json">
    <link rel="shortcut icon" href="%PUBLIC_URL%/favicon.ico">
    <!--
      Notice the use of %PUBLIC_URL% in the tags above.
      It will be replaced with the URL of the `public` folder during the build.
      Only files inside the `public` folder can be referenced from the HTML.
      Unlike "/favicon.ico" or "favicon.ico", "%PUBLIC_URL%/favicon.ico" will
      work correctly both with client-side routing and a non-root public URL.
      Learn how to configure a non-root public URL by running `npm run build`.
    -->
    
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.0/css/bootstrap.min.css" 
    integrity="sha384-9gVQ4dYFwwWSjIDZnLEWnxCjeSWFphJiwGPXr1jddIhOegiu1FwO5qRGvFXOdJZ4" 
    crossorigin="anonymous">
    
    <title>React App</title>
  </head>
  <body>
    <noscript>
      You need to enable JavaScript to run this app.
    </noscript>
    <div id="root"></div>
    <!--
      This HTML file is a template.
      If you open it directly in the browser, you will see an empty page.
      You can add webfonts, meta tags, or analytics to this file.
      The build step will place the bundled scripts into the <body> tag.
      To begin the development, run `npm start` or `yarn start`.
      To create a production bundle, use `npm run build` or `yarn build`.
    -->
    
    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" 
    integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" 
    crossorigin="anonymous"></script>
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.0/umd/popper.min.js" 
    integrity="sha384-cs/chFZiN24E4KMATLdqdvsezGxaGsi4hLGOzlXwp5UZB1LY//20VyM2taTB4QvJ" 
    crossorigin="anonymous"></script>
    
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.0/js/bootstrap.min.js" 
    integrity="sha384-uefMccjFJAIv6A+rW+L4AHf99KvxDjWSu1z9VI8SKNVmz4sk7buKt/6v9KI65qnm" 
    crossorigin="anonymous"></script>
    
  </body>
</html>
```

이제 Retact 앱 구성 요소에서 기본 제공 부트스트랩 클래스 및 JavaScript 구성 요소를 사용할 수 있습니다.

### 2. 종속성으로 부트스트랩 가져오기

빌드 도구 또는 웹 팩과 같은 모듈 번들을 사용하는 경우 이 옵션을 사용하여 대응 응용 프로그램에 부트스트랩을 추가하는 것이 좋습니다. 앱의 종속성으로 부트스트랩을 설치해야 합니다.

```coffeescript
npm install bootstrap
```

또는 Yarn을 사용하는 경우:

```undefined
yarn add bootstrap
```

부트스트랩을 설치한 후에는 앱의 항목 JavaScript 파일에 부트스트랩을 포함합니다. `create-react-app`에 익숙한 경우 이 파일은 `src/index.js` 파일이어야 합니다.

```coffeescript
import 'bootstrap/dist/css/bootstrap.min.css';
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import registerServiceWorker from './registerServiceWorker';

ReactDOM.render(<App />, document.getElementById('root'));
registerServiceWorker();
```

첫 번째 종속성으로 부트스트랩 미니어처 CSS를 가져왔습니다. 이를 통해 Retact 앱 구성 요소에 내장된 부트스트랩 클래스를 계속 사용할 수 있습니다. 그러나 앱에서 부트스트랩의 JavaScript 구성 요소를 사용하려면 먼저 `jquery`와 `popper.js`를 설치해야 합니다.

```coffeescript
npm install jquery popper.js
```

그런 다음 `src/index.js` 파일을 추가로 변경하여 다음 조각에 표시된 것처럼 새 종속성을 추가합니다.

```coffeescript
import 'bootstrap/dist/css/bootstrap.min.css';
import $ from 'jquery';
import Popper from 'popper.js';
import 'bootstrap/dist/js/bootstrap.bundle.min';
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import registerServiceWorker from './registerServiceWorker';

ReactDOM.render(<Dropdown />, document.getElementById('root'));
registerServiceWorker();
```

여기서 우리는 "$"와 "Popper"의 수입을 추가했다. 또한 부트스트랩 JavaScript 미니어처 번들 파일도 가져왔습니다. 이제 Retact 앱에서 부트스트랩 JavaScript 구성 요소를 사용할 수 있습니다.

### 3. 반응 부트스트랩 패키지 설치

반응 앱에 부트스트랩을 추가하는 세 번째 방법은 반응 구성 요소로 작동하도록 설계된 부트스트랩 구성 요소를 재구성한 패키지를 사용하는 것입니다. 가장 인기 있는 두 가지 패키지는 다음과 같습니다.

- ➡➡➡➡
- `스트랩

두 패키지는 모두 Retact 앱과 함께 부트스트랩을 사용하는 데 매우 적합합니다. 그들은 매우 비슷한 특징을 가지고 있다.

#### 기본 제공 부트스트랩 클래스 및 구성 요소 사용

다른 클래스와 마찬가지로 기본 제공 클래스를 적용하여 Retact 앱의 요소 및 구성 요소에 직접 부트스트랩을 사용할 수 있습니다. 간단한 테마 전환기 Retact 구성 요소를 구축하여 부트스트랩 클래스 및 구성 요소를 사용하여 시연해 보겠습니다.

이 데모에 나온 것처럼, 우리는 우리의 테마 스위처를 구현하기 위해 부트스트랩에서 사용할 수 있는 드롭다운 구성 요소를 사용하고 있다. 또한 내장 버튼 클래스를 사용하여 드롭다운 버튼의 크기와 색상을 설정합니다.

우리는 우리의 테마 스위처 컴포넌트의 코드를 작성하기 위해 계속 진행할 것입니다. 대응 응용 프로그램이 이미 설정되어 있는지 확인합니다. 구성 요소에 대한 새 파일을 생성하고 다음 코드 조각을 추가합니다.

```js
import React, { Component } from 'react';

class ThemeSwitcher extends Component {

  state = { theme: null }
  
  resetTheme = evt => {
    evt.preventDefault();
    this.setState({ theme: null });
  }
  
  chooseTheme = (theme, evt) => {
    evt.preventDefault();
    this.setState({ theme });
  }
  
  render() {
  
    const { theme } = this.state;
    const themeClass = theme ? theme.toLowerCase() : 'secondary';
    
    return (
      <div className="d-flex flex-wrap justify-content-center position-absolute w-100 h-100 align-items-center align-content-center">
      
        <span className={`h1 mb-4 w-100 text-center text-${themeClass}`}>{ theme || 'Default' }</span>
        
        <div className="btn-group">
        
          <button type="button" className={`btn btn-${themeClass} btn-lg`}>{ theme || 'Choose' } Theme</button>
          
          <button type="button" className={`btn btn-${themeClass} btn-lg dropdown-toggle dropdown-toggle-split`} data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
            <span className="sr-only">Toggle Theme Dropdown</span>
          </button>
          
          <div className="dropdown-menu">
          
            <a className="dropdown-item" href="#" onClick={e => this.chooseTheme('Primary', e)}>Primary Theme</a>
            <a className="dropdown-item" href="#" onClick={e => this.chooseTheme('Danger', e)}>Danger Theme</a>
            <a class="dropdown-item" href="#" onClick={e => this.chooseTheme('Success', e)}>Success Theme</a>
            
            <div className="dropdown-divider"></div>
            
            <a className="dropdown-item" href="#" onClick={this.resetTheme}>Default Theme</a>
          </div>
          
        </div>
        
      </div>
    );
    
  }
  
}

export default ThemeSwitcher;
```

여기서는 부트스트랩의 드롭다운 구성 요소와 몇 가지 기본 제공 클래스를 활용하여 매우 간단한 테마 스위처 구성 요소를 구축했습니다. 여기서 부트스트랩 드롭다운 사용에 대해 자세히 알아볼 수 있습니다.

먼저 테마 속성을 가진 구성 요소의 상태를 설정하고 Null로 초기화했습니다. 다음으로, 우리는 두 번의 클릭 이벤트 핸들러의 재설정을 정의했다.테마() 및 `선택`테마를 선택하고 테마를 재설정하기 위한 구성요소 클래스의 테마()입니다.

렌더링() 방법에서는 세 가지 테마를 포함하는 분할 버튼 드롭다운 메뉴를 렌더링한다. 즉 경선, 위험, 성공이다. 각 메뉴 항목은 적절한 작업을 위해 클릭 이벤트 핸들러에 부착됩니다. 드롭다운 버튼과 텍스트 모두에 대한 테마 색 클래스를 가져오기 위해 `teme.toLowerCase()`를 사용하는 방법에 주목하십시오. 테마가 설정되지 않은 경우 기본적으로 `보조`를 테마 색상으로 사용합니다.

이 예에서는 Retact 앱에서 부트스트랩의 기본 제공 클래스 및 구성 요소를 얼마나 쉽게 사용할 수 있는지 확인했습니다. 기본 제공 클래스 및 구성 요소에 대한 자세한 내용은 부트스트랩 설명서를 참조하십시오.

#### ➡-➡의 예

이제 우리는 리액트 부트스트랩을 사용하여 테마 스위처를 재구성할 것이다. 우리는 `create-react-app` 명령줄 도구를 사용하여 앱을 만들 것입니다. 컴퓨터에 `react-app 생성` 도구가 설치되어 있는지 확인합니다.

다음과 같이 `create-react-app`을 사용하여 새 React 앱을 만듭니다.

```undefined
create-react-app react-bootstrap-app
```

다음으로 다음과 같이 종속성을 설치합니다.

```undefined
yarn add bootstrap@3 react-bootstrap
```

`react-bootstrap`은 현재 부트스트랩 v3를 대상으로 합니다. 그러나 부트스트랩 v4 지원을 제공하기 위한 작업이 활발하게 진행되고 있습니다.

프로젝트의 `src` 디렉터리에 `ThemeSwitcher.js`라는 새 파일을 만들고 다음 내용을 넣습니다.

```js
import React, { Component } from 'react';
import { SplitButton, MenuItem } from 'react-bootstrap';

class ThemeSwitcher extends Component {

  state = { theme: null }
  
  chooseTheme = (theme, evt) => {
    evt.preventDefault();
    if (theme.toLowerCase() === 'reset') { theme = null }
    this.setState({ theme });
  }
  
  render() {
  
    const { theme } = this.state;
    const themeClass = theme ? theme.toLowerCase() : 'default';
    
    const parentContainerStyles = {
      position: 'absolute',
      height: '100%',
      width: '100%',
      display: 'table'
    };
    
    const subContainerStyles = {
      position: 'relative',
      height: '100%',
      width: '100%',
      display: 'table-cell',
      verticalAlign: 'middle'
    };
    
    return (
      <div style={parentContainerStyles}>
        <div style={subContainerStyles}>
        
          <span className={`h1 center-block text-center text-${theme ? themeClass : 'muted'}`} style={ marginBottom: 25 }>{theme || 'Default'}</span>
          
          <div className="center-block text-center">
            <SplitButton bsSize="large" bsStyle={themeClass} title={`${theme || 'Default'} Theme`}>
              <MenuItem eventKey="Primary" onSelect={this.chooseTheme}>Primary Theme</MenuItem>
              <MenuItem eventKey="Danger" onSelect={this.chooseTheme}>Danger Theme</MenuItem>
              <MenuItem eventKey="Success" onSelect={this.chooseTheme}>Success Theme</MenuItem>
              <MenuItem divider />
              <MenuItem eventKey="Reset" onSelect={this.chooseTheme}>Default Theme</MenuItem>
            </SplitButton>
          </div>
          
        </div>
      </div>
    );
    
  }
  
}

export default ThemeSwitcher;
```

여기서는 `react-bootstrap`을 사용하여 초기 예제를 최대한 모방하려고 했습니다. 우리는 `react-bootstrap` 패키지에서 두 가지 구성 요소인 `SplitButton`과 `MenuItem`을 가져옵니다. 자세한 내용은 `react-bootstrap` 드롭다운 설명서를 참조하십시오.

먼저 테마 속성을 가진 구성 요소의 상태를 설정하고 Null로 초기화했습니다. 다음으로 `선택`을 정의합니다.테마를 선택하거나 테마를 재설정하려면 테마()를 클릭하십시오.

우리는 부트스트랩 3.3.7을 사용하고 있기 때문에 수평 및 수직 중심화를 달성하기 위해 `렌더()` 방식으로 컨테이너 스타일을 만들었다.

bsSize 프로펠러를 사용하여 `SplitButton` 구성 요소의 버튼 크기를 지정하는 방법에 주목하십시오. 또 테마클래스를 bs스타일 소품으로 전환해 국가 테마에 따라 버튼 색상을 역동적으로 변화시키고 있다.

우리는 테마 이름을 각 메뉴 항목의 `eventKey` 받침대에 전달합니다. 또한 `onSelect` 핸들러를 `this.choose`로 설정합니다.앞에서 정의한 테마. 메뉴 항목 구성 요소는 다음과 같이 `eventKey`와 `event` 자체를 `onSelect` 처리기로 전달합니다.

```undefined
(eventKey: any, event: Object) => any
```

마지막으로 `src/index.js` 파일을 다음과 같은 코드 조각처럼 보이도록 수정한다.

```coffeescript
import 'bootstrap/dist/css/bootstrap.min.css';
import 'bootstrap/dist/css/bootstrap-theme.min.css';
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import ThemeSwitcher from './ThemeSwitcher';
import registerServiceWorker from './registerServiceWorker';

ReactDOM.render(<ThemeSwitcher />, document.getElementById('root'));
registerServiceWorker();
```

여기서는 먼저 부트스트랩의 축소 CSS 파일을 가져옵니다. 또한 테마 스위처 구성 요소를 가져와 DOM에 렌더링합니다.

yarn start 또는 npm start 명령을 사용하여 지금 앱을 실행하면 됩니다. 앱은 포트 `3000`에서 시작해야 하며 다음과 같은 데모 모양이어야 합니다.

#### '스트랩' 예

이번에는 리액트 스트랩을 이용해 테마 스위처를 다시 만들겠다. 우리는 `create-react-app` 명령줄 도구를 사용하여 앱을 만들 것입니다. 컴퓨터에 `react-app 생성` 도구가 설치되어 있는지 확인합니다.

다음과 같이 `create-react-app`을 사용하여 새 React 앱을 만듭니다.

```undefined
create-react-app reactstrap-app
```

다음으로 다음과 같이 종속성을 설치합니다.

```undefined
yarn add bootstrap reactstra
```

프로젝트의 `src` 디렉터리에 `ThemeSwitcher.js`라는 새 파일을 생성하고 다음 내용을 넣습니다.

```coffeescript
import React, { Component } from 'react';
import { Button, ButtonDropdown, DropdownToggle, DropdownMenu, DropdownItem } from 'reactstrap';

class ThemeSwitcher extends Component {

  state = { theme: null, dropdownOpen: false }
  
  toggleDropdown = () => {
    this.setState({ dropdownOpen: !this.state.dropdownOpen });
  }
  
  resetTheme = evt => {
    evt.preventDefault();
    this.setState({ theme: null });
  }
  
  chooseTheme = (theme, evt) => {
    evt.preventDefault();
    this.setState({ theme });
  }
  
  render() {
  
    const { theme, dropdownOpen } = this.state;
    const themeClass = theme ? theme.toLowerCase() : 'secondary';
    
    return (
      <div className="d-flex flex-wrap justify-content-center position-absolute w-100 h-100 align-items-center align-content-center">
      
        <span className={`h1 mb-4 w-100 text-center text-${themeClass}`}>{theme || 'Default'}</span>
        
        <ButtonDropdown isOpen={dropdownOpen} toggle={this.toggleDropdown}>
          <Button id="caret" color={themeClass}>{theme || 'Custom'} Theme</Button>
          <DropdownToggle caret size="lg" color={themeClass} />
          <DropdownMenu>
            <DropdownItem onClick={e => this.chooseTheme('Primary', e)}>Primary Theme</DropdownItem>
            <DropdownItem onClick={e => this.chooseTheme('Danger', e)}>Danger Theme</DropdownItem>
            <DropdownItem onClick={e => this.chooseTheme('Success', e)}>Success Theme</DropdownItem>
            <DropdownItem divider />
            <DropdownItem onClick={this.resetTheme}>Default Theme</DropdownItem>
          </DropdownMenu>
        </ButtonDropdown>
        
      </div>
    );
    
  }
  
}

export default ThemeSwitcher;
```

여기서는 react 스트랩을 사용하여 초기 예제를 재구성했습니다. 우리는 리액트 스트랩에서 몇 가지 부품을 수입한다. 자세한 내용은 `react strap` 버튼 드롭다운 설명서를 참조하십시오.

먼저 다음 두 가지 속성을 사용하여 구성 요소의 상태를 설정합니다.

- `➡` 재산이 `➡`로 초기화되다
- dropdown Open은 false로 초기화됩니다. 이 속성은 드롭다운의 토글 상태를 유지하기 위해 `react 스트랩`의 `ButtonDrop down` 구성 요소에 사용됩니다.

또한 드롭다운의 열린 상태를 전환하기 위해 `toggleDropdown()` 방법을 정의한다. 버튼 드롭다운 구성 요소에도 사용됩니다.
reactstrap은 또한 isOpen 프로포트와 toggle 핸들러 프로포트가 작동하지 않아도 되는 제어되지 않는 구성요소 UncontrolleButtonDropdown을 제공한다. 대부분의 경우 버튼 드롭다운 대신 이 기능을 사용할 수 있습니다.

드롭다운 메뉴의 각 항목은 `DropDownItem` 구성 요소를 사용하여 렌더링됩니다. DropdownToggle 구성 요소의 버튼 크기를 `size` prop를 사용하여 지정하는 방법에 주목하십시오. 또한 테마 클래스를 버튼과 드롭 다운 토글 구성 요소의 컬러 소품으로 전달하여 국가 테마에 따라 버튼 색상을 동적으로 변경하고 있습니다.

또한 "선택"을 사용하여 이전과 같이 각 "DropDown Item"에 "onClick" 핸들러를 설정합니다.테마() 및 `리셋`앞에서 정의한 테마() 핸들러입니다.

마지막으로 `src/index.js` 파일을 다음과 같은 코드 조각처럼 보이도록 수정한다.

```coffeescript
import 'bootstrap/dist/css/bootstrap.min.css';
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import ThemeSwitcher from './ThemeSwitcher';
import registerServiceWorker from './registerServiceWorker';

ReactDOM.render(<ThemeSwitcher />, document.getElementById('root'));
registerServiceWorker();
```

여기서는 먼저 부트스트랩 축소 CSS 파일을 가져옵니다. 또한 테마 스위처 구성 요소를 가져와 DOM에 렌더링합니다.

yarn start 또는 npm start 명령을 사용하여 지금 앱을 실행하면 됩니다. 앱은 포트 `3000`에서 시작해야 하며 다음과 같은 데모 모양이어야 합니다.

## 부트스트랩을 사용하여 대응 앱 만들기

좀 더 자세한 정보가 있는 앱을 만들기 위해 이것을 조금 더 진행해보자. 우리는 가능한 한 많은 부트스트랩 클래스 및 구성 요소를 사용할 수 있도록 노력할 것입니다. 우리는 또한 부트스트랩이 부트스트랩 4를 지원하기 때문에 부트스트랩과 리액트를 통합하는 데 리액트스트랩을 사용할 것이다.

우리는 `create-react-app` 명령줄 도구를 사용하여 앱을 만들 것입니다. 컴퓨터에 `react-app 생성` 도구가 설치되어 있는지 확인합니다. 다음은 우리가 무엇을 만들 것인가에 대한 스크린샷입니다.

다음과 같이 `create-react-app`을 사용하여 새 React 앱을 만듭니다.

```undefined
create-react-app sample-app
```

다음으로 다음과 같이 종속성을 설치합니다.

```undefined
yarn add axios bootstrap reactstrap
```

여기서 우리는 의존성으로 `axios`를 설치한다는 것을 주목하라. Axios는 브라우저와 Node.js를 위한 약속 기반 HTTP 클라이언트이다. BaconIpsum JSON API에서 게시물을 가져올 수 있습니다.

부트스트랩 미니어처 CSS 파일을 포함하도록 `src/index.js` 파일을 약간 수정하십시오. 다음과 같은 조각으로 보입니다.

```coffeescript
import 'bootstrap/dist/css/bootstrap.min.css';
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import registerServiceWorker from './registerServiceWorker';

ReactDOM.render(<App />, document.getElementById('root'));
registerServiceWorker();
```

프로젝트의 `src` 디렉토리에 `components`라는 새 디렉토리를 생성합니다. 다음 내용으로 방금 만든 `구성 요소` 디렉토리에 `Header.js`라는 새 파일을 만듭니다.

```xml
import React from 'react';
import logo from '../logo.svg';

import {
  Container, Row, Col, Form, Input, Button, Navbar, Nav,
  NavbarBrand, NavLink, NavItem, UncontrolledDropdown,
  DropdownToggle, DropdownMenu, DropdownItem
} from 'reactstrap';

const AVATAR = 'https://www.gravatar.com/avatar/429e504af19fc3e1cfa5c4326ef3394c?s=240&d=mm&r=pg';

const Header = () => (
  <header>
    <Navbar fixed="top" color="light" light expand="xs" className="border-bottom border-gray bg-white" style={ height: 80 }>
    
      <Container>
        <Row noGutters className="position-relative w-100 align-items-center">
        
          <Col className="d-none d-lg-flex justify-content-start">
            <Nav className="mrx-auto" navbar>
            
              <NavItem className="d-flex align-items-center">
                <NavLink className="font-weight-bold" href="/">
                  <img src={AVATAR} alt="avatar" className="img-fluid rounded-circle" style={ width: 36 } />
                </NavLink>
              </NavItem>
              
              <NavItem className="d-flex align-items-center">
                <NavLink className="font-weight-bold" href="/">Home</NavLink>
              </NavItem>
              
              <NavItem className="d-flex align-items-center">
                <NavLink className="font-weight-bold" href="/">Events</NavLink>
              </NavItem>
              
              <UncontrolledDropdown className="d-flex align-items-center" nav inNavbar>
                <DropdownToggle className="font-weight-bold" nav caret>Learn</DropdownToggle>
                <DropdownMenu right>
                  <DropdownItem className="font-weight-bold text-secondary text-uppercase" header disabled>Learn React</DropdownItem>
                  <DropdownItem divider />
                  <DropdownItem>Documentation</DropdownItem>
                  <DropdownItem>Tutorials</DropdownItem>
                  <DropdownItem>Courses</DropdownItem>
                </DropdownMenu>
              </UncontrolledDropdown>
              
            </Nav>
          </Col>
          
          <Col className="d-flex justify-content-xs-start justify-content-lg-center">
            <NavbarBrand className="d-inline-block p-0" href="/" style={ width: 80 }>
              <img src={logo} alt="logo" className="position-relative img-fluid" />
            </NavbarBrand>
          </Col>
          
          <Col className="d-none d-lg-flex justify-content-end">
            <Form inline>
              <Input type="search" className="mr-3" placeholder="Search React Courses" />
              <Button type="submit" color="info" outline>Search</Button>
            </Form>
          </Col>
          
        </Row>
      </Container>
      
    </Navbar>
  </header>
);

export default Header;
```

이 코드 조각에서 방금 만든 구성 요소는 탐색 메뉴가 포함된 `헤더` 구성 요소입니다. 다음으로, 다음 내용으로 `components` 디렉토리에 `sideCard.js`라는 파일을 새로 만들겠습니다.

```js
import React, { Fragment } from 'react';

import {
  Button, UncontrolledAlert, Card, CardImg, CardBody,
  CardTitle, CardSubtitle, CardText
} from 'reactstrap';

const BANNER = 'https://i.imgur.com/CaKdFMq.jpg';

const SideCard = () => (
  <Fragment>
  
    <UncontrolledAlert color="danger" className="d-none d-lg-block">
      <strong>Account not activated.</strong>
    </UncontrolledAlert>
    
    <Card>
      <CardImg top width="100%" src={BANNER} alt="banner" />
      <CardBody>
        <CardTitle className="h3 mb-2 pt-2 font-weight-bold text-secondary">Glad Chinda</CardTitle>
        <CardSubtitle className="text-secondary mb-3 font-weight-light text-uppercase" style={ fontSize: '0.8rem' }>Web Developer, Lagos</CardSubtitle>
        <CardText className="text-secondary mb-4" style={ fontSize: '0.75rem' }>Full-stack web developer learning new hacks one day at a time. Web technology enthusiast. Hacking stuffs @theflutterwave.</CardText>
        <Button color="success" className="font-weight-bold">View Profile</Button>
      </CardBody>
    </Card>
    
  </Fragment>
);

export default SideCard;
```

그런 다음 `구성 요소` 디렉토리에 `Post.js`라는 새 파일을 만들고 다음 코드 조각을 추가합니다.

```js
import React, { Component, Fragment } from 'react';
import axios from 'axios';
import { Badge } from 'reactstrap';

class Post extends Component {

  state = { post: null }
  
  componentDidMount() {
    axios.get('https://baconipsum.com/api/?type=meat-and-filler&paras=4&format=text')
      .then(response => this.setState({ post: response.data }));
  }
  
  render() {
    return (
      <Fragment>
        { this.state.post && <div className="position-relative">
        
          <span className="d-block pb-2 mb-0 h6 text-uppercase text-info font-weight-bold">
            Editor's Pick
            <Badge pill color="success" className="text-uppercase px-2 py-1 ml-3 mb-1 align-middle" style={ fontSize: '0.75rem' }>New</Badge>
          </span>
          
          <span className="d-block pb-4 h2 text-dark border-bottom border-gray">Getting Started with React</span>
          
          <article className="pt-5 text-secondary text-justify" style={ fontSize: '0.9rem', whiteSpace: 'pre-line' }>{this.state.post}</article>
          
        </div> }
      </Fragment>
    );
  }
  
}

export default Post;
```

여기서 우리는 페이지에 게시물을 렌더링하는 `Post` 구성 요소를 만들었습니다. post 속성이 null로 설정된 구성 요소의 상태를 초기화합니다. 구성 요소가 마운트되면 `axios`를 사용하여 BaconIpsum JSON API에서 4개 단락의 임의의 게시물을 가져오고 상태의 `post` 속성을 업데이트한다.

마지막으로 `src/App.js` 파일을 다음 코드 조각처럼 보이도록 수정합니다.

```js
import React, { Fragment } from 'react';
import axios from 'axios';
import { Container, Row, Col } from 'reactstrap';

import Post from './components/Post';
import Header from './components/Header';
import SideCard from './components/SideCard';

const App = () => (
  <Fragment>
  
    <Header />
    
    <main className="my-5 py-5">
      <Container className="px-0">
        <Row noGutters className="pt-2 pt-md-5 w-100 px-4 px-xl-0 position-relative">
        
          <Col xs={ order: 2 } md={ size: 4, order: 1 } tag="aside" className="pb-5 mb-5 pb-md-0 mb-md-0 mx-auto mx-md-0">
            <SideCard />
          </Col>
          
          <Col xs={ order: 1 } md={ size: 7, offset: 1 } tag="section" className="py-5 mb-5 py-md-0 mb-md-0">
            <Post />
          </Col>
          
        </Row>
      </Container>
    </main>
    
  </Fragment>
);

export default App;
```

여기서는 단순히 `Header`, `SideCard`, `Post` 구성 요소를 `App` 구성 요소에 포함합니다. 부트스트랩에서 제공하는 응답성 유틸리티 클래스를 사용하여 앱을 다양한 화면 크기에 맞게 조정하는 방법에 주목하십시오.

지금 yarn start 또는 npm start라는 명령으로 앱을 실행하면 포트 3000에서 앱이 시작되고 앞에서 본 스크린샷과 똑같이 보입니다.

## 결론

이 튜토리얼에서는 Bootboot를 React 앱과 통합할 수 있는 몇 가지 방법을 살펴봤다. 우리는 또한 어떻게 가장 인기 있는 리액트 부트스트랩 라이브러리인 `리액트 부트스트랩`과 `리액트 스트랩`을 사용할 수 있는지 보았다.

이 튜토리얼에서는 알림, 배지, 드롭다운, 탐색, 폼, 버튼, 카드 등과 같은 몇 가지 부트스트랩 구성 요소만 사용했습니다. 테이블, 모델, 툴팁, 캐러셀, 점보트론, 페이지 지정, 탭 등 몇 가지 부트스트랩 구성 요소가 여전히 있습니다.

이 튜토리얼에서 사용한 패키지의 설명서를 확인하여 사용할 수 있는 더 많은 방법을 찾을 수 있습니다. 이 튜토리얼의 모든 데모 앱의 소스 코드는 GitHub에서 찾을 수 있다.