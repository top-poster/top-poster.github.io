---
layout: post
title: "CRUD 요청에 대해 AdonisJs에서 Cloud Firestore 구성"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/configuring-cloud-firestore-adonisjs-crud.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/configuring-cloud-firestore-adonisjs-crud.png?fit=730%2C487&ssl=1)

AdonisJs는 마이크로 서비스 쓰기를 위해 특별히 제작된 Node.js 프레임워크이다. 많은 사람들은 이 프레임워크가 PHP(Laravel)에서 JavaScript로 마이그레이션하는 개발자들을 위한 최고의 자바스크립트 프레임워크라고 생각한다.

대부분의 Node.js 프레임워크와 마찬가지로 AdonisJs는 내장 CLI를 가지고 있으며 기본적으로 MVC 패턴을 지원한다. AdonisJs는 Postgres와 같은 SQL 데이터베이스와 Firebase와 같은 SQL 데이터베이스를 모두 지원하며, 이 튜토리얼의 초점이 될 것이다.

Cloud Firestore는 Firebase 및 Google Cloud Platform의 모바일, 웹 및 서버 개발을 위한 유연하고 확장 가능한 데이터베이스입니다. Firebase Realtime Database와 마찬가지로 실시간 수신기를 통해 클라이언트 앱 간에 데이터 동기화를 유지하고 모바일 및 웹에 대한 오프라인 지원을 제공하므로 네트워크 지연 시간이나 인터넷 연결과 관계없이 응답 가능한 앱을 구축할 수 있습니다.

### 도입 및 전제 조건

이 튜토리얼에서는 다음을 수행합니다.

- 새 Google Cloud Firestore 생성 및 구성
- 새 AdonisJs 응용 프로그램 설치 및 설정
- 데이터베이스 스토리지에 대해 AdonisJs에서 Firestore 구성
- CRUD API 요청 만들기
- 응용 프로그램 테스트

이 튜토리얼을 따르기 위해서는 자바스크립트 및 MVC 아키텍처에 대한 기본적인 지식이 중요하다. 시작하기 전에 이 프로젝트에 대한 레포도 확인하실 수 있습니다.

## Google Cloud Firestore 생성 및 구성

시작하려면 Firebase 콘솔로 이동하여 새 프로젝트를 만드십시오. 내 이름을 도니스Js-fire store로 지을 거야

그런 다음 Cloud Firestore 탭으로 이동하여 새 데이터베이스를 생성합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/create-database-cloud-firestore.jpeg?resize=730%2C224&ssl=1)

이 작업이 완료되면 Firestore에서 새 컬렉션을 만들도록 요구할 것입니다. 이는 단순한 문서 목록입니다. 예를 들어, 우리는 모든 사용자의 정보를 저장하는 `사용자` 컬렉션을 가질 수 있으며, 각 컬렉션은 문서로 표시됩니다. 내 경우 이름과 전자 메일을 필드로 새 `사용자` 컬렉션을 만듭니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/starting-new-collection-screen.png?resize=730%2C516&ssl=1)

## AdonisJs 설치 및 설정

아도니스Js는 노드를 사용하여 구축되었으므로 npm을 사용하여 설치하고 서비스할 수 있습니다. 설치 지침은 여기에서 확인할 수 있습니다. 다음 명령을 사용하여 AdonisJs CLI를 설치합니다.

```coffeescript
npm i -g adonisjs/cli
```

AdonisJs CLI를 전체적으로 설치하지 않으려면 위의 명령에서 `-g` 플래그를 제거하십시오.

그런 다음 다음 명령을 사용하여 프로젝트 이름이 donisJs-firestore인 새 AdonisJs 애플리케이션을 생성합니다.

```coffeescript
adonis new adonisJs-firestore
```

AdonisJs 애플리케이션 설치가 완료되면 다음을 사용하여 애플리케이션을 지원할 수 있습니다.

```undefined
adonis serve --dev
```

이렇게 하면 `http://127.0.0.1:333`에 로컬 포트가 열립니다.

## AdonisJs에서 Firestore 구성

