---
layout: post
title: "Magnus UI 튜토리얼 : React Native UI 컴포넌트 빌드
 "
author: 'Code Tower'
thumbnail: url("https://blog.logrocket.com/wp-content/uploads/2021/01/Screen-Shot-2021-01-28-at-3.33.30-PM.png")
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Screen-Shot-2021-01-28-at-3.33.30-PM.png?fit=730%2C482&ssl=1)

뛰어난 UI와 UX를 갖는 것은 모든 앱의 성공에 가장 중요한 요소 중 하나입니다. 특히 거의 모든 앱에 사용할 수있는 대안이 너무 많을 때 그렇습니다.
 앱이 원하는대로 보이고 작동하려면 시간과 인내와 연습이 필요합니다.
 

하지만 앱을 만들 때 디자이너와 UX 개발자의 도움이 없다면 아이디어를 실현하는 데 대부분의 시간을 할애하게 될 것입니다.
 UI를 예쁘게 보이게 만드는 것이 아마도 당신의 걱정거리가 될 것입니다. 특히 솔로 개발자라면 더욱 그렇습니다.
 

이것은 Magnus UI와 같은 도구가 들어오는 곳입니다. 소수의 구성 요소와 직관적 인 유틸리티 소품을 사용하면 멋지게 보이고 느껴지는 React Native 앱용 UI 구성 요소를 매우 빠르고 쉽게 구축 할 수 있습니다.
 

유틸리티 우선 스타일 라이브러리는 블록에서 가장 인기있는 새로운 것입니다. 글쎄요, 한동안 사용되어 왔지만 최근에는 상당히 빠르게 관심을 받고 있습니다.
 Magnus UI는 이러한 추세를 따르고 있습니다.
 그 모든 과대 광고가 무엇인지 알고 싶다면 이것은 당신을위한 게시물입니다.
 

이 블로그 게시물에서는 Magnus UI를 사용하여 두 개의 완전한 앱 화면을 구축하는 방법을 안내합니다.
 나는 디자이너와 가까운 사람이 아니므로 Dribbble에서 무언가를 선택하는 데 의지 할 것입니다.
 이것이 어떻게 보이는지 :
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Dribble-Design-Mobile-Template.png?resize=730%2C548&ssl=1)

이 디자인을 선택하는 이유는 거의 모든 앱에있는 가장 일반적인 UI 블록 몇 가지가 포함되어 있기 때문입니다.
 

- 각각의 이미지와 일부 텍스트 콘텐츠의 목록입니다.
 이 경우 코스 카테고리 목록입니다.
 
- 여러 페이지를 탐색 할 수있는 하단 탐색
 
- 각 카테고리에 코스를 보여주는 자체 화면과 화면에 대한 작업 버튼이있는 마스터 세부 패턴
 
- 사용자 프로필, 검색 창, 통계 표시 등과 같은 작은 세부 정보
 

물론 Dribbble 모형에는 항상 최종 사용자에게 가치를 추가하지 않고 디자인을 돋보이게하는 역할 만하는 추가 비트가 포함되어 있습니다.
 따라서 우리는이 게시물의 내용을 관련성 있고 적절하게 유지하기 위해 약간 잘라낼 것입니다.
 

자, 이제 시작하겠습니다!
 아, 그러기 전에 전체 내용을 읽기 전에 최종 결과가 어떻게 보이는지 살펴보고 싶다면 여기에 빠른 미리보기가 있고 GitHub의 전체 코드베이스가 있습니다.
 

## 툴킷
 

우선, 저는 TypeScript를 좋아합니다. 일단 사용하기 시작하면 JavaScript로 돌아가는 것이 옳지 않은 것 같습니다.
 따라서이 프로젝트에 TypeScript를 사용할 것이지만 원하는 경우 JS 버전을 고수 할 수 있습니다.
 

React Native 프로젝트를 시작하고 관리하는 것은 약간 지루할 수 있기 때문에 제한 사항이 있더라도 항상 Expo를 선호합니다.
 운 좋게도 두 개의 화면으로 된 앱을 구축하는 것을 보여주기 위해 우리는 이러한 한계에 부딪히지 않을 것입니다.
 

코스 카테고리와 세부 정보 페이지 사이의 마스터 세부 정보 탐색을 위해 react-navigation 패키지를 선택합니다.
 또한 하단 내비게이션 바, 상단 헤더 영역 등과 같은 복잡한 것들을 빠르게 스캐 폴드하는 데 도움이 될 것입니다.
 

물론, 쇼의 메인 스타 인 Magnus UI가 목록에 포함되어야합니다.
 디자인에는 별도로 구할 수없는 일러스트레이션이 몇 개 있기 때문에 프로젝트에 맞는 멋진 일러스트레이션을 빠르게 찾을 수있는 절대적으로 뛰어난 웹 사이트 인 unDraw의 일러스트레이션으로 대체하겠습니다.
 

