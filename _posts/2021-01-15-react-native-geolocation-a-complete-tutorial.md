---
layout: post
title: "반응 네이티브 지오로 위치: 전체 자습서"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/react-native-geolocation-tutorial.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/react-native-geolocation-tutorial.png?fit=730%2C487&ssl=1)

배달 및 차량 연결 앱이 특정 시점의 현재 위치를 어떻게 알고 있는지 궁금해한 적이 있습니까? 정답은 지리 위치입니다.

이 튜토리얼에서는 React Native(원본 반응)에서 지오로케이션 구현 방법에 대해 설명합니다. 사용자의 위치를 경도 및 위도 좌표로 지도에 표시하고 사용자가 자신의 위치를 Twitter와 같은 다른 소스로 보낼 수 있도록 하는 React Native(원본 반응)에 예제 앱을 구축합니다.

다음 내용을 다루겠습니다.

- 지리 위치란 무엇인가?
- 반응 네이티브의 지리 위치
- 기본 반응 프로젝트 초기화
- React Native(iOS 및 Android)의 지리적 위치에 필요한 종속성
- React Native(원본 반응) 앱에 지리 위치 추가
- `react-native-library 가져오기
- `RNlocation.configure`
- 위치 데이터에 대한 액세스 권한 요청
- 사용자의 위치 데이터 가져오기
- React Native에서 위치 데이터 전송
- Twitter에 사용자 위치 데이터 전송

## 지리 위치란 무엇인가?

지리적 위치는 인터넷에 연결된 장치의 지리적 위치를 추론할 수 있는 기능입니다. 대부분의 경우, 사용자는 자신의 위치 데이터에 접근하기 위해 앱에 대한 권한을 부여해야 한다.

지리적 위치의 한 가지 간단한 예는 장치의 IP 주소를 사용하여 장치가 위치한 국가, 도시 또는 우편 번호를 결정하는 것입니다.

## 반응 네이티브의 지리 위치

React Native에는 `@react-native-community/geol location`이라는 API가 있습니다. 그러나 구글은 리액트 네이티브 API가 권장되는 구글 위치 서비스 API보다 정확도가 낮고 속도가 느리기 때문에 지리 위치 파악을 위해 리액트 네이티브 API를 사용하는 것을 권장하지 않는다.

다행히, 리액트 네이티브 커뮤니티는 리액트 네이티브에서 지오로케이션 구현을 위한 두 개의 훌륭한 라이브러리를 개발했다.

- ➡-➡-➡
- 도시-지리-지리-로케이션-서비스

이 React Native Geolocation 튜토리얼에서는 "React-Native Location Library"를 사용합니다. 노드 패키지로 제공되므로 npm에 사용할 수 있습니다.

다음 작업을 수행하는 앱을 구축합니다.

- Android 및 iOS 기기의 위치 데이터에 액세스할 수 있는 권한을 요청합니다.
- 위치 좌표 선택
- 위치 좌표를 저장하거나 보냅니다.

노드 버전 10+를 사용하는 것이 좋습니다. 노드 버전을 확인하려면:

```shell
$ node -v

