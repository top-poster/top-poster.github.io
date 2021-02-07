---
layout: post
title: "Node.js 15: 새로운 기능 및 개발자 환경이 어떻게 향상되었음"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/nodejs15-updates.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/nodejs15-updates.png?fit=730%2C487&ssl=1)

이제 Node.js의 다음 주요 릴리스를 사용할 수 있습니다. 지금까지 Node.js 15 릴리스에서는 몇 가지 개선 사항, 몇 가지 새로운 JavaScript 언어 기능, 심지어 몇 가지 변경 사항도 확인했습니다.

NPM의 새 버전과 같은 이러한 개선 사항 중 일부는 상당한 수준이며 개발자의 경험을 획기적으로 개선합니다. 새로운 버전의 N-API나 QUIC 프로토콜에 대한 실험적인 지원과 같은 다른 변화들은 대중들에게 어필하지 않을 수 있지만 Node.js가 계속해서 확장 가능하고 웹의 미래에 대비하도록 보장하는 데 중요하다.

무엇이 새로운 것인지, 왜 당신이 관심을 가져야 하는지 살펴봅시다.

> 팁: Node.js 15는 LTS(장기 지원) 릴리스가 아닙니다. 이 릴리스에 대한 지원은 첫 번째 Node.js 16 릴리스가 나온 직후인 2021년 6월에 종료될 예정이다. 당분간 운영 애플리케이션을 Node.js 14에 유지하는 것을 고려해 보는 것이 좋습니다.

## NPM 7

다양한 새로운 기능 덕분에 NPM 7의 포함은 이번 릴리스의 최대 헤드라이너일 것이다. 가장 큰 기능은 NPM 워크스페이스로, 단일 파일 시스템에서 여러 NPM 패키지를 생성하고 관리할 수 있는 기본 지원의 시작입니다. 이미 Warn 패키지 관리자 또는 Lerna를 적극적으로 사용하고 있는 개발자는 NPM의 작업 공간 구현과 유사성을 확인해야 합니다.

NPM 워크스페이스로 시작하는 것은 비교적 쉽다. 먼저 파일 시스템에 모든 NPM 패키지 폴더를 정렬하고 최상위 패키지를 생성합니다.json은 이러한 각 폴더를 참조하고 모든 NPM 패키지에서 명령을 실행하기 시작합니다.

이것이 NPM에서 워크스페이스의 초기 구현이라는 것을 기억하라. 숙련된 Warn 또는 Lerna 사용자인 경우 많은 테이블 테이크 기능이 여전히 누락되어 있음을 알 수 있습니다. 예를 들어, 작업영역의 특정 패키지 내에서 스크립트를 실행할 수 있는 것은 NPM CLI를 사용할 수 없습니다. 작업 공간 기능이 빠르게 확장될 것으로 확신합니다.

> 팁: '--prefix' 인수를 사용하여 작업영역의 특정 패키지에서 스크립트를 실행할 수 있습니다. 예를 들어 npm run --' prefix recipe-generator test는 현재 작업 공간에서 recipe-generator 패키지의 test 스크립트를 실행한다.

작업 공간 외에도 NPM 7에 도입된 몇 가지 주요 변경 사항이 있습니다.

피어 종속성은 NPM 6에서 기본적으로 무시되었습니다. NPM 7에서는 기본적으로 설치됩니다. 이 변경에 대한 자세한 내용은 NPM RFC0025에서 확인할 수 있습니다. 흥미롭게도, 리액트 개발자 커뮤니티가 이러한 변화에 영향을 미쳤을 수도 있습니다.

NPM 패키지를 먼저 설치하지 않고 실행하는 것은 npm Exec 명령을 통해 핵심 NPM의 일부가 되었습니다. 이 새 명령은 npx를 대체하지만 동작도 약간 다르다. npx는 npm exec을 사용하기 위해 다시 작성되었지만 이전 버전과의 호환성을 위해 동일한 CLI를 사용했다. 이에 대한 자세한 내용은 NPM 설명서를 참조하십시오.

NPM은 이제 arn.lock 파일을 지원합니다. 이 파일이 있으면 NPM이 패키지를 가져올 위치를 결정하는 데 사용됩니다. NPM은 패키지가 추가되거나 제거될 때 이를 업데이트합니다. 패키지-락.json을 yarn.lock으로 교체할 계획이 없습니다. 실제로 package-lock.json 파일은 여전히 패키지 버전 메타데이터의 권한 있는 원본으로 생성되고 유지됩니다.

