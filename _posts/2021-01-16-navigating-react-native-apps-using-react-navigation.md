---
layout: post
title: "React Native 탐색 : React Navigation 예제 및 자습서
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2019/06/react-native-navigation-tutorial.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2019/06/react-native-navigation-tutorial.png?fit=730%2C487&ssl=1)

편집자 주 :이 React Native 탐색 튜토리얼은 가장 최근의 안정적인 React Navigation 릴리스 인 React Navigation 5.0에 대한 정보를 포함하도록 2021 년 1 월에 마지막으로 업데이트되었습니다.
 

모바일 앱은 여러 화면으로 구성됩니다.
 모바일 앱을 빌드 할 때 가장 중요한 것은 앱을 통한 사용자의 탐색 (예 : 화면 표시 및 화면 전환)을 처리하는 방법입니다.
 

React Navigation은 React에서 사용할 수있는 가장 잘 알려진 탐색 라이브러리 중 하나입니다.
 이 튜토리얼에서는 React Native 탐색의 기본 사항을 살펴보고 React Native 앱에서 React Navigation을 사용하는 방법을 보여주고 React Native 탐색 예제를 살펴 봅니다.
 

다음 내용을 다룹니다.
 

- React Navigation이란 무엇입니까?
 
- 반응 탐색 5.0
 
- React Native Navigation이란 무엇입니까?
 
- React Navigation 설치
 
- React Native 스택 네비게이터
 
- React Native 탐색 예제
스택 탐색기를 사용하여 화면 구성 요소 간 탐색
React Navigation에서 탭 탐색 사용
React Navigation에서 서랍 탐색 사용
React Navigation에서 화면에 매개 변수 전달
 
- 스택 탐색기를 사용하여 화면 구성 요소 간 탐색
 
- React Navigation에서 탭 탐색 사용
 
- React Navigation에서 서랍 탐색 사용
 
- React Navigation에서 화면에 매개 변수 전달
 

## React Navigation이란 무엇입니까?
 

React Navigation은 React Native 애플리케이션에서 탐색 기능을 구현할 수있는 독립 실행 형 라이브러리입니다.
 

React Navigation은 JavaScript로 작성되었으며 iOS 및 Android에서 기본 탐색 API를 직접 사용하지 않습니다.
 오히려 해당 API의 일부 하위 집합을 다시 만듭니다.
 이를 통해 Objective-C, Swift, Java, Kotlin 등을 배울 필요없이 타사 JS 플러그인의 통합, 최대 사용자 정의 및 더 쉬운 디버깅이 가능합니다.
 

### 반응 탐색 5.0
 

작성 당시 React Navigation의 가장 안정적인 버전은 2020 년 2 월에 출시 된 React Navigation 5.0입니다. React Navigation 블로그에 따르면 최신 릴리스는 핵심 React Navigation 라이브러리 및 API를보다 동적으로 만드는 것을 목표로합니다.
 

React Navigation 5.0에 포함 된 주요 변경 사항 및 새로운 기능은 다음과 같습니다.
 

- 동적, 구성 요소 기반 구성
 
- `useNavigation`,`useRoute` 및`useNavigationState`를 포함한 일반적인 사용 사례를위한 새로운 후크
 
- 화면 탐색 옵션을보다 직관적으로 구성 할 수있는 새로운`setOptions` 메서드
 
- 보다 쉬운 사용자 정의를위한 개선 된 테마 시스템
 
- TypeScript를 사용한 일류 자동 완성 및 유형 검사
 
- Redux DevTools 통합
 
- `react-native-screens`를 사용하는 탐색을 위해 기본 탐색 프리미티브를 사용하는 기본 스택 탐색기
 
- `react-native-viewpager` 및`ScrollView`를 기반으로하는 Material top tab navigator의 새로운 백엔드
 

시각적 학습자 인 경우 React Native에서 React Navigation 5를 사용하는 방법에 대한 비디오 자습서가 있습니다.
 

## React Native Navigation이란 무엇입니까?
 

React Native Navigation은 React Navigation의 인기있는 대안입니다.
 React Native와 함께 사용하도록 설계된 모듈입니다.
 React Native Navigation은 iOS 및 Android에서 네이티브 탐색 API를 직접 사용한다는 점에서 약간 다릅니다.
 

React Navigation과 React Native Navigation의 차이점에 대한 자세한 내용은 "React Navigation vs. React Native Navigation : 어떤 것이 나에게 적합할까요?"를 확인하세요.
 

## React Navigation 설치
 

Yarn이 설치되어 있다고 가정하면 첫 번째 단계는 React Native 앱을 설정하는 것입니다.
 React Native를 시작하는 가장 쉬운 방법은 Xcode 또는 Android Studio를 설치 및 구성하지 않고도 프로젝트를 시작할 수있는 Expo 도구를 사용하는 것입니다.
 

다음을 실행하여 Expo를 설치하십시오.
 

```coffeescript
npm install -g expo-cli
```

Mac에서 오류가 발생하면 다음과 같이 실행 해보십시오.
 

```undefined
sudo npm install --unsafe-perm -g expo-cli
```

그런 다음 다음을 실행하여 새 React Native 프로젝트를 만듭니다.
 

```undefined
expo init ReactNavigationDemo
```

이렇게하면 다운로드가 시작되고 구성 변수를 입력하라는 메시지가 표시됩니다.
 아래와 같이`expo-template-blank`를 선택하고 종속성 설치를 위해`yarn`을 선택합니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2019/06/react-native-navigation-example-creating-react-native-project.png?resize=720%2C141&ssl=1)

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2019/06/react-native-navigation-example-initial-configuration-values.png?resize=720%2C129&ssl=1)

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2019/06/react-native-navigation-example-choosing-expo-template.png?resize=720%2C166&ssl=1)

다음으로 프로젝트 폴더로 이동하여 코드 편집기를 엽니 다.
 

```bash
cd ReactNavigationDemo
```

VS Code를 사용하는 경우 다음을 사용하여 편집기에서 현재 폴더를 열 수 있습니다.
 

```undefined
code .
```

다음을 사용하여 앱을 시작하십시오.
 

```undefined
yarn start
```

다음 단계는 React Native 프로젝트에`react-navigation` 라이브러리를 설치하는 것입니다.
 

```undefined
yarn add react-navigation
```

<h2 =”stacknavigator”> React Native 스택 탐색기
 

React Navigation은 JavaScript로 빌드되었으며 진정한 네이티브처럼 보이고 느껴지는 구성 요소와 탐색 패턴을 만들 수 있습니다.
 

React Navigation은 스택 내비게이터라는 기능을 사용하여 앱 내에서 사용자가 선택한 경로를 기반으로 적절한 화면의 내비게이션 기록과 표시를 관리합니다.
 한 번에 하나의 화면 만 사용자에게 표시됩니다.
 

종이 더미를 상상해보십시오.
 새 화면으로 이동하면 해당 화면이 스택 맨 위에 배치되고 뒤로 이동하면 해당 화면이 스택에서 제거됩니다.
 스택 내비게이터는 또한 기본 iOS 및 Android와 같은 느낌의 전환 및 제스처를 제공합니다.
 앱에는 둘 이상의 스택 탐색기가있을 수 있습니다.
 

## React Native 탐색 예제
 

이 섹션에서는 React Native 탐색 패턴의 몇 가지 예와 React Navigation 라이브러리를 사용하여이를 달성하는 방법을 살펴 봅니다.
 

### 1. 스택 탐색기를 사용하여 화면 구성 요소 간 탐색
 

먼저 프로젝트의 루트에`/ components` 폴더를 만들어 보겠습니다.
 그런 다음`Homescreen.js`와`Aboutscreen`이라는 두 개의 파일을 만듭니다.
 

