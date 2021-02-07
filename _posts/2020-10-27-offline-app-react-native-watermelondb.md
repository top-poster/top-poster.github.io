---
layout: post
title: "리액티브 네이티브 및 수박으로 완전 오프라인 앱 구축DB"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/offline-app-react-native-watermelondb.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/offline-app-react-native-watermelondb.png?fit=730%2C487&ssl=1)

## 펩토크

때때로, 기능적이고 유용한 앱에서 전체 기능을 제공하기 위해, 우리는 실제로 인터넷에 연결할 필요가 없다. 데이터를 API 서버로 보내고 굳이 데이터를 저장할 필요가 없다면 왜 많은 비용을 지불해야 할까요?

때때로, 사용자는 안정적인 인터넷 연결 걱정 없이 정보를 저장하고 나중에 보여줄 수 있는 앱을 원한다. 동의하지만 이러한 앱을 만드는 방법을 잘 모르신다면, 잘 하셨습니다. 지금 올바른 블로그 게시물을 읽고 계십니다. 🙂

이 게시물에서는 사용자가 자신의 가중치를 추적하고, 시간이 지남에 따라 저장된 데이터를 온디맨드 방식으로 시각화할 수 있는 React Native 앱을 구축하는 과정을 안내합니다. 이 모든 것이 나쁜 인터넷에 연결할 필요 없이!

## 테이블로 가져가야 할 것

아무것도 없어. 네가 뭘 가져오든 식탁에 와도 환영이야.

그러나 이 게시물은 사용자가 TypeScript 및 React Native에 익숙하고 데이터베이스에 대한 기본적인 이해도를 가지고 있다고 가정합니다. 위의 내용 중 하나 또는 모두를 잘 알지 못하더라도 계속 읽어보셔도 좋습니다. 하지만 어떤 것들은 너무 모호해 보일 수 있고 여러분의 목적에 대한 더 많은 연구가 필요할 것입니다.

저처럼 글을 읽기 전에 코드를 먼저 보고 싶어하는 성급한 사람들을 위해, 깃허브에 있는 모든 코드베이스를 소개합니다.

## 시작 중

어떤 사업에서 가장 먼저 해결해야 할 문제는 무엇입니까? 수익 흐름이죠?

틀렸어! 이름이야. 앱을 만들 때 다른 것보다 올바른 이름을 고르는 것이 항상 먼저 모든 사람이 집중해야 하는 주요 사항입니다.

물론, 제가 이 글을 쓰기 전에 웨이트리스라는 이름을 생각해 냈습니다. 여러분이 눈치채지 못하셨다면, 앱이 여러분의 몸무게를 추적한다는 사실에 대한 아주 영리한 단어플레이입니다. 대단하죠?

뭐? 그것보다 더 잘할 수 있을 것 같아? 음, 네가 노력하는 모습을 보고 싶어. 만약 네가 웨이트 트래킹 앱에 더 좋은 이름을 생각해 낼 수 있다면 트윗해 줘!

농담은 그만하고 이제 일을 시작합시다.

React Native(원본 대응)를 처음 사용하는 경우 시작하기 전에 몇 가지 도구를 사용하여 시스템을 설정해야 할 수 있습니다. RN의 공식 환경 설정 문서로 이동하여 설치 프로세스를 진행하십시오. 모든 것을 설정한 후에는 Return Native CLI에서 다음과 같은 간단한 실행만으로 보일러 플레이트 앱을 만들 수 있습니다.

```cpp
npx react-native init weightress --template react-native-template-typescript
```

이렇게 하면 `weightress`라는 이름의 새 폴더가 생성되고 그 안에 많은 파일이 삭제됩니다. 즐겨찾기 코드 편집기로 폴더를 열고 터미널 창에서 폴더 내부를 탐색합니다.

사용자의 데이터를 기기에 저장하기 위해, 우리는 수박DB를 사용할 것입니다. 먼저, 우리는 Ract Native 앱의 기본 패키지 매니저인 Warn을 사용하여 의존성으로 설치할 것입니다. 다음 명령을 실행합니다.

```undefined
cd weightress
yarn add @nozbe/watermelondb @nozbe/with-observables
yarn add --dev @babel/plugin-proposal-decorators
```

첫 번째 명령은 수박DB 관련 종속성을 설치하고 두 번째 명령은 코드에서 ES6 장식기 구문을 사용하는 데 필요한 개발 전용 패키지를 설치합니다.

패키지를 설치하는 것만으로는 문제가 되지 않습니다. 여전히 Babel에게 패키지를 알려야 합니다. `babel.config.js` 파일을 열고 다음과 같은 새 플러그인 속성을 추가하십시오.

```java
module.exports = {
  presets: ['module:metro-react-native-babel-preset'],
  plugins: [
    ["@babel/plugin-proposal-decorators", { "legacy": true }]
  ]
};
```

이제 빌드하는 플랫폼에 따라 DB 설정을 완료하려면 다른 지침을 따라야 합니다. iOS의 경우 다음 단계를 따르고 Android의 경우 다음 단계를 수행하십시오.

그래, 잘 살아서 잘 견뎌냈구나. 네이티브 개발을 위해 패키지를 설정하는 것은 다소 지루하기 때문에, 이 패키지의 유지 관리자들이 곧 자동 링크 기능을 구현할 수 있기를 바랍니다.

이제 장치를 개발 컴퓨터에 연결하거나 에뮬레이터에서 앱을 실행할 수 있습니다. 안드로이드 기기를 사용하겠지만 어느 쪽이든 단말기에서 yarn start를 실행하고, 다른 단말 창에서 yarnios나 yarn Android를 실행하면 된다.

그러면 장치/에뮬레이터에서 앱이 열리고 다음과 같은 화면이 나타납니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/react-native-starter-template.png?resize=450%2C800&ssl=1)

## 데이터 구조

새로운 앱이나 기능을 만들 때, 저는 처음부터 계획을 세우는 것을 좋아합니다. 그런 종류의 계획에 적합한 장소는 데이터베이스 레벨입니다. 저장할 데이터의 종류와 각 엔티티가 포함할 데이터를 파악하면 UI 및 UX를 구축하는 데 필요한 접근 방식을 훨씬 더 잘 이해할 수 있습니다.

저희 앱은 사용자가 원할 때 언제든지 무게를 입력할 수 있도록 하며, 데이터베이스에 저장할 것입니다. 그런 다음 나중에 기록 항목을 검색하여 사용자에게 표시합니다. 이 간단한 데이터의 경우 사용자로부터의 숫자 입력과 입력의 타임스탬프만 저장하면 됩니다.

또한 사용자가 자신의 체중을 기록할 때 추가 노트를 입력하도록 하는 것이 좋을 것이다. 아마도 그들은 그날의 기분, 무엇을 먹었는지 등을 적고 싶어할 것이다. 메모는 또한 특정 항목을 빠르게 검색하는 데 유용합니다. 이제 ThemberonDB를 사용하여 데이터 스토리지를 쉽게 만들어 보겠습니다.

`data/`라는 새 폴더와 `schema.ts`라는 새 파일을 만들고 다음 코드를 넣습니다.

```bash
import {appSchema, tableSchema} from '@nozbe/watermelondb';

export default appSchema({
  version: 1,
  tables: [
    tableSchema({
      name: 'weights',
      columns: [
        {name: 'weight', type: 'number'},
        {name: 'created_at', type: 'number'},
        {name: 'note', type: 'string', isOptional: true},
      ],
    }),
  ],
});
```

위의 코드에서, 우리는 세 개의 열이 있는 가중치라는 이름의 테이블에 대한 스키마를 설정했다. 노트 열에는 isOptional 속성이 true로 설정되어 있습니다. 사용자가 모든 항목에 대해 메모를 입력하도록 강요하고 싶지는 않습니다. 따라서 노트를 선택 사항으로 만들고 있습니다.

또한 `created_at` 열은 날짜임에도 불구하고 숫자 유형을 가집니다. 그 이유는 날짜가 수박DB에 Unix 타임스탬프로 저장되기 때문입니다. 그리고 isIndexed는 노트와 create_at에 대해서만 true로 설정되어 있습니다. 사용자가 특정 날부터 노트를 검색하거나 항목을 찾을 수 있도록 하기 때문입니다. 인덱싱은 해당 열이 관련된 경우 쿼리를 훨씬 빠르게 실행할 수 있도록 합니다.

데이터베이스 설정은 여기까지이지만 코드는 여전히 해당 데이터와 통신해야 합니다. 모델들이 들어오는 곳이죠. 모델은 원시 데이터를 코드로 표현한 것입니다. 일반적으로 테이블마다 하나의 모델이 있습니다. 이제 체중표 모형을 만들어 보겠습니다.

`data/` 폴더 내에 `weight.ts`라는 새 파일을 만들고 다음 파일로 채우십시오.

```coffeescript
import {Model} from '@nozbe/watermelondb';
import {field, readonly, date} from '@nozbe/watermelondb/decorators';

export default class Weight extends Model {
  static table = 'weights';

  @field('note') note;
  @field('weight') weight;
  @readonly @date('created_at') createdAt;
}
```

보시는 바와 같이, 데이터베이스 테이블에 대해 방금 만든 스키마의 미러입니다. 이것은 단지 데이터베이스 열과 모델 객체의 속성 사이에 지도를 만드는 것입니다. 여기서 한가지 마법 같은 것은 현재 타임스탬프를 자동으로 생성하여 새로운 항목이 데이터베이스에 삽입될 때마다 우리를 위해 삽입하는 `@date` 장식기이다.

