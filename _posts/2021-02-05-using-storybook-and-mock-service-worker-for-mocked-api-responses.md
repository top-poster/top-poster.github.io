---
layout: post
title: "Storybook 및 Mock Service Worker를 사용하여 API 응답 조롱"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/02/storybook-mockserviceworker-API-components.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/storybook-mockserviceworker-API-components.png?fit=730%2C487&ssl=1)

Storybook은 여러 상태에서 구성 요소를 미리 볼 수 있고 코드의 대화형 문서 역할을 할 뿐만 아니라 스토리 우선 개발이 가능하도록 라이브 환경을 갖추고 있기 때문에 JavaScript 애플리케이션을 위한 UI 구성 요소를 개발하는 가장 좋은 방법 중 하나입니다.

스토리북에 작은 UI 유닛을 제시하는 것은 간단하지만 API 요청을 하는 컴포넌트에 대해서는 개발자가 API를 조롱하는 솔루션을 손 내밀어 응답을 통제하고 실제 HTTP 통신을 빼내야 한다.

이 기사에서는 Mock Service Worker라는 API Mocking 라이브러리를 Storybook 프로젝트로 통합하려고 합니다.

## Mock Service Worker란 무엇인가?

MSW(Mock Service Worker)는 브라우저와 Node.js를 위한 API 모킹 라이브러리이다. REST와 GraphQL API의 풍부한 지원 외에도, 라이브러리의 주요 기능은 ServiceWorker를 통한 네트워크 수준의 인터셉션 요청이다. 즉, 테스트 또는 개발 중인 구성 요소가 어떠한 종류의 조롱도 인식하지 못하고 생산 시 동일한 요청을 계속 수행하므로 변경 사항이 전혀 발생하지 않습니다.

MSW는 스토리북과 결합해 내부와 외부 API 통신을 원활하게 제어할 수 있는 방법을 제공함으로써 타의 추종을 불허하는 컴포넌트 개발 경험을 강화한다. MSW가 Storybook에서 API를 가로채는 권장 방법 중 하나라는 것은 당연하다!

## Storybook 및 Mock Service Worker 프로젝트 설정

우리는 새로운 Create React App 프로젝트를 사용할 것입니다. Storybook과 MSW는 모두 프레임워크에 구애받지 않는 도구이므로 이 문서의 단계를 사용하여 Angular, Vue.js 또는 Svelte를 비롯한 다른 JavaScript 프로젝트에 통합할 수 있습니다.

> 프로젝트의 전체 소스 코드는 GitHub에서 볼 수 있습니다.

### 스토리북 설치

먼저 Storybook 설치부터 시작하겠습니다.

```shell
$ npx sb init
```

> 설치에 대한 자세한 내용은 Storybook 설명서의 시작하기 페이지를 참조하십시오.

스토리북을 설치하면 다음 두 개의 새 디렉터리가 프로젝트에 나타납니다.

```undefined
|-- .storybook
|   |-- main.js
|   |-- preview.js
|-- src
|   |-- /stories
```

다음으로 `msw` 패키지를 추가하겠습니다.

```shell
$ npm install msw --save-dev
```

### 서비스 작업자 초기화

Mock ServiceWorker는 브라우저에서 요청 가로채기를 활성화하는 worker 스크립트를 사용합니다. 라이브러리에는 해당 작업자 스크립트를 자동으로 초기화할 수 있는 지정된 CLI가 함께 제공됩니다.

