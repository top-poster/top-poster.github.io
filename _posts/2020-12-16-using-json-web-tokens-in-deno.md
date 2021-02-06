---
layout: post
title: "Deno에서 JSON 웹 토큰 사용"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/JWT-authentication-deno.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/JWT-authentication-deno.png?fit=730%2C487&ssl=1)

Deno는 V8을 사용하는 JavaScript와 TypeScript를 위한 간단하고 현대적이며 안전한 런타임이며 Rust에 내장되어 있습니다. Node.js와 달리 Deno는 기본적으로 안전한 TypeScript를 기본적으로 지원합니다. Deno는 브라우저 호환 URL이 있는 타사 패키지를 사용하여 로컬 컴퓨터로 가져오거나 캐시되는 대신 모듈을 관리합니다.

JWT(JSON Web Token)는 일부 클레임을 주장하는 JSON을 페이로드에 포함하는 선택적 서명 및/또는 선택적 암호화로 데이터를 생성하기 위한 인터넷 표준이다. 간단히 말해서, 기본적으로 인증에 사용됩니다. 사용자가 응용 프로그램에 로그인하면 응용 프로그램이 JWT를 생성하여 사용자에게 다시 보냅니다.

Deno에서 JWT의 대부분의 사용 사례는 개발자가 인증 시스템을 구현하여 사용자가 특정 데이터에 액세스하기 위해 로그인해야 하는 경우이다.

이 기사에서는 통합용 Deno의 djwt 패키지를 사용하여 JWT를 Deno 애플리케이션에 통합할 것입니다.

## 전제조건

- JavaScript에 대한 확실한 이해
- 텍스트 편집기(우리의 경우 VS 코드를 사용합니다)
- 로컬 컴퓨터에 설치된 POSTMAN

## Deno에서 JSON 웹 토큰 시작

Deno 어플리케이션에서 JWT를 사용하려면 djwt Deno 라이브러리를 사용해야 합니다. `djwt`는 어떤 형태의 인증이나 권한 부여도 처리하지 않으며, 그 역할은 유효한 JSON 웹 토큰을 생성하고 검증하는 것이다.

시작하려면 응용 프로그램의 홈 디렉토리에 새 디렉토리를 만들어 보겠습니다. 이 디렉터리 내에 `index.ts` 파일을 만들어 코드를 작성합니다.

```bash
cd desktop && mkdir denojwt && cd denojwt
touch index.ts
code .
```

그러면 디렉토리와 `index.ts` 파일이 생성됩니다. `code.` 명령을 실행하면 VS Code에서 응용 프로그램이 열립니다. 원하는 텍스트 편집기를 자유롭게 사용하십시오.

djwt 라이브러리를 사용하려면 이 방법을 응용 프로그램으로 가져와야 합니다.

```coffeescript
import { validateJwt } from "https://deno.land/x/djwt/validate.ts";
import { makeJwt, setExpiration,Jose,Payload } from "https://deno.land/x/djwt/create.ts";
```

여기서 validateJwt 메서드는 토큰이 유효한지 여부를 확인합니다. makeJwt 메소드는 유효한 JWT를 생성하며, setExpiration 메소드는 토큰의 만료 시간을 설정합니다. Payload는 JWT 페이로드 또는 데이터를 위한 TypeScript 인터페이스이다. Jose는 토큰의 알고리즘과 유형을 나타냅니다.

우리는 서버의 경로와 설정을 정의하기 위해 oak 라이브러리를 사용할 것입니다. Oak를 사용하여 간단한 서버를 설정하고 라우팅합니다.

```coffeescript
import { Application, Router } from "https://deno.land/x/oak/mod.ts";
const app = new Application();
const PORT:number = 8080
//create a new instance of router
const router = new Router();
router
  .get("/test", (context) => {
    context.response.body = "Hello world!";
  })
  .get("/user", (context) => {
    context.response.body = "My name is Wisdom Ekpot";
  })
  app.use(router.routes());
app.use(router.allowedMethods());
await app.listen({ port: PORT });
```

## JSON 웹 토큰 생성

JWT를 사용할 때는 비밀키, 페이로드, 헤더를 설정해야 합니다. 이것은 기본 JWT 구성입니다. 이 구성을 변수에 저장하는 것이 좋습니다.

```js
const key = "mynameisxyzekpot";

const header: Jose = {
    alg: "HS256",
    typ: "JWT",
}

let payloader = (name:string) => {
  let payload:Payload = {
    iss: name,
    exp: setExpiration(new Date("2021-01-01"))
  }
  return payload
}
```

