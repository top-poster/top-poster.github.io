---
layout: post
title: "데이터 캐시에 AdonisJs에서 Redis 사용"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/02/redis-adonisjs-data-caching.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/redis-adonisjs-data-caching.png?fit=730%2C487&ssl=1)

AdonisJs는 마이크로 서비스 쓰기를 위해 특별히 제작된 Node.js 프레임워크이다. 필수 백엔드 프레임워크와 마찬가지로, AdonisJs는 Redis를 지원하는데, Redis는 애플리케이션의 요청/응답 시간을 단축시켜 프로세스를 원활히 하고 애플리케이션을 경량화한다. 레디스(Redis)는 원격 디렉토리 서버를 뜻하는 오픈 소스 인메모리 데이터 구조 저장소이다.

Redis는 다중 데이터 구조 또는 데이터 유형을 지원하는 디스크 영구 키 값 데이터베이스로, 데이터를 저장하고 검색할 수 있는 매핑된 키 값 기반 문자열(기존 유형의 데이터베이스에서 지원되는 데이터 모델과 유사함)을 지원하지만 목록, 세트 등과 같은 다른 복잡한 데이터 구조도 지원합니다.

진행하면서 레디스가 지원하는 데이터 구조에 대해 알아보겠습니다. 간단히 말하면, 이것은 파일을 캐시하므로 매번 데이터베이스에 요청을 할 필요가 없습니다.

매월 DB-Engineers 순위에 따르면, Redis는 종종 가장 인기 있는 키-값 데이터베이스이며 기술 세계에서 가장 인기 있는 10대 데이터베이스 중 하나로 선정되기도 합니다.

> N.B. 로컬 시스템에 Redis가 설치되어 있어야 합니다. 로컬 컴퓨터에 설치되어 있지 않으면 AdonisJs와 함께 작동하지 않습니다.

개발 시스템에 Redis를 설치하는 방법에 대한 지침은 여기에서 확인할 수 있습니다.

- 창문들
- macOS

설치하는 동안 기본 포트를 `6379`로 설정해야 합니다. 레디스 전용 포트로, 포트 6380을 사용 중인 경우 포트 6380을 사용한다.

## 도입 및 전제 조건

이 튜토리얼에서는 다음 방법에 대해 설명합니다.

- AdonisJs 애플리케이션의 새 인스턴스 생성
- Redis를 설치하고 애플리케이션에 설정
- 요청을 작성하도록 데이터베이스 구성
- Redis의 get 및 set 방법 사용
- Redis를 사용할 때와 데이터베이스에서 직접 호출할 때의 요청 시간 차이 표시
- Redis의 pub/sub 메서드를 사용하여 요청 게시 및 구독

이 튜토리얼을 계속 진행하려면 이 튜토리얼에서 기본 사항을 다루지 않으므로 JavaScript, AdonisJs 및 SQL 데이터베이스에 대한 기본 지식이 있어야 합니다.

### 시스템 요구 사항

- 노드
- MySQL
- 레디스

시작하려면 CLI 명령을 사용하여 애플리케이션을 생성하고 제공할 수 있도록 로컬 시스템에 Adonis CLI를 전체적으로 설치해야 합니다.

```coffeescript
npm -g @adonisjs/cli
```

다음으로, 우리는 새로운 AdonisJs 애플리케이션을 만들고 다음 코드를 사용하여 그것을 실행할 수 있다; 이것은 AdonisJs의 새로운 인스턴스를 만들 것이다. 그런 다음 응용 프로그램 폴더로 이동하여 응용 프로그램을 실행합니다.

```bash
adonis new adonis_redis

cd adonis_redis

adonis serve --dev
```

## AdonisJs 앱에 대한 Redis 구성

이제 애플리케이션을 실행하고 Redis를 설치합니다.

```coffeescript
npm install @adonisjs/redis
```

이렇게 하면 `subscribe` 방식을 포함하는 `start/redis.js`라는 새 파일이 생성됩니다. 우리는 나중에 튜토리얼에서 그 문제를 다룰 것이다.

설치가 완료되면 다음 `start/app.js` 파일에 Redis 제공자를 등록할 수 있습니다.

```undefined
const providers = [
  '@adonisjs/redis/providers/RedisProvider'
]
```

