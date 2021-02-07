---
layout: post
title: "대응 및 Firebase를 사용하여 Google Keep 클론 생성"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/firebase.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/firebase.png?fit=730%2C487&ssl=1)

이 문서에서는 React, React Hooks, Styled Components 및 Firebase를 사용하여 Google Keep의 복제본을 생성합니다. Google Keep은 노트를 만드는 앱으로, 복제될 기능 중 일부는 노트를 만들고 Firebase에 저장하는 것을 포함합니다.

이것이 바로 keep-react-react-clone.netliify.app입니다.

## 전제조건

React(기능 구성 요소), React Hooks 및 JavaScript에 대한 기본 지식이 필요합니다.

또한 기기에 Node = 8.10 및 npm = 5.6이 설치되어 있는지 확인하십시오. 여기에 Node.js를 설치할 수 있습니다.

## 슬슬 출발 해야지.

먼저 대응 앱을 만들어 보겠습니다.

```undefined
npx create-react-app google-keep-clone
cd google-keep-clone
```

이제 Firebase를 설치합니다.

```undefined
npm install --save firebase
```

또한 스타일 구성 요소 설치:

```undefined
npm install --save styled-components
```

Firebase 프로젝트를 만들려면 Firebase 콘솔을 열고 로그인하여 "Add project(프로젝트 추가)"를 클릭하여 프로젝트를 만듭니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/create-Project.png?resize=730%2C328&ssl=1)

그런 다음 프로젝트 이름을 입력하십시오.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/Naming-project.png?resize=730%2C331&ssl=1)

다음 페이지에서 Google Analytics를 사용하도록 결정할 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/Google-firebsae.png?resize=730%2C328&ssl=1)

마지막으로 프로젝트를 만듭니다.

작업이 완료되면 Firebase 콘솔로 이동합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/configure-project.png?resize=730%2C327&ssl=1)

Firebase 콘솔에서 Project Overview(프로젝트 개요) 옆의 설정 아이콘을 클릭하고 Project Settings(프로젝트 설정)으로 이동합니다.

앱으로 스크롤을 내려 웹 앱의 버튼을 클릭합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/there-are-no-apps-yet.png?resize=730%2C314&ssl=1)

지금 웹 앱을 등록할 수 있는 인터페이스가 있어야 합니다. 닉네임을 입력하고 버튼을 클릭합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/register-app.png?resize=730%2C278&ssl=1)

지금 Firebase 구성 세부 정보를 표시하고 스크립트 태그에 있는 모든 내용을 복사합니다.

코드로 돌아가 src 폴더에 Firebase.js 파일을 생성하고 설치한 Firebase를 가져온 다음 구성 세부 정보를 스크립트 태그에 모두 붙여넣습니다.

이 파일은 React 구성 요소로 가져올 것이므로 Firebase를 내보내는 것을 잊지 마십시오.

다음과 같은 것이 있어야 합니다.

```js
import firebase from 'firebase';
  const firebaseConfig = {
    apiKey: "xxxxxxxxxx",
    authDomain: "keep-react-clone.firebaseapp.com",
    databaseURL: "https://keep-react-clone.firebaseio.com",
    projectId: "keep-react-clone",
    storageBucket: "keep-react-clone.appspot.com",
    messagingSenderId: "xxxxxxxx",
    appId: "xxxxxxxxxx",
    measurementId: "xxxxxxx"
  };
 
  firebase.initializeApp(firebaseConfig);
  firebase.analytics();
  export default firebase
```

참고: "xxxxxxxxx"는 자리 표시자일 뿐이며 실제 문자일 수 있습니다.

App.js의 기본값을 지우면 React 및 CSS 가져오기와 App 구성 요소를 그대로 사용할 수 있습니다.

이제 Firebase 파일을 다음으로 가져옵니다.

```coffeescript
import firebase from ./firebase
```

## 레이아웃 작성

우리 헤더를 만들자.

이것이 우리가 이루고자 하는 것이다. 너무 어렵지 않습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/hello-world-1.png?resize=730%2C319&ssl=1)

구성 요소 폴더 내에 Header.js 파일을 생성하겠습니다(기본적으로 여기에 표시되지 않는 경우 src 폴더에 구성 요소 폴더를 생성).

React 및 Styled 구성 요소:

```coffeescript
import React from "react";
import styled from "styled-components";
```

또한 로고 이미지를 가져오십시오.

```coffeescript
import keepLogo from '../assets/keep-logo.png'
import reactLogo from '../logo.svg'
import firebaseLogo from '../assets/firebase-logo.png'
```

그런 다음 기능 구성 요소를 생성하여 내보냅니다.

```coffeescript
const Header = () => {
   return ();
  };
 
 export default Header;
```

## 스타일링된 구성요소를 사용하여 요소 작성

