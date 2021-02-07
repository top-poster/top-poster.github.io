---
layout: post
title: "웹 게임에서 반응 사용"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/usingreactinwebgames-nocdn.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/usingreactinwebgames-nocdn.png?fit=730%2C487&ssl=1)

이 기사에서는 리액트가 틱택토 게임을 구축하여 웹 게임을 개발하는 데 어떻게 사용되는지 살펴볼 것이다.

## 도입

공식 문서에 따르면, React는 사용자 인터페이스를 구축하기 위한 자바스크립트 라이브러리이다. 또한 React Native를 사용하여 JavaScript에서 네이티브 애플리케이션을 구축하는 데도 사용할 수 있습니다.

## 전제조건

이 튜토리얼에서는 판독기에 다음이 있다고 가정합니다.

- 시스템에 설치된 Node.js 및 NPM
- JavaScript의 기본 지식 및 React 작동 방식

먼저 공식 React 애플리케이션 비계 툴인 `create-react-app`을 설치합니다.

```undefined
npm install -g create-react-app
```

디렉터리 이름을 지정하도록 요청하는 `생성-react-app`을 실행하여 설치를 확인할 수 있습니다.

## 게임에서 리액트 사용

React를 사용하면 재사용 가능한 구성 요소를 만들 수 있으며, 그 결과 애플리케이션의 여러 부분에 걸쳐 재사용할 수 있는 장치나 구성 요소를 구축할 수 있습니다. 이를 통해 다음 구성 요소를 구축할 것입니다.

- `Box` 구성 요소 - 이 구성 요소는 게임의 박스 구성 요소이며 틱택-토우 개체를 처리하는 장치입니다.
- 게임 구성 요소 - "게임 오버" 및 "리셋"과 같은 게임 로직을 초기 상태 기능으로 처리합니다.
- 레이아웃 구성 요소 - 이 구성 요소는 게임의 레이아웃을 처리합니다. 우리는 우리의 레이아웃을 만들기 위해 CSS 그리드를 사용할 것이다.

## 시작 중

"프로젝트를 부트스트랩하기 위해 `크리에이트-리액트-앱`을 사용할 것이며, `크리에이트-리액트-앱`은 개발자들이 단기간에 `리액트` 프로젝트를 시작할 수 있도록 페이스북과 커뮤니티가 관리하는 오픈 소스 툴이다.

`create-react-app` 상용판을 사용하여 새 프로젝트를 생성하려면 원하는 터미널에서 다음 명령을 실행합니다.

```undefined
create-react-app react-tictactoe
```

"react-ticactoe"라는 이름은 이 튜토리얼의 프로젝트 이름으로 사용되며, 사용할 이름으로 바꿀 수 있습니다.

그런 다음 프로젝트 디렉터리로 변경하고 다음을 실행하여 개발 서버를 시작하십시오.

```bash
cd react-tictactoe && npm start
```

이 명령을 실행하면 브라우저 탭이 열리고 기본 보일러 플레이트 응용 프로그램이 렌더링됩니다. 그런 다음 아래 이미지에 있는 것처럼 보이도록 응용 프로그램 폴더를 재구성합니다.