Firestore가 AdonisJs에서 작동하도록 하려면 다음 명령을 사용하여 Firebase admin SDK를 AdonisJs 응용 프로그램에 설치해야 합니다.

```undefined
npm install firebase-admin --save
```

설치가 성공하면 `패키지`에 추가됩니다.json` 파일(아래 참조):

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/firebase-admin-installed.jpeg?resize=730%2C298&ssl=1)

### Firestore 모델 생성

파이어스토어 데이터 모델을 `app/Models/Firestore.js` 파일에 저장할 예정이니 모델 파일을 생성해 봅시다.

```java
const Model = use('Model');

const admin = require('firebase-admin');

class Firestore extends Model {

}

module.exports = Firestore;
```

위에 우리는 새로운 모델을 만들었고 이전에 설치한 Firebase admin SDK가 필요했습니다. 다음 프로세스는 Firestore를 초기화하고 서비스 계정 자격 증명을 얻는 것입니다.

### Firestore 초기화 중

```java
const Model = use('Model');

const admin = require('firebase-admin');

admin.initializeApp({
  credential: admin.credential.applicationDefault()
});

const db = admin.firestore();

class Firestore extends Model {

}

module.exports = Firestore;
```

위에서 관리 값을 갖는 상수 변수 `db`를 생성했다.소방서 특정 Google 서비스에서 이 통화를 사용할 것입니다.

따라서 이 시점에서는 Firestore를 초기화했지만, Firestore가 어떤 서비스를 가리키는지 알 수 있는 애플리케이션 자격 증명을 아직 추가하지 못했습니다. 우리는 또한 애플리케이션에 대한 사용자 허가를 받아야 합니다.

이제 Firebase 콘솔에 있는 Settings > Service 계정으로 이동하여 서비스 계정 자격 증명을 얻습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/generating-private-key.png?resize=730%2C374&ssl=1)

그런 다음 Create new private key 버튼을 클릭하여 서비스 계정 자격 증명이 포함된 JSON 파일을 다운로드합니다. 다운로드한 JSON 파일을 AdonisJs 프로젝트 폴더에 복사하여 폴더 루트 디렉터리에 배치합니다.

이 작업이 완료되면 Firestore 모델에 다운로드한 서비스 계정 인증 JSON 파일이 필요하며 이 인증 정보를 Firestore 초기화에 추가합니다.

```js
const Model = use('Model');

const admin = require('firebase-admin');

const serviceAccount = require("../../adonisjs-firestore-firebase-adminsdk-ajfbl-6a02cfaeb1.json");

admin.initializeApp({
  credential: admin.credential.cert(serviceAccount),
  databaseURL: "https://adonisjs-firestore.firebaseio.com"
});

class Firestore extends Model {

  // call on firestore
  db() {
  return admin.firestore();
  }
}

module.exports = Firestore;
```

몇 가지 유의할 사항:

- 데이터베이스URL`은 필수 사항. 올바른 데이터베이스 URL을 가리켜야 합니다.
- 서비스 계정이 자격 증명이 포함된 JSON 파일을 올바르게 가리키는지 확인합니다.

## CRUD API 요청 중

우리는 우리의 AdonisJs 응용 프로그램에 Firestore 설치와 초기화를 마쳤습니다. 이제 CRUD 요청을 작성하겠습니다. 그러기 위해서는 다음을 수행해야 합니다.

- CRUD 요청을 포함할 사용자 컨트롤러 생성
- 컨트롤러의 Firestore 모델에 전화하십시오.
- API가 컨트롤러에 연결할 수 있는 경로 생성

새 사용자 컨트롤러를 생성하려면 다음 명령을 사용합니다.

```bash
adonis make:controller UserController --type http
```

새 파일인 `UserController.js`가 생성됩니다.

```js
// app/Controllers/Http/UserController.js
'use strict';

class UserController {
}

module.exports = UserController;
```

이 작업이 완료되면 컨트롤러의 Firestore 모델을 호출하여 다음과 같은 요청에 admin SDK를 사용합니다.

```js
'use strict';

const Firestore = use('App/Models/Firestore');
const firestore = new Firestore;
const db = firestore.db();

class UserController {
}

module.exports = UserController;
```

