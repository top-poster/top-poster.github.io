---
layout: post
title: "리액션 및 파이어베이스를 이용한 푸시 알림"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/pushnotificationswithreactandfirebase.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/pushnotificationswithreactandfirebase.png?fit=730%2C487&ssl=1)

## 도입

파이어베이스는 구글이 2014년 인수한 기업이다. 그 이후, 구글은 이제 파이어베이스를 모바일 앱을 위한 원스톱 백엔드 서비스형 솔루션으로 채택할 정도로 플랫폼을 여러 차례 개선했다. 파이어베이스 우산에 속하는 중앙 집중식 인증, 실시간 데이터베이스, 클라우드 기능(구글의 AWS 람다 등가)과 같은 여러 구글 솔루션 중 앱 알림 관리를 위한 Go to 솔루션으로 꼽힌 솔루션이 있다. 이는 이전에 Google 클라우드 메시징(GCM)이었던 FCM(Firebase Cloud Messaging)입니다. 오늘 기사에서는 프런트 엔드 리액션 응용 프로그램에서 푸시 알림 기능을 활성화하기 위한 프로세스에 대해 알아보겠습니다.

## 리액트 앱

### 기본 앱

이 기사의 목적상, 우리는 `react-app` 생성-react-pm 패키지로 만들어진 스켈레톤 리액트 앱을 사용할 것이다. 이제 `firebase-notifications`라는 폴더를 만들고 그 안에서 명령을 실행합니다(npm 및 npx가 설치되어 있는지 확인).

```undefined
npx create-react-app fire_client
```

이렇게 하면 기본 반응 앱이 만들어집니다. 앱을 실행하려면 `cd`를 폴더에 넣고 `npm run start`를 실행하여 로컬 호스트에서 앱이 시작되고 다음과 같은 친숙한 페이지가 나타납니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/react-app-start-localhost.png?resize=730%2C367&ssl=1)

### 라이브러리 통합

이제 몇 가지 부트스트랩 기반 React UI 구성 요소를 얻기 위해 Firebase Cloud Messaging의 클라이언트 측 기능과 `react-bootstrap`을 통합하는 종속성을 추가해 보겠습니다.

```undefined
npm install --save firebase react-bootstrap bootstrap
```

## 부트스트랩 설정

작업이 완료되면 `src/App.js` 파일을 다음과 같이 변경합니다.

```coffeescript
import logo from './logo.svg';
import './App.css';
import {useState} from 'react';
import { getToken, onMessageListener } from './firebase';
import {Button, Row, Col, Toast} from 'react-bootstrap';
import 'bootstrap/dist/css/bootstrap.min.css';

function App() {

  getToken();

  const [show, setShow] = useState(false);

  onMessageListener().then(message => {
    setShow(true);
  }).catch(err => console.log('failed: ', err));

  return (
    <div className="App">
        <Toast onClose={() => setShow(false)} show={show} delay={3000} autohide animation style={
          position: 'absolute',
          top: 20,
          right: 20,
        }>
          <Toast.Header>
            <img
              src="holder.js/20x20?text=%20"
              className="rounded mr-2"
              alt=""
            />
            <strong className="mr-auto">Notification</strong>
            <small>12 mins ago</small>
          </Toast.Header>
          <Toast.Body>There are some new updates that you might love!</Toast.Body>
        </Toast>
      <header className="App-header">


        <img src={logo} className="App-logo" alt="logo" />
        <Button onClick={() => setShow(true)}>Show Toast</Button>
      </header>


    </div>
  );
}

export default App;
```

우리는 `App.js`에서 부트스트랩 CSS를 가져왔으며, 또한 3초 후에 자동으로 숨기는 토스트 알림을 불러오는 쇼 토스트 버튼이 있다. 이 방법은 다음과 같습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/toast-notification.gif?resize=730%2C536&ssl=1)

이 장치가 설치되면 이제 Firebase 설정으로 이동할 수 있습니다.

## 파이어베이스 설정

### 프로젝트 생성

Firebase 콘솔에 로그인/가입하고 프로젝트 추가를 클릭합니다. 다음 단계에 따라 프로젝트를 추가하십시오.

- 프로젝트에 적절한 이름 지정
- 사용자의 선호도에 따라 분석 사용/사용 안 함
- 설치가 완료될 때까지 기다립니다.

작업이 완료되면 다음과 같은 모양의 프로젝트 홈 화면으로 리디렉션하십시오.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/project-home-screen.png?resize=730%2C375&ssl=1)

### 프로젝트 링크

이제 Firebase 프로젝트와 방금 만든 프런트 엔드 리액트 앱 사이에 링크를 만들어야 합니다. 우리는 파이어베이스 프로젝트 구성을 사용하여 그렇게 할 것입니다. 이전 스크린샷에서 볼 수 있는 IOS, Android 및 웹 옵션 중에서 웹(/>) 아이콘을 클릭합니다. 등록 앱 UI로 이동합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/register-app-page.png?resize=730%2C427&ssl=1)

