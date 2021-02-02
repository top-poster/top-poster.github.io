---
layout: post
title: "uvu 사용 방법 : 빠르고 가벼운 테스트 실행기
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/02/uvu.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/uvu.png?fit=730%2C487&ssl=1)

uvu (최종 속도의 줄임말, unleashed)는 Node.js 및 브라우저를위한 가장 빠르고 가벼운 테스트 실행기 중 하나로 간주됩니다.
 주요 기능에는 개별 테스트 파일 실행, 비동기 테스트 지원, 네이티브 ES 모듈 지원, 브라우저와의 호환성, 뛰어난 경량 크기, 친숙한 API 및 놀라운 성능이 포함됩니다.
 이 블로그 게시물에서는 uvu의 사용, Jest 및 AVA라는 다른 두 개의 인기있는 테스트 실행기 라이브러리와 비교, 테스트에 사용하는 이유와시기에 대해 설명합니다.


## 왜 uvu를 사용합니까?


우선, uvu는 일부 테스트 라이브러리가 지원하는 일반적인 이점 중 하나 인 비동기 테스트를 지원합니다.
 테스트중인 코드가 다음 테스트로 이동하기 전에 테스트 프로세스를 완료했는지 확인하는 데 도움이됩니다.
 비동기 (비동기) 함수의 주요 목적은 약속 기반 API를 사용하는 데 필수적인 구문을 명확히하는 것입니다.
 비동기 테스트에서는 테스트 프로세스의 완료를 결정하는 `콜백`또는 `프로 미스`와 같은 메소드가 사용됩니다.


또 다른 주요 기능은 브라우저 호환성입니다.
 처음에는 uvu가 브라우저와 호환되지 않는 문제 였지만 프로세스 파일을 약간 수정하여 해결했습니다.
 여기에서 문제 해결에 관한 토론을 찾을 수 있습니다.
 따라서 브라우저 호환성에 문제가있는 경우에도이 링크를 확인하여 문제를 더 잘 이해하고 분류 할 수 있습니다.


## uvu 사용


uvu 사용은 간단하며 다음과 같이 작동합니다.


```coffeescript
// tests/demo.js
// Source: https://github.com/lukeed/uvu

import { test } from 'uvu';
import * as assert from 'uvu/assert';

test('Math.sqrt()', () => {
  assert.is(Math.sqrt(4), 2);
  assert.is(Math.sqrt(144), 12);
  assert.is(Math.sqrt(2), Math.SQRT2);
});

test('JSON', () => {
  const input = {
    foo: 'hello',
    bar: 'world'
  };
  const output = JSON.stringify(input);
  assert.snapshot(output, `{"foo":"hello","bar":"world"}`);
  assert.equal(JSON.parse(output), input, 'matches original');
});

test.run();
```

이제해야 할 일은이 테스트 파일을 실행하는 것입니다.


