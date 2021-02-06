---
layout: post
title: "반응형 피커 선택 사용 방법"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/reactnativepickerselect1.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/reactnativepickerselect1.png?fit=730%2C487&ssl=1)

## 도입

react-native-picker-select는 안드로이드와 iOS의 네이티브 선택 인터페이스를 모방한 React Native picker 구성 요소이다.

네이티브 `select` 인터페이스를 모방하지만 개발자는 적합한 인터페이스를 사용자 정의할 수 있다.

iOS의 경우 기본적으로 "untyled TextInput" 구성 요소만 뒤틀립니다. 개발자는 스타일을 전달하여 모양과 느낌을 사용자 정의할 수 있습니다. 그러나 Android의 경우 기본 선택기가 기본적으로 사용됩니다.

안드로이드 사용자는 네이티브 안드로이드 피커 스타일(native Android pickerstyle) 소품으로 false를 전달하여 styled Text Input을 사용하도록 강제할 수 있다. 이제 스타일을 전달하여 추가 사용자 지정을 수행할 수 있습니다.

➡-➡-➡-➡-➡-➡-➡-➡-➡-➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡ 다음 섹션에서 사용하겠습니다.

## 시작 중

모든 npm 패키지와 마찬가지로 설치도 간단해야 합니다. 그러나 특히 expo를 사용할 경우 react-native-picker-select에는 gotcha가 붙는다.

설치하려면 다음을 실행하십시오.

```undefined
npm i react-native-picker-select
```

작동 가능하지만 다음과 같은 오류가 발생할 수 있습니다.

```coffeescript
requireNativeComponent "RNCPicker" was not found in the UIManager error
```

다음을 실행하여 이 오류를 해결할 수 있습니다.

```java
// React Native CLI
npm install @react-native-community/picker
npx pod-install
```

그리고

```java
// Expo users
expo install @react-native-community/picker
```

환경에 따라 다음 명령을 실행하면 문제가 해결됩니다.
앱 디렉터리에 `cd`를 설치한 후 다음을 실행합니다.

```undefined
npm install or yarn install
```

이렇게 하면 필요한 종속성이 모두 설치됩니다. 그런 다음 실행:

```undefined
npm start or yarn start
```

애플리케이션용 개발 서버를 시작하여 개발 환경 설정을 완료합니다.

일단 환경을 조성하고 나면, 우리는 `반(反)토종-피커-선택`을 구체적으로 어떻게 사용하는지 바로 알 수 있다.

다음 코너에서 하자.

## 'react-native-picker-select' 구성 요소 사용

개발 환경 설정이라는 번거로움을 겪고 싶지 않으시다면, 저는 당신을 위한 시작 앱을 만들었습니다.

다음 작업을 실행하여 복제하기만 하면 됩니다.

```php
git clone https://github.com/lawrenceagles/react-native-picker-demo
```

`cd`를 앱 폴더에 넣고 이미 명시된 대로 앱을 시작합니다. 우리는 아래와 같이 "안녕의 세계"와 "선택"의 구성 요소로 환영받는다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/startingmetrobundler.png?resize=730%2C425&ssl=1)

위의 앱은 react-native-picker-select의 가장 기본적인 사용법이다. 아래 코드를 고려하십시오.

```undefined
 import React from "react";
 import RNPickerSelect from "react-native-picker-select";
 import { StyleSheet, Text, View } from "react-native";
 export default function App () {
     return (
         <View style={styles.container}>
             <Text>Hello World!</Text>
             <RNPickerSelect
                 onValueChange={(value) => console.log(value)}
                 items={[
                     { label: "JavaScript", value: "JavaScript" },
                     { label: "TypeStript", value: "TypeStript" },
                     { label: "Python", value: "Python" },
                     { label: "Java", value: "Java" },
                     { label: "C++", value: "C++" },
                     { label: "C", value: "C" },
                 ]}
             />
         </View>
     );
 }
 const styles = StyleSheet.create({
     container : {
         flex            : 1,
         backgroundColor : "#fff",
         alignItems      : "center",
         justifyContent  : "center",
     },
 });
```

react-native-picker-select로 작업하려면 RNpickerSelect 구성 요소를 가져와야 합니다.

```coffeescript
import RNPickerSelect from "react-native-picker-select";
```