// returns node version
```

`nvm`을 사용하여 시스템에 설치된 여러 노드 버전을 관리할 수 있습니다.

ReactNative에는 새 프로젝트를 생성하는 데 사용할 수 있는 명령줄 인터페이스가 내장되어 있습니다. Node.js와 함께 제공되는 npx를 사용하여 전역으로 아무것도 설치하지 않고도 액세스할 수 있습니다.

## 기본 반응 프로젝트 초기화

프로젝트를 시작하려면 터미널에서 다음 명령을 실행하십시오.

```shell
$ npx react-native init ReactNativeGeolocation
```

`npx`는 시스템에 이러한 종속성을 설치하지 않고도 터미널에서 노드 기반 종속성을 실행하는 데 사용할 수 있는 노드 기반 명령입니다.

명령의 init 부분은 프로젝트 파일이 설치될 디렉토리로 ReactNativeLocation을 사용하여 새로운 ReactNative 프로젝트를 초기화하라는 명령이다.

시스템에 ReactNative를 설치할 필요 없이 `npx react-native` 명령이 작동합니다. 또는 시스템에 ReactNative를 전체적으로 설치하여 `npx`를 사용하지 않고 ReactNative 명령을 실행할 수 있습니다.

React Native를 전체적으로 설치하려면 다음 명령을 실행합니다.

```shell
$ npm install -g react-native-cli
```

그런 다음 다음을 사용하여 React Native가 설치되어 있는지 확인할 수 있습니다.

```shell
$ react-native -v
```

그러면 컴퓨터에 설치된 React Native(원본 반응) 버전이 콘솔에 인쇄됩니다.

즐겨찾는 IDE를 열고 `Ract Native Geolocation` 디렉토리로 이동합니다. 프로젝트를 초기화하기 위해 실행한 명령으로 React Native 앱을 설정했습니다.

우리는 모든 코드를 `App.js` 파일에 쓸 것이다. 명령을 실행하려면 앱의 루트 디렉터리에 있어야 합니다.

첫 번째 단계는 메트로를 시작하는 것이다. 메트로(Metro)는 리액트 네이티브와 함께 제공되는 자바스크립트 번들러이다. Metro는 "항목 파일과 다양한 옵션을 사용하고 모든 코드와 해당 종속성을 포함하는 단일 JavaScript 파일을 반환합니다."

```shell
$ npx react-native start
```

터미널에서 Metro를 실행한 상태에서 다른 터미널을 열고 다음을 실행합니다.

```shell
$ npx react-native run-android
```

Android 에뮬레이터가 실행 중이거나 모바일 장치를 디버그 모드로 PC에 연결하여 모바일 장치에서 앱을 실행합니다. 그런 다음 다음 화면이 표시됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/react-native-setup-1.png?resize=580%2C1033&ssl=1)

이제 App.js로 이동하여 앱을 변경할 수 있습니다.

`App.js`의 기존 코드를 모두 제거하고 이 코드로 대체합니다.

```js
/**
* Sample React Native App
* https://github.com/facebook/react-native
*
* @format
* @flow strict-local
*/

import React from 'react';
import {StyleSheet, View, Text} from 'react-native';

const App = () => {
 return (
   <View style={styles.container}>
     <Text>Welcome!</Text>
   </View>
 );
};

const styles = StyleSheet.create({
 container: {
   flex: 1,
   backgroundColor: '#fff',
   alignItems: 'center',
   justifyContent: 'center',
 },
});

export default App;
```

이것은 우리가 프로그램을 계속 쓰는 새로운 화면을 제공한다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/react-geolocation-welcome-screen-1.jpg?resize=580%2C1154&ssl=1)

## React Native의 지리적 위치에 필요한 종속성

다음 중 하나를 사용하여 라이브러리를 설치합니다.

```shell
$ yarn add react-native-location
```

오후:

```shell
$ npm install --save react-native-location
```

이 npm 패키지를 사용하려면 iOS 및 Android 장치용 추가 설치 지침이 필요합니다.

### iOS

다음 줄을 포드 파일에 추가하여 Cocoapods를 사용하여 라이브러리를 연결할 수 있습니다.

```js
$ pod 'react-native-location', :path => '../node_modules/react-native-location/react-native-location.podspec'
```

그런 다음 `Info.plist` 파일에 올바른 사용 설명이 있는지 확인해야 합니다. 앱에서 사용 권한을 요청하면 경고 상자에 메시지가 표시되고 사용자가 해당 사용 권한을 요청하는 이유를 사용자에게 알립니다. 또한 앱 스토어 검토 프로세스의 일부이기도 합니다.

앱이 사용 중일 때만 위치 액세스를 요청하는 경우 `NS 위치`만 있으면 됩니다.Plist의 InUseUsageDescription` 항목을 사용할 때. 백그라운드 권한을 요청하는 경우 NSL 위치도 추가해야 합니다.항상 그리고사용 중 사용 설명 및 `NSLocation`항상 사용 설명`을 `PLIST` 파일에 추가합니다.

이러한 정보를 추가하는 가장 쉬운 방법은 `Info.plist`를 찾는 것입니다. 즐겨찾는 IDE를 열고 프로젝트 원본 디렉토리에서 파일을 찾습니다. 그런 다음 파일에 다음 항목을 입력할 수 있습니다.

```undefined
<key>NSLocationWhenInUseUsageDescription</key>

<string>This is the plist item for NSLocationWhenInUseUsageDescription</string>

<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>