설치 단계가 거의 끝났습니다. 이제 `data/` 폴더 안에 `database.ts`라는 다른 파일을 만들어 다음 내용을 넣으십시오.

```js
import {Database} from '@nozbe/watermelondb';
import SQLiteAdapter from '@nozbe/watermelondb/adapters/sqlite';

import Weight from './weight';
import schema from './schema';

const adapter = new SQLiteAdapter({
  schema,
});

export const database = new Database({
  adapter,
  modelClasses: [Weight],
  actionsEnabled: true,
});
```

여기서는 스키마와 방금 만든 모델을 가져와 스키마와 데이터 모델을 찾을 수 있는 새 데이터베이스 인스턴스를 만듭니다. 이 모든 것을 갖추었으므로 이제 로컬 데이터베이스에서 데이터를 생성, 읽기, 업데이트 및 삭제할 준비가 되었습니다.

그러나, 우려 사항을 구분하기 위해, 우리는 컴포넌트에서 데이터베이스에 직접 접근/조작하는 대신 모든 데이터 관련 코드를 한 곳에 보관할 것이다. helpers.ts라는 이름의 `data/` 폴더 안에 마지막 파일을 하나 만들고 다음 코드를 넣읍시다.

```js
import {database} from './database';

export type Weight = {
  createdAt?: Date;
  weight: string | number;
  note: string | undefined;
};

const weights = database.collections.get('weights');

export const observeWeights = () => weights.query().observe();
export const saveWeight = async ({weight, note}: Weight) => {
  await database.action(async () => {
    await weights.create((entry) => {
      entry.weight = Number(weight);
      entry.note = note;
    });
  });
};
```

여기서는 구성 요소 간에 전달되는 데이터가 적절하게 입력될 수 있도록 정의된 필드와 함께 `가중치` 유형을 정의하고 내보냅니다. 그런 다음, 우리는 아래 함수에서 사용하는 데이터베이스에서 우리의 `weights` 테이블/컬렉션에 대한 컨테이너를 정의하고 있다.

현재로서는, 우리는 단지 getter와 setter를 정의하고 있을 뿐이다. 관찰 가중치 기능은 가중치 테이블에 헤드라인 등록을 설정하며, 데이터베이스가 변경될 때마다 이 헤드라인에 대한 수신자가 변경 내용을 수신합니다. 변경사항은 작성 중인 새 항목, 업데이트 중인 기존 항목 또는 제거 중인 항목일 수 있습니다.

마지막으로 내보내는 기능은 가중치와 노트가 주어진 경우 데이터베이스의 가중치 표에 새 항목을 만드는 기능입니다. 한 가지 주의해야 할 점은 가중치 입력은 입력 필드에서 온다는 것이며, 입력 입력 입력은 일반적으로 React Native에서 문자열로 제공된다는 것입니다. 그러나 데이터베이스에서는 숫자로 저장하려고 하므로 저장하기 전에 문자열을 숫자로 변환하고 있습니다.

## 사용자 인터페이스

위의 모든 것은 백엔드 작업입니다. 백엔드 작업이란 앱은 작동하지만 각광을 받지 못하는 작업입니다. UI는 사용자가 마지막에 보는 것이므로, 멋진 작업을 넣읍시다.

예전과 마찬가지로, 우리는 처음부터 계획을 세우고 건설할 것입니다. 이제 UI 관점에서 잠시 앱을 생각해 보겠습니다. 사용자가 체중 항목을 삽입할 수 있는 양식과 이전에 삽입한 모든 체중을 표시하는 목록이라는 두 개의 주요 섹션으로 쉽게 구분할 수 있다. 좀 더 흥미롭게 만들기 위해, 기존의 단순한 목록 대신, 차트를 사용하여 과거 데이터를 보여줍시다.

쾌적한 UX를 위해 차트를 항상 볼 수 있도록 하고 입력 양식을 온 디맨드에만 표시하도록 합시다. 이를 위해 헤드러, 크리에이터, 차트 등 세 가지 요소로 나눠 앱을 분리할 수 있다. 이를 염두에 두고 `App.tsx` 파일을 열고 해당 파일의 모든 내용을 제거한 후 다음 코드를 넣으십시오.

