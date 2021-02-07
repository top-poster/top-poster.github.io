---
layout: post
title: "Deno에서 MongoDB 설정"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/setting-up-mongodb-deno.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/setting-up-mongodb-deno.png?fit=730%2C487&ssl=1)

Deno는 기본적으로 TypeScript를 지원하고 구성하는 JavaScript의 간단하고 안전한 런타임입니다. 몽고DB는 개발자들이 널리 사용하는 크로스 플랫폼 문서 지향 데이터베이스 프로그램이다. 이 기사에서는 Deno 애플리케이션에 MongoDB를 통합하는 방법에 대해 알아보겠습니다.

### 전제조건

- TypeScript에 익숙함
- VS 코드 또는 개발 시스템에 설치된 모든 코드 편집기.
- 몽고DB의 기본 지식.
- 로컬 컴퓨터에 POSTMAN이 설치되어 있습니다.

시작하기 전에 로컬 컴퓨터에 Deno가 설치되어 있는지 확인하십시오. Mac을 사용 중인데 아직 설치하지 않은 경우 터미널을 열고 다음 명령을 실행합니다.

```undefined
brew install deno
```

Windows 시스템을 사용하는 경우 터미널에서 다음 명령을 실행하여 설치할 수 있습니다.

```undefined
iwr https://deno.land/x/install/install.ps1 -useb | iex
```

Chocolatey를 설치한 경우 다음을 실행할 수 있습니다.

```undefined
choco install deno
```

## 시작 중

먼저 server.js 파일을 만든 다음 Deno 서버를 설정합니다. 포장이 없기 때문에.Node.js의 json 파일처럼 Deno는 URL 또는 파일 경로별로 모듈 참조를 사용합니다.

```php
import { Application, Router } from "https://deno.land/x/oak/mod.ts";
const app = new Application();
const router = new Router();
const PORT = 3000;
router
  .get("/", (ctx) => {
    ctx.response.body = "This is the home route";
  });
app.use(router.routes());
app.use(router.allowedMethods());
app.listen({ port: PORT });
```

Oak를 사용하여 서버를 실행하고 있습니다. Oak는 HTTP 서버를 위한 Deno 미들웨어 프레임워크이며 라우터 미들웨어도 포함한다.

이제 터미널에서 `deno run --allow-net server.ts`를 실행하여 애플리케이션을 지원할 수 있습니다. Deno의 보안 때문에 서버를 실행하려면 --allow-net을 추가해야 합니다. 우리는 localhost:8000에서 어플리케이션을 조회할 수 있다.

이제 서버가 가동되고 있으므로 응용 프로그램에서 MongoDB를 설정할 수 있습니다. DB 구성을 설정할 `db.js` 파일을 작성합니다.

```undefined
// import the package from url
import { init, MongoClient } from "https://deno.land/x/mongo@v0.12.1/mod.ts";
// Intialize the plugin
await init();
// Create client
const client = new MongoClient();
// Connect to mongodb
client.connectWithUri("mongodb://localhost:27017");
// Give your database a name
const dbname: string = "denoMongoApp";
const db = client.database(dbname);
// Declare the mongodb collections here. Here we are using only one collection (i.e user).
// Defining schema interface
interface UserSchema {
  _id: { $oid: string };
  name: string;
  email: string;
  phone: string;
}
const User = db.collection<UserSchema>("user");
export { db, User };
```

여기서 플러그인을 가져온 다음 초기화합니다. 우리는 mongoClient의 인스턴스를 만들어 mongoDB에 연결한다. 우리는 몽고 구성을 테스트하기 위해 사용할 사용자 컬렉션을 설정합니다. 우리는 또한 이름, 이메일, 전화, 생성된 Mongo ID를 사용하는 MongoDB 모델에 대한 TypeScript 인터페이스를 정의합니다.

이제 이 기능이 설정되었으므로 테스트하기 위한 간단한 CRUD 애플리케이션을 구축하겠습니다. 애플리케이션의 확장성을 높이기 위해서는 routs.ts와 controller.ts 파일을 생성해야 합니다. 우리의 `routes.ts` 파일은 우리의 경로를, `controller.ts` 파일은 우리의 몽고DB 논리를 다룰 것이다.

routes.ts 파일은 다음과 같아야 합니다.

```coffeescript
import { Router } from "https://deno.land/x/oak/mod.ts";
import { getUsers, createUser } from "./controller.ts";
const router = new Router();
router
  .get("/", (ctx) => {
    ctx.response.body = "This is the home route";
  })
  .get("/get-users", getUsers)
  .post("/create-user", createUser);
export default router;
```

이 새로운 구성을 통해 `server.ts` 파일을 다음과 같이 수정해야 합니다.

```js
import { Application } from "https://deno.land/x/oak/mod.ts";
const app = new Application();
import router from "./routes.ts";
const PORT: number = 3000;
//
app.use(router.routes());
app.use(router.allowedMethods());
app.listen({ port: PORT });
```

이제 우리는 `router.ts` 파일에서 호출한 우리의 경로 방법을 정의해야 한다. 먼저 getUser 메소드를 만드는 것부터 시작하겠습니다. db.ts 파일에 생성한 데이터베이스 인스턴스를 가져와야 합니다.

```js
import { User } from "./db.ts";

let getUsers = async (ctx: any) => {
  try {
    const data: any = await User.find();
    ctx.response.body = { "status": true, data: data };
    ctx.response.status = 200;
  } catch (err) {
    ctx.response.body = { status: false, data: null };
    ctx.response.status = 500;
    console.log(err);
  }
};
```

이제 get-user 끝점을 호출할 수 있으며, 기본적으로 빈 배열을 반환하고 상태를 200으로 표시합니다.

동일한 기술을 사용하여 `createUser` 방법을 구현합니다.

```undefined
let createUser = async (ctx: any) => {
  try {
    let body: any = await ctx.request.body();
    console.log(await body.value);
    const { name, phone, email } = await body.value;
    const id = await User.insertOne({
      name: name,
      phone: phone,
      email: email,
    });
    ctx.response.body = { status: true, data: id };
    ctx.response.status = 201;
  } catch (err) {
    ctx.response.body = { status: false, data: null };
    ctx.response.status = 500;
    console.log(err);
  }
};
```

비동기 요청이므로 항상 `body.value`를 기다려야 합니다. 이렇게 하지 않으면 개체 ID만 반환됩니다.

애플리케이션을 실행하십시오.

```undefined
deno run --allow-all --unstable server.ts
```

`--allow-all` 플래그를 사용하면 모든 권한이 허용되고 Deno의 보안이 해제됩니다.

## 엔드포인트 테스트

POSTMAN을 사용하여 엔드포인트를 테스트해 보겠습니다. 다음은 사용자를 위한 테스트입니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/postman-test-getting-users.png?resize=730%2C463&ssl=1)

다음은 사용자를 생성하기 위한 테스트입니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/postman-test-creating-user.png?resize=730%2C402&ssl=1)

## 결론

Deno에서 애플리케이션을 설정하는 것은 매우 쉽고 안전합니다. Deno 컨트롤러만 있으면 원하는 데이터베이스를 사용할 수 있습니다. 데노에게 `패키지`가 없다는 사실 때문에.json 파일은 로컬 컴퓨터의 모든 모듈을 캐슁하므로 속도가 훨씬 빠릅니다.