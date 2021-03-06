---
layout: post
title: "비동기/대기 모드로 가져오기 리팩터링"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![Image for post](https://miro.medium.com/max/11520/0*O5h71XhA7DuN5Cov)

Async/wait은 지난 몇 년간 자바스크립트에 추가된 가장 혁신적인 기능 중 하나이다. 그것은 때때로 약속하는 경향이 있는 구문론적 혼란에 대한 직관적인 대체를 제공한다.

기본적으로 Ajax 요청 등을 위해 사용할 수 있는 양호한 이전 Fetch API에는 아무런 문제가 없습니다.

기본 `페치` 요청은 설정하기가 정말 간단합니다. 다음 코드를 살펴보십시오.

# 비동기/대기 101

Async/wait는 비동기식 코드를 쓰는 비교적 새로운 방식(일명 ECMAScript 2017 JavaScript 에디션의 일부)이다. 비동기식 코드의 이전 대안은 콜백과 약속이었다. 비동기/대기(Async/wait)는 실제로 약속 위에 만들어진 구문론적인 설탕일 뿐이며, 비동기식 코드를 동기식 코드처럼 보이게 하고 동작한다. 일부에서는 비동기식 코드를 찾기가 더 어렵고 익숙해져야 할 수도 있다고 주장한다.

주목해야 할 점은 `대기` 키워드는 `sync` 기능 밖에서는 사용할 수 없다는 점이다.

## 비동기/대기 수용 이유

## 간편한 세 가지 단계로 가져오기 기능을 비동기/대기 기능으로 전환

다음은 이 예제를 위해 변환할 함수입니다.

비동기 함수를 식별하고 그 앞에 `sync`를 붙입니다.

```undefined
비동기 함수 가져오기 앨범() {
...
}
```

기능 내 여러 가지 약속을 파악하여 각각의 약속 앞에 기다림을 추가한다. 변수에 할당:

```undefined
=가 가져오기를 기다립니다(apiUrl)
constjson = wait res.jsonclass
```

나머지는 동기식으로 보이도록 리팩터링하십시오.

```undefined
비동기 함수 가져오기 앨범() {
const 응답 = 가져오기 대기(apiUrl)
constjson = wait response.jsonty
콘솔.log(json)
}
```

sync/wait를 사용하는 것은 함수 키워드를 사용하는 함수에 대해 예약되지 않습니다. 구문이 약간 다르지만 화살표 함수로도 사용할 수 있습니다.

```undefined
constfetchAlbums = 비동기() = {}
const 응답 = 가져오기 대기(apiUrl)
constjson = response.json195
콘솔.log(json)
}
```

또한 다음 예에서와 같이 비동기 기능이 Google 인증 구성에서 `passport.use` 함수에 전달된 인수 중 하나인 익명 함수가 될 수 있습니다. 여기서는 "fetch"와 함께 사용됩니다.

sync/wait로 리팩터링한 후: