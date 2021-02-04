---
layout: post
title: "React Navigation v5에서 공유 요소 전환을 사용하는 방법
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/reactnavigationv5.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/reactnavigationv5.png?fit=730%2C487&ssl=1)

모바일 애플리케이션의 전환은 설계 연속성을 제공합니다.
 이 연속성은 앱에서 탐색하는 동안 한보기에서 다음보기로 공통 요소를 연결하여 제공됩니다.
 

## 공유 요소 전환이란 무엇입니까?
 

서로 다른보기 또는 활동 간의 전환에는 서로 독립적 인 전체보기 계층 구조를 애니메이션화하는 진입 및 종료 전환이 포함됩니다.
 연속성에있는 두 개의 서로 다른 뷰가 공통 요소를 갖는 경우가 있습니다.
 이러한 공통 요소를 한보기에서 두 번째보기로 전환하는 방법을 제공하고 전환 간의 연속성을 강조합니다.
 이러한 전환의 특성은 콘텐츠에 대한 최종 사용자의 초점을 유지하고 원활한 경험을 제공합니다.
 공유 요소 전환은 초점과 경험을 유지하기 위해 두 개의 서로 다른보기가 하나 이상의 요소를 공유하는 방법을 결정합니다.
 

## 전제 조건
 

시작하기 전에 로컬 환경에 다음이 설치되어 있는지 확인하십시오.
 

- Node.js 버전> = 12.x.x 설치됨
 
- npm, yarn 또는 npx와 같은 하나의 패키지 관리자에 대한 액세스
 
- expo-cli 설치 또는 npx 사용
 

시연을 위해 iOS 시뮬레이터를 사용할 것입니다.
 Android 기기 또는 에뮬레이터를 사용하려는 경우이 게시물에서 공유 한 코드 스 니펫이 동일하게 실행됩니다.
 

## 공유 요소 전환 라이브러리 설치
 

시작하려면`expo-cli`를 사용하여 새로운 React Native 프로젝트를 만들어 보겠습니다.
 터미널 창에서 아래 명령을 실행 한 다음 새로 생성 된 프로젝트 디렉터리 내부를 탐색합니다.
 탐색 후 공유 요소 전환을 생성하는 데 필요한 라이브러리를 설치합니다.
 스택 탐색 패턴을 사용하여 한 화면에서 다른 화면으로 `반응 탐색`을 사용해 보겠습니다.
 

React Navigation 라이브러리를 설치하려면 공식 문서에서 다음 지침을 참조하십시오.
 이러한 종속성은 시간에 따라 변경됩니다.
 

```undefined
npx expo init shared-element-transitions
cd shared-element-transitions
yarn add @react-navigation/native react-native-animatable
expo install react-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-context @react-native-community/masked-view
yarn add react-native-shared-element react-navigation-shared-element@next
```

이러한 라이브러리를 설치 한 후 Expo 앱을 실행하는 방법을 확인해 보겠습니다.
 터미널에서`yarn start` 명령을 실행하여 Expo 앱의 빌드를 트리거합니다.
 그런 다음 시뮬레이터 또는 장치에 따라 터미널 프롬프트에서 올바른 옵션을 선택하십시오.
 예를 들어 iOS 시뮬레이터에서이 앱을 초기 상태로 실행하려면 `i`를 누릅니다.
 

iOS 시뮬레이터의 출력이 표시되는 방법은 다음과 같습니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/outputiniossimulator.png?resize=300%2C557&ssl=1)

이 출력은 Expo 앱이 실행 중인지 확인합니다.
 

## 홈 화면 만들기
 

이 예제 앱의 전환은 홈 화면과 세부 정보 화면 사이에서 이루어집니다.
 홈 화면은 스크롤 가능한 이미지와 일부 데이터 목록이 될 것입니다.
 모의 데이터 배열 세트를 사용할 것입니다.
 시험해보고 싶은 데이터를 자유롭게 사용할 수 있습니다.
 데이터 세트에 대해 신경 쓰지 않고 모의 데이터를 사용할 수 있습니다.
 `config /`라는 새 디렉토리를 만들고 그 안에 다음 배열과 객체로`data.js`라는 새 파일을 만듭니다.
 