![image](https://blog.logrocket.com/wp-content/uploads/2020/10/folderstructure-nocdn.png)

이렇게 하려면 `src` 폴더에서 `App.css`, `App.test.js`, `serviceWorker.js` 및 `setup` 파일을 삭제하십시오.Tests.js`입니다.

이 파일들은 반응 애플리케이션의 테스트 설정에 주로 사용되며, 우리는 인라인 CSS로 작업할 것이기 때문에 App.css는 필요하지 않을 것이다. 이러한 파일을 제거하면 브라우저에 다음과 같은 오류가 발생합니다.

![image](https://blog.logrocket.com/wp-content/uploads/2020/10/failedtocompilemessage-nocdn.png)

위의 문제를 해결하기 위해, 우리는 우리의 `src` 디렉토리 안에 있는 `App.js` 파일로 가서 아래의 코드 줄들을 제거한다.

```coffeescript
 import logo from './logo.svg';
 import './App.css';
```

그럼 이제 게임 구성 요소 구축을 시작해 보겠습니다.

## 게임 구성 요소 구축

박스 구성품부터 시작하겠습니다.

### 박스 구성요소 작성

`src` 폴더에 구성 요소 폴더를 생성하고 구성 요소 폴더에 `Box.js`라는 파일을 생성해 보겠습니다. 파일 이름은 원하는 대로 지정할 수 있습니다.

다음으로, 우리는 `값`과 `클릭` 속성을 가진 기능적 구성 요소를 생성하는데, 이 구성 요소는 다음과 같은 속성이 구조화된 버튼 태그를 반환합니다.

```js
import React from 'react';
function Box({ value, onClick }) {
    return (
        <button
            style={style}
            onClick={onClick}>
            {value}
        </button>
    );
}
export default Box;
```

위의 코드 블록에서 Box 구성 요소는 `값`과 `onClick` 소품을 받아들인다. 왜냐하면 우리 어플리케이션은 3행 3열의 상자 모양의 형태를 갖게 될 것이고 각각의 상자를 클릭하면 `값`이 나타나기 때문이다. 브라우저를 새로 고치기 전에 `Box` 구성 요소에 포함시킨 대로 `style`을 작성하겠습니다.

```undefined
const style = {
    background: '#fff',
    border: '2px solid lightblue',
    fontSize: '30px',
    fontWeight: '800',
    cursor: 'pointer',
    outline: 'none'
}
```

### 레이아웃 구성요소 작성

이 구성 요소에서는 Box 구성 요소를 가져와 게임의 안정적인 사용자 인터페이스를 만들기 위한 백본으로 사용할 것이며, 이를 위해서는 Box 구성 요소에 있는 것과 유사한 기능 구성 요소를 만들어야 하며, 그 다음 기능 소품으로 "Box"와 "onClick"을 전달하여 "Box" 구성 요소를 매핑할 것이다.ent를 입력하여 3개의 행과 3개의 열을 생성합니다.

```js
import React from 'react';
import Box from './Box'

function Layout({boxes, onClick }) {
    return (
      <div style={style}>
        {boxes.map((box, i) => (
          <Box key={i} value={box} onClick={() => onClick(i)} />
      ))}
      </div>
    );
}
export default Layout;
```

위의 코드 블록에서는 앱에 3개의 행과 열이 있을 때까지 "Box" 구성 요소에 스타일 객체를 추가하기 위해 필요한 수의 상자를 추가하지 않을 때까지 "Box" 구성 요소에 있는 상자를 매핑합니다.

```undefined
const style = {
  border: '4px solid lightblue',
  borderRadius: '10px',
  width: '320px',
  height: '320px',
  margin: '0 auto',
  display: 'grid',
  gridTemplate: 'repeat(3, 1fr) / repeat(3, 1fr)'
};
```

위의 스타일에서, 우리는 세 개의 행과 열을 가지려는 경우에 응용 프로그램에서 원하는 행과 열의 수를 지시하기 위해 그리드 템플릿을 추가했습니다.

### 게임 구성 요소

이 구성 요소는 우리의 게임 응용 프로그램의 주요 구성 요소이며, 여기서는 승자 선언을 포함한 대부분의 논리를 만들 것입니다.

먼저 사용자의 클릭을 처리하기 위해 나중에 사용할 `레이아웃` 구성 요소를 확인하는 기능을 가져오겠습니다. 다음으로, "div" 태그를 추가하고 그 안에 당첨자를 표시하는 단락을 추가할 것이며, 곧 자동으로 다음 사항을 결정하는 데 도움이 되는 기능을 추가할 것입니다.

```js
import React, { useState } from 'react';
import Layout from './Layout';

const styles = {
    width: '200px',
    margin: '20px auto',
};
const pStyle = {
    color: 'green'
}

function Game() {
    return (
        <React.Fragment>
            <Layout boxes={layout} />
            <div style={styles}>
                <p style={pStyle}> The winner goes here
              </p>        
            </div>
        </React.Fragment>
    )
}
export default Game;
```

위의 코드에서 게임 구성 요소는 리액트로 싸여 있습니다."조각" 다음으로, 우리는 우리의 `레이아웃` 부품을 소품으로 추가하고 우승자를 표시하는 `p` 태그를 추가했다.

다음으로는 응용 프로그램에 상태를 추가하고 구성 요소에 이벤트 수신기를 추가하는 것입니다. 이것은 우리에게 우승자가 있을 때, 우리의 승자를 선언하기 위한 `p` 태그를 표시합니다.

```php
function Game() {
    const [layout, setLayout] = useState(Array(9).fill(null));
    const [xIsNext, setXisNext] = useState(true);
    const winner = checkWinner(layout)

    const handleClick = (i) => {
        const layoutState = [...layout];
        if (winner || layoutState[i]) return;
        layoutState[i] = xIsNext ? 'X' : 'O';
        setLayout(layoutState);
        setXisNext(!xIsNext);
    }

    return (
            <React.Fragment>
            <Layout boxes={layout} onClick={handleClick} />
            <div style={styles}>
                <p style={pStyle}>
                    {winner ? 'Winner: ' + winner : 'Next Player '
                    + (xIsNext ? 'X' : 'O')}
                </p>

            </div>
        </React.Fragment>
            )
}
export default Game;
```

위의 코드 블록에서는 useState 후크를 사용하여 레이아웃의 상태를 만들고 상자 배열을 사용하고 있습니다. 다음으로 게임 중 클릭할 다음 문자를 알려주는 xIsNext(xIsNext) 상태를 설정했다.

마지막 변수인 우승자는 일단 게임에서 승리자가 발견되면 한 번의 클릭을 막을 수 있다. handleClick 기능은 우리의 상태를 직접 변형시키는 대신 우리의 layoutState의 복사된 버전을 취하며 layoutState는 빈 세트를 반환한다.

다음으로 빈 상자를 클릭하면 `xIsNext` 상태가 빈 상자에서 X 또는 O를 렌더링하고 레이아웃 상태로 설정합니다.

### 게임 논리 구축

이 구성 요소는 게임이 완료되기 전에 마지막으로 작업할 구성 요소입니다. 여기서는 게임의 승자를 계산하는 내보내기 기능을 사용할 것입니다.

```undefined
export function checkWinner(boxes) {
 const lines = [
   [0, 1, 2],
   [3, 4, 5],
   [6, 7, 8],
   [0, 3, 6],
   [1, 4, 7],
   [2, 5, 8],
   [0, 4, 8],
   [2, 4, 6],
 ];
 for (let i = 0; i < lines.length; i++) {
   const [x, y, z] = lines[i];
   if (boxes[x] && boxes[x] === boxes[y] && boxes[x] === boxes[z]) {
     return boxes[x];
   }
 }
 return null;
}
const boxes = [null, null, null, "X", "X", "O", null, null, null];
console.log(checkWinner(boxes));
```

위의 코드 블록에서 우리는 승자를 확인하는 `체크위너`라는 함수를 쓰는데, 이 함수는 `박스`를 인수로 받아들인다. 다음으로, 우리는 0부터 8까지 숫자를 세는 모든 성공적인 동작을 포함하는 룩업 어레이를 만들었기 때문에, 우리는 박스를 순환하는 대신 룩업 어레이를 순환할 것이다. 다음으로, ES6 구문을 사용하여 우승 상자를 파괴하고 기본 문자 x, y, z를 준 다음, 롤에 X 또는 O가 있는지 확인합니다. 만약 우리가 그렇게 한다면, 우리는 그 길을 계속 갈 것이고, 그렇지 않으면 다음 편지는 우리가 얻은 것과 반대일 것이다. 그렇지 않으면 첫 번째 값이 두 번째 값과 일치하는지 확인하고 두 값이 세 번째 값과 일치하는지 확인한 다음, 이 값이 모두 수행되지 않으면 null을 반환합니다. 즉, 승자가 없다는 의미입니다.

이 프로젝트를 마치면 다음과 같이 보일 것입니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/tictactoe.png?resize=755%2C467&ssl=1)

## 결론

이 게시물에서는 React에 대해 알아보고 React를 사용하여 웹 게임을 구축하는 방법과 ES JavaScript를 사용하여 게임 로직을 작성하는 방법 및 React Hooks를 상태 관리에 사용하는 방법에 대해서도 알아봤습니다. 여기서 LogRocket에서 반응 기사를 더 많이 읽을 수 있습니다. 이 기사에 사용된 코드는 깃허브에서 찾을 수 있으며, 이 게임의 작동 버전은 여기에서 찾을 수 있다.