worker 스크립트를 초기화하려면 `npx mswinit` 명령을 실행하여 프로젝트의 공용 디렉터리에 대한 상대 경로(생성-react-app의 경우 //public)를 제공하십시오.

```shell
$ npx msw init ./public
```

> 공용 디렉토리는 프로젝트에 따라 다를 수 있습니다. 참조할 수 있는 공용 디렉토리 목록을 참조하십시오.

## 반응 구성 요소 생성

우리의 프로젝트는 GitHub 사용자에 대한 짧은 세부사항을 표시하는 Retact 구성 요소가 될 것이다. 이러한 구성 요소를 다음과 같이 렌더링하는 것이 목적입니다.

```xml
<GitHubUser username="any-username" />
```

GitHubUser 구성 요소의 소스 코드를 간략히 살펴보겠습니다.

```js
// src/GitHubUser.jsx
import React from 'react'
import { useFetch } from '../../../hooks/useFetch'
import './GitHubUser.css'

export const GitHubUser = ({ username }) => {
  // Fetch user details from the GitHub API V3.
  const { data, loading, error, refetch } = useFetch(
    `https://api.github.com/users/${username}`
  )
  const { name, login, avatar_url } = data || {}

  // Compose some conditional classes based on the request state.
  const containerClassNames = [
    'container',
    loading && 'loading',
    error && 'error',
  ]
    .filter(Boolean)
    .join(' ')

  // Eventually, render some markup.
  return (
    <div className={containerClassNames}>
      <div className="avatar-container">
        {avatar_url && <img className="avatar" src={avatar_url} alt={name} />}
      </div>
      {error ? (
        <div>
          <p>Failed to fetch a GitHub user.</p>
          <button onClick={refetch}>Retry</button>
        </div>
      ) : (
        <div>
          <p className="name">{name}</p>
          <p className="username">{login}</p>
        </div>
      )}
    </div>
  )
}
```

지정된 사용자의 세부 정보를 가져오기 위해 이 구성 요소는 사용자 지정 `useFetch` 후크를 통해 GitHub API V3를 호출하는데, 이는 네이티브 `창`에 대한 작은 추상화이다.petch. API 호출이 실패할 경우 "재시도" 기능도 제공합니다.

구성 요소의 동작 중 유효한 부분이지만, 구성 요소가 수행하는 HTTP 요청은 Storybook에 속하지 않습니다. 스토리, 특히 타사 제공자에게 실제 요청을 하면 각 서비스에 대한 UI의 엄격한 의존성이 확립되어, 우리가 쓰는 스토리를 재현할 수 없게 되고 스토리북의 오프라인 사용을 비활성화할 수 있다.

## 이야기 쓰기

오늘은 Storybook에서 API를 조롱하는 데 초점을 맞추고 있기 때문에 기본(성공) 동작을 보여주는 GitHubUser 구성 요소에 대한 이야기를 추가해 보겠습니다.

```js
// stories/GitHubUser.stories.js
import { GitHubUser } from '../src/GitHubUser'

export default {
  title: 'GitHub User',
  component: GitHubUser,
}

export const DefaultState = () => <GitHubUser username="hamilton.elly" />
```

> 스토리북 설명서에 있는 스토리 작성에 대해 자세히 알아보십시오.

이 때 구성 요소는 렌더링하지만 실제 HTTP 요청을 만듭니다. 이제 혼합물에 API를 추가해야 할 때입니다.

## API 모킹 구현

MSW가 어떤 API 호출을 모의 실행할지 알려면 요청 술어(캡처할 요청)와 응답 해결사(그 요청에 응답하는 방법)를 설명하는 기능인 요청 처리기 집합을 선언해야 한다. 이후 동일한 요청 처리기를 사용하여 브라우저 내 조롱을 위한 작업자를 선언하거나 Node.js 환경에서 조롱을 위한 "서버"를 선언할 수 있다.

### 요청 처리기를 선언하는 중

프로젝트에 `src/mocks` 디렉토리를 생성하여 API Mocking과 관련된 모든 내용을 저장합니다. 이 디렉토리에서 `handler.js`라는 파일을 생성하고 다음 예에 따라 `GET/user/:userId` 요청에 대한 요청 처리기를 선언합니다.

```coffeescript
// src/mocks/handlers.js
import { rest } from 'msw'

export const handlers = [
  // Capture a GET /user/:userId request,
  rest.get('/user/:userId', (req, res, ctx) => {
    // ...and respond with this mocked response.
    return res(ctx.json({}))
  }),
]
```

> 요청 처리기는 스토리북 내에서, 로컬 개발 중에, 테스트 또는 디버깅을 위해 여러 용도로 재사용될 수 있기 때문에 별도의 모듈에서 선언합니다. 한 번 쓰고 어디에서나 재사용할 수 있습니다.

조롱을 쓸 때는 MSW를 조롱하는 "서버"라고 생각하라. 라이브러리는 실제 서버를 설정하지 않지만 응용프로그램에 대한 서버 역할을 합니다. 이를 염두에 두고 글로벌 `mocks/handler.js` 모듈에서 API의 "성공" 경로를 유지하면서 각 개별 사용 표면(예: 특정 스토리 또는 통합 테스트)에 더 가까운 시나리오별 재정의(예: 오류 응답)를 위임할 것을 권장한다.

MSW는 서비스 작업자를 사용하여 브라우저에서 요청 및 모의 응답을 가로채고 있습니다. 그렇기 때문에 우리는 그러한 감시를 책임지는 `노동자` 인스턴스를 만들 것이다.

`setupWorker` API를 사용하여 이전에 선언한 요청 처리기와 함께 제공하여 설정 단계에서 초기화한 ServiceWorker를 등록하고 활성화합니다.

```js
// src/mocks/browser.js
import { setupWorker } from 'msw'
import { handlers } from './handlers'