```js
export const data = [
  {
    id: '1',
    title: 'Manarola, Italy',
    description: 'The Cliffs of Cinque Terre',
    image_url:
      'https://images.unsplash.com/photo-1516483638261-f4dbaf036963?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=633&q=80',
    iconName: 'location-pin'
  },
  {
    id: '2',
    title: 'Venezia, Italy',
    description: 'Rialto Bridge, Venezia, Italy',
    image_url:
      'https://images.unsplash.com/photo-1523906834658-6e24ef2386f9?ixlib=rb-1.2.1&ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&auto=format&fit=crop&w=630&q=80',
    iconName: 'location-pin'
  },
  {
    id: '3',
    title: 'Prague, Czechia',
    description: 'Tram in Prague',
    image_url:
      'https://images.unsplash.com/photo-1513805959324-96eb66ca8713?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=634&q=80',
    iconName: 'location-pin'
  }
];
```

그런 다음 두 앱 화면이 표시 될`screens /`라는 새 디렉토리를 만듭니다.
 내부에`HomeScreen.js`라는 파일을 만들고 다음 문을 가져옵니다.
 

```coffeescript
import React from 'react';
import {
  ScrollView,
  Text,
  View,
  TouchableOpacity,
  Image,
  Dimensions
} from 'react-native';
import { StatusBar } from 'expo-status-bar';
import { SimpleLineIcons } from '@expo/vector-icons';
import { data } from '../config/data';
```

React Native의`Dimensions` API를 사용하여 이미지 구성 요소의 초기 너비와 높이를 정의 해 보겠습니다.
 아래 코드 스 니펫에서 화면의 `너비`를 사용하여 너비와 높이를 모두 계산합니다.
 

```undefined
const { width } = Dimensions.get('screen');
const ITEM_WIDTH = width * 0.9;
const ITEM_HEIGHT = ITEM_WIDTH * 0.9;
```

`HomeScree` 컴포넌트는`navigation`이라는 하나의 prop을 받아들이는 기능적인 React 컴포넌트가 될 것입니다.
 `navigation` 소품은 홈 화면에서`DetailScreen`으로의 탐색을 허용합니다.
 모든 React Native 앱에서 React Navigation 라이브러리는 자동으로 `navigation`객체에 대한 액세스를 추가로 제공하는 컨텍스트를 제공합니다.
 소품에는 탐색 작업을 전달하는 다양한 함수가 포함되어 있습니다.
 

```xml
export default function HomeScreen({ navigation }) {
  return (
    <View style={ flex: 1, backgroundColor: '#0f0f0f' }>
      <StatusBar hidden />
      {/* Header */}
      <View style={ marginTop: 50, marginBottom: 20, paddingHorizontal: 20 }>
        <Text style={ color: '#888', textTransform: 'uppercase' }>
          Saturday 9 January
        </Text>
        <Text style={ color: '#fff', fontSize: 32, fontWeight: '600' }>
          Today
        </Text>
      </View>
  )
}
```

이 기능적 구성 요소는 표시 할 더미 정보를 나타내는 헤더를 렌더링하고 그 아래에 이미지 목록을 스크롤하는 `ScrollView`를 렌더링합니다.
 각 이미지는 아이콘과 이미지에 관한 정보를 표시합니다.
 이 이미지와 그 위에있는 텍스트는 나중에 홈 화면과 상세 화면간에 전환이 발생할 때 큰 역할을합니다.
 `ScrollView` 구성 요소 내에서 JavaScript의`map ()`메서드를 사용하여 모의 데이터를 렌더링 해 보겠습니다.
 어딘가에 호스팅 된 REST API에서 데이터를 주입하고 특정 데이터 세트의 항목 수를 잘 모르는 경우 `ScrollView`대신 React Native의 `FlatList`구성 요소를 사용하세요.
 