그런 다음 이 구성 요소를 코드에서 재사용하여 `선택` 보기를 렌더링합니다. 그것은 수많은 소품들을 가지고 있고 그것들 중 두 개가 필요하다.

필요한 소품은 다음과 같습니다.

### '소품' 소품:

```undefined
[
    { label: "JavaScript", value: "JavaScript" },
    { label: "TypeStript", value: "TypeStript" },
    { label: "Python", value: "Python" },
    { label: "Java", value: "Java" },
    { label: "C++", value: "C++" },
    { label: "C", value: "C" },
]
```

이것은 개체의 배열입니다. 각 항목은 레이블 및 값 속성을 가진 개체입니다. 선택` 구성 요소를 클릭하면 항목이 사용자에게 제공된 옵션으로 나타납니다.

아래 이미지를 고려하십시오.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/items.png?resize=730%2C510&ssl=1)

### 온 밸류 체인지(onValue Change) 소품:

```undefined
onValueChange={(value) => console.log(value)}
```

선택한 `값`과 해당 `인덱스`를 반환하는 콜백 함수입니다. 이 콜백에서 반환된 데이터로 많은 작업을 수행할 수 있으므로 유용합니다.

지금은 선택한 값을 콘솔에 로깅하는 중입니다. 하지만 그 데이터로 매우 흥미로운 일들을 하는 패턴이 있습니다.

일반적인 패턴은 아래 코드와 같이 선택한 데이터로 상태를 설정하는 것입니다.

```undefined
onValueChange={(value) => setState(value)}
```

따라서 몇 가지 매우 강력한 패턴이 나타나므로 위의 예제를 업그레이드할 수 있습니다.

```undefined
import React, { useState } from "react";
import RNPickerSelect from "react-native-picker-select";
import { StyleSheet, Text, View } from "react-native";
export default function App () {
    const [ language, setLanguage ] = useState("");
    return (
        <View style={styles.container}>
            <Text>
                {language ?
                  `My favourite language is ${language}` :
                    "Please select a language"
                }
            </Text>
            <RNPickerSelect
                onValueChange={(language) => setLanguage(language)}
                items={[
                    { label: "JavaScript", value: "JavaScript" },
                    { label: "TypeStript", value: "TypeStript" },
                    { label: "Python", value: "Python" },
                    { label: "Java", value: "Java" },
                    { label: "C++", value: "C++" },
                    { label: "C", value: "C" },
                ]}
            />
        </View>
    );
}
```

위에서는 `onValueChange` 콜백 함수에서 선택한 값을 얻을 수 있습니다. 그 안에서 우리는 그것을 현재 언어로 언어 상태에 저장한다.

```undefined
onValueChange={(language) => setLanguage(language)}
```

그런 다음 현재 언어가 사용자에게 표시됩니다. 그러나 언어를 선택하지 않으면 기본 텍스트가 사용자에게 렌더링됩니다.

```js
<Text>
  {language ? `My favourite language is ${language}` : "Please select a language"}
</Text>
```

아래 이미지를 고려하십시오.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/setstate.png?resize=730%2C472&ssl=1)

다른 옵션 소품도 있지만, 기본적인 용도의 경우 위의 두 가지가 필요합니다. 주목할 만한 옵션 소품들을 살펴봅시다.

## 자리 표시자 소품:

아래 이미지를 고려하십시오.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/theplaceholderprop.png?resize=730%2C462&ssl=1)

항목을 선택하기 전에 `select an item…` 기본값이 `select component`에 표시됩니다. 이것은 현재 실행 중인 자리 표시자 지지입니다.

레이블과 값 속성을 모두 가진 개체입니다. 그러나 이 개체의 값은 null이고 레이블 값은 `" 항목 선택...아래와 같이 "

```undefined
{label: "select an item...", value: null } // the placeholder prop
```

우리는 앱을 다음과 같은 사용자 지정 자리 표시자 객체로 전달하여 보다 상호 작용하게 합니다.

```undefined
<View style={styles.container}>
  <Text>Hello World!</Text>
  <RNPickerSelect
      placeholder={ label: "Select you favourite language", value: null }
      onValueChange={(value) => console.log(value)}
      items={[
          { label: "JavaScript", value: "JavaScript" },
          { label: "TypeStript", value: "TypeStript" },
          { label: "Python", value: "Python" },
          { label: "Java", value: "Java" },
          { label: "C++", value: "C++" },
          { label: "C", value: "C" },
      ]}
  />
</View>
```

