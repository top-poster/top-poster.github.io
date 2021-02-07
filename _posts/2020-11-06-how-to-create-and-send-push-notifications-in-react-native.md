---
layout: post
title: "React Native에서 푸시 알림을 생성하고 보내는 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/React-Native-Push-Notifications-Using-Expo.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/React-Native-Push-Notifications-Using-Expo.png?fit=730%2C487&ssl=1)

푸시 알림(앱을 설치한 사용자에게 전송되는 앱의 메시지)은 비즈니스 및 사용자용 애플리케이션을 구축할 때 고려해야 할 중요한 요소가 되었습니다.

그것들은 SMS 메시지와 비슷하지만, 보내는 데 비용이 들지 않습니다. 그 결과, 이제 많은 기업이 앱이 설치된 사용자에게 정보 및 알림을 보내기 위해 푸시 알림을 선호합니다. 이 기사에서는 React Native의 푸시 알림을 살펴보겠습니다.

## 푸시 알림이란?

푸시 알림은 사용자가 설치한 응용 프로그램의 메이커가 보낸 메시지 또는 알림입니다. 알림에는 두 가지 주요 유형이 있습니다. 즉, 포그라운드 알림과 백그라운드 알림입니다.

포그라운드 알림은 앱이 현재 열려 있고 실행 중일 때 사용자에게 표시되는 알림 유형이며, 앱이 현재 열려 있는지 여부는 백그라운드 알림이 전송됩니다.

푸시 알림은 다음과 같은 여러 가지 이유로 모바일 앱 개발 분야에서 널리 사용됩니다.

- 그것들은 기업들이 낮은 비용으로 제품과 상품을 광고할 수 있게 한다.
- 전반적인 사용자 환경 개선
- 그들은 거래 영수증을 더 빨리 보내는 것을 도왔다.
- 더 많은 사용자를 변환하는 데 사용됩니다.

## React Native의 푸시 알림 아키텍처

React Native(원본 대응)에서 푸시 알림을 설정하는 방법에는 세 가지가 있습니다.

- Apple Push Notification 서비스(APN)
- Expo 푸시 알림 서비스
- FCM(Firebase Cloud Messaging)

APN은 Apple에서 iOS 알림을 위해 클라우드 서비스로 사용하고 있으며, 앱에 액세스하려면 앱을 등록해야 합니다. APN의 이점 중 하나는 사용자가 해당 IP 주소를 알지 못하는 상태에서 사용자에게 알림을 보낼 수 있다는 것입니다.

FCM은 Google에서 제공하며 백엔드 시스템에 통합하거나 독립적으로 사용할 수 있습니다. APN과 FCM을 사용하면 개발자는 디바이스에서만 디바이스 토큰을 얻는 반면, 이러한 클라우드 서비스는 사용자 디바이스로 푸시 메시지를 보내는 데 초점을 맞춘다.

Expo 푸시 알림 서비스는 Expo 및 React Native에서 제공하며, 이 서비스는 본 튜토리얼에서 사용할 것과 같은 Expo 관리 Retact Native 응용 프로그램에서 무료로 작동합니다.

React Native 응용 프로그램에서 푸시 알림을 사용하려면 먼저 푸시 알림 토큰을 얻으려면 앱을 등록해야 합니다. 이 토큰은 각 장치를 고유하게 식별하는 긴 문자열입니다. 그런 다음 토큰을 서버의 데이터베이스에 저장하고 알림을 보내고 받은 알림을 처리합니다.

## 프로젝트 설정

너무 깊이 파고들기 전에 이미 초기화된 프로젝트에 푸시 알림을 추가할 예정입니다. 이 프로젝트는 중고품 판매를 위한 전자상거래 React Native 애플리케이션입니다. 로컬에서 초기화하려면 터미널에서 다음 명령을 실행합니다.

```php
git clone https://github.com/iamfortune/Done-With-It-App.git
```

그런 다음 프로젝트에 필요한 종속성을 설치하고 서버를 시작합니다.

```undefined
yarn install 
yarn start 
```

