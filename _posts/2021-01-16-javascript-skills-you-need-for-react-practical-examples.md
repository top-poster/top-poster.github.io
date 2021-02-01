---
layout: post
title: "React에 필요한 JavaScript 기술 (+ 실용적인 예)
 "
author: 'Code Tower'
thumbnail: https://www.freecodecamp.org/news/content/images/size/w600/2021/01/7-js-skills-for-react.png
tags: undefined
---


React에 대해 이해해야 할 가장 중요한 것 중 하나는 기본적으로 JavaScript라는 것입니다.
 이것은 당신이 JavaScript를 더 잘할수록 React를 더 성공적으로 사용할 수 있음을 의미합니다.
 

React를 마스터하기 위해 JavaScript에 대해 알아야 할 7 가지 필수 개념을 분석해 보겠습니다.
 

그리고 이러한 개념이 필수적이라고 말하면 React 개발자가 만드는 모든 단일 애플리케이션에서 거의 예외없이 사용된다는 것을 의미합니다.
 

이러한 개념을 배우는 것은 React 프로젝트를 만드는 능력을 가속화하고 숙련 된 React 개발자가되기 위해 할 수있는 가장 가치있는 일 중 하나이므로 시작하겠습니다.
 

## 이 가이드의 사본을 원하십니까?
 

여기에서 PDF 형식의 치트 시트를 다운로드하십시오 (5 초 소요).
 

## 1. 함수 선언 및 화살표 함수
 

모든 React 애플리케이션의 기본은 구성 요소입니다.
 React에서 컴포넌트는 JavaScript 함수와 클래스로 정의됩니다.
 

그러나 JavaScript 함수와 달리 React 컴포넌트는 애플리케이션 인터페이스를 구성하는 데 사용되는 JSX 요소를 반환합니다.
 

```undefined
// JavaScript function: returns any valid JavaScript type
function javascriptFunction() {
  return "Hello world";
}

// React function component: returns JSX
function ReactComponent(props) {
  return <h1>{props.content}</h1>   
}
```

JavaScript 함수 이름과 React 함수 구성 요소 사이의 대소 문자가 다릅니다.
 JavaScript 함수는 낙타 대문자로 이름이 지정되고 React 함수 구성 요소는 파스칼 대소 문자로 작성됩니다 (모든 단어가 대문자로 표시됨).
 

JavaScript에서 함수를 작성하는 방법에는 두 가지가 있습니다. 기존 방식은 함수 선언이라고하는`function` 키워드를 사용하는 방법과 ES6에 도입 된 화살표 함수입니다.
 

함수 선언과 화살표 함수는 모두 React에서 함수 구성 요소를 작성하는 데 사용할 수 있습니다.
 

화살표 기능의 주요 이점은 간결성입니다.
 불필요한 상용구를 제거하는 함수를 작성하기 위해 몇 가지 속기를 사용할 수 있으므로 모든 것을 한 줄에 작성할 수도 있습니다.
 

```undefined
// Function declaration syntax
function MyComponent(props) {
  return <div>{props.content}</div>;
}
 
// Arrow function syntax
const MyComponent = (props) => {
  return <div>{props.content}</div>;
}
 
// Arrow function syntax (shorthand)
const MyComponent = props => <div>{props.content}</div>;

/* 
In the last example we are using several shorthands that arrow functions allow:

1. No parentheses around a single parameter
2. Implicit return (as compared to using the "return" keyword)
3. No curly braces for function body
*/
```

화살표 함수보다 함수 선언을 사용하는 작은 이점 중 하나는 호이 스팅 문제에 대해 걱정할 필요가 없다는 것입니다.
 

호이 스팅의 JavaScript 동작으로 인해 원하는 순서대로 단일 파일에서 함수 선언으로 만든 여러 함수 구성 요소를 사용할 수 있습니다.
 

그러나 화살표 기능으로 만든 기능 구성 요소는 원하는 방식으로 주문할 수 없습니다.
 JavaScript 변수가 호이스트되기 때문에 화살표 함수 구성 요소를 사용하기 전에 선언해야합니다.
 

```undefined
function App() {
  return (
    <>
      {/* Valid! FunctionDeclaration is hoisted */}
      <FunctionDeclaration />
      {/* Invalid! ArrowFunction is NOT hoisted. Therefore, it must be declared before it is used */}
      <ArrowFunction />
    </>
}
  
function FunctionDeclaration() {
  return <div>Hello React!</div>;   
}

function ArrowFunction() {
  return <div>Hello React, again!</div>;   
}  
```

함수 선언 구문 사용의 또 다른 작은 차이점은 함수가 선언되기 전에`export default` 또는`export`를 사용하여 파일에서 구성 요소를 즉시 내보낼 수 있다는 것입니다.
 화살표 함수 앞에`export` 키워드 만 사용할 수 있습니다 (기본 내보내기는 구성 요소 아래 줄에 있어야 함).
 

```undefined
// Function declaration syntax can be immediately exported with export default or export
export default function App() {
  return <div>Hello React</div>;   
}

// Arrow function syntax must use export only
export const App = () => {
  return <div>Hello React</div>;     
}
```

## 2. 템플릿 리터럴
 

JavaScript는 특히 여러 문자열을 함께 연결하거나 연결하려는 경우 문자열 작업의 서투른 역사를 가지고 있습니다.
 ES6가 도착하기 전에 문자열을 함께 추가하려면`+ `연산자를 사용하여 각 문자열 세그먼트를 서로 추가해야했습니다.
 

ES6이 추가됨에 따라 작은 따옴표 또는 큰 따옴표 대신 두 개의 백틱````로 구성된 템플릿 리터럴이라는 새로운 형식의 문자열이 제공되었습니다.
 

`+`연산자를 사용하는 대신 특수한`$ {}`구문 내에 JavaScript 표현식 (예 : 변수)을 넣어 문자열을 연결할 수 있습니다.
 

```undefined
/* 
Concatenating strings prior to ES6.
Notice the awkward space after the word Hello?
*/
function sayHello(text) {
  return 'Hello ' + text + '!';
}

sayHello('React'); // Hello React!
 
/* 
Concatenating strings using template literals.
See how much more readable and predictable this code is?
*/
function sayHelloAgain(text) {
  return `Hello again, ${text}!`;
}

sayHelloAgain('React'); // Hello again, React!
```

템플릿 리터럴의 강력한 점은`$ {}`구문 내에서 JavaScript 표현식 (즉, 값으로 해석되는 JavaScript의 모든 것)을 사용할 수 있다는 것입니다.
 

삼항 연산자를 사용하여 조건부 논리를 포함 할 수도 있습니다. 이는 주어진 JSX 요소에 클래스 또는 스타일 규칙을 조건부로 추가하거나 제거하는 데 적합합니다.
 

```undefined
/* Go to react.new and paste this code in to see it work! */
import React from "react";

function App() {
  const [isRedColor, setRedColor] = React.useState(false);

  const toggleColor = () => setRedColor((prev) => !prev);

  return (
    <button
      onClick={toggleColor}
      style={{
        background: isRedColor ? "red" : "black",
        color: 'white'
      }}
    >
      Button is {isRedColor ? "red" : "not red"}
    </button>
  );
}

export default App;
```

간단히 말해 템플릿 리터럴은 동적으로 문자열을 생성해야 할 때마다 React에 적합합니다.
 예를 들어 사이트의 head 또는 body 요소에서 변경할 수있는 문자열 값을 사용하는 경우 :
 

```undefined
import React from 'react';
import Head from './Head';

function Layout(props) {
  // Shows site name (i.e. Reed Barger) at end of page title
  const title = `${props.title} | Reed Barger`;  
    
  return (
     <>
       <Head>
         <title>{title}</title>
       </Head>
       <main>
        {props.children}
       </main>
     </>
  );
}

```

## 3. 짧은 조건 : &&, ||, 삼항 연산자
 

React가 JavaScript 일 뿐이라는 점을 고려하면 간단한 if 문을 사용하고 때로는 switch 문을 사용하여 JSX 요소를 조건부로 표시하거나 숨기는 것이 매우 쉽습니다.
 

