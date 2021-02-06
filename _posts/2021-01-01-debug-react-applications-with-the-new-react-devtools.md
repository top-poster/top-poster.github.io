---
layout: post
title: "React DevTools를 사용한 디버그 대응 애플리케이션"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2019/10/devtools.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2019/10/devtools.png?fit=730%2C412&ssl=1)

편집자 참고: 이 기사는 2021년 1월에 업데이트되었습니다.

## React 응용 프로그램을 디버그해야 하는 이유는 무엇입니까?

디버깅은 개발자가 가질 수 있는 가장 유용한 기술 중 하나이다. 이를 통해 코드에서 오류를 빠르고 효율적으로 탐색하고 검색할 수 있습니다. 현대의 웹에서는 다양한 도구와 기술을 활용하여 이것을 가능하게 한다.

React는 가장 빠르게 성장하는 프런트 엔드 프레임워크 중 하나입니다. 복잡하고 대화형 UI를 쉽게 만들 수 있습니다. 다른 프레임워크와 마찬가지로, 리액트 개발 툴이라는 디버깅 툴 세트를 가지고 있다.

## React DevTools란?

React DevTools(React DevTools)는 Chrome, Firefox 및 Chrome 개발자 도구에서 React 구성 요소 계층을 검사할 수 있는 독립 실행형 앱으로 사용할 수 있는 브라우저 확장입니다. 이 제품은 추가 대응별 검사 위젯 세트를 제공하여 개발을 지원합니다. 창사 이래, 핵심 팀에서 많은 방출이 있었다.

이 튜토리얼에서는 최신 React DevTools 릴리스에 추가된 주요 사항에 대해 강조하고 해당 기능을 활용하여 React 앱을 더 효과적으로 디버깅할 수 있는 몇 가지 방법을 시연합니다.

## React DevTools 설치 방법

React DevTools는 Chrome 및 Firefox의 확장 기능으로 사용할 수 있습니다. 확장을 이미 설치한 경우 자동으로 업데이트됩니다. React Native 또는 Safari와 같은 독립 실행형 셸을 사용하는 경우 NPM에서 새 버전을 설치할 수 있습니다.

```coffeescript
npm install -g react-devtools@^4
```

## 테스트 응용 프로그램 설정

저는 기사가 디버깅에 초점을 맞추도록 보장하면서 쉬운 설정과 오버헤드를 줄이기 위해 시작 프로젝트를 만들었습니다. 애플리케이션의 골격은 이미 설정되었으며, 여기에는 몇 가지 구성 요소, 스타일 및 프로젝트 구조가 포함되어 있습니다. 나를 실험하려면 다음 명령을 실행하여 리포지토리를 복제하십시오.

```php
git clone https://github.com/Kennypee/react-contacts
```

다음 명령을 실행하여 폴더를 열고 프로젝트의 종속성을 설치합니다.

```bash
cd react-contacts && npm install
```

Retact app 서버를 시작하려면 프로젝트의 루트 폴더에서 `npm start` 명령을 실행하십시오. "localhost:3000"에 브라우저를 열면 프로젝트가 라이브로 표시됩니다!

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/09/devtools.gif?resize=730%2C365&ssl=1)

## 대응 DevTools 성능 향상

DevTools는 상당한 성능 향상과 향상된 탐색 환경을 제공합니다. 특정 측면이 수정되어 대규모 애플리케이션에 사용할 수 있게 되었습니다.

## React DevTools로 구성 요소 필터링

이전 버전의 DevTools에서는 대형 구성 요소 트리를 탐색하는 작업이 다소 지루했습니다. 하지만 이제 DevTools는 사용자가 관심 없는 구성 요소를 숨길 수 있도록 구성 요소를 필터링할 수 있는 방법을 제공합니다.

이 기능에 액세스하려면 샘플 응용 프로그램의 세 가지 구성 요소를 필터링하십시오. DevTools를 열면 세 가지 구성 요소가 나열되어 있습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/09/threecomponentslisted.png?resize=730%2C300&ssl=1)

구성 요소를 필터링하고 관심 있는 구성 요소에 초점을 맞추려면 구성 요소 탭 아래의 설정 아이콘을 클릭하십시오. 팝업 창이 표시됩니다. 구성요소 탭을 누른 후 원하는 정렬 선택사항을 선택합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/09/threecomponents.gif?resize=730%2C313&ssl=1)

