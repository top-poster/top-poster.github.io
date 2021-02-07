---
layout: post
title: "Deno의 SMTP 클라이언트 사용"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/deno-stmp-client.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/deno-stmp-client.png?fit=730%2C487&ssl=1)

Deno는 V8 JavaScript 엔진과 Rust를 기반으로 하는 JavaScript 및 TypeScript의 런타임입니다.

2019년 4월 EU JSconf에서 Node.js의 오리지널 크리에이터인 라이언 달이 공식 발표한 데노는 일급 TypeScript 지원을 받고 있다. 즉, 설정을 위해 수동 구성을 작성할 필요는 없지만 TypeScript를 사용하여 코드를 작성하도록 제한되는 것은 아닙니다.

Deno는 패키지 관리자가 없다는 점에서 Node와 상당히 다르다. 패키지를 호스팅하고 가져오기 위해 URL에 의존해야 하는 것에는 장단점이 있습니다.

이 튜토리얼에서는 Deno의 SMTP 메일 클라이언트를 사용하여 다른 사용자에게 메일을 보내는 Deno 응용 프로그램을 구축합니다. 따라서 로컬 컴퓨터에 설치된 JavaScript, 텍스트 편집기(VS Code 사용 예정) 및 POSTMAN에 대한 기본적인 이해가 필요합니다.

## Deno 설치

Deno를 설치하는 가장 좋고 쉬운 방법은 HomeBrew를 사용하는 것입니다.

터미널을 열고 다음을 입력합니다.

```undefined
brew install deno
```

설치가 완료되면 터미널에서 `deno`를 실행하여 성공적으로 설치되었는지 확인합니다.

## Deno 서버 설정

이제 응용 프로그램을 위한 서버를 설정하겠습니다. 우리는 또한 라우팅을 지원하는 Deno의 HTTP 서버를 위한 미들웨어 프레임워크인 Oak를 사용하여 우리의 서버를 구축할 것입니다.

server.ts 파일을 생성하고 다음을 추가하십시오.

```js
import { Application } from "https://deno.land/x/oak/mod.ts";
const app = new Application();
import router from "./routes.ts";
const PORT: number = 3000;
app.use(router.routes());
app.use(router.allowedMethods());
app.listen({ port: PORT });
```

여기서 우리는 `http` 패키지에서 `serve` 기능을 포장하는 `Application` 클래스에 접속했다. 우리는 이 인스턴스를 경로를 정의하고 포트를 수신하는 데 사용할 `앱` 변수에 저장했다.

## 경로 생성

다음으로, "routes.ts" 파일을 작성합니다. 여기서 경로를 생성하여 컨트롤러 파일로 통신합니다.

```coffeescript
import { Router } from "https://deno.land/x/oak/mod.ts";
const router = new Router();
router
  .get("/", (ctx) => {
    ctx.response.body = "This is the home route";
  })
export default router;
```

Oak에서 Router 클래스를 가져온 다음 새 인스턴스를 만든 방법에 주목하십시오.

이제 터미널에서 deno run --allow-all server.ts를 실행하여 애플리케이션을 실행할 수 있습니다. 이 애플리케이션은 포트 `3000`에서 실행됩니다. 이제 애플리케이션에 액세스하려고 하면 "이것이 홈 경로"가 표시됩니다.

다음 단계는 메시지를 보낼 새로운 경로를 추가한 후 메일러 기능을 구현하는 것입니다.

```coffeescript
import { Router } from "https://deno.land/x/oak/mod.ts";
import { sendMail } from "./controller.ts";
const router = new Router();
router
  .get("/", (ctx) => {
    ctx.response.body = "This is the home route";
  })
  .post("/send-mail", sendMail);
export default router;
```

우리는 우편물 발송을 위한 `POST` 요청인 새로운 경로를 추가했습니다. 그런 다음 `controller.ts` 파일을 만들어 경로 방법을 정의합니다. 또한 메일러 구성에 대한 `index.ts` 파일도 생성합니다.

controller.ts 파일을 생성하고 다음 코드를 추가합니다.

```js
import { mailerObj } from "./index.ts";
let sendMail = async (ctx: any) => {
  try {
    let body: any = await ctx.request.body();
    let dataObj = await body.value;
    await mailerObj(dataObj);
    ctx.response.body = { status: true, message: "Mail Sent" };
    ctx.response.status = 201;
  } catch (err) {
    ctx.response.body = { status: false, data: null };
    ctx.response.status = 500;
    console.log(err);
  }
};
export { sendMail };
```