스타일링된 구성 요소는 JavaScript에 CSS를 쓰는 데 도움이 됩니다. 이렇게 하면 미리 정의된 스타일을 사용하여 재사용 가능한 요소를 만들 수 있습니다.

내비게이션을 작성하려면, 헤더의 래퍼로 사용할 몇 가지 스타일을 사용하면 됩니다. 그런 다음, 우리는 CSS 스타일에서 그것의 값을 `backtick:` 다음에 `styled.(요소 이름)`로 부른다.

다음과 같이 보입니다.

```cpp
const Nav = styled.nav`
  display: flex;
  justify-content: space-between;
  align-items:center;
  padding: 4px 25px;
  border-bottom: 1px solid rgba(60, 64, 67, 0.2);
  `;
```

오른쪽 끝에 있는 로고 포장지에 대해서도 동일한 작업을 수행합니다.

```js
const ImgWrap = styled.div`
display: flex;
align-items:center;
`;
```

그리고 우리의 이미지 요소:

```js
const Img = styled.img`
width:40px;
height:40px;
`;
```

이러한 모든 기능을 통해 다음과 같이 Header 기능을 업데이트할 수 있습니다.

```xml
const Header = () => {
    return (
      <Nav>
        <p>Keep clone</p>
        <ImgWrap>
          <Img src={keepLogo} alt="Google keep logo" />
          <p>+</p>
          <Img src={reactLogo} alt="React logo"/>
          <p>+</p>
          <Img src={firebaseLogo} alt="firebase logo"/>
        </ImgWrap>
      </Nav>
    );
  };
```

앱 신체로 이사하는 것, 수입의 반응과 스타일 구성 요소처럼 우리가 위에서 한 Main.js 파일을 만들자.

그런 다음 메인 HTML 요소를 반환하는 메인이라는 기능 구성요소를 만듭니다.

```js
const Main = () =>{ 
   return(
      <main>
      </main>
    )
  }
  export default Main
```

그런 다음 노트 제목에 대한 입력과 노트 본문에 대한 텍스트 영역을 포함하는 양식을 만듭니다. 스타일링된 구성 요소를 사용하여 `NoteInput`으로 이름을 지정합니다.

```cpp
const NoteInput = styled.form`
  box-shadow: 0 1px 2px 0 rgba(60,64,67,.3),
    0 2px 6px 2px rgba(60,64,67,.15);
  width:600px;
  border-radius:8px;
  margin:20px auto;
  padding:20px;
  
```

그런 다음 제목과 텍스트 영역의 스타일을 지정합니다.

```php
const Title = styled.input`
    border:none;
    color:#000;
    display:block;
    width:100%;
    font-size:18px;
    margin:10px 0;
    outline:none;
    &::placeholder{
      color:#3c4043;
      opacity:1;
    }
  
```

```php
const TextArea = styled.textarea`
      border:none;
      color:#000;
      display:block;
      width:100%;
      font-family: 'Noto Sans', sans-serif;
      font-size:13px;
      font-weight:bold;
      outline:none;
      resize: none;
      overflow: hidden;
      min-height: 10px;
      &::placeholder{
        color:#3c4043;
        opacity:1;
      }
  
```

> 참고: CSS 스타일링은 기본 스타일링일 뿐이며 이 글은 CSS를 가르치는 것을 목표로 하지 않기 때문에 자세히 설명하지 않습니다.

구성 요소에 `노트 입력`, 제목 및 텍스트 영역을 추가해 보겠습니다.

```coffeescript
const Main = () =>{ 
   return(
      <main>
       <NoteInput action="">
         <Title type="text" placeholder="Title"/> 
         <TextArea name="" id="" cols="30" rows="1" placeholder="Take a note..."/>
        </NoteInput>
      </main>
    )
  }
  export default Main
```

App.js 파일에서 지금까지 수행한 작업을 확인하려면 Header 및 Main 구성 요소를 모두 가져와야 합니다. 또한 useState와 use Effect를 수입하여 나중에 다시 방문하겠습니다.

```coffeescript
import React, { useState, useEffect } from "react";
import Header from "./components/Header";
import Main from "./components/Main";
```

브라우저에는 다음이 있어야 합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/take-a-note.png?resize=730%2C172&ssl=1)

그러나 Google Keep 앱을 보면 텍스트 영역을 클릭할 때까지 제목이 표시되지 않습니다. 이를 위해 showInput의 상태를 만들고 텍스트 영역을 클릭했는지 여부에 따라 true와 false 사이에서 값을 전환합니다.

```cpp
const [showInput, setShowInput] = useState(false);
```

제목 입력 및 텍스트 영역에 대한 상태도 만들어 보겠습니다.

```undefined
const [textValue, setTextValue] = useState('');
const [titleValue, setTitleValue] = useState('');
```

그런 다음 `showInput`, `textValue`, `titleValue` 및 해당 상태 처리기를 Main 구성 요소로 전달합니다.

```undefined
<Main
textValue = {textValue}
titleValue = {titleValue}
showInput={showInput}
onShowInput = {(state)=>setShowInput(state)}
onTextChange = {(state)=>setTextValue(state)}
onTitleChange = {state=>setTitleValue(state)}
/>
```

Main.js에서는 기능 구성 요소에 소품을 전달하고 제목 입력을 동적으로 표시할 수 있습니다.

```php
{props.showInput ? <Title type="text" name="" id="" 
 placeholder="Title" 
 value={props.titleValue}
 onFocus={()=>props.onTitleFocus(true)}
 onBlur={()=>props.onTitleFocus(false)}
 onChange={(e)=>props.onTitleChange(e.target.value)}
 /> : ''
 }
