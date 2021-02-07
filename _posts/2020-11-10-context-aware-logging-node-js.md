---
layout: post
title: "Node.js의 컨텍스트 인식 로깅"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/context-aware-logging-node-js.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/context-aware-logging-node-js.png?fit=730%2C487&ssl=1)

우수한 애플리케이션 모니터링의 가장 기본적인 요구 사항 중 하나는 진행 중인 모든 상황을 추적하기 위한 강력한 로깅을 갖추는 것입니다. 하지만, 그들 스스로, 로그 메시지는 우리에게 많은 것을 말해줄 수 있을 뿐이다. 동영상이 업로드되었음을 나타내는 로그 메시지를 보는 것은 유용하지만, 일반적으로 이 "이벤트"에 대한 컨텍스트도 갖고 싶습니다.

각 로그 메시지에 포함되어야 하는 최소 상황별 정보는 요청 식별자입니다. 단일 요청에 의해 생성된 모든 로그를 그룹화할 수 있으므로 라이프사이클을 쉽게 추적하고 다양한 단계에서 로그의 동작을 모니터링할 수 있습니다. 현재 인증된 사용자 ID와 사용자 서비스 같은 로그가 전송된 위치도 컨텍스트에 포함될 수 있다.

## 요청을 추적하는 기본 방법 및 요청이 충분하지 않은 이유

Express.js 응용 프로그램에서 생성된 각 로그에 요청 식별자를 첨부하는 방법을 살펴보겠습니다. 코드로 바로 가길 원하시면, 여기 리포고를 찾으실 수 있습니다.

가장 간단한 방법은 요청에 ID를 첨부하여 어디에서나 사용하는 것입니다.

```undefined
req.id = uuid.v4();
```

그런 다음 처리기의 로그에 수동으로 연결합니다.

```undefined
// handler
logger.log('This is a log from the handler for the request with id:', req.id)
```

처음에 그것은 비록 약간 지루하고 반복적이긴 하지만 좋은 해결책으로 보일 수 있다.

각 로그에 수동으로 "req.id"을 첨부하는 것은 반복적이지만, 결국에는 로그 방법으로 쉽게 포맷할 수 있는 객체로 대체될 것이라고 주장할 수 있다.

```undefined
logger.log('This is a log from the handler for the request', { requestId: req.id })
```

이 컨텍스트 객체를 두 번째 인수로 전달하면 로그 메시지에 자동으로 첨부할 수 있습니다. 그러나 요청 개체에 액세스할 수 없는 함수에서 메시지를 기록하려고 하면 문제가 더 분명해집니다.

```js
app.use((req) => createUser())

function createUser() { logger.log('User created!', {}) }
```

createUser 함수는 어떤 요청이 실행되었는지 알 수 없다.

이 문제는 컨텍스트 인수를 CreateUser 함수로 전달하여 해결할 수 있지만, 그렇다고 해결되는 것은 없습니다. createUser로 불리는 또 다른 기능이 있다면 어떨까? 어떤 것을 기록해야 하는 모든 기능들은 이 "컨텍스트" 매개변수를 받아들여야 하고 결과적으로 코드를 엉망으로 만들어야 한다.

대신, 로거가 이 수신 로그 메시지가 지정된 ID를 가진 요청을 처리하는 처리기에 의해 호출되었다는 것을 어떻게든 아는 것이 더 나은 해결책일 것이다. 이 컨텍스트 인식 로거를 사용하면 이 컨텍스트 객체를 전달하는 데 대해 걱정할 필요가 없으며 필요한 모든 정보를 logger.log 방법으로 직접 얻을 수 있습니다. 그러면 정보가 각 발송 로그 메시지에 자동으로 첨부될 수 있습니다.

많은 개발자들이 이 기능에 익숙하지 않을 수 있지만, 노드에서는 노드 8에서 도입된 비동기 후크라는 기능을 사용하여 동기식 및 비동기식 함수 호출의 "체인"을 추적할 수 있다.

