---
layout: post
title: "반응 후크를 사용하여 데이터 요청 조절"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/throttling-data-requests-with-react-hooks-2.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/throttling-data-requests-with-react-hooks-2.png?fit=730%2C487&ssl=1)

응용 프로그램이 데이터를 로드하면 일반적으로 HTTP 요청이 상대적으로 적게 수행됩니다. 예를 들어 학생 관리 응용 프로그램을 만든다고 가정할 경우 "보기" 화면에서 해당 학생의 데이터를 표시하기 전에 로드하기 위한 단일 HTTP 요청을 만들 수 있습니다.

경우에 따라 응용 프로그램이 많은 HTTP 요청을 해야 합니다. 데이터를 로드한 다음 프레젠테이션을 위해 집계하는 보고 애플리케이션을 고려하십시오.

이러한 요구는 다음과 같은 두 가지 흥미로운 문제를 제시합니다.

- 데이터를 어떻게 점진적으로 로드합니까?
- 로드 진행률을 사용자에게 어떻게 표시합니까?

이 튜토리얼에서는 사용자 정의 React Hook을 사용하여 이러한 문제를 해결하는 방법에 대해 설명합니다.

## 크롬을 굴복시키자.

이제 Create React App이 포함된 TypeScript React 앱의 회전 속도를 높여 여행을 시작하겠습니다.

```undefined
npx create-react-app throttle-requests-react-hook --template typescript
```

우리는 비동기식 통화를 여러 번 할 것이기 때문에 use Async 후크에 널리 사용되는 `react-use`에 기대어 코드를 단순화시킬 것이다.

```bash
cd throttle-requests-react-hook
yarn add react-use
```

App.css 파일을 다음과 같이 바꿉니다.

```css
.App {
  text-align: center;
}

.App-header {
  background-color: #282c34;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: calc(10px + 2vmin);
  color: white;
}

.App-labelinput > * {
  margin: 0.5em;
  font-size:24px;
}

.App-link {
  color: #61dafb;
}

.App-button {
  font-size: calc(10px + 2vmin);
  margin-top: 0.5em;
  padding: 1em;
  background-color: cornflowerblue;
  color: #ffffff;
  text-align: center;
}

.App-progress {
  padding: 1em;
  background-color: cadetblue;
  color: #ffffff;
}

.App-results {
  display: flex;
  flex-wrap: wrap;
}

.App-results > * {
  padding: 1em;
  margin: 0.5em;
  background-color: darkblue;
  flex: 1 1 300px;
}
```

그런 다음 `App.tsx` 콘텐츠를 다음과 같이 바꿉니다.

```js
import React, { useState } from "react";
import { useAsync } from "react-use";
import "./App.css";

function use10_000Requests(startedAt: string) {
  const responses = useAsync(async () => {
    if (!startedAt) return;

    // make 10,000 unique HTTP requests
    const results = await Promise.all(
      Array.from(Array(10_000)).map(async (_, index) => {
        const response = await fetch(
          `/manifest.json?querystringValueToPreventCaching=${startedAt}_request-${index}`
        );
        const json = await response.json();
        return json;
      })
    );

    return results;
  }, [startedAt]);

  return responses;
}


function App() {
  const [startedAt, setStartedAt] = useState("");
  const responses = use10_000Requests(startedAt);

  return (
    <div className="App">
      <header className="App-header">
        <h1>The HTTP request machine</h1>
        <button
          className="App-button"
          onClick={(_) => setStartedAt(new Date().toISOString())}
        >
          Make 10,000 requests
        </button>
        {responses.loading && <div>{progressMessage}</div>}
        {responses.error && <div>Something went wrong</div>}
        {responses.value && (
          <div className="App-results">
            {responses.value.length} requests completed successfully
          </div>
        )}
      </header>
    </div>
  );
}

export default App;
```

