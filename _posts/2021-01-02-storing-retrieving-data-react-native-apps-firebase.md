---
layout: post
title: "Firebase를 사용하여 React Native 앱에 대한 데이터 가져오기"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/02/storing-retrieving-data-react-native-apps-firebase.jpeg
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/02/storing-retrieving-data-react-native-apps-firebase.jpeg?fit=730%2C486&ssl=1)

편집자 참고: 이 튜토리얼은 2021년 1월 27일에 업데이트되었습니다.

사용자 생성 데이터를 저장하고 검색하는 것은 오늘날 모바일 및 웹 앱에서 매우 흔하게 볼 수 있으며, 따라서 모바일 및 웹 개발자에게 데이터 저장 기능을 제공하는 다양한 서비스가 제공되고 있습니다.

이들 서비스 중에는 구글의 파이어베이스도 있다. Firebase는 BaaS(Backend-as-a-Service)로, 웹 및 모바일 개발자가 사용자 인증과 같은 일반적인 백엔드 작업을 수행하거나 유지보수할 필요가 없는 데이터베이스를 생성할 수 있도록 지원합니다.

반면 리액트 네이티브는 개발자들이 널리 사용되는 프런트엔드 프레임워크인 리액트에 대한 지식을 활용해 안드로이드와 iOS 기기 모두에서 작동하는 네이티브 앱을 만들 수 있는 플랫폼이다. React Native는 개발자들이 특정 앱의 iOS 버전과 Android 버전 모두에 대해 서로 다른 두 개의 코드베이스를 관리할 필요가 없기 때문에 생산성을 향상시킨다.

이 문서에서는 Firebase를 사용하여 사용자 생성 데이터를 저장, 검색 및 업데이트하는 방법에 대해 설명합니다. 이 기사의 말미에는 Firebase에서 제공하는 데이터베이스 서비스를 사용하여 데이터베이스에 서로 다른 작업관리 항목을 저장하고 검색하는 작업관리(To-Do-Do) 앱이 구축될 예정입니다.

이 기사를 완벽하게 따르기 위해서는, 여러분은 반응과 반응 네이티브에 대한 기본적인 이해가 있어야 합니다.

## 반응 네이티브 설정

먼저 새로운 React Native 프로젝트를 만들어 보겠습니다. 기본 설정에 대한 지침을 보려면 공식 기본 문서를 방문하십시오.

작업이 완료되면 이제 새 React Native 프로젝트를 생성할 수 있습니다.

```java
npx react-native init ToDoApp
```

또한 프로젝트에서 사용할 종속성을 설치해야 합니다. 이러한 종속성은 다음과 같습니다.

- 파이어베이스
- 반응 검사함

터미널에서 다음 명령을 실행하여 이러한 종속성을 설치합니다.

```undefined
npm install --save firebase
npm install react-native-check-box --save
```

설치가 끝났으니 이제 앱을 만들 수 있습니다!

## 방화벽 설정

코드 작성을 시작하기 전에 Firebase가 앱과 완벽하게 작동하도록 몇 가지 설정을 해야 합니다.

루트 디렉터리에 `src` 폴더를 생성하고 이 폴더에 `config.js`라는 파일을 생성하십시오.

이제, 파이어베이스 웹사이트를 방문하세요.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/02/firebase-homepage.jpeg?resize=730%2C327&ssl=1)

시작 단추 를 클릭하면 새 프로젝트를 만들 수 있는 페이지로 이동합니다. 새로운 프로젝트를 만든 보드는 페이지 이미지를 아래와 비슷한도록 주의해야 한다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/02/firebase-get-started-page.jpeg?resize=730%2C321&ssl=1)

사이드 메뉴에서 Database(데이터베이스) 옵션을 선택합니다. 이 작업을 마친 후에는 Firebase에서 제공하는 두 가지 데이터베이스 옵션 중에서 선택할 수 있습니다. 실시간 데이터베이스 및 Firestore는 둘 다 유사한 기능을 가지고 있습니다.

사용할 데이터베이스 서비스를 모를 경우 데이터베이스를 선택할 때 Firebase로 이 가이드를 확인할 수 있습니다. 이 기사에서는 실시간 데이터베이스를 활용하겠습니다.

