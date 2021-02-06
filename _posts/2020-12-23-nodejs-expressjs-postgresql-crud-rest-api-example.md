---
layout: post
title: "Node.js, Express.js 및 PostgreSQL: CRUD REST API 예제"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2018/10/nodejs-expressjs-postgresql-crud-rest-api-example.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2018/10/nodejs-expressjs-postgresql-crud-rest-api-example.png?fit=730%2C487&ssl=1)

편집자 참고: 이 게시물은 2020년 12월 23일에 업데이트되었습니다.

최신 웹 개발자인 경우 소프트웨어 시스템 간의 통신을 용이하게 하기 위해 API를 사용하는 방법을 아는 것이 무엇보다 중요합니다.

이 튜토리얼에서는 Express.js 서버에서 실행되고 Postgre를 사용하여 Node.js 환경에서 CRUD RESTful API를 생성하는 방법을 보여 줍니다.SQL 데이터베이스. Express.js 서버를 Postgre와 연결하는 방법에 대해 살펴보겠습니다.노드 사후 처리를 사용하는 SQL입니다. API는 Postgre에 해당하는 HTTP 요청 방법을 처리할 수 있습니다.API가 데이터를 가져올 SQL 데이터베이스입니다. Postgre 설치 방법도 배울 것입니다.SQL을 사용하고 명령줄 인터페이스를 통해 SQL과 함께 작동합니다.

이 튜토리얼의 목적은 해당 데이터베이스 명령을 실행하는 API에서 CRUD 작업(GET, POST, PUT, DELETE)을 허용하는 것이다. 이를 위해 각 엔드포인트에 대한 경로와 각 쿼리에 대한 함수를 설정합니다.

다음 사항에 대해 자세히 설명합니다.

- RESTful API란?
- CRUD API란?
- Express.js란 무엇입니까?
- PostgreSQL이란?
- 노드-포스트그레스란?
- Postgre 생성SQL 데이터베이스.
- PostgreSQL 명령 프롬프트
- Postgres에서 역할 생성
- Postgres에서 데이터베이스 만들기
- Postgres에서 테이블 만들기
- Express.js 서버 설정
- Node.js에서 Postgres 데이터베이스에 연결
- CRUD 작업을 위한 경로 생성
- REST API에서 CRUD 기능 내보내기
- REST API에서 CRUD 기능 설정

이 튜토리얼을 최대한 활용하려면 다음을 수행해야 합니다.

- JavaScript 구문 및 기본 사항에 익숙함
- 명령줄에 대한 기본 지식 보유
- Node.js 및 npm 설치

## RESTful API란?

대표 상태 전송(REST)은 웹 서비스에 대한 일련의 표준을 정의합니다. API는 소프트웨어 프로그램이 서로 통신하기 위해 사용하는 인터페이스이다. 따라서 RESTful API는 REST 아키텍처 스타일과 제약 조건을 준수하는 API이다.

REST 시스템은 상태 비저장, 확장, 캐시 가능 및 균일한 인터페이스를 제공합니다.

## CRUD API란?

API를 빌드할 때 모델이 리소스를 생성, 읽기, 업데이트 및 삭제할 수 있어야 하는 4가지 기본 기능을 제공하려고 합니다. 이러한 일련의 필수 작업을 일반적으로 CRUD라고 합니다.

RESTful API는 HTTP 요청을 가장 많이 사용합니다. REST 환경에서 가장 많이 사용되는 HTTP 방식은 개발자가 CRUD 시스템을 만들 수 있는 GET, POST, PUT, DELETE 등 4가지다.

REST 환경에서 CRUD를 적용하는 방법은 다음과 같습니다.

- 생성 - REST 환경에서 리소스를 생성하려면 `HTTP POST` 방법을 사용하십시오.
- 읽기 - 리소스를 읽으려면 `GET` 방법을 사용합니다. 리소스를 읽으면 데이터를 변경하지 않고 검색됩니다.
- 업데이트 - 리소스를 업데이트하려면 `PUT` 방법을 사용하십시오.
- 삭제 - 시스템에서 리소스를 제거하려면 `DELETE` 방법을 사용하십시오.

