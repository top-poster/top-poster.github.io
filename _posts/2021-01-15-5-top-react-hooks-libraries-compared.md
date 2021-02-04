---
layout: post
title: "5 개의 상위 React Hooks 라이브러리 비교
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/top-react-hooks-libraries-compared.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/top-react-hooks-libraries-compared.png?fit=730%2C487&ssl=1)

React의 Hooks API는 이제 구성 요소를 만드는 사실상의 방법입니다.
 클래스 구성 요소 API와 함께 공존하므로 JavaScript 클래스로 구성 요소를 만들 수 있습니다.
 

React 라이브러리와 함께 제공되는 표준 후크 외에도 개발자는 자체 후크를 만들 수 있습니다.
 당연히 React 앱을 더 쉽게 만들 수 있도록 많은 커스텀 Hooks 라이브러리가 등장했습니다.
 

이 기사에서는 5 개의 유용한 React Hooks 라이브러리를 살펴보고 그 유틸리티를 비교해 보겠습니다.
 

## 1. React Hooks Lib
 

React Hooks Lib는 React 클래스 컴포넌트의 라이프 사이클 메소드와 유사한 Hooks를 제공합니다.
 

예를 들어 컴포넌트가 마운트 될 때 실행되는`useDidMount`와 컴포넌트가 업데이트 될 때 실행되는`useDidUpdate`와 같은 Hooks를 제공합니다.
 또한 `useCounter`와 같이 상태를 관리하기위한 후크가 있는데,이를 관리하기위한 자체 기능이있는 숫자 상태를 추가합니다.
 

패키지를 설치하려면 다음을 실행할 수 있습니다.
 

```coffeescript
npm i react-hooks-lib --save
```

그런 다음 모듈에서 가져 와서 후크 중 일부를 사용할 수 있습니다.
 다음과 같이 작성하여`useDidMount` 후크를 사용할 수 있습니다.
 

```js
import React from "react";
import { useDidMount } from "react-hooks-lib";

export default function App() {
  useDidMount(() => {
    console.log("did mount");
  });

  return (
    <div className="App">
      <h1>Hello world</h1>
    </div>
  );
}
```

마찬가지로 다음과 같이 작성하여`useCounter` 후크를 사용할 수 있습니다.
 

```js
import React from "react";
import { useCounter } from "react-hooks-lib";

export default function App() {
  const { count, inc, dec, reset } = useCounter(0);
  return (
    <div>
      {count}
      <button onClick={() => inc(1)}>increment</button>
      <button onClick={() => dec(1)}>decrement</button>
      <button onClick={reset}>reset</button>
    </div>
  );
}
```

`count` 상태와`inc`,`dec`,`reset` 메소드가있는 객체를 반환합니다.
 이 방법의 기능을 분석해 보겠습니다.
 

- `inc`는 주어진 숫자만큼`count`를 증가시킵니다.
 
- `dec`은 주어진 숫자만큼`count`를 감소시킵니다.
 
- `reset`은`count`를 초기 값으로 재설정합니다.
 

React Hooks Lib에는 입력 값을 쉽게 얻고 설정할 수 있도록`useField` Hook이 함께 제공됩니다.
 다음과 같이 작성하여 사용할 수 있습니다.
 

```js
import React from "react";
import { useField } from "react-hooks-lib";

export default function App() {
  const { value, bind } = useField("text");

  return (
    <div>
      <input type="text" {...bind} />
      <p>{value}</p>
    </div>
  );
}
```

전체`bind` 객체를 입력에 전달하고이를 사용하여 입력에서 데이터를 가져와`value`의 값으로 설정할 수 있습니다.
 `bind`에는 입력 값을 설정하는`value` 속성도 있습니다.
 

또한 선택 요소와 함께 작동합니다.
 

```js
import React from "react";
import { useField } from "react-hooks-lib";

export default function App() {
  const { value, bind } = useField("text");

  return (
    <div>
      <select {...bind}>
        <option value="apple">apple</option>
        <option value="orange">orange</option>
      </select>
      <p>{value}</p>
    </div>
  );
}
```

React Hooks Lib는 유사한 기능을 제공하는 다른 많은 Hooks와 함께 제공됩니다.
 전반적으로 몇 가지 편리한 작업을 수행 할 수있는 사용자 지정 후크를 제공합니다.
 그러나 제공된 모든 후크가 문서화되어있는 것은 아닙니다.
 

## 2. 반응 행거
 

react-hanger는 다양한 상태를보다 쉽게 관리 할 수 있도록 React Hooks를 제공하는 라이브러리입니다.
 다음 후크와 함께 제공됩니다.
 

- `useInput` – 입력 제어 값 가져 오기 및 설정
 
- `useBoolean` – 부울 상태 가져 오기 및 설정
 
- `useNumber` – 숫자 상태 가져 오기 및 설정
 
- `useArray` – 배열 상태 가져 오기 및 설정
 
- `useOnMount` – 구성 요소가 마운트 될 때 코드 실행
 
- `useOnUnmount` – 구성 요소가 마운트 해제 될 때 코드 실행
 
- `useStateful` – 구성 요소 상태 가져 오기 및 설정
 

다음과 같이`useInput`을 사용하여 입력 값을 더 간단하게 처리 할 수 있습니다.
 

```js
import React from "react";
import { useInput } from "react-hanger";

export default function App() {
  const input = useInput("");

  return (
    <div>
      <input type="text" value={input.value} onChange={input.onChange} />
      <p>{input.value}</p>
    </div>
  );
}
```

`useNumber`를 사용하여 숫자 상태를 가져오고 설정할 수 있습니다.
 

```js
import React from "react";
import { useNumber } from "react-hanger";

export default function App() {
  const counter = useNumber(3, { lowerLimit: 0, upperLimit: 5 });

  return (
    <div>
      <p> {counter.value} </p>
      <button onClick={() => counter.increase()}> increase </button>
      <button onClick={() => counter.decrease()}> decrease </button>
    </div>
  );
}
```

첫 번째 인수로 `counter.value`의 초기 값을 설정하고 두 번째 인수로 `counter.value`상태의 하한과 상한을 설정할 수 있습니다.
 `useArray` 및`useBoolean` 후크는 유사한 방식으로 작동합니다.
 

모든 종류의 상태를 설정하려면`useStateful` 후크를 사용할 수 있습니다.
 

```js
import React from "react";
import { useStateful } from "react-hanger";

export default function App() {
  const username = useStateful("tom");

  return (
    <div>
      <p> {username.value} </p>
      <button onClick={() => username.setValue("tom")}> tom </button>
      <button onClick={() => username.setValue("jerry")}> jerry </button>
    </div>
  );
}
```

`value`속성에서 상태 값을 가져올 수 있고 반환 된 객체의 `setValue`메서드로 값을 설정할 수 있다는 점에서 React의 `useState`Hook보다 낫습니다.
 React의 내장 된`useState` Hook과 달리 상태 및 setter 함수를 가져 오는 데 구조화가 필요하지 않습니다.
 

## 3. React hookedUp
 

수많은 다른 라이브러리와 마찬가지로 React hookedUp을 사용하면 구성 요소 상태를 관리 할 수 있습니다.
 그러나이를 사용하여 HTML 요소의 포커스 및 호버링을 관리 할 수도 있습니다.
 

클래스 컴포넌트 라이프 사이클 후크의 기능을 복제하는 후크와 지금까지 검토 한 다른 라이브러리에서 제공하지 않은 몇 가지 유용한 타이머 및 네트워크 후크가 함께 제공됩니다.
 

입력 위로 마우스를 가져 가면 감지하는`useHover` 후크를 살펴 보겠습니다.
 

```js
import React from "react";
import { useHover } from "react-hookedup";

export default function App() {
  const { hovered, bind } = useHover();

  return (
    <div>
      <p>{hovered ? "hovered" : "not hovered"}</p>
      <input {...bind} />
    </div>
  );
}
```

우리는 단지 전체`bind` 객체를`input`에 대한 props로 퍼뜨립니다.
 `useFocus` Hook도 비슷한 방식으로 사용할 수 있습니다.
 

클래스 컴포넌트에서 좋은 ol ``componentDidMount` 메소드를 놓친 경우`useOnMount` 후크를 사용하여 함수 컴포넌트에서 동일한 기능을 제공 할 수 있습니다.
 

```js
import React from "react";
import { useOnMount } from "react-hookedup";

export default function App() {
  useOnMount(() => console.log("mounted"));
  return <div> hello world </div>;
}
```

코드에서`setInterval`을 호출하려면`useInterval` 후크를 사용할 수 있습니다.
 예를 들어 다음과 같이 작성할 수 있습니다.
 

```js
import React, { useState } from "react";
import { useInterval } from "react-hookedup";

export default function App() {
  const [time, setTime] = useState(new Date().toString());

  useInterval(() => setTime(new Date().toString()), 1000);

  return <p>{time}</p>;
}
```

`setInterval`함수와 같이 첫 번째 인수로 실행되도록 콜백을 전달하고 두 번째 인수로 간격을 전달합니다.
 

또한 주어진 지연 후 콜백을 실행하기위한`useTimeout` 후크도 함께 제공됩니다.
 

```js
import React from "react";
import { useTimeout } from "react-hookedup";

export default function App() {
  useTimeout(() => alert("hello world"), 1500);

  return <h1>hello world</h1>;
}
```

React hookedUp의 또 다른 유용한 Hook은 앱의 온라인 상태를 볼 수있는`useOnlineStatus`입니다.
 예를 들어 다음과 같이 작성할 수 있습니다.
 

```js
import React from "react";
import { useOnlineStatus } from "react-hookedup";