```js
return (
  {/* Scrollable content */}
<View style={ flex: 1, paddingBottom: 20 }>
  <ScrollView
    indicatorStyle='white'
    contentContainerStyle={ alignItems: 'center' }
  >
    {data.map(item => (
      <View key={item.id}>
        <TouchableOpacity
          activeOpacity={0.8}
          style={ marginBottom: 14 }
          onPress={() => navigation.navigate('DetailScreen', { item })}
        >
          <Image
            style={
              borderRadius: 14,
              width: ITEM_WIDTH,
              height: ITEM_HEIGHT
            }
            source={ uri: item.image_url }
            resizeMode='cover'
          />
          <View
            style={
              position: 'absolute',
              bottom: 20,
              left: 10
            }
          >
            <View style={ flexDirection: 'row' }>
              <SimpleLineIcons size={40} color='white' name={item.iconName} />
              <View style={ flexDirection: 'column', paddingLeft: 6 }>
                <Text
                  style={
                    color: 'white',
                    fontSize: 24,
                    fontWeight: 'bold',
                    lineHeight: 28
                  }
                >
                  {item.title}
                </Text>
                <Text
                  style={
                    color: 'white',
                    fontSize: 16,
                    fontWeight: 'bold',
                    lineHeight: 18
                  }
                >
                  {item.description}
                </Text>
              </View>
            </View>
          </View>
        </TouchableOpacity>
      </View>
    ))}
  </ScrollView>
</View>);
```

## 상세 화면 만들기
 

`DetailScreen` 구성 요소는 홈 화면에서 스크롤 목록의 일부인 각 이미지에 대한 세부 정보를 렌더링합니다.
 이 화면에서는 화면 상단에있는 뒤로 탐색 버튼과 함께 이미지가 표시됩니다.
 React Navigation 라이브러리에서`route.params`를 사용하여 해체 된`item` 객체 형태로 데이터를 수신합니다.
 이미지 아래에는 홈 화면과 일부 더미 텍스트와 공유 될 제목이 표시됩니다.
 

`screens /`디렉토리 내에`DetailScreen.js`라는 새 파일을 만들고 다음 코드 스 니펫을 추가합니다.
 

```coffeescript
import React, { useRef } from 'react';
import {
  StyleSheet,
  Text,
  View,
  ScrollView,
  Image,
  Dimensions
} from 'react-native';
import { SimpleLineIcons, MaterialCommunityIcons } from '@expo/vector-icons';
const { height } = Dimensions.get('window');
const ITEM_HEIGHT = height * 0.5;
const DetailScreen = ({ navigation, route }) => {
  const { item } = route.params;
  return (
    <View style={ flex: 1, backgroundColor: '#0f0f0f' }>
      <Image
        source={ uri: item.image_url }
        style={
          width: '100%',
          height: ITEM_HEIGHT,
          borderBottomLeftRadius: 20,
          borderBottomRightRadius: 20
        }
        resizeMode='cover'
      />
      <MaterialCommunityIcons
        name='close'
        size={28}
        color='#fff'
        style={
          position: 'absolute',
          top: 40,
          right: 20,
          zIndex: 2
        }
        onPress={() => {
          navigation.goBack();
        }
      />
      <View
        style={ flexDirection: 'row', marginTop: 10, paddingHorizontal: 20 }
      >
        <SimpleLineIcons size={40} color='white' name={item.iconName} />
        <View style={ flexDirection: 'column', paddingLeft: 6 }>
          <Text
            style={
              color: 'white',
              fontSize: 24,
              fontWeight: 'bold',
              lineHeight: 28
            }
          >
            {item.title}
          </Text>
          <Text
            style={
              color: 'white',
              fontSize: 16,
              fontWeight: 'bold',
              lineHeight: 18
            }
          >
            {item.description}
          </Text>
        </View>
      </View>
      <ScrollView
        indicatorStyle='white'
        style={
          paddingHorizontal: 20,
          backgroundColor: '#0f0f0f'
        }
        contentContainerStyle={ paddingVertical: 20 }
      >
        <Text
          style={
            fontSize: 18,
            color: '#fff',
            lineHeight: 24,
            marginBottom: 4
          }
        >
          Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
          eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad
          minim veniam, quis nostrud exercitation ullamco laboris nisi ut
          aliquip ex ea commodo consequat. Duis aute irure dolor in
          reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla
          pariatur. Excepteur sint occaecat cupidatat non proident, sunt in
          culpa qui officia deserunt mollit anim id est laborum.
        </Text>
        <Text
          style={
            fontSize: 18,
            color: '#fff',
            lineHeight: 24,
            marginBottom: 4
          }
        >
          Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
          eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad
          minim veniam, quis nostrud exercitation ullamco laboris nisi ut
          aliquip ex ea commodo consequat. Duis aute irure dolor in
          reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla
          pariatur. Excepteur sint occaecat cupidatat non proident, sunt in
          culpa qui officia deserunt mollit anim id est laborum.
        </Text>
      </ScrollView>
    </View>
  );
};
export default DetailScreen;
```