NPM에는 이제 결정론적 빌드를 지원하는 새로운 package-lock.json 형식이 있습니다. 간단히 말해, package-lock.json 파일이 변경되지 않은 경우 NPM은 빌드에서 설치된 패키지의 버전이 동일하게 유지되도록 보장합니다. 결정론적 구조는 꽤 오랫동안 Yarn의 특징이었다.

NPM 7의 표면만 긁어내는데, NPM 7의 새로운 기능에 대한 자세한 내용은 여기를 참조하십시오.

## V88.6

Node.js 15는 8.4에서 8.6까지 사용하는 V8 엔진의 버전을 범프합니다. V8은 Node.js가 맨 위에서 실행하는 기본 자바스크립트 엔진입니다.

그런데 왜 신경써야 하죠? V8은 개발자가 사용할 수 있는 JavaScript 언어 기능을 정의하기 때문입니다. 이 버전 범프는 작지만 써야 하는 코드의 양을 줄이고 코드를 더 읽기 쉽게 만드는 몇 가지 새로운 언어 기능이 있습니다.

먼저 변수에 값을 조건부로 할당할 수 있는 몇 가지 새로운 논리적 할당 연산자가 있습니다.

```js
// new logical AND operator
let a = 1;
let b = 0;
a &&= 2;
b &&= 2;
console.log(a); // prints 2
console.log(b); // prints 0

// new logical OR operator
a = 1;
b = null;
a ||= 3;
b ||= 3;
console.log(a); // prints 1
console.log(b); // prints 3

// new logical nullish operator
a = 1;
b = null;
a ??= 4;
b ??= 4; 
console.log(a); // prints 1
console.log(b); // prints 4
```

약속의 대상은 하나 이상의 약속을 받아들이고 먼저 해결되는 약속을 되돌려주는 새로운 임의() 기능이 있다. 거부된 약속은 무시됩니다. 모든 약속이 거부되면 거부된 모든 약속의 오류를 포함하는 새로운 집계 오류가 반환된다.

많은 개발자들은 그들의 뇌에 대한 정규 표현 구문을 가지고 있지 않다. 다행히도 문자열 개체에는 새로운 replace All() 기능이 추가되어 문자열의 모든 발생을 대체하기 위해 조회해야 하는 정규식이 하나 더 적습니다.

myString.replace All(regex)=기능(function)=마이스 스트링 대체 올(myString.replace all)은 간단하다.

## QUIC 프로토콜에 대한 실험적 지원(HTTP/3)

HTTP/1 표준은 1996년 즈음에 나타났다. 약 19년 후와 많은 원본 표준 확장 이후, 2015년에 HTTP/2가 표준이 되었다.

이제 우리는 TCP에서 QUIC로 네트워크 전송 계층 프로토콜을 대체하게 될 HTTP/3에 이어 뜨거운 관심을 보이고 있다. Node.js는 QUIC 프로토콜에 대한 실험적인 지원을 추가하여 최신 표준보다 앞서 나가겠다는 개발자 커뮤니티에 대한 약속을 계속 보여주고 있다.

표준은 훨씬 더 빠른 속도로 발전하고 있습니다. 하지만 이것이 주류 Node.js 개발자에게 어떤 의미일까요? 글쎄요, 여러분이 무엇을 짓느냐에 따라 큰 의미가 있을 수 있어요.

응용 프로그램에서 소켓을 만드는 데 `net` 모듈을 사용하는 경우 새로운 실험 기능 및 객체를 활용하여 QUIC 프로토콜을 사용하여 연결을 설정할 수 있습니다. QUIC는 TCP에 비해 지연 시간을 줄일 수 있으며 TLS 암호화 기능이 내장되어 있습니다.

## 거부된 약속 처리되지 않은 약속

Node.js 15에서 가장 큰 변화는 거부된 약속이 처리되지 않은 상태로 처리되는 방법이다. Node.js 14에서 거부된 약속을 명시적으로 처리하지 않은 경우 다음과 같은 경고가 표시됩니다.