기능이 오랫동안 사용되었기 때문에 성숙했다고 가정할 수 있지만, 노드 15에서는 여전히 실험적인 것으로 간주됩니다. 그러나 cls-hook이라는 라이브러리는 비동기식 훅보다 훨씬 더 오래 사용되었으며 일련의 함수 호출을 통해 전파되는 비동기식 컨텍스트를 만들고 추적하기 위한 일관되고 안정적인 API를 제공하였다.

함수 호출 체인은 함수 `하나`()를 `둘`()을 `둘`()로 부르고 다른 함수들을 `뭉치`라고 부르는 함수가 있다고 말하는 멋진 방법이다. 즉, 동기식 및 비동기식 함수 모두에 대한 함수 호출 스택과 같습니다.

cls-hook을 사용하여 비동기 컨텍스트를 만들고 관리하는 방법에 대해 살펴보겠습니다.

## 노드에서 컨텍스트 인식 로깅을 위해 cls-hook 사용

라이브러리를 설치하려면 다음을 실행하십시오.

```undefined
npm install --save cls-hooked
```

### 네임스페이스 만들기

네임스페이스는 현재 함수 체인에서 사용할 수 있는 비동기 컨텍스트에서 값을 가져오고 설정하기 위한 API를 제공하는 개체입니다.

네임스페이스를 생성하는 방법은 다음과 같습니다.

```undefined
const applicationNamespace = createNamespace('APP_NAMESPACE');
```

함수 호출 체인에 비동기 컨텍스트를 사용하려면 다음과 같이 컨텍스트 인식 콜백에서 "top" 함수를 명시적으로 실행해야 한다.

```css
applicationNamespace.run(contextAwareFunction);
```

.run() 메서드를 사용하여 네임스페이스가 빈 컨텍스트를 생성하여 `contextAwareFunction`과 거기서 발생하는 모든 함수 호출에 제공해야 한다고 말하고 있습니다.

네임스페이스의 get 및 set 메서드를 사용하면 다음 예에서 볼 수 있듯이 매우 직관적으로 사용할 수 있습니다.

```js
import { createNamespace } from 'cls-hooked';

const applicationNamespace = createNamespace('APP_NAMESPACE');

function deepContextAwareFunction() {
  console.log(applicationNamespace.get('TEST'));
}

function contextAwareFunction() {
  applicationNamespace.set('TEST', 150);

  deepContextAwareFunction();
}

applicationNamespace.run(() => contextAwareFunction());
```

예상대로 console.log 방식은 150을 산출할 것이다.

이제 라이브러리의 API를 알게 되었으니 Express 애플리케이션과 실제 사례를 통합해 보겠습니다.

### Express.js 응용 프로그램과 통합

cls-hooked 라이브러리는 Express 생태계에 쉽게 연결할 수 있습니다. 이는 Express의 가장 강력한 기능인 미들웨어가 비동기 컨텍스트인 함수 호출 체인과 동일한 개념을 기반으로 하기 때문입니다.

다음은 이 예제에 사용된 상용구 응용 프로그램입니다.

```js
const log = (message: string) => {
  console.log(message);
}

const nestedHandler = () => {
  log('This is a nested handler.');
};

const helloWorldHandler: RequestHandler = (req, res) => {
  log('This is a hello world log message.');

  nestedHandler();

  res.send('Hello world');
};

async function bootstrap() {
  const app = express();
  const port = 3000;

  // routes
  app.get('/', helloWorldHandler);

  app.listen(port, () => {
    console.log(`Listening on port: ${port}.`);
  });
}

bootstrap();
```

그것은 매우 간단한 설정이다. 포트 3000에서 수신하는 서버를 만들고 `hello world` 메시지로 응답하는 경로 `/`를 하나만 갖는다.

hello World Handler라는 경로 핸들러 외에도 경로 핸들러에 의해 실행되는 nestedHandler라는 기능도 있다. 두 기능 모두 `log` 기능을 사용하여 콘솔에 메시지를 기록합니다. 여기서의 목표는 두 로그 메시지 모두에 컨텍스트 정보가 첨부되도록 하는 것입니다.