독자 여러분이 시작하기 전에 이러한 도구를 익힐 수있는 기회를 제공하기 위해이 모든 것을 나열하고 있습니다. 그러나 필요에 따라 이러한 도구의 사용법을 모두 문서화하기 위해 최선을 다할 것이기 때문에 필수는 아닙니다.
 

## React Native 앱 스캐 폴딩
 

React Native 또는 Expo CLI에 익숙하지 않은 경우 둘 다 프로젝트를 시작할 준비가 된 매우 편리한 상용구가 있습니다.
 Expo를 사용하고 있으므로 우리의 요구에 가장 적합한 것은 탭이있는 템플릿입니다.
 

상용구를 얻으려면 터미널에서`expo init elarn` 명령을 실행하고 프롬프트에서 템플릿을 선택합니다.
 완료되면`cd elarn`을 사용하여 새로 생성 된 폴더로 이동합니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Boilerplate-CCommand-Expo-Init-Elarn.gif?resize=730%2C326&ssl=1)

상용구에는 이미 많은 양의 코드가 있지만 자세히 살펴보기 전에 npm에서 Magnus UI를 설치하겠습니다.
 여기에서 설치 가이드를 따를 수 있지만 다음을 실행해야합니다.
 

```java
yarn add react-native-magnus color react-native-modal react-native-animatable -S
```

좋습니다. 이제 상용구 코드가있는 앱이 어떻게 보이는지 살펴 보겠습니다.
 `yarn start`를 실행하여 Expo 서비스를 시작합니다.
 서비스가 실행되면 Expo 클라이언트를 사용하여 선택한 물리적 장치 (iOS / Android)에서 앱을 실행하거나 에뮬레이터 / 시뮬레이터에서 실행할 수 있습니다.
 iOS 용 Simulator를 사용하겠습니다.
 이러한 문제가있는 경우 Expo 문서를 따르십시오.
 앱이 실행되면 아래와 같은 화면이 나타납니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/App-Run-Outputin-iOS.png?resize=730%2C1578&ssl=1)

보시다시피, 우리가 선택한 놀라운 Dribbble 디자인처럼 보이도록하기 위해해야 할 일이 상당히 많으니 시작하겠습니다!
 

## 하단 탭 스타일링
 

Expo 탭 템플릿은 두 개의 탭 화면을 포함하여 구현 된 반응 탐색과 함께 기본 제공됩니다.
 그러나 탭은 장치 OS에 따라 기본 기본 스타일을 따릅니다.
 좀 더 디자인처럼 보이게하려면 약간 구성해야합니다.
 

react-navigation과 Expo 상용구가 이미 대부분을 구축했기 때문에 Magnus UI를 너무 많이 사용하지 않도록 노력할 것입니다.
 대신 일부 원시 React Native 구성 및 스타일을 사용하여 디자인처럼 보이도록 할 것입니다.
 

상용구에서는 두 개의 탭만 제공되지만 디자인에는 네 개의 탭이 있습니다.
 이제 탭을 몇 개 더 추가하고 탭이 디자인처럼 보이도록 아이콘을 추가하겠습니다.
 @ expo / vector-icons 패키지를 통해 다양한 아이콘을 사용할 수 있으며 탭 아이콘의 경우 Ionicons 아이콘 세트를 사용하고 있습니다.
 

먼저`navigation / BottomTabNavigator.tsx` 파일을 열고`BottomNavigator` 구성 요소를 다음으로 바꿉니다.
 

```xml
   <BottomTab.Navigator
      initialRouteName="TabOne"
      tabBarOptions={{
          showLabel: false,
          activeTintColor: Colors[colorScheme].tint,
          style: {
              marginLeft: 50,
              marginRight: 50,
              marginBottom: 30,
              borderRadius: 35,
              paddingBottom: 10,
              borderTopWidth: 0,
              position: 'absolute',
              paddingHorizontal: 20,
              backgroundColor: Colors[colorScheme].tabBarBackground,
          }
      }}>
      <BottomTab.Screen
        name="TabOne"
        component={TabOneNavigator}
        options={{
          tabBarIcon: ({ color }) => <TabBarIcon name="ios-home" color={color} />,
        }}
      />
      <BottomTab.Screen
        name="TabTwo"
        component={TabTwoNavigator}
        options={{
          tabBarIcon: ({ color }) => <TabBarIcon name="ios-folder-outline" color={color} />,
        }}
      />
      <BottomTab.Screen
        name="TabThree"
        component={TabTwoNavigator}
        options={{
          tabBarIcon: ({ color }) => <TabBarIcon name="ios-chatbox-outline" color={color} />,
        }}
      />
      <BottomTab.Screen
        name="TabFour"
        component={TabTwoNavigator}
        options={{
          tabBarIcon: ({ color }) => <TabBarIcon name="ios-cog" color={color} />,
        }}
      />
    </BottomTab.Navigator>
```

