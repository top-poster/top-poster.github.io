---
layout: post
title: "Oak 및 MongoDB를 사용하여 Deno에서 RESTful API 빌드
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/denomongo.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/denomongo.png?fit=730%2C487&ssl=1)

공식 문서에 따르면 Deno는 V8을 사용하고 Rust로 빌드 된 JavaScript 및 TypeScript를위한 간단하고 현대적이며 안전한 런타임입니다.
 기본적으로 TypeScript 및 Vanilla JS를 지원하고 브라우저 호환 URL이있는 타사 패키지를 사용하여 모듈을 관리합니다.
 

기본적으로 Deno는 안전합니다. 즉, 명시 적으로 활성화하지 않는 한 파일, 네트워크 또는 환경 액세스가 없습니다.
 Deno가 무엇인지, 어떻게 작동하며 왜 인기가 있는지에 대한 자세한 설명은 여기에서 확인하세요.
 

이 기사에서는 Deno와 함께 Oak 및 MongoDB를 사용하여 RESTful 웹 API를 구축하는 방법을 살펴 보 겠지만, 그 전에 Oak 및 MongoDB를 살펴 보겠습니다.
 

### 오크 란?
 

Oak는 Deno의 http 서버를위한 미들웨어 프레임 워크로, 라우터 미들웨어도 포함하고 있습니다. 두 미들웨어 모두 Koa에서 영감을 받았습니다.
 Oak는 Deno로 웹 애플리케이션을 구축 할 때 가장 인기있는 미들웨어 프레임 워크입니다.
 

### MongoDB 란 무엇입니까?
 

MongoDB는 개발자가 애플리케이션 데이터를 처리하는 데 사용하는 크로스 플랫폼 문서 지향 데이터베이스 프로그램입니다.
 MongoDB는 최적의 개발 생산성을 위해 JSON과 유사한 문서에 데이터를 저장합니다.
 

아래 예제에서 MongoDB를 추가 할 것이지만 자세한 설정 정보는이 기사를 확인하는 것이 좋습니다.
 

## 데모 : Deno를 사용하여 간단한 REST API 빌드
 

다음 섹션을 최대한 활용하기 위해이 자습서에서는 몇 가지 요구 사항을 가정합니다.
 

- JavaScript / TypeScript의 이해
 
- 컴퓨터에 설치된 Deno (여기에서 설치 가이드)
 
- 원하는 텍스트 편집기
 
- Postman (API 테스트 용)
 

### 목표 : CRUD 작업을 사용하여 REST API 생성
 

기본적인 CRUD 작업을 수행 할 수있는 간단한 REST API를 구축 할 것입니다.
 

```undefined
| METHOD | URL           | Description                   |
|--------|---------------|-------------------------------|
| GET    | /rooms        | Return all rooms from the DB  |
| GET    | /rooms/:id    | Return a single room          |
| POST   | /rooms         | Create a new room            |
| PUT    | /rooms/:id    | Update existing room          |
| DELETE | /rooms/:id    | Delete room                   |
```

### Oak로 Deno 서버 만들기
 

먼저`server.ts`라는 파일을 생성합니다.
 그런 다음 Deno 서버를 만들고 Oak에서 `Router`를 가져옵니다.
 다음으로 Oak 미들웨어 프레임 워크를 사용하여 경로 미들웨어 기능을 생성합니다.
 

아래 참조 :
 

```undefined
// server.ts
import { Application, Router } from "https://deno.land/x/oak/mod.ts";

// these functions does not exist yet, we will create them later 
import { getAllRooms, createRooms, getRoom, updateRoom, deleteRoom  } from './routes.ts'
const app = new Application();
const router = new Router(); 
const port: number = 8000;

router.get('/', (ctx) => {
    ctx.response.body = 'Hello from Deno'
})
// these functions does not exist yet, we will create them later 
    .get('/rooms', getAllRooms)
    .get('/rooms/:id', getRoom)
    .post('/rooms', createRooms)
    .put('/rooms/:id', updateRoom)
    .delete('/rooms/:id', deleteRoom)

// Here, we are telling our application to use the router
app.use(router.routes());
app.use(router.allowedMethods())
app.listen({ port })
console.log(`Server is running on port ${port}`);
```