우리가 만든 앱은 매우 간단합니다. 버튼을 누르면 Fetch API를 사용하여 10,000개의 HTTP 요청을 병렬로 실행할 수 있습니다. 이 경우 요청되는 데이터는 임의 JSON 파일인 manifest.json입니다. 자세히 보면 캐시된 데이터를 가져오지 않기 위해 URL을 사용하여 쿼리 문자열 트릭을 수행하는 것을 볼 수 있습니다.

실제로 이 데모에서는 이러한 HTTP 요청의 결과에 관심이 없습니다. 대신, 브라우저가 이 접근 방식을 어떻게 대처하는지에 관심이 있습니다(스포일러: 좋지 않음). 브라우저와 동일한 컴퓨터에서 실행 중인 서버에서 텍스트 파일을 요청하는 속도가 빨라야 한다는 점을 고려해 볼 필요가 있습니다.

그래서 우리 `http://localhost:3000`에 앱으로 가서`yarnstart` 운영할 것입니다. Devtools를 열고 실행하면 다음과 같은 문제가 발생합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/i-want-it-all.gif?resize=720%2C327&ssl=1)

위의 gif는 간결하게 편집되었습니다. 실제로 첫 번째 요청이 발사되는 데 20초가 걸렸다. 그 전에는 크롬이 반응이 없었다. 요청이 실행되기 시작했을 때 `net::ERR_INSUFFICE_RESOURCES`. 실행되기 전에 실행 중지된 요청이 있습니다. 이는 Chrome이 연결 수를 제한한 결과입니다(모든 브라우저는 다음 작업을 수행합니다).

> 이 오리진에 대해 열려 있는 TCP 연결이 이미 6개 있습니다. 제한입니다. HTTP/1.0 및 HTTP/1.1에만 적용됩니다.

요약하자면, 현재 접근 방식의 문제는 세 가지입니다.

- 브라우저가 응답하지 않습니다.
- 리소스 부족으로 인해 HTTP 요청이 실패함
- 사용자가 진행과 관련된 정보를 보지 못함

## 이것 좀 집어치워.

모든 요청을 한 번에 실행하여 브라우저를 망치로 치는 대신, 우리는 대신 스로틀을 구현할 수 있습니다. 스로틀은 작업을 수행하는 속도를 제한할 수 있는 메커니즘입니다.

이 경우 HTTP 요청 속도를 제한하고자 합니다. 스로틀은 처음 두 가지 문제를 해결하며, 기본적으로 브라우저를 자유롭고 쉽게 유지하고 요청이 성공적으로 전송되도록 합니다. 또한 사용자에게 진행 상황에 대해 계속 알려드리고자 합니다.

이제 `스로틀 요청 사용` 후크를 공개해야 할 때이다.