```undefined
import React from "react";

function App() {
  const isLoggedIn = true;

  if (isLoggedIn) {
    // Shows: Welcome back!
    return <div>Welcome back!</div>;
  }

  return <div>Who are you?</div>;
}

export default App;
```

필수 JavaScript 연산자의 도움으로 반복을 줄이고 코드를 더 간결하게 만듭니다.
 

삼항 연산자를 사용하여 위의 if 문을 다음과 같이 변환 할 수 있습니다.
 삼항 연산자는 if 문과 정확히 동일하게 작동하지만 더 짧고 표현식 (문이 아님)이며 JSX 내에 삽입 될 수 있습니다.
 

```undefined
import React from "react";

function App() {
  const isLoggedIn = true;

  // Shows: Welcome back!
  return isLoggedIn ? <div>Welcome back!</div> : <div>Who are you?</div>
}

export default App;
```

삼항 연산자는 중괄호 안에 사용할 수도 있습니다 (다시 말하지만 표현식이므로).
 

```undefined
import React from "react";

function App() {
  const isLoggedIn = true;

  // Shows: Welcome back!
  return <div>{isLoggedIn ? "Welcome back!" : "Who are you?"</div>;
}

export default App;
```

위의 예를 변경하고 사용자가 로그인 한 경우에만 텍스트를 표시하려는 경우 (`isLoggedIn`이 true 인 경우)`&&`(and) 연산자에 대한 훌륭한 사용 사례가 될 것입니다.
 

조건부의 첫 번째 값 (피연산자)이 참이면`&&`연산자는 두 번째 피연산자를 표시합니다.
 그렇지 않으면 첫 번째 피연산자를 반환합니다.
 그리고 그것은 거짓이기 때문에 (자바 스크립트에 의해 자동으로 부울`false`로 변환 된 값임) JSX에 의해 렌더링되지 않습니다.
 

```undefined
import React from "react";

function App() {
  const isLoggedIn = true;

  // If true: Welcome back!, if false: nothing
  return <div>{isLoggedIn && "Welcome back!"}</div>;
}

export default App;
```

우리가 지금하고있는 일의 반대를 원한다고 가정 해 봅시다. "당신은 누구입니까?"
 `isLoggedIn`이 거짓 인 경우.
 사실이면 아무것도 표시하지 않습니다.
 

이 논리에는`||`(또는) 연산자를 사용할 수 있습니다.
 본질적으로`&&`연산자와 반대로 작동합니다.
 첫 번째 피연산자가 참이면 첫 번째 (거짓) 피연산자가 반환됩니다.
 첫 번째 피연산자가 거짓이면 두 번째 피연산자가 반환됩니다.
 

```undefined
import React from "react";

function App() {
  const isLoggedIn = true;

  // If true: nothing, if false: Who are you?
  return <div>{isLoggedIn || "Who are you?"}</div>;
}

export default App;
```

## 4. 세 가지 배열 메서드 : .map (), .filter (), .reduce ()
 

JSX 요소에 기본 값을 삽입하는 것은 쉽습니다. 중괄호 만 사용하면됩니다.
 

기본 값 (문자열, 숫자, 부울 등)과 기본 값을 포함하는 객체 속성을 포함하는 변수를 포함하여 유효한 표현식을 삽입 할 수 있습니다.
 

```undefined
import React from "react";

function App() {
  const name = "Reed";
  const bio = {
    age: 28,
    isEnglishSpeaker: true
  };

  return (
    <>
      <h1>{name}</h1>
      <h2>I am {bio.age} years old</h2>
      <p>Speaks English: {bio.isEnglishSpeaker}</p>
    </>
  );
}

export default App;
```

배열이 있고 해당 배열을 반복하여 개별 JSX 요소 내의 각 배열 요소를 표시하려면 어떻게해야합니까?
 

이를 위해`.map ()`메소드를 사용할 수 있습니다.
 내부 함수로 지정한 방식으로 배열의 각 요소를 변환 할 수 있습니다.
 

화살표 기능과 함께 사용하면 특히 간결합니다.
 