6 번과 7 번 줄에서는 애플리케이션을 초기화하고 Oak 모듈에서 라우터를 가져 왔습니다.
 

9 행에서는 API 루트에 요청이있을 때`Hello from Deno` 응답을 구성했습니다.
 

10 행에서는 콜백이 컨텍스트의 약자 인`ctx`라는 매개 변수를 받도록 만들었습니다.
 참고로 Oak의 "context"는 Oak의 미들웨어를 통해 전송되는 현재 요청을 나타냅니다.
 

마지막으로 우리 앱은`port 8000`에서 실행됩니다.
 

### MongoDB 추가
 

MongoDB를 설정하기 위해`mongodb.ts` 파일을 만들고 다음 코드 줄을 추가합니다.
 

```js
import { MongoClient } from "https://deno.land/x/mongo@v0.12.1/mod.ts";

const client = new MongoClient();

client.connectWithUri("mongodb+srv://<DBNAME>:<DBPASSWWORD>@cluster0.6lbak.mongodb.net/deno-oak?retryWrites=true&w=majority");

const db = client.database('denoOakApi')

export default db;
```

아시다시피 첫 번째 단계는 Deno MongoDB 드라이버 패키지에서 `MongoClient`를 가져온 다음 초기화하고 호스팅 된 데이터베이스에 연결하는 것입니다.
 

### 기본 CRUD 작업 수행
 

이제 Deno 및 Oak를 사용하여 MongoDB 데이터베이스에서 데이터를 생성, 읽기, 업데이트 및 삭제할 수있는 기본 CRUD 작업을 수행 할 준비가되었습니다.
 

#### 데이터 읽기
 

먼저`routes.ts`라는 파일을 만들고 다음 코드 줄을 추가합니다.
 

```js
import { RouterContext } from "https://deno.land/x/oak/mod.ts";
import db from './mongodb.ts'

const roomsCollection = db.collection('rooms');

const getAllRooms = async (ctx: RouterContext) => {
    const rooms = await roomsCollection.find()
    ctx.response.body = rooms
}
```

이제 Postman을 사용하여 API를 테스트 할 수 있습니다.
 앱을 실행하려면 작업 디렉터리의 루트로 이동하고 터미널에서 다음 명령을 실행합니다.
 

```undefined
 deno run --allow-net --allow-plugin --allow-read --allow-write --unstable server.ts
```

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/deno-server-running.png?resize=560%2C132&ssl=1)

이제 Postman을 열고`localhost : 8000 / rooms`에`GET` 요청을합니다.
 올바르게 수행되면 빈 배열이 생성됩니다 (아래 참조).
 아직 데이터베이스에 데이터를 추가하지 않았기 때문입니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/testing-deno-api.png?resize=927%2C378&ssl=1)

#### 데이터 생성
 

데이터를 읽기 위해서는 먼저 읽을 데이터가 있어야합니다.
 아래 코드 블록에서 데이터베이스에 데이터를 추가하도록 설정합니다.
 

```undefined
const createRooms = async (ctx: RouterContext) => {
    const { room_number, size, price, isAvailable } = await ctx.request.body().value;
    const room: any = {
        room_number,
        size,
        price,
        isAvailable
    }
    const id = await roomsCollection.insertOne(room)
    room._id = id;
    ctx.response.status = 201
    ctx.response.body = room
}
```

2 행에서는 요청 본문에서`room_number`,`size`,`price` 및`isAvailable`을 구조화 한 다음 MongoDB 데이터베이스의`roomsCollection`에 해당 값을 삽입했습니다.
 11 행에서 새로 생성 된 방을 사용자에게 반환하는 201 상태를 보냈습니다.
 

이 기능을 테스트하기 위해 Postman에 새 방을 만들 수 있습니다.
 이렇게하려면 요청 본문에 다음 데이터를 추가 한 후`localhost : 8000 / rooms`에`POST` 요청을합니다.
 

