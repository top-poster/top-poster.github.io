---
layout: post
title: "반응 네이티브를 사용하여 회전각 구현"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/carousel-react-native.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/carousel-react-native.png?fit=730%2C487&ssl=1)

모바일 애플리케이션에서 수평 공간을 가장 잘 사용하는 방법을 생각해 본 적이 있다면 회전식 캐러셀을 사용하는 것을 고려해 보는 것이 좋습니다. 캐러셀은 사용자가 가로와 세로로 모두 표시할 수 있는 일련의 콘텐츠를 순환할 수 있도록 도와 개발자가 다양한 종류의 기기, 특히 모바일 기기에 콘텐츠를 쉽게 표시할 수 있도록 돕는다.

이 튜토리얼에서는 Ract Native를 사용하여 모바일 애플리케이션에 회전식 회전식 회전식을 만듭니다.

## 전제조건

틴더 애니메이션을 사용하여 회전식 캐러셀을 만들 예정이지만 시작하기 전에 이 튜토리얼에서는 다음과 같은 전제 조건이 있다고 가정합니다.

- JavaScript 지식(ES6+)
- 반응 v16+에 익숙함
- 반응 네이티브에 대한 지식
- 코드 편집기
- 터미널의 기본 지식
- npm 및/또는 실
- Node.js(v10+)

## 시작 중

먼저 엑스포로 애플리케이션을 부트스트랩하겠습니다. 터미널을 열고 아래에 코드를 입력하여 Expo CLI가 설치되어 있지 않으면 설치하십시오.

```coffeescript
npm install expo-cli --global
```

Expo를 설치한 후 React Native를 초기화하는 데 사용할 수 있습니다. 프로젝트를 추가할 디렉터리로 이동하고 다음을 입력하십시오.

```undefined
expo init ReactNativeCarouselTutorial
```

관리되는 워크플로 섹션에서 빈 템플릿을 선택하십시오.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/blank-template.png?resize=730%2C151&ssl=1)

필요한 종속성을 모두 설치한 후 터미널의 프로젝트 디렉터리로 이동하여 프로젝트를 시작하십시오.

```undefined
expo start
```

이렇게 하면 브라우저에서 Metro Bundler가 실행되어 선택한 에뮬레이터에서 응용 프로그램을 실행할 수 있습니다. 아래 스크린샷과 유사한 스크린샷이 표시됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/metro-bundler-in-browser.png?resize=730%2C385&ssl=1)

## 회전각 생성

회전목마를 만들려면 터미널을 열고 프로젝트 디렉터리로 이동한 다음 다음 다음 명령을 입력하십시오.

```undefined
npm install --save react-native-snap-carousel
```

이렇게 하면 회전식 스냅-카로젤이 설치됩니다. 이 패키지는 회전식 캐로젤을 생성하는 데 사용됩니다.

설치 후 루트 디렉터리에 `data.js` 파일을 생성하고 아래 코드를 붙여넣으십시오.

```coffeescript
export default data = [
  {
    title: "Aenean leo",
    body: "Ut tincidunt tincidunt erat. Sed cursus turpis vitae tortor. Quisque malesuada placerat nisl. Donec quam felis, ultricies nec, pellentesque eu, pretium quis, sem.",
    imgUrl: "https://picsum.photos/id/11/200/300"
  },
  {
    title: "In turpis",
    body: "Aenean ut eros et nisl sagittis vestibulum. Donec posuere vulputate arcu. Proin faucibus arcu quis ante. Curabitur at lacus ac velit ornare lobortis. ",
    imgUrl: "https://picsum.photos/id/10/200/300"
  },
  {
    title: "Lorem Ipsum",
    body: "Phasellus ullamcorper ipsum rutrum nunc. Nullam quis ante. Etiam ultricies nisi vel augue. Aenean tellus metus, bibendum sed, posuere ac, mattis non, nunc.",
    imgUrl: "https://picsum.photos/id/12/200/300"
  }
]
```

데이터 파일은 회전목마를 채우는 데 사용할 모의 데이터를 포함하는 배열입니다.