여기에 추가 된 사항은 다음과 같습니다.
 

- `BottomTab.Navigator` 구성 요소의`tabBarOptions` 소품에있는`style` 및`showLabel` 필드
 이것은 디자인처럼 보이도록 몇 가지 사용자 정의 스타일을 추가합니다.
 
- 기존 화면만을 가리키는`TabThree` 및`TabFour` 탭, 본질적으로`TabTwo`를 복제하지만 다른 아이콘을 사용합니다.
 

이제 이러한 아이콘이 디자인에 비해 너무 크게 보이므로 동일한 파일에서`TabBarIcon` 구성 요소를 찾아`size` 소품을 25로 변경하십시오. 새 크기로 간격을 조정하려면`marginBottom`을 변경하십시오.
 -5 :
 

```js
 return <Ionicons size={25} style={{ marginBottom: -5 }} {...props} />;
```

위의 변경으로 인해 TypeScript는 유형을 올바르게 정의하지 않고 두 개의 새 탭을 추가했기 때문에 약간의 불만이 있습니다.
 `types.tsx` 파일을 열고`BottomTabParamList` 정의에 아래와 같이`TabThree` 및`TabFour`를 추가합니다.
 

```coffeescript
export type BottomTabParamList = {
  TabOne: undefined;
  TabTwo: undefined;
  TabThree: undefined;
  TabFour: undefined;
};
```

앱의 현재 상태를보고 있다면 디자인에 비해 색상이 화면에서 어색해 보인다는 사실을 깨달았을 것이므로 수정하겠습니다.
 `const tintColorLight = `# D84343`;`을 설정하여`constants / Colors.ts`에서 탭의 색조 색상을 변경 한 다음`background : `# EFE3E3``를 설정하여 탭 모음 배경을 변경합니다.
 

이 상용구는 어두운 모드와 밝은 모드를 모두 지원하므로 어두운 모드에 대해서도 약간의 디자인을 조정 해 보겠습니다.
 `tabBarOptions`속성의 스타일 속성에서이 속성을 사용했기 때문에 밝고 어두운 스키마에 새 속성`tabBarBackground : `# fff``를 추가합니다.
 

참고로 Dribbble 디자인에서 직접이 16 진수 값을 추출 했으므로 기대치 나 선호도와 일치하지 않는 경우 적절하다고 판단되는대로 자유롭게 조정할 수 있습니다.
 

위의 변경이 완료되면 화면이 다음과 같이 보일 것입니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Boilerplate-Adjustment-for-Dark-Mode.png?resize=730%2C1578&ssl=1)

## React Native UI를 빌드하기위한 Magnus UI 구성
 

마지막으로 Magnus UI로 페이지 콘텐츠를 만드는 데 집중합니다.
 스타일링에 모든 Magnus UI 유틸리티 소품을 사용하려면 몇 가지 기본 세부 사항으로 구성해야합니다.
 먼저`App.tsx` 파일에서 Magnus`ThemeProvider`를 가져 와서 기본 색상으로 사용자 정의 테마를 정의 해 보겠습니다.
 

```js
import { ThemeProvider } from "react-native-magnus";

const ElarnTheme = {
  colors: {
    pink900: '#D84343',
  }
}
```

이러한 색상 정의는 앱 전체에서 더 기억에 남고 쉽게 식별 할 수있는 이름으로 사용할 수있는 색상 변형의 하위 집합을 만드는 데 도움이됩니다.
 이제해야 할 일은 전체 앱을`ThemeProvider`로 감싸고`theme` 객체를 제공자에게 전달하는 것입니다.
 

```xml
       <ThemeProvider theme={ElarnTheme}>
          <Navigation colorScheme={colorScheme} />
          <StatusBar />
        </ThemeProvider>
```

이제 위에서부터 홈 화면을 만들어 보겠습니다.
 가장 먼저 눈에 띄는 것은 `Tab One Title`이라는 헤더가 아픈 엄지처럼 튀어 나온 것입니다.
 이를 없애려면`BottomTabNavigator.tsx`로 돌아가서 새 prop을`TabOneScreen` 정의에 전달해야합니다.
 

```xml
  <TabOneStack.Screen
        name="TabOneScreen"
        component={TabOneScreen}
        options={{
            headerShown: false
        }}
      />
```

분명히`headerShown : false`는 헤더를 숨기는 것입니다.
 

## 1 단계 : 홈 화면 UI 구축
 

아시다시피 파일 이름, 그 안에있는 코드 등은 `TabOne`, `TabTwo`등을 사용하여 일반적인 방식으로 작성됩니다. 원하는 경우 이름을 자유롭게 변경하되 그대로 유지하겠습니다.
 그대로.
 `screens / TabOne.tsx`로 시작하여 모든 스타일 정의를 제거한 후 다음 코드를 배치합니다.
 

