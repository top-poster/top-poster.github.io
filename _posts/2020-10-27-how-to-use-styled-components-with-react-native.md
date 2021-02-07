---
layout: post
title: "React Native와 함께 스타일링된 구성 요소를 사용하는 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/styledcomponentsreactnative-nocdn.jpg"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/styledcomponentsreactnative-nocdn.jpg?fit=730%2C487&ssl=1)

응용 프로그램을 스타일링하는 것은 개발의 중요한 측면이다. 스타일을 관리하면 코드 가독성이 향상될 수 있습니다. React Native 앱에서는 구성요소 스타일링이 기본 스타일시트 API로 이루어진다. 그러나 스타일링된 구성요소를 사용하는 CSS-in-JS 접근 방식은 React Native 앱에서 UI 구성요소를 만들고 스타일링하는 또 다른 방법이다.

이 튜토리얼에서는 React Native의 일반 `StyleSheet` 매니저보다 스타일 구성 요소 같은 라이브러리가 어떤 이점을 가지고 있는지 논의해 보자.

## 스타일시트와 스타일 구성 요소 라이브러리의 차이점은 무엇입니까?

styled-components는 오픈 소스이며 React Native 개발자로서 단일 파일 위치에서 UI 컴포넌트와 스타일을 정의할 수 있는 CSS-in-JS 라이브러리이다. 대형 애플리케이션 작업 시 최적화된 개발자 경험을 얻을 수 있는 적합한 구성 요소와 스타일링을 결합하는 것이 쉬워집니다.

ReactNative의 기본 스타일 정의 방식보다 장점이 있는 것은 자바스크립트 객체를 사용하여 정의된 스타일보다 일반 CSS를 사용할 수 있다는 것이다. 웹 기반 배경 경험이 있는 개발자에게 더 좋습니다.

예를 들어 React Native는 CSS 속성 이름을 정의할 때 스타일링 규칙을 따릅니다. camelCase 기반 속성 이름을 사용한다. `StyleSheet` API를 사용하는 React Native 앱의 배경색 속성은 다음과 같이 작성됩니다.

```bash
backgroundColor: 'tomato';
```

스타일 구성 요소 라이브러리를 사용할 때 CSS 명명 규칙을 사용할 수 있습니다. 따라서 background color는 일반 CSS처럼 background-color로 표기된다.

React Native를 사용하면 px(픽셀)와 같은 단위 없이 정의하여 여백, 패딩, 그림자, 글꼴 크기 등과 같은 속성에 대한 값을 정의할 수 있습니다. 예를 들어, `StyleSheet` API를 사용할 때의 글꼴 크기는 다음과 같이 정의됩니다.

```undefined
fontSize: 20;
```

이러한 단위 없는 값은 특정 화면의 픽셀 밀도로 인해 다양한 화면 크기에 따라 다르게 렌더링됩니다. 그러나 글꼴 크기 같은 속성 값을 정의할 때는 스타일 구성 요소를 사용하여 px 접미사를 사용해야 합니다. 하지만 이것은 이 도서관에 의해 야기되는 단점이 있다는 것을 의미합니다. 뒤에서 스타일 구성 요소는 css-to-react-native라는 패키지를 사용하여 일반 CSS 속성을 React Native 스타일시트 개체로 변환한다.

## 스타일 구성 요소 설치

새 React Native 프로젝트를 만드는 것부터 시작합니다. 빠른 개발 과정을 위해 엑스포-cli를 이용하려고 합니다. 프로젝트 디렉터리가 생성되면 스타일 구성 요소 라이브러리를 설치해야 합니다. 터미널 창을 열고 다음 명령을 실행합니다.

```undefined
npx expo-cli init [Your Project Name]
# after the project is created
cd [Your Project Name]
# then install the library
yarn add styled-components@5.2.0
# also install the icons library
expo install @expo/vector-icons
```

## 스타일 구성 요소 사용

이 섹션에서는 `App.js` 파일에 앱의 제목을 표시하는 첫 번째 스타일 구성 요소를 작성하겠습니다. 시작하려면 라이브러리를 가져옵니다.

```coffeescript
import styled from 'styled-components/native';
```

Retact Native 앱에서 `tyled-components` 라이브러리를 사용하려면 Retact Native에서 직접 가져오는 대신 원시 구성 요소에 액세스할 수 있도록 `/native`를 가져와야 합니다.