```coffeescript
import React, {useState} from 'react';
import {SafeAreaView, ScrollView, StatusBar} from 'react-native';

import Chart from './components/chart';
import Header from './components/header';
import Creator from './components/creator';

const App = () => {
  const [showCreator, setShowCreator] = useState<boolean>(false);
  return (
    <>
      <StatusBar />
      <SafeAreaView>
        <ScrollView contentInsetAdjustmentBehavior="automatic">
          <Header onOpenCreator={() => setShowCreator(true)} />
          <Creator
            isCreatorVisible={showCreator}
            onHideCreator={() => setShowCreator(false)}
          />
          <Chart />
        </ScrollView>
      </SafeAreaView>
    </>
  );
};

export default App;
```

이것은 꽤 간단하다. 우리는 우리가 계획했던 3가지 구성 요소를 들여와 `상태 표시줄` 구성 요소를 맨 위에 놓고 `세이프 에어리어 뷰` 구성 요소에 모든 것을 포장하여 우리의 콘텐츠가 상태 표시줄과 겹치거나 무서운 높이에 의해 끊어지지 않도록 하고 있다. 그런 다음 내부의 콘텐츠가 화면의 수직 높이에 넘칠 경우 사용자가 아래로 스크롤할 수 있는 또 다른 래퍼인 스크롤뷰가 있다.

사용자에게 "Creator"가 표시되는지 여부를 결정하는 `showCreator`라는 로컬 부울 상태가 표시됩니다. Header 구성 요소를 렌더링할 때, 우리는 기능 소품 on OpenCreator를 전달하는데, 이는 호출될 때 ShowCreator를 true로 설정하기만 하면 된다.

Creator 구성 요소에서는 show Creator 상태와 show Creator를 false로 설정하여 크리에이터를 숨기는 기능을 전달하고 있습니다.

마지막으로, 우리는 받침이 필요 없는 `차트` 구성 요소를 가지고 있습니다. 어느 시점에서는 사용자 상호 작용을 통해 Header 구성 요소가 on OpenCreator를 트리거하고 이를 통해 사용자가 자신의 가중치를 입력할 수 있는 Creator를 볼 수 있습니다.

이제 이러한 각 구성 요소의 내부를 자세히 살펴보도록 하겠습니다. 컴포넌트라는 이름의 새 폴더를 만들고 내부에 chart.tsx, creator.tsx, 헤더.tsx의 세 개의 새 파일을 만듭니다.

### '헤더' 구성 요소

`components/header.tsx` 파일을 열고 다음 코드를 입력합니다.

```js
import React, {FC} from 'react';
import {View, Text, TouchableHighlight} from 'react-native';
import {headerStyles} from './styles';

const Header: FC<{onOpenCreator: () => void}> = ({onOpenCreator}) => {
  return (
    <>
      <View style={headerStyles.container}>
        <Text style={headerStyles.headerTitle}>Weightress</Text>
        <TouchableHighlight
          style={headerStyles.addButton}
          onPress={() => onOpenCreator()}>
          <Text>+ Add</Text>
        </TouchableHighlight>
      </View>
    </>
  );
};

export default Header;
```

앱 이름인 Weightress만으로 헤더 텍스트를 렌더링하는 작은 구성 요소이며, 누르면 `onOpenCreator` 기능 소품이 트리거됩니다. 버튼은 터치식 하이라이트(Touchable Highlight) 구성 요소를 사용하여 렌더링되지만 버튼 또는 누름식(Pressable) 구성 요소를 사용하여 재생할 수 있습니다.

아직 생성하지 않은 파일에서 가져온 세 가지 구성 요소에 모두 스타일을 적용하기 위해 `헤더 스타일`을 사용하고 있습니다. 그러면 `components/style.ts` 파일을 생성하고 다음 코드를 입력해 보겠습니다.

```js
import {StyleSheet} from 'react-native';

export const primaryColor = '#FB8C00';

export const headerStyles = StyleSheet.create({
  container: {
    alignItems: 'center',
    paddingVertical: 20,
    flexDirection: 'row',
    paddingHorizontal: 15,
    justifyContent: 'space-between',
  },
  addButton: {
    borderColor: primaryColor,
    paddingHorizontal: 20,
    paddingVertical: 8,
    borderRadius: 3,
    borderWidth: 1,
  },
  headerTitle: {
    fontSize: 25,
    fontWeight: 'bold',
    borderLeftWidth: 3,
    paddingLeft: 10,
    borderLeftColor: primaryColor,
  },
});
```

보시다시피 헤더 스타일은 세 가지 속성을 가진 리액트 네이티브 스타일시트입니다. 각 속성은 한 구성 요소의 스타일을 포함합니다. 컨테이너는 전체 헤더 주위에 약간의 공간을 두고, `content: `space-between`을 사용하여 헤더 텍스트와 추가 버튼을 화면의 반대편에 배치하도록 합니다.

버튼 추가는 오른쪽 버튼 주위에 공백을 두고 텍스트 주위에 둥근 테두리(너무 둥글지는 않은)를 추가해 버튼 에스크해 보이게 한다. 테두리 색상은 `기본 색상` 변수를 사용하여 설정됩니다. 메인 컬러 액센트를 분리하면 다른 컴포넌트 스타일로 재사용할 수 있으며 앱의 브랜딩을 한 곳에서 쉽게 변경할 수 있습니다.

마지막으로, 로고처럼 보이게 하기 위해 테두리가 있는 제목 텍스트에 사용할 스타일이 있습니다. 아직 저희 앱에서 볼 수 없기 때문에 위의 코드로 어떻게 보일지 스크린샷을 보여드리겠습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/header-component-preview.png?resize=730%2C154&ssl=1)

### '생성자' 구성 요소

`components/creator.tsx` 파일을 열고 다음 코드를 입력합니다.

```xml
import React, {FC, useState} from 'react';
import {
  Button,
  Modal,
  Text,
  TextInput,
  TouchableHighlight,
  View,
} from 'react-native';
import {saveWeight} from '../data/helpers';
import {creatorStyles, primaryColor} from './styles';

