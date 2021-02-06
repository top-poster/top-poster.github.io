---
layout: post
title: "React Frontload를 사용하는 방법 및 이유"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/react-frontload.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/react-frontload.png?fit=730%2C487&ssl=1)

웹 앱은 데이터를 로드하고 효과적으로 표시하는 두 가지 주요 활동에 중점을 둡니다. 리액트는 네이티브 CSS, JS의 CSS, 그리고 다른 스타일링 대안들의 도움으로 데이터를 표시하는 일을 잘 하지만, 로딩 부분에 관해서는 부족하다.

적절한 라이프사이클 이벤트와 함께 가져오는 것이 완전한 클라이언트 측 애플리케이션에 적합한 솔루션일 수 있지만 서버에 문제가 발생하면 충분하지 않습니다. 여기서 React Frontload라고 하는 타사 라이브러리가 사용됩니다. React Frontload는 환경 간 데이터 가져오기 문제를 해결하는 데 효과적인 도구이며 데이터를 인라인으로 로드할 수 있는 완벽한 인터페이스를 갖추고 있습니다.

그게 다예요? 알아보자!

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/react-frontload-logo-1.png?resize=730%2C181&ssl=1)

## 반응 프론트 로드란 무엇입니까?

React Frontload는 서버 및 클라이언트 환경 모두에서 인라인으로 데이터를 가져올 수 있는 React 기반 응용 프로그램용 라이브러리입니다. 통합 및 사용이 매우 간단한 몇 가지 기능을 제공합니다. 데이터 로드를 위한 후크(Hook)와 상태 저장 객체의 데이터를 반환하는 `frontloadMeta` 객체를 제공하여 네트워크 요청 상태를 추적합니다.

여기서 주목해야 할 중요한 사항은 Frontload 최신 버전에는 기본 제공 상태 관리 솔루션이 함께 제공되므로 개발자로서 Redux 또는 MobX와 같은 상태 컨테이너에 데이터를 수동으로 넘길 염려가 없다는 것입니다.

리액트 프런트로드가 npm을 통해 매주 6k가 넘는 다운로드를 기록하며 간단한 설정 및 개발자 친화적인 인터페이스로 주목을 받기 시작했다. 실행 방법에 대해 자세히 알아보기 전에 원래 문제와 Frontload가 문제를 해결하는 방법을 자세히 살펴보겠습니다.

## 어떤 문제를 해결합니까?

앞에서 언급한 바와 같이 React는 데이터 페팅을 처리할 수 있는 기본 솔루션을 제공하지 않습니다. JavaScript의 가져오기 API는 외부 API 끝점을 타격하고 데이터를 수신할 수 있을 만큼 충분히 가능합니다. 하지만 React 애플리케이션에 대해서는 더 많은 작업이 수행되어야 합니다. 데이터가 수신되면 React DOM에 요청 결과를 알려야 합니다.

요청이 실패하면 시나리오를 적절하게 처리해야 합니다. 이러한 경우에 적합한 솔루션을 찾을 때쯤이면 서버측 렌더링된 응용프로그램이 시작되므로 가져오기 + 사용 효과 솔루션이 무의미해집니다.

서버 측 렌더링에 문제가 있습니다. 렌더링 UI와 병렬로 데이터를 가져오는 비동기식 방식은 개발자에게 매우 적합했지만, 서버측 렌더링은 사용자가 모든 API 호출이 완료되고 모든 데이터가 로드되기 전에 기다렸다가 레이아웃을 렌더링하여 브라우저 클라이언트로 전송해야 합니다.

구성 요소 내부에서 데이터를 비동기적으로 가져오는 기존 방법이 무관하게 되는 이유다.

이를 해결하기 위한 좋은 방법은 서버측 렌더링된 앱에서 데이터를 전체적으로 로드하여 다양한 구성 요소에 전달하는 것이라고 주장할 수 있습니다. 루트 구성 요소는 여전히 데이터를 가져와 소품을 통해 하위에게 전달할 수 있기 때문에 클라이언트 측에서도 작동합니다.

그러나 이것은 실질적인 해결책이 아니다. 이 접근 방식은 검색 결과 페이지나 사용자 프로파일 페이지와 같은 구조에서 효과적일 수 있지만, 사용자의 피드나 소셜 미디어 게시물과 같은 경우 불필요하게 복잡성을 증가시킨다.

React Frontload는 이 문제를 해결하는 데 도움이 될 수 있습니다. 이 라이브러리는 SSR 애플리케이션과의 내부 호환성을 통해 데이터 가져오기를 구현하는 구성 요소 중심 접근 방식을 제공합니다. 즉, 두 시나리오에 대해 서로 다른 솔루션을 구현할 필요가 없으며 애플리케이션 아키텍처도 타협해야 할 필요가 없습니다.

### 그것은 어떻게 작동하나요?