```js
import { useMemo, useReducer } from "react";
import { AsyncState } from "react-use/lib/useAsync";

/** Function which makes a request */
export type RequestToMake = () => Promise<void>;

/**
 * Given an array of requestsToMake and a limit on the number of max parallel requests
 * queue up those requests and start firing them
 * - inspired by Rafael Xavier's approach here: https://stackoverflow.com/a/48007240/761388
 *
 * @param requestsToMake
 * @param maxParallelRequests the maximum number of requests to make - defaults to 6
 */
async function throttleRequests(
  requestsToMake: RequestToMake[],
  maxParallelRequests = 6
) {
  // queue up simultaneous calls
  const queue: Promise<void>[] = [];
  for (let requestToMake of requestsToMake) {
    // fire the async function, add its promise to the queue,
    // and remove it from queue when complete
    const promise = requestToMake().then((res) => {
      queue.splice(queue.indexOf(promise), 1);
      return res;
    });
    queue.push(promise);

    // if the number of queued requests matches our limit then
    // wait for one to finish before enqueueing more
    if (queue.length >= maxParallelRequests) {
      await Promise.race(queue);
    }
  }
  // wait for the rest of the calls to finish
  await Promise.all(queue);
}

/**
 * The state that represents the progress in processing throttled requests
 */
export type ThrottledProgress<TData> = {
  /** the number of requests that will be made */
  totalRequests: number;
  /** the errors that came from failed requests */
  errors: Error[];
  /** the responses that came from successful requests */
  values: TData[];
  /** a value between 0 and 100 which represents the percentage of requests that have been completed (whether successfully or not) */
  percentageLoaded: number;
  /** whether the throttle is currently processing requests */
  loading: boolean;
};

function createThrottledProgress<TData>(
  totalRequests: number
): ThrottledProgress<TData> {
  return {
    totalRequests,
    percentageLoaded: 0,
    loading: false,
    errors: [],
    values: [],
  };
}

/**
 * A reducing function which takes the supplied `ThrottledProgress` and applies a new value to it
 */
function updateThrottledProgress<TData>(
  currentProgress: ThrottledProgress<TData>,
  newData: AsyncState<TData>
): ThrottledProgress<TData> {
  const errors = newData.error
    ? [...currentProgress.errors, newData.error]
    : currentProgress.errors;

  const values = newData.value
    ? [...currentProgress.values, newData.value]
    : currentProgress.values;

  const percentageLoaded =
    currentProgress.totalRequests === 0
      ? 0
      : Math.round(
          ((errors.length + values.length) / currentProgress.totalRequests) * 100
        );

  const loading =
    currentProgress.totalRequests === 0
      ? false
      : errors.length + values.length < currentProgress.totalRequests;

  return {
    totalRequests: currentProgress.totalRequests,
    loading,
    percentageLoaded,
    errors,
    values,
  };
}

type ThrottleActions<TValue> =
  | {
      type: "initialise";
      totalRequests: number;
    }
  | {
      type: "requestSuccess";
      value: TValue;
    }
  | {
      type: "requestFailed";
      error: Error;
    };

/**
 * Create a ThrottleRequests and an updater
 */
export function useThrottleRequests<TValue>() {
  function reducer(
    throttledProgressAndState: ThrottledProgress<TValue>,
    action: ThrottleActions<TValue>
  ): ThrottledProgress<TValue> {
    switch (action.type) {
      case "initialise":
        return createThrottledProgress(action.totalRequests);

      case "requestSuccess":
        return updateThrottledProgress(throttledProgressAndState, {
          loading: false,
          value: action.value,
        });

      case "requestFailed":
        return updateThrottledProgress(throttledProgressAndState, {
          loading: false,
          error: action.error,
        });
    }
  }

  const [throttle, dispatch] = useReducer(
    reducer,
    createThrottledProgress<TValue>(/** totalRequests */ 0)
  );

  const updateThrottle = useMemo(() => {
    /**
     * Update the throttle with a successful request
     * @param values from request
     */
    function requestSucceededWithData(value: TValue) {
      return dispatch({
        type: "requestSuccess",
        value,
      });
    }

    /**
     * Update the throttle upon a failed request with an error message
     * @param error error
     */
    function requestFailedWithError(error: Error) {
      return dispatch({
        type: "requestFailed",
        error,
      });
    }

    /**
     * Given an array of requestsToMake and a limit on the number of max parallel requests
     * queue up those requests and start firing them
     * - based upon https://stackoverflow.com/a/48007240/761388
     *
     * @param requestsToMake
     * @param maxParallelRequests the maximum number of requests to make - defaults to 6
     */
    function queueRequests(
      requestsToMake: RequestToMake[],
      maxParallelRequests = 6
    ) {
      dispatch({
        type: "initialise",
        totalRequests: requestsToMake.length,
      });

      return throttleRequests(requestsToMake, maxParallelRequests);
    }

    return {
      queueRequests,
      requestSucceededWithData,
      requestFailedWithError,
    };
  }, [dispatch]);

  return {
    throttle,
    updateThrottle,
  };
}
```

`usse ThrottleRequests` 후크는 다음 두 가지 속성을 반환합니다.

