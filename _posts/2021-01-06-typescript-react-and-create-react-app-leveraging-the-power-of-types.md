---
layout: post
title: "TypeScript, React 및 Create-react-app: Type의 기능 활용"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/TypeScript-create-react-app-feature_.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/TypeScript-create-react-app-feature_.png?fit=730%2C487&ssl=1)

자바스크립트는 브라우저를 넘어 API, 모바일 개발, 데스크톱 애플리케이션 등으로 영역을 확장하고 있는 환상적인 프로그래밍 언어이다. 그러나 강력한 기능과 향상된 잠재력이 있는 JavaScript에는 코드 구성 및 효율성, 즉 유형을 지원하는 중요한 기능이 없습니다.

예를 들어, 다음 코드를 살펴보고 분석하겠습니다.

```undefined
let someText = "Hello World!";
someText = 123;
someText = [123, 'hello', { isThisCorrect: true }]
```

대부분의 프로그래밍 언어에서 이것을 코딩하는 것은 작동하지 않지만, 자바스크립트는 그것을 허용한다. 변수를 문자열로 만든 다음 숫자, 개체 또는 배열을 할당할 수 있습니다. 하지만 여기에는 장단점이 있습니다.

애플리케이션이 커지고, 시스템이 복잡해지고, 프로젝트의 의존성이 증가함에 따라, 특히 프레임워크와 외부 의존성으로 작업할 때 개발자는 변수, 속성 및 방법을 완전히 이해하기가 어려울 수 있습니다.

이 개체가 원격 가져오기 작업의 결과라도 IDE가 입력하는 동안 개체의 속성을 제안하고 자동 완료할 수 있다면 좋지 않을까요? 아니면 변수를 검사하여 모든 변수를 쉽게 확인할 수 있을까요?

이것은 정확히 TypeScript가 변수에 Type을 추가하여 해결하는 문제입니다.

이제 TypeScript를 실제로 보려면 React 프레임워크를 사용하여 간단한 애플리케이션을 개발하고 TypeScript를 지원하는 CRA(Create-react-app)를 사용해 보겠습니다.

## TypeScript를 사용하여 새 프로젝트 설정

시작하려면 TypeScript에서 앱을 설정하고 다음 명령으로 create-react-app을 사용하여 웹 팩, Babel 및 종속성을 사전 구성합니다.

```undefined
npx create-react-app my-app --template typescript
```

프로젝트를 시작하려면 다음을 실행하십시오.

```bash
cd my-app
npm start
```

이 사이트는 JavaScript 버전과 유사합니다. 하지만 코드를 살펴보기 시작하면 몇 가지 차이점을 발견할 수 있습니다.

## TypeScript 컴파일러

이제 `App.tsx` 파일부터 구성 요소 및 테스트에 대해 자세히 살펴보겠습니다. 가장 먼저 눈에 띄는 것은 파일 확장명입니다. 이제 모든 구성 요소 파일이 `jsx`가 아닌 `tsx`이며 마찬가지로 `setup`과 같은 파일입니다.Tests.js는 이제 `setup`입니다.Tests.ts. 그런데 왜?

현재 브라우저와 노드는 TypeScript에 대한 직접적인 지원을 제공하지 않으며, JavaScript만 이해하므로 TypeScript 파일을 JavaScript 파일로 변환해야 합니다. TypeScript 컴파일러가 이 작업을 수행합니다.

컴파일러는 순수 자바스크립트 파일 대 컴파일할 파일을 알아야 하기 때문에 위에서 언급한 것과 같은 다른 파일 확장자가 사용된다.

## TypeScript에서 변수 입력

JavaScript는 입력되지 않으므로 변수에 데이터 유형을 할당할 수 없지만 JavaScript는 여전히 숫자, 문자열 또는 개체 간의 차이를 이해합니다. JavaScript는 변수의 유형을 포함하는 값으로 추론하기 위해 런타임에 최선을 다합니다.

다음 예를 살펴보겠습니다.

```bash
let helloWorld = "Hello World";
```

helloWorld라는 변수에 포함된 값이 문자열이기 때문에 해당 변수는 문자열 유형이며 문자열에서 일반적으로 수행할 수 있는 모든 작업을 수행할 수 있습니다. 그러나 나중에 해당 변수가 변경되면 변수의 유형이 변경됩니다.

TypeScript는 이 방식을 채택하고 선언 시 값에 따라 변수를 할당하므로 TypeScript에서 동일한 코드 줄이 작동합니다. 이 차이는 변수에 다른 유형의 새 값을 할당할 때 발생하는 작업에 따라 달라집니다.

우리는 이미 자바스크립트에서 당신이 그것을 할 수 있다는 것을 알고 있었지만, 우리가 TypeScript로 같은 시도를 한다면 어떻게 될까요?

```undefined
let helloWorld = "Hello World";
helloWorld = 10;
```

흥미롭게도, IDE(VS Code, WebStorm 또는 TypeScript를 지원하는 다른 모든)는 이미 문제를 감지하고 빨간색으로 변수를 강조 표시했으며 이에 대한 정보를 제공해 주었다.

또한 컴파일 프로세스에서 오류가 발생하여 응용 프로그램 실행을 계속할 수 없습니다.

지금까지, 우리는 TypeScript가 변수 데이터 유형을 결정하도록 한다. 이를 추론으로 유형이라고 한다. 그러나 다른 경우에는 값이 아직 없거나 변수가 둘 이상의 유형을 가질 수 있기 때문에 변수에 수동으로 유형을 할당할 수도 있습니다.

이제 명시적 유형을 사용하여 변수를 선언합니다.

```undefined
let helloWorld:string  = "Hello World";
```

쉽게 변수 이름 바로 뒤에 선언하는 동안 형식을 전달할 수 있습니다.

## 반응 > 3TypeScript

React의 기본 프로젝트 생성기는 JavaScript와 함께 작동하지만 React와 TypeScript는 서로에 대해 만들어집니다. TypeScript로 구성 요소를 구축하고 활용하는 것이 훨씬 더 깔끔합니다.

이제 우리 앱에 대해 이야기 할 시간이야. 사용자가 자신의 검색 API를 이용해 깃허브 내 저장소를 검색할 수 있는 간단한 화면이다. 여기 앱 데모:

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/app-demo.gif?resize=730%2C562&ssl=1)

GitHub에서 전체 코드를 확인할 수 있습니다.

### 원격 데이터 유형

API에서 데이터를 가져와야 하기 때문에 이를 저장할 개체가 필요하고, 해당 개체를 유형에 매핑해야 합니다. 이제 반응을 분석하고 사용자 정의 유형을 구축하여 API 데이터 유형을 정의해 보겠습니다.

다음은 다음을 참조하기 위한 샘플 응답입니다.

```undefined
{
  "total_count": 40,
  "incomplete_results": false,
  "items": [
    {
      "id": 3081286,
      "node_id": "MDEwOlJlcG9zaXRvcnkzMDgxMjg2",
      "name": "Tetris",
      "full_name": "dtrupenn/Tetris",
      "owner": {
        "login": "dtrupenn",
        "id": 872147,
        "node_id": "MDQ6VXNlcjg3MjE0Nw==",
        "avatar_url": "https://secure.gravatar.com/avatar/e7956084e75f239de85d3a31bc172ace?d=https://a248.e.akamai.net/assets.github.com%2Fimages%2Fgravatars%2Fgravatar-user-420.png",
        "gravatar_id": "",
        "url": "https://api.github.com/users/dtrupenn",
        "received_events_url": "https://api.github.com/users/dtrupenn/received_events",
        "type": "User"
      },
      "private": false,
      "html_url": "https://github.com/dtrupenn/Tetris",
      "description": "A C implementation of Tetris using Pennsim through LC4",
      "fork": false,
      "url": "https://api.github.com/repos/dtrupenn/Tetris",
      "created_at": "2012-01-01T00:31:50Z",
      "updated_at": "2013-01-05T17:58:47Z",
      "pushed_at": "2012-01-01T00:37:02Z",
      "homepage": "",
      "size": 524,
      "stargazers_count": 1,
      "watchers_count": 1,
      "language": "Assembly",
      "forks_count": 0,
      "open_issues_count": 0,
      "master_branch": "master",
      "default_branch": "master",
      "score": 1.0
    }
  ]
}
```