```coffeescript
import * as React from 'react';
import Constants from "expo-constants";
import {Avatar, Div, Icon, Input, Text} from "react-native-magnus";

import { categories, CategoryCard } from '../components/CategoryCard';
import {ScrollView} from "react-native";

export default function TabOneScreen() {
  return (
      <ScrollView>
        <Div px={25}>
            <Div mt={Constants.statusBarHeight} row justifyContent={"space-between"} alignItems="center">
                <Div>
                    <Div row>
                        <Text fontSize="5xl" mr={5}>Hi</Text>
                        <Text fontSize="5xl" fontWeight="bold">Sheila</Text>
                    </Div>
                    <Text fontSize="lg">Let's upgrade your skill</Text>
                </Div>
                <Avatar shadow={1} source={{uri: 'https://i.pravatar.cc/300'}}/>
            </Div>
            <Div my={30}>
                <Input
                    py={15}
                    px={25}
                    bg="white"
                    rounded={30}
                    placeholder="Search Class"
                    prefix={<Icon name="search" color="gray900" fontFamily="Feather" />}
                />
            </Div>
            <Div row pb={10} justifyContent="space-between" alignItems="center">
            <Text fontWeight="bold" fontSize="3xl">Popular</Text>
            <Text fontWeight="bold" fontSize="lg">See all</Text>
        </Div>
            <Div row flexWrap="wrap" justifyContent="space-between">
                {categories.map((category) => (
                    <CategoryCard {...category} />
                ))}
            </Div>
        </Div>
      </ScrollView>
  );
}
```

이것을 분해 해 보겠습니다.
 코스 카테고리 목록을 스크롤 할 수 있도록 전체 페이지를`ScrollView 구성 요소`로 래핑하고 있습니다.
 그런 다음`px = {25}`소품이있는`Div`라는 또 다른 래퍼가 있습니다.
 

`Div`는 React Native의 기본`View` 구성 요소에 대한 Magnus UI 대안입니다.
 스타일링을 위해 모든 유틸리티 소품을 사용할 수 있으며`px`는 이러한 소품 중 하나입니다.
 컨테이너 주위에 수평 패딩을 설정합니다.
 

헤더 영역을 제거했기 때문에 탭 화면 내의 콘텐츠가 장치의 상태 표시 줄 영역과 겹치기 시작합니다.
 이를 방지하기 위해 div에서 상단 영역 콘텐츠를 래핑하고 expo-constants 라이브러리에서`Constants.statusBarHeight`를 사용하여 추출 할 수있는 기기의 상태 표시 줄과 동일한 상단 여백을 추가합니다.
 

`row justifyContent = { "space-between"}`덕분에이 구성 요소의 직계 자식은 같은 행에 차례로 배치되고 컨테이너의 수평 가장자리로 밀려서 수직으로 정렬됩니다.
 컨테이너의 중심.
 

그 안에는 사용자를 맞이하기 위해 모든 텍스트 내용을 감싸는`Div`가 있습니다.
 텍스트 스타일을 지정하기 위해`fontSize` 및`fontWeight` 소품을 사용하고 있습니다.
 `fontSize`의 값은 실제 크기 대신 `sm`, `lg`, `xl`등으로 정의되어 앱 전체에서 일관성을 보장합니다.
 

오른쪽에 프로필 사진을 표시하기 위해 Magnus UI의 `Avatar`구성 요소를 사용하고 아바타 서비스의 URL을 전달합니다.
 이 구성 요소가 이미지를 원 안에 얼마나 멋지게 배치하고`shadow = {1}`소품을 전달하여 그림자를 추가하는 것이 얼마나 쉬운 지 확인하십시오.
 

다음으로 Magnus UI의 또 다른 구성 요소 인 검색 입력 필드가 있습니다.
 입력 필드의 테두리에 30px 반경을 제공하는`rounded = {30}`과 같은 편리한 소품으로 쉽게 스타일을 지정할 수 있습니다.
 `color = "gray900"`은 기본 테마 정의에서`gray900` 색상을 가져 오는 또 다른 흥미로운 소품입니다.
 

이러한 색상은`ThemeProvider`에 전달 된 사용자 정의 테마에서 간단히 정의하여 재정의 할 수 있습니다.
 다른 소품은 우리가 지금까지 보았던 것들과 비슷합니다.
 

이제 코스 카테고리 목록에 대한 데이터가 필요합니다.
 이상적으로는 실제 앱에서 이들은 어딘가의 서버에서 제공됩니다.
 지금은 하드 코딩 만하겠습니다.
 

코드를 깔끔하게 유지하기 위해 카테고리 디스플레이를 `CategoryCard`라는 자체 컴포넌트로 이동하고 카테고리 데이터를 모든 카테고리에 매핑하는 동안 전달합니다.
 둘 다 아직 존재하지 않는`components / CategoryCard.tsx`에서 가져오고 있습니다.
 이제 해당 파일을 만들고 내부에 다음 코드를 넣습니다.
 