위에서, 우리는 우리의 모델에 전화를 한 다음, `db` 기능을 사용하여 우리의 소방서 서비스를 정확히 찾아냈다. 다음으로, 데이터를 가져올 Firestore의 컬렉션을 가리킵니다.

```js
'use strict';

const Firestore = use('App/Models/Firestore');
const firestore = new Firestore;
const db = firestore.db();

// Reference to
const userReference = db.collection('users');

class UserController {
}

module.exports = UserController;
```

"collection"은 Firestore에서 데이터를 가져오려는 특정 컬렉션을 가리킵니다. 이 경우 "사용자"입니다.

이제 CRUD 기능을 만들어 보겠습니다.

### 1. 만들기

요청을 작성하기 위해 사용자 제어기(UserController)에서 sync 함수를 생성한 다음 내장된 검증기(Validator) 함수를 호출하여 요청 매개 변수가 설정되었는지 확인합니다.

```undefined
// app/Controllers/Http/UserController.js
const { validate } = use('Validator');

class UserController {

  async create({ request, response}) {

    const rules = {
      name: 'required',
      email: 'required'
    };

    const data = request.only(['name', 'email']);

    const validation = await validate(request.all(), rules);

    if (validation.fails()) {
      return response.status(206).json({
        status: false,
        message: validation.messages()[0].message,
        data: null
      });
    }
  }
}
```

`Validator`가 작동하려면 다음 명령을 사용하여 설치해야 합니다.

```undefined
adonis install adonisjs/validator
```

다음으로, `start/app.js` 파일에 제공자를 등록해야 합니다.

```undefined
const providers = [
'@adonisjs/validator/providers/ValidatorProvider'
]
```

규칙 필드가 입력되지 않은 경우 검증자는 오류 메시지를 던집니다. 다음으로, Firebase `create` 기능을 추가합니다.

```undefined
let create = await userReference.add({
  name: data.name,
  email: data.email
});

if(create) {
  return response.status(201).json({
    status: true,
    message: 'User created successfully',
    data: null
  });
}
```

위의 코드는 사용자 컬렉션에 있는 Firestore에 새 문서를 만든 다음, 성공한 경우 `201 Created` 상태를 반환합니다.

### 2. 읽기

이제 읽기 요청인 `all`을 만들어 사용자 컬렉션에서 모든 사용자를 가져옵니다.

```js
async all({ response }) {

  let users = [];

  await userReference.get().then((snapshot) => {
    snapshot.forEach(doc => {
      let id = doc.id;
      let user = doc.data();

      users.push({
        id,
        ...user
      });
    })
  });

  return response.status(201).json({
    status: true,
    message: 'All users',
    data: users
  });
}
```

위 코드에서, 우리는 빈 어레이를 만들었고, 우리는 파이어스토어에서 우리의 데이터를 밀어낼 것이다. "또한 사용자 컬렉션을 참조하고 루프를 돌려 ID와 데이터를 사용자 어레이에 밀어 넣었다.

### 3. 업데이트

이제 `사용자` 컬렉션에서 단일 문서를 편집하는 기능을 만듭니다.

```undefined
async update({ request, response }) {

  const rules = {
    id: 'required',
  };

  const data = request.only(['id', 'name', 'email']);

  const validation = await validate(request.all(), rules);

  if (validation.fails()) {
    return response.status(206).json({
      status: false,
      message: validation.messages()[0].message,
      data: null
    });
  }

  let getUser = await userReference.doc(data.id).get();
  let user = getUser.data();

  let update = await userReference.doc(data.id).update({
    name: data.name ? data.name : user.name,
    email: data.email ? data.email : user.email
  });

  if(update) {
    let getUser = await userReference.doc(data.id).get();
    let user = getUser.data();

    return response.status(201).json({
      status: true,
      message: 'User updated successfully',
      data: user
    });
  }
}
```

위 코드에서 우리는 `id`만 검증했는데, 이것은 `name`이나 `e-mail`을 업데이트하지 않도록 선택할 수 있다는 것을 의미한다. 그런 다음 전달된 ID에서 사용자 컬렉션의 단일 사용자 문서를 가져온 다음 추가된 내용에 따라 이름 또는 전자 메일 중 하나를 업데이트합니다.