React Frontload는 구성 요소별 데이터 가져오기 방법을 기반으로 구축되며 동기화된 요청을 처리하기 위해 응용 프로그램의 서버 측 렌더링을 조정합니다. 즉, 웹 서버가 표준 React 클라이언트와 마찬가지로 비동기식으로 작동하도록 지시되므로 React Hook을 통해 Frontload API에 액세스할 수 있고 SSR에서 계속 작동할 수 있습니다.

React Frontload는 서버가 비동기식으로 데이터를 로드하고 페이지를 렌더링하도록 도와줍니다. 클라이언트 측 수화 데이터를 동시에 반환합니다. 당신이 해야 할 일은 데이터에 수분을 공급하는 코드 한 줄을 클라이언트에 추가하는 것이다. 도서관의 설정을 조사해 보면 더 잘 이해할 수 있을 거예요.

React Frontload는 `reactFrontload Server` 호출을 사용하여 내부적으로 `use Frontload` 후크를 찾는 React 소스에서 클라이언트측 코드를 렌더링합니다. 이러한 후크가 발생할 때마다 후크의 약속은 수집됩니다. 모든 약속이 수집되면 대기하고 데이터가 해당 구성요소에 주입됩니다.

작업이 완료되면 `reactFrontLoad Server` 호출이 소스를 통해 다시 실행되어 이전의 약속 컬렉션에서 비롯되었을 수 있는 새로운 `useFrontload` 호출을 찾습니다. 새로운 호출이 발견되면 해당 호출과 함께 프로세스가 반복됩니다. 새 호출이 발생하지 않으면 렌더러가 최종으로 표시되고 클라이언트로 전송됩니다.

## React Frontload 시작

### 초기 설정

React Frontload를 사용하기 전에 몇 가지 구성을 변경해야 합니다.

다음과 같은 작업을 수행해야 합니다.

React Frontload 공급자로 앱 포장

```js
import { FrontloadProvider } from 'react-frontload'

const App = ({ frontloadState }) => (
  <FrontloadProvider initialState={frontloadState}>
    <Content>...</Content>
  </FrontloadProvider>
)
```

SSR 앱이 있는 경우 기존 동기식 서버 렌더 코드를 `reactFrontloadServerRender`로 줄바꿈합니다.

reactFrontloadServerRender는 React 서버 렌더링의 작동 방식과 유사합니다. 위에서 설명한 대로 비동기식 로딩 부품만 추가된다.

```js
import { renderToString } from 'react-dom/server'
import { createFrontloadState, frontloadServerRender } from 'react-frontload'
import apiClient from './api_client'

app.get('*', async (req, res) => {
  ...

  // create a new state object for each render
  // this will eventually be passed to the FrontloadProvider in <App>
  const frontloadState = createFrontloadState.server({

  // apiClient is a custom implementation of an API handler, that can make HTTP calls, DB operations etc 
    context: { api: apiClient}
  })

  try {
    // frontloadServerRender is first used to fetch data instead of the generic, straightforward renderToString
    const { rendered, data } = await frontloadServerRender({
      frontloadState,
      render: () => renderToString(<App frontloadState={frontloadState} />)
    })

    res.send(`
      <html>
        ...
        <!-- server rendered markup -->
        ${rendered}

        <!-- loaded data hydration on client -->
        <script>window._frontloadData=${toSanitizedJSON(data)}</script>
        ...
      </html>
    `)
  } catch (err) {
    ...
  }
})
```

frontloadServerRender는 렌더링된 마크업뿐만 아니라 데이터도 반환합니다. 그런 다음 마크업이 그대로 클라이언트에 전송되고 데이터는 수분 보충으로 표시됩니다.

클라이언트측 렌더링 시 구성 요소의 초기 보기와 관련된 데이터가 다시 로드되지 않고 `initialState`를 통해 구성 요소에 사전 추출된 데이터가 주입되도록 하기 위해 수행됩니다.

고객 측에서 수화 작업을 처리합니다.

SSR 지원 애플리케이션의 마지막 단계는 클라이언트 측에서 초기 데이터 부하를 처리하는 것입니다.

이 작업은 다음 코드로 수행할 수 있습니다.

```js
import apiClient from './api_client'

const frontloadState = createFrontloadState.client({
  // use client's implementation of api to load data.
  context: { api: apiClient },

  // hydrate state from SSR
  serverRenderedData: window._frontloadData
})
...
ReactDOM.hydrate(<App frontloadState={frontloadState} />, ...)
```

초기 설정은 여기까지입니다. 이제 구성 요소 내부에서 데이터를 가져올 때 이 기능을 사용하는 방법에 대해 살펴보겠습니다.

### 반응 전면 부하 사용 방법

다음은 React Frontload를 사용하여 API에서 데이터를 비동기식으로 로드하고 HTTP 요청의 가능한 모든 결과도 처리하는 샘플 구성 요소입니다.

