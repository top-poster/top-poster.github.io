---
layout: post
title: "반응 탐색 5에 대한 전체 가이드"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/12/react-navigator.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/react-navigator.png?fit=730%2C487&ssl=1)

## 도입

모바일 애플리케이션 개발에는 많은 이동 부품이 필요합니다. 스타일링, 기능, 인증 및 세션 지속성부터 탐색까지. 이 기사의 목표는 React Navigation 5를 사용하여 React Native 기반 앱에서 간단하고 복잡한 내비게이션 시스템을 구축하는 데 필요한 기술과 지식을 습득하는 것입니다.

React Navigation은 React Native에서 화면을 쉽게 만들고 연결할 수 있는 구성 요소로 구성된 라이브러리입니다. 모바일 애플리케이션에서 내비게이션이 어떻게 작동하는지, 모바일 기반 내비게이션과 웹 기반 내비게이션 사이에 어떤 큰 차이가 있는지 이해하는 것이 중요하다.

이전에 웹 사이트를 설계하고 개발한 적이 있는 경우, 개발자가 웹 페이지의 탐색을 조작할 수 있는 다양한 속성으로 구성된 앵커 태그 `a`를 통해 다양한 웹 페이지 간을 탐색할 수 있습니다. 개발자는 브라우저에서 처리할 수 있으므로 페이지의 동작을 명시적으로 정의할 필요가 없습니다. 모바일 애플리케이션의 경우 개발 초기 단계에서 내비게이션이 명시적으로 처리되고 정의되어야 합니다. 탐색 유형을 정의하는 것에서부터 네비게이션에 중첩되는 것까지, 모바일 응용프로그램에서의 탐색은 더 많은 노력을 필요로 하며 때로는 상당히 복잡할 수 있다.

## 반응 탐색의 탐색 유형

### 서랍 탐색

화면 간 및 화면 통과를 위해 왼쪽(때로는 오른쪽)의 드로어를 사용해야 하는 탐색 패턴으로 구성됩니다. 일반적으로 화면 간에 이동할 수 있는 게이트웨이를 제공하는 링크로 구성됩니다. 웹 기반 애플리케이션의 사이드바와 유사합니다. 트위터는 서랍 내비게이션을 활용하는 인기 있는 응용 프로그램입니다.

### 탭 탐색

대부분의 모바일 애플리케이션에서 가장 일반적인 탐색 형식입니다. 탭 기반 구성 요소를 통해 탐색할 수 있습니다. 아래 탭과 위 탭으로 구성되어 있습니다. 탭 기반 네비게이션을 활용한 인기 앱은 인스타그램이다. 아래쪽 탐색에는 홈 화면을 사용하고 위쪽 탭 탐색에는 프로필 화면을 사용합니다.

### 스택 탐색

화면 간 전환은 스택 형태로 가능합니다. 다양한 화면이 서로 겹쳐져 있고, 화면 간 이동은 한 화면을 다른 화면으로 교체하는 것을 포함한다. 스택 탐색을 활용하는 인기 있는 응용 프로그램은 WhatsApp 채팅 화면입니다.

대부분의 경우, 모바일 애플리케이션은 애플리케이션의 다양한 화면과 기능을 구축하기 위해 모든 유형의 네비게이터를 사용한다. 이를 중첩 네비게이터라고 합니다. 이 개념을 활용한 인기 앱은 트위터다. 홈 화면에는 다른 화면으로 이동하는 서랍 탐색 유형이 있습니다. 검색 탭에는 상단 탭 탐색이 포함되어 있으며 선택한 트윗을 기준으로 DM(Direct Messages) 화면에서 채팅으로 이동할 수 있습니다. 이것은 스택 탐색을 통해 가능합니다.

이 자료에서는 이러한 네비게이션을 차례로 작성하는 방법에 대해 설명합니다. 그리고 나서, 우리는 둥지를 통해 그것들을 결합할 것입니다.

### 전제조건

- Nodejs LTS 릴리스 이상
- Expo(아래 설치)
- Mac 사용자용 Watchman

이 튜토리얼을 위해 엑스포를 활용하겠습니다. Expo는 React Native를 사용하여 모바일 애플리케이션을 매우 쉽게 개발할 수 있도록 지원합니다. 다른 혜택이 많으니, 여기 있는 설명서를 확인해 보세요.