### 4. 삭제

생성, 읽기 및 업데이트 요청 기능을 성공적으로 생성했습니다. 이제 `사용자` 컬렉션에서 문서를 삭제하는 `삭제` 기능만 추가하면 됩니다.

```undefined
async delete({ request, response }) {

  const rules = {
    id: 'required',
  };

  const data = request.only(['id']);

  const validation = await validate(request.all(), rules);

  if (validation.fails()) {
    return response.status(206).json({
      status: false,
      message: validation.messages()[0].message,
      data: null
    });
  }

  await userReference.doc(data.id).delete();

  return response.status(201).json({
    status: true,
    message: 'User deleted successfully',
    data: null
  });
}
```

우리는 CRUD 요청 작성을 완료했고, 이제 API를 컨트롤러 기능에 연결할 경로를 만듭니다. 그러기 위해, 우리는 우리의 경로 파일 `start/routes`로 간다.js:

```js
// start/routes.js

'use strict'

const Route = use('Route')

Route.post('/create', 'UserController.create');
Route.get('/all', 'UserController.all');
Route.post('/update', 'UserController.update');
Route.post('/delete', 'UserController.delete');
```

이에 따라 사용자 컨트롤러에서 CRUD 기능을 가리키는 경로를 추가하였습니다.

## 응용 프로그램 테스트

테스트를 위해 우리는 우리의 요청에 우체부를 사용할 것입니다. 포스트맨은 개발자들이 API를 쉽게 만들고, 공유하고, 테스트하고, 문서화할 수 있게 해주는 인기 있는 API 클라이언트이다.

계속하기 전에 요청을 처리하기 위해 CSRF(사이트 간 요청 위조) 보호를 활성화해야 합니다. 이렇게 하면 확인되지 않은 요청을 거부하여 CSRF 공격으로부터 애플리케이션을 보호할 수 있습니다.

이를 사용하도록 설정하려면 다음 명령을 사용하여 AdonisJs `shield` 미들웨어를 설치합니다.

```undefined
adonis install adonisjs/shield
```

다음으로, `start/app.js` 파일에 제공자를 등록합니다.

```undefined
const providers = [
'@adonisjs/shield/providers/ShieldProvider'
]
```

`start/kernel.js` 파일에 글로벌 미들웨어를 등록합니다.

```undefined
const globalMiddleware = [
  'Adonis/Middleware/Shield'
]
```

마지막으로 `config/shield.js` 파일을 업데이트하고 `csrf` 개체를 편집합니다.

```bash
csrf: {
  enable: false,
  methods: ['POST', 'PUT', 'DELETE'],
  filterUris: [],
  cookieOptions: {
    httpOnly: false,
    sameSite: true,
    path: '/',
    maxAge: 7200
  }
}
```

이제 우체부에 새 사용자 생성 요청을 추가해 주십시오. 그러면 `사용자 컨트롤러`에서 `생성` 기능이 호출됩니다.

사용자 요청 만들기:

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/create-user-request.png?resize=730%2C569&ssl=1)

모든 사용자 요청 가져오기:

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/get-all-users-request.png?resize=730%2C800&ssl=1)

사용자 요청 업데이트(모든 사용자 요청에서 가져온 문서에서 `id`를 가져옵니다.)

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/update-users-request.png?resize=730%2C374&ssl=1)

사용자 요청 삭제:

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/delete-user-request.png?resize=730%2C380&ssl=1)

## 결론

우리는 데이터 생성, 가져오기, 저장 및 삭제를 위해 아도니스Js와 구글 Firebase Firestore를 사용하는 API 세트를 성공적으로 구축했습니다. 우리는 또한 AdonisJs 프레임워크에서 데이터베이스로 Google Firestore를 설정하는 기본 사항을 검토했다.

이것이 도움이 되었기를 바라며, 이제 당신이 아도니스Js에 파이어스토어를 설치하는 기본 사항을 이해해주길 바랍니다. 계속해서 레포 복제 및 사용자 전용의 경우 구성할 수 있습니다.