실시간 데이터베이스 옵션을 선택하면 데이터베이스에 대한 보안 규칙을 설정하는 모달이 팝업됩니다. 테스트 모드에서 시작을 선택합니다. 이제 데이터베이스의 데이터를 표시하는 페이지에 있어야 합니다(현재 비어 있음).

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/02/empty-database.jpeg?resize=730%2C364&ssl=1)

데이터베이스가 생성되었으므로 Firebase 프로젝트에서 몇 가지 정보(Firebase 구성)를 가져와야 합니다. 이러한 정보나 데이터는 우리의 코드를 우리의 파이어베이스 프로젝트에 연결하는 데 도움이 될 것이다.

Firebase 구성 세부 정보를 보려면 프로젝트 개요 페이지로 돌아가서 Firebase 프로젝트에 웹 앱을 추가하십시오. 앱을 등록한 후 JavaScript 개체 형태로 구성 세부 정보를 얻어야 합니다.

```undefined
const firebaseConfig = {
  apiKey: 'AIzaXXXXXXXXXXXXXXXXXXXXXXX',
  authDomain: 'test-XXXX.firebaseapp.com',
  databaseURL: 'https://test-XXXXXX.firebaseio.com',
  projectId: 'test-XXXX',
  storageBucket: 'test-XXXX.appspot.com',
  messagingSenderId: 'XXXXXXX',
  appId: "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
};
```

이제 `config.js` 파일을 열고 Firebase를 가져옵니다. Firebase를 가져온 후에는 Firebase 구성 세부 정보를 파일에 복사하여 붙여넣어야 합니다.

```js
import Firebase from "firebase";
const firebaseConfig = {
    apiKey: 'AIzaXXXXXXXXXXXXXXXXXXXXXXX',
    authDomain: 'test-XXXX.firebaseapp.com',
    databaseURL: 'https://test-XXXXXX.firebaseio.com',
    projectId: 'test-XXXX',
    storageBucket: 'test-XXXX.appspot.com',
    messagingSenderId: 'XXXXXXX',
    appId: "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    measurementId: "X-XXXXXXXXXX"
  };
```

이제 Firebase에서 가져온 구성 세부 정보를 사용하여 Firebase 앱을 초기화하기만 하면 됩니다. 작업이 완료되면 Firebase에서 제공하는 앱에 대한 데이터베이스 서비스를 참조하여 내보낼 것입니다.

```cpp
const app = Firebase.initializeApp(firebaseConfig);
export const db = app.database();
```

이제 실시간 데이터베이스를 읽고 쓸 수 있습니다!

## 반응 구성 요소 생성

코드 작성을 시작하겠습니다!

> 참고: 모든 코드를 'app.js' 파일에 씁니다.

우선, 우리는 해야 할 일을 표시하고 해야 할 일의 상태를 관리하기 위한 구성요소가 필요합니다. 이 구성 요소는 특정 작업 수행 여부를 나타내는 역할을 합니다. 우리는 이 구성 요소를 `ToDoItem`이라고 부를 것입니다.

```js
import React from 'react';
import {
  StyleSheet,
  Alert,
  View,
  Text,
  Button,
  ScrollView,
  TextInput,
} from 'react-native';
import CheckBox from 'react-native-check-box';

const ToDoItem = () => {
  return (
    <View>
      <CheckBox
        checkBoxColor="skyblue"
      />
      <Text>
        A random To-Do item
      </Text>
    </View>
  );
};
```

보다시피, 우리는 relative-native-contact 박스에서 체크박스 구성 요소를 가져왔습니다. 이 확인란은 특정 작업관리 태스크가 완료되었는지 여부를 나타내는 데 사용됩니다.

이제 구성 요소를 스타일링해 보겠습니다! React Native는 여러 요소에 대한 스타일을 정의하는 데 사용할 수 있는 스타일시트와 함께 제공됩니다.

```undefined
const styles = StyleSheet.create({
  todoItem: {
    flexDirection: 'row',
    marginVertical: 10,
    alignItems: 'center',

  },
  todoText: {
    borderColor: '#afafaf',
    paddingHorizontal: 5,
    paddingVertical: 7,
    borderWidth: 1,
    borderRadius: 5,
    marginRight: 10,
    minWidth: "50%",
    textAlign: "center"
  },
});
```

이제 다음과 같이 각 요소의 스타일 속성에 이러한 스타일을 적용할 수 있습니다.

```xml
<View style = {styles.todoItem}></View>
```

