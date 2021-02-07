---
layout: post
title: "React Profiler API 및 Profiler DevTool을 사용한 디버깅 React 응용 프로그램"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/reactprofilercomponent.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/reactprofilercomponent.png?fit=730%2C487&ssl=1)

## 전제조건

이 튜토리얼을 최대한 활용하려면 React에 대한 지식이 필요합니다. 또한, React.memo를 이용한 React의 성능 최적화와 구성요소 수준 메모에 대한 지식은 플러스이다.

## 성능 측정 도구

React에는 성능 최적화를 측정하는 몇 가지 도구가 있습니다. 그러나 이 게시물에서 우리는 리액트 프로파일러 디브툴과 리액트 프로파일러 API에 초점을 맞추고 있다.

앞으로 나아가기 전에, 성과는 트레이드오프라는 점을 유념해야 한다. 따라서 최적화 후 애플리케이션 속도가 느려질 수 있습니다.
React Profiler DevTool, React Profiler API 등의 도구를 사용하여 적용된 기법의 성공/실패를 측정하는 것이 중요합니다.

## 메모화에 의한 성능 최적화

이 섹션과 다음 섹션에서는 메모화라는 특정 기술을 사용하여 성능 최적화에 대해 자세히 설명하겠습니다. 그런 다음 예제 응용 프로그램에 적용하고 `React Profiler DevTool`과 `React Profiler API`를 사용하여 효과를 측정하겠습니다.

## 메모란 무엇인가?

> 컴퓨팅에서 메모라이제이션(memoization)은 값비싼 함수 호출의 결과를 저장하고 동일한 입력이 다시 발생할 때 캐시된 결과를 반환하여 컴퓨터 프로그램의 속도를 높이는 데 주로 사용되는 최적화 기법이다.
–위키피디아

애플리케이션은 라이프사이클 전체에 걸쳐 여러 개의 고가의 장기 실행 계산을 수행합니다. 메모화의 아이디어는 이러한 연산을 순수한 기능에서 처리하는 것이다. 이러한 계산에 필요한 모든 종속성은 인수로 전달되고 함수가 반환되면 해당 값이 캐시됩니다. 함수가 동일한 인수를 사용하여 호출될 때마다 캐시된 값이 반환됩니다. 즉, 불필요한 함수 호출이 발생하지 않아 시간이 오래 걸리는 I/O가 순식간에 발생할 수 없다. 이로 인한 성능상의 이점은 매우 큽니다.

암기력은 좋지만 단점이 있다. 서로 다른 종속성에 대한 결과를 계속 저장함에 따라 캐시 크기가 커집니다. 간단히 말해서, 우리는 속도를 위해 공간을 거래하고 있다.

메모에 대한 개요를 제공했는데, 어떻게 대응에 적용됩니까?

## 반응의 메모화

응답 응용프로그램에는 여러 가지 방법으로 메모화를 적용할 수 있습니다. 리액션은 이를 위해 use Memo와 use Callback 후크를 제공한다. `리셀렉트`와 같은 라이브러리는 후드 아래 메모장을 사용하고, `로다시`와 같은 유틸리티 라이브러리는 `.memoize` 기능을 갖는다.

이 튜토리얼에서는 `Ract.memo`로 작업하겠습니다. 이 함수는 반응 함수 구성 요소를 인수로 사용합니다. 동사는 현재 렌더링된 출력을 메모하고 불필요한 리렌더를 건너뛰기 때문에 `prop`가 바뀌어야 부품을 재임대할 수 있다.

다음 중 하나에 들어가기 전에 먼저 성능 규칙을 생각해 보겠습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/performanceillustration.png?resize=730%2C490&ssl=1)

위의 다이어그램은 반응 앱이 콘텐츠를 렌더링하는 방식을 보여줍니다. 응용 프로그램이 장기 실행 계산을 수행하는 동안 로드 구성 요소가 표시되는 것을 알 수 있다. 이 작업이 완료되면 내용이 사용자에게 렌더링됩니다. 또한 `로드 타임`이 장기 실행 계산을 수행하고 적절한 콘텐츠를 렌더링하는 데 걸리는 시간임을 알 수 있다.

