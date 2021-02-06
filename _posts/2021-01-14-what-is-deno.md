---
layout: post
title: "Deno는 무엇이며 Node.js와 어떻게 다릅니까?"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2019/07/deno-vs-nodejs.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2019/07/deno-vs-nodejs.png?fit=730%2C487&ssl=1)

편집자 참고: 데노 대. Node.js 비교 게시물은 가장 많은 릴리스인 Deno v1.7.0에 도입된 업데이트를 반영하기 위해 2021년 1월에 마지막으로 업데이트되었다.

Node.js의 크리에이터 라이언 달은 2018년 5월 데노를 공식 출시했다. 자바스크립트를 위한 이 새로운 런타임은 Node.js에 내재된 문제의 긴 목록을 수정하기 위해 고안되었다.

오해하지 마세요. 노드는 자체적으로 훌륭한 서버측 JavaScript 런타임입니다. 대부분 방대한 에코시스템과 JavaScript 사용 때문입니다. 그러나 Dahl은 2018년 EU JSconf에서 보안, 모듈, 의존성 등 몇 가지를 언급하기 위해 좀 더 생각해봤어야 했다고 인정했다.

## Deno가 뭐죠?

Deno는 자바스크립트용 구글 런타임 엔진인 V8을 기반으로 구축된 보안 타입스크립트 런타임이다.

Deno는 다음과 같이 제작되었습니다.

- Rust(Deno의 코어는 Rust로 작성되었으며, Node는 C++로 작성됨)
- Tokio(Rust로 작성된 이벤트 루프)
- TypeScript(Deno는 즉시 JavaScript와 TypeScript를 모두 지원함)
- V8(Google의 JavaScript 런타임은 Chrome 및 Node에서 사용됨)

## 왜 Deno를 사용하죠?

Deno의 기능은 Node.js의 기능을 향상시키도록 설계되었다. 이제 Deno를 Node의 강력한 대안으로 만드는 몇 가지 주요 기능에 대해 자세히 살펴보겠습니다.

### Deno의 보안(권한)

데노의 가장 중요한 특징 중 하나는 보안에 초점을 맞춘다는 것이다.

Node.js와 반대로 Deno는 기본적으로 샌드박스에서 코드를 실행하는데, 이는 런타임이 다음 항목에 액세스할 수 없음을 의미합니다.

- 파일 시스템
- 네트워크
- 다른 스크립트 실행
- 환경변수

권한 시스템이 어떻게 작동하는지 살펴보겠습니다.

```undefined
(async () => {
 const encoder = new TextEncoder();
 const data = encoder.encode('Hello world\n');

 await Deno.writeFile('hello.txt', data);
 await Deno.writeFile('hello2.txt', data);
})();
```

이 스크립트는 hello라는 두 개의 텍스트 파일을 만듭니다.txt와 hello2.hello world 메시지가 포함된 txt. 샌드박스 내에서 코드가 실행 중이어서 파일 시스템에 액세스할 수 없습니다.

또한 Node.js에서처럼 FS 모듈 대신 Deno 네임스페이스를 사용하고 있습니다. Deno 네임스페이스는 많은 기본 도우미 기능을 제공합니다. 네임스페이스를 사용함으로써 브라우저 호환성이 상실되고 있으며, 이는 나중에 논의될 것입니다.

실행해서 실행할 때:

```css
deno run write-hello.ts
```

다음 메시지가 표시됩니다.

```undefined
⚠️Deno requests write access to "/Users/user/folder/hello.txt". Grant? [a/y/n/d (a = allow always, y = allow once, n = deny once, d = deny always)]
```

샌드박스의 각 통화는 허가를 요청해야 하기 때문에 우리는 실제로 두 번 메시지를 받는다. 물론, 만약 우리가 `항상 허용` 옵션을 선택한다면, 우리는 딱 한 번만 질문을 받을 것이다.

거부 옵션을 선택하면 권한 거부 오류가 발생하고 오류 처리 논리가 없어 프로세스가 종료됩니다.

다음 명령으로 스크립트를 실행하는 경우:

```undefined
deno run --allow-write write-hello.ts
```

프롬프트가 없고 두 파일이 모두 생성됩니다.

파일 시스템의 --allow-write 플래그 외에도 네트워크 요청, 환경 액세스 및 하위 프로세스 실행을 위해 각각 "--allow-net", "--allow-env", "--allow-run" 플래그가 있다.

### 데노 모듈

Deno는 브라우저와 마찬가지로 URL별로 모듈을 로드합니다. 서버 측에 URL이 있는 수입명세서를 보고 많은 사람들이 처음에는 혼란스러워했지만, 실제로는 이해가 됩니다. 참고하세요:

```coffeescript
import { assertEquals } from "https://deno.land/std/testing/asserts.ts";
```

URL로 패키지를 가져오는 데 무슨 문제가 있나요? 답은 간단하다. URL을 사용하면 최근 문제가 많이 발생한 npm 등 중앙집중식 레지스트리 없이 데노 패키지를 배포할 수 있다.

URL을 통해 코드를 가져오면 패키지 작성자가 적합한 위치에 코드를 호스팅할 수 있습니다. 즉, 최상의 분산 기능을 제공합니다. 더 이상 `패키지`는 없습니다.json 및 `node_contract`입니다.

응용 프로그램을 시작하면 Deno가 가져온 모든 모듈을 다운로드하고 캐시합니다. 일단 캐시되면, Deno는 우리가 특별히 "--reload" 플래그로 요청하기 전까지는 다시 다운로드하지 않을 것입니다.

