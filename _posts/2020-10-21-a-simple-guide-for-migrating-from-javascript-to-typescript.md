---
layout: post
title: "JavaScript에서 TypeScript로 마이그레이션하기 위한 간단한 가이드"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/TypeScript.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/TypeScript.png?fit=730%2C487&ssl=1)

이미 알고 있는 것처럼 TypeScript는 널리 사용되는 정적 유형 검사기로, JavaScript의 상위 집합으로 구축됩니다. TypeScript는 일반 Javascript에 없는 규칙인 형식 오류를 실행하기 전에 찾는 데 매우 유용합니다. 그러나 Typescript는 Javascript처럼 부드럽게 실행됩니다.

우리는 당신이 자바스크립트에서 프로젝트를 시작했다고 가정하고 다시 시작하기에는 너무 늦었다. TypeScript에서 작업하려고 하는데 너무 오래 걸려서 다시 시작할 수 없습니다.

무슨 일을 하세요?

이 기사에서는 Typescript로 원활하고 효율적으로 마이그레이션할 수 있는 방법에 대해 설명합니다.

TypeScript로 마이그레이션할 때 기억해야 할 한 가지 기본 사항은 TypeScript 파일의 확장자가 .js가 아닌 .ts라는 것입니다. 그것은 충분히 간단하다. 마이그레이션 작업을 간편하게 수행할 수 있는 또 다른 사항은 TypeScript 컴파일러에서 JavaScript 파일을 허용하도록 구성할 수 있다는 것입니다.

앞에서 말했듯이 TypeScript는 Javascript의 상위 집합일 뿐이므로 한 번에 하나의 파일을 마이그레이션할 수 있으며 마이그레이션을 위해 완전히 별도의 분기를 만들 걱정은 없습니다.

컴파일러에 대해 말하자면, 현재 JavaScript를 번들하거나 컴파일하는 것은 모두 그대로 두는 것이 좋습니다. TypeScript는 상황을 망치지 않고 컴파일러 위에 간단히 자신을 추가하는 쉬운 방법을 가지고 있다. npm을 사용하는 경우 npmi-g typescript를 실행하면 됩니다.

TypeScript 설정에 더 들어가기 전에 한 가지 고려해야 할 사항은 IDE가 TypeScript와 함께 작동하도록 구성되어 있는지 여부입니다. 사전 컴파일 자동 수정 TypeScript는 이 단계에서 오류를 탐지하는 기능뿐만 아니라 사용자가 놓치고 싶지 않은 기능입니다. VS Code를 사용하는 경우 TypeScript에 대한 지원이 기본으로 제공됩니다. 그렇지 않으면 TypeScript에 대한 IDE 지원을 받으려면 패키지를 찾아야 할 수 있습니다.

## 타이프스크립트 컴파일러

이제 TypeScript 컴파일러를 추가하려고 합니다.

프로젝트의 루트 디렉터리에 `tsconfig.json`이라는 파일을 생성하십시오. 이 파일을 구성하는 방법은 수백 가지가 있지만 몇 가지 기본 사항만 살펴보겠습니다.

먼저 파일을 직접 만드는 대신 다음 프로젝트의 루트 디렉터리에서 이 명령을 실행할 수 있습니다.

```undefined
npx tsc --init
```

이렇게 하면 `tsconfig.json` 파일이 생성되고 몇 가지 기본 옵션이 실행되며 여러 가지 옵션을 볼 수 있습니다.

보시는 것처럼 JSON 파일이 됩니다. 따라서 여기서 구성할 두 가지 공통 속성은 compileOptions와 include입니다.

CompileOptions는 TypeScript가 파일을 JavaScript로 전송하는 방식을 변경하기 위해 옵션을 true로 설정할 수 있는 객체이다.