이전 단계에서 생성한 Retact 앱에 닉네임을 할당하고 Register app 버튼을 클릭합니다. 화면에서 곧 Retact 앱에 통합될 구성을 생성하는 동안 잠시 기다려 주십시오. 최종 생성된 구성은 다음과 같습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/final-config.png?resize=730%2C432&ssl=1)

"firebaseConfig" 객체는 이 특정 firebase project와 연결되는 우리의 react 앱에 통합될 객체이다.

## 프로젝트 연결

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/react-firebase-projects-linked.png?resize=730%2C280&ssl=1)

`firebase`라는 새 파일을 만들어 보겠습니다.js`는 React 코드베이스에 있으며 다음과 같은 코드 라인을 추가합니다.

```js
import firebase from 'firebase/app';
var firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_AUTH_DOMAIN",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_STORAGE_BUCKET",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID"
};

firebase.initializeApp(firebaseConfig);
```

`firebaseConfig`를 이전 단계에서 생성된 것으로 교체해야 합니다. 이제 App.js에서 firebase.js 파일을 가져오면 두 프로젝트가 함께 연결됩니다. 이제 Firebase에서 클라우드 메시징 설정으로 이동하겠습니다.

## 클라우드 메시징 통합

다음 단계로 웹 푸시 인증서 키를 생성해야 합니다. 프로젝트의 클라우드 메시징 탭으로 이동하고 웹 구성 섹션으로 스크롤합니다. 웹 푸시 인증서에서 키 쌍 생성을 클릭합니다. 생성되는 키를 기록해 두십시오.

`firebase.js` 파일로 돌아가 이제 메시징을 사용하도록 설정해야 합니다. 가져오기에 이 줄을 추가합니다.

```coffeescript
import 'firebase/messaging';
```

그런 다음 다음과 같은 `firebase` 개체에서 메시징 개체에 액세스할 수 있습니다.

```cpp
const messaging = firebase.messaging();
```

### 알림 권한 및 클라이언트 등록

브라우저에 푸시 알림을 보내려면 먼저 사용자로부터 권한을 받아야 합니다. 그러면 이 사이트에서 알림 사용 설정이 열리나요? 팝업 창이 표시됩니다. 이 요청을 시작하는 방법은 get을 호출하는 것입니다.방화벽이 제공하는 토큰 메서드입니다.

그 전에 알림에 액세스할 수 있는지 여부를 추적하는 `App.js` 파일의 상태에 변수를 추가하십시오.

```js
const [isTokenFound, setTokenFound] = useState(false);
getToken(setTokenFound);

// inside the jsx being returned:
{isTokenFound && <h1> Notification permission enabled 👍🏻 </h1>}
{!isTokenFound && <h1> Need notification permission ❗️ </h1>}
```

get을 추가하겠습니다.토큰의 기능을 방화벽에 적용합니다.js:

```coffeescript
export const getToken = (setTokenFound) => {
  return messaging.getToken({vapidKey: 'GENERATED_MESSAGING_KEY'}).then((currentToken) => {
    if (currentToken) {
      console.log('current token for client: ', currentToken);
      setTokenFound(true);
      // Track the token -> client mapping, by sending to backend server
      // show on the UI that permission is secured
    } else {
      console.log('No registration token available. Request permission to generate one.');
      setTokenFound(false);
      // shows on the UI that permission is required 
    }
  }).catch((err) => {
    console.log('An error occurred while retrieving token. ', err);
    // catch error while creating client token
  });
}
```

`세트`를 어떻게 통과하는지 주목해 보십시오.get에 대한 토큰 찾기 기능토큰은 클라이언트 토큰(즉, 알림 권한 제공 여부)을 추적할 수 있도록 기능합니다. 지금 코드를 저장하고 실행하면 알림을 요청하는 팝업이 표시됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/popup-requesting-notifications.gif?resize=730%2C453&ssl=1)

`메시지. 받아.토큰`은 알림을 위한 UI를 표시하고 사용자 입력을 기다렸다가 허용할 경우 콘솔에서 볼 수 있는 클라이언트에 고유 토큰을 할당합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/unique-token.png?resize=730%2C342&ssl=1)

## 메시지 수신기 구성

### 백그라운드 수신기

이제 브라우저 측에 알림 권한과 클라이언트 토큰에 대한 참조가 있으므로, 다음 단계는 클라이언트를 향한 수신 푸시 알림에 수신기를 추가하는 것입니다. 이를 위해 응답 앱의 공용 폴더에 `firebase-messaging-sw.js` service-worker 파일을 추가하고 다음 코드를 추가합니다.

```js
// Scripts for firebase and firebase messaging
importScripts('https://www.gstatic.com/firebasejs/8.2.0/firebase-app.js');
importScripts('https://www.gstatic.com/firebasejs/8.2.0/firebase-messaging.js');

