---
layout: post
title: "Node.js 및 AdminBro를 사용하여 관리 패널 생성"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/markus-winkler-IrRbSND5EUc-unsplash.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/markus-winkler-IrRbSND5EUc-unsplash.png?fit=730%2C487&ssl=1)

웹 애플리케이션은 다양한 소스의 데이터를 지속적으로 처리합니다. 수많은 데이터베이스 및 저장소에서 컨텐츠 스트림이 들어오고 이러한 모든 레코드를 추적하는 것은 관리자에게 빠르게 문제가 될 수 있습니다.

AdminBro는 단일 패널에서 모든 데이터를 관리할 수 있는 통합 관리 인터페이스를 제공하여 이 문제를 해결하려고 노력합니다.

## AdminBro란?

AdminBro는 자동으로 생성된 관리 패널을 Node.js 응용 프로그램에 추가하는 Software Brothers의 오픈 소스 패키지입니다.

다양한 데이터베이스를 관리 인터페이스에 연결하고 레코드에서 표준 CRUD 작업(작성, 읽기, 업데이트, 삭제)을 수행할 수 있습니다. 이렇게 하면 여러 소스에 걸쳐 앱 데이터를 찾고, 모니터링하고, 업데이트하는 기능이 크게 간소화 및 확장됩니다.

## 이 튜토리얼 정보

이 문서에서는 처음부터 끝까지 앱에 AdminBro 관리 패널을 추가하는 방법을 보여 줍니다. 다음은 다루게 될 주제입니다.

- 피어 종속성 설치 및 프레임워크 플러그인을 사용하여 AdminBro 설치
- 라우터 미들웨어 생성 및 설정
- 리소스 데이터베이스 연결
- 관리 패널 확인 및 열기

이 튜토리얼을 마치면 앱 데이터 관리를 시작하는 데 사용할 수 있는 관리 인터페이스가 제공됩니다.

## 전제조건

다음 단계를 완료하려면 다음 구성 요소를 포함하는 표준 웹 개발 스택이 필요합니다.

- Node.js에 대한 웹 프레임워크입니다. AdminBro는 다음과 같은 일반적인 프레임워크에서 작동합니다.
익스프레스
하피
코아
- 익스프레스
- 하피
- 코아
- ODM/ORM이 있는 리소스 데이터베이스입니다. AdminBro는 현재 다음과 같은 ODM 및 ORM을 지원합니다.
몽고DB용 몽구스
Postgres, MySQL, MariaDB, SQLite 및 Microsoft SQL Server의 후속 버전
MySQL용 ORM, Postgre 입력SQL, MariaDB, SQLite, Microsoft SQL Server, Oracle, SAP Hana 및 WebSQL
- 몽고DB용 몽구스
- Postgres, MySQL, MariaDB, SQLite 및 Microsoft SQL Server의 후속 버전
- MySQL용 ORM, Postgre 입력SQL, MariaDB, SQLite, Microsoft SQL Server, Oracle, SAP Hana 및 WebSQL

갈 준비 완료? 슬슬 출발 해야지.

## 관리 패널 만들기

이 튜토리얼에서는 Mongoose ORM을 사용하여 Express 프레임워크와 MongoDB를 사용하여 관리 패널을 설정하는 예제 단계를 단계별로 살펴보겠습니다. 목표는 AdminBro 설치 프로세스의 일반적인 요지를 제공하는 것입니다. 필요에 따라 이러한 지침을 다른 프레임워크 및 리소스 모델에 쉽게 적용할 수 있습니다.

### 1. 프레임워크 플러그인을 사용하여 AdminBro 설치

먼저 AdminBro 프레임워크 플러그인의 피어 종속성인 관련 모듈과 함께 프레임워크를 설치했는지 확인하십시오. 우리의 경우 양식 데이터를 구문 분석하기 위해 Express 및 Express-moughable 모듈을 설치하려고 합니다.

```coffeescript
npm install express express-formidable
```

이제 Express 플러그인을 사용하여 AdminBro 패키지를 설치할 수 있습니다.

```css
npm install admin-bro @admin-bro/express
```

AdminBro는 Express 지원 외에도 Hapi 및 Koa 플러그인을 위한 설명서 페이지가 있으며, 이러한 프레임워크 중 하나를 사용하려면 설정 코드 작성에 대한 지침을 제공합니다. 또한 피어 종속성으로 설치해야 하는 패키지 및 모듈을 정확하게 알려줍니다.

### 2. 라우터 미들웨어 구축 및 설정

이제 AdminBro 트래픽을 처리하기 위한 Express 라우터를 생성하겠습니다. 다음 코드에서 트릭을 수행합니다.

```js
const AdminBro = require(‘admin-bro’)
const AdminBroExpress = require(‘@admin-bro/express’)

const express = require(‘express’)
const app = express()

const adminBro = new AdminBro ({
    Databases: [],
    rootPath: ‘/admin’,
})

const router = AdminBroExpress.buildRouter (adminBro)
```