```js
import * as React from "react";
import {Div, Image, Text} from "react-native-magnus";
import {ImageSourcePropType} from "react-native";

type Category = {
    count: number;
    category: string;
    picture: ImageSourcePropType;
}

export const categories = [
    {category: 'Marketing', count: 12, picture: require('../assets/images/illustration_one.png')},
    {category: 'Investing', count: 8, picture: require('../assets/images/illustration_two.png')},
    {category: 'Drawing', count: 22, picture: require('../assets/images/illustration_three.png')},
    {category: 'Marketing', count: 12, picture: require('../assets/images/illustration_one.png')},
    {category: 'Investing', count: 8, picture: require('../assets/images/illustration_two.png')},
    {category: 'Drawing', count: 22, picture: require('../assets/images/illustration_three.png')},
    {category: 'Drawing', count: 22, picture: require('../assets/images/illustration_three.png')},
];

export const CategoryCard = ({category, count, picture}: Category) => (
    <Div rounded="lg" bg="white" mb={10} w="48%">
        <Image source={picture} h={120} roundedTop="lg" />
        <Div p={10}>
            <Text fontWeight="bold" fontSize="xl">{category}</Text>
            <Text>{count} Courses</Text>
        </Div>
    </Div>
);
```

구성 요소 자체는 매우 간단합니다.
 둥근 테두리와 흰색 배경의 `Div`가 있습니다.
 각 카드는 사용 가능한 너비의 48 %를 차지하므로 모든 행은 중간에 약간의 간격을두고 두 카드에만 맞습니다.
 이미지는 상단에 표시되고 일부 카테고리 및 코스는 카드 하단에 표시됩니다.
 

또한 화면을 채우기 위해 배열에 가짜 카테고리 데이터를 정의하고 있습니다.
 각 카테고리의`picture` 소품은 내가 unDraw에서 다운로드 한 3 개의 일러스트레이션을 넣은`assets / images` 디렉토리의 로컬 파일을 가리 킵니다.
 이 시점에서 홈 화면에 다음과 같은 화면이 표시됩니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Home-Screen-with-Uploaded-Images.png?resize=730%2C1578&ssl=1)

## 2 단계 : 세부 정보 페이지 UI 빌드
 

마스터-디테일 패턴에서는 일반적으로 항목 목록이 있으며 항목 중 하나를 클릭하면 항목에 대한 자세한 내용이있는 다른 화면으로 이동합니다.
 모든 카테고리에 대한 세부 정보 화면을 만들기 위해`screens / CourseDetail.tsx`라는 새 파일을 만들고 내부에 다음 코드를 넣습니다.
 

```xml
import * as React from 'react';
import {ScrollView} from "react-native";
import { useHeaderHeight } from '@react-navigation/stack';
import {Div, Icon, Image, Text} from "react-native-magnus";
import {StackScreenProps} from "@react-navigation/stack";
import {ListHeader} from "../components/ListHeader";
import {CourseVideo} from "../components/CourseVideo";
import {Category, TabOneParamList} from "../types";

export default function CourseDetailScreen({route}: StackScreenProps<TabOneParamList, 'CourseDetailScreen'>) {
    const { category }: { category: Category } = route.params;
    const headerHeight = useHeaderHeight();
    return (
        <ScrollView style={{marginTop: headerHeight}}>
            <Div px={25}>
                <Div row mt={15} mb={15} justifyContent="space-between">
                    <Div pb={50}>
                        <Text fontSize="4xl" fontWeight="bold">{ category.category }</Text>
                        <Div row>
                            <Text>by </Text>
                            <Text fontWeight="bold">{category.author}</Text>
                        </Div>
                        <Div row mt={15}>
                            <Div pr={30} row>
                                <Icon fontFamily="Ionicons" fontSize={20} name='people' color="pink500" />
                                <Text fontSize="lg" ml={5} fontWeight="bold">{ category.subscriberCount }</Text>
                            </Div>
                            <Div row>
                                <Icon fontFamily="Ionicons" fontSize={20} name='star' color="pink500" />
                                <Text fontSize="lg" ml={5} fontWeight="bold">{ category.rating }</Text>
                            </Div>
                        </Div>
                        <Div row mt={15}>
                            <Icon name="time-outline" fontFamily="Ionicons" fontSize={20} color="pink500"  />
                            <Text fontSize="lg" ml={5}>{category.duration}</Text>
                        </Div>
                    </Div>
                    <Image source={category.picture} w="50%" />
                </Div>
                <ListHeader title="Course Content" />
                <Div mt={15}>
                    {category.videos.map(video => (
                        <CourseVideo {...video} />
                    ))}
                </Div>
            </Div>
        </ScrollView>
    );
}
```

이 화면에는 사용자가 선택한 카테고리를 기반으로하는 동적 콘텐츠가 있으므로 카테고리를 경로 매개 변수로 수락해야합니다.
 경로 매개 변수는 화면 사이를 탐색 할 때 한 화면에서 다른 화면으로 데이터를 전달하는 데 사용됩니다.
 