장기 실행 컴퓨팅(I/O)은 많은 시간과 리소스를 필요로 하는 작업입니다. 파일 업로드, API 호출(갤러리의 모든 이미지에 대한 요청), 복잡한 계산 등이 가능합니다.

성능 최적화에서 목표는 각 구성 요소의 로드 시간을 측정하고 성능 문제를 일으키는 구성 요소를 식별하여 수정하는 것입니다. 샘플 어플리케이션으로 다음 섹션에서 자세히 설명하겠습니다.

## 성능 최적화 측정

이 섹션을 최대한 활용하려면 리포지토리를 복제하고 다음 단계를 수행하여 로컬 설정을 해야 합니다.

- 리포지토리 복제:

```php
git clone https://github.com/lawrenceagles/profiler-demo
```

- 응용 프로그램 디렉터리로 변경:

```bash
cd profiler-demo
```

- 앱 실행:

```undefined
yarn start or npm start
```

아래 코드를 고려하십시오.

```js
import "./App.css";
import React, { useEffect, useState } from "react";
// component
import Languages from "./components/Languages";
function App () {
    let topLang = [ "JavaScript", "Python", "Rust", "TypeScript", "C++", "C" ]
    const [ languages, setLanaguages ] = useState([]);
    const [ count, setCount ] = useState(0);
    const dummyAPICall = () => setTimeout(() => setLanaguages(topLang), 5000)

    useEffect(() => {
        dummyAPICall();
    }, []);
    return (
        <div className='App'>
            <header className='App-header'>
                {languages.length === 0 && <h2>Welcome!</h2>}
                {languages.length ? (
                    <Languages languages={languages} />
                ) : (
                    <div className='text-warning text-center m-4'>
                        Loading languages...
                    </div>
                )}
                <hr />
                <h4>{count}</h4>
                <button
                    className='mb-3 btn btn-primary'
                    onClick={() => setCount((count) => count + 1)}
                >
                    Add count
                </button>
            </header>
        </div>
    );
}

export default App;
```

위의 코드는 앱의 작은 부분에 불과합니다. 더미(dummy)를 불러 장기연산을 시뮬레이션한다.구성 요소 `마운트` 시 APICall 기능. 이 작업이 완료되면 언어 상태가 업데이트되고 해당 값이 언어 구성요소에 `prop`로 전달됩니다. 이 두 트리거 모두 예상대로 렌더링됩니다. 그리고 언어 목록이 표시됩니다.

또한 앱과의 사용자 상호 작용을 시뮬레이션하는 데 사용되는 버튼이 있습니다. 이 옵션을 클릭하면 카운트 상태가 업데이트되고 렌더링 업데이트가 트리거됩니다. 이 이벤트는 언어 상태나 구성 요소에 영향을 미치지 않으므로 언어 구성 요소를 다시 렌더링해서는 안 됩니다. 그렇죠? 글쎄요, 다시 렌더링이 되고 그게 문제죠.

아래의 구현으로부터:

```js
import React from "react";
import PropTypes from "prop-types";
const Languages = ({ languages }) => {
    return (
        <div>
            <h3>Top 6 Languages:</h3>
            {(console.log("I am mounting!"),
              languages.map((language, index) => <p key={index}> {language} </p>))
            }
        </div>
    );
};
Languages.propTypes = {
    languages : PropTypes.array.isRequired,
};
export default Languages;
```

여기서 언어 구성요소가 `언어 소품`(어레이)을 수신하여 이를 통해 매핑하고 각 배열 항목을 렌더링하는 것을 볼 수 있다. console.log() 문을 추가하였습니다. 이 구성 요소가 다시 렌더링되는 횟수를 추적할 수 있습니다. 그것은 가장 능숙한 도구는 아니지만 꽤 편리하다.

아래 이미지를 고려하십시오.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/languagegetsrerendered.png?resize=730%2C390&ssl=1)

버튼 클릭 횟수만큼(이 경우 15회) 언어 구성요소가 다시 렌더링되는 것을 알 수 있습니다. 이 버튼은 달력 보기, 주석 상자 등과 같은 실제 `UI` 구성 요소를 시뮬레이션하는 데 사용됩니다. 이는 사용자가 날짜를 선택하거나 주석을 달거나 `UI`와 상호 작용할 때 우리의 언어 구성요소가 다시 렌더링됨을 의미한다.