그런 다음 `CarouselCardItem.js`라는 파일을 만들어 엽니다. 다음 코드를 삽입합니다.

```js
import React from 'react'
import { View, Text, StyleSheet, Dimensions, Image } from "react-native"

export const SLIDER_WIDTH = Dimensions.get('window').width + 80
export const ITEM_WIDTH = Math.round(SLIDER_WIDTH * 0.7)

const CarouselCardItem = ({ item, index }) => {
  return (
    <View style={styles.container} key={index}>
      <Image
        source={ uri: item.imgUrl }
        style={styles.image}
      />
      <Text style={styles.header}>{item.title}</Text>
      <Text style={styles.body}>{item.body}</Text>
    </View>
  )
}
const styles = StyleSheet.create({
  container: {
    backgroundColor: 'white',
    borderRadius: 8,
    width: ITEM_WIDTH,
    paddingBottom: 40,
    shadowColor: "#000",
    shadowOffset: {
      width: 0,
      height: 3,
    },
    shadowOpacity: 0.29,
    shadowRadius: 4.65,
    elevation: 7,
  },
  image: {
    width: ITEM_WIDTH,
    height: 300,
  },
  header: {
    color: "#222",
    fontSize: 28,
    fontWeight: "bold",
    paddingLeft: 20,
    paddingTop: 20
  },
  body: {
    color: "#222",
    fontSize: 18,
    paddingLeft: 20,
    paddingLeft: 20,
    paddingRight: 20
  }
})

export default CarouselCardItem
```

회전식 카드 아이템은 회전식 카드 모양을 구현한다. 소품으로 전달된 항목을 표시할 구성 요소를 반환합니다.

이제 회전목마를 표시할 수 있는 부품을 만들어 보겠습니다. `CarouselCards`라는 파일을 만들어 열고 다음 코드를 입력하십시오.

```js
import React from 'react'
import { View } from "react-native"
import Carousel from 'react-native-snap-carousel'
import CarouselCardItem, { SLIDER_WIDTH, ITEM_WIDTH } from './CarouselCardItem'
import data from './data'

const CarouselCards = () => {
  const isCarousel = React.useRef(null)

  return (
    <View>
      <Carousel
        layout="tinder"
        layoutCardOffset={9}
        ref={isCarousel}
        data={data}
        renderItem={CarouselCardItem}
        sliderWidth={SLIDER_WIDTH}
        itemWidth={ITEM_WIDTH}
        inactiveSlideShift={0}
        useScrollView={true}
      />
    </View>
  )
}


export default CarouselCards
```

우리는 반응성 스냅 캐러셀 패키지의 캐러셀 구성요소를 사용하여 캐러셀을 구현한다. 구성 요소에서 수용되는 소품은 다음과 같습니다.

- ➡: 아이템의 렌더링 방식을 정의합니다. 옵션에는 기본값, 스택 및 틴더가 포함됩니다.
- `layoutCardOffset`: 스택 및 틴더 레이아웃 모두에서 기본 카드 오프셋을 늘리거나 줄이는 데 사용됩니다.
- `ref`: 회전각의 인스턴스에 대한 참조를 생성합니다.
- `data`: 반복할 항목의 배열
- `renderItem`: 데이터에서 항목을 가져와 목록에 렌더링합니다.
- `슬라이더 폭`: 회전각의 픽셀 폭
- `itemWidth`: 회전목마 항목의 픽셀 단위
- `ScrollView 사용`: 성능상의 이유로 Flatlist 구성 요소가 아닌 ScrollView 구성 요소를 사용하도록 선택합니다.
- "OnSnapToItem": 프로그래밍 방식으로 회전식 항목에 스냅

## 페이지 추가

사용자가 연속해서 스와이프하지 않고도 캐러셀의 특정 항목으로 건너뛸 수 있도록 페이징을 추가할 수 있습니다. 우리가 가장 먼저 해야 할 일은 부품을 수입하는 것입니다. 이렇게 하려면 Carousel 구성 요소를 가져온 라인으로 이동하십시오.

```coffeescript
import Carousel from 'react-native-snap-carousel'
```