```

텍스트 영역에 대한 값과 변경 시 이벤트를 추가하고 showInput을 실제 onFocus로 설정합니다.

```undefined
<TextArea name="" id="" cols="30" rows="1" 
            placeholder="Take a note..." 
            value={props.textValue} 
            onFocus={()=> {
              props.onShowInput(true);
            }
            onChange={(e)=>props.onTextChange(e.target.value)}
          />
```

TextArea는 텍스트와 함께 자동으로 증가하지 않으므로 이를 위해 `자동 증가` 기능을 생성합니다.

```coffeescript
const autoGrow = (elem) =>{
      elem.current.style.height = "5px";
      elem.current.style.height = (10 + 
      elem.current.scrollHeight)+"px";
    }
```

Ref 후크를 사용하여 원하는 요소를 autoGrow 기능으로 전달할 것입니다.

`useRef`를 사용하려면 해당 항목을 가져와야 하므로 가져오기를 다음과 같이 업데이트합니다.

```coffeescript
import React, { useRef} from 'react'
```

use Ref Hook을 사용할 수 있습니다.

```undefined
const textAreaRef = useRef(null);
```

텍스트 영역의 ref 값을 `textAreaRef`로 추가합니다.

```undefined
ref={textAreaRef}
```

또한 "onFocus"를 통해 참조가 집중되고 있는지 확인하고 "OnInput"을 사용하여 "autoGrow" 함수를 호출합니다.

이제 업데이트된 텍스트 영역입니다.

```undefined
<TextArea name="" id="" cols="30" rows="1" 
        placeholder="Take a note..." 
        value={props.textValue} onFocus={()=> {
         props.onShowInput(true);
         textAreaRef.current.focus();
        } onInput={()=>autoGrow(textAreaRef)} ref={textAreaRef}  
        onChange={(e)=>props.onTextChange(e.target.value)}
/>
```

Google Keep의 작동 방식 텍스트 상자 밖을 클릭하면 해당 노트가 목록에 추가됩니다. 이를 위해 우리는 텍스트 영역과 제목 입력이 집중되어 있는지 여부를 확인해야 한다.

이제 App.js 파일로 돌아가서 `textfocused`와 `titlefocused`의 두 가지 상태를 만들어 보겠습니다.

```cpp
const [textFocused, setTextFocused] = useState(false);
const [titleFocused, setTitleFocused] = useState(false);
```

다음과 같이 주 구성 요소에 전달합니다.

```undefined
onTextFocus={(state) => setTextFocused(state)}
onTitleFocus={(state)=>setTitleFocused(state)}
```

제목을 작성하기 위해 Main.js로 돌아갑니다. 온포커스(onFocus) 행사는 텍스트 포커스를 true로, 온블러(onBlur)는 텍스트 포커스를 false로 설정한다.

```undefined
onFocus={()=>props.onTitleFocus(true)}
onBlur={()=>props.onTitleFocus(false)}
```

텍스트 영역도 마찬가지입니다.

```undefined
onFocus={()=>props.onTitleFocus(true)}
onBlur={()=>props.onTextFocus(false)}
```

그런 다음 앱을 클릭할 때 호출되는 기능을 만듭니다. 이 함수는 새 노트를 추가하기 전에 textFocused와 titleFocused가 모두 거짓인지, textValue 또는 titleValue 중 하나가 비어 있지 않은지 확인합니다.

```js
const blurOut = () => {
    if (!textFocused && !titleFocused) {
      if(textValue !== '' || titleValue !== ''){
        setShowInput(false)
        let noteObj = {
          title:titleValue,
          text:textValue
        }
        setTextValue('');
        setTitleValue('')
      }
    }
  };
```

노트 개체만 생성했습니다. 노트를 위한 상태를 만들어 보겠습니다.

```cpp
const [notes, setNotes] = useState([]);
```

blurOut 함수에서 setTitleValue(`)`) 뒤에 추가할 수 있습니다.

```css
setNotes([...notes, noteObj])
```

스프레드 운영자를 사용하는 이유가 궁금하실 경우, 기본적으로 기존 노트에 noteObj가 추가됩니다. 스프레드 연산자에 대한 자세한 내용을 볼 수 있습니다.

노트를 표시하려면 노트 구성 요소에 대한 Note.js 파일을 만들어 보겠습니다.

기본 스타일 외에 별다른 것이 없으므로 다음과 같이 복사할 수 있습니다.

```js
import React from 'react'
  import styled from "styled-components";
 
  const NoteDiv = styled.div`
  padding:20px;
  border:1px solid #e0e0e0;
  border-radius:8px;
  text-align:left;
  font-size:18px;
  margin:10px;
  min-width:300px;
  `
  const H = styled.h3`
  font-size:20px;
  font-weight:bold;`
  const Note = (props) =>{
    return(
      <NoteDiv>
        <H>{props.note.title}</H>
        <p>{props.note.text}</p>
      </NoteDiv>
    )
  }
  export default Note;
```

이제 Note.js를 Main.js 구성 요소로 가져옵니다. 먼저 `노트콘`이라는 노트에 대한 래퍼를 만듭니다.

```undefined
const NoteCon = styled.div`
padding:20px;
display:flex;
flex-wrap:wrap;
justify-content:center;

```

노트 입력 뒤에 래퍼를 추가하고 App.js에서 가져올 노트를 노트 구성 요소에 매핑합니다. React에서 목록 렌더링에 대해 모르는 경우 이 항목을 보십시오.

```js
<NoteCon>
 {props.notes.map((note,index)=><Note note={note} 
 key={index} />)}
</NoteCon>
```

App.js로 돌아가 주 구성 요소에 대한 소품으로 노트를 전달합니다.

```undefined
notes={notes}
```

앱 구성 요소의 blurOut 기능도 클릭해서 불러보겠습니다.

## Firebase에서 데이터 저장 및 가져오기

지금까지 우리가 한 일은 지속되지 않는다. 새로 고치면 지워집니다. 노트 정보를 Firebase에 저장하고 새로 고칠 때마다 거기서 가져와야 합니다.

이미 Firebase를 가져왔기 때문에 Firebase 데이터베이스의 데이터 참조를 호출하고 Firebase 내장 방법을 사용하여 데이터를 저장할 수 있습니다.

```undefined
const db = firebase.database().ref('data');
db.push().set(noteObj)
```

업데이트된 blurOut 기능은 다음과 같아야 합니다(오류 처리에 try...catch를 사용했습니다).

```js
const blurOut = () => {
    if (!textFocused && !titleFocused) {
      if(textValue !== '' || titleValue !== ''){
        setShowInput(false)
        let noteObj = {
          title:titleValue,
          text:textValue
        }
        setTextValue('');
        setTitleValue('')
        try{
          setNotes([...notes, noteObj])
            const db = firebase.database().ref('data');
            db.push().set(noteObj)
        }
        catch(error){
          console.log(error);
        }
      }
    }
  };
```

데이터를 가져오려면 `getData` 함수를 만듭니다.

```coffeescript
const getData = ()=>{}
```

그런 다음 노트에 대한 빈 배열을 만들고 데이터베이스의 각 값을 순환한 다음 배열로 이동합니다. 상태를 업데이트하기 전에 어레이가 비어 있지 않은지 확인합니다.

```js
const getData = ()=>{
    let notesArr = [];
    try{  
      const db = firebase.database().ref('data');
      db.orderByValue().once("value", snapshot =>{
        snapshot.forEach((note)=>{
          // console.log(notes)
          // setNotes([...notes, note.val()])
          notesArr.push(note.val());
        })
 
        if(notesArr.length !== 0){
         setNotes(notesArr)
        }
      })
     }
    catch(error){
       console.log(error)
    }
  };
```

이펙트 후크 사용을 가져왔습니다. use Effect 내부의 get Data 기능을 호출합니다.

이 반응 후크는 페이지가 렌더링된 후 즉시 해당 기능이 호출되도록 합니다.

```coffeescript
useEffect(()=>{
 getData();
}, [])
```

끝의 빈 배열은 이 useEffect가 구성 요소를 처음 마운트할 때 한 번만 실행된다는 것을 의미합니다. 이것이 바로 우리가 원하는 것입니다.
참고로 이 보고서를 보고 실제로 무엇을 했는지 확인할 수 있습니다.

## 결론

이를 통해 Google Keep의 간단한 복제본을 만들었습니다. 그러나 노트 편집, 삭제, 다른 범주로 분할, 사용자마다 다른 데이터베이스 계정 만들기 등 일부 기능은 여전히 누락되어 있습니다.

이러한 기능들을 직접 추가해 보셔도 되고, 저는 당신이 한 일을 기꺼이 볼 수 있습니다. 트위터로 연락하시면 됩니다.