그러면 에뮬레이터를 사용하여 Android Studio에서 볼 수 있고 Expo를 사용하여 자신의 장치에서 볼 수 있는 React Native 프로젝트가 초기화됩니다. 다음으로, React Native Expo에서 푸시 알림 토큰을 받을 것입니다.

## 푸시 알림 토큰 가져오기

React Native 응용 프로그램에서 푸시 알림을 사용하려면 먼저 푸시 알림 토큰을 얻으려면 앱을 등록해야 합니다. 여기서는 Expo에서 알림 API를 사용할 것입니다.

이렇게 하려면 네비게이션 디렉토리와 앱 네비게이터 구성요소에 cd를 넣자. 여기서 우리는 엑스포에서 토큰을 도출할 것이다. 아래 엑스포에서 알림 및 사용 권한 기능을 살펴보겠습니다.

```coffeescript
import { Notifications } from 'expo';
import * as Permissions from 'expo-permissions';
```

위의 기능은 푸시 알림을 보내기 위한 사용자 권한을 요청하는 데 도움이 됩니다. 이제 React Native Expo에서 토큰을 요청하는 비동기 함수를 `AppNavigator` 구성 요소에 작성합니다.

```js
 const registerForPushNotifications = async () => { 
    try {
       const permission = await Permissions.askAsync(Permissions.NOTIFICATIONS);
       if (!permission.granted) return;
       const token = await Notifications.getExpoPushTokenAsync();
    console.log(token);
    } catch (error) {
      console.log('Error getting a token', error);
    }
  }
```

위 코드에서는 최근에 설치한 `권한` 모듈을 사용하여 사용자의 알림에 대한 권한을 얻고 있습니다. 함수는 권한 개체를 받기 위해 대기합니다. 다음으로 허가가 났는지 확인하고, 허가가 안 났으면 바로 돌아옵니다.

토큰을 얻기 위해 우리는 Expo 푸시 알림 토큰을 받고 토큰을 콘솔에 기록하는 `알림` 기능에 대한 호출을 기다리고 있다. 발생할 수 있는 오류를 포착하기 위해 코드를 `트리 캐치` 블록으로 묶습니다.

위의 기능을 응용 프로그램에 호출하기 위해 React의 use Effect 후크를 사용할 예정입니다.

```coffeescript
const AppNavigator = () => {
  useEffect(() => {
    registerForPushNotifications();
  }, [])
```

위의 코드에서는 react에서 가져온 use Effect 후크를 통과시키고 registerForPush Notifications 기능을 전달하여 한 번만 호출되도록 했습니다. 장치에서 볼 때 Expo 푸시 토큰이 표시되도록 빈 브레이스도 추가했습니다.

## 프로젝트에 푸시 알림 토큰 저장

서버의 푸시 알림을 저장하고 사용하려면 새로운 사용자와 장치를 등록할 수 있는 방식으로 애플리케이션 사용자 인터페이스를 구성해야 합니다. 이를 위해 프로젝트의 api 디렉토리로 이동하여 expoPush라는 새 파일을 열어보자.토큰.js`입니다. 그런 다음 다음을 수행합니다.

```coffeescript
import client from './client';
const register = (pushToken) => client.post('/expoPushTokens', { token: pushToken });
export default {
    register,
}
```

위의 코드에서, 우리는 먼저 클라이언트 모듈을 가져오고 있는데, 이것은 또한 `api` 디렉토리에 있습니다.

우리는 push를 취하는 함수 `register`를 정의한다.토큰`. 이제 클라이언트 또는 새 사용자를 백엔드 `/expoPush`에 있는 `url`에 게시합니다.토큰. 요청 본문에 push로 설정된 token 개체를 추가할 예정입니다.토큰. 그러면 `register` 메서드를 사용하여 기본 개체로 내보냅니다.

다음으로, 우리는 `AppNavigator` 컴포넌트로 돌아가서 콘솔에 토큰을 기록하는 대신 서버로 보낼 것입니다.