export const worker = setupWorker(...handlers)
```

worker 인터페이스는 API를 제어하기 위해 API를 노출하지만(예: start, stop) 우리는 아직 그것을 사용하지 않을 것이다. 대신 다음 단계에서 Storybook에 책임을 위임하겠습니다.

## MSW 및 API 통합

우리가 사용하는 툴이 변화에 탄력적으로 대처하는 것은 매우 중요합니다. MSW를 채택해야 하는 주요 이유 중 하나는 클라이언트와 무관하게 요청하기 때문에 응용 프로그램이 내일 다른 요청 라이브러리로 마이그레이션되거나 완전히 다른 API 규약으로 마이그레이션되더라도 동일한 통합을 사용할 수 있다는 것입니다.

이제 Storybook에서 .storybook/preview.js 파일을 편집하여 작업자를 조건부로 요구하고 이를 시작하도록 하여 API를 전체적으로 모킹할 수 있도록 합니다.

```undefined
// .storybook/preview.js
if (typeof global.process === 'undefined') {
  const { worker } = require('../src/mocks/browser')
  worker.start()
}
```

> node.js에서 실행되는 스토리북 빌드 중에 preview.js도 실행되므로 global.process 검사를 통해 Storybook이 브라우저가 아닌 환경에서 ServiceWorker를 활성화하려고 시도하지 않도록 합니다.

이 단계를 완료하면 다음 스토리의 브라우저 DevTools에서 MSW의 활성화 성공 메시지를 볼 수 있습니다.

MSW에 의해 UI와 DevTools 콘솔에서 요청이 성공적으로 처리되었음을 알 수 있습니다. 이 설정의 가장 좋은 점은 응용 프로그램의 코드를 변경할 필요가 없다는 것입니다! 여전히 GitHub API와 통신하지만 우리가 지정한 모조 응답을 수신합니다.

`src/mocks/handler.js`에 나열된 글로벌 요청 처리기는 성공적인 API 상호 작용을 유지하는 데 매우 좋습니다. 그러나 모든 교호작용이 성공적인 것은 아닙니다.

방탄 UI를 구축하려면 오류를 예상해야 하며 구성 요소가 사용자를 위해 이러한 오류를 올바르게 처리할 수 있는지 확인해야 합니다. 또한 각 스토리의 여러 네트워크 종속 상태에서 구성 요소의 시각적 그림을 살펴볼 수 있어야 합니다.

## 계층별 API 응답

Storybook의 이점 중 하나는 여러 주에서 하나의 구성 요소를 보여줄 수 있다는 것입니다. 구성 요소의 경우 구성 요소가 응답을 기다리는 동안 로드 상태와 GitHub API의 오류 응답 등 다양한 HTTP 통신 시나리오의 처리를 설명할 수 있다. 이를 위해 요청 핸들러를 계층별로 재정의할 수 있습니다.

우리는 스토리 장식기를 사용하여 런타임 요청 핸들러로 개별 스토리를 강화하려고 한다. 즉, 스토리가 렌더링될 때 런타임에 핸들러를 추가하거나 다시 쓰는 API이다.

### 로드 상태 모킹

비동기 작업은 시간이 걸릴 수 있으며 HTTP 호출도 예외가 아닙니다. 탁월한 사용자 환경을 보장하려면 구성 요소가 로드 상태를 처리할 수 있어야 하며, 스토리북은 로드 상태를 재현 가능하고 예측 가능한 방식으로 설명해야 한다.

운 좋게도, 당신은 그들의 응답 시간을 포함하여 조롱 받는 반응을 책임지고 있습니다. 그러나 관련 없는 이야기에 영향을 미치는 것을 원치 않을 수 있으므로 글로벌 요청 처리기에서 로드 상태를 조롱하는 것은 최선의 선택이 아닙니다. 대신, 적재 상태를 위한 조롱 논리는 이야기 바로 옆에 두세요. 다음과 같은 방법을 사용할 수 있습니다.

```js
// src/stories/Component.story.js
import { rest } from 'msw'
import { worker } from '../mocks/browser'