그러면 사용자에게 보다 적절한 메시지가 표시됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/selectcomponent.png?resize=730%2C485&ssl=1)

플레이스홀더를 완전히 사용 불가능으로 설정합니다. 이렇게 하려면 빈 개체를 전달할 수 있습니다.

```cpp
placeholder={} // disables the placeholder message
```

## 네이티브 안드로이드 피커 스타일 사용

안드로이드 전용 소품으로 안드로이드 전용이다. 앞서 담화에서 얘기한 것처럼 부울적 가치가 필요하다.

기본적으로 `react-native-picker-select`는 네이티브 안드로이드 `select`를 모방한다. 이것을 막기 위해 우리는 이 소품에게 거짓을 전달할 수 있다. 이렇게 하면 렌더링된 `select` 구성 요소가 `untyled TextInput`을 사용하게 됩니다. 이에 따라 디폴트(default)로 iOS처럼 행동하게 된다.

아래의 코드와 이미지를 고려하십시오.

```undefined
<View style={styles.container}>
  <Text>Hello World!</Text>
  <RNPickerSelect
      onValueChange={(value) => console.log(value)}
      useNativeAndroidPickerStyle={false}
      placeholder={ label: "Select your favourite language", value: null }
      items={[
          { label: "JavaScript", value: "JavaScript" },
          { label: "TypeStript", value: "TypeStript" },
          { label: "Python", value: "Python" },
          { label: "Java", value: "Java" },
          { label: "C++", value: "C++" },
          { label: "C", value: "C" },
      ]}
  />
</View>
```

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/reactnativepickerselect.png?resize=730%2C481&ssl=1)

모든 소품에 대한 자세한 내용은 여기에서 확인할 수 있습니다.

## 스타일링

➡-➡-➡-➡-➡-➡-➡ 선택➡은 스타일링을 위해 사용할 수 있는 여러 가지 특성을 가지고 있다. 이 모든 것은 `스타일` 소품 아래에 내포되어 있어야 한다. 이러한 속성 중 일부는 `플랫폼별`입니다. 입력을 대상으로 하여 iOS 및 Android 장치 모두에 대해 선택 구성요소를 스타일링할 수 있습니다.IOS와 inputAndroid 속성.

코드를 고려하십시오.

```undefined
const customPickerStyles = StyleSheet.create({
  inputIOS: {
    fontSize: 14,
    paddingVertical: 10,
    paddingHorizontal: 12,
    borderWidth: 1,
    borderColor: 'green',
    borderRadius: 8,
    color: 'black',
    paddingRight: 30, // to ensure the text is never behind the icon
  },
  inputAndroid: {
    fontSize: 14,
    paddingHorizontal: 10,
    paddingVertical: 8,
    borderWidth: 1,
    borderColor: 'blue',
    borderRadius: 8,
    color: 'black',
    paddingRight: 30, // to ensure the text is never behind the icon
  },
});
```

이 기능은 작동하지만 iOS select 구성 요소는 기본적으로 "untyled TextInput"을 감싸기 때문에 안드로이드 select 구성 요소보다 훨씬 더 많은 "스타일 객체"를 제공한다.

안드로이드 플랫폼에서는 기본적으로 입력안드로이드컨테이너, 입력안드로이드, 자리 표시자 등의 스타일 객체를 사용할 수 없다.

그러나 native Android Picker Style 사용 시 false를 전달하면 이를 활성화할 수 있다.

여기서 스타일링에 대한 자세한 정보를 얻을 수 있습니다.

또한 여기서 더 많은 스타일링 구현도 볼 수 있습니다.

## 마지막 생각

리액트 네이티브 픽커 셀렉트는 매우 강력하고 사용하기 쉬운 리액트 네이티브 부품이다.

완전한 양식 처리 구성 요소는 아니지만, 이 구성 요소는 매우 훌륭합니다. 그리고 Retact Native 응용 프로그램에서 `선택 필드`를 구현하는 것이 쉽고 재미있는 작업이 될 수 있다.

나는 당신이 이 담론을 즐겼기를 바랍니다. 그리고 이 멋진 구성 요소에 대한 자세한 정보가 필요하시면 해당 문서를 방문하십시오.