- `스로틀`, `서슬픈 진보`TDATA>`는 다음과 같은 데이터를 포함한다.
총 요청 수, 총 요청 수
요청 실패 시 발생하는 오류
성공적인 요청에서 나온 응답인 `값`
0에서 100 사이의 값으로 완료된 요청(성공 여부)의 백분율을 나타냅니다.
스로틀이 현재 요청을 처리 중인지 여부 `로드`
- 총 요청 수, 총 요청 수
- 요청 실패 시 발생하는 오류
- 성공적인 요청에서 나온 응답인 `값`
- 0에서 100 사이의 값으로 완료된 요청(성공 여부)의 백분율을 나타냅니다.
- 스로틀이 현재 요청을 처리 중인지 여부 `로드`
- 다음 세 가지 기능을 표시하는 개체인 `update Throttle`:
대기열에 넣고 조절된 방식으로 실행해야 하는 요청을 전달하는 함수인 `queueRequests`
`요청 성공`Data를 사용하여 요청이 데이터 제공에 성공할 경우 호출되는 함수
requestFailedWithError, 요청이 오류를 제공하지 못할 경우 호출되는 함수
- 대기열에 넣고 조절된 방식으로 실행해야 하는 요청을 전달하는 함수인 `queueRequests`
- `요청 성공`Data를 사용하여 요청이 데이터 제공에 성공할 경우 호출되는 함수
- requestFailedWithError, 요청이 오류를 제공하지 못할 경우 호출되는 함수

그것은 우리의 `스로틀 요청 사용` 후크를 설명하는 많은 단어이다. `use10_000Requests` 후크를 마이그레이션하여 사용 방법을 살펴보겠습니다.

다음은 `App.tsx`의 새로운 구현입니다.

```js
import React, { useState } from "react";
import { useAsync } from "react-use";
import { useThrottleRequests } from "./useThrottleRequests";
import "./App.css";

function use10_000Requests(startedAt: string) {
  const { throttle, updateThrottle } = useThrottleRequests();
  const [progressMessage, setProgressMessage] = useState("not started");

  useAsync(async() => {
      if (!startedAt) return;

      setProgressMessage("preparing");

      const requestsToMake = Array.from(Array(10_000)).map(
        (_, index) => async () => {
          try {
            setProgressMessage(`loading ${index}...`);

            const response = await fetch(
              `/manifest.json?querystringValueToPreventCaching=${startedAt}_request-${index}`
            );
            const json = await response.json();

            updateThrottle.requestSucceededWithData(json);
          } catch (error) {
            console.error(`failed to load ${index}`, error);
            updateThrottle.requestFailedWithError(error);
          }
        }
      );

      await updateThrottle.queueRequests(requestsToMake);

  }, [startedAt, updateThrottle, setProgressMessage]);

  return { throttle, progressMessage };
}

function App() {
  const [startedAt, setStartedAt] = useState("");

  const { progressMessage, throttle } = use10_000Requests(startedAt);

  return (
    <div className="App">
      <header className="App-header">
        <h1>The HTTP request machine</h1>
        <button
          className="App-button"
          onClick={(_) => setStartedAt(new Date().toISOString())}
        >
          Make 10,000 requests
        </button>
        {throttle.loading && <div>{progressMessage}</div>}
        {throttle.values.length > 0 && (
          <div className="App-results">
            {throttle.values.length} requests completed successfully
          </div>
        )}
        {throttle.errors.length > 0 && (
          <div className="App-results">
            {throttle.errors.length} requests errored
          </div>
        )}
      </header>
    </div>
  );
}