확인란이 확인되면 ToDoItem 구성 요소를 다르게 스타일링하려고 합니다. 이를 위해, 우리는 `doneState`라는 상태를 만들어 작업이 완료되었는지 여부에 관계없이 반응에서 `useState` 후크를 사용하여 작업 수행 상태를 관리할 것이다.

먼저 use State Hook을 Return과 함께 다음과 같이 가져옵니다.

```coffeescript
import React, {useState} from 'react';
```

이제 use State Hook을 수입하면 다음과 같이 우리의 상태를 만들 수 있습니다.

```cpp
const [doneState, setDone] = useState(false);
```

후크에 익숙하지 않은 경우 대응 문서를 확인해야 합니다.

기본적으로 초기 값이 거짓인 doneState라는 상태를 만들었다. 향후 어느 시점에 doneState 값을 업데이트하여 true로 설정하려는 경우 setDone을 항상 사용할 수 있습니다.

```bash
setDone(true);
```

체크박스는 isChecked라는 소품이 함께 제공돼 체크박스를 언제 체크해야 하는지 제어할 수 있다. 이 경우 doneState 값이 true일 때 작업관리 태스크가 수행되었을 때 확인란을 선택하려고 합니다.

확인란 구성요소의 isChecked는 doneState 값에 따라 달라지기 때문에 확인란이 클릭될 때마다 doneState 값을 현재 값과 반대로 변경하고자 합니다.

이 작업은 체크박스 구성 요소와 함께 제공되는 onClick(온클릭) 소품으로 수행할 수 있습니다. 확인란의 onClick 이벤트를 위해 onCheck라는 이벤트 핸들러를 생성하겠습니다.

```coffeescript
const onCheck = () => {
    setDone(!doneState);
};
```

이제 앞서 지적했듯이 일단 확인만 하면 ToDoItem 구성 요소를 다르게 스타일링하고 싶습니다. 이 옵션을 선택하면 `작업 항목`의 불투명도가 감소하고 확인란이 비활성화됩니다. 다음을 구현해 보겠습니다.

```js
return (
    <View style={styles.todoItem}>
      <CheckBox
        checkBoxColor="skyblue"
        onClick={onCheck}
        isChecked={doneState}
        disabled={doneState}
      />
      <Text style={[styles.todoText, {opacity: doneState ? 0.2 : 1}]}>
        A random To-Do item
      </Text>
    </View>
  );
};
```

이제 `ToDoItem` 구성 요소의 기본 부분이 준비되었으니 앱의 홈 스크린 역할을 할 `앱` 구성 요소를 만들어보자. 당사의 `App` 구성 요소는 클래스 기반 구성 요소가 되겠지만, 원하는 경우 기능을 갖춘 구성 요소로 만들 수 있습니다.

우리의 앱 구성 요소인 모든 할 일 항목이 포함된 `토두`와 현재 추가될 `현재 할 일`의 두 가지 상태가 필요하다. 또한 작업관리 항목 목록을 포함하는 View 구성 요소, 사용자가 새로운 작업관리 항목을 입력하는 TextInput 구성 요소, 작업관리 항목을 추가하는 버튼 및 데이터베이스를 지우는 다른 버튼을 추가하여 모든 작업관리 항목을 제거해야 합니다.

약간의 스타일링을 통해 당사의 `앱` 구성 요소는 다음과 같을 수 있습니다.

```php
class App extends React.Component {
  constructor() {
    super();
    this.state = {
      todos: {},
      presentToDo: '',
    };
  }
  componentDidMount() {
  }
  addNewTodo() {
  }
  clearTodos() {
  }
  render() {
    return (
      <ScrollView
        style={styles.container}
        contentContainerStyle={styles.contentContainerStyle}>
        <View>
          {/*Empty view: will contain to-do items soon*/}
        </View>
        <TextInput
          placeholder="Add new Todo"
          value={this.state.presentToDo}
          style={styles.textInput}
          onChangeText={e => {
            this.setState({
              presentToDo: e,
            });
          }
          onSubmitEditing = {this.addNewTodo}
        />
        <Button
          title="Add new To do item"
          onPress={this.addNewTodo}
          color="lightgreen"
        />
        <View style={marginTop: 20}>
          <Button title="Clear todos" onPress={this.clearTodos} color="red" />
        </View>
      </ScrollView>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: 'white',
  },
  contentContainerStyle: {
    alignItems: 'center',
  },
  textInput: {
    borderWidth: 1,
    borderColor: '#afafaf',
    width: '80%',
    borderRadius: 5,
    paddingHorizontal: 10,
    marginVertical: 20,
    fontSize: 20,
  },
  todoItem: {
    flexDirection: 'row',
    marginVertical: 10,
    alignItems: 'center',
  },
  todoText: {
    borderColor: '#afafaf',
    paddingHorizontal: 5,
    paddingVertical: 7,
    borderWidth: 1,
    borderRadius: 5,
    marginRight: 10,
    minWidth: '50%',
    textAlign: 'center',
  },
});
```