이렇게 보이도록 리팩터링합니다.

```coffeescript
import Carousel, { Pagination } from 'react-native-snap-carousel'
```

그런 다음 현재 페이지 지정 인덱스를 저장할 상태를 생성합니다.

```cpp
const [index, setIndex] = React.useState(0)
```

그런 다음 Carousel 구성 요소 아래에 다음 코드를 붙여 넣습니다.

```xml
<Pagination
  dotsLength={data.length}
  activeDotIndex={index}
  carouselRef={isCarousel}
  dotStyle={
    width: 10,
    height: 10,
    borderRadius: 5,
    marginHorizontal: 0,
    backgroundColor: 'rgba(0, 0, 0, 0.92)'
  }
  inactiveDotOpacity={0.4}
  inactiveDotScale={0.6}
  tappableDots={true}
/>
```

이렇게 하면 페이지 분할 구성요소가 렌더링되며, 기본적으로 점 목록은 다음과 같습니다. 페이지 지정 구성 요소는 다음과 같은 소품을 사용합니다.

- `dotsLength`: 표시할 점 수
- `activeDotIndex`: 현재 활성 도트의 인덱스
- carauselRef: 페이지 분할이 연결된 carausel 구성 요소에 대한 참조
- `dotstyle`: 기본값으로 병합할 도트 스타일
- `비활성 도트 불투명도`: 비활성 도트에 적용되는 불투명도 값
- 비활성 도트스케일: 비활성 도트에 적용되는 스케일 변환 값
- `tapable dots`: 기본 도트를 태핑할 수 있도록 하여 도트를 두드리면 캐러셀이 해당 항목으로 미끄러집니다.

수정된 `Carousel Cards` 구성 요소는 아래 코드와 유사해야 합니다.

```coffeescript
import React from 'react'
import { View } from "react-native"
import Carousel, { Pagination } from 'react-native-snap-carousel'
import CarouselCardItem, { SLIDER_WIDTH, ITEM_WIDTH } from './CarouselCardItem'
import data from './data'


const CarouselCards = () => {
  const [index, setIndex] = React.useState(0)
  const isCarousel = React.useRef(null)


  return (
    <View>
      <Carousel
        layout="tinder"
        layoutCardOffset={9}
        ref={isCarousel}
        data={data}
        renderItem={CarouselCardItem}
        sliderWidth={SLIDER_WIDTH}
        itemWidth={ITEM_WIDTH}
        onSnapToItem={(index) => setIndex(index)}
        useScrollView={true}
      />
      <Pagination
        dotsLength={data.length}
        activeDotIndex={index}
        carouselRef={isCarousel}
        dotStyle={
          width: 10,
          height: 10,
          borderRadius: 5,
          marginHorizontal: 0,
          backgroundColor: 'rgba(0, 0, 0, 0.92)'
        }
        inactiveDotOpacity={0.4}
        inactiveDotScale={0.6}
        tappableDots={true}
      />
    </View>

  )
}



export default CarouselCards
```

이제 `App.js` 파일로 이동하여 다음과 같은 방식으로 코드를 리팩터링합니다.

```js
import React from 'react'
import { SafeAreaView, StyleSheet } from 'react-native'
import CarouselCards from './CarouselCards'
export default function App() {
  return (
    <SafeAreaView style={styles.container}>
      <CarouselCards />
    </SafeAreaView>
  );
}
const styles = StyleSheet.create({
  container: {
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
    padding: 50
  },
});
```

저장 후 다음과 유사한 스크린샷이 표시됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/successfully-implemented-carousel.png?resize=315%2C646&ssl=1)

축하합니다! 당신은 당신의 회전목마를 다 마쳤습니다.

## 결론.

이 튜토리얼에서, 우리는 React Native를 가진 회전목마를 구현하는 과정을 거쳤습니다. 우리는 회전식 스냅 캐러셀을 사용했는데, 이것은 캐러셀을 만들기 위한 많은 옵션을 제공한다. 이 튜토리얼의 스낵 쇼케이스를 보고 싶으시면, 여기서 확인해 보세요.