<string>This is the plist item for NSLocationAlwaysAndWhenInUseUsageDescription</string>

<key>NSLocationAlwaysUsageDescription</key>

<string>This is the plist item for NSLocationAlwaysUsageDescription</string>
```

### 안드로이드

CLI 도구를 사용하여 라이브러리를 자동으로 연결합니다. CLI 도구를 사용하여 라이브러리를 연결하는 가장 쉬운 방법은 프로젝트의 루트에서 이 명령을 실행하는 것입니다.

```java
$ react-native link react-native-location
```

Android 매니페스트 사용 권한을 활성화하려면 Android > app > src > main > `AndroidManifest.xml`로 이동하여 다음 사용 권한을 `AndroidManifest.xml`에 추가하십시오.

```xml
<uses-permission android:name="android.permission.ACCESSCOARSELOCATION"/>
```

좋은 위치에 액세스하려면 다음 항목도 포함해야 합니다.

```xml
<uses-permission android:name="android.permission.ACCESSFINELOCATION"/>
```

다음 단계는 Google Fused Location 공급자 종속성(선택 사항)을 설치하는 것입니다. 이 라이브러리는 Android에서 위치를 얻는 두 가지 방법을 제공합니다. 기본값은 기본 제공 위치 관리자입니다. 그러나 선택적으로 Fused Location 라이브러리를 설치하도록 선택할 수 있으며, 이 라이브러리는 더 정확하고 빠른 결과를 제공합니다. 단점은 구글 플레이 서비스가 설치되고 구성된 장치에서만 작동한다는 것이다(서부에서는 대부분의 안드로이드 장치이지만 킨들 장치나 아시아 시장에서는 작동하지 않는다).

Google Play Services Fused Location 공급자를 사용하려면 다음 종속성을 Android/app/build.gradle 파일에 추가해야 합니다.

```css
implementation "com.google.android.gms:play-services-base:16.0.1"

implementation "com.google.android.gms:play-services-location:16.0.0"
```

기본적으로 사용자의 위치에 액세스할 수 있는 권한을 가집니다. 그런 다음 사용자의 좌표를 가져와 이 좌표를 보냅니다.

## React Native(원본 반응) 앱에 지리 위치 추가

React Native 앱에 두 개의 개별 버튼을 추가해 보겠습니다. 하나는 사용자의 위치를 가져오고, 다른 하나는 앱 상태로 저장하여 사용자의 위치를 Twitter로 보내는 것입니다.

시작 텍스트 아래의 `App.js`에 두 개의 버튼을 추가합니다.

```js
<View style={marginTop: 10, padding: 10, borderRadius: 10, width: '40%'}>
       <Button
         title="Get Location"
         />
     </View>
     <Text>Latitude: </Text>
     <Text>Longitude: </Text>
     <View style={marginTop: 10, padding: 10, borderRadius: 10, width: '40%'}>
       <Button
         title="Send Location"
        />
         </View>
```

이제 앱은 다음과 같습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/react-native-geolocation-longitude-latitude-1.png?resize=580%2C1038&ssl=1)

## 'react-native-library 가져오기

일단 `반향 위치` 라이브러리가 설치되면, 우리는 위도와 경도 좌표를 얻기 위한 논리를 구현할 수 있다. ES6을 사용하여 "GET LOCATION" 버튼에 의해 트리거되는 "On Press" 버튼에 이벤트가 처리할 비동기 요청을 작성합니다.

이 이벤트 핸들러가 트리거되면 사용자에게 자신의 위치에 액세스할 수 있는 권한을 요청하여 요청을 승인하면 해당 위치가 사용자에게 반환됩니다.

먼저 react-native-library를 가져오고 구성 방법을 호출합니다.

```coffeescript
import RNLocation from 'react-native-location';

RNLocation.configure({
 distanceFilter: null
})
```

### 'RNlocation.configure'

이 메서드는 하나의 개체인 `distance Filter`를 사용합니다. 거리 필터는 샘플 앱에서 새 위치 업데이트 콜백을 호출하기 전에 장치 위치가 변경되는 최소 거리(미터)입니다. 장치를 이동할 때마다 변경되도록 `null`로 설정되어 있습니다.

다음과 같이 "GET LOCATION" 버튼을 업데이트합니다.

```xml
<Button title="Get Location"
   onPress={permissionHandle}
 />
```

## 위치 데이터에 대한 액세스 권한 요청

온프레스(On Press)는 이 기능을 permissionHandle이라고 하는 이벤트 핸들러이다. 이제 명명된 함수 permissionHandle에 대한 논리를 작성하겠습니다.

```js
const permissionHandle = async () => {

   console.log('here')


   let permission = await RNLocation.checkPermission({
     ios: 'whenInUse', // or 'always'
     android: {
       detail: 'coarse' // or 'fine'
     }
   });

   console.log('here2')
   console.log(permission)

 }
```

`console.log` 문장은 함수 호출에 액세스할 수 있는지 확인하는 것입니다. 사용 권한의 콘솔 로그가 `false`를 반환합니다. Check Permission(사용 권한 확인) 방법은 사용자가 앱을 통해 자신의 장치 위치를 조회할 수 있는 권한을 부여했는지 여부를 확인하는 데 사용됩니다. requestPermission 앞에 `CheckPermission(권한 확인)`을 걸어 사용자가 필요한 권한 상태를 부여했는지 확인하는 것이 유용하다.

사용자로부터 허가가 요청되지 않았기 때문에 `거짓`인 이유다. 사용자에게 권한을 받으려면 `requestPermission` 방법을 사용합니다.

```coffeescript
permission = await RNLocation.requestPermission({
       ios: "whenInUse",
       android: {
         detail: "coarse",
         rationale: {
           title: "We need to access your location",
           message: "We use your location to show where you are on the map",
           buttonPositive: "OK",
           buttonNegative: "Cancel"
         }
       }
     })
     console.log(permission)
```

위치 업데이트에 가입하기 전에 `requestPermission` 메서드를 호출해야 합니다. requestPermission 방식은 플랫폼 유형을 객체 키(iOS와 Android)로 사용한다. Android 개체에는 내부 이성 개체가 있으며, 사용자가 두 번째 권한을 요청할 때 이 개체가 표시됩니다. request Permission은 권한이 부여된 경우 TRUE로, 부여되지 않은 경우 FALSE로 해결되는 약속을 반환합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/react-native-geolocation-location-access-request-permission.png?resize=580%2C1037&ssl=1)

사용자가 권한을 부여하면 TRUE가 콘솔에 기록됩니다. 사용자가 사용 권한을 거부하면 `FALSE`가 콘솔에 기록됩니다. 버튼을 다시 클릭하면 요청 권한 화면이 다시 나타나기 전에 사용자에게 합리적인 개체가 구문 분석됩니다.

사용 권한을 거부한 후 사용자가 버튼을 다시 클릭하면 다음과 같은 화면이 나타납니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/react-native-geolocation-location-access-prompt.png?resize=580%2C1045&ssl=1)

## 사용자의 위치 데이터 가져오기

그런 다음 사용자가 권한을 부여할 때 사용자의 위치를 가져오기 위해 코드를 업데이트해야 합니다.

```js
let location;