## Express.js란 무엇입니까?

Express.js는 Node.js에 가장 인기 있는 프레임워크 중 하나이다. 사실 MERN, MEVN, MEAN Stack의 "E"는 "Express"의 약자이다.

공식 Express.js 설명서에 따르면, "Express는 Node.js를 위한 빠르고, 의견이 없으며, 미니멀리스트적인 웹 프레임워크이다."

익스프레스는 미니멀리스트임에도 불구하고 유연성이 뛰어나고, 이로 인해 생각할 수 있는 거의 모든 작업이나 문제를 해결하는 데 사용할 수 있는 다양한 익스프레스 미들웨어가 개발되었습니다.

## PostgreSQL이란?

PostgreSQL(PostgreSQL)은 자유-오픈 소스 관계형 데이터베이스 관리 시스템이다. Postgre와 경쟁하는 MySQL, Microsoft SQL Server 또는 MariaDB와 같은 몇 가지 유사한 데이터베이스 시스템에 익숙할 수 있습니다.SQL입니다.

PostgreSQL은 1997년부터 존재해 온 강력한 관계형 데이터베이스이며 리눅스, 윈도, macOS 등 모든 주요 운영 체제에서 사용할 수 있다. 포스트그레 이후SQL은 안정성, 확장성 및 표준 규정 준수로 잘 알려져 있으며 개발자와 기업이 데이터베이스 요구사항에 사용할 수 있는 인기 있는 옵션입니다.

Secretize를 사용하여 Node.js RESTful CRUD API를 생성할 수도 있습니다. Secretize는 Postgres, MySQL, MariaDB, SQLite 및 Microsoft SQL Server를 위한 약속 기반 Node.js ORM이다.

Node.js REST API에서 Secretize를 사용하는 방법에 대한 자세한 내용은 아래 비디오 튜토리얼을 참조하십시오.

## 노드-포스트그레스란?

node-postgres는 비차단 Postgre입니다.Node.js용 SQL 클라이언트입니다. 기본적으로 Node-postgres는 Postgre와 인터페이스하기 위한 Node.js 모듈의 모음이다.SQL 데이터베이스.

노드 포스트그레스 지원은 콜백, 약속, 비동기/대기, 연결 풀링, 준비된 문, 커서, 리치 유형 구문 분석, C/C++ 바인딩 등이 있다.

## Postgre 생성SQL 데이터베이스.

Postgre를 설치하여 이 튜토리얼을 시작합니다.SQL, 새 사용자 생성, 데이터베이스 생성, 스키마 및 일부 데이터로 테이블 초기화.

### 설치

Windows(윈도우)를 사용하는 경우 Postgre의 Windows 설치 관리자를 다운로드합니다.SQL입니다.

Mac을 사용하는 경우 이 튜토리얼에서는 새 프로그램을 설치하기 위한 패키지 관리자로 컴퓨터에 홈브루가 설치되어 있다고 가정합니다. 그렇지 않은 경우 링크를 클릭하고 지침에 따라 홈브루를 설치하십시오.

터미널을 열고 `brew`와 함께 `postgresql`을 설치합니다.

```undefined
brew install postgresql
```

> 웹에서 postgresql이 아닌 brew install postgres라는 설명을 볼 수 있다. 이 두 옵션 모두 Postgre를 설치합니다.컴퓨터의 SQL입니다.

설치가 완료되면 postgresql을 가동하여 실행하려고 합니다. postgresql은 services start로 할 수 있습니다.

```coffeescript
brew services start postgresql
==> Successfully started `postgresql` (label: homebrew.mxcl.postgresql)
```

> 언제든지 'postgresql' 서비스를 중지하려면 'brew services stop postgresql'을 실행할 수 있습니다.

PostgreSQL이 지금 설치되어 있으므로 다음 단계는 SQL 명령을 실행할 수 있는 postgres 명령줄에 연결하는 것입니다.