다음 명령을 사용하여 Expo(엑스포)를 설치합니다.

```coffeescript
npm install expo-cli --global
```

이 튜토리얼을 따라 하기 위해 필요한 기본 설정이 포함된 GitHub 저장소를 제공했습니다. 다양한 반응 탐색 패키지를 함께 설치할 것입니다. 여기에서 리포지토리를 복제합니다.

다음 명령을 사용하여 반응 탐색을 설치합니다.

```css
npm install @react-navigation/native
```

Expo를 사용하여 종속성 설치:

```undefined
expo install react-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-context @react-native-community/masked-view
```

참고: 종속성을 Expo 기반 프로젝트가 아닌 프로젝트에 설치할 수 있습니다. 우리는 엑스포를 활용하기 때문에, 이러한 의존성을 설치하기 위해 엑스포를 사용할 필요가 있습니다. Expo는 설치된 종속성의 버전이 애플리케이션의 React Native 버전과 호환되는지 확인합니다. 비Expo 기반 응용 프로그램에 설치하려면 설명서를 참조하십시오.

방금 설치한 패키지에 대해 잠시 살펴보겠습니다. 리액트 네이티브에서 가능한 최상의 터치 기반 경험을 구축하기 위한 네이티브 기반 제스처 관리 API를 제공하기 위해 `액트 네이티브 제스처 핸들러`를 설치했다.

또한 제스처 기반 애니메이션을 처리할 수 있는 `반동 민족`과 `원래 항해 컨테이너 구성품을 제공할 수 있는 `반동 민족-반동 민족`과 `반동 민족-안전 영역 컨텍스트`를 설치하여 구성 요소의 안전한 영역이나 경계를 제공하였다.

설치를 건너뛰고 이 프로세스 중 1단계에서 시작할 수 있습니다. 그 안에 우리가 위에 설치한 모든 것을 찾을 수 있을 것이다. 계속하려면 npm i를 실행하십시오.

### 프로젝트 구조

먼저 App.js 파일을 찾을 수 있습니다. 그것이 우리 지원서의 항목이다.

Expo Babel 구성을 포함하는 babel.config.js 파일도 있습니다. Expo에서 자동으로 처리하므로, 이 문제로 신경 쓸 필요가 없습니다.

IOS, Android 및 기타 웹 플랫폼용 애플리케이션의 공유 구성을 포함하는 app.json 파일도 제공됩니다. 여기서는 스플래시 화면 구성, 방향, 버전 등을 처리할 수 있습니다. 이 가이드에서 사용 가능한 광범위한 구성 목록을 확인해야 합니다.

루트 디렉터리에서 src라는 새 폴더를 생성합니다.

mkdir src

내부에서 `mkdir {components, 화면, 탐색}` 디렉터리를 3개 더 생성했습니다.
구성 요소 디렉토리에는 구성 요소가, 화면에는 각 화면이, 탐색 디렉토리에는 탐색 구성이 포함됩니다.

## 탭 탐색 데모

진행하기 전에 우리가 무엇을 만들어야 하는지 이해하자.

내비게이션 유형이 다른 5개의 스크린이 있습니다: 스택, 탭, 드로어. 사물을 보아하니, 우리는 다양한 네비게이션 타입을 결합하여 어플리케이션을 만들고 있다. 네 번째와 다섯 번째 화면을 포함하는 서랍 탐색, 두 번째 화면의 상하 탭 탐색, 첫 번째 화면의 공유 하단 탐색, 세 번째 화면의 스택 탐색.

이제 우리가 해야 할 일이 이해되었으니, 네 개의 파일을 만들어 봅시다. 3가지 유형의 네비게이션과 3가지 유형을 모두 조합한 네비게이션 입력으로 사용할 수 있는 색인 3가지입니다.

세 가지 파일:

cd src/내비게이션
{index, appStackNavigator, appTabNavigator, appDrawerNavigator}.js를 누릅니다.
cd –

반응 탐색 5의 탐색은 탐색 컨테이너에 포장된 탐색 화면 구성을 프로그램의 특정 화면에 매핑함으로써 가능합니다. 탐색을 성공적으로 만들려면 연결할 화면이 있어야 합니다.

다음을 실행합니다.

```bash
cd src/screens
```

```undefined
touch {FirstScreen,SecondScreen,ThirdScreen,FourthScreen,FifthScreen}.js
```

각 화면으로 이동하여 아래 코드를 붙여넣으십시오.

파일 이름을 기준으로 올바른 화면을 정의 및 내보내고 있는지 확인합니다.

```js
import React from 'react'
import { View, Text } from 'react-native'