여기서 몇 가지 중요한 질문을 할 수 있습니다.

#### 웹사이트가 다운되면 어쩌지?

중앙 집중식 레지스트리가 아니기 때문에 모듈을 호스팅하는 웹 사이트는 여러 가지 이유로 다운될 수 있습니다. 개발 중 또는 생산 중 가동되는 상태에 따라 위험 부담이 더욱 커집니다.

앞에서 언급했듯이, Deno는 다운로드된 모듈을 캐시합니다. 캐시가 로컬 디스크에 저장되므로 Deno의 작성자는 버전 제어 시스템(예: git)에서 캐쉬를 확인하여 저장소에 보관할 것을 권장합니다. 이렇게 하면 웹 사이트가 다운되더라도 모든 개발자는 다운로드된 버전에 액세스할 수 있습니다.

Deno는 `$DENO_DIR` 환경 변수 아래에 지정된 디렉토리에 캐시를 저장합니다. 우리가 직접 변수를 설정하지 않으면 시스템의 기본 캐시 디렉토리로 설정됩니다. 우리는 로컬 리포지토리 어딘가에 `$DENO_DIR`를 설정하여 버전 제어 시스템에서 확인할 수 있다.

#### 항상 URL로 가져와야 하나요?

URL을 계속 입력하는 것은 매우 지루할 것이다. 고맙게도, Deno는 우리에게 그렇게 하지 않기 위한 두 가지 옵션을 제시합니다.

첫 번째 옵션은 가져온 모듈을 로컬 파일에서 다시 내보내는 것입니다.

```bash
export { test, assertEquals } from "https://deno.land/std/testing/mod.ts";
```

위의 파일을 local-test-utils.ts라고 합니다. 이제 다시 `테스트` 또는 `등가 주장` 기능을 사용하려면 다음과 같이 참조하면 됩니다.

```coffeescript
import { test, assertEquals } from './local-test-utils.ts';
```

따라서 URL에서 로드하든 그렇지 않든 상관 없습니다.

두 번째 옵션은 JSON 파일에 지정한 가져오기 맵을 생성하는 것입니다.

```undefined
{
   "imports": {
      "http/": "https://deno.land/std/http/"
   }
}
```

그런 다음 다음과 같이 가져옵니다.

```coffeescript
import { serve } from "http/server.ts";
```

Deno에게 수입지도에 대해 --importmap 플래그를 포함하여 알려주어야 합니다.

```undefined
deno run --importmap=import_map.json hello_server.ts
```

#### Deno의 패키지 버전 관리

버전 지정은 패키지 공급자가 지원해야 하지만 클라이언트 측에서 다음과 같이 URL에서 버전 번호를 설정하는 것으로 귀결됩니다.

```undefined
https://unpkg.com/liltest@0.0.5/dist/liltest.js
```

### 브라우저 호환성 거부

Deno는 브라우저 호환을 목표로 한다. 엄밀히 말하면, ES 모듈을 사용할 때, 애플리케이션을 브라우저에서 사용할 수 있도록 하기 위해 웹 팩과 같은 빌드 도구를 사용할 필요가 없습니다.

그러나, Babel과 같은 도구들은 이 코드를 ES5 버전의 자바스크립트로 옮겨서, 그 결과, 그 언어의 모든 최신 기능을 지원하지 않는 오래된 브라우저에서도 이 코드를 실행할 수 있다. 그러나 이는 또한 불필요한 코드를 최종 파일에 많이 포함시키고 출력 파일을 부풀리는 가격에도 해당됩니다.

우리의 주요 목표가 무엇인지 결정하고 그에 따라 선택하는 것은 우리에게 달려 있다.

### Deno는 즉시 TypeScript를 지원합니다.

Deno를 사용하면 구성 파일이 없어도 TypeScript를 쉽게 사용할 수 있습니다. 그래도 프로그램을 일반 자바스크립트로 작성해서 데노로 문제 없이 실행할 수 있다.

## Deno는 생산 준비가 되었습니까?

Deno는 꽤 오랫동안 꾸준히 성장해왔다. 데노 v1.0은 5월 13일 팡파르로 공식 출시되었고 그 이후로 꾸준히 성장하고 있다.

집필 당시 가장 최근 안정적 출시는 2021년 1월 19일 출간된 Denov v1.7.0이다. Deno의 최신 버전은 컴파일 및 데이터 URL 기능을 개선했습니다. 새로운 릴리스의 하이라이트는 안정적인 지원되는 아키텍처(Windows x64, MacOS x64, Linux x64)에서 `deno compile`로 교차 컴파일할 수 있는 기능을 포함한다. 게다가 `deno compile`은 현재 Denov. 1.6에서 생성된 것보다 40-60% 작은 이진 파일뿐만 아니라 CA 인증서, 사용자 지정 V8 플래그, 잠긴 권한 등을 내장한 이진 파일도 생성한다.

Denov.1.7에서 다른 주목할 만한 기능으로는 데이터 URL 지원, denempt로 Markdown 파일 포맷 지원, 스크립트의 문서 위치를 설정하는 --location 플래그 등이 있다. Deno에 대한 최신 업데이트에 대해 자세히 알아보십시오.

Deno는 분산형 접근 방식을 통해 npm인 중앙 집중식 패키지 레지스트리에서 JavaScript 에코시스템을 해방시키는 데 필요한 단계를 밟는다.