다음 단계는 Express.js 앱 개체를 사용하여 라우터를 미들웨어로 설정하는 것입니다.

```coffeescript
app.use(adminBro.options.rootPath, router)
app.listen(8080, () => console.log(‘AdminBro is under localhost:8080/admin’))
```

> 팁: 기존 미들웨어 스택이 있는 앱에서 이 설정을 수행하는 것이 좋습니다. AdminBro를 맨 위에 올려놓기만 하면 됩니다. 이렇게 하면 AdminBro는 다른 미들웨어에 의해 처리되고 변환된 요청을 처리할 수 없기 때문에 관리 패널이 정상적으로 작동합니다.

### 3. 리소스 연결

이제 리소스 데이터베이스를 연결하여 관리 패널을 데이터로 시드해야 합니다. 앱에서 데이터를 가져오는 각 ODM/ORM에 대해 이 설정을 반복합니다.

여기에서는 Mongoose에 대한 예제 단계와 코드를 보여드리겠습니다. Suppledize 또는 TypeORM과 같이 지원되는 다른 리소스 모델을 연결하려는 경우 해당 ORM에 대한 설명서 페이지의 지침을 따라야 합니다.

먼저 Mongoose용 AdminBro의 데이터베이스 어댑터를 설치합니다.

```css
npm install @admin-bro/mongoose
```

그런 다음 프로젝트에서 사용할 수 있도록 어댑터를 등록합니다.

```js
const AdminBro = require(‘admin-bro’)
const AdminBroMongoose = require(‘@admin-bro/mongoose’)

AdminBro.registerAdapter(AdminBroMongoose)
```

이제 전체 데이터베이스를 `AdminBro({})` 옵션으로 전달할 준비가 되었습니다. 데이터베이스 전체를 전달할 때도 모델 경로를 프로젝트 범위에 포함시켜야 하는 방법을 참고하십시오.

```js
const mongoose = require(‘mongoose’)

require(‘path-to-your/mongoose-model1’)
require(‘path-to-your/mongoose-model2’)

const run = async () => {
    const connection = await mongoose.connect(‘mongodb://localhost:27017/test’, {
        useNewUrlParser: true,
    })
    const AdminBro = new AdminBro ({
        Database: [connection]
    }]
}
run()
```

> 팁: 데이터베이스를 'AdminBro({})' 옵션으로 리소스를 전달하기 전에 항상 데이터베이스에 대한 연결을 설정하십시오. 이렇게 하면 AdminBro에서 리소스를 찾을 수 있습니다.

전체 데이터베이스를 AdminBro에 전달하는 대신 리소스를 개별적으로 전달할 수도 있습니다. 이 대안을 사용하면 다양한 리소스 옵션 범위를 보다 효과적으로 제어할 수 있습니다. 문서를 읽으면 개별 리소스를 전달하는 방법을 배울 수 있습니다.

### 4. 관리자 패널을 확인하고 엽니다.

이 때 기본적으로 관리 인터페이스를 구축했습니다. 모든 작업을 올바르게 수행했는지 확인하려면 먼저 데이터베이스가 가동되었는지 확인한 후 서버를 실행하십시오.

```css
node index.js
```

앱 터미널에 이 콘솔 로그가 표시되면 홈 프리 상태임을 알 수 있습니다.

```coffeescript
AdminBro is under localhost:8080/admin
AdminBro: bundle ready
```

이제 `http://localhost:8080/admin`으로 이동하여 AdminBro에 액세스할 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/AdminBro-Local-Host.png?resize=730%2C454&ssl=1)

축하합니다. 사용자 자신의 관리 패널 만들기를 마쳤습니다!

## 요약

요약하면, 처음부터 관리 패널을 작성하는 단계는 다음과 같습니다.

- 피어 종속성 설치(필요한 모듈이 있는 프레임워크 패키지)
- 프레임워크 플러그인을 사용하여 AdminBro 설치
- 라우터를 빌드하고 미들웨어로 설정
- 데이터베이스 어댑터 설치 및 등록
- 관리 Broo에 리소스 전달
- 서버를 실행하고 콘솔 로그 확인
- 포트 8080에서 관리 패널을 엽니다.

이 단계를 완료하면 앱 데이터를 보고 조작하는 데 사용할 수 있는 단일 통합 대시보드가 생성됩니다.

## 다음 단계

새 관리 패널을 가동하면 관리브로가 제공해야 하는 다양한 기능을 탐색할 수 있습니다. 다음과 같은 몇 가지 작업만 수행할 수 있습니다.

- 리소스 모양 사용자 정의
- 양식 필드 확인
- 선택한 리소스에 작업 후크 설정
- 위젯을 사용하여 대시보드 사용자 지정
- 역할 기반 액세스 제어 구성
- 외부 기능 패키지를 추가하여 AdminBro 기능 확장

자세한 내용은 관리 브로스 문서를 참조하십시오.