실제 시나리오에서는 이러한 작업이 비용이 많이 들기 때문에 각 렌더 비용을 측정하기 위해 타이밍 정보를 수집해야 합니다. 이를 위해 우리는 `React Profiler DevTool`을 사용할 것이다.

## 프로파일러 개발 도구

Profiler DevTool 플러그인은 React 16.5에 소개되었다. 후드 아래에 있는 리액트 프로파일러 API를 사용하여 렌더링된 각 구성 요소의 타이밍 정보를 수집합니다. 이를 통해 React 애플리케이션의 성능 병목 현상을 식별할 수 있는 유용한 툴이 됩니다. `프로파일러 DevTool`을 사용하여 구성 요소를 프로파일링하려면 다음 단계를 따르십시오.

- 콘솔을 열고 프로파일러 탭을 클릭합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/profilertab.png?resize=730%2C255&ssl=1)

- 이 이미지에서 세 가지 중요한 항목에 레이블을 지정했습니다. 첫 번째(1)는 프로파일러 탭이고 두 번째(2)는 레코드 단추이고 세 번째(3)는 다시 로드 단추입니다.
- 프로파일링을 시작하려면 Record(기록) 또는 reload(재로드) 버튼을 클릭합니다. 여기서는 마운트 단계와 프로파일링의 모든 렌더링을 캡처하기 위해 다시 로드 기능을 사용합니다.
- 다시 로드 버튼을 클릭하고 장기 실행 계산이 완료될 때까지 기다립니다. 언어 구성 요소가 렌더링되면 버튼을 여러 번 클릭한 다음 기록 버튼을 클릭하여 프로파일링을 중지합니다. 이와 유사한 것을 얻을 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/addacount.png?resize=730%2C184&ssl=1)

첫 번째 두 개가 가장 높은 막대 배열과 같은 높이의 녹색 막대 여섯 개를 표시하는 타임라인 섹션을 주목하십시오.

첫 번째는 앱 구성 요소의 `마운트 단계`를 나타내므로 렌더링하는 데 가장 많은 시간이 걸렸다. 렌더링 기간이 6ms임을 알 수 있습니다. 마운팅은 DOM 필수 변경사항이기 때문에 후속 리렌더보다 비싸다. 이후 재렌더는 `DOM`의 일부 값만 변경하고 새 `DOM 노드`는 생성하지 않습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/addcount.png?resize=730%2C188&ssl=1)

6개의 녹색 막대는 클릭 이벤트로 인한 6개의 리렌더를 나타냅니다. 각 막대를 클릭하면 렌더 기간 및 추가 커밋 정보가 표시됩니다.

대부분의 앱 렌더링이 밀리초 단위로 발생하지만 실제 상황에서는 그렇지 않습니다. 렌더링 기간은 쉽게 매우 길어질 수 있으며, 이는 앱 성능에 큰 영향을 미칠 수 있습니다. 이 예제의 목적은 성능 정보를 수집하여 성능 병목 현상을 식별하는 방법을 보여주는 것입니다. 또한 Flamegraph와 순위 채팅에서 더 많은 성능 정보를 확인할 수 있습니다.

우리가 필요로 하는 매우 중요한 정보 중 하나는 각 렌더링 시 `prop`의 상태를 아는 것입니다. 이렇게 하려면 시간 표시줄에서 원하는 막대를 클릭하고 구성 요소 탭을 클릭합니다. 이렇게 하면 렌더링 시 구성 요소 `prop`가 제공됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/languagespropinconsole.png?resize=730%2C187&ssl=1)

위의 이미지에서 각 리렌더 동안 언어 구성요소 특성이 변경되지 않았음을 알 수 있다. 따라서 각 리렌더가 불필요하고 비용이 훨씬 더 많이 들기 때문에 앱을 개선해야 하는 영역입니다.

## 성능 최적화를 위한 메모 적용

앱에서 최적화의 필요성을 파악할 수 있었고, 입력을 측정할 수 있는 방법이 있기 때문에, 앱 최적화를 진행할 수 있습니다.