## PostgreSQL 명령 프롬프트

psql은 Postgre이다.SQL 대화형 터미널. `psql`을 실행하면 Postgre에 연결됩니다.SQL 호스트입니다. `psql --help`를 실행하면 `psql`로 연결하는 데 사용할 수 있는 옵션에 대한 자세한 정보가 제공됩니다.

- `--h` — `--host=호스트 이름` | 데이터베이스 서버 호스트 또는 소켓 디렉토리(기본값: "local socket")
- `--p` — `--port=PORT` | 데이터베이스 서버 포트(기본값: "5432")
- "--U" — "--➡="사용자 이름` | 데이터베이스 사용자 이름(기본값: "your_username")
- `--w` — `--비밀번호 없음` | 비밀번호를 묻지 않음
- `--W` — `--password` | 암호 강제 프롬프트(자동으로 발생)

옵션 플래그가 없는 기본 로그인 정보로 기본 `postgres` 데이터베이스에 연결합니다.

```undefined
psql postgres
```

우리가 새로운 연결을 시작했다는 것을 알 수 있을 것이다. 우리는 지금 postgres 데이터베이스의 psql 안에 있다. 프롬프트는 슈퍼 사용자 또는 루트로 로그인했음을 나타내는 `#`으로 끝납니다.

```undefined
postgres=#
```