응용 프로그램이 매우 간단하기 때문에 모든 필드를 매핑할 필요는 없지만 어떻게 작동하는지 살펴보겠습니다. `src` 아래에 `Types/GitHub.ts`라는 이름의 파일 및 폴더를 생성하겠습니다.

이 파일에서는 JSON 응답을 나타내는 사용자 정의 데이터 유형을 정의합니다. TypeScript에는 유형을 정의하는 두 가지 기본 옵션이 있습니다: `인터페이스`와 `types`.

각각 속성 및 규칙이 있으며 TypeScript의 Type vs Interface에서 자세히 다루지 않으므로 여기서는 자세히 설명하지 않겠지만 일반적으로 소품용 인터페이스와 객체를 나타내는 Type을 사용하는 것이 좋습니다.

우리의 경우, 우리는 객체를 표현하기 때문에 `유형` 구문을 사용하여 발견된 리포지토리의 총_수(숫자), `불완전한_결과` 플래그(참 또는 거짓), 항목 목록(리포지토리)으로 구성된 `GitHubSearchResultType`을 선언할 것이다.

처음 두 속성은 매우 간단합니다. 각 속성은 TypeScript 기본 유형에 해당하므로 즉시 참조할 수 있습니다.

```bash
export type GitHubSearchResultType = {
    total_count: number;
    incomplete_results: boolean;
}
```

이제 `물건`의 속성과 관련해서는 상황이 좀 더 복잡해졌다. 개체 배열이라는 것을 알고 있으므로 TypeScript의 키워드 `any`를 사용할 수 있습니다. 이 키워드는 Type을 무시하는 JavaScript 동작으로 기본 설정됩니다. 이렇게 보일 수도 있습니다.

```bash
export type GitHubSearchResultType = {
    total_count: number;
    incomplete_results: boolean;
    Items: Array<any>;
}
```