## Firebase 데이터베이스 사용

이제 몇 가지 Firebase 코드를 사용하여 작업을 시작할 준비가 되었습니다.

먼저 `config.js` 파일에서 데이터베이스를 가져와야 합니다.

```coffeescript
import {db} from './src/config';
```

다음으로 넘어가기 전에, 우리는 Firebase가 제공하는 Realtime Database에서 데이터가 어떻게 구조화되는지 이해해야 합니다. 데이터는 문자 그대로 JSON 트리로 구성됩니다.

설명서에 따라:

> "모든 Firebase Realtime Database 데이터는 JSON 개체로 저장됩니다. 데이터베이스를 클라우드 호스팅된 JSON 트리로 생각할 수 있습니다. SQL 데이터베이스와 달리 테이블이나 레코드가 없습니다. JSON 트리에 데이터를 추가하면 해당 트리가 연결된 키가 있는 기존 JSON 구조의 노드가 됩니다."

따라서 실시간 데이터베이스를 쉽고 친숙하게 사용할 수 있습니다.

앱 구성 요소가 마운트되면 가장 먼저 현재 해야 할 모든 항목을 가져와 작업관리 상태 개체에 추가하는 것입니다. 그러기 위해서는 데이터베이스의 특정 위치를 참조해야 합니다. 이 위치의 경로를 `토도스`라고 합니다. 일단 그렇게 하고 나면, 우리는 그 경로의 내용이나 위치에 대한 변경을 들을 필요가 있습니다. 이것은 바닐라 자바스크립트에서 이벤트를 듣는 것과 매우 비슷하다. 이 경우, 우리는 `가치`라는 이벤트를 듣게 될 것이다.

다시 한 번 Firebase 설명서에 따르면:

> "[이 이벤트를 통해 우리는] 주어진 경로에서 콘텐츠의 정적 스냅샷을 읽을 수 있습니다. 이 방법은 수신기가 연결되면 한 번 트리거되고 어린이를 포함한 데이터가 변경될 때마다 다시 트리거됩니다.

이벤트 콜백 함수는 하위 데이터를 포함하여 해당 위치의 모든 데이터를 포함하는 스냅샷을 전달합니다. 이제 `val()` 방법을 사용하여 스냅샷에서 데이터를 가져올 수 있습니다. 이 데이터는 일반적으로 JavaScript 개체 형태로 제공됩니다.

```undefined
componentDidMount() {
    db.ref('/todos').on('value', querySnapShot => {
      let data = querySnapShot.val() ? querySnapShot.val() : {};
      let todoItems = {...data};
      this.setState({
        todos: todoItems,
      });
    });
  }
```

우리가 읽는 경로에 데이터가 없을 경우 데이터 변수에 빈 객체를 할당하기 위해 터미널 연산자를 사용했다.

또한 "Add NewTodo() 방법에 코드를 추가해야 합니다. 각 작업관리 항목에는 이름 또는 제목과 관련 작업 수행 여부를 나타내는 값이 있어야 합니다.

따라서, 우리는 데이터베이스의 각 작업관리 항목을 나타내는 객체를 사용할 것입니다. 특정 위치에 데이터를 추가하려면 데이터를 추가할 위치의 경로를 참조하고 push() 방식을 사용해야 합니다. 또한 새로운 작업관리 항목을 추가한 후 사용자에게 경고가 표시되도록 합니다.

```undefined
addNewTodo() {
    db.ref('/todos').push({
      done: false,
      todoItem: this.state.presentToDo,
    });
    Alert.alert('Action!', 'A new To-do item was created');
    this.setState({
      presentToDo: '',
    });
  }
```

`작업` 경로의 데이터를 지우려면 해당 위치를 참조하고 제거() 방법을 사용하면 됩니다.

```undefined
clearTodos() {
    db.ref('/todos').remove();
  }
```