```js
import expoPushTokensApi from '../api/expoPushTokens';

 const registerForPushNotifications = async () => { 
   ///
       const token = await Notifications.getExpoPushTokenAsync();
  expoPushTokensApi.register(token)
    } catch (error) {
      console.log('Error getting a token', error);
    }
  }
```

이제 토큰과 사용자 정보를 얻기 위해 새로운 사용자를 보냅니다.

## 테스트 알림 전송

Expo를 사용하여 React Native에서 테스트 알림을 보내려면 먼저 Expo 알림 문서로 이동하여 필요한 양식을 작성하십시오. 그러나 알림을 받으려면 다음 형식으로 `데이터(JSON)`를 설정할 때 다음을 작성해야 합니다.

```bash
{*_displayInForeground*: true}
```

이렇게 하면 앱이 포그라운드에 있을 때 네이티브가 알림을 보내도록 알 수 있습니다.

## 서버에서 알림 보내기

서버에 푸시 알림을 보내려면 Expo에서 제공하는 SDK 중 하나를 사용해야 합니다. 설명서를 참조하면 여러 언어로 서버에 푸시 알림을 구현하는 방법에 대한 정보를 제공합니다.

이 튜토리얼에서는 노드를 사용하여 작업합니다.JS 서버. 다음은 이 튜토리얼에 사용한 서버 링크입니다. 서버의 유틸리티 디렉토리를 방문하여 Expo SDK를 포함하도록 하겠습니다. 이를 위해 먼저 다음 작업을 수행합니다.

```php
git clone https://github.com/iamfortune/DoneWithIt-Backend
```

다음으로 다음 명령을 사용하여 `npm` 패키지를 설치합니다.

```coffeescript
npm install 
```

그런 다음 다음을 통해 개발 서버를 시작합니다.

```coffeescript
npm start 
```

이제 `pushnotifications.js` 파일로 이동하여 패키지에 `Expo` SDK를 추가하십시오.

```coffeescript
npm i expo-server-sdk
const { Expo } = require("expo-server-sdk");
```

다음으로 푸시 토큰과 사용자에게 보낼 메시지 등 푸시 알림을 받을 수 있는 기능을 작성하겠습니다. 그런 다음 푸시 알림을 처리하는 청크 메서드를 새로 만듭니다.

```undefined
const sendPushNotification = async (targetExpoPushToken, message) => {
  const expo = new Expo();
  const chunks = expo.chunkPushNotifications([
    { to: targetExpoPushToken, sound: "default", body: message }
  ]);
```

다음으로 받은 알림 처리 방법을 결정하겠습니다.

## React Native에서 수신된 알림 처리

수신된 알림을 처리하려면 먼저 사용자가 알림을 클릭할 때마다 호출되는 이벤트 수신기가 있어야 합니다. 알림 개체를 포함하는 `AppNavigator` 기능 내에 이벤트 수신기를 추가해 보겠습니다.

```js
 const AppNavigator = () => {
  useEffect(() => {
    registerForPushNotifications();

    Notifications.addListener(notification => console.log(notification))
  }, [])
```

다음으로, 브라우저의 `expo 알림`을 사용하여 알림을 보냅니다(브라우저에서 알림을 볼 수 있어야 합니다. 이제 수신자가 통지를 받도록 설정하여 발송된 통지를 확인해야 합니다.

```coffeescript
const useNotifications = (notificationListener) => {
  useEffect(() => {
    registerForPushNotifications();

    if (notificationListener) Notifications.addListener(notificationListener);
  }, []);
```

여기서는 알림 수신기가 새 알림을 받으면 사용자에게 이를 표시해야 한다고 말합니다.

## 결론

이 기사에서는 푸시 알림이 인기 있는 이유와 애플리케이션 환경에 머무르는 이유를 살펴 보았고, React Native 응용 프로그램에서 푸시 알림을 보내는 방법에 대해 알아보았습니다. 알림 토큰을 추가하고 서버에서 전송한 다음 사용자 장치에 표시합니다.

Expo 문서 및 React Native 문서에서 자세한 내용을 볼 수 있습니다.