```undefined
/* Note that this isn't exactly the same as the normal JavaScript .map() method, but is very similar. */
import React from "react";

function App() {
  const programmers = ["Reed", "John", "Jane"];

  return (
    <ul>
      {programmers.map(programmer => <li>{programmer}</li>)}
    </ul>
  );
}

export default App;
```

.map () 메서드에는 관련 작업을 수행하고 서로 결합하여 연결할 수 있으므로 알아야하는 다른 특징이 있습니다.
 

왜?
 `.map ()`은 많은 배열 메서드와 마찬가지로 반복 된 배열의 얕은 복사본을 반환하기 때문입니다.
 이렇게하면 반환 된 배열이 체인의 다음 메서드로 전달 될 수 있습니다.
 

이름에서 알 수 있듯이`.filter ()`를 사용하면 배열에서 특정 요소를 필터링 할 수 있습니다.
 예를 들어, "J"로 시작하는 모든 프로그래머 이름을 제거하려면`.filter ()`를 사용하면됩니다.
 

```undefined
import React from "react";

function App() {
  const programmers = ["Reed", "John", "Jane"];

  return (
    <ul>
      {/* Returns 'Reed' */}
      {programmers
       .filter(programmer => !programmer.startsWith("J"))
       .map(programmer => <li>{programmer}</li>)}
    </ul>
  );
}

export default App;
```

`.map ()`과`.filter ()`모두`.reduce ()`배열 메서드의 다른 변형 일 뿐이며, 배열 값을 사실상 모든 데이터 유형으로 변환 할 수 있습니다.
 배열 값.
 

위의`.filter ()`메서드와 동일한 작업을 수행하는`.reduce ()`는 다음과 같습니다.
 

```undefined
import React from "react";

function App() {
  const programmers = ["Reed", "John", "Jane"];

  return (
    <ul>
      {/* Returns 'Reed' */}
      {programmers
        .reduce((acc, programmer) => {
          if (!programmer.startsWith("J")) {
            return acc.concat(programmer);
          } else {
            return acc;
          }
        }, [])
        .map((programmer) => (
          <li>{programmer}</li>
        ))}
    </ul>
  );
}

export default App;
```

## 5. 객체 트릭 : 속성 속기, 구조 분해, 분산 연산자
 

배열과 마찬가지로 객체는 React를 사용할 때 익숙해 져야하는 데이터 구조입니다.
 

객체는 배열과 달리 체계적인 키-값 저장을 위해 존재하므로 객체 속성에 매우 편안하게 액세스하고 조작 할 수 있어야합니다.
 

객체를 생성 할 때 객체에 속성을 추가하려면 속성과 해당 값의 이름을 지정합니다.
 기억해야 할 매우 간단한 속기 중 하나는 속성 이름이 값과 같으면 속성 이름 만 나열하면된다는 것입니다.
 

다음은 개체 속성의 속기입니다.
 

```undefined
const name = "Reed";

const user = {
  // instead of name: name, we can use...
  name
};

console.log(user.name); // Reed
```

객체에서 속성에 액세스하는 표준 방법은 점 표기법을 사용하는 것입니다.
 그러나 훨씬 더 편리한 접근 방식은 객체 분해입니다.
 이를 통해 주어진 객체에서 동일한 이름의 개별 변수로 속성을 추출 할 수 있습니다.
 

객체를 반대로 작성하는 것처럼 보이므로 프로세스가 직관적입니다.
 값을 가져오고 싶을 때마다 액세스하기 위해 객체 이름을 여러 번 사용하는 것보다 사용하는 것이 훨씬 좋습니다.
 

```undefined
const user = {
  name: "Reed",
  age: 28,
  isEnglishSpeaker: true
};
 
// Dot property access
const name = user.name;
const age = user.age;
 
// Object destructuring
const { age, name, isEnglishSpeaker: knowsEnglish } = user;
// Use ':' to rename a value as you destructure it

console.log(knowsEnglish); // true
```

이제 기존 객체에서 객체를 생성하려면 속성을 하나씩 나열 할 수 있지만 매우 반복적 일 수 있습니다.
 

속성을 수동으로 복사하는 대신 객체 스프레드 연산자를 사용하여 객체의 모든 속성을 다른 객체 (생성 할 때)로 분산 할 수 있습니다.
 

```undefined
const user = {
  name: "Reed",
  age: 28,
  isEnglishSpeaker: true
};

const firstUser = {
  name: user.name,
  age: user.age,
  isEnglishSpeaker: user.isEnglishSpeaker
};

// Copy all of user's properties into secondUser 
const secondUser = {
  ...user  
};
```

개체 스프레드의 장점은 원하는만큼 많은 개체를 새 개체에 펼칠 수 있고 속성처럼 정렬 할 수 있다는 것입니다.
 그러나 나중에 동일한 이름으로 제공되는 속성은 이전 속성을 덮어 씁니다.
 

```undefined
const user = {
  name: "Reed",
  age: 28
};

const moreUserInfo = {
  age: 70,
  country: "USA"
};

// Copy all of user's properties into secondUser 
const secondUser = {
  ...user,
  ...moreUserInfo,
   computer: "MacBook Pro"
};

console.log(secondUser);
// { name: "Reed", age: 70, country: "USA", computer: "Macbook Pro" }
```

## 6 : 약속 + 비동기 / 대기 구문
 

거의 모든 React 애플리케이션은 실행되는 데 무한한 시간이 걸리는 비동기 코드로 구성됩니다.
 특히 Fetch API 또는 타사 라이브러리 axios와 같은 브라우저 기능을 사용하여 외부 API에서 데이터를 가져 오거나 변경해야하는 경우.
 

Promise는 비동기 코드를 해결하는 데 사용되어 정상 동기 코드처럼 해결되며 위에서 아래로 읽을 수 있습니다.
 

Promises는 전통적으로 콜백을 사용하여 비동기 코드를 해결합니다.
 우리는`.then ()`콜백을 사용하여 성공적으로 해결 된 promise를 해결하고`.catch ()`콜백을 사용하여 오류로 응답하는 promise를 해결합니다.
 

다음은 내 프로필 이미지를 표시하기 위해 Fetch API를 사용하여 내 GitHub API에서 데이터를 가져 오기 위해 React를 사용하는 실제 예제입니다.
 데이터는 약속을 사용하여 해결됩니다.
 

```undefined
/* Go to react.new and paste this code in to see it work! */
import React from 'react';
 
const App = () => {
  const [avatar, setAvatar] = React.useState('');
 
  React.useEffect(() => {
    /* 
      The first .then() lets us get JSON data from the response.
      The second .then() gets the url to my avatar and puts it in state. 
    */
    fetch('https://api.github.com/users/reedbarger')
      .then(response => response.json())
      .then(data => setAvatar(data.avatar_url))
      .catch(error => console.error("Error fetching data: ", error);
  }, []);
 
  return (
    <img src={avatar} alt="Reed Barger" />
  );
};
 
export default App;
```

Promise에서 데이터를 확인하기 위해 항상 콜백을 사용해야하는 대신 비동기 코드와 동일하게 보이는 정리 된 구문 인 async / await 구문을 사용할 수 있습니다.
 

async 및 await 키워드는 함수 (React 함수 구성 요소가 아닌 일반 JavaScript 함수)에만 사용됩니다.
 

이를 사용하려면 비동기 코드가 `async`키워드가 앞에 추가 된 함수에 있는지 확인해야합니다.
 모든 promise의 값은 앞에 `await`키워드를 배치하여 확인할 수 있습니다.
 

```undefined
/* Go to react.new and paste this code in to see it work! */
import React from "react";

const App = () => {
  const [avatar, setAvatar] = React.useState("");

  React.useEffect(() => {
    /* 
   Note that because the function passed to useEffect cannot be async, we must create a separate function for our promise to be resolved in (fetchAvatar)
    */
    async function fetchAvatar() {
      const response = await fetch("https://api.github.com/users/reedbarger");
      const data = await response.json();
      setAvatar(data.avatar_url);
    }

    fetchAvatar();
  }, []);

  return <img src={avatar} alt="Reed Barger" />;
};

export default App;

```

우리는`.catch ()`콜백을 사용하여 전통적인 프라 미스 내의 오류를 처리하지만 어떻게 async / await로 오류를 포착합니까?
 

