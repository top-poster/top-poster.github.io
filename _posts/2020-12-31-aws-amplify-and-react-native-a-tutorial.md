---
layout: post
title: "AWS Amply and React Native: 튜토리얼"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/aws-amplify-react-native-tutorial.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/aws-amplify-react-native-tutorial.png?fit=730%2C487&ssl=1)

인증은 응용 프로그램의 중요한 부분에 대한 사용자 액세스를 제어하는 데 도움이 됩니다. 따라서 사실상 모든 유형의 소프트웨어에서 매우 중요한 부분입니다.

이 튜토리얼에서는 React Native(원본 반응) 및 AWS Expmise(AWS 증폭)를 사용하여 모바일 응용 프로그램에서 인증을 설정하는 방법에 대해 설명합니다.

다음 사항에 대해 자세히 설명합니다.

- AWS Amply란?
- AWS 확장 설정
- 기본 반응 앱 설정
- 반응 네이티브 애플리케이션에 AWS 확장 추가
- Amazon Cognito를 통한 인증 추가
- 로그아웃 기능 추가

이 튜토리얼을 따르려면 다음을 수행해야 합니다.

- 노드 v10 이상
- npm v5.2 이상
- AWS 계정
- JavaScript 및 대응에 대한 지식
- 반응 네이티브에 대한 지식
- Android 및/또는 IOS 에뮬레이터
- git v2.14.1 이상

시작해 봅시다!

## AWS Amply란?

AWS 앰프(AWS Amply)는 모바일 및 프런트엔드 웹 개발자가 아마존 웹 서비스에 풀스택 애플리케이션을 구축하고 배포하는 데 도움을 주는 아마존의 제품 및 도구 모음이다.

AWS Amply의 주목할 만한 기능은 다음과 같습니다.

- 인증 — 로그인, 등록, 로그아웃, 소셜 인증 등
- 온라인 및 오프라인에서 데이터를 유지할 수 있는 데이터스토어
- 백엔드 데이터에 원활하게 액세스할 수 있는 API(GraphQL 및 REST)
- 개인, 공용 또는 보호되는 사용자 콘텐츠를 관리하는 데 도움이 되는 스토리지 솔루션
- 분석
- 사용자에게 대상 통신을 보낼 수 있는 알림 푸시

## AWS 확장 설정

응용 프로그램 내에서 AWS 증폭을 사용하려면 Amply CLI가 있어야 합니다. 터미널을 열고 아래 코드를 붙여넣어 확장 CLI를 설치합니다.

```css
npm install -g @aws-amplify/cli 
```

Amply CLI를 성공적으로 설치한 후 다음 코드를 실행하여 CLI를 구성합니다.

```undefined
amplify configure
```

이 대화형 CLI 명령은 입력이 완료되기 전에 여러 단계에서 필요합니다. 다음 단계는 다음과 같습니다.

- AWS 계정 인증
- IAM(ID 및 액세스 관리) 사용자 추가

사용자를 생성할 때는 Amply, Cognito, Cloudfront와 같은 AWS 서비스에 `Administrator Access`를 사용하여 사용자를 생성해야 합니다. 이 작업은 다음을 위해 필요합니다.

- AWS 영역 지정
- 생성한 사용자의 `accessKeyId` 및 `secretAccessKey` 추가
- AWS 프로파일 추가

아래 스크린샷에는 앞에서 설명한 단계의 요약이 나와 있습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/summarized-steps.png?resize=730%2C535&ssl=1)

## 기본 반응 앱 설정

React Native 애플리케이션을 부트스트랩하기 위해 Expo를 사용합니다. Expo는 모든 사용자의 장치에서 기본적으로 실행되는 프로젝트를 개발, 빌드 및 배포하는 데 도움이 되는 프레임워크입니다.

Expo를 시작하려면 터미널을 시작하고 다음 코드를 붙여넣으십시오.

```undefined
npm install --g expo-cli
```

위의 명령은 Expo CLI를 설치합니다. 설치 후 Expo CLI를 사용하여 React Native 프로젝트를 초기화할 수 있습니다. 아래 코드를 붙여넣고 실행합니다.

```undefined
expo init aws-amplify-authentication-tutorial
```

템플릿을 선택하라는 메시지가 나타나면 빈 템플릿을 선택합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/choose-blank-template.png?resize=730%2C132&ssl=1)

설치가 성공하면 명령이 실행된 디렉터리에 `aws-amplify-authentication-utututorial`이라는 Retact Native 프로젝트가 나타납니다.

## 반응 네이티브 애플리케이션에 AWS 확장 추가

이제 반응형 애플리케이션과 증폭 프레임워크가 설정되었으므로, 우리는 반응 네이티브 애플리케이션에 증폭을 연결해야 한다.

React Native 애플리케이션 디렉토리로 이동하여 다음 명령을 실행합니다.

```undefined
amplify init
```

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/running-amplify-init-command.png?resize=730%2C297&ssl=1)

다음을 지정하라는 메시지가 표시됩니다.

- 프로젝트명
- 환경 이름 — 개발 환경에 있으므로 기본 `dev`를 고수하는 것이 좋습니다(기본 옵션을 선택하려면 Enter 키를 누르십시오).
- 기본 선택 편집기
- 만들고 있는 앱 유형(JavaScript)
- 사용 중인 JavaScript 프레임워크(Ract Native)
- 원본 디렉토리 경로(기본 루트 디렉토리를 선택하려면 Enter 키를 누릅니다)
- 배포 디렉토리 경로(기본 루트 디렉토리를 선택하려면 Enter 키를 누릅니다)
- 빌드 명령(기본 명령을 선택하려면 Enter 키를 누름)
- Start command(기본 명령을 선택하려면 Enter 키를 누름)
- AWS 프로파일을 사용할지 여부(`예`를 입력하고 Enter 키를 누름)
- 예인 경우, 사용할 프로파일

이 양식을 작성한 후:

- 응용 프로그램 루트 디렉터리에 `amplify`라는 폴더가 생성되어 인증과 같은 응용 프로그램의 백엔드 구성을 저장합니다.
- Amply를 초기화할 때 지정한 원본 디렉터리 경로에 `aws-exports.js`라는 파일이 생성됩니다. 이 파일에는 증폭을 사용하여 생성한 서비스에 대한 정보가 들어 있습니다.
- .gitignore` 파일이 수정되어 Git 저장소에 추가해서는 안 되는 Amply에서 생성된 파일 및 폴더를 포함합니다.
- AWS Amply Console에서 클라우드 프로젝트가 생성됩니다. 앰프 콘솔을 실행하여 언제든지 액세스할 수 있습니다.

다음으로, 우리는 몇 가지 로컬 앰프 종속성을 설치해야 합니다. 터미널에 다음을 붙여넣습니다.

```css
npm install aws-amplify aws-amplify-react-native @react-native-community/netinfo
```

코드 편집기에서 `App.js` 파일을 열고 마지막 가져오기 명령문 뒤에 다음 줄의 코드를 추가합니다.

```coffeescript
import Amplify from 'aws-amplify';
import config from './aws-exports';
Amplify.configure({
  ...config,
  Analytics: {
    disabled: true,
  },
});
```

Respect Native 프로젝트에 Amply를 성공적으로 추가했습니다. `expo start`를 실행하여 원하는 에뮬레이터에서 프로젝트를 시작합니다.

## Amazon Cognito를 통한 인증 추가

Amazon Amply는 후드 아래 Amazon Cognito를 사용하여 인증 프로세스에 전원을 공급합니다. 아마존 코니토는 인증(가입, 로그인, 로그아웃, OAuth, 멀티팩터 인증 등)을 추가하는 과정을 단순화한 서비스다.

시작하려면 프로젝트 루트 디렉터리에서 터미널을 열고 다음을 실행하십시오.

```undefined
amplify add auth
```

명령을 입력한 후 나타나는 대화형 프롬프트에서:

- 기본 구성 선택
- 또한 사용자가 응용프로그램에 로그인할 수 있는 방법을 선택하십시오.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/option-to-choose-default-config.png?resize=730%2C98&ssl=1)

인증 서비스를 초기화한 후 아래 명령을 실행하여 배포하십시오.

```undefined
amplify push
```

이 시점에서, 우리는 우리의 애플리케이션에 인증 서비스를 통합할 수 있습니다. Amply Framework는 통합하기 쉬운 사용자 지정이 가능한 내장 UI 구성 요소를 제공하여 프로세스를 간소화하는 데 도움이 됩니다.

`App.js`로 이동하여 가져오기 문 아래에 다음 줄을 추가하십시오.

```coffeescript
import { withAuthenticator } from 'aws-amplify-react-native';
```

`인증자 포함`은 응용 프로그램의 인증 상태를 자동으로 감지하고 UI를 업데이트하는 고차 구성 요소입니다.

`App.js` 끝의 기본 내보내기를 다음과 같이 변경합니다.

```coffeescript
export default withAuthenticator(App)
```

`App.js`는 다음과 유사하게 표시되어야 합니다.

```js
import { StatusBar } from 'expo-status-bar';
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';
import Amplify from 'aws-amplify';
import config from './aws-exports';
import { withAuthenticator } from 'aws-amplify-react-native';
Amplify.configure({
  ...config,
  Analytics: {
    disabled: true,
  },
});
function App() {
  return (
    <View style={styles.container}>
      <Text>Open up App.js to start working on your app!</Text>
      <StatusBar style="auto" />
    </View>
  );
}
const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
});
export default withAuthenticator(App);
```

시뮬레이터에서 앱을 저장하고 실행합니다.

```undefined
expo start
```

다음과 같은 것을 볼 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/sign-in-amazon-page.png?resize=730%2C1298&ssl=1)

## 로그아웃 기능 추가

인증에 성공하면 로그아웃할 방법이 없이 앱 화면으로 이동됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/app-screen-no-logout-function.png?resize=730%2C1298&ssl=1)

로그아웃 버튼을 추가하려면 루트 디렉터리에 `Home.js` 파일을 생성하고 다음을 붙여넣으십시오.

```js
import React from 'react';
import { StyleSheet, Text, View, Pressable, Dimensions } from 'react-native';
import { Auth } from 'aws-amplify';
const { width } = Dimensions.get('window');
const Home = () => {
  const signOut = async () => {
    try {
      await Auth.signOut({ global: true });
    } catch (error) {
      console.log('error signing out: ', error);
    }
  };
  return (
    <View style={styles.container}>
      <View style={styles.header}>
        <Text style={styles.headerText}>Welcome!</Text>
        <Pressable style={styles.button} onPress={() => signOut()}>
          <Text style={styles.buttonText}>Sign out</Text>
        </Pressable>
      </View>
    </View>
  );
};
const styles = StyleSheet.create({
  container: {
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
    width: width,
    paddingVertical: 20,
  },
  header: {
    display: 'flex',
    flexDirection: 'row',
    justifyContent: 'space-between',
    padding: 20,
    width: width,
    alignItems: 'center',
  },
  headerText: {
    fontSize: 28,
    fontWeight: 'bold',
  },
  button: {
    backgroundColor: '#ff9900',
    padding: 10,
    borderRadius: 6,
  },
  buttonText: {
    color: '#fff',
    fontSize: 18,
  },
});
export default Home;
```

이렇게 하면 환영 메시지와 로그아웃 버튼을 렌더링하는 `홈` 구성 요소가 생성됩니다. 사용자가 로그아웃 버튼을 클릭하면 auth 클래스에서 제공하는 aws-amplify(aws-amplify) 방법 중 하나인 auth.signOut() 메서드가 호출됩니다. 이 방법은 사용자를 로그아웃시킵니다.

`App.js` 파일을 열고 `Home` 구성 요소를 포함하도록 다시 팩터링합니다.

```js
import { StatusBar } from 'expo-status-bar';
import React from 'react';
import { StyleSheet, View } from 'react-native';
import Amplify from 'aws-amplify';
import config from './aws-exports';
import { withAuthenticator } from 'aws-amplify-react-native';
import Home from './Home';
Amplify.configure({
  ...config,
  Analytics: {
    disabled: true,
  },
});
function App() {
  return (
    <View style={styles.container}>
      <Home />
      <StatusBar style="auto" />
    </View>
  );
}
const styles = StyleSheet.create({
  container: {
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
});
export default withAuthenticator(App);
```

저장 후 화면은 다음과 같아야 합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/signout-button.png?resize=730%2C1298&ssl=1)

이제 응용 프로그램에서 로그아웃할 수 있습니다.

## 결론

이 튜토리얼에서, 우리는 AWS Amply Framework를 사용하여 React Native 애플리케이션에서 사용자를 인증하는 방법을 살펴 보았다. Amply Framework는 우리가 다룰 수 없었던 더 많은 기능을 제공합니다.

이 튜토리얼에 내장된 이 응용 프로그램의 리포지토리는 GitHub에서 사용할 수 있습니다. 질문이 있으시면 언제든지 댓글로 달아주세요.