우리의 경우 선택한 카테고리 데이터를이 화면에 전달하려고합니다.
 데이터 전달을 용이하게하려면`TabOneParamList`를 통해이 구성 요소를 올바르게 입력해야하므로`types.tsx` 파일을 열고 아래와 같이 업데이트해야합니다.
 

```bash
export type TabOneParamList = {
  TabOneScreen: undefined;  
  CourseDetailScreen: { category: Category};
};
```

이제이 파일에있는 동안 카테고리의 세부 정보 페이지에는 평가, 구독자, 코스 동영상 목록 등 모든 카테고리의 홈 화면에 표시된 것보다 훨씬 많은 데이터가 있습니다.
 

따라서 카테고리 데이터를 업데이트해야합니다. 원시 데이터이기 때문에 카드 구성 요소 내부에이 데이터가 있으면 안됩니다.
 이제`components / CategoryCard.tsx`에서 제거하고`types.tsx` 파일에 아래 코드를 추가해 보겠습니다.
 

```js
export const categories = [
  {
    category: 'Marketing',
    count: 12,
    picture: require('./assets/images/illustration_one.png'),
    author: 'Jack Mi',
    subscriberCount: '11K',
    rating: 4.8,
    duration: '2 hours 30 minutes',
    bg: 'pink500',
    videos: [
      {title: '01. Introduction', duration: '03:53'},
      {title: '02. Whats investing', duration: '08:13'},
      {title: '03. Fundamentals', duration: '15:23'},
      {title: '04. Lessons', duration: '20:33'},
    ]
  },
  {
    category: 'Investing',
    count: 8,
    picture: require('./assets/images/illustration_two.png'),
    author: 'Jack Mi',
    subscriberCount: '11K',
    rating: 4.8,
    duration: '2 hours 30 minutes',
    bg: 'purple500',
    videos: [
      {title: '01. Introduction', duration: '03:53'},
      {title: '02. Whats investing', duration: '08:13'},
      {title: '03. Fundamentals', duration: '15:23'},
      {title: '04. Lessons', duration: '20:33'},
    ]
  },
  {
    category: 'Drawing',
    count: 22,
    picture: require('./assets/images/illustration_three.png'),
    author: 'Jack Mi',
    subscriberCount: '11K',
    rating: 4.8,
    duration: '2 hours 30 minutes',
    bg: 'red500',
    videos: [
      {title: '01. Introduction', duration: '03:53'},
      {title: '02. Whats investing', duration: '08:13'},
      {title: '03. Fundamentals', duration: '15:23'},
      {title: '04. Lessons', duration: '20:33'},
    ]
  },
  {
    category: 'Marketing',
    count: 12,
    picture: require('./assets/images/illustration_one.png'),
    author: 'Jack Mi',
    subscriberCount: '11K',
    rating: 4.8,
    duration: '2 hours 30 minutes',
    bg: 'blue500',
    videos: [
      {title: '01. Introduction', duration: '03:53'},
      {title: '02. Whats investing', duration: '08:13'},
      {title: '03. Fundamentals', duration: '15:23'},
      {title: '04. Lessons', duration: '20:33'},
    ]
  },
  {
    category: 'Investing',
    count: 8,
    picture: require('./assets/images/illustration_two.png'),
    author: 'Jack Mi',
    subscriberCount: '11K',
    rating: 4.8,
    duration: '2 hours 30 minutes',
    bg: 'pink500',
    videos: [
      {title: '01. Introduction', duration: '03:53'},
      {title: '02. Whats investing', duration: '08:13'},
      {title: '03. Fundamentals', duration: '15:23'},
      {title: '04. Lessons', duration: '20:33'},
    ]
  },
  {
    category: 'Drawing',
    count: 22,
    picture: require('./assets/images/illustration_three.png'),
    author: 'Jack Mi',
    subscriberCount: '11K',
    rating: 4.8,
    duration: '2 hours 30 minutes',
    bg: 'pink500',
    videos: [
      {title: '01. Introduction', duration: '03:53'},
      {title: '02. Whats investing', duration: '08:13'},
      {title: '03. Fundamentals', duration: '15:23'},
      {title: '04. Lessons', duration: '20:33'},
    ]
  },
  {
    category: 'Drawing',
    count: 22,
    picture: require('./assets/images/illustration_three.png'),
    author: 'Jack Mi',
    subscriberCount: '11K',
    rating: 4.8,
    duration: '2 hours 30 minutes',
    bg: 'pink500',
    videos: [
      {title: '01. Introduction', duration: '03:53'},
      {title: '02. Whats investing', duration: '08:13'},
      {title: '03. Fundamentals', duration: '15:23'},
      {title: '04. Lessons', duration: '20:33'},
    ]
  },
];
```