우리는 여전히`.catch ()`를 사용하며, 200 또는 300 상태 범위에있는 API로부터 응답을 받았을 때와 같이 오류가 발생했을 때 :
 

```undefined
/* Go to react.new and paste this code in to see it work! */
import React from "react";

const App = () => {
  const [avatar, setAvatar] = React.useState("");

  React.useEffect(() => {
    async function fetchAvatar() {
      /* Using an invalid user to create a 404 (not found) error */
      const response = await fetch("https://api.github.com/users/reedbarge");
      if (!response.ok) {
        const message = `An error has occured: ${response.status}`;
        /* In development, you'll see this error message displayed on your screen */
        throw new Error(message);
      }
      const data = await response.json();

      setAvatar(data.avatar_url);
    }

    fetchAvatar();
  }, []);

  return <img src={avatar} alt="Reed Barger" />;
};

export default App;

```

## 7. ES 모듈 + 가져 오기 / 내보내기 구문
 

ES6는 ES 모듈을 사용하는 타사 라이브러리뿐만 아니라 자체 JavaScript 파일간에 코드를 쉽게 공유 할 수있는 기능을 제공했습니다.
 

또한 Webpack과 같은 도구를 활용하면 이미지 및 SVG와 같은 자산은 물론 CSS 파일을 가져 와서 코드에서 동적 값으로 사용할 수 있습니다.
 

```undefined
/* We're bringing into our file a library (React), a png image, and CSS styles */
import React from 'react';
import logo from '../img/site-logo.png';
import '../styles/app.css';
 
function App() {
  return (
    <div>
      Welcome!
      <img src={logo} alt="Site logo" />
    </div>
  );
}
 
export default App;
```

ES 모듈의이면에있는 아이디어는 JavaScript 코드를 여러 파일로 분할하여 모듈화하거나 앱 전체에서 재사용 할 수 있도록하는 것입니다.
 

JavaScript 코드에 관한 한 우리는 변수와 함수를 가져오고 내보낼 수 있습니다.
 가져 오기 및 내보내기에는 이름이 지정된 가져 오기 / 내보내기와 기본 가져 오기 / 내보내기의 두 가지 방법이 있습니다.
 

파일 당 기본 가져 오기 또는 내보내기를 수행 할 수있는 것은 한 가지 뿐이며 가져 오기 / 내보내기라는 이름을 원하는만큼 만들 수 있습니다.
 예를 들면 :
 

```undefined
// constants.js
export const name = "Reed";

export const age = 28;

export default function getName() {
  return name;   
}

// app.js
// Notice that named exports are imported between curly braces
import getName, { name, age } from '../constants.js';

console.log(name, age, getName());
```

또한 각 변수 또는 함수 옆 대신 파일 끝에 모든 내보내기를 작성할 수 있습니다.
 

```undefined
// constants.js
const name = "Reed";

const age = 28;

function getName() {
  return name;   
}

export { name, age };
export default getName;

// app.js
import getName, { name, age } from '../constants.js';

console.log(name, age, getName());
```

이름이 지정된 가져 오기에`as `키워드를 사용하여 가져 오는 항목의 별칭을 지정하거나 이름을 바꿀 수도 있습니다.
 기본 내보내기의 이점은 원하는대로 이름을 지정할 수 있다는 것입니다.
 

```undefined
// constants.js
const name = "Reed";

const age = 28;

function getName() {
  return name;   
}

export { name, age };
export default getName;

// app.js
import getMyName, { name as myName, age as myAge } from '../constants.js';

console.log(myName, myAge, getMyName());
```

### 좋아하는 TV 프로그램을 보는 데 걸리는 시간에 React로 연간 $ 100,000의 경력을 시작할 수 있습니다.
 

이 프리미엄 React 교육 과정에서 실제 비용으로 삶을 변화시키는 결과를 제공하는 지식, 기술 및 자신감을 얻을 수 있습니다.
 

수백 명의 개발자가 이미 React를 마스터하고 꿈의 직업을 찾고 미래를 관리하는 데 사용한 내부 정보를 얻으십시오.
 

코스가 열릴 때 알림을 받으려면 여기를 클릭하십시오.
 