그러면 react-native 라이브러리에서 View와 Text 구성 요소를 교체해 봅시다. 이 새로운 요소들은 `스타일 구성요소`의 사용자 정의 의미론을 따를 것이다.

다음은 스타일 구성 요소 라이브러리의 스타일 지정 개체를 사용하여 `보기` 및 `텍스트` 구성 요소의 예입니다.

```js
const Container = styled.View`
  flex: 1;
  background-color: white;
  align-items: center;
  justify-content: center;
`;
const Text = styled.Text`
  font-size: 18px;
  color: blue;
  font-weight: 500;
`;
```

이러한 사용자 지정 스타일 구성 요소는 모든 React Native 앱과 동일한 CSS 로직을 사용하여 구성 요소를 스타일링합니다. 여기서 다른 점은 속성 이름과 속성 값을 쓰는 것이 Retact Native가 아닌 일반 CSS 규칙을 잘 따른다는 것이다.

수정된 `App` 구성 요소는 다음과 같습니다.

```js
export default function App() {
  return (
    <Container>
      <Text>Open up App.js to start working on your app!</Text>
    </Container>
  );
}
```

이제 터미널 창으로 돌아가서 실행 중인 코드를 보려면 yarn start 명령을 실행합니다. 이제 모든 iOS 또는 Android 에뮬레이터 또는 Expo Client 앱이 있는 실제 장치에서 다음 결과를 볼 수 있습니다.