if(!permission) {
    permission = await RNLocation.requestPermission({
       ios: "whenInUse",
       android: {
         detail: "coarse",
         rationale: {
           title: "We need to access your location",
           message: "We use your location to show where you are on the map",
           buttonPositive: "OK",
           buttonNegative: "Cancel"
         }
       }
     })
     console.log(permission)
     location = await RNLocation.getLatestLocation({timeout: 100})
     console.log(location, location.longitude, location.latitude, 
           location.timestamp)
} else {
    console.log("Here 7")
    location = await RNLocation.getLatestLocation({timeout: 100})
    console.log(location, location.longitude, location.latitude,   
                location.timestamp)
}
```

코드를 분류해 봅시다.

- 첫째, 다른 시간에 재할당할 수 있도록 `let` 키워드를 사용하여 `위치` 변수를 선언(이 때 할당하지 않음)합니다.
- 다음으로 장치에서 부여한 권한 상태를 확인했습니다. 사용자에게 앱에 대한 권한을 부여하도록 요청하기 위해 `requestPermission` 방법을 사용하여 획득됩니다. 이 논리는 `제1절`에서 구현된다.IF 구성
- 사용자로부터 사용 권한을 얻은 경우 `getLatestLocation` 방법을 사용하여 이 시점에서 앱이 작동하는 것을 볼 수 있도록 콘솔에 위치를 기록합니다.
- ELSE 파트는 getLatestLocation 방식을 반환하며, 이는 사전에 허가를 받은 경우이다.
- 약속 시간 내에 해결되지 않으면 getLatestLocation(최신 위치 확인) 방법이 시간 초과된다.

위치 개체는 JSON 형식의 다음 예제 정보를 사용하여 콘솔에 기록됩니다.

```undefined
{

"accuracy": 2000, "altitude": 0, "altitudeAccuracy": 0, "course": 0, "courseAccuracy": 0,

"fromMockProvider": false, "latitude": 37.42342342342342, "longitude": -122.08395287867832,

"speed": 0, "speedAccuracy": 0, "timestamp": 1606414495665

}
```

개체 응답에서 필요한 것은 위도 및 경도입니다. 이 경도는 콘솔에도 기록됩니다.

거기서 사용자의 위치를 확인할 수 있습니다.

## React Native(원본 반응)에서 지리 위치 데이터 전송

사용자의 Go 위치 데이터를 Twitter로 전송하려면 "Send Location" 버튼을 "Tweet" 버튼으로 전환하면 됩니다.

사용자에게 위치를 보내기 전에 예제에서 반환된 데이터를 표시하는 것이 좋습니다. 이를 위해 Retact의 State Hook 사용을 앱에 추가합니다.

```coffeescript
import React, { useState } from 'react';
```

그런 다음 App 기능 내부에 상태를 정의합니다.

```undefined
[viewLocation, isViewLocation] = useState([])
```

if 문을 업데이트하여 isViewLocation 메서드를 호출하고 반환된 Location 정보 개체로 viewLocation 배열을 업데이트합니다.

```undefined
isViewLocation(location)
```

반환된 정보를 보려면 `위치` 및 `경도` 텍스트 태그를 업데이트하십시오.

```xml
<Text>Latitude: {viewLocation.latitude} </Text>
<Text>Longitude: {viewLocation.longitude} </Text>
```

`viewLocation.latitude`는 `isViewLocation` 메서드에서 호출되는 개체 응답의 위도를 반환합니다. 경도도 마찬가지입니다.

이 때 사용자의 위치를 Twitter로 전송하기 위해 버튼을 업데이트할 수 있습니다.

다음은 최종 예제 앱 보기입니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/react-native-geolocation-example-app.png?resize=580%2C1032&ssl=1)

## Twitter에 사용자 위치 데이터 전송

반환될 때 위치를 차지하는 상태를 초기화하여 트윗할 콘텐츠로 설정해야 합니다.

```cpp
const [tweet, setTweet] = useState([viewLocation.longitude, viewLocation.latitude]);
```

Send Location 버튼은 함수 호출을 트리거하는 이벤트 핸들러를 사용합니다.

다음과 같이 단추를 업데이트합니다.

```xml
<Button
    title="Send Location"        
    onPress={tweetLocation}
 />
```

다음 기능을 작성할 수 있습니다.

```js
const tweetLocation = () => {
    let twitterParams = [];

    try {
     if (tweet)
     twitterParams.push(${tweet});
     const url = 'https://twitter.com/intent/tweet?' + twitterParams.join('&');
     Linking.openURL(url)
    } catch (error) {
     console.log(error);
    }
   }   
```

이 함수는 빈 배열인 `twitterParams`를 설정하는 간단한 스크립트이다. 조건부 if 문은 콘솔이 캐치 블록에 오류를 기록하는 try catch 블록으로 구문 분석됩니다. 조건부 `if`는 트윗을 `twitterParams` 빈 배열로 밀어넣고 Retact Native Linking 구성 요소를 사용하여 URL을 `open`으로 엽니다.URL 메서드.

`setTweetContent` 메서드를 `getLocation` 함수에 추가합니다.

```css
setTweetContent([viewLocation.longitude, viewLocation.latitude])
```

이제 위치 좌표를 트윗할 수 있습니다.

## 결론

이제 사용자의 위치를 파악하는 방법을 알게 되었으므로 할 수 있는 일이 훨씬 많습니다. 배달 서비스를 구축하고, 사용자를 추적하며, 비상 버튼 경보 시스템도 구축할 수 있습니다. 가능성은 무궁무진합니다!

GitHub에서 이 프로젝트의 전체 소스 코드에 액세스할 수 있습니다.