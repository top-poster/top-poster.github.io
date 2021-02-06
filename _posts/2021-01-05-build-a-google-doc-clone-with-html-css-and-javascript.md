---
layout: post
title: "HTML, CSS 및 JavaScript를 사용하여 Google 문서 복제 구축"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/JS-HTML-CSS.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/JS-HTML-CSS.png?fit=730%2C487&ssl=1)

Google 워드프로세서(Google Docs)는 온라인으로 문서를 작성, 편집, 다운로드 및 공유하고 인터넷에 연결되어 있는 한 모든 컴퓨터에서 문서에 액세스할 수 있는 Google의 브라우저 기반 워드 프로세서입니다.

이 튜토리얼에서는 프레임워크 없이 HTML, CSS, JavaScript를 사용하여 Google 워드프로세서 버전을 빌드하는 방법에 대해 알아보겠습니다. 이를 위해 모바일, 웹 및 서버 개발을 위한 유연하고 사용하기 쉬운 데이터베이스인 Firebase Cloud Firestore와 협력할 예정입니다.

시작하기 전에 이 튜토리얼의 전체 코드를 살펴보려면 GitHubrepo에서 확인하십시오.

## 소방서 데이터베이스 구축

먼저 Cloud Firestore 데이터베이스를 구성하는 방법을 살펴보겠습니다. Firebase 프로젝트를 생성하고 프로젝트에 Firebase 앱을 추가한 후 대시보드 사이드바의 Cloud Firestore로 이동합니다. 그런 다음 아래 이미지와 같이 데이터베이스를 구성합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/google-docs-clone-firestore.png?resize=730%2C354&ssl=1)

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/cloud-firestore.png?resize=730%2C354&ssl=1)

방금 우리가 한 일은 파이어스토어 데이터 모델링입니다.

우리는 워드프로세서라는 최상위 컬렉션을 만들었습니다. 문서는 모든 사용자의 문서가 들어 있는 모음입니다.

다음은 그 문서의 고유 식별자입니다. 고유 식별자는 임의 식별자가 아닙니다. 이 프로젝트에 Google 인증을 구현하기 때문에 인증된 사용자로부터 검색되는 생성된 ID입니다.

우리는 또한 문서라는 하위 컬렉션을 만들었습니다. 문서는 인증된 사용자의 문서가 들어 있는 하위 모음입니다.

마지막으로 문서의 고유 식별자입니다. 이 섹션에는 다음과 같은 필드가 있습니다.

- 이름 – 사용자 이름
- 내용 – 문서의 내용
- 작성 – 문서 작성 시점의 타임스탬프
- 업데이트됨 – 문서가 마지막으로 업데이트된 시점의 타임스탬프

이제 클라우드 Firestore 데이터베이스를 구성했으므로 Firebase 프로젝트에서 Firebase Google 인증을 사용할 수 있도록 한 단계 더 발전시켜 보겠습니다.

이렇게 하려면 대시보드 사이드바의 인증 탭으로 이동하여 로그인 방법 탭을 클릭합니다. 그런 다음 Google 섹션을 클릭합니다. 팝업 대화 상자가 나타납니다. 사용 단추를 클릭하고 저장을 클릭하여 변경 내용을 저장합니다.

## 프로젝트 설정

프로젝트 폴더로 이동하여 `signup.html` 파일, `signup.css` 파일, `firebase.js` 및 `code.auth.js` 파일을 생성합니다.

먼저 프로젝트에 Firebase를 추가하겠습니다. Firebase Google 콘솔로 이동하여 프로젝트 구성을 복사한 후 아래 코드와 같이 firebase.js 파일에 코드를 붙여넣습니다.

```undefined
const config = {
        apiKey: 'project key',
        authDomain: 'project.firebaseapp.com',
        databaseURL: 'https://project.firebaseio.com',
        projectId: 'project',
        storageBucket: 'project.appspot.com',
        messagingSenderId: 'projectSenderId',
        appId: 'ProjectId'
};
firebase.initializeApp(config);
```

### Google 추가 인증

`signup.html` 파일에 다음 코드를 작성합니다.

```js
<div class="google">
<button class="btn-google" id="sign-in">
        <img src="image/Google.png" width="24" alt="" />
         <span>Sign up with Google</span>
    </button>
</div>
```

위 코드에는 google 클래스가 있는 div와 btn-google 클래스 버튼이 있다. 우리는 id 가입도 하고 있다.

div는 auth 버튼을 감싼 컨테이너다.

Google 인증 기능을 구현해 보겠습니다. auth.js 파일 내에서 아래의 코드를 복사하여 붙여넣으십시오.

```js
function authenticateWithGoogle() {
    const provider = new firebase.auth.GoogleAuthProvider();
  firebase
  .auth()
  .signInWithPopup(provider)
  .then(function (result) {
          window.location.href = '../index.html';
  })
  .catch(function (error) {
      const errorCode = error.code;
      const errorMessage = error.message;
      const email = error.email;
      const credential = error.credential;
      console.log(errorCode, errorMessage, email,   credential);
  });
}
```

위의 코드에서, 우리는 구글의 authentication()이라는 함수를 만들었다. 이 기능은 사용자가 프로젝트 웹 페이지에서 Google 등록 버튼을 클릭하면 Firebase Google 팝업을 트리거합니다. 등록이 성공하면 개발자 콘솔이 사용자에게 로그인하고 팝업을 닫습니다.

이제 Google 가입이 실행되었으므로 다음 작업으로 넘어가겠습니다.

### 텍스트 편집기 만들기

단어를 입력하고 편집할 수 있는 기본 텍스트 편집기를 만들 것입니다. 그러기 위해 먼저 `editor.html` 파일을 생성하고 아래 코드를 작성하겠습니다.

```js
<div class="edit-content">
  <p class="loading" id="loading">Saving document....</p>
  <div class="editor" contenteditable="true" id="editor"></div>
</div>
```

위의 코드에서 우리는 div를 만들고 속성 `contentable`을 바인딩하고 값을 true로 설정한다. `내용 편집 가능` 속성은 모든 컨테이너를 편집 가능한 텍스트 필드로 변환합니다.

웹 페이지를 보면 디브가 편집 가능한 텍스트 필드로 바뀐 것을 볼 수 있습니다. 다음으로 해야 할 일은 기울임꼴, 굵게, 텍스트 정렬 등과 같은 텍스트 형식 지정 기능을 구현하는 것입니다.

## 텍스트 형식 구현

### 대담한

우리가 구현할 첫 번째 텍스트 형식은 굵게 표시됩니다. 아래 코드를 살펴보겠습니다.

```xml
<a href="javascript:void(0)" onclick="format('bold')">
  <span class="fa fa-bold fa-fw"></span>
</a>
```

위의 코드는 적절한 값을 취하여 함수 호출 시 포맷하는 내장 자바스크립트 함수이다.

### 이탤릭체

```xml
<a href="javascript:void(0)" onclick="format('italic')">
  <span class="fa fa-italic fa-fw"></span>
</a>
```

기울임꼴 함수는 텍스트를 기울임꼴로 표시합니다. 텍스트가 강조 표시될 때마다 함수가 실행됩니다. 함수가 강조 표시되지 않을 때에도 함수는 실행됩니다.

### 순서 없는 목록 및 순서 목록

```xml
<a href="javascript:void(0)" onclick="format('insertunorderedlist')">
  <span class="fa fa-list fa-fw"></span>
</a>
<a href="javascript:void(0)" onclick="format('insertOrderedList')">
  <span class="fa fa-list-ol fa-fw"></span>
</a>
```

순서 없는 목록 함수는 글머리 기호 점을 텍스트에 추가하고 순서 없는 목록 함수는 텍스트에 숫자를 추가합니다.

### 좌파를 정당화하고, 전체를 정당화하고, 중심을 정당화하고, 우파를 정당화하라.

```xml
<a href="javascript:void(0)" onclick="format('justifyLeft')">
  <span class="fa fa-align-left fa-fw"></span>
</a>
          
<a href="javascript:void(0)" onclick="format('justifyFull')">
  <span class="fa fa-align-justify fa-fw"></span>
</a>
          
<a href="javascript:void(0)" onclick="format('justifyCenter')">  
<span class="fa fa-align-center fa-fw"></span>
</a>

<a href="javascript:void(0)" onclick="format('justifyRight')">
  <span class="fa fa-align-right fa-fw"></span>
</a>
```

함수 이름에서 왼쪽 맞춤 함수가 텍스트를 왼쪽으로 정렬한다는 것을 알 수 있다. 기본적으로 모든 텍스트가 왼쪽에 정렬되어 있으므로 변경사항이 표시되지 않을 수 있습니다.

Justify Full은 텍스트를, Justify Center와 Justify Right는 텍스트를 가운데에, Justify Right는 텍스트를 오른쪽에 각각 정렬합니다.

### 밑줄

```xml
<a href="javascript:void(0)" onclick="format('underline')">
  <span class="fa fa-underline fa-fw"></span>
</a>
```

밑줄 함수는 함수가 실행될 때 텍스트의 밑줄을 그립니다.

### 색상 선택, 글꼴 크기 변경 및 글꼴 선택

```xml
<input class="color-apply" type="color" onchange="chooseColor()"
id="myColor"/>
<select id="input-font" class="input" onchange="changeFont (this);">
    <option value="Arial">Arial</option>
    <option value="Helvetica">Helvetica</option>
    <option value="Times New Roman">Times New Roman</option>
    <option value="Sans serif">Sans serif</option>
    <option value="Courier New">Courier New</option>
    <option value="Verdana">Verdana</option>
    <option value="Georgia">Georgia</option>
    <option value="Palatino">Palatino</option>
    <option value="Garamond">Garamond</option>
    <option value="Comic Sans MS">Comic Sans MS</option>
    <option value="Arial Black">Arial Black</option>
    <option value="Tahoma">Tahoma</option>
    <option value="Comic Sans MS">Comic Sans MS</option>
</select>
<select id="fontSize" onclick="changeSize()">
    <option value="1">1</option>
    <option value="2">2</option>
    <option value="3">3</option>
    <option value="4">4</option>
    <option value="5">5</option>
    <option value="6">6</option>
    <option value="7">7</option>
    <option value="8">8</option>
</select>
```

글꼴 크기 변경은 다양한 글꼴 크기를 표시하고 선택한 크기의 값을 취하는 선택 드롭다운입니다. 강조 표시된 텍스트에 적용되며, `글꼴 선택` 기능은 다양한 글꼴을 보여주는 선택 드롭다운이다. 선택한 글꼴 값을 가져와서 강조 표시된 텍스트에 적용합니다.

위의 형식 설정이 구현된 경우 다음과 같은 것이 있어야 합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/doc-with-tools.png?resize=730%2C326&ssl=1)

위의 이미지에서는 다른 텍스트 형식 옵션을 가진 편집기 필드와 도구 모음을 볼 수 있습니다.

이 때 텍스트에 서로 다른 텍스트 형식을 입력하고 구현하면 아무 일도 일어나지 않습니다. 그 이유는 아직 JavaScript 내장 기능 또는 문서와 명령을 가져와 문서에 적용하는 도우미 기능을 구현하지 않았기 때문입니다.

## 도우미 기능 구현

main.js 파일을 생성하고 아래 코드를 작성합니다.

```js
function format(command, value) { 
  document.execCommand(command, false, value);
}
```

텍스트 형식을 클릭할 때마다 `형식` 기능이 실행됩니다. 함수는 명령과 값의 두 가지 인수를 사용합니다. 명령은 실행 중인 텍스트 형식의 이름이며, 값은 강조 표시된 텍스트입니다.

document.execCommand는 사용자 상호 작용의 일부로 호출된 경우에만 true를 반환합니다.

### 글꼴 크기 변경을 위한 도우미 기능 구현 및 글꼴 선택

```js
function changeFont() {
  const Font = document.getElementById('input-font').value;
  document.execCommand('fontName', false, Font);
}

function changeSize() {
  const size = document.getElementById('fontSize').value;
  document.execCommand('fontSize', false, size);
}
```

첫 번째 도우미 함수는 `changeFont` 함수입니다. 이 기능은 글꼴 변경 형식을 실행할 때 실행됩니다. 선택한 글꼴을 가져와서 강조 표시된 텍스트에 적용합니다.

두 번째 함수는 ChangeSize 함수이다. ChangeFont 기능과 동일하게 동작하지만, 강조 표시된 텍스트의 글꼴 크기를 변경한다는 점이 다릅니다.

텍스트를 입력하고 형식 옵션을 적용하면 강조 표시된 텍스트에 적용된 형식을 볼 수 있습니다.

이제 텍스트 편집기와 몇 가지 텍스트 형식을 구현했습니다. 다음으로 살펴볼 것은 구조화된 Firebase Cloud Firestore Database에 문서를 저장하는 방법입니다.

### 사용자 문서를 Cloud Firestore에 저장