// Initialize the Firebase app in the service worker by passing the generated config
var firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_AUTH_DOMAIN",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_STORAGE_BUCKET",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID"
};

firebase.initializeApp(firebaseConfig);

// Retrieve firebase messaging
const messaging = firebase.messaging();

messaging.onBackgroundMessage(function(payload) {
  console.log('Received background message ', payload);

  const notificationTitle = payload.notification.title;
  const notificationOptions = {
    body: payload.notification.body,
  };

  self.registration.showNotification(notificationTitle,
    notificationOptions);
});
```

이 코드는 응용 프로그램이 포그라운드에 없을 때(현재 브라우저 탭에 표시됨) 응용 프로그램에 도달하는 모든 알림을 처리합니다.

## 전경 수신기

앱이 포그라운드에서 활성화된 경우를 처리하려면 이 코드를 `firebase.js` 파일에 추가해야 합니다.

```coffeescript
export const onMessageListener = () =>
  new Promise((resolve) => {
    messaging.onMessage((payload) => {
      resolve(payload);
    });
});
```

또한 이것을 `App.js`로 가져와 구문 분석된 페이로드로 알림을 생성할 수 있는 논리를 추가해야 합니다.

```xml
function App() {

  const [show, setShow] = useState(false);
  const [notification, setNotification] = useState({title: '', body: ''});
  const [isTokenFound, setTokenFound] = useState(false);
  getToken(setTokenFound);

  onMessageListener().then(payload => {
    setShow(true);
    setNotification({title: payload.notification.title, body: payload.notification.body})
    console.log(payload);
  }).catch(err => console.log('failed: ', err));

  return (
    <div className="App">
        <Toast onClose={() => setShow(false)} show={show} delay={3000} autohide animation style={
          position: 'absolute',
          top: 20,
          right: 20,
          minWidth: 200
        }>
          <Toast.Header>
            <img
              src="holder.js/20x20?text=%20"
              className="rounded mr-2"
              alt=""
            />
            <strong className="mr-auto">{notification.title}</strong>
            <small>just now</small>
          </Toast.Header>
          <Toast.Body>{notification.body}</Toast.Body>
        </Toast>
      <header className="App-header">
        {isTokenFound && <h1> Notification permission enabled 👍🏻 </h1>}
        {!isTokenFound && <h1> Need notification permission ❗️ </h1>}
        <img src={logo} className="App-logo" alt="logo" />
        <Button onClick={() => setShow(true)}>Show Toast</Button>
      </header>


    </div>
  );
}
```

그리고 우리는 모두 리액트 앱에서 전경 및 배경 알림을 받을 준비가 되어 있습니다.

## 푸시 알림 테스트

이제 앱의 클라우드 메시징 섹션을 방문하여 테스트 알림을 트리거함으로써 알림 기능이 작동하는지 테스트할 수 있습니다.

- 알림 탭에서 새 알림을 누르십시오.
- 알림 제목 및 알림 텍스트 입력
- 장치 미리 보기 섹션에서 테스트 메시지 보내기를 클릭합니다.
- 열리는 팝업에서 콘솔에 FCM 등록 토큰으로 기록된 클라이언트 토큰을 입력하고 + 버튼을 누릅니다.
- FCM 토큰이 선택되어 있는지 확인하고 "테스트"를 클릭합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/test-device-ui.png?resize=730%2C438&ssl=1)

응답 앱이 포그라운드 브라우저 탭에서 열려 있는 경우 방금 채워진 내용이 포함된 메시지 팝업이 표시됩니다. 탭이 백그라운드에 있는 경우 다음과 같은 기본 알림이 표시됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/default-notification.png?resize=730%2C343&ssl=1)

이 테스트가 완료되고 원하는 대로 메시지가 나타나면 앱을 선택하고 캠페인을 만들어 푸시 알림을 구성하는 프로세스를 계속할 수 있습니다. 이 경우, 해당 앱의 모든 사용자는 방금 테스트한 것과 유사한 푸시 알림을 받게 됩니다.

## 결론

Firebase를 React 앱에 통합하고 전경과 배경 메시지를 모두 구성하는 단계를 다루었습니다. 이 흐름에서는 Firebase의 클라우드 메시징 패널을 통해 알림을 트리거합니다. 다른 방법은 클라이언트-토큰 매핑을 백엔드 서버에 저장한 후 통지를 사용하도록 설정한 후 나중에 특정 토큰을 대상으로 하여 `firebase-admin` 라이브러리를 사용하여 통지를 트리거하는 것입니다. 그러면 통지에 대한 보다 세분화된 제어가 가능해집니다. 그러나 클라이언트 측 장치(iOS, Android 또는 웹)에 푸시 알림을 보내는 한, Firebase 플랫폼을 사용하는 것이 바람직하다. 다음 번에 프로젝트에서 이와 같은 요구 사항을 발견하면 시도해 보십시오. 이 GitHub 저장소에서 전체 코드를 찾습니다.