```js
const Component = () => {
  const { data, frontloadMeta } = useFrontload('some-component', async ({ api }) => ({
    something: await api.fetchSomething()
  }))

  if (frontloadMeta.pending) return <div>loading</div>
  if (frontloadMeta.error)   return <div>error</div>

  return <div>{data.something}</div>
}
```

위의 예에서 useFrontload 후크는 메타데이터와 메타데이터를 가져오는 데 사용되었습니다. 데이터 개체는 API 호출의 응답을 포함하며 API 클라이언트가 가져오는 속성 `something`을 포함하는 개체로 정의됩니다.

이 API 클라이언트(`api`)는 초기 설정의 3단계에서 전달할 클라이언트와 동일합니다. 일반적인 API 클라이언트는 원격 서버 기반 작업을 수행하고 결과 및/또는 데이터를 반환하는 데 사용할 수 있는 방법을 노출합니다.

API 클라이언트가 데이터를 로드하는 동안 메타데이터 개체는 보류 중인 속성을 `true`로 설정하며, 위의 경우 사용자에게 로드 UI를 표시하는 데 사용됩니다. 또한 `meta.error` 속성을 확인하여 오류를 쉽게 처리할 수 있습니다.

use Frontload는 다양한 상황에서 유용하게 사용할 수 있는 일부 다른 특성도 노출한다. 예를 들어 사용자 작업을 기반으로 더 많은 데이터를 가져오려는 경우 나중에 데이터 개체를 업데이트할 수 있는 데이터 변수와 함께 `setData` 메서드가 노출됩니다.

구현 방법은 다음과 같습니다.

```js
const Component = () => {
  // setData is a tiny reducer that can update the data object on the fly.
  const { data, setData, frontloadMeta } = useFrontload('some-component', async ({ api }) => ({
    something: await api.fetchSomething()
  }))

  if (frontloadMeta.pending) return <div>loading</div>
  if (frontloadMeta.error)   return <div>error</div>

  const updateSomething = async () => {
    try {
      const updatedSomething = await updateSomething('new value') // API call to update the data
      setData(data => ({ ...data, something: updatedSomething })) // update data in state
    } catch {
      ...
    }
  }

  return (
    <>
      <div>{data.something}</div>
      <button onClick={updateSomething}>Update!</button>
    <>
  )
```

간단해 보이지 않나요? 사태를 한 단계 격상시키자.

다음은 React Frontload를 사용하는 한 구성 요소에서 두 개의 API 호출입니다.

```js
const Component = () => {
  const { data, frontloadMeta } = useFrontload('some-component', async ({ api }) => ({
    something: await api.fetchSomething(),
    someMoreThings: await api.fetchSomeMoreThings() // just added this as an additional API call
  }))

  if (frontloadMeta.pending) return <div>Loading!</div>
  if (frontloadMeta.error)   return <div>Something went wrong</div>

  return <div>{data.something} and {data.someMoreThings}</div> 
}
```

우리가 놓쳐버린 작은 것이 하나 있다. 위의 예에서 사용 중인 useFrontload 호출의 성능을 살펴보면 속도가 상당히 느립니다.

하나하나 약속을 기다리고 있기 때문인 것으로 보인다. 이 값은 모두 병렬로 로드하면 개선될 수 있습니다.

동일한 조각에서 이 문제를 해결할 수 있는 방법은 다음과 같습니다.

```js
const Component = () => {
  const { data, frontloadMeta } = useFrontload('some-component', async ({ api }) => {
    const [something, someMoreThings] = await Promise.all([
    api.fetchSomething(),
    api.fetchSomeMoreThings()
  ])

  return { something, someMoreThings }
  })

  if (frontloadMeta.pending) return <div>Loading!</div>
  if (frontloadMeta.error)   return <div>Something went wrong</div>

  return <div>{data.something} and {data.someMoreThings}</div> 
}
```

바로 그거예요. 상태 컨테이너와 함께 제공되는 초고속 데이터 가져오기 기능을 통해 데이터를 최대한 신속하게 가져오고 렌더링하고 업데이트할 수 있습니다.

## 결론

React Frontload는 원격 데이터를 효율적으로 처리하는 데 유용한 도구이며, 모든 기능을 고려할 때만 성능이 향상됩니다. React 개발자는 신뢰할 수 있는 애플리케이션의 신속한 확장을 목표로 하는 경우 라이브러리를 한 번 이상 체크 아웃해야 합니다. 저는 이것이 여러분의 요구에 적합하고 여러분의 개발 과정을 훨씬 더 빠르게 만들어 줄 것이라고 확신합니다.

이 프로젝트는 현재 개발이 진행 중이며, 최신 버전에 가장 인기 있는 기능들이 많이 소개되었기 때문에 시간이 지나면 좋아질 것이라고 해도 과언이 아니다.