`psql` 내의 명령은 백슬래시(`\`)로 시작합니다. 첫 번째 명령을 테스트하기 위해 `\conninfo` 명령을 사용하여 연결한 데이터베이스, 사용자 및 포트를 확인할 수 있습니다.

```coffeescript
postgres=# \conninfo
You are connected to database "postgres" as user "your_username" via socket in "/tmp" at port "5432".
```

다음은 이 튜토리얼에서 사용할 몇 가지 일반적인 명령의 참조 표입니다.

- `\q` | `psql` 연결 종료
- `\c` | 새 데이터베이스에 연결
- `\dt` | 모든 테이블 나열
- `\du` | 모든 역할 나열
- \list | 데이터베이스 나열

슈퍼 사용자 권한이 있는 기본 계정을 사용하지 않도록 새 데이터베이스와 사용자를 생성하겠습니다.

## Postgres에서 역할 생성

먼저 `나`라는 역할을 만들어 비밀번호부터 알려드리겠습니다. 역할은 사용자 또는 그룹으로 작동할 수 있으므로, 이 경우 해당 역할을 사용자로 사용합니다.

```undefined
postgres=# CREATE ROLE me WITH LOGIN PASSWORD 'password';
```

우리는 `나`가 데이터베이스를 만들 수 있기를 바란다.

```undefined
postgres=# ALTER ROLE me CREATEDB;
```

\du를 실행하여 모든 역할/사용자를 나열할 수 있습니다.

```undefined
me          | Create DB                           | {}
postgres    | Superuser, Create role, Create DB   | {}
```

이제 `me` 사용자로부터 데이터베이스를 생성하려고 합니다. 종료할 경우 기본 세션에서 "\q"로 종료합니다.

```undefined
postgres=# \q
```

컴퓨터의 기본 터미널 연결로 돌아왔습니다. 이제 postgres와 me를 연결하겠습니다.

```undefined
psql -d postgres -U me
```

postgres=#postgres 프롬프트 대신 postgres=>라는 메시지가 표시됩니다. 즉, 더 이상 수퍼유저로 로그인하지 않습니다.

## Postgres에서 데이터베이스 만들기

SQL 명령을 사용하여 데이터베이스를 생성할 수 있습니다.

```undefined
postgres=> CREATE DATABASE api;
```

사용 가능한 데이터베이스를 보려면 `\list` 명령을 사용하십시오.

```undefined
Name    |    Owner    | Encoding |   Collate   |    Ctype    |
api     | me          | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
```

`\c`(connect) 명령을 사용하여 `me`로 새 `api` 데이터베이스에 연결

```undefined
postgres=> \c api
You are now connected to database "api" as user "me".
api=>
```

이제 프롬프트에 `api`에 연결되었음을 표시합니다.

## Postgres에서 테이블 만들기

psql 명령 프롬프트에서 마지막으로 수행할 작업은 VARCAR 유형 2개와 PARTIC KEY ID의 3개 필드가 있는 사용자 테이블을 만드는 것입니다.

```undefined
api=>
CREATE TABLE users (
  ID SERIAL PRIMARY KEY,
  name VARCHAR(30),
  email VARCHAR(30)
);
```

> Postgre에서 테이블을 만들고 작업할 때 백 체크(') 문자를 사용하지 마십시오.SQL. MySQL에서는 백틱이 허용되지만 Postgre에서는 유효하지 않습니다.SQL. 또한 'CREATE TABLE' 명령에는 후행 쉼표가 없어야 합니다.

사용할 데이터가 있는 `사용자`에 두 개의 항목을 추가합니다.

```undefined
INSERT INTO users (name, email)
  VALUES ('Jerry', 'jerry@example.com'), ('George', 'george@example.com');
```

사용자`의 모든 항목을 가져와 올바르게 추가되었는지 확인합니다.

```undefined
api=> SELECT * FROM users;
id |  name  |       email        
----+--------+--------------------
  1 | Jerry  | jerry@example.com
  2 | George | george@example.com
```

이제 사용자, 데이터베이스, 테이블 및 일부 데이터가 있습니다. Node.js RESTful API를 구축하여 Postgre에 저장된 이 데이터에 연결할 수 있습니다.SQL 데이터베이스.

이제 모든 Postgre를 마칩니다.SQL 작업. Node.js 앱과 Express 서버 설정을 시작할 수 있습니다.

## Express.js 서버 설정

Node.js 앱 및 Express.js 서버를 설정하려면 프로젝트가 상주할 디렉토리를 생성합니다.

```bash
mkdir node-api-postgres
cd node-api-postgres
```

npm in-y를 실행하여 패키지를 생성할 수 있습니다.json 또는 아래 코드를 복사하여 `crape`로 만듭니다.json의 파일

```undefined
{
  "name": "node-api-postgres",
  "version": "1.0.0",
  "description": "RESTful API with Node.js, Express, and PostgreSQL",
  "main": "index.js",
  "license": "MIT"
}
```

서버에 Express.js를 설치하고 Postgre에 연결할 노드-postgres(pg)를 설치하려고 합니다.SQL입니다.

```coffeescript
npm i express pg
```

이제 종속성이 `node_modules`와 `package`에 로드됩니다.제이슨의

서버의 진입점으로 사용할 `index.js` 파일을 만듭니다. 맨 위에서는 바디파서 미들웨어에 내장된 익스프레스 모듈을 요구하고 앱과 포트 변수를 설정한다.

```php
const express = require('express')
const bodyParser = require('body-parser')
const app = express()
const port = 3000

app.use(bodyParser.json())
app.use(
  bodyParser.urlencoded({
    extended: true,
  })
)
```

루트(`/`) URL에서 `GET` 요청을 찾고 일부 JSON을 반환하는 경로를 알려드립니다.

```coffeescript
app.get('/', (request, response) => {
  response.json({ info: 'Node.js, Express, and Postgres API' })
})
```

이제 설정한 포트에서 수신하도록 앱을 설정합니다.

```coffeescript
app.listen(port, () => {
  console.log(`App running on port ${port}.`)
})
```

명령줄에서 `index.js`를 눌러 서버를 시작할 수 있습니다.

```undefined
node index.js
App running on port 3000.
```

브라우저의 URL 표시줄에 있는 "http://localhost:3000"으로 이동하면 앞에서 설정한 JSON이 표시됩니다.

```css
{
  info: "Node.js, Express, and Postgres API"
}
```

Express 서버가 지금 실행 중이지만 우리가 생성한 일부 정적 JSON 데이터만 보냅니다. 다음 단계는 Postgre에 연결하는 것입니다.동적 쿼리를 만들 수 있는 Node.js의 SQL입니다.

## Node.js에서 Postgres 데이터베이스에 연결

노드-포스트그레스 모듈을 사용하여 연결 풀을 만들 것입니다. 이렇게 하면 질의를 할 때마다 클라이언트를 열고 닫을 필요가 없습니다.

> Postgre의 경량 연결 풀러인 pgBouncer를 사용하는 것이 일반적인 생산 풀링 옵션이다.SQL입니다.

`query.js`라는 파일을 만들고 Postgre의 구성을 설정합니다.SQL 연결.

```js
const Pool = require('pg').Pool
const pool = new Pool({
  user: 'me',
  host: 'localhost',
  database: 'api',
  password: 'password',
  port: 5432,
})
```

프로덕션 환경에서는 버전 제어에서 액세스할 수 없는 제한된 사용 권한이 있는 별도의 파일에 구성 세부 정보를 저장하려고 하지만 이 튜토리얼의 단순성을 위해 쿼리와 동일한 파일에 보관합니다.

이 튜토리얼의 목적은 해당 데이터베이스 명령을 실행하는 API에서 CRUD 작업(GET, POST, PUT, DELETE)을 허용하는 것이다. 이를 위해 각 끝점에 대한 경로와 각 쿼리에 해당하는 함수를 설정합니다.

## CRUD 작업을 위한 경로 생성

우리는 아래와 같이 6개 노선에 대해 6개의 기능을 만들 것입니다.

먼저 각 경로에 대한 모든 기능을 만듭니다. 그런 다음 액세스할 수 있도록 기능을 내보냅니다.

- GET — / | 디스플레이 홈()
- GET — /users | GetUsers()
- `GET` — `/users/:id` | `getUserById()`
- POST — 사용자 | 사용자 생성()
- `PUT` — `/users/:id` | `updateUser()
- `DELETE` — `/users/:id` | `DeleteUser()

index.js에서는 함수를 포함하는 루트 끝점에 대한 app.get()을 만들었다. 이제 `query.js`에서는 모든 사용자를 표시하고, 단일 사용자를 표시하고, 새 사용자를 만들고, 기존 사용자를 업데이트하고, 사용자를 삭제하는 엔드포인트를 만들 것입니다.

### 모든 사용자 'GET'

우리의 첫 번째 끝점은 `GET` 요청이 될 것이다. 수영장 안에요.query() `api` 데이터베이스를 터치할 원시 SQL을 넣을 수 있습니다. 모든 사용자를 `선택`하고 ID별로 주문하겠습니다.

```coffeescript
const getUsers = (request, response) => {
  pool.query('SELECT * FROM users ORDER BY id ASC', (error, results) => {
    if (error) {
      throw error
    }
    response.status(200).json(results.rows)
  })
}
```

### ID별 단일 사용자 'GET'

우리의 `/users/:id` 요청에 대해, 우리는 URL을 통해 사용자 정의 `id` 매개 변수를 얻고 결과를 표시하기 위해 `WHERE` 절을 사용하여 결과를 표시합니다.

SQL 조회에서는 `id=$1`을 찾고 있습니다. 이 경우 `$1`은 번호가 매겨진 자리 표시자이며, 이 자리 표시자는 Postgre입니다.SQL은 다른 종류의 SQL에서 익숙한 자리 표시자 대신 기본적으로 사용됩니다.

```coffeescript
const getUserById = (request, response) => {
  const id = parseInt(request.params.id)

  pool.query('SELECT * FROM users WHERE id = $1', [id], (error, results) => {
    if (error) {
      throw error
    }
    response.status(200).json(results.rows)
  })
}
```

### 새 사용자 'POST'

< API는 GET 및 POST 요청을 /users 끝점으로 가져옵니다. POST 요청에 새 사용자를 추가합니다. 이 기능에서는 요청 본문에서 이름 및 이메일 속성을 추출하고 값을 삽입하는 INSERT를 추출합니다.

```coffeescript
const createUser = (request, response) => {
  const { name, email } = request.body

  pool.query('INSERT INTO users (name, email) VALUES ($1, $2)', [name, email], (error, results) => {
    if (error) {
      throw error
    }
    response.status(201).send(`User added with ID: ${result.insertId}`)
  })
}
```

### 기존 사용자의 'PUT' 업데이트 데이터

또한 /users/:id 끝점은 기존 사용자를 수정하기 위해 getUserById에 대해 생성한 GET와 PUT의 두 가지 HTTP 요청을 사용합니다. 이 질의에서는 GET와 POST에서 배운 내용을 결합하여 UPDATE 절을 사용할 것입니다.

PUT는 같은 통화를 반복할 수 있고 같은 결과를 낼 수 있다는 뜻에서 이상하다는 점을 주목할 필요가 있다. 똑같은 호출을 반복하면 같은 데이터를 가진 새로운 사용자가 계속 생겨나는 POST와는 다르다.

```coffeescript
const updateUser = (request, response) => {
  const id = parseInt(request.params.id)
  const { name, email } = request.body

  pool.query(
    'UPDATE users SET name = $1, email = $2 WHERE id = $3',
    [name, email, id],
    (error, results) => {
      if (error) {
        throw error
      }
      response.status(200).send(`User modified with ID: ${id}`)
    }
  )
}
```

### 사용자 '삭제'

마지막으로 `/users/:id`에 대한 `DELETE` 절을 사용하여 ID별로 특정 사용자를 삭제합니다. 이 통화는 "getUserById() 기능과 매우 유사합니다.

```coffeescript
const deleteUser = (request, response) => {
  const id = parseInt(request.params.id)

  pool.query('DELETE FROM users WHERE id = $1', [id], (error, results) => {
    if (error) {
      throw error
    }
    response.status(200).send(`User deleted with ID: ${id}`)
  })
}
```

## REST API에서 CRUD 기능 내보내기

이러한 기능에 `index.js`에서 액세스하려면 해당 기능을 내보내야 합니다. 우리는 `module.exports`로 이것을 할 수 있고, 기능의 객체를 만들 수 있다. ES6 구문을 사용하고 있기 때문에 getUsers:getUsers 등이 아니라 getUsers라고 쓸 수 있습니다.

```java
module.exports = {
  getUsers,
  getUserById,
  createUser,
  updateUser,
  deleteUser,
}
```

여기 완전한 `query.js` 파일이 있습니다.

```coffeescript
const Pool = require('pg').Pool
const pool = new Pool({
  user: 'me',
  host: 'localhost',
  database: 'api',
  password: 'password',
  port: 5432,
})
const getUsers = (request, response) => {
  pool.query('SELECT * FROM users ORDER BY id ASC', (error, results) => {
    if (error) {
      throw error
    }
    response.status(200).json(results.rows)
  })
}

const getUserById = (request, response) => {
  const id = parseInt(request.params.id)

  pool.query('SELECT * FROM users WHERE id = $1', [id], (error, results) => {
    if (error) {
      throw error
    }
    response.status(200).json(results.rows)
  })
}

const createUser = (request, response) => {
  const { name, email } = request.body

  pool.query('INSERT INTO users (name, email) VALUES ($1, $2)', [name, email], (error, results) => {
    if (error) {
      throw error
    }
    response.status(201).send(`User added with ID: ${result.insertId}`)
  })
}

const updateUser = (request, response) => {
  const id = parseInt(request.params.id)
  const { name, email } = request.body

  pool.query(
    'UPDATE users SET name = $1, email = $2 WHERE id = $3',
    [name, email, id],
    (error, results) => {
      if (error) {
        throw error
      }
      response.status(200).send(`User modified with ID: ${id}`)
    }
  )
}

const deleteUser = (request, response) => {
  const id = parseInt(request.params.id)

  pool.query('DELETE FROM users WHERE id = $1', [id], (error, results) => {
    if (error) {
      throw error
    }
    response.status(200).send(`User deleted with ID: ${id}`)
  })
}

module.exports = {
  getUsers,
  getUserById,
  createUser,
  updateUser,
  deleteUser,
}
```

## REST API에서 CRUD 기능 설정

이제 모든 쿼리가 확보되었으므로 마지막으로 해야 할 일은 해당 쿼리를 `index.js` 파일로 끌어와 우리가 만든 모든 쿼리 기능의 끝점 경로를 만드는 것이다.

내보낸 모든 함수를 `query.js`에서 가져오려면 파일을 `요구`하여 변수에 할당합니다.

```js
const db = require('./queries')
```

이제 각 끝점에 대해 HTTP 요청 방법, 끝점 URL 경로 및 관련 기능을 설정합니다.

```undefined
app.get('/users', db.getUsers)
app.get('/users/:id', db.getUserById)
app.post('/users', db.createUser)
app.put('/users/:id', db.updateUser)
app.delete('/users/:id', db.deleteUser)
```

API 서버의 진입점인 전체 `index.js`입니다.

```coffeescript
const express = require('express')
const bodyParser = require('body-parser')
const app = express()
const db = require('./queries')
const port = 3000

app.use(bodyParser.json())
app.use(
  bodyParser.urlencoded({
    extended: true,
  })
)

app.get('/', (request, response) => {
  response.json({ info: 'Node.js, Express, and Postgres API' })
})

app.get('/users', db.getUsers)
app.get('/users/:id', db.getUserById)
app.post('/users', db.createUser)
app.put('/users/:id', db.updateUser)
app.delete('/users/:id', db.deleteUser)

app.listen(port, () => {
  console.log(`App running on port ${port}.`)
})
```

이제 이 두 개의 파일만으로도 서버, 데이터베이스 및 API가 모두 설정되었습니다. index.js를 다시 눌러 서버를 시작할 수 있습니다.

```undefined
node index.js
App running on port 3000.
```

이제 "http://localhost:3000/users" 또는 "http://localhost:3000/users/1"로 이동하면 두 "GET" 요청의 JSON 응답이 표시됩니다. 그러나 우리의 POST, PUT, DELETE 요청을 어떻게 시험할 수 있을까?

이 작업은 터미널에서 이미 사용할 수 있는 명령줄 도구인 컬로 수행할 수 있습니다. 다음은 명령줄에서 실행하여 모든 프로토콜을 테스트할 수 있는 예입니다.

> 별도의 창에서 이러한 명령을 실행하는 동안 서버가 하나의 터미널 창에서 실행 중인지 확인하십시오.

### '포스트'

이름 일레인(Elaine)과 e-mail(이메일) elaine@example.com을 포함한 새 사용자를 추가합니다.

```undefined
curl --data "name=Elaine&email=elaine@example.com" 
http://localhost:3000/users
```

### 'PUT

사용자를 ID `1`로 업데이트하여 "이름" 크래머와 "이메일" kramer@example.com을 갖도록 합니다.

```undefined
curl -X PUT -d "name=Kramer" -d "email=kramer@example.com" 
http://localhost:3000/users/1
```

### '삭제

ID가 `1`인 사용자를 삭제합니다.

```cpp
curl -X "DELETE" http://localhost:3000/users/1
```

## 결론

축하합니다. 이제 작동 중인 API 서버가 Node.js에서 실행되고 활성 Postgre에 연결되어 있어야 합니다.SQL 데이터베이스. 이 튜토리얼에서는 Postgre 설치 및 설정 방법에 대해 배웠습니다.명령줄의 SQL, 사용자, 데이터베이스 및 테이블을 생성하는 방법 및 SQL 명령 실행 방법. 또한 여러 HTTP 방법을 처리할 수 있는 Express 서버를 생성하는 방법과 Pg 모듈을 사용하여 Postgre에 연결하는 방법도 배웠습니다.노드의 SQL입니다.

이러한 지식을 바탕으로 이 API를 구축하여 개인 또는 전문 개발 프로젝트에 활용할 수 있어야 합니다.