먼저 메일러 인스턴스를 가져오는 것으로 시작합니다. 곧 정의할 것입니다. 각 컨트롤러 방법에는 컨텍스트가 전달되며, 이 컨텍스트를 사용하여 요청 본문을 생성합니다. 본문은 우리가 발송하는 메일 본문인 본문과 메일을 발송하는 수신인 수신인으로 구성됩니다.

우리는 쉽게 접근할 수 있도록 시체를 `메일러 오브제` 방식으로 전달할 것이다.

메일러 방식을 설정하기 전에 보안 수준이 낮은 옵션을 설정해야 합니다. 이 작업이 완료되면 구성을 계속할 수 있습니다.

## Deno SMTP 클라이언트 설정

index.ts 파일을 생성하고 다음 코드를 추가합니다.

```undefined
import { SmtpClient } from "https://deno.land/x/smtp/mod.ts";
const client = new SmtpClient();
await client.connectTLS({
  hostname: "smtp.gmail.com",
  port: 465,
  username: <gmail email>,
  password: <gmail password>
});
let mailerObj = async (data: any) => {
  await client.send({
    from: "Mail from Wisdom", // Your Email address
    to: data.to, // Email address of the destination
    subject: "Deno is Great",
    content: data.body,
  });
  await client.close();
};
export { mailerObj };
```

그런 다음 Deno의 `SmtpClient` 모듈을 가져와 해당 인스턴스를 만듭니다. `Smtp` 인스턴스를 사용하여 Gmail에 연결합니다. 이 구성은 Gmail 사용자 이름과 암호를 포함합니다. 보안을 위해 환경 변수에 이러한 세부 정보를 저장해야 합니다.

다음 단계는 발신인, 수신인, 제목 및 내용과 같은 메시지에 대한 세부 정보를 포함하는 메일러 개체를 정의하는 것입니다. 이 구성을 내보내야 다른 파일에 액세스할 수 있습니다.

## 환경변수

Gmail 정보를 저장하기 위해 환경 변수를 사용하여 코드를 수정합시다.

응용 프로그램 루트에 `.env` 파일을 생성하고 다음을 추가하십시오.

```undefined
GMAIL_USERNAME=<gmail email>
GMAIL_PASSWORD=<gmail password>
```

그런 다음 환경 변수를 수신하도록 메일러 구성을 수정합니다.

```undefined
import "https://deno.land/x/dotenv/load.ts";
import { SmtpClient } from "https://deno.land/x/smtp/mod.ts";
const client = new SmtpClient();
await client.connectTLS({
  hostname: "smtp.gmail.com",
  port: 465,
  username: Deno.env.get("GMAIL_USERNAME"),
  password: Deno.env.get("GMAIL_PASSWORD"),
});
let mailerObj = async (data: any) => {
  await client.send({
    from: "Mail from Wisdom", // Your Email address
    to: data.to, // Email address of the destination
    subject: "Testing Deno SMTP client",
    content: data.body,
  });
  await client.close();
};
export { mailerObj };
```

.env 파일에 저장된 데이터에 액세스하려면 Deno의 env 모듈을 가져와 `Deno.env.get(name)`을 사용하여 저장된 값을 가져와야 합니다.

.env 파일에 저장된 키가 같지만 값은 없는 `.env.example` 파일을 생성하고 .env 파일을 `.gitignore` 파일에 추가해야 한다는 점을 항상 기억하십시오.

응용 프로그램을 테스트하려면 POSTMAN을 열고 사후 요청을 합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/post-request-in-postman.png?resize=720%2C439&ssl=1)

요청을 한 후 수신자 메일을 열어 메일이 전송되었는지 확인할 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/testing-deno-stmp-client.png?resize=720%2C155&ssl=1)

## 결론

Deno SMTP 클라이언트를 사용하여 메시지를 보내는 것은 매우 쉽습니다. 이 모듈의 일반적인 사용 사례로는 구독자에게 뉴스레터 보내기 및 템플릿 전자 메일 보내기 등이 있습니다.

이 튜토리얼에서는 Gmail 및 기타 메일 공급자에 대한 SMTP 구성을 설정하는 방법과 사용자에게 동적 메시지를 보내는 방법에 대해 설명했습니다. 보안을 강화하기 위해 항상 중요한 구성 세부 정보를 환경 변수에 저장하는 것이 좋습니다.

이 튜토리얼에 사용된 전체 소스 코드를 보려면 GitHub로 이동하십시오.