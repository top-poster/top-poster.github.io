---
layout: post
title: "고급 반응 후크: 사용자 정의 재사용 후크 만들기"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/Create_Custom_Reusable_React_Hook.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/Create_Custom_Reusable_React_Hook.png?fit=730%2C487&ssl=1)

리액트 16.8.0 릴리스에 처음 도입된 리액트 훅스는 개발자가 기능 구성요소를 과충전할 수 있는 새로운 API이다. `후크`는 우리가 수업으로만 할 수 있는 기능적인 요소들로 우리가 할 수 있게 한다.

결과적으로, 우리는 클래스를 작성하지 않고도 상태 및 기타 대응 기능을 사용할 수 있다.

훅스는 도입 이후 리액트 생태계에 지진을 가져왔다. 그리고 그들은 리액트 앱의 제작 방식을 영원히 바꾸어 놓았습니다.

이 기사에서는 재사용 가능한 후크 패턴의 실제 적용에 대해 알아보겠습니다. 이러한 점을 고려할 때 후크(Hooks)는 합성 가능한 것으로서, 사용자 정의 후크(Hook) 안에 있는 후크(Hook)를 또 다른 후크(Hook)라고 부를 수 있습니다.

## 재사용 가능한 '후크' 패턴의 사용 사례

아래의 재사용 가능한 `후크` 패턴을 살펴보겠습니다.

### UseIsMounted' 후크

React에서 한 구성 요소가 `장착 해제`되면 다시는 `장착`되지 않으므로 `장착되지 않은` 구성 요소에 상태를 설정하지 않습니다. 왜냐하면 그것은 결코 재임대되지 않을 것이기 때문이다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/unmounted-example-app.png?resize=730%2C441&ssl=1)

위의 이미지에는 이 문제가 있는 작은 기여 예제 앱이 있습니다.

위의 앱에서 `Dev 구성 요소`는 `showDev 상태`가 참일 때만 렌더링됩니다. 그리고 `정지` 버튼을 클릭하면 `Show Dev state`의 값이 전환된다. 따라서 `Dev 구성 요소 마운트 해제`는 `showDev`가 거짓일 때 발생합니다.

다음은 `Dev 컴포넌트`의 구현이다.

```js
function Dev() {
  const [devProfile, setDevProfile] = useState("Fetching Dev...");
  const getDevProfile = () => {
    setTimeout(() => setDevProfile("Lawrence Eagles"), 4000);
  };
  useEffect(() => {
    getDevProfile();
  });
  return (
    <div className="mb-4 text-center">
      <p>{devProfile}</p>
    </div>
  );
}
```

위의 코드에서 구성 요소 `mounts`가 시작되면 getDevProfile 함수가 호출된다는 것을 알 수 있다. 이 작업은 4000밀리초가 소요되며 이후 devProfile 상태를 로렌스 이글스로 업데이트한다.

4000밀리초 전에 `Dev 구성 요소`(정지 버튼 클릭)를 `제거`하면 위의 이미지에 표시된 경고 오류가 표시됩니다.

이 오류로 인해 UI가 손상되지는 않지만 메모리 누수가 발생하여 성능이 저하되는 것으로 알려져 있습니다.

이 문제를 방지하기 위해 일부 개발자는 다음과 같이 수행합니다.

```coffeescript
if (this.isMounted()) { // This is bad.
  this.setState({...});
}
```

그러나 대응팀은 isMounted 기능을 사용하는 것을 반대 패턴으로 간주하고 있으므로 마운트 상태를 직접 추적할 것을 권장합니다.

```js
  useEffect(() => {
    let isMounted = true; // sets mounted flag true
    return () => {
      // simulate an api call and update state here
      isMounted = false;
    }; // use effect cleanup to set flag false, if unmounted
  }, []);
  return isMounted;
};
```

우리의 목표는 위의 논리를 코드에 재사용할 수 있는 사용자 정의 후크(Hook)로 추상화하여 코드 "DREY"를 유지하는 것이다.

이렇게 하려면 위의 모든 보일러 플레이트 코드를 아래 그림과 같이 사용자 정의 `후크`(`IsMounted Hook` 사용)로 캡슐화하십시오.

```js
import { useEffect, useState } from "react";
const useIsMounted = () => {
  const [isMounted, setIsMouted] = useState(false);
  useEffect(() => {
    setIsMouted(true);
    return () => setIsMouted(false);
  }, []);
  return isMounted;
};
export default useIsMounted;
```

이제 앱에서 다음과 같이 사용할 수 있습니다.

```js
function Dev() {
  const isMounted = useIsMounted();
  const [devProfile, setDevProfile] = useState("Fetching Dev...");
  useEffect(() => {
    function getDevProfile() {
      setTimeout(() => {
        if (isMounted) setDevProfile("Lawrence Eagles");
      }, 4000);
    }
    getDevProfile();
  });
  return (
    <div className="mb-4 text-center">
      <p>{devProfile}</p>
    </div>
  );
}
```

위의 코드는 구성 요소가 여전히 `마운트` 상태일 때만 상태가 업데이트되도록 보장합니다.

### 사용 로드 후크

구성 요소가 마운트되면 로드되는 리소스에 연결되는 버튼이 여러 개 있는 시나리오에서 시간을 절약할 수 있는 잘 생각해 둔 재사용 가능한 후크입니다.