```undefined
{
    "room_number": 214,
    "size": "18 by 22 FT",
    "price": 450,
    "isAvailable": false
}
```

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/data-and-id.png?resize=1007%2C513&ssl=1)

이제 고유 ID로 새로 생성 된 데이터가 있습니다.
 

#### 단일 문서 얻기
 

이 예에서는 ID로 싱글 룸을 검색합니다.
 이를 위해 아래 3 행에 설명 된대로 요청의`param`에서 문서의 ID를 가져옵니다.
 마지막으로 MongoDB 데이터베이스를 쿼리하여`_id` 값이`ctx.params.id`에서 오는 값과 동일한 문서를 찾습니다.
 

```undefined
const getRoom = async (ctx: RouterContext) => {
    // get the id of the document from the params object
    const id = ctx.params.id
    const room = await roomsCollection.findOne({ _id: { $oid: id } })
    ctx.response.body = room
}
```

Postman에서 테스트하려면`localhost : 8000 / rooms / <documentID>`에 GET 요청을합니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/retrieving-doc.png?resize=995%2C552&ssl=1)

#### 기존 데이터 업데이트
 

기존 문서를 업데이트하려면 문서의 ID를 가져온 다음 업데이트하려는 필드에서 `$ set`을 호출해야합니다.
 여기서 `$ set`을 사용하는 것이 핵심입니다.
 `$ set`이 없으면 코드베이스에 지정되지 않은 다른 모든 필드를 삭제합니다.
 

```undefined
const updateRoom = async (ctx: RouterContext) => {
    // get the id of the document from the params object
    const id = ctx.params.id
    const { room_number, size, price, isAvailable } = await ctx.request.body().value;
    const { modifiedCount } = await roomsCollection.updateOne({ _id: { $oid: id } }, {
        $set: {
            price,
            isAvailable
        }
    })
    // If the id does not exist in collection, we return a 404 status and send a custom message
    if (!modifiedCount) {
        ctx.response.status = 404;
        ctx.response.body = { message: 'Room not found' }
        return;
    }
    ctx.response.body = await roomsCollection.findOne({ _id: { $oid: id } })
}
```

Postman에서 API를 테스트하려면`localhost : 8000 / rooms / <documentID>`에 PUT 요청을합니다.
 

#### 데이터 삭제
 

Deno 및 Oak를 사용하여 MongoDB 컬렉션에서 데이터를 삭제하는 프로세스는 매우 간단합니다.
 

`ctx.params.id`를 사용하여 매개 변수에서 문서의 ID를 가져 오면 (이전 예제에서했던 것처럼) 남은 일은 컬렉션에서 MongoDB `deleteOne`메서드를 호출하고 ID를 전달하는 것입니다.
 삭제하려는 문서의
 

```undefined
const deleteRoom = async (ctx: RouterContext) => {    
    const id = ctx.params.id

    const room = await roomsCollection.deleteOne({ _id: { $oid: id } });

    if (!room) {
        ctx.response.status = 404;
        ctx.response.body = { message: 'Room not found' }
        return;
    }
    ctx.response.status = 204;
}
```

## 결론
 

이 기사에서는 Deno 프레임 워크 Oak를 사용하여 간단한 CRUD 앱을 빌드하여 RESTful API를 구현했습니다.
 또한 Deno 앱을 MongoDB에 연결했습니다.
 

예상대로 우리가 다룬 내용은 Deno 런타임 및 Oak 프레임 워크로 달성 할 수있는 것의 표면에 불과합니다.
 더 많은 연습을 위해 Deno 및 Postgres를 사용하여 RESTful API를 빌드하는 방법에 대한이 자습서를 읽으십시오.
 Deno에 대한 자세한 내용은 공식 문서를 참조하십시오.
 

이 튜토리얼에 대해 어떻게 생각했는지 아래의 의견 섹션에 알려주십시오.
 저는 Twitter와 GitHub에서 소셜입니다.
 읽어 주셔서 감사합니다.
 