```shell
# via `uvu` cli, for all `/tests/**` files
$ uvu -r esm tests

# via `node` directly, for file isolation
$ node -r esm tests/demo.js
```

위의 명령 줄에서 주목해야 할 점은 Ecmascript (ES) 모듈이 Node.js 버전> 12.x에 배치되기 때문에`–r esm`은 레거시 Node.js 모듈에만 지정된다는 것입니다.
 기본적으로`.js` 및`.cjs` 파일은 Common.js로 처리되며`.mjs` 파일 확장자는 Ecmascript 모듈 (ESM)로 제공되는 파일입니다.


위의 예는 Node.js가 ES 모듈을로드하고 필요할 때 모듈을 가져 오도록 허용하는 가장 간단한 방법으로 간주 할 수 있습니다.
 또한 아래에 표시된 다른 방법으로 모듈을로드 할 수도 있습니다.


Node.js가 ES 모듈을로드하는 다른 방법도 있습니다.
 이러한 방법에는 유형, 모듈 및 esm 패키지 절차가 포함됩니다.
 다음은 이러한 방법에 대한 전체 가이드입니다.


- esm 패키지 https://github.com/lukeed/uvu/tree/master/examples/esm.loader

- 유형 모듈 https://github.com/lukeed/uvu/tree/master/examples/esm.dual


## 메인 uvu 모듈


기본 uvu 모듈은 모든 uvu 테스트에 필요한 테스트 또는 테스트 슈트 (코드의 특정 기능과 관련된 일련의 개별 테스트)와 관련하여 도움을줍니다.
 사용자는 여기서`uvu.test` 또는`uvu.suite`를 선택할 수 있습니다.
 `uvu.suite`를 통해 한 번에 여러 파일을 테스트하는 것과 같은 수많은 추가 특전을 파악할 수 있지만 단일 파일 만 테스트하려는 경우에는`uvu.test`를 선택해야합니다 (기술적으로`uvu.test`는 이름이 지정되지 않은 테스트 스위트입니다).


## `uvu.suite (이름 : 문자열, 컨텍스트? : T)`


동일한 파일에 원하는만큼의 모음을 포함 할 수 있지만 uvu의 대기열에 추가 할 각 모음에 대해 모음을`run `으로 호출해야합니다.
 이것은 새로운 스위트 생성과 함께 스위트를 반환합니다.
 여기에서 이름은 제품군의 이름에 해당하며 문자열 유형입니다.
 이렇게하면 모든 콘솔 출력이 결합되고 실패한 테스트의 이름이 접미사로 추가됩니다.
 Suite의 컨텍스트에는 기본값으로 빈 개체가 있으며 모든 유형입니다.
 이것은 스위트 내의 모든 테스트 블록과 후크로 전달됩니다.


## `uvu.test (이름 : 문자열, 콜백 : 함수)`


하나의 파일 만 테스트해야하는 경우이`uvu.test`를 가져올 수 있습니다.
 여기서 이름은 분명히 테스트의 이름을 나타내며 문자열 유형이며 여기서 콜백은 테스트 코드로 구성되며`promise <any>`또는`function <any>`유형입니다.
 콜백은 비동기적일 수 있으며 포기하더라도 값을 반환합니다.


## 행동 양식


### 생성


각각의 모든 모음은이`suite (name, callback)`처럼 호출 될 수 있습니다.


### 달리는


스위트를 실행하려면 uvu 테스트 큐에 스위트를 추가하고`suite.run () `을 사용해야합니다.


### 건너 뛰는 중


스위트를 건너 뛰면`suite.skip (name, callback) `으로 전체 테스트 블록을 누락하는 데 도움이 될 수 있습니다.


## 추가 방법


환경을 구성하거나 픽스처를 설정하는 경우 이상적인 경우는 `suite.before (callback)`방식으로 소송 시작 전에 주어진 콜백을 요청하는 것입니다.


또한 환경이나 픽스쳐를 마무리하기위한 이상적인 경우는 `suite.after (callback)`방식으로 스위트 완료 후 콜백을 요청하는 것입니다.


다음은 위 설명의 샘플 코드입니다.


```coffeescript
import { suite } from 'uvu';
import * as assert from 'uvu/assert';
import * as dates from '../src/dates';

const Now = suite('Date.now()');

let _Date;
Now.before(() => {
  let count = 0;
  _Date = global.Date;
  global.Date = { now: () => 100 + count++ };
});

Now.after(() => {
  global.Date = _Date;
});

// this is not run (skip)
Now.skip('should be a function', () => {
  assert.type(Date.now, 'function');
});

// this is not run (only)
Now('should return a number', () => {
  assert.type(Date.now(), 'number');
});

// this is run (only)
Now.only('should progress with time', () => {
  assert.is(Date.now(), 100);
  assert.is(Date.now(), 101);
  assert.is(Date.now(), 102);
});

Now.run();
```

## uvu가 Jest 및 AVA보다 나은 이유


먼저 테스트 러너의 시간을 비교해 보겠습니다.
 다음은 두 가지 타이밍을 가진 소수의 테스트 실행자가 실행 한 샘플 테스트 (여기에있는 샘플 코드를 테스트하여 달성)의 결과입니다.
 첫 번째 값은 전체 프로세스의 총 실행 시간이고 다른 값은 알려진 경우에만 자체보고 된 성능 시간입니다.


```bash
~> "ava"   took   594ms ( ???  )
~> "jest"   took   962ms (356 ms)
~> "mocha" took   209ms (4 ms)
~> "tape"   took   122ms (  ???  )
~> "uvu"   took    72ms (1.3ms)
```

위의 결과에서 uvu가 경쟁사 중에서 가장 빠른 옵션임을 알 수 있습니다.


이제 기능 비교에 대해서도 조금 이야기하겠습니다.


- AVA와 uvu는 모두 비동기 테스트를 제공하지만 Jest는 제공하지 않습니다.

- Jest 및 uvu를 사용하면 다른 앱에 매우 쉽게 통합 할 수 있지만 AVA는 최소한의 테스트 라이브러리이므로 다른 두 앱과 같은 통합을 제공하지 않습니다.

- 간단한 API 만 포함하는 AVA는 모의 지원을위한 추가 라이브러리를 설치해야하며 Jest 및 uvu는 사용자가 다양한 기능을 지원하기 위해 추가 라이브러리를 포함 할 필요가없는 광범위한 API를 가지고 있습니다.


## 결론


테스트 러너의 성능에 대한 우려는 항상 있었지만 uvu가 제공 한 기능은 최고의 기능 중 하나임이 입증되었습니다.
 브라우저 호환성, 고속 테스트, 네이티브 ES 모듈 지원, 비동기 테스트, 단일 라이브러리에서 개별적으로 테스트 파일 실행에 대해 걱정하는 모든 사람을위한 올인원 라이브러리와 같습니다.
 따라서 이러한 모든 것에 대해 염려 할 때마다 하나의 솔루션으로 전환하면됩니다. 이것이 바로 uvu입니다.