```js
// Homescreen.js
import React, { Component } from 'react';
import { Button, View, Text } from 'react-native';
import { createStackNavigator, createAppContainer } from 'react-navigation';

export default class Homescreen extends Component {
  render() {
    return (
      <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
        <Text>Home Screen</Text>
          <Button
          title="Go to About"
          onPress={() => this.props.navigation.navigate('About')}
/>
      </View>
    )
  }
}
```

위 버튼의 `onPress`프롭에 주목하세요. 나중에 무엇을하는지 설명하겠습니다.
 

```js
// Aboutscreen.js
import React, { Component } from 'react';
import { Button, View, Text } from 'react-native';
import { createStackNavigator, createAppContainer } from 'react-navigation';

export default class Aboutscreen extends Component {
  render() {
    return (
      <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
        <Text>About Screen</Text>
      </View>
    )
  }
}
```

프로젝트 폴더는 아래 이미지에 표시된 것과 같아야합니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2019/06/project-folder-contents.png?resize=512%2C410&ssl=1)

`App.js`도 일부 변경해 보겠습니다.
 `react-navigation`에서 필요한 것을 가져 와서 거기에 내비게이션을 구현할 것입니다.
 

`App.js`에서 내 보낸 구성 요소는 React Native 앱의 진입 점 (또는 루트 구성 요소)이고 다른 모든 구성 요소는 자손이므로 루트`App.js` 파일에서 탐색을 구현하는 것이 유용합니다.
 

보시다시피 내비게이션 함수 내에 다른 모든 구성 요소를 캡슐화합니다.
 

```coffeescript
// App.js
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';
import { createStackNavigator, createAppContainer } from "react-navigation";

import HomeScreen from './components/HomeScreen';
import AboutScreen from './components/AboutScreen';


export default class App extends React.Component {
  render() {
    return <AppContainer />;
  }
}

const AppNavigator = createStackNavigator({
  Home: {
    screen: HomeScreen
  },
  About: {
    screen: AboutScreen
  }
});

const AppContainer = createAppContainer(AppNavigator);

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
});
```

위의 코드에서`createStackNavigator`는 앱이 화면 사이를 전환하는 방법을 제공합니다. 여기서 각각의 새 화면은 스택 맨 위에 배치됩니다.
 익숙한 iOS 및 Android 모양과 느낌을 갖도록 구성되어 있습니다. 새 화면은 iOS에서 오른쪽에서 슬라이드 인하 고 Android에서는 하단에서 페이드 인됩니다.
 

경로 구성 객체를`createStackNavigator` 함수에 전달합니다.
 `Home`경로는 `HomeScreen`에 해당하고 `About`경로는 `AboutScreen`에 해당합니다.
 

경로 구성을 작성하는 선택적이고 간결한 방법은`{screen : HomeScreen}`구성 형식입니다.
 

또한 API에 지정된대로 다른 옵션 개체를 선택적으로 추가 할 수 있습니다.
 초기 경로를 나타내려면 별도의 개체를 추가 할 수 있습니다.
 

```cpp
const AppNavigator = createStackNavigator({
  Home: {
    screen: HomeScreen
  },
  About: {
    screen: AboutScreen
  }
},{
        initialRouteName: "Home"
});
```

`Home` 및`About` 경로 이름-값 쌍은 전체 경로 객체로 묶여 있습니다.
 옵션 개체는 포함되어 있지 않지만 별도의 개체입니다.
 

`createStackNavigator` 함수는 뒤에서`HomeScreen` 및`AboutScreen` 구성 요소에 탐색 소품을 전달합니다.
 navigate prop을 사용하면 지정된 화면 구성 요소로 이동할 수 있습니다.
 이것이 바로 `HomeScreen.js`의 버튼에서 사용할 수있는 이유입니다.이 버튼을 누르면 아래와 같이 `AboutScreen`페이지로 연결됩니다.
 

```coffeescript
<Button title="Go to About" 
onPress={() => this.props.navigation.navigate('About')}
/>
```

`App.js` 코드에서 마침내`const AppContainer = createAppContainer (AppNavigator);`를 사용하여 앱 컨테이너를 생성했습니다.
 이 컨테이너는 탐색 상태를 관리합니다.
 

앱을 실행하려면 Expo 클라이언트 앱을 다운로드해야합니다.
 iOS 및 Android 버전을 얻을 수 있습니다.
 명령 줄이 프로젝트 폴더를 가리키는 지 확인하고 다음 명령을 실행합니다.
 

```coffeescript
npm start
```

단말기에 QR 코드가 표시됩니다.
 Android에서 Expo 앱으로 QR 코드를 스캔하고 iOS 앱의 경우 일반 iPhone 카메라를 사용하여 스캔 할 수 있습니다. 그러면 Expo 앱을 열려면 클릭하라는 명령이 표시됩니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2019/06/scanning-qr-code-stack-nav-1.gif?resize=288%2C512&ssl=1)

### 2. 탭 탐색 사용
 

대부분의 모바일 앱에는 두 개 이상의 화면이 있습니다.
 이러한 모바일 앱의 일반적인 탐색 스타일은 탭 기반 탐색입니다.
 여기서는`createBottomTabNavigator`를 사용하여 탭 탐색을 구현하는 방법에 초점을 맞출 것입니다.
 

`/ components` 아래에`ContactScreen.js` 파일을 만들어 앱에 다른 화면을 추가해 보겠습니다.
 

```js
import React, { Component } from 'react'

export default class ContactScreen extends Component {
  render() {
    return (
      <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
        <Text>Contact Screen</Text>
      </View>
    )
  }
}
```

이제`App.js` 파일 상단에 가져 오기를 추가해 보겠습니다.
 

```coffeescript
import ContactScreen from './components/ContactScreen';
```

루트`App.js` 구성 요소에서 탐색을 구현하는 것이 유용합니다.
 따라서 `App.js`에서 `createBottomTabNavigator`를 가져 와서 탭 탐색을 구현합니다.
 `createStackNavigator`를 대체하겠습니다.
 

```coffeescript
import { createBottomTabNavigator, createAppContainer } from "react-navigation";
```

또한`AppNavigator` 객체에서`createStackNavigator`를`createBottomTabNavigator`로 바꿉니다.
 

```cpp
const AppNavigator = createBottomTabNavigator({
  Home: {
    screen: HomeScreen
  },
  About: {
    screen: AboutScreen
  }
}, {
  initialRouteName: "Home"
});
```

`navigator` 객체에 새 화면을 추가합니다.
 

```cpp
const AppNavigator = createBottomTabNavigator({
  Home: {
    screen: HomeScreen
  },
  About: {
    screen: AboutScreen
  },
  Contact: {
    screen: ContactScreen
  }
}, {
  initialRouteName: "Home"
});
```

`npm start`로 앱을 실행하고 Expo 클라이언트에서 열면 하단 탐색이 구현 된 것을 볼 수 있습니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2019/06/scanning-qr-code-bottom-nav-2.gif?resize=288%2C512&ssl=1)

### 3. 서랍 탐색 사용
 

서랍 탐색 구현을 즉시 시작하려면 코드에서`createBottomTabNavigator`를`createDrawerNavigator`로 바꿉니다.
 

import 문부터 시작하겠습니다.
 

```coffeescript
import { createDrawerNavigator, createAppContainer } from "react-navigation";
```

`AppNavigator` 변수도 업데이트 해 보겠습니다.
 

```cpp
const AppNavigator = createDrawerNavigator({
  Home: {
    screen: HomeScreen
  },
  About: {
    screen: AboutScreen
  },
  Contact: {
    screen: ContactScreen
  }
}, {
    initialRouteName: "Home"
  });
```

`npm start`를 실행하면 변경 사항을 바로 확인할 수 있습니다.
 창 탐색을 보려면 왼쪽에서 스 와이프합니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/06/scanning-qr-code-drawer-example.gif?resize=288%2C512&ssl=1)

경로 이름 옆에 아이콘을 추가하여 서랍 탐색을 사용자 정의 할 수 있습니다.
 이 프로젝트의 자산 폴더에는 현재 세 가지 아이콘이 있습니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2019/06/icons-in-assets-folder.png?resize=824%2C272&ssl=1)

다음 화면 구성 요소 파일에`navigationOptions`를 추가하여 사용자 지정할 수 있습니다.
 

```coffeescript
// in HomeScreen.js

import React, { Component } from 'react';
import { Button, View, Text, Image, StyleSheet } from 'react-native';
import { createStackNavigator, createAppContainer } from 'react-navigation';

export default class HomeScreen extends Component {

  static navigationOptions = {
    drawerLabel: 'Home',
    drawerIcon: ({ tintColor }) => (
      <Image
        source={require('../assets/home-icon.png')}
        style={[styles.icon, { tintColor: tintColor }]}
      />
    ),
  };

  render() {
    return (
      <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
        <Text>Home Screen</Text>
        <Button
          title="Go to About"
          onPress={() => this.props.navigation.navigate('About')}
        />
      </View>
    )
  }
}

const styles = StyleSheet.create({
  icon: {
    width: 24,
    height: 24,
  }
});
```

```js
// in AboutScreen.js

import React, { Component } from 'react';
import { Button, View, Text, Image, StyleSheet } from 'react-native';
import { createStackNavigator, createAppContainer } from 'react-navigation';

export default class AboutScreen extends Component {

  static navigationOptions = {
    drawerLabel: 'About',
    drawerIcon: ({ tintColor }) => (
      
    ),
  };
  render() {
    return (
      
        About Screen
      
    )
  }
}

const styles = StyleSheet.create({
  icon: {
    width: 24,
    height: 24,
  }
});
```

```js
// in ContactScreen.js

import React, { Component } from 'react';
import { Button, View, Text, Image, StyleSheet } from 'react-native';

export default class ContactScreen extends Component {

  static navigationOptions = {
    drawerLabel: 'Contact',
    drawerIcon: ({ tintColor }) => (
      
    ),
  };

  render() {
    return (
      
        Contact Screen
      
    )
  }
}

const styles = StyleSheet.create({
  icon: {
    width: 24,
    height: 24,
  }
});
```

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2019/06/navigation-icons-in-drawer-nav.gif?resize=288%2C512&ssl=1)

`tintColor` 소품을 사용하면 탐색 탭 및 레이블의 활성 또는 비활성 상태에 따라 모든 색상을 적용 할 수 있습니다.
 예를 들어, 탐색 창 레이블의 활성 상태 색상을 변경할 수 있습니다.
 `AppNavigator` 변수로 이동하여 옵션 개체에 추가합니다.
 

```undefined
const AppNavigator = createDrawerNavigator({
  Home: {
    screen: HomeScreen
  },
  About: {
    screen: AboutScreen
  },
  Contact: {
    screen: ContactScreen
  }
}, {
    initialRouteName: "Home",
      contentOptions: {
        activeTintColor: '#e91e63'
     }
  });
```

이로 인해 색상이 변경됩니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2019/06/color-change-in-drawer-nav.jpg?resize=1242%2C597&ssl=1)

## React Navigation에서 화면에 매개 변수 전달
 

매개 변수를 경로에 전달하는 간단한 두 단계가 있습니다.
 

- 매개 변수를`navigation.navigate` 함수에 대한 두 번째 매개 변수로 객체에 넣어 경로에 전달합니다.
this.props.navigation.navigate ( `RouteName`, {/ * params go here * /})
 
- 화면 구성 요소에서 매개 변수를 읽으십시오.
this.props.navigation.getParam (paramName, defaultValue)
 

## 결론
 

이 기사가 기존 또는 향후 React Native 프로젝트에서 React Navigation 패키지 사용을 시작하기를 바랍니다.
 

수행 할 수있는 작업이 더 많으며 React Navigation이 대부분의 요구 사항을 충족합니다.
 

자세한 내용은 React Navigation 문서를 확인하고 내 GitHub 저장소에서 최종 코드를 가져 오세요.
 