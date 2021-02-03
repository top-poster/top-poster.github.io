---
layout: post
title: "로컬 우선 개발에 Firebase Emulator Suite 및 React 사용
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/firebase-emulator-local.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/firebase-emulator-local.png?fit=730%2C487&ssl=1)

## Firebase 에뮬레이터 란?
 

Firebase 에뮬레이터 제품군은 프로토 타이핑에 완벽 할뿐만 아니라 Firebase 콘솔과 직접 통신하는 것보다 더 빠르며 개발 환경이 완전히 격리되어 있기 때문에 더 안전합니다.
 프로덕션 데이터를 손상 시키거나 삭제하는 방법), 더 저렴합니다.
 

프로젝트에서 작업하는 개발자 수에 관계없이 Firebase와의 모든 상호 작용은 로컬로 유지됩니다.
 

## 전제 조건
 

이 가이드를 사용하려면 다음이 필요합니다.
 

- React, Firebase 및 명령 줄에 대한 기본 지식.
 
- Firebase 계정.
 
- 시스템에 Java를 설치합니다.
 

## Firebase 에뮬레이터를 React와 통합하는 방법
 

시작하려면`$ yarn create react-app todo && cd my-app`을 사용하여`react-app`을 만들고`$ yarn global add firebase-tools`를 실행하여 전역 적으로`firebase-tools`를 설치합니다.
 `$ firebase login`을 실행하여 자신을 인증하세요.
 

Firebase 프로젝트를 만들려면`$ firebase projects : create`를 실행하고 고유 ID를 입력하세요.
 변수`$ fbid = your-unique-id-here` (`$ fbid`로 사용할 수 있음)에 ID를 저장할 수 있습니다.
 

다음으로`$ firebase apps : create --project $ fbid web my-app`을 실행하여 프로젝트에 Firebase 웹 앱을 만듭니다.
 

반응 앱 디렉토리의 루트 내에서`$ firebase init firestore --project $ fbid`를 실행하여 Firestore를 활성화합니다. 그러면 Firebase 콘솔에서 기능을 활성화하는 링크와 함께 오류가 발생합니다.
 그런 다음`$ firebase init firestore --project $ fbid`를 다시 실행하면 몇 개의 파일이 생성됩니다.
 

이제`$ firebase init emulators --project $ fbid`를 실행하고`Authentication` 및`Firestore` 에뮬레이터를 선택합니다.
 각 에뮬레이터에 대한 기본 포트를 수락하고 에뮬레이터 UI를 활성화하고 에뮬레이터를 다운로드합니다.
 (다시 말하지만 에뮬레이터를 실행하려면 Java가 시스템에 설치되어 있어야합니다.)
 

`.gitignore`에 다음을 추가합니다.
 

```cpp
firebase-debug.log*
firestore-debug.log*
ui-debug.log*
```

그런 다음`firebase`를 종속성으로 설치하십시오.
 

```shell
$ yarn add firebase
```

Firebase 프로젝트와 앱을 초기화 한 후`$ firebase apps : sdkconfig --project $ fbid> src / firebase.js`를 실행하여 앱 구성을`src / firebase.js`에 넣고 다음과 같이 파일을 수정합니다.
 

```js
// src/firebase.js

import firebase from 'firebase/app';
import 'firebase/auth';
import 'firebase/firestore';

// TODO: Use a configuration object
firebaseApp.initializeApp({
  projectId: '',
  appId: '',
  databaseURL: '',
  storageBucket: '',
  locationId: '',
  apiKey: '',
  authDomain: '',
  messagingSenderId: '',
});

const db = firebase.firestore();
const auth = firebase.auth;

// eslint-disable-next-line no-restricted-globals
if (location.hostname === 'localhost') {
  db.useEmulator('localhost', 8080);
  auth().useEmulator('http://localhost:9099/', { disableWarnings: true });
}

export default firebase;
export { db, auth };
```

여기서 주목할만한 부분은`development` 및`test` 환경을 다루는`localhost`에 있는지 확인하고`useEmulator`에`firebase.auth ()`및`firebase.firestore ()`를 알려준다는 것입니다.
 .
 

`auth``useEmulator` 호출에서 경고를 비활성화하여 앱과 에뮬레이터 간의 데이터가 암호화되지 않은 HTTP를 통해 전송되기 때문에 실제 자격 증명 (예 : Google 자격 증명)을 사용하지 않도록 경고합니다.
 

## Firebase 에뮬레이터 실행
 

두 개의 별도 셸에서`$ yarn start` 및`$ firebase emulators : start`를 실행합니다.
 `http : // localhost : 4000 / firestore`에서 Firestore 에뮬레이터 UI를 엽니 다. 다음과 같이 표시됩니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/firebase-emulator-suite-firestore-emulator.png?resize=730%2C410&ssl=1)

또는 npm 스크립트` "emulators": "firebase emulators : start"`를 추가하고`$ yarn emulators`를 사용하여 에뮬레이터를 시작할 수 있습니다.
 