// Create a new loading state story.
const LoadingState = () => <GitHubUser username="hamilton.elly" />

// Use Storybook decorators and MSW runtime handlers
// to handle the same HTTP call differently for this particular story.
LoadingState.decorators = [
  (Story) => {
    worker.use(
      rest.get('https://api.github.com/users/:username', (req, res, ctx) => {
        // Mock an infinite loading state.
        return res(ctx.delay('infinite'))
      })
    )
    return <Story />
  },
]
```

런타임 요청 처리기를 프로비저닝하기 위해 `worker.use() 메서드를 사용하는 방법에 주목하십시오. 우리는 여전히 동일한 요청 방법과 URL을 제공하지만 응답을 무기한 지연시키는 다른 해결사 함수를 제공한다(`ctx.delay` 유틸리티 참조). 이렇게 하면 UI에서 구성 요소가 로드 상태를 처리하는 방법을 표시하는 데 필요한 응답을 보류 상태로 유지합니다.

브라우저의 DevTools에서 Network(네트워크) 탭을 검사하면 GitHub API 요청이 해결되지 않아 스토리의 상태를 미리 볼 수 있습니다. 그렇기 때문에 구성 요소에서 발생하는 API 호출에 대한 유연성과 제어력을 확보하기 위해 API가 필요합니다.

> MSW에는 간단한 API와 다양한 유틸리티가 포함되어 있어 응답 상태 코드, 헤더, 서버 쿠키 등을 에뮬레이트하여 인증, CORS 또는 미디어 콘텐츠 스트리밍과 같은 실제 시나리오를 조롱할 수 있다.

### 오류 응답 모킹

로드 상태와 마찬가지로 오류 응답에 대한 별도의 스토리를 작성할 수 있으며 특정 HTTP 서버 오류로 항상 응답하는 런타임 요청 처리기가 있을 수 있습니다.

```js
// src/stories/Component.story.js
import { msw } from 'msw'
import { worker } from '../mocks/browser'

const ErrorState = () => <GitHubUser username="hamilton.elly" />
ErrorState.decorators = [
  (Story) => {
    worker.use(
      rest.get('https://api.github.com/users/:username', (req, res, ctx) => {
        // Respond with a 500 response status code.
        return res(ctx.status(500))
      })
    )
    return <Story />
  },
]
```

> 'ctx.status' 및 기타 컨텍스트 유틸리티를 사용하여 구성 요소의 동작을 표시하는 데 필요한 정확한 HTTP 응답을 모델링합니다.

변경 내용을 저장하고 Storybook으로 이동하면 다음과 같은 오류 상태가 재현됩니다.

이제 오류 처리가 표시되지만, 재시도 버튼을 클릭하면 런타임 요청 처리기에서 지정한 것과 같이 항상 500개의 응답을 반환하는 요청이 생성됩니다.

GitHub API에 대한 첫 번째 요청만 하면 오류 응답을 돌려주면 좋을 것 같습니다. 런타임 처리기에서 `res` 대신 `res.once` 함수를 사용하여 다음을 수행할 수 있습니다.

```coffeescript
rest.get('https://api.github.com/users/:username', (req, res, ctx) => {
-  return res(ctx.status(500))
+  return res.once(ctx.status(500))
})
```

## 결론

본 튜토리얼에서는 Storybook과 Mock Service Worker 간의 시너지, 여러 상태에서 동일한 구성 요소를 표시할 때 조롱당한 API 응답에 대한 세분화된 제어의 이점, 그리고 두 기술을 원활하게 통합하는 방법에 대해 알아보았다.

더욱이 MSW는 브라우저와 Node.js 모두에서 실행될 수 있기 때문에, 우리는 테스트와 개발을 위해 동일한 API 모킹 논리를 재사용할 수 있으며, 생산적이고 원활한 통합을 끝낼 수 있다.

이 예제의 소스 코드는 GitHub에서 찾을 수 있으며 API Mocking에 대한 자세한 내용은 MSW 설명서를 참조하십시오.