메모화에서 입력(이 경우 `props`)이 변경되지 않으면 캐시된 값이 반환되어 성능이 향상된다는 점을 기억하십시오. 이를 위해, 우리는 `react.memo`를 사용하여 언어 구성요소를 메모하여 `prop`가 변경되지 않더라도 다시 렌더링되지 않도록 할 수 있다.

따라서 `언어 구성 요소`는 다음과 같이 다시 구현될 수 있습니다.

```js
import React, {memo} from "react";
import PropTypes from "prop-types";
const Languages = ({ languages }) => {
    return (
        <div>
            <h3>Top 6 Languages:</h3>
            {(console.log("I am mounting!"),
              languages.map((language, index) => <p key={index}> {language} </p>))
            }
        </div>
    );
};
Languages.propTypes = {
    languages : PropTypes.array.isRequired,
};
export default memo(Languages);
```

현재 우리는 암기된 언어 구성요소를 수출하고 있는데, 이것은 "props"가 전달되어야만 다시 렌더링될 수 있을 것이다.

이제 효율성을 측정하기 위해 구성요소 프로파일링을 다시 시작할 수 있습니다. 아래 이미지를 고려하십시오.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/languagesdidnotrender.png?resize=730%2C164&ssl=1)

위의 이미지에서 타임라인의 각 렌더링 단계에서 언어 구성요소가 렌더링되지 않았음을 알 수 있다. 따라서 언어 구성요소가 마운트되면 "prop"가 변경될 때만 다시 렌더링됩니다. 이렇게 하면 불필요한 렌더링 비용이 없어지고 결과적으로 앱 성능이 향상됩니다.

React DevTool을 사용하지 않으려면 React 프로파일러 API를 직접 사용할 수 있습니다.

### 프로파일러 구성 요소

프로파일러 API는 성능 정보를 수집하여 렌더링 비용을 측정할 수 있는 구성 요소입니다.

```js
import "./App.css";
import React, { useEffect, useState, Profiler } from "react";
// component
import Languages from "./components/Languages";
function App () {
    let topLang = [ "JavaScript", "Python", "Rust", "TypeScript", "C++", "C" ]
    const [ languages, setLanaguages ] = useState([]);
    const [ count, setCount ] = useState(0);
    const dummyAPICall = () => setTimeout(() => setLanaguages(topLang), 5000)

    useEffect(() => {
        dummyAPICall();
    }, []);
    return (
        <div className='App'>
            <header className='App-header'>
                {languages.length === 0 && <h2>Welcome!</h2>}
                {languages.length ? (
                    <Profiler
                      id="language"
                      onRender={
                          (id, phase, actualDuration) => {
                          console.log({id, phase, actualDuration})
                          }
                      }>
                      <Languages languages={languages} />
                    </Profiler>
                ) : (
                    <div className='text-warning text-center m-4'>
                        Loading languages...
                    </div>
                )}
                <hr />
                <h4>{count}</h4>
                <button
                    className='mb-3 btn btn-primary'
                    onClick={() => setCount((count) => count + 1)}
                >
                    Add count
                </button>
            </header>
        </div>
    );
}

export default App;
```

위에서 우리는 단순히 `프로파일러 API`를 수입하여 언어 구성 요소를 포장하여 실행하였습니다. 두 개의 `프롭`이 필요하며, 이는 다음과 같습니다.

- ID, 즉 문자열
- 온렌더 콜백입니다. 이 콜백에는 여러 가지 주장이 필요하지만 첫 번째 세 가지(id, 위상, 실제 기간)가 필요합니다.

이 `온렌더` 함수에서 우리는 이 세 가지 인수로 콘솔에 기록하고 반대한다. 또한 아래 이미지와 같이 필요한 모든 성능 정보를 제공합니다.

## 마지막 생각

리액트 프로파일러 구성 요소와 리액트 프로파일러 개발 도구는 모두 놀랍고 함께 사용할 수 있다. React Profiler API에 대한 자세한 내용은 여기에서 확인할 수 있습니다.

프로덕션에서 React Profiler API를 사용하지 않는 것이 좋습니다. 하지만, 만약 그것이 매우 필요하다면, 당신은 여기서 그것을 어떻게 사용하는지에 대한 지침을 받을 수 있습니다. 이 게시물이 성능 최적화에 대한 접근 방식을 더 좋게 변화시키기를 바랍니다.