## Firestore 에뮬레이션
 

`src / App.js`에서`db`를 가져오고 일부 데이터를 Firestore Emulator에 씁니다.
 

```js
// src/App.js
import React from "react";
import { db } from "./firebase";

function App() {
  db.doc("hello/world").set({ hello: "world" });
  return <div />;
}

export default App;
```

이제`Firestore` 탭에 우리가 설정 한 데이터가 있어야합니다.
 그렇지 않은 경우 페이지를 새로 고침하거나 다른 곳으로 이동하여 탭으로 돌아갑니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/firestore-emulator-with-written-data.png?resize=730%2C410&ssl=1)

Firestore 에뮬레이터에서 데이터를 읽으려면 먼저 UI에 데이터를 입력하세요.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/writing-firestore-database-data-in-firestore-emulator.gif?resize=730%2C418&ssl=1)

```js
// src/App.js

import React from 'react';
import { db } from './firebase';

function App() {
  const [data, setData] = React.useState();
  React.useEffect(() => {
    db.doc('people/me')
      .get()
      .then((data) => setData(data.data()));
  }, []);

  return <p>{data?.hungry ? "I'm hungry" : "I'm full"}!</p>;
}

export default App;
```

빠른 팁 : 모든 Firestore 메소드를 실행하고 개발 도구에서 직접 로컬 Firestore 인스턴스와 상호 작용할 수 있습니다.
 

## 실시간 데이터베이스 에뮬레이터 UI에서 데이터 가져 오기 및 내보내기
 

기본적으로 에뮬레이션 된 데이터베이스에 기록 된 데이터는 세션간에 유지되지 않지만 실시간 데이터베이스 에뮬레이터 UI에서 데이터를 수동으로 내보내고 가져 오거나 자동으로 수행하도록`emulators` npm 스크립트를 변경할 수 있습니다.
 

```bash
  "emulators": "firebase emulators:start --import=data --export-on-exit",
```

그러면`.gitignore`에 추가 할 수있는`data` 디렉토리가 생성됩니다.
 인증 에뮬레이터가 현재 `firebase-tools`(버전 `9.0.1`)에서 자동 가져 오기 / 내보내기를 지원하지 않는다는 점은 주목할 가치가 있습니다.
 

하지만이 기능은 이미 업스트림에 병합되어 있으므로 세션간에 사용자를 유지할 수있을 때까지 오래 걸리지 않습니다.
 이 기능이 필요하고 기다릴 수없는 경우이 문제에서 다양한 해결 방법을 확인하십시오.
 

## Firebase에서 인증 에뮬레이션
 

인증 에뮬레이터 사용을 시작하려면 Firebase 에뮬레이터가 실행 중인지 확인하고 원하는 공급자를 사용하여 인증 로직을 구현하세요.
 팝업과 함께 Google 인증 공급자를 사용하겠습니다.
 

```js
import React from 'react';
import { auth } from './firebase';

function Login() {
  const login = async () => {
    const provider = new auth.GoogleAuthProvider();
    try {
      await auth().signInWithPopup(provider);
    } catch (e) {}
  };
  return <button onClick={login}>login</button>;
}

export default Login;
```

`로그인`버튼을 클릭하면 이전에 생성 된 사용자가있을 수있는 로그인 페이지로 리디렉션됩니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/authentication-emulator-sign-in-google.png?resize=730%2C410&ssl=1)

다시 말하지만 실제 자격 증명을 입력하지 않는 것이 좋습니다.
 새로 생성 된 사용자는 인증 탭에 나타납니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/authentication-eumulator-google-account.png?resize=730%2C410&ssl=1)

두 가지 방법으로 사용자를 검색 할 수 있습니다.
 로그인에 성공하면 사용자를`auth (). currentUser`로 설정합니다.
 

```undefined
try {
  await auth().signInWithPopup(provider);
  const user = await auth().currentUser;
  setUser(user);
} catch(e) {}
```

또는`useEffect` 후크 내에서 인증 변경을 수신합니다.
 

```coffeescript
React.useEffect(() => {
  return auth().onAuthStateChanged((user) => {
    if (user) {
      setUser(user);
    } else {
      setUser(null);
    }
  });
}, []);
```

어느 쪽이든 인증이 에뮬레이션되고 공급자를 통해 실제 사용자가 생성되지 않습니다.
 Firebase 콘솔에서 Google 공급자를 활성화 할 필요가 없었습니다.
 그러나 배포 전에 그렇게해야합니다.
 

## 결론
 

이 문서에서는 명령 줄을 사용하여 Firebase 프로젝트를 설정하는 방법과 로컬 우선 워크 플로에 Firebase 에뮬레이터를 사용하여 Firestore 및 Firebase 인증을 에뮬레이션하는 방법을 배웠습니다.
 읽어 주셔서 감사합니다.
 