이제 `TextInput` 구성 요소를 사용하여 작업관리 항목을 추가해 보십시오. 데이터베이스 대시보드 또는 콘솔에서 데이터베이스를 변경해야 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/02/to-do-item-added-database.jpeg?resize=730%2C280&ssl=1)

작업관리 항목 목록을 콘솔에 기록한 경우 다음과 같은 항목이 있어야 합니다.

```undefined
{
  "-LzTe5MyZ7aUL7y-iFku": {
    "done": false,
    "todoItem": "HEY"
  }
}
```

이 결과를 보면 작업관리 항목 목록이 Firebase에 의해 모든 작업관리 항목에 할당된 고유 ID와 동일한 키 이름을 가진 개체임을 알 수 있습니다. 각 작업관리 항목의 값은 개체이며, 이 개체는 "AddNewToDo() 방법"에서 지정한 형식과 유사합니다.

이 정보를 손에 넣음으로써 이제 우리는 우리의 `작업` 상태 객체에서 키를 추출하고 각각의 `작업항목` 구성요소에 필요한 소품을 전달할 수 있다. 키를 추출하여 `todosKeys`라는 변수에 할당해 보겠습니다.

```js
render() {
    let todosKeys = Object.keys(this.state.todos);
    return (
      <ScrollView ...>
        ...
      </ScrollView>
    );
}
```

앱 구성 요소의 빈 `보기` 구성 요소에 추가해 보겠습니다.

```js
<View>
  {todosKeys.length > 0 ? (
    todosKeys.map(key => (
      <ToDoItem
        key={key}
        id={key}
        todoItem={this.state.todos[key]}
      />
    ))
  ) : (
        <Text>No todo item</Text>
  )}
</View>
```

이제 우리는 `ToDoItem` 구성 요소를 변경해야 합니다. 이제 작업관리 항목 구성요소가 적절한 이름과 상태에 액세스할 수 있으므로, 플레이스홀더 값을 제거할 수 있습니다. 이제 `ToDoItem` 구성 요소는 다음과 같습니다.

```js
const ToDoItem = ({todoItem: {todoItem: name, done}, id}) => {
  const [doneState, setDone] = useState(done);
  const onCheck = () => {
    setDone(!doneState);
  };
  return (
    <View style={styles.todoItem}>
      <CheckBox
        checkBoxColor="skyblue"
        onClick={onCheck}
        isChecked={doneState}
        disabled={doneState}
      />
      <Text style={[styles.todoText, {opacity: doneState ? 0.2 : 1}]}>
        {name}
      </Text>
    </View>
  );
};
```

지금은 간단한 앱이 대부분이에요. 그러나 여전히 약간의 문제가 있습니다. 작업관리 항목을 완료 상태로 확인하고 앱을 다시 로드하면 해당 작업관리 항목이 원래 선택 취소됨 상태로 돌아갑니다. 업데이트할 수 없는 데이터베이스를 사용하는 목적은 무엇입니까?

따라서 확인란이 확인될 때마다 데이터베이스에서 done 값을 업데이트해야 합니다. 업데이트() 방법으로 이 작업을 수행할 수 있습니다. 특정 필드를 업데이트하려면 업데이트된 필드가 포함된 개체를 전달하기만 하면 됩니다.

```js
const onCheck = () => {
    setDone(!doneState);
    db.ref('/todos').update({
      [id]: {
        todoItem: name,
        done: !doneState,
      },
    });
  };
  return (
    <View style={styles.todoItem}>
      <CheckBox
        checkBoxColor="skyblue"
        onClick={onCheck}
        isChecked={doneState}
        disabled={doneState}
      />
        ...
      </View>
    )
  }
```

이제 특정 작업관리 항목을 선택한 경우, 데이터베이스에서 작업관리 항목의 "완료" 값이 업데이트되어야 합니다.

## 결론

다 됐다! 더 이상 어쩔 수 없다! Firebase에서 제공하는 백엔드를 사용하여 Android 앱을 만들었습니다! Android 앱을 만들었습니다!

일반적으로 이 앱에 일부 인증을 추가하면 사용자가 전 세계에서 지원되는 장치에 로그인하고 데이터에 액세스할 수 있습니다. 이 작업은 Firebase에서도 수행할 수 있습니다. 이 주제에 관한 제 튜토리얼을 여기서 보실 수 있습니다.