구성 요소를 필터링한 후에는 기본적으로 숨겨지지만 필터를 사용하지 않도록 설정하면 해당 구성 요소가 표시됩니다. 이 기능은 여러 구성 요소를 사용하여 프로젝트를 수행할 때 유용하며 빠른 정렬이 실제로 필요한 경우 유용합니다. 이 기능에서 더욱 흥미로운 점은 세션 간에 필터 기본 설정이 기억된다는 것입니다.

## React DevTools의 인라인 소품은 이제 과거의 것이 되었습니다.

더 큰 구성 요소 트리를 더 쉽게 탐색하고 DevTools를 더 빠르게 만들기 위해 트리의 구성 요소에는 더 이상 인라인 소품이 표시되지 않습니다.

이 기능을 사용하려면 구성 요소를 선택하면 콘솔 오른쪽에 모든 구성 요소의 소품, 상태 및 후크가 표시됩니다.

샘플 애플리케이션에서는 `연락처` 구성 요소에만 소품을 전달합니다. 이를 클릭하면 전달된 소품의 가치가 표시되고 다른 구성 요소를 클릭하면 소품이 전달되지 않았음을 알 수 있습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/09/contactcomponent.gif?resize=730%2C241&ssl=1)

이 기능은 소규모 React 프로젝트에는 유용하지 않을 수 있지만 대규모 React 프로젝트 작업 시 유용합니다.

## 예기치 않은 프로펠러 값 및 구성 요소 디버깅

다음 반응 클래스를 고려하십시오.

```js
import ABC from 'abc';
import XYZ from 'xyz';

class Main extends Component {
  constructor(props) {
    super(props);

    this.state = { name : "John" }
 }
 render() {
    const { name } = this.state;
      return (
        <ABC>
          <XYZ name={name} />
        </ABC>
      )
  }
}
```

ABC는 XYZ의 부모지만 메인(Main)은 부품 소유자로 소품만 내려보낼 수 있다.

최신 버전의 React Dev 도구에서는 상위 항목을 건너뛰어 예기치 않은 프로펠러 값을 빠르게 디버깅할 수 있습니다. DevTools v4는 오른쪽 창에 소유자 목록을 빠르게 단계별로 이동하여 디버깅 프로세스를 가속화할 수 있는 렌더링 기준 목록을 추가합니다.

응용 프로그램의 구성 요소를 클릭하면 해당 구성 요소를 렌더링한 구성 요소를 볼 수 있습니다. 이것은 특정 소품의 출처를 추적할 때 매우 유용합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/09/listcomponent.gif?resize=730%2C241&ssl=1)

오너 트리라는 역함수도 있다. 특정 구성요소가 "소유"하는 항목 목록입니다. 이 보기는 구성 요소의 렌더링 방법의 출처를 보는 것과 비슷하며, 익숙하지 않은 대규모 반응 응용 프로그램을 탐색하는 데 도움이 될 수 있습니다.

이 기능을 사용하여 응용 프로그램을 디버깅하려면 구성 요소를 두 번 클릭하여 사용자 트리를 보고 "x" 단추를 클릭하여 전체 구성 요소 트리로 돌아가십시오. 트리에서 이동하여 구성 요소의 모든 어린이를 볼 수도 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2019/09/contactcomponent2.gif?resize=730%2C313&ssl=1)

## React DevTools의 시각적 개선 사항

### 들여쓰기된 구성요소 보기

이전 버전의 React DevTools에서는 깊이 중첩된 구성 요소를 보려면 수직 및 수평 스크롤이 모두 필요하므로 대형 구성 요소 트리를 추적하기가 어렵습니다. 이제 DevTools는 중첩 들여쓰기를 동적으로 조정하여 수평 스크롤을 제거합니다.

앱에서 이 기능을 사용하려면 구성 요소 탭을 클릭한 다음 아무 구성 요소나 클릭하면 다음 구성 요소에서 자동으로 모든 하위 항목이 해당 아래에 나타납니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/09/indentedcompview.gif?resize=600%2C257&ssl=1)

### 검색 개선

이전에는 DevTools에서 검색할 때 결과가 일치하는 노드를 루트로 표시하는 필터링된 구성 요소 트리(즉, 다른 구성 요소가 숨겨지고 검색 일치가 루트 요소로 표시됨)인 경우가 많았다. 이것은 조상을 형제자매로 보여주기 때문에 애플리케이션의 전체적인 구조를 추론하기 어렵게 만들었다.

이제 브라우저의 페이지 찾기 검색과 유사한 인라인으로 결과를 표시하여 구성 요소를 쉽게 검색할 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2019/09/improvedsearching.gif?resize=600%2C257&ssl=1)

## React DevTools의 기능 향상

### 향상된 후크 지원

버전 4의 Hooks는 이제 소품 및 상태와 동일한 수준의 지원을 제공하므로 Hook 기반 React 프로젝트를 더 빠르고 효율적으로 디버깅할 수 있습니다. 값을 편집하거나 배열 및 개체를 드릴로 천공할 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2019/09/improvedhooks.gif?resize=730%2C178&ssl=1)

### 다시 로드 간에 선택 항목 복원

디버깅하는 동안 다시 로드를 누르면 이제 DevTools는 마지막으로 선택한 요소를 복원하려고 시도합니다.

페이지 새로 고침이 발생하기 전에 샘플 응용 프로그램에서 Persons 구성 요소를 정렬하고 있었다고 가정해 보자. DevTools는 자동으로 선택된 Persons 구성 요소로 재개된다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2019/09/restoringselection.gif?resize=600%2C257&ssl=1)

### 서스펜스 토글

React의 Suspend API를 사용하면 구성 요소가 렌더링하기 전에 "대기" 또는 "뭔가"를 수행할 수 있습니다. `Suspense` 구성 요소는 트리에서 더 깊은 구성 요소가 렌더링 대기 중일 때 로드 상태를 지정하는 데 사용할 수 있습니다.

DevTools를 사용하여 다음과 같은 로드 상태를 테스트할 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2019/09/suspensedemo.gif?resize=599%2C223&ssl=1)

## React DevTools의 프로파일러 변경 사항

### 다시 로드 및 프로파일

프로파일러는 React 구성 요소를 튜닝하기 위한 강력한 도구입니다. 기존 DevTools는 프로파일링을 지원했지만 프로파일링 지원 버전의 React를 감지한 후에야 프로파일링을 지원합니다. 이 때문에 애플리케이션의 초기 마운트(가장 성능에 민감한 부분 중 하나)를 프로파일링할 방법이 없었습니다.

이제 이 기능은 "재로드 및 프로파일" 작업에서 지원됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/09/reactwindow.gif?resize=498%2C386&ssl=1)

### 구성 요소 렌더링 목록

프로파일러는 프로파일링 세션 중에 선택한 구성요소가 렌더링되는 각 시간 목록과 각 렌더 기간을 표시합니다. 이 목록을 사용하여 특정 구성 요소의 성능을 분석할 때 커밋 사이를 빠르게 이동할 수 있습니다.

샘플 애플리케이션의 경우, 일부 구성 요소가 섹션 중에 두 번 렌더링을 수행한다는 것을 알 수 있으며, 이제 성능을 잠재적으로 향상시킬 수 있는 디버깅을 지향하게 되었다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2019/10/support.gif?resize=730%2C207&ssl=1)

## React DevTools 지원

다음 버전의 대응만 지원됩니다.

리액션 돔
0-14.x: 지원되지 않음
15.x: 지원됨(새 구성 요소 필터 기능 제외)
16.x: 지원됨

반작용이 있는
0-0.61: 지원되지 않음
0.62: 지원됨
0.63: 지원됨

따라서 특정 기능이 특정 프로젝트에서 작동하지 않는 경우 사용 중인 대응 버전을 확인해야 합니다.

## 결론

본 튜토리얼에서는 DevTools로 React 애플리케이션을 디버깅하는 방법에 대해 설명했습니다. 우리는 그것에 수반되는 몇 가지 추가사항과 개선사항을 살펴보았다. 또한 코드를 디버깅하기 쉽게 만드는 방법에 대해서도 알아봤습니다. 질문, 의견 또는 추가 사항이 있으면 의견을 삭제하십시오. 해피 코딩!