보시다시피, 비디오 배열을 포함하여 각 카테고리에 몇 가지 새로운 속성이 있습니다.
 즉, `Category`에 대한 유형 정의를 조정해야합니다.
 `components / CategoryCard.tsx`에서 유형 정의를 제거하고이 코드를`types.tsx` 파일에 배치하겠습니다.
 

```cpp
import {ImageSourcePropType} from "react-native";
export type Video = {
  title: string;
  duration: string;
};

export type Category = {
  bg: string;
  count: number;
  author: string;
  rating: number;
  category: string;
  duration: string;
  videos: Video[];
  subscriberCount: string;
  picture: ImageSourcePropType;
}
```

좋습니다. 이제 다시 화면으로 돌아가겠습니다.
 홈페이지와 유사하게 왼쪽에 몇 가지 세부 정보가있는 상단 영역이 있습니다.
 카테고리의 이미지는 오른쪽에 있으므로 플렉스 래퍼를 사용하여이를 포함합니다.
 

`subscriberCount` 아이콘은`color = "pink500"`을 사용합니다.
 디자인과 일치 시키려면`App.tsx`의 테마 수정자를 통해 기본값을 덮어 써야합니다.
 

```undefined
const ElarnTheme = {
  colors: {
    pink900: '#D84343',
    pink500: '#D59999'
  }
}
```

그런 다음, 코스의 모든 비디오를 매핑하고`CourseVideo` 구성 요소를 렌더링합니다.
 `components / CourseVideo.tsx`라는 새 파일을 만들고 여기에 다음 코드를 입력 해 보겠습니다.
 

```js
import {Button, Div, Icon, Text} from "react-native-magnus";
import React from "react";

export const CourseVideo = ({title, duration}: {title: string, duration: string}) => (
    <Button block bg="white" mb={15} py={15} px={15} rounded="circle" shadow="xl">
        <Div row flex={1} alignItems="center" pr={10}>
            <Div bg="pink900" rounded="circle" p={10} mr={10}>
                <Icon name="play" fontFamily="Ionicons" color="white" fontSize="3xl" />
            </Div>
            <Div flex={2} row justifyContent="space-between">
                <Text fontWeight="bold" fontSize="lg">{ title }</Text>
                <Text fontWeight="bold" fontSize="lg">{duration}</Text>
            </Div>
        </Div>
    </Button>
);
```

이 화면에서 마지막으로 주목하고 싶은 것은 `ListHeader`구성 요소를 사용하여 동영상 목록 헤더를 표시한다는 것입니다.
 이 헤더는 홈 화면에있는 목록 헤더와 정확히 동일하므로 자체 구성 요소로 추상화하는 것이 좋습니다.
 

새 파일`components / ListHeader.tsx`를 만들고 파일에 다음 코드를 입력 해 보겠습니다.
 

```js
import * as React from "react";
import {Div, Text} from "react-native-magnus";

export const ListHeader = ({title}: {title: string}) => (
    <Div row pb={10} justifyContent="space-between" alignItems="center">
        <Text fontWeight="bold" fontSize="3xl">{title}</Text>
        <Text fontWeight="bold" fontSize="lg">See all</Text>
    </Div>
);
```

단순히 홈 화면에서 헤더 내용을 복사하고 소품을 통해 제목을 동적으로 만들었습니다.
 이제 홈 화면의 헤더를`<ListHeader title = "Popular"/>`로 바꿀 수 있습니다.
 

이제 사용자가 카테고리를 선택하고 세부 정보 화면을 열 수 있도록 먼저 모든 카테고리 카드를 버튼으로 전환하고 `onPress`이벤트 핸들러를 첨부해야합니다.
 `components / CategoryCard.tsx` 파일을 열고`Div` 래퍼를 다음으로 바꿉니다.
 

```js
export const CategoryCard = ({category, count, picture, bg, onPress}: CategoryCardProp) => (
     <Button
        block
        w="48%"
        mb={10}
        p="none"
        bg="white"
        rounded="lg"
        onPress={onPress}>
```

`onPress`는 소품으로 예상되므로 홈 화면으로 돌아가서이 `onPress`함수를 모든 카테고리에 전달해야합니다.
 react-navigation의 모든 화면은 화면 사이를 이동하는 데 사용할 수있는`navigation` 소품을받습니다.
 `screens / TabOneScreen.tsx`를 열고 다음과 같이 구성 요소 정의를 조정합니다.
 

```js
export default function TabOneScreen({ navigation }: StackScreenProps<any>) {
```

그런 다음`onPress` 소품을 아래와 같이`CategoryCard` 구성 요소에 전달합니다.
 

```undefined
                    <CategoryCard
                        {...category}
                        onPress={() => navigation.navigate('TabOne',{
                            screen: 'CourseDetailScreen',
                            params: {category}
                        })}

                    />
```