그런 다음 응용 프로그램에서 Redis를 시작하기 위해 `server.js` 파일에 아래 코드를 추가합니다. 이 추가가 없으면 애플리케이션이 실행될 때마다 Redis가 실행되지 않습니다.

```coffeescript
new Ignitor(require('@adonisjs/fold'))
  .preLoad('start/redis')
  .appRoot(__dirname)
```

이제 `REDIS_CONNECTION`을 `config/redis.js` 파일의 `local` 구성을 가리키는 `.env` 파일에 추가합니다.

```undefined
REDIS_CONNECTION=local
```

그러면 이제 컨트롤러, 경로 및 모델을 생성하여 데이터베이스에서 데이터를 가져오고 Redis를 사용하여 데이터를 캐시할 수 있습니다. 그런 다음 응답 시간의 차이를 확인하기 위해 포스트맨의 레디스로부터 데이터를 가져올 것입니다.

## 데이터베이스 설정

데이터베이스를 갖는 것은 이 튜토리얼의 기본이므로, 먼저 데이터베이스를 설정해 보겠습니다. 그러기 위해, 우리는 아래 코드를 사용하여 우리 응용 프로그램에 `@adonisjs/lucid` 패키지를 설치할 것입니다. Lucid는 활성 레코드 패턴으로 데이터를 저장하는 Adonis용 SQL ORM입니다.

```css
adonis install @adonisjs/lucid
```

설치가 완료되면 Lucid 제공자를 `start/app.js` 파일에 추가할 수 있으며, 응용 프로그램의 공급자 목록에 Lucid 패키지를 추가할 수 있습니다.

```undefined
const providers = [
  '@adonisjs/lucid/providers/LucidProvider'
]

const aceProviders = [
  '@adonisjs/lucid/providers/MigrationsProvider'
]
```

이 작업이 완료되면 아래 코드를 사용하여 `mysql`을 애플리케이션에 설치합니다.

```coffeescript
npm install mysql
```

그런 다음 `mysql` 구성을 사용하여 데이터베이스에 연결하도록 `.env` 파일을 구성합니다. 기본적으로 AdonisJs는 데이터 저장에 SQLite를 사용하기 때문에 이 작업이 필수적입니다.

```undefined
DB_CONNECTION=mysql
DB_USER=root
DB_PASSWORD=
DB_DATABASE=adonis_redis
```

이제 `config/database`에서 연결을 변경합니다.js`와 MySQL 연결:

```undefined
connection: Env.get('DB_CONNECTION', 'mysql'),
```

## 컨트롤러 생성

데이터베이스에서 모든 사용자를 가져올 `사용자` 컨트롤러를 만듭니다. 이를 위해 아래 코드를 사용하여 새 컨트롤러를 생성합니다.

```bash
adonis make:controller User --type http
```

그런 다음 `UserController.js` 파일의 `데이터베이스` 패키지를 가져와 데이터베이스에 연결하고 액세스합니다.

```undefined
// app/Controllers/UserController.js

const Database = use('Database')
```

다음으로, 데이터베이스를 호출하고 모든 사용자를 가져와 JSON으로 반환하는 `async` 기능을 추가합니다.

```undefined
// app/Controllers/UserController.js

async index({ response }) {
  let users = await Database.table('users').select('*');
  return response.status(201).json({
      status: true,
      message: 'fetched all users',
      data: users
  });
}
```

## '사용자' 경로 생성

컨트롤러를 설정하면 이제 애플리케이션에 API 호출을 할 수 있도록 경로를 구성할 수 있습니다. `start/routes.js`로 이동하여 다음을 추가합니다.

```undefined
// start/routes.js

