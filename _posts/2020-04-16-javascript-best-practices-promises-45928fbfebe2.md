---
layout: post
title: "JavaScript를 통한 모범 사례"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![Image for post](https://miro.medium.com/max/8558/1*tMTv7ixeltje8jepPhhHKg.jpeg)

자바스크립트 코드를 쓰는 어떤 방법들은 다른 방법들보다 낫다.

이 기사에서는 최신 JavaScript 약속 기능을 사용하기 위한 모범 사례를 살펴보겠습니다.

# 약속 없는 비동기 코드를 약속으로 변환

우리는 약속되지 않은 비동기 코드를 `util.promisify() 방식`으로 약속으로 변환할 수 있다. 이를 통해 우리는 많은 함수들을 비동기 콜백으로 변환할 수 있다. 여기서 `err`은 오류가 있는 개체이고 `res`는 비동기 코드의 결과를 갖는다.

이에 대해 정의된 유망한 형식을 가진 모든 함수도 이 유틸리티를 사용하여 약속 양식을 반환할 수 있습니다. 약속 버전이 기호 속성인 util.promisify.custom의 값으로 저장되기만 하면 약속 버전을 얻을 수 있습니다.

예를 들어, setTimeout은 Node.js의 표준 라이브러리에 약속 기반 버전을 가지고 있으므로 다음과 같이 약속으로 변환할 수 있다.

```undefined
constutil = required util';
constleep = util.promisify(setTimeout);
(httpsc () =>
취침 대기(1000);
console.log면';
})()
```

이제 콜백(callback)을 중첩시키는 대신 원하는 시간 동안 실행을 일시 중지하는 절전 기능이 생겼다.

# 비동기 체인

옛날 방식으로 우리는 그때의 방식으로 약속을 이어간다.

예를 들어 다음과 같이 씁니다.

```undefined
약속1
.then((res) => {
//...
답례약속2
})
.then((res) => {
//...
답례 약속 3
})
.then((res) => {
//...
})
```

우리는 다음과 같이 `sync`와 `wait`로 이것을 정리할 수 있다.

```undefined
(httpsc () =>
constance1 = 약속 대기1;
//...
constance2 = wait promise2;
//...
조건 3 = 약속 대기 3;
//...
})();
```

이것은 약속 확인 값이 왼쪽의 상수 또는 변수에 할당되므로 훨씬 깨끗합니다.

# 캐치를 사용하여 오류 잡기

동기식 코드와 마찬가지로, 우리는 오류를 잡아내고 그것들을 우아하게 처리해야 합니다.

그것을 약속 코드로 하기 위해서, 우리는 `catch` 방식을 부를 수 있다. 예를 들어:

```undefined
약속1
.then((res) => {
//...
답례약속2
})
.then((res) => {
//...
답례 약속 3
})
.then((res) => {
//...
})
.vs(((으) => {
//...
})
```

sync와 wait를 사용할 경우 다음과 같이 동기 코드와 같이 try...catch를 사용합니다.

```undefined
(httpsc () =>
{}을(를) 시도하다
constance1 = 약속 대기1;
//...
constance2 = wait promise2;
//...
조건 3 = 약속 대기 3;
//...
} 캐치(ex) {
//...
}
})();
```

두 예에서 모두 catch의 코드는 첫 번째 약속이 거부될 때 실행됩니다.

# 약속의 결과에 상관없이 실행해야 하는 코드를 실행하기 위해 최종 사용

약속 사슬의 결과와 상관없이 실행되어야 할 코드를 실행해야 한다면 비동기화에는 finally(최종) 방식이나 finally(최종) 블록을 사용하고 코드를 기다려야 한다.

예를 들어 다음과 같이 `finally`에서 정리 코드를 실행할 수 있습니다.

```undefined
약속1
.then((res) => {
//...
답례약속2
})
.then((res) => {
//...
답례 약속 3
})
.then((res) => {
//...
})
.vs(((으) => {
//...
})
```

또는 다음과 같이 쓸 수 있습니다.

```undefined
(httpsc () =>
{}을(를) 시도하다
constance1 = 약속 대기1;
//...
constance2 = wait promise2;
//...
조건 3 = 약속 대기 3;
//...
} 캐치(ex) {
console.log(ex);
} 마침내 {}
//...
}
})();
```

# Promise.all()을 포함한 여러 비동기식 통화

여러 개의 관련 없는 약속을 한꺼번에 실행하려면 약속.올로기를 사용해야 한다.

예를 들어 다음과 같이 한꺼번에 실행할 수 있습니다.

```undefined
약속해. 모두.
약속1
약속2,
약속3,
])
.then((res) => {
//...
})
```

sync와 wait를 사용하면 다음과 같이 쓸 수 있다.

```undefined
(httpsc () =>
const [val1, val2, val3] = wait Promise.all([])
약속1
약속2,
약속3,
])
})();
```

두 가지 예에서 세 가지 약속의 해결된 값은 모두 `res`에 있거나 왼쪽 배열에 할당됩니다(두 번째 예에서는).

이렇게 하면 각 항목이 해결되기를 기다리지 않고 한 번에 실행됩니다.

# 요약