예를 들어 `compileOptions`에서 `NoImplicit`를 켤 수 있습니다.Any: true(false일 경우, TypeScript는 "any"의 유형을 유추하며, "strictNullChecks: true"(false일 경우 Typescript는 "null" 및 "undefined"를 무시합니다.)

이 두 가지 옵션을 설정하면 `.ts` 파일이 실제로 유형을 확인할 수 있습니다. 그렇지 않은 경우 .ts 파일에서 일반 JavaScript를 실행할 수 있습니다.

포함 옵션은 TypeScript 파일과 일치하도록 파일 이름 또는 전역 패턴을 지정하는 방법입니다. 특정 파일 이름 또는 모든 TypeScript 파일이 따르는 패턴을 포함할 수 있는 배열입니다. 간단한 설정은 이 옵션을 포함 ["src/**/*"](`*_`은 모든 디렉토리와 일치하지만 `_`은 모든 파일과 일치함)으로 정의하는 것입니다.

이렇게 하면 기본적으로 확장자가 .ts, .tsx 또는 .d.ts인 src 디렉토리 아래의 파일을 선택할 수 있습니다.

compileOptions에 `allowJs:true`를 추가하면 위에서 정의한 `포함`에는 `src` 디렉토리의 `.js` 및 `.jsx` 파일도 포함됩니다.

짧은 참고: .d.ts 확장과 일반 .ts의 차이점은 .d.ts는 TypeScript에 무언가를 선언하기 위한 것이고, .ts는 TypeScript에서 자바스크립트로 컴파일하기 위한 것이다. .d.ts는 TypeScript가 알 수 있도록 `npm` 모듈에서 형식을 선언하는 데 가장 일반적으로 사용되는 반면, .ts는 모든 JavaScript 파일에 사용됩니다. .ts와 .tsx의 차이는.tsx와 같은 JSX 파일에 사용된다.

또 다른 기본적인 이해 방법은 `제외`이다. `제외`에 범위가 넓은 경우 컴파일러가 일치하지 않을 파일이 있을 수 있습니다. 예를 들어, 모든 것을 포함하는 것이 하나의 옵션이지만, `node_modules`를 제외 어레이 아래에 놓기를 원할 수 있습니다.

TypeScript 컴파일러 구성에 대한 자세한 내용은 공식 설명서를 참조하십시오.

`tsconfig.json` 파일 구성을 마치면 `package`에 스크립트를 추가할 수 있습니다.json을 사용하여 컴파일러를 실행합니다. 스크립트에서 "tsc:w": "tsc -w"와 같은 것을 추가합니다. 그런 다음 스크립트를 실행하면 `tsconfig.json` 파일을 가리키고 컴파일합니다!

### @types

React와 같은 프런트 엔드 프레임워크를 사용하는 경우 `@types` 패키지도 설치해야 합니다. TypeScript의 모든 것이 형식 정의를 요구하기 때문에, `@types/react`와 같은 패키지는 TypeScript가 모든 기본 React 클래스, 함수, 구성 요소 등에 대한 형식을 알 수 있도록 한다.

이것은 TypeScript를 만족시키기 위해 필요하며, 당연히 그렇다. 또한 프로젝트에 추가된 다른 라이브러리(예: `@types/react-router-dom`, `@types/react-bootstrap` 또는 `@types/react-redux`)에 유형을 추가해야 합니다.

`@types` 패키지의 또 다른 예는 `@types/node`이다. 이 패키지가 없으면 우리가 익숙한 키워드가 Typescript 파일(htts` 파일)에서 인식되지 않습니다.

예를 들어, `require`를 사용하여 패키지를 가져오면 `Error: (3, 12) TS2304: require 이름을 찾을 수 없음`이라는 오류가 반환됩니다. 패키지 `@type/node`는 TypeScript에 대해 이것을 정의하기 때문에 이제 오류 없이 require를 사용할 수 있다.

일부 JavaScript 패키지에는 이미 유형이 기록되어 있습니다. 이러한 경우 유형 패키지를 설치할 필요가 없습니다. 그러나 패키지에 유형이 기록되어 있지 않고 설치할 유형 패키지가 없으면 어떻게 됩니까?

우선, TypeScript는 많은 지원을 받고 있기 때문에 이것을 찾기가 어렵다. 그러나 TypeScript를 널리 사용하는 경우 TypeScript 지원 기능이 없는 JavaScript 패키지를 찾을 수 밖에 없습니다.

여기서 쉬운 솔루션은 TypeScript 컴파일러에게 모듈이 존재함을 알리는 것입니다. 이렇게 하면 TypeScript가 패키지에 추가되지는 않지만 TypeScript 검사기를 바이패스하여 사용할 수 있습니다. TypeScript를 패키지에 직접 완전히 추가하는 등 여러 가지 해결 방법이 있지만(대부분의 경우 많은 작업이 소요됨) 이는 간단한 해결 방법입니다.

파일을 만들고 `MyModuleDesc.d.ts`(중요한 부분)와 같은 이름을 지정합니다.

이 파일에는 `모듈 선언`(타입 스크립트가 없는 패키지로 `마이모듈`을 쓰시면 됩니다)이라고만 쓰시면 됩니다. 그런 다음 `tsconfig.json` 파일의 `포함` 배열 아래에 `MyModuleDesc.d.ts` 파일을 추가합니다(이 파일이 프로젝트의 루트에 있다고 가정함).

### 반응 예제

자, 이제 기본적인 방법에 대해 알아보겠습니다. 이 마이그레이션이 실제 실행 중인 작업을 살펴보겠습니다. Swapi.dev에서 수신하는 데이터에 대한 간단한 테이블을 만들었습니다.

```js
export default function TableComponent(props) {

  const { result } = props;
  return (
    <Table>
      <Row><Cell>{result.name}</Cell></Row>
      <Row><Cell>Hair Color</Cell><Cell>{result.hairColor}</Cell></Row>
      <Row><Cell>Height</Cell><Cell>{result.height}</Cell></Row>
      <Row><Cell>Weight</Cell><Cell>{result.weight}</Cell></Row>
      <Row><Cell>Date of Birth</Cell><Cell>{result.dateOfBirth}</Cell></Row>
      <Row>
        <Cell>{result.filmNames.length <= 1 ? "Film" : "Films"}</Cell>
        <Cell>{result.filmNames.length === 0 ?
          "None" :
          result.filmNames.map((film, idx) => (
            <LineItem key={'film' + idx}>{film}</LineItem>))}
        </Cell>
      </Row>
    </Table>
  )
}
```

이것이 좋은 예가 될 것이다. 여기서 우리가 보는 각 요소는 내가 다른 곳에서 정의한 `styled-component`이다. 이러한 요소들은 `const Table = styled.div;`(따옴표 사이에 정의된 CSS 묶음)처럼 정의된다.

우선, 이 작업은 내 프로젝트에서 TypeScript를 전혀 사용하지 않습니다. 파일 이름은 `TableComponent.js`이며 단순성을 유지하기 위해 App.js의 하위 항목입니다. 저는 원래 `생성-반응-앱`을 사용하여 이 프로젝트를 초기화했고, 여러분도 쉽게 그렇게 할 수 있습니다. 당신이 따라야 할 유일한 패키지는 `스타일링 컴포넌트`이다.

다음으로 TypeScript를 추가하려면 `npm`을 통해 몇 가지 사항을 설치할 수 있습니다.

```coffeescript
npm i --save typescript @types/react @types/react-dom @types/styled-components
```

이 수준에서 터미널에서 다음 명령을 실행합니다.

```undefined
npx tsc --init
```

그러면 `tsconfig.json` 파일이 생성됩니다.

이제 `tsconfig.json` 파일에 오류가 표시될 수 있습니다. 파일의 맨 위 첫 번째 곱슬머리 가새 위에 마우스를 놓으면 다음과 같은 오류가 표시됩니다.

오류 TS18003: 구성 파일 `tsconfig.json`에서 입력을 찾을 수 없습니다. 지정된 포함 경로는 [**/*]이고 제외 경로는 []입니다.

편집기를 닫았다가 다시 가져오면 가끔 이 오류가 발생하지만 앱을 실행하려고 하면 이 오류가 표시됩니다.

여기서 기본적인 아이디어는 컴파일러가 작동하기 위해 적어도 하나의 TypeScript 파일이 필요하다는 것이다. TableComponent.js의 파일 이름을 TableComponent.tsx로 변경합니다. Visual Studio Code를 사용하는 동안처럼 IDE를 다시 시작해야 할 수도 있지만, 소품 아래 및 다른 몇 군데에 빨간색 줄이 표시되어 오류를 나타냅니다.

먼저 다음 명령으로 실행해 보겠습니다.

```coffeescript
npm run start
```

상당히 멀어질 것이고, 앱 구성 요소가 마무리되는 것을 볼 수도 있지만, 우리의 `테이블 구성 요소`는 오류를 나타낼 것입니다.

보다 구체적으로, "매개 변수 `props`에는 암시적으로 `임의` 유형이 있습니다. TS7006²입니다.

이것이 바로 빨간색 선이 가리키는 것입니다. `암묵적 없음` 때문이다.Any"는 기본값으로 설정되어 있으며 이것이 바로 우리가 원하는 것입니다. tsconfig.json 파일로 이동하여 false로 변경할 수 있습니다. 그러면 오류가 없겠지만 기본적으로는 일반 JavaScript가 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/typescript.png?resize=730%2C159&ssl=1)

한 가지 중요한 점은 TypeScript가 현재 번들러 위에서 중단 없이 실행된다는 것입니다.

TypeScript를 호출하지 않았는데도 npm 런스타트를 실행했습니다. 엄밀히 말하면, `NoEmit` 옵션이 true로 설정된 상태에서, TypeScript는 단순히 JSX의 유형을 확인했지만, Bundler는 이를 방출/출력했다. 또 다른 것은 `tsconfig.json` 파일을 보는 것입니다. 이 파일은 우리의 앱을 읽고 우리를 위한 몇 가지 기본 옵션을 채웠습니다.

TypeScript가 프로젝트에서 어떤 작업을 했는지도 기록해 두십시오. TypeScript는 자동으로 `react-app-env.d.ts`라는 이름의 파일을 `src` 폴더에 생성했습니다. 이 파일은 그다지 중요하지 않지만 삭제하더라도 각 시작/빌드에 자동으로 생성됩니다. 이것은 TypeScript가 react-script 유형에 대해 간단히 알 수 있도록 하는 이상한 TypeScript입니다.

추가 작업 없이 이 파일을 TypeScript로 변경해 보겠습니다.

### JSX에서 TSX로

변수 끝에 `: any`를 더하면 레드라인 오류를 쉽게 없앨 수 있지만, 그것은 기본적으로 무용지물이다.

더 좋은 방법은 TypeScript의 용도를 수행하는 것입니다. 즉, 오류를 방지하기 위해 유형을 확인하는 것입니다. 먼저 지도 함수의 변수 "필름"을 설명하는 것으로 시작하겠습니다.

여러분은 빨간색 선이 그 아래에 있고 솔직히 그들이 필요로 하는 유형을 이해하기 가장 쉬울 것입니다: 그것들은 모두 문자열이기 때문에 우리가 쉽게 통과해서 끝에 `: 문자열`이라고 주석을 달 수 있습니다.

그러면 idx에 `: number`로 주석을 달 수 있으며, TypeScript 컴파일러를 만족시킬 수 있다. 이 모든 것은 괜찮겠지만, 여전히 주석이 없는 소품 변수에 문제가 남아 있다. 그리고 솔직히, JSX의 소품들을 훑어보고 거기에 주석을 다는 것은 가장 좋은 방법이 아닙니다.

더 나은 방법은 전달되는 소품을 위한 TypeScript 인터페이스 또는 유형을 생성하는 것입니다. 이제 API에서 결과를 가져올 때(또는 생성한 API에서도 더 좋음) 상위 `App.js` 구성 요소에서 이 작업을 수행했을 것입니다. 하지만 TypeScript의 유연성을 볼 수 있으므로 그렇게 할 필요가 없습니다.

소품 종류를 만들어 보겠습니다.

```bash
type Props = {
  result: Result,
};
```

그런 다음 결과의 유형은 다음과 같습니다.

```cpp
type Result = {
  name: string,
  hairColor: string,
  height: string,
  weight: string,
  dateOfBirth: string,
  filmNames: string[],
};
```

그런 다음 소품에 간단히 주석을 달기만 하면 된다. 이제, 여러분은 이 영화가 더 이상 빨간색 밑줄이 그어져 있지 않다는 것을 볼 것입니다.

우리는 이 인터페이스들을 새로운 타입 대신에 만들 수 있었지만, 일반적인 경험의 법칙으로서, 좀 더 통제된 것들 (소품 같은) 타입으로 만들기 위해 노력하세요. 유형과 인터페이스 중 하나를 선택하는 방법에 대한 자세한 내용은 이 문서를 참조하십시오.

## 스타일 구성 요소

이것은 결코 TypeScript에 대한 완전한 가이드가 아니다. 다만 기본을 조금 더 보기 위해 `스타일링 컴포넌트`를 살펴보자.

현재 우리는 오류가 없지만, 스타일링된 구성 요소의 좋은 점 중 하나는 소품을 전달하는 능력입니다. 따라서 동일한 정의를 재사용하고 소품에 따라 약간만 변경할 수 있습니다.

예를 들어 JSX에서 첫 번째 셀(스타일 구성 요소)이 테이블의 전체 너비를 차지하기를 원하지만 다른 셀은 너비의 절반만 차지해야 합니다. 이제 "헤더" 받침대를 삽입해 보겠습니다.

```xml
<Cell header> {result.name} </Cell>.
```

TypeScript는 이것을 좋아하지 않고 어떻게 해야 할지 모릅니다. Cell에 대한 현재 정의는 다음과 같습니다.

```js
const Cell = styled.div`
  padding: 5px 20px;
  display: flex;
  border: 1px solid black;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 50%;
`;
```

일반적으로 다음과 같은 스타일에 소품을 추가할 수 있습니다.

```php
width: ${props => props.header ? '100%' : '50}
```

하지만 우리는 헤더 또한 정의하지 않았기 때문에 TypeScript에 따른 오류라는 것을 볼 수 있을 것이다.

다음과 같은 Cell 소품을 위한 새로운 유형을 만들어야 합니다.

```bash
type cellProp = {
  header: boolean,
};
```

이제 Cell 구성 요소에 추가합니다.

```js
const Cell =
  styled.div <
  cellProp >
  `
        ...
        width: ${(props) => (props.header ? "100%" : "50%")};
    `;
```

이렇게 하면 오류가 지워집니다. 그리고 나서 우리는 다른 것을 얻게 됩니다: 셀의 다른 모든 인스턴스들은 헤더 받침대가 없기 때문에 오류를 갖게 됩니다.

이상하게 보일 수 있지만 예상된 동작입니다. TypeScript에 Cell의 각 인스턴스에 이 헤더 부울이 필요하다고 알립니다.

각 인스턴스에 `false={false}라는 소품이 포함되도록 할 수 있지만 JSX에서는 더 복잡합니다.

더 나은 해결책은 헤더 프로펠러를 선택적으로 만드는 것이다. 유형 정의에 물음표를 추가하여 쉽게 수행할 수 있습니다.

```bash
type cellProps = {
  header?: boolean,
};
```

비올라! 더 이상 오류 없어.

## 결론

이것은 Retact 구성 요소를 TypeScript로 마이그레이션하는 약간의 맛이었다. 보시다시피 TypeScript로 마이그레이션하는 것은 전혀 번거롭지 않습니다. 유연하므로 우선 순위가 낮은 파일을 기다리면서 필요한 파일에 사용할 수 있습니다. 그리고 어느새 프로젝트에서 TypeScript를 사용할 수 있습니다!

자세한 내용은 공식 문서를 참조하십시오.