const FirstScreen = ({navigation, route}) => {
    return (
        <View>
            <Text>{} Screen</Text>
        </View>
    )
}

export default FirstScreen
```

스타일링과 아이콘에 대해 `반응-원소` 패키지를 활용해보자.

```undefined
npm install react-native-elements
```

탭 탐색을 탭 화면에 매핑해 보겠습니다. 탭 탐색에 필요한 종속성 설치:

```css
npm install @react-navigation/material-bottom-tabs react-native-paper @react-navigation/material-top-tabs react-native-tab-view
```

그런 다음 이러한 패키지가 이미 설치되어 있습니다. npmi를 실행합니다.

우리는 상단과 하단 네비게이터를 모두 사용할 것이다. appTabNavigator.js 파일의 첫 번째 및 두 번째 화면과 함께 가져옵니다. 하단 탭에도 아이콘이 필요합니다. 엑스포와 함께 미리 설치된 `반토종 벡터 아이콘`에서 수입해 보자. 맨 아래 탭 탐색에서도 아이콘을 사용합니다. react-native-vector-icons에서 아이콘을 가져오도록 하자.

```coffeescript
import React from "react";
import { createMaterialTopTabNavigator } from "@react-navigation/material-top-tabs";
import  { createMaterialBottomTabNavigator } from "@react-navigation/material-bottom-tabs";
import Icon from "react-native-vector-icons/FontAwesome5";

import FirstScreen from "../screens/FirstScreen";
import SecondScreen from "../screens/SecondScreen";
```

material tab navigator 및 material bottom tab navigator의 인스턴스를 만듭니다.

```cpp
const AppBottomNavigator = createMaterialBottomTabNavigator();
const AppTopNavigator = createMaterialTopTabNavigator();
```

두 번째 화면에는 상단 탭 네비게이터가 포함되어 있으므로 이 작업을 수행해 보겠습니다.

```coffeescript
const SecondScreenTopTabNavigator = () => (
  <AppTopNavigator.Navigator
    initialRouteName="CHATS"
    tabBarOptions={
      style: { backgroundColor: "#490222" },
      labelStyle: { fontSize: 14, fontWeight: "bold" },
      activeTintColor: "#ffffff",
      indicatorStyle: { height: 3, backgroundColor: "#fff", paddingBottom: 6 },
      inactiveTintColor: "#adadad",
      tabStyle: { height: 100, justifyContent: "flex-end" }
    }
  >
    <AppTopNavigator.Screen component={SecondScreen} name="CHATS" />
    <AppTopNavigator.Screen component={SecondScreen} name="REQUESTS" />
  </AppTopNavigator.Navigator>
);
```

각 탐색은 암시적으로 반환되는 반응 구성 요소입니다. 구성 요소에 다양한 스타일과 구성을 적용할 수 있습니다. 위에서 정의한 탭 탐색 인스턴스에서 제공된 화면을 네비게이터 구성요소에 싸고, `tabBar Options` 소품을 사용하여 스타일을 정의한다. 네비게이터 래퍼에는 네비게이션을 스타일링하고 구성할 수 있는 다양한 구성이 있습니다.

여기서는 배경을 UI 색상으로 만들고 각 탭의 활성 및 비활성 상태를 변경했습니다. 구성에 사용할 수 있는 옵션 목록을 보려면 각 탐색 유형의 설명서를 참조하십시오. 초기 경로 이름 소품은 탐색 구성 요소가 탐색하려는 특정 화면을 식별하는 데 사용하는 것입니다. 모든 화면에 대해 고유해야 합니다.

상단 탭 탐색기의 인스턴스는 정의된 화면을 탐색 래퍼에 연결하는 화면 구성 요소도 제공합니다. 다양한 소품을 가져갈 수 있는데, 그 중 하나가 화면을 고정하는 부품이며, 화면마다 고유 식별자 역할을 하는 이름이다. 기타 구성 옵션은 설명서를 참조하십시오.

아래 네비게이션으로 진행하겠습니다. 이전에 하단 탐색을 위한 인스턴스를 생성했음을 기억하십시오. 아래쪽 탐색기는 아이콘이 있기 때문에 다릅니다.

```xml
const AllScreenTabNavigator = () => (
  <AppBottomNavigator.Navigator
    initialRouteName="First"
    screenOptions={
      tabBarColor: "#490222"
    }
  >
    <AppBottomNavigator.Screen
      name="First"
      component={FirstScreen}
      options={
        tabBarIcon: () => <Icon name="book-open" size={25} color="#fff" />
      }
    />
    <AppBottomNavigator.Screen
      name="First Two"
      component={FirstScreen}
      options={
        tabBarIcon: () => <Icon name="file-alt" size={25} color="#fff" />
      }
    />
    <AppBottomNavigator.Screen
      name="Second"
      children={SecondScreenTopTabNavigator}
      options={
        tabBarIcon: () => <Icon name="comment-alt" size={25} color="#fff" />
      }
    />
    <AppBottomNavigator.Screen
      name="Second Two"
      children={SecondScreenTopTabNavigator}
      options={
        tabBarIcon: () => <Icon name="user" size={25} color="#fff" />
      }
    />
  </AppBottomNavigator.Navigator>
);