하나의 정의에는 메인 유형인 `Array`와 `<> 사이에 들어가는 배열 요소의 유형이라는 두 가지 유형이 있으므로 배열을 정의하는 구문이 약간 다릅니다.

이 작업은 수행되지만 이상적이지 않으며 대부분의 프로젝트에서 TypeScript를 사용하는 이유를 무시하기 때문에 `임의` 사용은 금지(그리고 보풀 규칙에 의해 모니터링)됩니다.

더 나은 방법은 두 번째 사용자 지정 유형을 정의하고 어레이를 이 유형에 매핑하는 것입니다.

```bash
typescript
export type GitHubSearchResultType = {
    total_count: number;
    incomplete_results: boolean;
    items: Array<GitHubRepository>
}
```

그거 잘됐네요! 항목이 무엇인지, 각 항목이 가질 속성이 무엇인지 분명합니다. 우리는 이제 깃허브 저장소의 정의만 놓치고 있다.

```cpp
export type GitHubRepository = {
    id: string;
    full_name: string;
    html_url: string;
}
```

### 구성 요소: 리포지토리 나열

이제 구성요소 작성을 시작할 때입니다. 우리는 매우 간단한 구성요소부터 시작하겠습니다. 이 구성요소는 리포지토리 목록을 받아 화면에 그릴 것입니다. 우리는 이 구성 요소의 이름을 `ListRepository.tsx`로 지정하고 `src/Components` 폴더에 저장할 것입니다.

React with TypeScript의 구성 요소는 JavaScript React 구성 요소와 두 가지 주요 차이점이 있습니다.

- TypeScript가 있으므로 Type 검사에 PropType이 필요하지 않습니다.
- 구성 요소 선언이 약간 다릅니다.

먼저 TypeScript로 소품을 정의하는 방법을 이해하겠습니다. 당사의 구성 요소는 소품으로 수신할 리포지토리 목록에 대한 액세스가 필요하므로 `인터페이스` 구문을 사용하여 정의하겠습니다.

```php
interface Props {
    repositories?: Array<GitHubRepository>;
}
```

`type`과 매우 유사한 인터페이스는 대괄호 사이의 본문에서 객체의 속성을 정의하며, 속성은 동일한 방법으로 정의된다.

우리의 특별한 예에서, 우리는 선택적 속성(변수 이름 바로 뒤에 ` ?`를 추가함으로써 주목되는)이라는 새로운 개념을 도입했습니다. 이러한 선택적 속성은 `정의되지 않음`, `null` 또는 지정된 유형의 값일 수 있습니다.

우리가 만든 인터페이스 이름은 프로프스(Props)였다. 그러나 `리포지토리 제안서 목록`이나 `리포지토리 제안서 유형 목록`과 같은 것은 무엇이든 될 수 있다.

그러므로 우리는 리액트에게 우리의 구성품을 선언할 때 소품 종류에 어떤 이름이 붙었는지 말해야 하며, 리액트를 사용하여 그것을 달성해야 한다.FC는 다음과 같습니다.

```coffeescript
const ListRepositories: React.FC<Props> = (props) => {
```

그 한 줄에서, 우리는 기능적인 요소를 만들었고 우리의 소품들이 `프롭스` 타입이라고 반응했다. 그리고 그것이 기능적인 요소이기 때문에, 우리는 물체 파괴와 같은 멋진 일들을 할 수 있습니다. 수정된 선언은 다음과 같습니다.

```coffeescript
const ListRepositories: React.FC<Props> = ({ repositories = [] }) => {
```

항상 값이 필요한 경우 리포지토리 속성을 옵션으로 설정하는 이유가 궁금하다면 이 구성 요소의 상위 항목이 정의되지 않은 항목을 전달할 수 있고, 그렇다면 빈 목록을 가정할 수 있기 때문입니다.

다음은 구성 요소의 전체 코드입니다.

```js
import React from 'react';
import {GitHubRepository} from "../Types/GitHub";

interface Props {
    repositories?: Array<GitHubRepository>;
}

const ListRepositories: React.FC<Props> = ({ repositories = [] }) => {
    return (
        <ul>
            {repositories.map(repository => (
                <li key={repository.id}>
                   <a href={repository.html_url} target="_blank">{repository.full_name}</a>
                </li>
            ))}
        </ul>
    );
}

export default ListRepositories;
```

### 검색 양식 작성

다음으로는 사용자 입력을 캡처하고 검색 작업을 시작하는 새로운 구성 요소를 구축하는 것입니다. 우리는 이미 React와 TypeScript에 대해 전문가이기 때문에, 나는 양식에 대한 전체 코드를 제시한 후 아래의 중요한 요소에 대해 이야기 할 것이다.

```js
import React from 'react';

interface Props {
    search(query: string): void;
}

const SearchForm: React.FC<Props> = (props) => {
    function handleSubmit(e: React.FormEvent) {
        e.preventDefault();
        const formData = new FormData(e.target as HTMLFormElement);
        props.search((formData.get('query') || '') as string);
    }

    return (
        <form onSubmit={handleSubmit}>
            <label>
                Query:
                <br />
                <input type="text" name="query" />
            </label>
            <button>Search</button>
        </form>
    );
}

export default SearchForm;
```

이전 부품과 마찬가지로 우리는 소품을 정의하기 위해 인터페이스를 사용하고 있지만, 그 대신 속성이 없고 방법이 있다.

TypeScript를 사용하면 함수 유형을 정의할 수 있으며 함수 이름, 매개 변수, 유형 및 반환 유형을 지정하여 이 작업을 수행할 수 있습니다. 또한 다음과 같은 여러 구문을 지원합니다.

```bash
functionName(param1: type, …): type;
```

또는 화살표 기능을 사용하여 다음을 수행합니다.

```coffeescript
functionName: (param1: type, ...) => type;
```

예:

```undefined
interface Props {
    search(query: string): void;
}

interface Props {
    search: (query: string) => void;
}
```

이 양식에서는 캐스팅인 TypeScript를 또 다른 특수 용도로 사용하고 있습니다. 여기서 확인할 수 있습니다.

```undefined
props.search((formData.get('query') || '') as string);
```

왜 캐스팅을 해야 하죠? formData.get 함수는 문자열, 파일 또는 정의되지 않은 다중 유형 중 하나일 수 있음을 의미하는 유니언 유형입니다. 우리의 목적을 위해 유일한 선택은 끈이라는 것을 알고, 그래서 우리는 캐스팅합니다.

### API를 모두 가져오고정

준비 다 됐어요. 이제 우리는 그것들을 `App.tsx` 컴포넌트에 결합하고 GitHub API에서 원격 데이터를 가져올 수 있는 기능을 추가해야 한다.

코드를 공개하기 전에 API에서 데이터를 가져오는 데 축을 사용하고 있음을 명확히 하는 것이 중요합니다. axios type을 설치할 수 있습니다.

```coffeescript
npm install axios
```

이제 코드를 입력해야 합니다.

```js
import React from 'react';
import axios from 'axios';
import {GitHubRepository, GitHubSearchResultType} from "./Types/GitHub";
import SearchForm from "./Components/SearchForm";
import ListRepositories from "./Components/ListRepositories";

function App() {
    const [repositories, setRepositories] = React.useState<Array<GitHubRepository>>();

    // Performs the search
    async function search(query: string) {
         const result = await axios.get<GitHubSearchResultType>(`https://api.github.com/search/repositories?q=${query}`);
         setRepositories(result.data.items);
    }

    return (
        <div>
            <SearchForm search={search}/>
            <ListRepositories repositories={repositories}/>
        </div>
    );
}

export default App;
```

TypeScript Generics를 사용하는 두 인스턴스를 제외하고 모두 표준 Retact 코드입니다.

제네릭을 사용하면 나중에 코드에서 완료되는 "빈" 유형을 정의할 수 있습니다. React의 완벽한 예는 React.useState 함수이다.

useState는 코드에서 정보를 React의 상태에 저장하는 데 사용할 수 있는 Getter와 Settter를 정의합니다. 하지만 어떻게 리액션이 이 게터와 세터의 유형을 알 수 있을까요?

답은 일반적이다. 본질적으로 useState와 같은 기능은 다음과 같이 선언된다.

```coffeescript
function useState<S = undefined>(): [S | undefined, Dispatch<SetStateAction<S | undefined>>];
```

여기서 `S`는 어떤 유형도 될 수 있으며 함수 호출 중에 다음과 같이 지정됩니다.

```js
const [repositories, setRepositories] = React.useState<Array<GitHubRepository>>();
```

또는 암묵적 유형 지정을 사용하여 다음을 수행합니다.

```undefined
const [someText, setSomeText] = React.useState('Hello World!');
```

제네릭을 사용하는 두 번째 예는 축소를 사용하는 경우입니다.

```undefined
const result = await axios.get<GitHubSearchResultType>(`https://api.github.com/search/repositories?q=${query}`);
         setRepositories(result.data.items);
```

이 경우 get 함수는 지정된 형식으로 매핑된 JSON 응답 데이터를 반환합니다.

## 결론

이 튜토리얼에서는 TypeScript의 힘을 활용하여 확장 가능하고 유지 관리가 가능한 React 애플리케이션을 구축하는 방법에 대해 배웠습니다. 내가 이 기사를 쓴 것 만큼이나 재미있게 읽으셨기를 바라며, 내가 늘 말하듯이, 일단 TypeScript에 가면, 절대 돌아가지 마세요!

읽어주셔서 감사합니다!