일반적으로 `로딩`이 있습니다.자원을 얻기 위해 `call call`이 실행될 때 일종의 스피너(spinner.

문제는 이러한 버튼의 수가 리소스 증가에 따라 증가하여 구성 요소가 서로 다른 `로드 상태`로 혼돈될 수 있다는 것이다.

다음 코드를 고려하십시오.

```js
import "./styles.css";
import React, { useState, useEffect } from "react";
export default function App() {
  const delay = (ms) => new Promise((resolve) => setTimeout(resolve, ms));
  const [isLoadingDev, setIsLoadingDev] = useState(true);
  const [isLoadingStack, setIsLoadingStack] = useState(true);
  const fetchDevs = async () => {
    console.log("this might take some time....");
    await delay(4000);
    setIsLoadingDev(false);
    console.log("Done!");
  };
  const fetchStacks = async () => {
    console.log("this might take some time....");
    await delay(5000);
    setIsLoadingStack(false);
    console.log("Done!");
  };
  useEffect(() => {
    fetchDevs();
    fetchStacks();
  }, []);

  return (
    <div className="app 
     container 
     d-flex 
     flex-column 
     justify-content-center 
     align-items-center"
    >
      <article className="d-flex flex-column my-2">
        <p className="text-center">Welcome to Dev Hub</p>
      </article>
      <article className="d-flex flex-column">
        <button className="m-2 p-3 btn btn-success btn-sm">
          {isLoadingDev ? "Loading Devs..." : "View Devs"}
        </button>
        <button className="m-2 p-3 btn btn-success btn-sm">
          {isLoadingStack ? "Loading Stacks..." : "View Stacks"}
        </button>
      </article>
    </div>
  )
}
```

위의 코드에서 fetchDev와 fetchStack은 sync request를 시뮬레이션하기 위해 고안되었다. 구성 요소가 `마운트`되면 해당 구성 요소가 호출되고 해당 기능이 완료되면 버튼의 메시지가 변경됩니다.

각 버튼의 로드 상태는 사용 상태 초기화에 의해 처리되며 더 많은 리소스를 로드할수록 숫자가 증가합니다.

이 코드는 DRY가 아니며 반복은 버그를 위한 레시피입니다.

우리는 다음과 같이 재사용 가능한 `사용 후크`를 만들어 위의 코드를 리팩터링할 수 있다.

```coffeescript
import { useState } from "react";
const useLoading = (action) => {
  const [loading, setLoading] = useState(false);
  const doAction = (...args) => {
    setLoading(true);
    return action(...args).finally(() => setLoading(false));
  };
  return [doAction, loading];
};
export default useLoading;
```

이 후크는 `sync 함수`를 사용하고 해당 함수와 `부하 상태`를 포함하는 배열을 반환합니다.

우리는 또한 우리의 `사용국가` 논리를 이 구성요소에 추상화할 수 있었고 우리는 단 하나의 초기화만 필요하다.

코드에서 사용할 수 있는 방법은 다음과 같습니다.

```js
import "./styles.css";
import React, { useEffect } from "react";
import useLoading from "./useLoading";
export default function App() {
  const delay = (ms) => new Promise((resolve) => setTimeout(resolve, ms));
  const fetchDevs = async () => {
    console.log("this might take some time....");
    await delay(4000);
    console.log("Done!");
  };
  const fetchStacks = async () => {
    console.log("this might take some time....");
    await delay(5000);
    console.log("Done!");
  };
  const [getDev, isLoadingDev] = useLoading(fetchDevs);
  const [getStacks, isLoadingStack] = useLoading(fetchStacks);
  useEffect(() => {
    getDev();
    getStacks();
  }, []);
  return (
    <div className="app 
         container 
         d-flex 
         flex-column 
         justify-content-center 
         align-items-center"
    >
      <article className="d-flex flex-column my-2">
        <p className="text-center">Welcome to Dev Hub</p>
      </article>
      <article className="d-flex flex-column">
        <button className="m-2 p-3 btn btn-success btn-sm">
          {isLoadingDev ? "Loading Devs..." : `View Devs`}
        </button>
        <button className="m-2 p-3 btn btn-success btn-sm">
          {isLoadingStack ? "Loading Stacks..." : "View Stacks"}
        </button>
      </article>
    </div>
  );
}
```

여기서 우리는 `어레이 파괴`를 사용하여 `신호 작용 기능`과 `부하 상태`를 얻었다.

```cpp
  const [getDev, isLoadingDev] = useLoading(fetchDevs);
  const [getStacks, isLoadingStack] = useLoading(fetchStacks);
```

그런 다음 이러한 기능은 use Effect Hook과 뷰에서 사용됩니다. 그 결과 `드라이(DREY)` 코드와 유지 관리가 용이한 클린 코드가 생성됩니다.

또한 모든 `로드 상태`가 완료된 후에는 구성요소 렌더링, 업데이트 상태 등의 작업을 쉽게 수행할 수 있습니다.

```undefined
if(isLoadingDev && isLoadingStack) {
  // do somthing
}

return {
  // normal component view
}
```

## 결론

이 강연을 마친 후에는 코드 `드라이(DREY)`를 유지해야 한다는 점을 인정하고 재사용 가능한 사용자 정의 후크(Hooks)를 작성할 준비가 되었으면 합니다.

이는 재사용 가능한 사용자 정의 후크를 만드는 고급 패턴의 두 가지 예에 불과하며, 이제 자신만의 고급 패턴을 만들 수 있기를 바랍니다.

여기서 여러분만의 훅을 만드는 것에 대해 더 많이 읽을 수 있습니다.