![image](https://blog.logrocket.com/wp-content/uploads/2020/10/emulator-nocdn.png)

## 스타일 구성 요소에서 소품 사용

모든 Reactor React Native 앱에서 사용자 지정 구성 요소를 생성하려면 소품을 사용해야 합니다. 여기서 장점은 응용 프로그램의 코드가 가능한 한 건성으로 유지된다는 것입니다. 스타일 구성 요소 라이브러리는 사용자 정의 구성 요소(StyleSheet 개체를 사용할 때 React Native에서 사용할 수 없는 항목)에서 소품을 소비할 수 있습니다.

자질구레한 예를 들어보자. `components/` 디렉토리에서 `PressableButton.js`라는 새 구성요소 파일을 작성합니다. 이 구성 요소는 나중에 `App` 구성 요소에 사용되어 사용자 지정 버튼 구성 요소를 표시합니다.

이 파일 안에서 `터치 가능 용량`과 `텍스트`를 사용하여 사용자 지정 구성 요소를 생성해 보겠습니다. 이 사용자 지정 버튼 구성 요소에는 사용자 지정 버튼 `타이틀`과 `bgColor`와 같은 소품이 포함되어 버튼 구성 요소의 배경색을 부모 구성 요소의 유효한 값으로 설정합니다.

```js
import React from 'react';
import styled from 'styled-components/native';
const ButtonContainer = styled.TouchableOpacity`
  margin-vertical: 40px;
  width: 120px;
  height: 40px;
  padding: 12px;
  border-radius: 10px;
  background-color: ${props => props.bgColor};
`;
const ButtonText = styled.Text`
  font-size: 16px;
  text-align: center;
`;
const PressableButton = ({ onPress, bgColor, title }) => (
  <ButtonContainer onPress={onPress} bgColor={bgColor}>
    <ButtonText>{title}</ButtonText>
  </ButtonContainer>
);
export default PressableButton;
```

이 사용자 지정 구성 요소의 배경색을 동적으로 설정하기 위해 `${dupper => 소품...과 같은 보간 함수를 전달할 수 있습니다.}`.

이 보간 함수는 다음 코드 조각과 동일합니다.

```php
background-color: ${(props) => {
  return (props.bgColor)
}
```

이를 테스트하려면 `App.js` 구성 요소 파일로 이동하여 다른 문 뒤에 `PressableButton` 구성 요소를 가져오십시오.

```coffeescript
import PressableButton from './components/PressableButton';
```

그런 다음 내용을 수정합니다.

```js
export default function App() {
  return (
    <Container>
      <Text>Open up App.js to start working on your app!</Text>
      <PressableButton
        onPress={() => true}
        title='First button'
        bgColor='papayawhip'
      />
    </Container>
  );
}
```

다음은 이 단계 후에 얻을 출력입니다.

다른 배경색으로 단추를 하나 더 추가합니다.

![image](https://blog.logrocket.com/wp-content/uploads/2020/10/firstbutton-nocdn.png)

```php
<PressableButton onPress={() => true} title='First button' bgColor='#4267B2' />
```

첫 번째 버튼 뒤에 표시됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/twofirstbuttons.png?resize=350%2C647&ssl=1)

스타일 구성 요소 라이브러리의 보간 함수를 사용하여 특정 속성의 스타일을 확장할 수도 있습니다. 예를 들어, 두 번째 단추의 텍스트 색상이 보기 좋지 않습니다. 이를 변경하기 위해 우리는 `누르는 버튼` 구성 요소의 두 가지 변형을 원한다.

첫 번째는 본문의 색이 흰색으로 되는 `기본`이 된다. 두 번째 변종은 검정색 텍스트 색을 띠지만 기본 변종처럼 동작한다. 즉, 두 번째 변형을 명시적으로 정의할 필요가 없습니다.

`ButtonText` 스타일 개체에 `color` 특성을 추가하여 `PressableButton.js` 파일을 수정합니다.

```js
const ButtonText = styled.Text`
  font-size: 16px;
  text-align: center;
  color: ${props => (props.primary ? 'white' : '#010101')};
`;
```

그런 다음 `PressableButton` 구성 요소에 `Primary`를 추가합니다.

```js
const PressableButton = ({ onPress, primary, bgColor, title }) => (
  <ButtonContainer onPress={onPress} bgColor={bgColor}>
    <ButtonText primary={primary}>{title}</ButtonText>
  </ButtonContainer>
);
```

이제 `App.js` 파일로 돌아가서 두 번째 단추에 propect 변형 `primary`를 추가합니다.

```php
<PressableButton
  onPress={() => true}
  title='First button'
  bgColor='#4267B2'
  primary
/>
```

변경 사항은 Expo Client 앱에 즉시 반영됩니다.

![image](https://blog.logrocket.com/wp-content/uploads/2020/10/openupappjs-nocdn.png)

## 처음부터 앱 화면 구축

스타일 구성 요소 라이브러리에서 `스타일링` 개체를 사용하여 서로 다른 원시 반응 네이티브 구성 요소를 정의하는 방법에 대해 앱 화면을 구축해 보겠습니다.

우리가 구축하려는 목적을 간략히 말씀드리자면, 최종 결과는 다음과 같습니다.

![image](https://blog.logrocket.com/wp-content/uploads/2020/10/user1user2-nocdn.png)

계속 진행하려면 다음 자료를 여기에서 다운로드하십시오.

## SafeAreaView 구성 요소 추가

먼저 앱의 머리글 섹션을 만드는 것부터 시작하겠습니다. `components/Header.js`라는 새 구성 요소 파일을 생성합니다. 이 구성 요소는 앱 `타이틀`과 두 개의 아이콘을 연속으로 표시합니다. 이 두 아이콘을 사용하여 추가 작업을 만들 수 있습니다. 아이콘을 표시하려면 `Material Community`를 사용하십시오.@expo/vector-icons의 아이콘입니다.

이 헤더 구성 요소는 상위 구성 요소인 앱에서 헤더타이틀을 받게 됩니다. 제목은 텍스트에서 정의되며 각 아이콘은 아이콘 버튼이라는 터치식 불투명 버튼으로 감싼다.

다음 코드 조각을 추가합니다.

```coffeescript
import React from 'react';
import styled from 'styled-components/native';
import { MaterialCommunityIcons } from '@expo/vector-icons';
const Container = styled.View`
  width: 100%;
  height: 50px;
  padding-horizontal: 10px;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
`;
const Title = styled.Text`
  font-size: 28px;
  font-weight: 700;
  letter-spacing: 0.25px;
  color: #4267b2;
`;
const IconButtonsRow = styled.View`
  flex-direction: row;
`;
const IconButton = styled.TouchableOpacity`
  width: 40px;
  height: 40px;
  border-radius: 20px;
  background: #e6e6e6;
  align-items: center;
  justify-content: center;
  margin-left: 12px;
`;
const Header = ({ headerTitle }) => {
  return (
    <Container>
      <Title>{headerTitle}</Title>
      <IconButtonsRow>
        <IconButton activeOpacity={0.7} onPress={() => true}>
          <MaterialCommunityIcons name='magnify' size={28} color='#010101' />
        </IconButton>
        <IconButton activeOpacity={0.7} onPress={() => true}>
          <MaterialCommunityIcons
            name='facebook-messenger'
            size={28}
            color='#010101'
          />
        </IconButton>
      </IconButtonsRow>
    </Container>
  );
};
export default Header;
```

위의 `아이콘 버튼` 코드 조각에서 `액티브 옵티비티`와 같은 사용 가능한 프로포트를 `터치 가능 옵티비티` 구성 요소에서 사용하는 것과 같은 방법으로 사용할 수 있다는 점을 주목하십시오. 스타일링된 구성 요소는 React Native 구성 요소에서 사용할 수 있는 소품을 계속 사용합니다.

그런 다음 `App.js` 파일 내에서 이 구성 요소를 가져와 수정합니다.

```js
import React from 'react';
import styled from 'styled-components/native';
import Header from './components/Header';
const Container = styled.View`
  flex: 1;
  background-color: white;
`;
export default function App() {
  return (
    <Container>
      <Header headerTitle='social' />
    </Container>
  );
}
```

다음은 iOS 시뮬레이터에서 얻을 출력입니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/iossimulator.png?resize=350%2C646&ssl=1)

헤더(Header) 부품이 iOS 기기의 안전한 영역 경계 뒤의 공간을 차지하고 있다. 이를 해결하기 위해 다행히 스타일 구성 요소 라이브러리는 React Native의 SafeAreaView와 같은 방식으로 작동하는 SafeAreaView라는 구성 요소를 제공한다.

다음 조각을 수정합니다.

```cpp
const Container = styled.SafeAreaView;
```

이제 출력이 원하는 결과입니다.

![image](https://blog.logrocket.com/wp-content/uploads/2020/10/social-nocdn.png)

## '이미지' 구성 요소를 사용하여 아바타 표시

아바타 이미지를 표시하기 위해 이미지 구성요소를 사용할 수 있습니다. `components/Avatar.js`라는 새 구성 요소 파일을 생성합니다. 그것은 표시할 이미지의 원천인 하나의 소품을 받을 것이다. `소스` 받침대를 사용하여 이미지를 참조할 수 있습니다.

아바타 이미지의 스타일링은 가로 64픽셀의 높이로 시작한다. 가로와 높이 값의 정확히 절반에 해당하는 테두리 반지름 특성을 지녀 이미지를 원으로 만든다. 테두리-반경(border-radius) 속성은 둥근 모서리를 만드는 데 사용됩니다.

다음 코드 조각을 추가합니다.

```js
import React from 'react';
import styled from 'styled-components/native';
const Container = styled.View`
  width: 64px;
  height: 64px;
`;
const Image = styled.Image`
  width: 64px;
  height: 64px;
  border-radius: 32px;
`;
const Avatar = ({ imageSource }) => {
  return (
    <Container>
      <Image source={imageSource} />
    </Container>
  );
};
export default Avatar;
```

이제 App.js 파일을 열고 아바타 구성 요소를 가져옵니다. 다음 스타일을 가진 다른 구성 요소인 `row Container`로 포장합니다.

```js
// after other import statements
import Avatar from './components/Avatar';
const RowContainer = styled.View`
  width: 100%;
  padding-horizontal: 10px;
  flex-direction: row;
`;
```

`App` 구성 요소에서 렌더링된 JSX를 수정합니다.

```xml
<Container>
  <Header headerTitle='social' />
  <RowContainer>
    <Avatar imageSource={require('./assets/images/avatar1.png')} />
  </RowContainer>
</Container>
```

아바타 이미지는 다음과 같습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/avatarimagedisplayed.png?resize=350%2C646&ssl=1)

## '위치: 절대' 속성을 사용하여 온라인 표시기 만들기

여백과 패딩과 같은 CSS 속성은 서로 다른 UI 요소 사이의 간격을 추가하기 위해 사용할 수 있다. 간격은 서로 관련이 있다. 이 관계가 앱 상태의 설계와 정확히 일치하지 않는 시나리오가 있습니다. 이러한 시나리오에서 우리는 종종 `position: 절대` 속성을 사용한다.

React Native는 기본적으로 `position: relative` 속성에 의존합니다. 그렇기 때문에 UI 구성 요소를 스타일링할 때마다 명시적으로 정의할 필요가 없습니다. 절대값은 정확한 설계 패턴에 맞는 시나리오에만 사용됩니다. 이 기능은 다음과 같이 조합하여 사용할 수 있습니다.

- 맨 위의
- 왼쪽의
- 맞다
- 밑바닥의

아바타 이미지 오른쪽 상단 모서리에 온라인 상태 표시기를 표시해 아바타 구성 요소를 개선해 보자.

온라인 표시기라는 새 사용자 지정 구성 요소를 정의합니다. 경계선뿐 아니라 폭과 높이값도 확정된다. 스타일 구성 요소를 사용하면 일반 CSS와 동일한 방식으로 `경계` 속성의 값을 정의할 수 있다는 장점이 있습니다.

`포지션`은 `절대`로 설정돼 있어 `온라인 표시기` 구성요소가 모 구성요소에 상대적으로 위치하게 된다. 위쪽과 오른쪽 모서리에 표시되어야 하므로 두 속성의 값을 `0`으로 설정해 보겠습니다.

픽셀 단위로 각 값을 늘리거나 줄여서 현재 따르는 설계 패턴을 충족하도록 이러한 값을 수정할 수 있습니다.

```js
const OnlineIndicator = styled.View`
  background-color: green;
  position: absolute;
  width: 16px;
  height: 16px;
  border-radius: 8px;
  top: 0;
  right: 0;
  border: 2px solid white;
`;
const Avatar = ({ imageSource }) => {
  return (
    <Container>
      <Image source={imageSource} />
      <OnlineIndicator />
    </Container>
  );
};
```

다음은 이 단계 이후의 출력입니다.

![image](https://blog.logrocket.com/wp-content/uploads/2020/10/onlineindicator-nocdn.png)

## '텍스트 입력' 추가

텍스트 입력 구성 요소를 추가하기 위해 `tyled` 개체는 `TextInput`을 사용합니다. `components/InputContainer.js`라는 새 파일을 생성하고 다음 코드 조각을 추가하십시오.

```js
import React from 'react';
import styled from 'styled-components/native';
const Container = styled.View`
  width: 100%;
`;
const TextInput = styled.TextInput`
  width: 100%;
  height: 60px;
  font-size: 18px;
  flex: 1;
  color: #010101;
  margin-left: 10px;
`;
const InputContainer = () => {
  return (
    <Container>
      <TextInput placeholder="What's on your mind?" />
    </Container>
  );
};
export default InputContainer;
```

아바타 이미지 옆에 이 구성 요소를 표시하려고 하므로 `App.js` 파일로 가져오도록 하겠습니다.

```js
<RowContainer>
  <Avatar imageSource={require('./assets/images/avatar1.png')} />
  <InputContainer />
</RowContainer>
```

출력은 다음과 같습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/status-ncdn.png?resize=350%2C646&ssl=1)

## ScrollView를 사용하여 메뉴 항목 목록 매핑

ScrollView 구성 요소를 사용하여 게시물 목록을 표시해 보겠습니다. 각 게시물에는 사용자의 아바타 이미지, 사용자 이름, 게시물 설명, 게시물 이미지가 포함됩니다. 데이터를 표시하기 위해 네 개의 다른 객체가 포함된 모의 배열을 사용할 것입니다. `App.js` 파일에 이 어레이 추가:

```js
const DATA = [
  {
    id: '1',
    userAvatar: require('./assets/images/avatar2.png'),
    userName: 'User 1',
    postText:
      'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua',
    postImage: require('./assets/images/post1.png')
  },
  {
    id: '2',
    userAvatar: require('./assets/images/avatar4.png'),
    userName: 'User 2',
    postText:
      'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua',
    postImage: require('./assets/images/post2.png')
  },
  {
    id: '3',
    userAvatar: require('./assets/images/avatar3.png'),
    userName: 'User 3',
    postText:
      'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua',
    postImage: require('./assets/images/post3.png')
  },
  {
    id: '4',
    userAvatar: require('./assets/images/avatar4.png'),
    userName: 'User 4',
    postText:
      'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua',
    postImage: require('./assets/images/post4.png')
  }
];
```

그런 다음 `components/Card.js`라는 새 구성 요소 파일을 만듭니다. 데이터 배열을 유일한 버팀목으로 받아들이기로 했다. data 배열을 통해 반복하기 위해 자바스크립트의 map 기능을 사용한다. 우리는 이미 표현된 데이터의 구조를 알고 있다.

스크롤 뷰에서 정의된 `수직 목록`을 생성하는 것부터 시작합니다. 그런 다음 빈 `List Container` 구성 요소를 만듭니다. 이 구성 요소는 `View` 구성 요소를 나타냅니다.

사용자 이름과 아바타 이미지를 표시하려면 `Flex-direction` 속성이 `row` 값으로 설정된 `row` 컨테이너 구성 요소를 생성하십시오. 스몰 아바타는 사용자 아바타 이미지를, 유저네임은 사용자 이름을 표시한다. 마찬가지로 포스트 설명과 포스트이미지는 포스트의 내용을 표시하는 데 사용된다.

다음 조각을 추가합니다.

```js
import React from 'react';
import styled from 'styled-components/native';
const VerticalList = styled.ScrollView`
  flex: 1;
`;
const ListContainer = styled.View``;
const Header = styled.View`
  height: 50px;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  margin-top: 6px;
  padding: 0 11px;
`;
const Row = styled.View`
  align-items: center;
  flex-direction: row;
`;
const SmallAvatar = styled.Image`
  width: 32px;
  height: 32px;
  border-radius: 16px;
`;
const UserName = styled.Text`
  padding-left: 8px;
  font-size: 14px;
  font-weight: bold;
  color: #010101;
`;
const PostDescription = styled.Text`
  font-size: 14px;
  color: #222121;
  line-height: 16px;
  padding: 0 11px;
`;
const PostImage = styled.Image`
  margin-top: 9px;
  width: 100%;
  height: 300px;
`;
const Card = ({ data }) => {
  return (
    <VerticalList showsVerticalScrollIndicator={false}>
      {data.map(item => (
        <ListContainer key={item.id}>
          <Header>
            <Row>
              <SmallAvatar source={item.userAvatar} />
              <UserName>{item.userName}</UserName>
            </Row>
          </Header>
          <PostDescription>{item.postText}</PostDescription>
          <PostImage source={item.postImage} />
        </ListContainer>
      ))}
    </VerticalList>
  );
};
export default Card;
```

ReactNative 구성 요소에서 사용 가능한 모든 속성은 `tyled` 개체를 사용하여 생성된 구성 요소에서 유효합니다. 예를 들어, 수직 스크롤 표시기를 숨기려면 위의 코드 조각처럼 `showVerticalScroll Indicator` 값을 false로 설정합니다.

ScrollView가 작동하는 것을 보려면 아래 그림과 같이 App.js 파일에 `Card` 구성 요소를 추가하십시오.

```js
// import the Card component
import Card from './components/Card';
return (
  <Container>
    {/* rest remains same */}
    <Card data={DATA} />
  </Container>
);
```

출력은 다음과 같습니다.

또한 `attrs`라는 체인형 방법을 사용하여 `ScrollView`로 스타일링된 객체를 확장할 수 있습니다. 스타일의 구성 요소에 소품을 부착합니다.

![image](https://blog.logrocket.com/wp-content/uploads/2020/10/scrollview-nocdn.png)

우리가 만든 VerticalList 구성 요소는 사용자 지정 소품이 필요 없지만 체인이 가능한 방법을 사용하여 ScrollView의 Content ContainerStyle 객체를 사용하여 스크롤 뷰 콘텐츠 컨테이너에 스타일을 적용할 수 있습니다. 20의 세로 패딩과 배경색을 추가해 보겠습니다.

```js
const VerticalList = styled.ScrollView.attrs(props => ({
  contentContainerStyle: {
    backgroundColor: '#e7e7e7',
    paddingVertical: 20
  }
}))`
  flex: 1;
`;
```

## 결론

나는 네가 이 튜토리얼을 재미있게 읽었기를 바래. React Native와 함께 스타일 구성 요소 라이브러리를 사용하면 이 게시물에서 논의한 바와 같이 이점을 공유할 수 있다.

React Native와 함께 스타일링 구성 요소를 사용해 본 적이 있습니까? 그렇지 않다면, 다음 프로젝트에서 지금 시도해 볼 건가요? 아래에 주석을 달아라.

이 GitHubrepo에서 사용할 수 있는 소스 코드입니다.