export default App;
```

새로운 `10_000 Requests` 후크를 보면 우리의 이전 구현과는 미묘한 차이가 몇 가지 있다. 우선, 우리는 이제 `스로틀 프로그레스`인 `스로틀 프로그레스`를 노출시키고 있다.TDATA > 01. 업데이트된 후크는 스로틀 실행 시 업데이트되는 useState와 함께 저장된 단순한 문자열인 `progressMessage`도 노출한다.

사실, 여기서 표면화되고 있는 정보는 그다지 흥미롭지 않습니다. `progressMessage`는 디스플레이 목적으로 요청이 완료될 때 일부 데이터를 캡처할 수 있다는 것을 보여주기 위한 것입니다(예를 들어 실행 중인 합계).

그렇다면, 우리의 새로운 후크 접근 방식은 어떤 성능을 발휘할까요?

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/i-want-it-all-with-hook.gif?resize=730%2C332&ssl=1)

정말 성능이 좋군요! 다시, 위의 gif가 간략하게 편집되었습니다. 이전 접근 방식에 직면했던 문제를 되돌아보면 어떻게 비교할 수 있을까요?

- 브라우저가 응답성을 유지합니다.
- 브라우저에 HTTP 요청에 오류가 발생하지 않습니다.
- 진행 상황 세부 정보가 사용자에게 표시됩니다.

굉장해!

## 뭘 지을까요?

우리의 현재 예는 분명히 조작된 것이다. 좀 더 현실적인 시나리오에 `스로틀 요청 사용` 후크를 적용해보자.

우리는 GitHub에 대한 보고서를 통해 모든 기여자들의 블로그를 나열하는 애플리케이션을 만들 것입니다. GitHub 프로필에 블로그 URL을 지정할 수 있습니다. 많은 사람들이 이를 사용하여 자신의 트위터 프로필을 지정합니다.

탁월한 GitHub REST API 덕분에 이를 구축할 수 있는데, 이 API는 리포지토리 기여자를 나열하고 사용자를 얻는 두 가지 목표를 제시합니다.

### 1. 리포지토리 기여자 나열

리포지토리 기여자 나열 URL에 지정된 리포지토리에 대한 기여자 목록: `GET https://api.github.com/repos/{owner}/{repo}/declosators` 응답은 객체의 배열로, 사용자의 API 끝점을 가리키는 `url` 속성을 가지고 있다.

```cpp
[
  // ...
  {
    // ...
    "url": "https://api.github.com/users/octocat",
    // ...
  },
  // ...
]
```

### 2. 사용자 가져오기

Get user는 위의 url 속성이 참조하는 API입니다. 호출되면 사용자에 대해 공개적으로 사용 가능한 정보를 나타내는 개체를 반환합니다.

```cpp
{
  // ...
  "name": "The Octocat",
  // ...
  "blog": "https://github.blog",
  // ...
}
```

## 블로그 개발 v1.0

이제 블로그 개발 앱을 구축할 준비가 되었습니다. 기존 `App.tsx`를 다음으로 교체해 보겠습니다.