페이로드 방식은 페이로드가 매개 변수로 전달되고 만료 데이터 지속시간이 `2021-01-01`로 설정된다. 우리는 makeJwt 방식으로 사용할 수 있도록 페이로드 객체를 반환해야 할 것이다.

이렇게 정의하면 이제 정의된 구성을 사용하여 유효한 토큰을 반환하는 간단한 방법을 작성할 수 있습니다. 토큰을 생성하려면 다음과 같은 방법으로 makeJwt 방법을 사용합니다.

```coffeescript
const generateJWT = (name:string) => {
  return makeJwt({ key:secret_key, header, payload:payloader(name) })
}
```

여기서는 사용자가 입력한 이름을 매개 변수로 전달한 다음 페이로드 기능을 페이로드로 사용합니다.

이제 이 메서드를 호출하고 유효한 토큰을 응답으로 보내는 간단한 경로를 설정할 수 있습니다.

서버 및 라우팅에 Oak를 사용하기 때문에 다음과 같은 간단한 사후 경로를 만들어 유효한 토큰을 생성할 수 있습니다.

```undefined
.post("/generate", async (context) => {
     let body: any = await context.request.body();
    const { name } = await body.value;
    let token = await generateJWT(name)
    context.response.body = { status: true, data: name,token:token };
  });
```

다음으로, `generate J`를 사용하는 새 `/generate` 사후 요청 경로를 추가합니다.입력한 이름을 기준으로 사용자에 대한 토큰을 생성하는 WT 메서드입니다.

➡.request.body ➡은 사용자가 입력한 이름을 얻을 수 있는 요청 본문을 가져옵니다. 이제 POSTMAN을 사용하여 엔드포인트를 테스트해 보겠습니다.

![image](https://paper-attachments.dropbox.com/s_51CDB6FB7D9E8A97555A2DDC95AC82A147B0B68FE0ED89E487EFB24666A59BAA_1603480673378_Screen+Shot+2020-10-23+at+8.17.41+PM.png)

게시 요청을 `/generate` 경로에 보내고 이름을 본문으로 전달하면 해당 사용자의 토큰이 생성됩니다.

## JSON 웹 토큰 확인 중

우리는 가져온 validateJwt를 사용하여 토큰이 유효한지 여부를 확인할 수 있습니다. 이 방법은 토큰, 키, 알고리즘을 매개 변수로 사용한다. 우리는 makeJwt 방식으로 받은 토큰을 테스트에 사용할 것입니다.

먼저 검증 방법을 만드는 것으로 시작하겠습니다.

```coffeescript
const validateToken = (token:string) => {
    return validateJwt({jwt:token, key:secret_key,algorithm:header.alg});
}
```

헤더 개체에서 정의한 알고리즘을 사용하고 동일한 `secret_key`도 사용했다는 점에 유의한다.

이제 검증을 위한 새 사후 경로를 생성할 수 있습니다.

```undefined
.post("/validate", async (context) => {
    let body: any = await context.request.body();
    const { token } = await body.value;
    let validator =  await validateToken(token)
   context.response.body = {validator};
  });
```

다음으로, 검증자 방법을 사용하여 토큰이 유효한지 여부를 확인해 보겠습니다. 토큰이 유효하면 생성 시 사용한 페이로드를 반환합니다. 그러나 토큰이 잘못된 경우 다음과 같이 응답할 수 있습니다.

```bash
"validator": {
        "jwt": "yJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJXaXNkb20gRWtwb3QiLCJleHAiOjE2MDk0NTkyMDB9.-uucC6ORuOGNWAkj2d7CTRYzBJTnIn7rcaZXslrSxlg",
        "error": {
            "name": "JwtError",
            "date": "2020-10-23T19:40:29.472Z"
        },
        "isValid": false,
        "isExpired": false
    }
```

잘못된 토큰의 예제 응답입니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/JSON-web-token-deno-invalid.png?resize=730%2C448&ssl=1)

여기서 `isValid` 매개 변수는 false로 반환되고 오류 개체도 반환됩니다.

유효한 JWT는 다음과 같습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/JWT-deno-valid-token.png?resize=730%2C475&ssl=1)

## 결론

Deno 애플리케이션에 어떤 형태의 인증도 추가하는 것은 애플리케이션 보안에 매우 중요합니다. JWT는 서로 다른 기술에 광범위하게 사용되고 있으므로, 애플리케이션에 인증 및 인증을 구현할 때 고려해야 할 훌륭한 선택입니다.

이 프로젝트의 소스 코드는 내 GitHubrepo를 참조하십시오.