const Creator: FC<{
  isCreatorVisible: boolean;
  onHideCreator: () => void;
}> = ({onHideCreator, isCreatorVisible}) => {
  const [isSaving, setIsSaving] = useState<boolean>(false);
  const [weight, setWeight] = useState<string>('');
  const [note, setNote] = useState<string>('');

  const handleSavePress = async () => {
    setIsSaving(true);
    await saveWeight({weight, note});
    // hide modal
    onHideCreator();
    // Clear out the inputs
    setWeight('');
    setNote('');
    // Make button active again
    setIsSaving(false);
  };

  return (
    <Modal animationType="slide" transparent={true} visible={isCreatorVisible}>
      <View style={creatorStyles.centeredView}>
        <View style={creatorStyles.modalView}>
          <View style={creatorStyles.topActions}>
            <Text>Add your weight</Text>
            <TouchableHighlight
              onPress={() => {
                onHideCreator();
              }>
              <Text style={creatorStyles.topCloseButton}>×</Text>
            </TouchableHighlight>
          </View>
          <TextInput
            style={creatorStyles.input}
            placeholder="Your weight"
            keyboardType="decimal-pad"
            onChangeText={(text) => setWeight(text)}
            value={weight}
          />
          <TextInput
            placeholder="Additional note (optional)"
            style={creatorStyles.input}
            onChangeText={(text) => setNote(text)}
            value={note}
          />
          <Button
            title="Save"
            disabled={isSaving}
            color={primaryColor}
            onPress={handleSavePress}
          />
        </View>
      </View>
    </Modal>
  );
};

export default Creator;
```

이것은 헤더 성분보다 조금 더 고소한 것 같아요, 그렇죠? 지역 주부터 짐을 풀자. 우리는 이즈 세이빙(is saving), 중량(weight), 노트(note) 세 가지를 가지고 있다.

데이터베이스에 가중치를 저장하는 도우미 기능은 비동기 기능이기 때문에 데이터 저장소를 요청하는 것과 완료하는 것 사이에 약간의 지연이 있을 수 있습니다. 그런 가운데 `절약`이 이용될 수 있는 배후 활동을 사용자에게 알리고자 한다. 나머지 두 개에는 사용자의 입력이 포함됩니다.

우리가 `크리에이터`를 온 디맨드로만 보여준다고 했던 거 기억나? 우리는 리액트 네이티브의 `모달` 구성 요소를 통해 그것을 달성한다. `진실`을 그 구성 요소의 `보이는` 지지대로 넘기는 것은 그것을 시야에 들어오게 할 것이고, `거짓`은 그것을 꺼낼 것이다. 우리는 `애니메이션`을 전달함으로써 전환을 보이고 기분 좋게 만든다.= "슬라이드" 받침대를 입력합니다.

모듈(Modal) 부품은 화면 전체를 커버하지만 입력값이 2개인 소형 폼의 경우 부동산이 그리 많이 필요하지 않다. 그래서 우리는 콘텐츠를 두 개의 `보기` 구성 요소로 포장하고, 포장지는 플렉스박스 마법을 사용하여 화면 아래쪽에 있는 모든 것을 담습니다.

그것을 성취할 수 있는 스타일을 살펴봅시다. styles.ts 파일로 돌아가서 새 스타일시트를 만듭니다.

```js
import {Dimensions, StyleSheet} from 'react-native';
// previous code.....
const windowDim = Dimensions.get('window');
export const windowHeight = windowDim.height;
export const creatorStyles = StyleSheet.create({
  centeredView: {
    flex: 1,
    justifyContent: 'flex-end',
    backgroundColor: 'rgba(255, 255, 255, 0.6)',
  },
  modalView: {
    backgroundColor: '#FFFFFF',
    borderTopLeftRadius: 20,
    borderTopRightRadius: 20,
    padding: 10,
    height: windowHeight / 2,
    shadowColor: '#cacaca',
    shadowOffset: {
      width: 0,
      height: 1,
    },
    shadowOpacity: 0.1,
    shadowRadius: 2,
    elevation: 1,
  },
}
```

central view는 안에 있는 모든 것을 캡슐화한 상위 래퍼로, 반투명 배경이다. 컨텐츠 정당화: 플렉스 엔드(Flex-end)는 안에 있는 모든 요소를 요소의 하단에 배치하도록 보장합니다. 내부 뷰는 흰색 바탕에 화면 높이의 절반인 modal view 스타일을 사용한다.

화면의 절반을 정확히 파악하려면 창의 높이와 폭 및 기타 크기 특성에 접근할 수 있는 리액트 네이티브의 치수 도우미를 사용해야 한다. 우리는 또한 사물을 멋지고 부드럽게 보이기 위해 요소에 그림자와 테두리를 추가하고 있습니다.

네, 구성 요소를 다시 살펴보겠습니다. 모달 안에서 먼저 다음과 같은 것을 볼 수 있습니다.

```xml
         <View style={creatorStyles.topActions}>
            <Text>Add your weight</Text>
            <TouchableHighlight
              onPress={() => {
                onHideCreator();
              }>
              <Text style={creatorStyles.topCloseButton}>×</Text>
            </TouchableHighlight>
          </View>
```

이렇게 하면 모달 내부에 헤더 텍스트가 추가되고 오른쪽 버튼을 누르면 `onHideCreator` prop 기능이 실행된다. App.tsx에서 이 정보를 전달하면 사용자가 모달 닫기를 허용한다는 점을 기억하십시오. `style.ts` 파일에서 이 항목의 스타일을 생성해 보겠습니다.

```js
export const creatorStyles = StyleSheet.create({
  // previous code...
  topActions: {
    flexDirection: 'row',
    alignItems: 'center',
    justifyContent: 'space-between',
  },
  topCloseButton: {
    padding: 10,
    color: '#494949',
    fontWeight: 'bold',
    fontSize: 25,
  },
```

이 제품은 플렉스박스를 사용하여 래퍼를 두 개의 수평 섹션으로 분할하는 헤더(Header) 구성 요소와 유사한 설정을 가지고 있다. 오른쪽에 있는 닫기 단추는 클릭이 용이하도록 주위에 공간이 있으며, 텍스트는 왼쪽에 남아 있습니다.

이제 모달의 주요 내용은 다음과 같습니다.

```xml
          <TextInput
            style={creatorStyles.input}
            placeholder="Your weight"
            keyboardType="decimal-pad"
            onChangeText={(text) => setWeight(text)}
            value={weight}
          />
          <TextInput
            placeholder="Additional note (optional)"
            style={creatorStyles.input}
            onChangeText={(text) => setNote(text)}
            value={note}
          />
          <Button
            title="Save"
            disabled={isSaving}
            color={primaryColor}
            onPress={handleSavePress}
          />
```

여기에는 텍스트 입력과 버튼 두 개가 있습니다. 첫 번째 "TextInput"은 "키보드"를 설정했기 때문에 숫자 입력을 받습니다.="decimal-pad" propod를 입력하면 "weight" 상태로 연결됩니다. 두 번째는 모든 추가 노트를 위한 것이므로 노트 상태와 연결됩니다. 마지막으로, 누르면 `핸들세이브프레스` 기능이 해제되는 `버튼`이 있습니다.

입력을 저장하는 동안 버튼을 사용하지 않도록 설정하려면 `disabled={isSaving}`을(를) 설정하고 브랜딩과 일치하도록 `color={primaryColor}을(를) 아직 이 필드의 스타일을 정의하지 않았으므로 `style.ts` 파일로 돌아가서 이 파일을 추가해 보겠습니다.

```js
export const creatorStyles = StyleSheet.create({
// previous code...
  input: {
    height: 50,
    borderWidth: 1,
    borderRadius: 3,
    marginBottom: 10,
    paddingVertical: 10,
    paddingHorizontal: 15,
    borderColor: '#c9c9c9',
  },
});
```

우리는 단지 필드 주변의 간격을 더하고 테두리를 추가하여 보기 좋게 만들고 있습니다. 이제 `핸들세이브프레스`가 무엇을 하는지 알아보자.

```undefined
 const handleSavePress = async () => {
    setIsSaving(true);
    await saveWeight({weight, note});
    // hide modal
    onHideCreator();
    // Clear out the inputs
    setWeight('');
    setNote('');
    // Make button active again
    setIsSaving(false);
  };
```

실행되면 먼저 상태 `is Save(저장 중)`가 true(참)로 전환되어 저장 버튼이 비활성화됩니다. 그런 다음 상태로부터의 가중치 및 노트 입력과 함께 `저장 무게` 함수를 호출한다. 데이터를 비동기식으로 수박DB에 저장합니다.

완료되면 먼저 `onHideCreator` 기능 소품을 발사하는데, 기억하시면 모달 자체를 숨깁니다. 그런 다음 입력 정보를 삭제하여 모달을 다시 열 때 사용자가 이전에 삽입한 가중치와 노트 대신 빈 입력 필드를 볼 수 있도록 합니다. 마지막으로, 사용자가 다음에 모달을 열 때 저장 버튼을 다시 활성화하기 위해 `isSave`를 false로 다시 전환합니다.

다시 말씀드리지만, 저희 앱에서는 아직 볼 수 없기 때문에 여기 여러분의 부리를 적시는 스크린샷이 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/creator-component-preview.png?resize=730%2C573&ssl=1)

### '차트' 성분

React Native 에코시스템에는 웹 또는 네이티브 플랫폼에서 사용할 수 있는 훌륭한 차트 작성 및 데이터 viz 라이브러리가 없습니다. 그러나, 우리의 필요에 따라, 반응형 차트의 키트는 모든 것을 즉시 제공합니다. 라이브러리는 ract-native-svg 라이브러리에 따라 다르므로 yarn add ract-native-svg ract-native-chart-kit를 실행하여 두 가지 모두를 설치하자.

이제 `components/chart.tsx` 파일을 열고 다음 코드를 입력합니다.

```js
import React, {FC} from 'react';
import withObservables from '@nozbe/with-observables';
import {LineChart} from 'react-native-chart-kit';

import {observeWeights, Weight} from '../data/helpers';
import {chartConfig, chartStyles, windowWidth} from './styles';

const Chart: FC<{weights: Weight[]}> = ({weights}) => {
  if (weights.length < 1) {
    return null;
  }

  const labels: string[] = [];
  const data: number[] = [];
  weights.forEach((w) => {
    labels.push(`${w?.createdAt.getDate()}/${w.createdAt.getMonth() + 1}`);
    data.push(w.weight);
  });
  return (
    <LineChart
      bezier
      height={250}
      width={windowWidth - 30}
      chartConfig={chartConfig}
      style={chartStyles.chart}
      data={labels, datasets: [{data}]}
    />
  );
};

const enhanceWithWeights = withObservables([], () => ({
  weights: observeWeights(),
}));

export default enhanceWithWeights(Chart);
```

먼저, 우리는 `차트` 구성 요소를 일련의 가중치를 수신하는 기능 구성 요소로 설정한다. 배열이 비어 있으면 아무것도 렌더링하지 않습니다. 그렇지 않으면 가중치 배열을 순환하고 각 항목의 날짜를 포함하는 `레이블` 배열을 `dd/mm` 형식으로 구축한다.

우리는 또한 모든 가중치 값을 `데이터` 배열에 `라벨` 배열과 같은 순서로 저장하고 있다. 우리는 원시 데이터를 데이터베이스에서 `라인 차트` 구성요소가 이해하고 시각화할 수 있는 이 구조로 변환하고 있다. 이 구성 요소는 반응형 차트 키트 라이브러리에서 가져온 `라인 차트` 구성 요소만 렌더링합니다.

"데이터" 소품은 우리가 방금 만든 "라벨"과 "데이터" 어레이를 통과하는 것이다. 베지어 소품은 선차트가 곡선 형태로 외삽되게 한다. 높이는 하드코딩되지만 너비는 윈도우에서 파생되는 높이와 너비를 지나고 있다.widthes.ts 파일에서 가져온 width 변수와 chartConfig, chartStyles.

`style.ts` 파일로 돌아가서 그것들이 무엇인지 봅시다.

```js
export const windowWidth = windowDim.width;
export const chartStyles = StyleSheet.create({
  chart: {
    marginLeft: 15,
    borderRadius: 10,
  },
});

export const chartConfig = {
  backgroundColor: primaryColor,
  backgroundGradientFrom: primaryColor,
  backgroundGradientTo: '#FFA726',
  decimalPlaces: 2,
  color: (opacity = 1) => `rgba(255, 255, 255, ${opacity})`,
  labelColor: (opacity = 1) => `rgba(255, 255, 255, ${opacity})`,
  style: {
    borderRadius: 16,
  },
  propsForDots: {
    r: '6',
    strokeWidth: '2',
    stroke: '#ffa726',
  },
};
```

`Dimension` 모듈에서 장치의 창 너비를 내보내고 있으며 차트의 너비는 전체 너비보다 30px 적게 됩니다. 그런 다음 차트 스타일즈에서는 왼쪽의 15px 여백을 부여하여 가로로 화면 중앙에 15px를 배치하고 어느 한쪽에 멋진 15px를 배치한다. 우리는 또한 그것의 경계선 주위에 약간의 반경을 추가하고 있다.

대부분의 스타일링은 리액티브 네이티브 스타일이 아닌 차트 구성(chartConfig) 소품을 통해 이루어진다. 우리는 `primary Color` 억양에서 시작하는 배경의 그라데이션으로 차트를 구성하고 있다. 나머지 속성은 차트 내부에 표시된 내용의 다양한 모양과 색상에 대한 것입니다. 라이브러리의 공식 설명서를 통해 차트를 구성하는 더 많은 방법을 찾을 수 있습니다.

부품 내부로 돌아가서, 우리가 다른 두 부품과 같이 "차트" 부품을 직접 수출하는 것은 아니라는 것을 알아두십시오. 대신 HOC(고차 구성 요소)로 포장하여 다음 래퍼 버전을 내보냅니다.

```coffeescript
const enhanceWithWeights = withObservables([], () => ({
  weights: observeWeights(),
}));

export default enhanceWithWeights(Chart);
```

이곳이 수박DB의 힘이 깃든 곳이다. `With Observables` HOC는 React 구성요소를 데이터베이스의 데이터에 직접 실시간으로 연결할 수 있는 방법을 제공한다.

여기서 우리는 `weights() 관찰 도우미 기능을 적용하고, 우리의 `chart` 구성 요소를 HOC로 감싸면 구성 요소가 자동으로 `weight`라는 이름의 소품을 받게 된다. 이 소품에는 데이터베이스의 모든 가중치 항목이 포함되며, 새 데이터가 추가되거나 기존 데이터가 업데이트 또는 제거될 때마다 구성 요소가 최신 데이터로 다시 렌더링됩니다.

## 그것을 한 바퀴 돌다.

이 모든 것을 갖추면, 우리는 마침내 기기나 에뮬레이터에서 우리의 앱을 열고 그것을 확인할 수 있다. 여기 제가 가지고 노는 비디오가 있습니다.

앱을 닫고 다시 열면 이전에 저장한 모든 가중치가 수박DB 덕분에 차트에 표시됩니다.

## 크게 가든지 아니면 집으로 가든지.

우선, 여기까지 온 것에 대해 여러분 자신의 등을 한 번 두드려 보세요. 하지만 재미있는 기차는 여기서 멈출 필요가 없어. 이 앱은 더 많은 것을 제공할 수 있습니다.

다음은 React Native, 데이터 구조, 스토리지 및 아키텍처에 대해 자세히 알아보고, 이 과정에서 이 프로젝트를 보다 세련된 프로덕션 지원 앱으로 전환할 수 있는 기능 목록입니다.

### 기간 선택기

시간이 지남에 따라 사용자가 점점 더 많은 데이터를 입력하면 차트 보기가 너무 붐벼서 모든 데이터를 표시할 수 없게 됩니다. 사용자가 기간을 선택하고 데이터베이스에서 데이터를 필터링하여 해당 기간의 가중치만 가져오고 표시하도록 허용

### 데이터 가져오기/내보내기

오프라인 앱의 가장 일반적인 문제 중 하나는 데이터 동기화입니다. 사용자는 전화기를 분실하거나, 파손하거나, 재설정할 수 있습니다. 장치 저장 데이터가 영구히 손실되는 다른 시나리오가 많이 있습니다. 이러한 혼란을 피하기 위해 오프라인 앱은 데이터를 CSV 또는 PDF 파일과 같은 휴대용 형식으로 내보낼 수 있는 방법을 제공해야 합니다.

이전에 내보낸 파일에서 데이터를 가져올 수 있는 가져오기 기능도 있어야 합니다. 이 기능은 사용자가 새 단말기를 가져와서 새 단말기에서 앱을 계속 사용하고 싶을 때 특히 유용합니다.

### 알림 알림

사용자가 그날 체중을 기록하지 않을 경우 앱이 종료 시 알림 푸시 알림을 보낼 수 있다면 매우 편리할 것입니다. 물론 원치 않는 알림을 푸시하는 것은 매우 성가실 수 있으므로 설정 옵션을 구축하여 사용자가 설정 또는 해제할 수 있습니다.

위의 세 가지 아이디어를 구현하고, 여러분이 원하는 어떤 아이디어라도 추가한다면, 저는 여러분이 이 아이디어를 다음 대형 앱으로 바꿀 수 있다고 확신합니다. @foysalit을 스토어에 게시하거나 이러한 기능 중 하나 이상을 구축하는 경우 트윗하는 것을 잊지 마십시오.