```undefined
import React, { useCallback, useMemo, useState } from "react";
import { useAsync } from "react-use";
import { useThrottleRequests } from "./useThrottleRequests";
import "./App.css";

type GitHubUser = { name: string; blog?: string };

function timeout(ms: number) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

function useContributors(contributorsUrlToLoad: string) {
  const { throttle, updateThrottle } = useThrottleRequests<GitHubUser>();
  const [progressMessage, setProgressMessage] = useState("");

  useAsync(async () => {
    if (!contributorsUrlToLoad) return;

    setProgressMessage("loading contributors");

    // load contributors from GitHub
    const contributorsResponse = await fetch(contributorsUrlToLoad);
    const contributors: { url: string }[] = await contributorsResponse.json();

    setProgressMessage(`loading ${contributors.length} contributors...`);

    // For each entry in result, retrieve the given user from GitHub
    const requestsToMake = contributors.map(({ url }, index) => async () => {
      try {
        setProgressMessage(
          `loading ${index} / ${contributors.length}: ${url}...`
        );

        const response = await fetch(url);
        const json: GitHubUser = await response.json();

        // wait for 1 second before completing the request
        // - makes for better demos
        await timeout(1000);

        updateThrottle.requestSucceededWithData(json);
      } catch (error) {
        console.error(`failed to load ${url}`, error);
        updateThrottle.requestFailedWithError(error);
      }
    });

    await updateThrottle.queueRequests(requestsToMake);

    setProgressMessage("");
  }, [contributorsUrlToLoad, updateThrottle, setProgressMessage]);

  return { throttle, progressMessage };
}

function App() {
  // The owner and repo to query; we're going to default
  // to using DefinitelyTyped as an example repo as it
  // is one of the most contributed to repos on GitHub
  const [owner, setOwner] = useState("DefinitelyTyped");
  const [repo, setRepo] = useState("DefinitelyTyped");
  const handleOwnerChange = useCallback(
    (event: React.ChangeEvent<HTMLInputElement>) =>
      setOwner(event.target.value),
    [setOwner]
  );
  const handleRepoChange = useCallback(
    (event: React.ChangeEvent<HTMLInputElement>) => setRepo(event.target.value),
    [setRepo]
  );

  const contributorsUrl = `https://api.github.com/repos/${owner}/${repo}/contributors`;

  const [contributorsUrlToLoad, setUrlToLoad] = useState("");
  const { progressMessage, throttle } = useContributors(contributorsUrlToLoad);

  const bloggers = useMemo(
    () => throttle.values.filter((contributor) => contributor.blog),
    [throttle]
  );

  return (
    <div className="App">
      <header className="App-header">
        <h1>Blogging devs</h1>

        <p>
          Show me the{" "}
          <a
            className="App-link"
            href={contributorsUrl}
            target="_blank"
            rel="noopener noreferrer"
          >
            contributors for {owner}/{repo}
          </a>{" "}
          who have blogs.
        </p>

        <div className="App-labelinput">
          <label htmlFor="owner">GitHub Owner</label>
          <input
            id="owner"
            type="text"
            value={owner}
            onChange={handleOwnerChange}
          />
          <label htmlFor="repo">GitHub Repo</label>
          <input
            id="repo"
            type="text"
            value={repo}
            onChange={handleRepoChange}
          />
        </div>

        <button
          className="App-button"
          onClick={(e) => setUrlToLoad(contributorsUrl)}
        >
          Load bloggers from GitHub
        </button>

        {progressMessage && (
          <div className="App-progress">{progressMessage}</div>
        )}

        {throttle.percentageLoaded > 0 && (
          <>
            <h3>Behold {bloggers.length} bloggers:</h3>
            <div className="App-results">
              {bloggers.map((blogger) => (
                <div key={blogger.name}>
                  <div>{blogger.name}</div>
                  <a
                    className="App-link"
                    href={blogger.blog}
                    target="_blank"
                    rel="noopener noreferrer"
                  >
                    {blogger.blog}
                  </a>
                </div>
              ))}
            </div>
          </>
        )}

        {throttle.errors.length > 0 && (
          <div className="App-results">
            {throttle.errors.length} requests errored
          </div>
        )}
      </header>
    </div>
  );
}

export default App;
```

이 애플리케이션을 통해 사용자는 GitHub 프로젝트의 조직 및 저장소를 입력할 수 있습니다. 그런 다음 단추를 클릭하면 다음과 같이 됩니다.

- 기여자를 로드합니다.
- 각 기여자에 대해 개별 사용자 로드(각 기여자에 대해 개별 HTTP 요청)
- 로드 시 진행 상황 전달
- 사용자가 로드될 때 나열된 블로그를 사용하여 각 사용자의 타일을 렌더링합니다.

데모를 좀 더 명확하게 하기 위해, 우리는 인위적으로 각 요청들의 지속시간을 1초씩 늦췄습니다. 합치면 다음과 같은 모양이 됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/blogging-devs-1.gif?resize=730%2C332&ssl=1)

브라우저의 UI를 차단하지 않고 데이터를 점진적으로 로딩할 수 있는 리액트 후크를 구축하였으며, 진행상황 데이터를 제공하여 사용자에게 계속 알려드립니다.