## 앱에 탐색 추가
 

홈 화면에서 세부 화면으로 이동했다가 뒤로 이동하려면 앱에 탐색 흐름이 있어야합니다.
 이것은`react-navigation-shared-element` 모듈의`createSharedElementStackNavigator` 메소드에 의해 제공됩니다.
 `react-native-shared-element`에 대한 React Navigation 라이브러리가 포함되어 있습니다.
 이 방법을 사용하면 두 개의 개별 화면간에 요소를 공유하는 초기 프로세스 인 스택 탐색기를 만들 수 있습니다.
 각 경로를 공유 요소로 감싸고 경로 변경을 감지하여 전환을 트리거합니다.
 이 방법을 사용하여 탐색 흐름을 정의하는 프로세스는 React Navigation의 stack-navigator 모듈과 유사합니다.
 

`navigation /`이라는 새 디렉토리를 만들고 그 안에`RootNavigator.js`라는 새 파일을 만듭니다.
 다음 문을 가져 와서`createSharedElementStackNavigator` 메서드의`Stack`이라는 인스턴스를 만듭니다.
 그런 다음 루트 탐색기를 정의합니다.
 

```coffeescript
import * as React from 'react';
import { NavigationContainer } from '@react-navigation/native';
import { createSharedElementStackNavigator } from 'react-navigation-shared-element';
import HomeScreen from '../screens/HomeScreen';
import DetailScreen from '../screens/DetailScreen';
const Stack = createSharedElementStackNavigator();
export default function RootNavigator() {
  return (
    <NavigationContainer>
      <Stack.Navigator headerMode='none' initialRouteName='HomeScreen'>
        <Stack.Screen name='HomeScreen' component={HomeScreen} />
        <Stack.Screen name='DetailScreen' component={DetailScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

실제 동작을 보려면 아래와 같이`App.js` 파일을 수정하십시오.
 

```js
import React from 'react';
import RootNavigator from './navigation/RootNavigator';
export default function App() {
  return <RootNavigator />;
}
```

iOS 시뮬레이터에서이 단계 이후의 결과는 다음과 같습니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/addnavigationtoreact.png?resize=300%2C557&ssl=1)

## SharedElement 매핑
 

이미지 구성 요소는 홈 화면과 세부 화면 사이의 원활한 앞뒤 전환을 지원합니다.
 이 전환은 스크롤 그리드에서 상세 화면으로, 그리고 관련 이미지로 다시 발생해야합니다.
 이렇게하려면`Image` 구성 요소를`<SharedElement>`로 래핑하고`HomeScreen`에 고유 한`id`를 제공합니다.
 

또한`react-navigation-shared-element` 모듈에서`<SharedElement>`구성 요소를 가져와야합니다.
 

```js
import { SharedElement } from 'react-navigation-shared-element';
// Wrap the image component as
return (
  // ...
  <SharedElement id={`item.${item.id}.image_url`}>
    <Image
      style={
        borderRadius: 14,
        width: ITEM_WIDTH,
        height: ITEM_HEIGHT
      }
      source={ uri: item.image_url }
      resizeMode='cover'
    />
  </SharedElement>
);
```

`<SharedElement>`구성 요소는 두 화면 사이의 공유 ID 인`id`라는 소품을받습니다.
 래핑 된 자식은 전환이 발생하는 실제 구성 요소입니다.
 

공유 요소 전환을 활성화하려면`DetailScreen`에서 위의 프로세스를 따라야합니다.
 

```js
import { SharedElement } from 'react-navigation-shared-element';
// Wrap the image component as
return (
  // ...
  <SharedElement id={`item.${item.id}.image_url`}>
    <Image
      source={ uri: item.image_url }
      style={
        width: '100%',
        height: ITEM_HEIGHT,
        borderBottomLeftRadius: 20,
        borderBottomRightRadius: 20
      }
      resizeMode='cover'
    />
  </SharedElement>
);
```

홈 화면과 세부 정보 화면 사이의 전환에 애니메이션을 적용하려면`DetailScreen` 구성 요소에서`sharedElements` 구성을 정의하세요.
 이렇게하면 두 화면 사이의 `이미지`구성 요소 전환이 매핑됩니다.
 

`DetailScreen.js`의`export` 문 앞에 코드 스 니펫을 추가합니다.
 

```js
DetailScreen.sharedElements = route => {
  const { item } = route.params;
  return [
    {
      id: `item.${item.id}.image_url`,
      animation: 'move',
      resize: 'clip'
    }
  ];
};
```

위의 구성 개체는 두 화면간에 공유 된 고유 ID를 기반으로 화면간에 공유 된 요소에 대한 전환 효과를 트리거합니다.
 이것은`id`라는 속성을 정의하여 수행됩니다.
 

`animation`속성은 두 화면 사이를 탐색 할 때 애니메이션이 발생하는 방식을 결정합니다.
 예를 들어 위의 코드 스 니펫에서 `animation`에는 `move`라는 값이 있습니다.
 이 속성의 기본값이기도합니다.
 `fade`, `fade-in`및 `fade-out`과 같은 다른 값을 사용할 수 있습니다.
 `resize`속성은 요소의 모양과 크기를 수정해야할지 여부를 결정하는 동작입니다.
 예를 들어 위의 스 니펫에서 `clip`값은 텍스트 표시 효과와 유사한 전환 효과를 추가합니다.
 

이 단계 이후의 출력은 다음과 같습니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/reaectnavigation.gif?resize=304%2C636&ssl=1)

위의 예에서 전환이 발생하면 화면이 그 사이에서 왼쪽에서 오른쪽으로 슬라이드합니다.
 이 동작을 수정하여 공유 요소의 전환 효과를 적용하려면`DetailScreen`에`options` 구성 객체를 추가하겠습니다.
 루트 네비게이터 파일에서 다음 구성을 추가하십시오.
 

```js
const options = {
  headerBackTitleVisible: false,
  cardStyleInterpolator: ({ current: { progress } }) => {
    return {
      cardStyle: {
        opacity: progress
      }
    };
  }
};
// Then add it to the DetailScreen
return (
  <Stack.Screen
    name='DetailScreen'
    component={DetailScreen}
    options={() => options}
  />
);
```

`cardStyleInterpolator` 함수는 카드의 여러 부분에 대한 보간 스타일을 지정합니다.
 두 화면 사이를 이동할 때 전환을 사용자 정의 할 수 있습니다.
 현재 화면의 애니메이션 노드 진행률 값을 나타내는`current.progress`라는 속성 값을받습니다.
 이 값을`opacity` 속성에 적용하면 애니메이션 노드가 공유 요소 구성 개체에 정의 된 애니메이션 값으로 변경됩니다.
 `cardStyle`속성은 카드를 나타내는 뷰에 스타일을 적용합니다.
 

## SharedElements 매핑 업데이트
 

이전 데모에서 이미지 구성 요소의 전환이 매끄럽지 만 위치 핀 아이콘, 제목 및 두 화면 사이의 항목 설명과 같은 다른 구성 요소가 공유되지 않음을 알 수 있습니다.
 

이를 해결하기 위해`<SharedElement>`구성 요소를 사용하여 매핑 해 보겠습니다.
 먼저 홈 화면에서 다음 구성 요소를 수정합니다.
 

```xml
return (
  // Icon
  <SharedElement id={`item.${item.id}.iconName`}>
    <SimpleLineIcons size={40} color='white' name={item.iconName} />
  </SharedElement>
  //Title
  <SharedElement id={`item.${item.id}.title`}>
  <Text
    style={
      color: 'white',
      fontSize: 24,
      fontWeight: 'bold',
      lineHeight: 28
    }
  >
    {item.title}
  </Text>
</SharedElement>
  // Description
  <SharedElement id={`item.${item.id}.description`}>
  <Text
    style={
      color: 'white',
      fontSize: 16,
      fontWeight: 'bold',
      lineHeight: 18
    }
  >
    {item.description}
  </Text>
</SharedElement>
);
```

마찬가지로`DetailScreen.js` 파일에서 다음 요소를 수정합니다.
 

```xml
// Icon
<SharedElement id={`item.${item.id}.iconName`}>
  <SimpleLineIcons size={40} color='white' name={item.iconName} />