export { AllScreenTabNavigator };
```

탭 탐색기를 성공적으로 완료했습니다. 옵션 소품을 통해 아이콘을 사용하고 두 번째 화면의 상단 탭 탐색기를 하위 소품을 통해 연결합니다. 두 번째 화면에도 탐색 구성 요소가 포함되어 있고 하단 탐색기의 하위 화면이기 때문에 구성요소 대신 하위 항목을 사용합니다.

index.js로 이동하여 현재 사용 중인 작업에 대한 데모를 확인하십시오. 잠시 후 이 파일을 다시 방문하여 상황을 파악합니다.

```js
import React from "react";
import { NavigationContainer } from "@react-navigation/native";
import { AllScreenTabNavigator } from "./appTabNavigator";

const RootNavigator = () => (
  <NavigationContainer>
    <AllScreenTabNavigator />
  </NavigationContainer>
);
export default RootNavigator;
```

또한 App.js 파일의 코드를 다음과 같이 바꿉니다.

```js
import "react-native-gesture-handler";
import React from "react";
import { StyleSheet } from "react-native";
import RootNavigator from "./src/navigation/index";

export default function App() {
  return (
    <>
      <RootNavigator />
    </>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: "#fff",
    alignItems: "center",
    justifyContent: "center"
  }
});
```

이제 Expo start를 사용하여 프로젝트를 실행하면 다음과 같이 표시됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/expo-screen.png?resize=730%2C1579&ssl=1)

전체 코드를 보려면 탭 탐색 분기로 이동하십시오.

## 서랍 탐색 데모

서랍 탐색 패키지를 설치합니다.

```css
npm install @react-navigation/drawer
```

필요한 패키지를 가져오고 드로어 탐색기의 인스턴스를 만듭니다.

```js
import React from 'react';
import { createDrawerNavigator } from '@react-navigation/drawer';
import FourthScreen from '../screens/FourthScreen'
import FifthScreen from '../screens/FifthScreen'

const Drawer = createDrawerNavigator();
```

탭 네비게이터를 대응 구성요소로 정의한 방법과 동일한 방법으로 서랍 네비게이터를 작성할 수 있습니다.

```xml
const AllDrawerNavigation = () => (
    <Drawer.Navigator initialRouteName='Fourth'>
        <Drawer.Screen component={FourthScreen} name='Fourth' />
        <Drawer.Screen component={FifthScreen} name='Fifth' />
    </Drawer.Navigator>
)