Route.get('users', 'UserController.index');
```

우리는 사용자 컨트롤러에 인덱스 기능을 호출하는 사용자 경로를 만들었습니다. 이제 우체국에 있는 경로를 이용하여 모든 사용자를 불러올 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/response-time-postman.png?resize=730%2C390&ssl=1)

여기서 응답 시간은 2.07초이며, 트래픽이 낮은 웹 응용 프로그램에 적합합니다. 그러나 동시에 많은 요청이 발생하면서 트래픽이 많은 응용 프로그램을 실행하는 경우 응용 프로그램의 로드 시간이 매우 느려집니다.

이제 Redis를 사용하여 애플리케이션을 테스트하여 데이터를 캐슁했으므로 이제 Redis를 활용하여 애플리케이션의 속도와 사용자 편의성을 높이는 데 중요한 응답 시간을 대폭 단축할 수 있습니다.

## get/'set' 방법 사용

get 메서드는 Redis에서 키 값을 가져오고 문자열을 반환합니다. 설정 방법에는 문자열 값이 있는 키가 들어 있으며, 이 키는 문자열이 이미 포함되어 있는 경우 키 값을 덮어씁니다. 다음 방법을 `app/Controller/Http/UserController.js` 파일에서 사용하여 데이터베이스에서 사용자를 캐시할 수 있습니다.

```undefined
// app/controllers/Http/UserController.js

async index() {
  const cachedUsers = await Redis.get('users')
        if (cachedUsers) {
          return JSON.parse(cachedUsers)
        }

        const users = await Database.table('users').select('*');
        await Redis.set('users', JSON.stringify(users))
        return users
      }
}
```

위에서는 키 `users`에서 값을 얻고 있으며, 비어 있지 않으면 JSON 파일로 반환됩니다. 비어 있으면 데이터베이스에서 가져온 다음 데이터베이스의 데이터로 새 키를 설정하는 것입니다.

## 펍/서브 설정

Pub/sub는 수신인(가입자)이 메시지를 수신하는 동안 발신인(Redis에서 게시자라고 함)이 메시지를 전송하는 메시징 시스템을 구현합니다.

pub/sub를 설정하기 위해 `start/redis.js`에 서브를 생성하겠습니다. 서브는 펍 시퀀스가 시작될 때마다 데이터베이스에 직접 연결하여 JSON에 데이터를 저장합니다.

시작하려면 `start/redis`에서 데이터베이스와 Redis를 모두 가져옵니다.js:

```undefined
const Database = use('Database')
const Redis = use('Redis')
```

그런 다음 데이터베이스에서 모든 사용자를 가져오는 새로운 `구독` 방법을 작성합니다. 세트(set)는 모든 사용자의 값을 JSON 형식으로 포함하는 새로운 키 사용자(users)를 만들어 레디스에 저장한다.

```undefined
// start/redis.js

Redis.subscribe('users', async () => {
    let users = await Database.table('users').select('*');
    await Redis.set('users', JSON.stringify(users));
})
```

이제 `사용자` 컨트롤러에서 Redis를 가져와 `사용자` JSON 파일이 생성되었는지 확인한 후 파일을 반환하고, 그렇지 않으면 데이터베이스로 이동하여 모든 사용자를 가져오고 응답을 게시합니다.

```undefined
// app/Controller/Http/UserController.js

const Database = use('Database')
const Redis = use('Redis')

async index({ response }) {
        let cachedUsers = await Redis.get('users');
        if(cachedUsers) {
            let users = JSON.parse(cachedUsers);
            return response.status(201).json({
                status: true,
                message: 'fetched all users',
                data: users
            });
        }else {
            let users = await Database.table('users').select('*');
            Redis.publish('users', '');
            return response.status(201).json({
                status: true,
                message: 'fetched all users',
                data: users
            });
        }
    }
```

이제 포스트맨에서 `http://127.0.1:3333/users`를 실행하십시오. 응답 시간의 차이를 기록합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/response-time-redis.png?resize=730%2C388&ssl=1)

요청/응답 시간이 대폭 단축되었습니다. Redis가 없는 2.07초에서 Redis가 있는 경우 33ms로 단축되었습니다!

## 결론

본 튜토리얼에서는 새로운 AdonisJs 애플리케이션을 생성하고 Redis를 설치할 수 있었습니다. 그런 다음 데이터를 캐시하도록 Redis를 구성하고, 데이터베이스를 구성하며, Redis `get`, `set`, 게시 및 구독 방법을 사용하고, 애플리케이션의 요청/응답 시간을 단축했습니다.

계속해서 리포지토리를 시스템에 복제하여 원하는 대로 조정할 수 있습니다. .env.exe 파일을 복사하여 .env에 붙여넣은 다음 adonis key:generate를 사용하여 새 키를 생성해야 합니다. 이 튜토리얼이 도움이 되었기를 바랍니다. ✌️