```undefined
(node:764) UnhandledPromiseRejectionWarning: something happened
(Use `node --trace-warnings ...` to show where the warning was created)
(node:764) UnhandledPromiseRejectionWarning: Unhandled promise rejection. This error originated either by throwing inside of an async function without a catch block, or by rejecting a promise which was not handled with .catch(). To terminate the node process on unhandled promise rejection, use the CLI flag `--unhandled-rejections=strict` (see https://nodejs.org/api/cli.html#cli_unhandled_rejections_mode). (rejection id: 2)
(node:764) [DEP0018] DeprecationWarning: Unhandled promise rejections are deprecated. In the future, promise rejections that are not handled will terminate the Node.js process with a non-zero exit code.
```

이제 Node.js 15에서 경고가 다음 오류로 대체되었습니다.

```js
node:internal/process/promises:218
          triggerUncaughtException(err, true /* fromPromise */);
          ^

[UnhandledPromiseRejection: This error originated either by throwing inside of an async function without a catch block, or by rejecting a promise which was not handled with .catch(). The promise rejected with the reason "recipe 
could not be generated".] {
  code: 'ERR_UNHANDLED_REJECTION'
}
```

이러한 변화는 특히 과거에 경고 메시지를 무시한 개발자들에게 많은 애플리케이션에 영향을 미칠 수 있는 가능성을 가지고 있다. 새로운 `--unhandle-disons=filename` 인수를 사용하여 Node.js를 시작하여 이 동작을 되돌릴 수 있습니다.

## 실험 진단 채널 모듈

Node.js 15.1.0은 `diagnostics_channel`이라는 새로운 실험 모듈을 도입한다. 이 모듈은 기본적으로 개발자가 임의 데이터를 다른 모듈이나 애플리케이션이 소비할 수 있는 채널에 게시하는 데 사용할 수 있는 게시-구독 패턴을 가능하게 한다. 이 모듈은 의도적으로 일반적이며 다양한 방법으로 사용될 수 있습니다.

모듈을 즉시 가져오고 사용할 수 있습니다.

```js
const diagnostics_channel = require('diagnostics_channel');
```

채널을 만들고 채널에 메시지를 게시하는 것은 사실 매우 간단합니다. 다음 예에서는 채널을 만들고 채널에 메시지를 게시하는 방법을 시연합니다.

```js
const getData = filter => {
    const alertChannel = diagnostics_channel.channel('my-module.alert');

    return new Promise((resolve, reject) => {
        const data = doSomething(filter);
        if (data) {
            resolve(data);
        } else {
            const errorMessage = "data could not be retrieved";
            alertChannel.publish(errorMessage);
            reject(errorMessage);
        }
    });
}
```

채널의 메시지 소비는 그만큼 쉽습니다. 다음은 다른 모듈이나 응용 프로그램에서 채널을 구독하고 채널에서 수신한 데이터를 활용하는 방법에 대한 예입니다.

```js
  const alertChannel = diagnostics_channel.channel('my-module.alert');
  alertChannel.subscribe(message => {
    console.log(`Alert received on my-module.alert channel: ${message}`);
  });
```

## 기타 주목할 만한 변경사항

지금까지 살펴본 것과 비교할 때 많은 개발자에게 영향을 미치지 않을 것 같은 몇 가지 주목할 만한 변화가 릴리스에 있습니다.

- N-API 7이 여기 있습니다. N-API는 개발자가 Node.js를 위한 네이티브 애드온을 구축할 수 있는 여러 가지 방법 중 하나이다. N-API 버전은 Node.js 6까지 거슬러 올라간다. N-API는 추가 기능이 이전, 현재 및 미래의 Node.js LTS 릴리스와 호환되도록 보장합니다.
- 사용되지 않는 `노드 디버그` 명령이 제거되었습니다. 널리 채택된 검사자 기반 접근 방식을 사용하여 Node.js를 디버깅하는 `Node inspect`로 대체된다.
- repl 모듈에서 사용되지 않는 몇 가지 기능이 제거되었습니다. 사용되지 않는 기능의 제거는 놀랄 일이 아니지만 응용 프로그램에서 `repl` 모듈을 사용하는 경우 릴리스 노트를 검토할 가치가 있을 수 있습니다.

## 다음에 할 일이 뭐죠?

2021년 4월까지 Node.js 15 마이너 및 패치 릴리스가 더 늘어날 것으로 예상됩니다. 그 후, 우리는 다음 LTS 릴리스가 될 Node.js 16의 처음 몇 번의 반복을 보기 시작할 것이다.