export default function App() {
  const { online } = useOnlineStatus();

  return <h1>{online ? "online" : "offline"}</h1>;
}
```

기기가 온라인 일 때 `true`인 `online`속성을 반환합니다.
 

앞서 언급했듯이 React hookedUp에는 앞서 살펴본 라이브러리와 함께 제공되는 상태 관리 후크 모음도 함께 제공됩니다.
 이들은 다른 라이브러리에서 작동하는 방식과 유사하게 작동합니다.
 

## 4. 반응 사용
 

react-use Hooks 라이브러리는 브라우저가 액세스 할 수있는 다양한 하드웨어를 활용하기위한 Hooks를 포함하여 지금까지 나열된 다른 라이브러리보다 더 많은 Hooks 컬렉션과 함께 제공됩니다.
 또한 화면 크기, 동작, 스크롤, 애니메이션을보고 CSS를 동적으로 조정할 수있는 후크도 함께 제공됩니다.
 

예를 들어`useMouse` 후크를 사용하여 사용자의 마우스 위치를 볼 수 있습니다.
 

```js
import React from "react";
import { useMouse } from "react-use";

export default function App() {
  const ref = React.useRef(null);
  const { docX, docY, posX, posY, elX, elY, elW, elH } = useMouse(ref);

  return (
    <div ref={ref}>
      <div>
        Mouse position in document - ({docX}, {docY})
      </div>
      <div>
        Mouse position in element - ({elX}, {elY})
      </div>
      <div>
        Element position- ({posX} , {posY})
      </div>
      <div>
        Element dimensions - {elW}x{elH}
      </div>
    </div>
  );
}
```

문서에서 마우스의 x, y 좌표를 얻기 위해`docX`와`docY`를 사용할 수 있습니다.
 또한`elX` 및`elY` 속성을 사용하여 모니터링중인 요소에서 마우스의 x 및 y 좌표를 가져올 수 있습니다.
 보고 싶은 요소에 ref를 할당합니다.
 

마찬가지로`useScroll` 후크를 사용하여 요소의 스크롤 위치를 추적 할 수 있습니다.
 

```js
import React from "react";
import { useScroll } from "react-use";

export default function App() {
  const scrollRef = React.useRef(null);
  const { x, y } = useScroll(scrollRef);

  return (
    <div ref={scrollRef} style={{ height: 300, overflowY: "scroll" }}>
      <div style={{ position: "fixed" }}>
        <div>x: {x}</div>
        <div>y: {y}</div>
      </div>
      {Array(100)
        .fill()
        .map((_, i) => (
          <p key={i}>{i}</p>
        ))}
    </div>
  );
}
```

스크롤 위치를보고 싶은 요소에 ref를 할당하고 내용을 스크롤 할 수 있도록`height`와`overflowY`를`scroll`로 설정합니다.
 `x` 및`y` 속성이 반환 된 스크롤 위치를 얻습니다.
 

또한 Hooks react-use가 애니메이션에 제공하는 것을 사용할 수 있습니다.
 예를 들어`useSpring` 후크를 사용하여 숫자 표시에 애니메이션을 적용 할 수 있습니다.
 

```js
import React, { useState } from "react";
import useSpring from "react-use/lib/useSpring";

export default function App() {
  const [target, setTarget] = useState(50);
  const value = useSpring(target);

  return (
    <div>
      {value}
      <br />
      <button onClick={() => setTarget(0)}>Set 0</button>
      <button onClick={() => setTarget(200)}>Set 100</button>
    </div>
  );
}
```

애니메이션 할 숫자를`useSpring` Hook에 전달합니다.
 그런 다음 `타겟`에 도달 할 때까지 숫자가 애니메이션됩니다.
 이 후크가 작동하려면 `리바운드`가 필요합니다.
 

라이브러리에는 데이터를 클립 보드에 복사하고 로컬 저장소를 조작하는 것과 같은 다양한 부작용을 커밋 할 수있는 후크도 있습니다.
 클립 보드에 복사 기능을 추가하려면`useCopyToClipboard` 후크를 사용할 수 있습니다.
 

```js
import React, { useState } from "react";
import useCopyToClipboard from "react-use/lib/useCopyToClipboard";

export default function App() {
  const [text, setText] = useState("");
  const [state, copyToClipboard] = useCopyToClipboard();

  return (
    <div>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      <button type="button" onClick={() => copyToClipboard(text)}>
        copy text
      </button>
      {state.error ? (
        <p>error: {state.error.message}</p>
      ) : (
        state.value && <p>Copied {state.value}</p>
      )}
    </div>
  );
}
```

컴포넌트에서`useCopyToClipboard`를 호출하여`copyToClipboard` 함수를 가져온 다음이를 호출하여 클립 보드에 인수로 전달 된 모든 것을 복사 할 수 있습니다.
 

마찬가지로`useLocalStorage` 후크를 통해 로컬 저장소로 쉽게 작업 할 수 있습니다.
 

```js
import React from "react";
import useLocalStorage from "react-use/lib/useLocalStorage";

export default function App() {
  const [value, setValue, remove] = useLocalStorage("key", "foo");

  return (
    <div>
      <div>Value: {value}</div>
      <button onClick={() => setValue("bar")}>bar</button>
      <button onClick={() => setValue("baz")}>baz</button>
      <button onClick={() => remove()}>Remove</button>
    </div>
  );
}
```

`value`는 주어진 키가있는 로컬 저장소 항목의 값을 가지며, `setValue`는 설정할 값을 전달하고, `remove`는 로컬 저장소에서 항목을 제거합니다.
 

리 액트 사용에 고유 한 앞서 언급 한 후크 외에도 라이브러리에는 상태 설정, 브라우저 API 사용, 비동기 코드 실행 등을위한 많은 후크가 제공됩니다.
 react-use는 지금까지 살펴본 것 중 가장 포괄적 인 Hooks 라이브러리입니다.
 

## 5. 반응 레시피
 

React Recipes는 많은 커스텀 Hooks와 함께 제공되는 또 다른 Hooks 라이브러리입니다.
 브라우저 API를 사용하고 상태를 관리하며 비동기 코드를 실행하는 후크와 같이 반응 사용과 동일한 후크를 많이 제공합니다.
 

예를 들어`useSpeechSynthesis` 후크를 사용하여 브라우저가 말하도록 할 수 있습니다.
 

```js
import React, { useState } from "react";
import { useSpeechSynthesis } from "react-recipes";

export default function App() {
  const [value, setValue] = useState("");
  const [ended, setEnded] = useState(false);
  const onBoundary = (event) => {
    console.log(`${event.name}: ${event.elapsedTime} milliseconds.`);
  };
  const onEnd = () => setEnded(true);
  const onError = (event) => {
    console.warn(event);
  };

  const {
    cancel,
    speak,
    speaking,
    supported,
    voices,
    pause,
    resume
  } = useSpeechSynthesis({
    onEnd,
    onBoundary,
    onError
  });

  if (!supported) {
    return "Speech is not supported.";
  }

  return (
    <div>
      <input value={value} onChange={(event) => setValue(event.target.value)} />
      <button
        type="button"
        onClick={() => speak({ text: value, voice: voices[1] })}
      >
        Speak
      </button>
      <button type="button" onClick={cancel}>
        Cancel
      </button>
      <button type="button" onClick={pause}>
        Pause
      </button>
      <button type="button" onClick={resume}>
        Resume
      </button>
      <p>{speaking && "Voice is speaking"}</p>
      <p>{ended && "Voice has ended"}</p>
      <div>
        <h2>Voices:</h2>
        <div>
          {voices.map((voice) => (
            <p key={voice.name}>{voice.name}</p>
          ))}
        </div>
      </div>
    </div>
  );
}
```

다양한 속성을 가진 객체를 반환하는`useSpeechSynthesis` 후크를 호출합니다.
 

- `취소`는 음성 합성을 취소합니다.
 
- `speak`는 브라우저가 말하기 시작하도록합니다.
 
- `speaking`은 음성 합성이 진행 중인지 알려주는 부울입니다.
 
- `supported`는 현재 브라우저에서 음성 합성이 지원되는지 여부를 알려주는 부울입니다.
 
- `voices`에는 선택할 수있는 음성 목록이 있습니다.
 
- `pause`는 말하기를 일시 중지합니다.
 
- `resume`을 사용하면 말하기를 다시 시작할 수 있습니다.
 

여기에 나열된 많은 Hooks와 함께 제공되며 React 자체에서 제공하지 않는 많은 기능을 제공합니다.
 정말 잘 문서화되고 포괄적 인 라이브러리라는 것도 가치가 없습니다.
 

## 판결?
 verified_user

우리가 오늘 검토 한 가장 포괄적이고 유용한 React Hooks 라이브러리는 react-use와 React Recipes입니다.
 다양한 사용 사례에 대한 많은 후크를 제공하므로 처음부터 직접 작성할 필요가 없습니다.
 

React Hooks Lib, react-hanger 및 React hookedUp은 상태 관리를 어느 정도 단순화 할 수있는 상태 관리를위한 몇 가지 기본 후크를 제공합니다.
 이것이 Hooks 라이브러리에서 우리가 찾고있는 전부라면 React Hooks Lib, react-hanger 및 React hookedUp이 유용합니다.
 모두 사용하기 쉬우 며 모두 (React Hooks Lib 제외) 명확하게 문서화되어 있습니다.
 