</SharedElement>
// Title
<SharedElement id={`item.${item.id}.title`}>
  <Text
    style={
      color: 'white',
      fontSize: 24,
      fontWeight: 'bold',
      lineHeight: 28
    }
  >
    {item.title}
  </Text>
</SharedElement>
// Description
<SharedElement id={`item.${item.id}.description`}>
  <Text
    style={
      color: 'white',
      fontSize: 16,
      fontWeight: 'bold',
      lineHeight: 18
    }
  >
    {item.description}
  </Text>
</SharedElement>
```

그런 다음 구성을 추가하십시오.
 

```js
DetailScreen.sharedElements = route => {
  const { item } = route.params;
  return [
    {
      id: `item.${item.id}.image_url`,
      animation: 'move',
      resize: 'clip'
    },
    {
      id: `item.${item.id}.title`,
      animation: 'fade',
      resize: 'clip'
    },
    {
      id: `item.${item.id}.description`,
      animation: 'fade',
      resize: 'clip'
    },
    {
      id: `item.${item.id}.iconName`,
      animation: 'move',
      resize: 'clip'
    }
  ];
};
```

이 단계 이후의 출력은 다음과 같습니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/sharedelementsmapping.gif?resize=304%2C636&ssl=1)

## 지연된 로딩
 

공유 요소 전환은 원활한 최종 사용자 경험을 지원하는 좋은 방법이지만 전환이 발생하기 전이나 후에로드되어야하는 요소를 처리 할 때 까다로울 수 있습니다.
 예를 들어 이전 데모에서 전환이 발생하기 전에 뒤로 버튼이 렌더링됩니다.
 동작을 제어하기 위해 React Native Animatable 라이브러리를 사용하여 애니메이션을 적용 해 보겠습니다.
 

`DetailScreen.js` 파일 내에서 가져옵니다.
 

```coffeescript
import * as Animatable from 'react-native-animatable';
```

닫기 버튼 아이콘은`<Animatable.View>`안에 래핑됩니다.
 이 구성 요소에는 애니메이션을 지연시키는`delay`라는 소품이 있습니다.
 `duration`이라는 소품을 사용하여 애니메이션이 실행되는 시간을 제어 할 수 있습니다.
 이 두 소품의 값은 밀리 초 단위로 제공됩니다.
 `ref` 값을 사용하면`fadeOut` 애니메이션이 아이콘에 적용됩니다.
 이 애니메이션 메서드는 비동기식이므로 애니메이션이 성공적으로 실행 된 후 프로 미스를 사용하여 홈 화면으로 돌아갈 수 있습니다.
 이 애니메이션 메서드에 전달되는 인수는 밀리 초 단위입니다.
 

```xml
const DetailScreen = ({ navigation, route }) => {
  const buttonRef = React.useRef();
  return (
    <Animatable.View
      ref={buttonRef}
      animation='fadeIn'
      duration={600}
      delay={300}
      style={[StyleSheet.absoluteFillObject]}
    >
      <MaterialCommunityIcons
        name='close'
        size={28}
        color='#fff'
        style={
          position: 'absolute',
          top: 40,
          right: 20,
          zIndex: 2
        }
        onPress={() => {
          buttonRef.current.fadeOut(100).then(() => {
            navigation.goBack();
          });
        }
      />
    </Animatable.View>
  );
};
```

최종 출력은 다음과 같습니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/reactnavigationexample.gif?resize=304%2C636&ssl=1)

## 결론
 

이 튜토리얼을 재미있게 읽으 셨기를 바랍니다.
 React Navigation 공유 요소 모듈을 사용하여 React Native의 화면간에 요소를 공유하면 개발 프로세스와 최종 사용자 경험이 모두 원활 해집니다.
 자세한 내용은 여기에서 공식 문서를 확인하는 것이 좋습니다.
 소스 코드는이 GitHub 저장소에서 사용할 수 있습니다.
 