각 미들웨어는 Express가 요청을 계속 처리할 수 있도록 `다음()` 함수를 호출할 책임이 있습니다. .run() 방법으로 다음() 함수를 줄임으로써 모든 미들웨어와 그 결과 경로 처리기를 컨텍스트 인식으로 만들 수 있다. 자, 이렇게 하죠.

```coffeescript
const attachContext: RequestHandler = (req, res, next) => {
  applicationNamespace.run(() => next());
}
```

실행 방법에 제공된 콜백 내부의 다음 함수를 호출하면 다음 미들웨어가 모두 동일한 비동기 컨텍스트를 사용할 수 있습니다. 컨텍스트를 활용하는 모든 기능보다 먼저 배치해야 합니다. 대개 첫 번째 미들웨어로 배치하는 것이 좋습니다.

이제 미들웨어가 네임스페이스를 사용하여 값을 가져오고 설정할 수 있다는 것을 알았으므로, 해당 컨텍스트에서 요청의 ID를 설정하는 미들웨어 기능을 만들어 보겠습니다.

```coffeescript
const setRequestId: RequestHandler = (req, res, next) => {
  applicationNamespace.set('REQUEST_ID', v4());

  next();
}
```

여기서는 UUID 라이브러리를 사용하여 요청에 대한 임의 UUID v4 ID를 생성합니다. ID는 `REQUEST_` 아래에 저장됩니다.아이디 키.

두 개의 미들웨어가 모두 생성된 상태에서 애플리케이션에 해당 미들웨어를 연결하겠습니다.

```js
async function bootstrap() {
  const app = express();
  const port = 3000;

  // context middleware
  app.use(attachContext, setRequestId);

  // routes
  app.get('/', helloWorldHandler);

  app.listen(port, () => {
    console.log(`Listening on port: ${port}.`);
  });
}
```

setRequestId가 네임스페이스를 사용하기 때문에 `attachContext` 미들웨어가 먼저여야 합니다.

마지막으로 `log` 기능을 다시 작성하여 컨텍스트 값을 사용하여 기록된 각 메시지에 첨부하면 됩니다.

```js
export const log  = (message: string) => {
  const requestId = applicationNamespace.get('REQUEST_ID');

  console.log({
    message,
    requestId,
  });
};
```

이제 `localhost:3000/pmit`에서 서버에 대한 요청이 있을 때마다 콘솔에서 다음을 확인할 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/server-requests-console.png?resize=730%2C313&ssl=1)

예상대로 요청의 ID는 컨텍스트에서 올바르게 설정된 다음 `log` 함수에 의해 검색됩니다.

## 로깅을 넘어

비동기 컨텍스트는 로깅으로만 제한될 필요가 없습니다. 비동기 후크가 코드를 단순화하는 데 도움이 될 수 있는 가장 흥미로운 예 중 하나는 개발자가 간단한 `@Transaction()` 장식기를 사용하여 데이터베이스 트랜잭션 관리를 처리할 수 있도록 하는 typorm-transaction-cls-hook이라는 패키지이다. 나는 당신이 그것의 코드베이스와 후드 아래에서 어떻게 cls-hook을 사용하는지 보길 권한다.

## 결론

응용 프로그램에 의해 생성된 각 로그에는 모니터링 및 디버깅 중에 개발자가 사용할 수 있도록 상황에 맞는 정보가 첨부되어야 합니다. 비동기 컨텍스트를 사용함으로써, 우리는 이 컨텍스트 정보를 무언가를 기록하는 각 함수에 전달할 필요가 없고 대신 컨텍스트에 보관함으로써 코드베이스를 크게 단순화할 수 있다.

비동기 후크는 노드 15에서 아직 안정적이라고 선언되지 않았지만 cls-hook과 같은 라이브러리 덕분에 기본 API에 대해 걱정할 필요가 없다.

한 가지 기억해야 할 점은 노드가 라이프사이클의 모든 기능과 다양한 단계를 추적해야 하기 때문에 비동기 후크가 성능 적중과 함께 발생할 수 있다는 것입니다. 그러나 기능이 안정되면 성능에 미치는 영향이 덜 눈에 띕니다.