따라서 카테고리 중 하나를 누르면`TabOne` 네비게이터 내`CourseDetailScreen`으로 이동하여`category` 정보를 param으로 화면에 전달합니다.
 이 탐색이 작동하려면`navigation / BottomTabNavigator.tsx`에 아래 탭 정의를 추가하여 탐색기에서 새로 만든 화면을 등록해야합니다.
 

```js
      <TabOneStack.Screen
          name="CourseDetailScreen"
          component={CourseDetailScreen}
          options={{
              headerTransparent: true,
              headerBackTitleVisible: false,
              headerTitle: '',
              headerLeft: props =>
                  <Button bg="transparent" p="none" {...props} ml={20}>
                      <Ionicons name="arrow-back" size={30} />
                  </Button>
          }}
      />
      </TabOneStack.Navigator>
```

이 탭의 경우 헤더는 유지하지만 디자인과 일치하는 아이콘으로 내용을 바꿉니다.
 아이콘 자체는 Magnus UI의 `Button`구성 요소를 사용하여 렌더링되며 뒤로 화살표 아이콘 만 포함합니다.
 

지금까지 모든 단계를 수행했다면 홈 화면에서 카테고리 항목을 클릭 할 수 있고 세부 정보 페이지가 열립니다.
 

그러나 상세 페이지에는 홈 화면과 동일한 하단 탭이있는 반면, 디자인에서 상세 페이지에는 홈 화면의 하단 탭이 차지하는 동일한 영역에 하나의 작업 버튼 만 있습니다.
 이것은 까다로운 부분이며 네비게이터의 일부 재배 선이 필요합니다.
 

## 마지막 단계 : 상세 화면의 하단 탭 교체
 

반응 탐색의 작동 방식으로 인해 탭 탐색기의 중첩 된 화면에서 하단 탭을 숨길 수 없습니다.
 따라서 세부 정보 화면에서 하단 탭을 숨기려면 탭 탐색기 밖으로 이동하여 기본 스택 탐색기에 넣어야합니다.
 `navigation / index.tsx`를 열고 아래와 같이 새 화면을 추가합니다.
 

```xml
      <Stack.Screen name="Root" component={BottomTabNavigator} />
      <Stack.Screen
        name="CourseDetailScreen"
        component={CourseDetailScreen}
        options={{
            headerShown: true,
            headerTransparent: true,
            headerBackTitleVisible: false,
            headerTitle: '',
            headerLeft: props =>
                <Button bg="transparent" p="none" {...props} ml={20}>
                    <Ionicons name="arrow-back" size={30} />
                </Button>
        }}
      />
```

그런 다음`navigation / BottomTabNavigator.tsx` 파일에서 하단 탭 탐색기에서 이전에 추가 한`CourseDetail` 화면을 제거합니다.
 

상세 화면은 이제 스택의 일부이므로 탐색 방법을 변경해야합니다.
 `TabOneScreen.tsx`로 돌아가서`onPress` 함수를 다음과 같이 바꾸십시오.
<pre”> <CategoryCard {… category} onPress = {() => navigation.navigate (‘CourseDetailScreen’, {category})} />
 

params 객체 아래가 아니라 필드 이름 `category`를 사용하여 매개 변수를 직접 전달하는 방법을 확인하세요.
 즉, 화면 구성 요소에 다르게 전달됩니다.
 `screens / CourseDetail.tsx` 정의를 다음과 같이 조정 해 보겠습니다.
 

```js
export default function CourseDetailScreen({route}: StackScreenProps<RootStackParamList, 'CourseDetailScreen'>) {
```

따라서`TabOneParamList` 대신`RootStackParamList`를 사용하고 있습니다.
 `RootStackParamList` 정의를 살펴보기 전에 이미 파일에 있으므로이 화면에 작업 버튼을 추가하겠습니다.
 `ScrollView` 구성 요소 아래에 다음 버튼 구성 요소를 추가하고 둘 다 조각 (`<>`)으로 래핑합니다.
 

```xml
        <>
         <ScrollView>
         //...previous code
         </ScrollView>
         <Button block bg="pink900" rounded="circle" mx={30} top={-30} pt={20} pb={15}>
            Get the course
        </Button>
        </>
```

마지막으로`types.tsx` 파일을 열고 아래와 같이 상세 화면 매개 변수 정의를 이동합니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Detail-Screen-Param-Definition.png?resize=730%2C270&ssl=1)

이 시점에서 상세 화면에는 더 이상 하단 탭이없고 대신 아래와 같은 작업 버튼이 표시되어야합니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Detail-Screen-with-Action-Button.png?resize=730%2C1578&ssl=1)

## 마무리
 verified_user

글을 통해 Magnus UI로 UI 구축이 얼마나 쉬워 지는지 분명해 졌으면합니다.
 처음부터 앱이기 때문에 많은 구성 단계를 수행했지만 일단 이러한 단계를 벗어나면 Magnus UI로 구성 요소를 계속 빌드 할 수 있으며 소품은 제 2의 특성이됩니다.
 