export default AllDrawerNavigation;
```

내보냅니다.

navigation/index.js 파일로 가져오고 `AllScreenTabNavigator`를 `AllDrawerNavigator`로 바꾸면 동작합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/fourth-and-fifth-screens.png?resize=730%2C1573&ssl=1)

전체 코드를 보려면 서랍-네비게이션 분기로 이동하십시오.

## 스택 탐색 데모

마지막 화면은 스택 탐색기입니다.

필요한 패키지 설치:

```css
npm install @react-navigation/stack
```

필요한 종속성을 가져오고 스택 탐색기의 인스턴스를 만듭니다.

```js
import React from 'react'
import { createStackNavigator } from "@react-navigation/stack";

import ThirdScreen from '../screens/ThirdScreen'

const StackNavigator = createStackNavigator();
```

위에서 만든 StackNavigator 인스턴스를 통해 세 번째 화면을 연결하고 내보냅니다.

```coffeescript
const ThirdScreenNavigation = () => (
  <StackNavigator.Navigator
    initialRouteName="Third"
    screenOptions={
      header: () => null
    }
  >
    <StackNavigator.Screen component={ThirdScreen} name="Third" />
  </StackNavigator.Navigator>
);

export default ThirdScreenNavigation;
```

index.js의 AllDrawerNavigation을 내보낸 ThirdScreenNavigation으로 대체하여 작동 상태를 확인합니다.

스택 탐색 분기로 이동하여 작동 방식을 확인합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/New-screen.png?resize=730%2C1523&ssl=1)

### 내비게이션 결합(네스팅)

이제 모든 네비게이터를 결합하겠습니다. 이 작업은 index.js 파일에서 수행됩니다. 계속하기 전에 화면 간의 공유 UI를 Layout.js라는 별도의 구성 요소로 내보내서 화면을 크게 개선해 보겠습니다.

구성 요소 디렉터리에 Layout.js 파일을 생성하십시오.

```js
import React from "react";
import { View, Text } from "react-native";

const Layout = props => {
  const { name = "" } = props;
  return (
    <View
      style={
        justifyContent: "center",
        alignItems: "center",
        flex: 1
      }
    >
      <Text>{name} Screen</Text>
    </View>
  );
};

export default Layout;
```

우리는 화면 사이의 공유 UI를 별도의 파일로 옮겼습니다. Layout 구성 요소에는 이름 받침대가 있어야 합니다. 우리는 소품을 파괴하고 거기에 이름이 전달되지 않을 경우에 대비하여 기본값을 전달하고 있습니다.

화면의 반품 내역서에 있는 모든 코드를 다음과 같이 바꿉니다.

```undefined
<>
      <Layout />
 </>
```

이제 모든 네비게이터를 합치자.

여기에서 후속 조치하려면 사전 결합된 내비게이션 지점을 확인하십시오.

반응에 이전에 만든 모든 탐색을 가져옵니다. 또한 전체 응용프로그램을 포장할 네비게이션 컨테이너를 가져오십시오. 이 탐색 컨테이너는 이전에 생성된 전체 탐색을 감싸고 애플리케이션의 입력 역할을 합니다.

```coffeescript
import React from "react";
import { NavigationContainer } from "@react-navigation/native";
import { AllScreenTabNavigator } from "./appTabNavigator";
import AllDrawerNavigation from "./appDrawerNavigator";
import ThirdScreenNavigation from "./appStackNavigator";
```

앱이 어떻게 구성되어 있는지 생각해 봅시다. 첫 번째 및 두 번째 화면은 세 번째 화면의 스택 탐색 유형에 대한 항목을 포함하는 탭 탐색 유형입니다.

이 화면에는 네 번째 화면과 다섯 번째 화면(드로어 탐색 유형)도 표시됩니다. 겉보기에는 5개의 화면 모두 일반적인 탐색 유형인 스택 탐색으로 연결되어 있는 것이 분명합니다.

탭에서 스택 탐색으로 이동하면 각 탐색 유형을 다른 탐색 유형으로 바꿀 수 있습니다. 따라서 전체 앱은 내비게이션 용기로 포장된 스택 탐색으로 연결됩니다.

`createStackNavigation`을 가져오고 아래 코드를 붙여넣으십시오.

```xml
const AllAppNavigation = createStackNavigator();