사용자가 문서를 만들 때 문서를 Firestore에 저장할 수 있는 방법에 대해 알아보겠습니다. `contentable` 속성을 가진 편집 가능한 div를 생성할 때 `id` 속성을 부여했다는 점을 기억하실 수 있습니다. 사용자가 id 속성을 사용하여 문서를 만드는 동안 편집 가능한 div를 듣고 값을 얻을 것입니다.

먼저, 우리는 사용자의 허가 여부를 확인하려고 합니다. 사용자에게 권한이 부여되면 사용자의 ID를 가져와 main.js 폴더 내의 변수에 할당합니다.

```js
let userId = '';
let userName = '';
firebase.auth().onAuthStateChanged(function(user) {
  if (user) {
    userId = user.uid;
    userName = user.displayName;
    init();
  } else {
    console.log(user + '' + 'logged out');
  }
});

function init(){
  const token = localStorage.getItem('token');
  if(!token){
    const docId = firebase.firestore().collection('docs')
                                      .doc(userId)
                                      .collection('documents')
                                      .doc().id;
      localStorage.setItem('token', docId);
    }else{
        delay(function(){
          getSingleDocDetails(token);
        }, 1000 );
    }
}
```

`폭격기`요auth(.onAuthStateChanged`) 기능은 사용자가 로그인했는지 여부를 확인하는 Firebase 기능입니다. 사용자가 존재하면 user.id을 얻어 위에 생성된 userId라는 변수에 id를 할당한다.

init() 함수는 localStorage에 저장된 문서 id가 있는지 확인합니다. 없으면 Firestore에서 doc `id`를 만들어 local storage 안에 설정한다. 있는 경우 getSingleDocDetails() 함수를 호출합니다. 그러나 GetSingleDoc Details() 기능은 나중에 살펴보겠습니다.

사용자 문서를 가져와서 저장하는 방법을 살펴보겠습니다.

main.js 폴더 안에 아래의 코드를 쓰십시오.

```js
const editor = document.getElementById('editor');
let dos = '';

editor.addEventListener('input', e => {
  dos = e.target.innerHTML;
  delay(function(){
    addDoc(word);
  }, 1000 );
});

var delay = (function(){
  var timer = 0;
  return function(callback, ms){
    clearTimeout (timer);
    timer = setTimeout(callback, ms);
  };
})();
```

우리는 `편집자`라는 변수를 만들었습니다. 우리가 할당한 id 속성을 사용하여 `내용 가능한` 속성을 가진 `div`로 값을 할당합니다.

document.getElementByID는 전달된 ID 이름의 HTML 태그를 검색합니다.

다음으로, 사용자가 이벤트 수신기를 `editor.addEventListener(input, (e))`라고 불러 입력을 시작한 시점을 파악하기 위해 `div`를 들었다.

.addEventListener(입력, (e))` 이벤트는 편집 가능한 파일 내에서 변경된 내용을 수신합니다. 변경이 완료되면 div `innerHtml`을 대상으로 하고 값을 함수에 매개 변수로 전달하였다.

참고: `.inner`를 사용했습니다.div와 함께 작업하기 때문에 HTML이 아닌 .value입니다.

우리는 `지연()` 함수라고도 합니다. 지연()은 클라우드 Firestore에 데이터를 저장하기 전에 사용자가 입력을 마칠 때까지 기다리도록 `addDoc()` 기능을 중지하는 기능입니다.

### addDoc() 함수 호출

```js
function addDoc(word) {

  const docId = localStorage.getItem('token');

    firebase
    .firestore()
    .collection('docs').doc(userId)
    .collection('documents').doc(docId).set({
      name: userName,
      createdAt: new Date(),
      updated: new Date(),
      content: word,
    })
    .then(() => {
      loading.style.display = 'none';
    })
    .catch(function(error) {
      console.error('Error writing document: ', error);
    });
}
```

addDoc() 기능 내에서는 먼저 로컬 스토리지에서 생성한 ID를 얻습니다. 다음으로 우리는 Firebase 쿼리 함수 `.set()를 호출하고 첫 번째 `.doc() 메소드에서 현재 로그인한 사용자의 가이드를 인수로 전달하며 두 번째 `.doc()에서 인수로 생성된 `docId`도 전달한다.

우리는 문서 이름을 현재 로그인한 사용자의 `userName`으로 설정했습니다. 그런 다음 이 개체가 새 `날짜()` 개체로 생성됩니다. 그런 다음 새 `날짜` 개체로 업데이트됩니다. 마지막으로, 사용자가 작성한 문서로 내용이 업데이트됩니다.

Firestore 데이터베이스를 확인하면 저장된 문서가 표시됩니다.

다음으로 살펴볼 것은 Cloud Firestore에서 데이터를 검색하는 방법입니다.

## Cloud Firestore에서 사용자 문서 가져오기

사용자 문서를 가져오기 전에 대시보드 페이지를 구현하려고 합니다. 아래에 코드를 쓰십시오.

```xml
<nav class="navbar">
      <div class="nav-col-logo">
        <a href="#"><i class="fa fa-book"></i> GDocs</a>
      </div>
      <div class="nav-col-input">
        <form id="searchForm">
          <input type="search" placeholder="Search" id="search" />
        </form>
      </div>
      <div class="nav-col-img">
        <a href="#"><i class="fa fa-user"></i></a>
      </div>
    </nav>
    <div class="documents">
      <div class="section group">
        <div class="col span_1_of_3"><h4>Today</h4></div>
        <div class="col span_1_of_3"><h4>Owned by anyone</h4></div>
        <div class="col span_1_of_3"><h4>Last opened</h4></div>
      </div>
      <div id="documents"></div>
    </div>
    <div class="creat-new-doc">
      <button class="btn-color" type="button" id="createNewDoc">
        +
      </button>
    </div>
```

위의 코드가 구현되었다면, 우리는 우리의 웹 페이지에 아래 그림과 같은 것을 가지고 있어야 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/goat-milk.png?resize=730%2C328&ssl=1)

위의 이미지에서는 파란색 배경의 버튼을 볼 수 있습니다. 이 단추를 누르면 사용자가 새 문서를 만들 수 있는 편집기 페이지로 이동합니다. 위의 기본 데이터는 사용자가 작성한 문서를 가져와 클라우드 방화벽에 저장한 후 문서 레이아웃이 표시되는 방식을 보여줍니다.

### 실제 데이터 가져오기

아래에 코드를 쓰십시오.

```js
let holdDoc = [];
function getDocuments(id) {
  // eslint-disable-next-line no-undef
  let db = firebase.firestore()
    .collection('docs')
    .doc(id)
    .collection('documents');
    db.get()
    .then((querySnapshot) => {
      querySnapshot.forEach(function(doc) {
        let dcus = doc.data();
        dcus.id = doc.id;
        holdDoc.push(dcus);
        showDoc();
      });
    });
}
```

getDocument() 함수를 만들었습니다. 함수 내부에서는 `.get() 방법을 사용하여 Firebase Firestore를 쿼리했습니다. 우리는 그것을 우리가 얻은 객체에서 반복하여 우리가 만든 문서뿐만 아니라 빈 배열로 밀어넣습니다. 그런 다음 실제 데이터를 표시하는 showDoc() 함수를 호출했습니다.

이제 실제 데이터를 표시해 보겠습니다.

```coffeescript
const docBook = document.getElementById('documents');
function showDoc() {
  docBook.innerHTML = null;
  for (let i = 0; i < holdDoc.length; i++){
    let date = new Date( holdDoc[i].updated.toMillis());
    let hour = date.getHours();
    let sec = date.getSeconds();
    let minutes = date.getMinutes();
    var ampm = hour >= 12 ? 'pm' : 'am';
    hour = hour % 12;
    hour = hour ? hour : 12;
    var strTime = hour + ':' + minutes + ':' + sec + ' ' + ampm;
    let subString = holdDoc[i].content.replace(/^(.{14}[^\s]*).*/, '$1');
    docBook.innerHTML += `
      <div class="section group">
        <div class="col span_1_of_3">
          <p><a id="${holdDoc[i].id}" onclick="getSingleDocId(id)">
            <i class="fa fa-book"></i> 
              ${subString}  
            <i class="fa fa-users"></i>
          </a></p>
        </div>
        <div class="col span_1_of_3">
          <p>${holdDoc[i].name}</p>
        </div>
        <div class="col span_1_of_3">
          <div class="dropdown">
            <p> ${strTime} 
              <i class="fa fa-ellipsis-v dropbtn" 
                onclick="myFunction()" >
              </i>
            </p>
            <div id="myDropdown" class="dropdown-content">
              <a href="#" target="_blank" >Delete Doc</a>
              <a href="#">Open in New Tab</a>
            </div>
          </div>
        </div>
      </div>
       `;
  }
}
```

먼저 문서를 표시할 div의 ID를 얻습니다. 그 후, 우리는 showDoc() 함수를 호출했습니다. showDoc() 함수 내에서는 먼저 우리가 얻은 객체를 순환한 다음 생성한 변수에 추가합니다. 웹 페이지를 로드하면 표시되는 데이터를 볼 수 있습니다.

다른 하나는 문서를 업데이트하거나 문서를 편집하는 방법입니다.

```js
function getSingleDocId(id){
  console.log(id);
  localStorage.setItem('token', id);
  window.location.href = '../editor.html';
}
```

우리가 작성한 showDoc() 함수를 보면 함수 안의 id를 매개 변수로 전달하는 것을 알 수 있다. 그리고 나서 우리는 그 기능을 밖으로 불렀다. 기능 내부에서는 id를 얻어 local storage에 저장한다. 그런 다음 사용자를 편집기 페이지로 이동할 수 있습니다.

`editor.js` 페이지 내에서 아래 코드를 작성합니다.

```js
function getSingleDocDetails(docId){
  firebase
    .firestore()
    .collection('docs')
    .doc(userId)
    .collection('documents')
    .doc(docId)
    .get()
    .then((doc) => {
      if (doc.exists) {
        editor.innerHTML += doc.data().content;
      } else {
        console.log('No such document!');
      }
    }).catch(function(error) {
      console.log('Error getting document:', error);
    });
}
```

편집기 페이지 내에서 "localStorage"에 "id"가 저장되어 있는지 확인하는 "init()" 함수를 정의합니다. 이 경우 getSignleDocDetails() 함수를 호출하고 클라우드 Firestore에서 문서를 가져와 사용자가 계속하도록 표시합니다.

사용자가 변경할 경우 문서가 업데이트됩니다.

### 온라인 및 오프라인 편집

온라인 및 오프라인 편집을 구현하는 방법을 살펴보겠습니다. 사용자가 오프라인으로 전환하더라도 사용자의 문서를 저장할 수 있고 사용자의 작업 중단 없이 다시 온라인 상태가 되면 Firebase와 동기화할 수 있기를 원합니다. 이를 위해 아래 코드를 작성하십시오.

```js
function updateOnlineStatus() {
  }
updateOnlineStatus();
```

위의 코드에서, 우리는 먼저 `업데이트 온라인 상태()`라는 함수를 만들고 스코프 밖의 함수를 호출했습니다. 이제 아래 코드와 같이 사용자 문서를 가져오는 방법을 복사하여 기능 내부에 붙여 넣을 것입니다.

```php
function updateOnlineStatus() {
  editor.addEventListener('input', e => {
      dos = e.target.innerHTML;
      delay(function(){
        addDoc(dos);
      }, 1000 );
    });
  }
```

그 후, 우리는 사용자가 온라인과 오프라인일 때를 추적하기 위해 브라우저를 들을 것입니다. 아래에 코드를 쓰십시오.

```php
editor.addEventListener('input', e => {
      dos = e.target.innerHTML;
      delay(function(){
        addDoc(dos);
      }, 1000 );
   if (navigator.onLine === true) {
      const word =  localStorage.getItem('document');
      addDoc(word);
      localStorage.removeItem('document');
      
      return;
    } else {
      localStorage.setItem('document', dos);
      
      return;
    }
      
});
```

우리는 if라는 문구를 사용하여 `filename.online ===`이 참인지 확인했습니다. navigator.online은 참 또는 거짓 값을 반환하는 속성입니다. True 값은 사용자가 온라인 상태일 때 반환되고, False 값은 사용자가 오프라인 상태일 때 반환됩니다.

문서를 편집하거나 작성하는 동안 사용자가 온라인 상태인지 확인하는 조건을 설정합니다. 사용자가 온라인 상태이면 로컬 저장소에서 문서를 가져와 클라우드 Firestore로 전송하지만, 사용자가 오프라인 상태이면 계속해서 문서를 로컬 저장소에 저장합니다.

## 결론

이 기사에서는 기본 텍스트 편집기를 만드는 방법에 대해 배웠습니다. 또한 클라우드 Firestore 데이터베이스의 구조화 방법, Firebase `.set()` 방법 사용 방법, 온라인과 오프라인 편집 통합 방법 등을 이해할 수 있었습니다. 이 튜토리얼의 전체 코드는 GitHub에서 찾을 수 있습니다.