const RootNavigator = () => (
  <NavigationContainer>
    <AllAppNavigation.Navigator
      initialRouteName="tab"
      screenOptions={
        header: () => null
      }
    >
      <AllAppNavigation.Screen name="tab" children={AllScreenTabNavigator} />
      <AllAppNavigation.Screen name="stack" children={ThirdScreenNavigation} />
      <AllAppNavigation.Screen name="drawer" children={AllDrawerNavigation} />
    </AllAppNavigation.Navigator>
  </NavigationContainer>
);
export default RootNavigator;
```

App.js 파일로 이동하여 해당 코드를 다음과 같이 바꿉니다.

```js
import "react-native-gesture-handler";
import React from "react";
import { StyleSheet } from "react-native";
import RootNavigator from "./src/navigation/index";

export default function App() {
  return (
    <>
      <RootNavigator />
    </>
  );
}
```

각 탐색 구성 요소의 이름을 지정하고 필요할 때 각 구성 요소를 탐색할 수 있도록 탭, 스택 및 드로어 구성 요소를 각각 만들었습니다. 내비게이션 아동을 나타내기 위해 다른 라벨을 사용할 수도 있지만, 편의상 이 라벨을 사용합니다. 지금 앱을 확인하세요. 모든 것이 연결되어 있고 작동됩니다.

### 화면 간 탐색

FirstScreen에서는 탐색 래퍼에서 전달된 탐색 및 경로 개체를 기록합니다.

내비게이션 객체에는 화면 간 전환을 가능하게 하는 방법과 이벤트가 있으며, 경로 객체에는 화면 간 공유 상태(파람) 정보가 포함되어 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/object-code-snippet.png?resize=730%2C560&ssl=1)

경로 객체의 `route.name` 키를 Layout 구성 요소로 전달합니다. 탐색 소품은 다른 화면으로 이동하는 데 사용됩니다. 두 개체를 모두 기록하면 이 값이 표시됩니다. 전부 커버하지는 않겠지만, 다른 화면들 사이를 옮겨야 할 몇 가지만 커버할 수 있을 거예요.

탐색 방법은 다른 화면으로 이동하는 데 사용됩니다.

GoBack(고백) 방법은 이전 화면으로 이동하는 데 사용됩니다.

푸시 방법은 탐색과 동일하게 작동합니다.

set Params는 파라미터를 다른 화면으로 전달하는 데 사용됩니다. 게시물의 id를 전달하여 해당 게시물의 댓글을 댓글 화면에 띄우는 것이 한 예다.

경로에 있는 이름 값은 화면에 지정된 이름입니다.

화면 사이를 통과하는 매개 변수를 경로 객체에 연결할 수 있습니다.

우리는 첫 화면에서 3번째 화면에 파일의 위 가는 것, 추가 단추를 추가할까요.

```coffeescript
import { Button } from "react-native-elements";
```

레이아웃 구성요소 아래에 다음을 추가합니다.

```bash
<Button
        type="solid"
        title="Third Screen"
        containerStyle={
          justifyContent: "center",
          alignItems: "center",
          height: 200
        }
        buttonStyle={
          borderColor: "#490222",
          backgroundColor: "#490222",
          width: 160,
          borderWidth: 1.3
        }
        titleStyle={
          color: "#fff"
        }
        onPress={() => navigation.navigate("stack")}
      />
```

화면을 네비게이션 오브젝트의 탐색 방법에 결합할 때 세 번째 화면에 제공한 레이블을 전달합니다.

세 번째 화면에서는 네 번째 화면으로 이동할 수 있는 버튼을 추가해 보겠습니다. 위의 단계를 반복하되 버튼 제목을 네 번째 화면으로 변경하고 드로어를 탐색 방법에 인수로 전달합니다.

## 결론

Respon Native(원본 반응)에서 화면 생성 및 화면 간 탐색에 대해 성공적으로 설명했습니다. 이 가이드에서는 프로젝트를 위한 간단하고 복잡한 탐색을 구축하는 데 필요한 대부분의 지식과 도구를 제공합니다. 네비게이션의 정의 및 구조는 첫 번째 작업에 포함되어야 합니다.

질문 있습니까? 제가 취재했으면 좋았을 만한 게 있나요? 얼마든지 댓글 남겨주세요.