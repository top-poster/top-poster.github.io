---
layout: post
title: "Firebase 클라우드 기능, TypeScript 및 Firestore를 포함한 REST API 구축"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/rest-api-firebase-cloud-functions-typescript-firestore.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/rest-api-firebase-cloud-functions-typescript-firestore.png?fit=730%2C487&ssl=1)

Firebase는 서버 없는 앱을 구축하는 선도적인 솔루션 중 하나입니다. 개발자는 처음부터 서버 설정 및 유지 보수에 드는 비용을 걱정하지 않고 확장 가능한 고품질 애플리케이션을 쉽게 개발할 수 있는 방법을 제공합니다. 또한 Firebase는 Google Analytics 및 Firebase 문서 데이터베이스인 Firestore와 같은 Google 서비스와 쉽게 통합할 수 있습니다.

이 문서에서는 Firebase 클라우드 기능, TypeScript 및 Firestore를 사용하여 REST API를 구축하는 방법에 대해 알아봅니다. 데모 앱을 구축하려면 Express.js에 대한 약간의 지식도 필요합니다. 그러나 앞서 언급한 기술 중 어느 기술에서도 프로가 될 필요는 없습니다. 데모 앱은 새 항목을 수락하고 기존 항목을 검색, 업데이트 및 삭제할 수 있는 저널 API입니다.

## 방화벽 설정

앱을 시작하려면 컴퓨터에 Node.js가 설치되어 있어야 합니다. Node.js의 설치 가이드를 보려면 여기를 클릭하십시오. 이미 설치되어 있는 경우 터미널에서 다음 명령을 실행하면 현재 Node.js 버전이 반환됩니다.

```undefined
node --version
```

경고 없이 이 튜토리얼을 따르려면 Firebase 클라우드에 사용할 동일한 버전의 Node.js를 설치해야 합니다. 현재 Firebase SDK는 Node.js 12 및 10을 지원합니다. 우리 앱에 버전 12를 사용하자. Node.js 설치는 npm과 함께 제공되며, 이 설치는 Firebase CLI를 설치하는 데 사용됩니다. 터미널에서 다음 명령을 실행하여 보겠습니다.

```coffeescript
npm install -g firebase-tools
```

Node.js 및 Firebase CLI를 설치하면 브라우저에서 Firebase 콘솔로 이동할 수 있습니다. Google 계정에 로그인해야 합니다. 로그인에 성공하면 다음과 같은 모양의 프로젝트 만들기 단추가 표시됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/firebase-create-a-project-button.png?resize=730%2C322&ssl=1)

이 워크스루(워크스루)를 수행하는 시기에 따라 사용자 인터페이스가 약간 다르게 보일 수 있지만 그래도 다른 방법을 찾을 수 있어야 합니다. Create a project(프로젝트 생성) 버튼을 클릭하면 Firebase project를 쉽게 만드는 프로세스를 볼 수 있습니다.

그런 다음 프로젝트 이름과 ID를 선택할 수 있는 옵션이 제공됩니다. 내 것은 저널-레스트-api를 사용할게요. ID는 프로젝트의 고유 식별자이므로 다른 프로젝트 ID를 선택해야 합니다. 이렇게 하면 Firebase 프로젝트를 성공적으로 만들 수 있습니다.

생성에 성공하면 다음과 같은 프로젝트 개요 페이지가 표시됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/overview-page.png?resize=730%2C304&ssl=1)

### 청구 계획 업그레이드

기본적으로 새 계정은 스파크 무료 요금제에 있어야 합니다. 그러나 Firebase 기능을 사용하려면 청구 계획을 Blaze Pay로 업그레이드해야 합니다. 이 데모에 대한 비용은 걱정하지 않아도 되지만, Firebase에서는 클라우드 기능을 사용하려면 청구 계획을 업그레이드해야 합니다.

청구 계획을 업그레이드하려면 왼쪽 탐색기에서 개발 드롭다운을 열고 기능을 클릭합니다. 기능 페이지에서 청구서를 업그레이드하라는 메시지가 표시됩니다. 메시지는 다음과 같아야 합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/upgrade-project-page.png?resize=730%2C302&ssl=1)

## Firestore 데이터베이스 생성

청구 계획이 업그레이드되면 앱에 대한 Firestore 데이터베이스를 생성하겠습니다. 왼쪽 창의 Cloud Firestore 옵션으로 이동한 다음 Create Database 버튼을 클릭합니다. 다음과 같은 모형이 표시됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/start-in-production-mode.png?resize=730%2C436&ssl=1)

이 데모에서는 Start in production mode 옵션을 사용합니다. 데이터베이스가 생성되었으므로 이제 첫 번째 클라우드 기능을 쓰고 구현할 수 있습니다.

## 첫 번째 클라우드 기능 작성

첫 번째 클라우드 기능을 작성하기 위해 터미널로 돌아가서 다음 명령을 실행하여 Firebase 계정에 로그인합니다.

```undefined
firebase login
```

명령을 달리기는 너의 브라우저에 대한 인증 페이지로 이동하야 한다. 인증에 성공하면 컴퓨터에 프로젝트 디렉토리를 생성하고 해당 디렉토리 내에서 다음 명령을 실행하여 Firebase 기능을 초기화합니다.

```bash
firebase init functions
```

설정에 대한 기본 옵션을 사용할 수 있습니다. 기존 프로젝트 사용 옵션을 선택한 다음 Firebase에서 방금 만든 프로젝트를 선택합니다. 클라우드 기능을 작성하기 위해 어떤 언어를 사용하시겠습니까? TypeScript를 선택합니다. 마지막으로 npm 종속성을 설치하는 옵션을 사용합니다. 모든 것이 잘 되면 다음과 같은 성공 메시지가 표시됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/success-message.png?resize=730%2C245&ssl=1)

새 앱의 프로젝트 구조는 다음과 같습니다.

```css
├── .firebaserc
├── .gitignore
├── .firebase.json
└── functions
    ├── package.json
    ├── tsconfig.json
    ├── .gitignore
    └── src
        ├── index.ts
```

`패키지`에 주목하십시오.json 파일은 `sign` 디렉토리 안에 있습니다. 이 때 우리 `npm` 명령 실행, 우리는 기능을 디렉터리에는 단말기에 이동해야 한다는 것을 의미한다. //functions/src/index.ts 파일을 열면 다음과 같은 내용이 표시됩니다.

```coffeescript
import * as functions from 'firebase-functions';
// // Start writing Firebase Functions
// // https://firebase.google.com/docs/functions/typescript
//
// export const helloWorld = functions.https.onRequest((request, response) => {
//   functions.logger.info("Hello logs!", {structuredData: true});
//   response.send("Hello from Firebase!");
// });
```

hellow world 기능을 해제하면 다음과 같은 이점이 있습니다.

```coffeescript
import * as functions from 'firebase-functions';

export const helloWorld = functions.https.onRequest((request, response) => {
  functions.logger.info("Hello logs!", {structuredData: true});
  response.send("Hello from Firebase!");
});
```

## 첫 번째 기능 구현

기본 `hellowworld` 기능을 배포하려면 터미널의 functions 디렉토리로 이동하여 다음 명령을 실행합니다.

```coffeescript
npm run deploy
```

배포에 성공하면 다음과 같은 응답을 받아야 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/deploy-complete-message.png?resize=730%2C264&ssl=1)

이렇게 하면 Firebase 콘솔의 기능 섹션으로 이동할 때 배포된 `hello World` 기능과 해당 URL이 표시됩니다. 브라우저에 표시된 URL로 이동하면 Firebase의 Hello 응답이 반환됩니다!

우리의 목표가 단순한 API 호출과 데이터베이스 쿼리를 만드는 것이라면, 지금까지 우리가 해온 것만으로 충분할 것이다. 그러나 미들웨어 및 다중 라우트로 보다 강력한 REST API를 구축하고자 하므로 Express와 같은 Node.js 프레임워크가 필요합니다. Express.js를 설치하려면 터미널에서 다음 코드를 실행하십시오. 아직 함수 디렉터리에 있습니다.

```coffeescript
npm i -s express
```

Express를 설치하면 `//index.ts` 파일의 코드를 다음과 같이 변경하여 앱에서 초기화할 수 있습니다.

```coffeescript
import * as functions from 'firebase-functions'
import * as express from 'express'

const app = express()
app.get('/', (req, res) => res.status(200).send('Hey there!'))
exports.app = functions.https.onRequest(app)
```

다음으로 단말기에서 npm 실행 deploy를 실행하여 앱을 다시 배포하겠습니다. 초기 `hello World` 기능의 삭제에 대한 경고가 표시되면 예를 선택합니다.

```undefined
The following functions are found in your project but do not exist in your local
 source code:
   helloWorld(us-central1)
...
Would you like to proceed with deletion? Selecting no will continue the rest of the deployments. (y/N)
```

이렇게 하면 Firebase 콘솔의 기능 페이지를 다시 방문하면 앱이라는 새 기능과 앱 옆에 요청 URL이 표시됩니다. 해당 URL로 이동하면 `/index.ts` 파일의 `라인 5`에 설명된 대로 "Hey there!" 응답이 반환됩니다.

```coffeescript
app.get('/', (req, res) => res.status(200).send('Hey there!'))
```

## 앱의 서비스 계정 만들기

앱에서 Firestore 데이터베이스 및 관리 도구에 액세스하려면 서비스 계정을 만들어야 합니다. 이렇게 하려면 왼쪽 창의 프로젝트 개요 옆에 있는 기어 아이콘을 클릭하십시오.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/gear-icon-next-to-project-overview.png?resize=730%2C389&ssl=1)

설정 페이지에서 서비스 계정으로 이동하고 Node.js를 선택한 다음 새 개인 키 생성 버튼을 클릭합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/generate-new-private-key.png?resize=730%2C465&ssl=1)

그러면 새 서비스 계정에 대한 JSON 구성 파일이 생성됩니다. JSON 파일은 다음과 같습니다.

```undefined
{
  "type": "service_account",
  "project_id": "journal-rest-api",
  "private_key_id": "private_key_id",
  "private_key": "private_key",
  "client_email": "client_email",
  "client_id": "client_id",
  "auth_uri": "auth_url",
  "token_uri": "token_url",
  "auth_provider_x509_cert_url": "auth_provider_x509_cert_url",
  "client_x509_cert_url": "client_x509_cert_url"
}
```

### 서비스 계정 구성에 액세스하는 중

새로운 서비스 계정 정보를 쉽게 사용할 수 있는 방법은 JSON 파일을 프로젝트 디렉토리로 이동한 다음 필요한 곳에 가져오는 것입니다. 그러나 이 정보는 비공개로 유지하는 것이 좋으며, 코드베이스를 원격 리포지토리에 밀어넣으면 프로젝트 디렉토리에 저장하면 공개 보기에 노출됩니다.

서비스 계정의 정보를 사용하는 더 좋은 방법은 필요한 키-값 쌍을 Firebase 환경 변수로 저장하는 것입니다. 응용 프로그램에는 `private_key`, `project_id`, `client_email`이 필요합니다. 환경 변수로 저장해 보겠습니다. 이를 위해 터미널로 돌아가서 다음 명령을 실행합니다.

```bash
firebase functions:config:set private.key="YOUR API KEY" project.id="YOUR CLIENT ID" client.email="YOUR CLIENT EMAIL"
```

위의 명령을 실행하기 전에 먼저 문자열 값을 JSON 파일에 있는 문자열 값으로 대체해야 합니다. 키에는 소문자만 허용되며 각 키에는 마침표를 사용하여 이름 지정(예: `비공개)해야 합니다.key. 업데이트가 성공하면 다음과 같은 응답을 받아야 합니다.

```undefined
+  Functions config updated.

Please deploy your functions for the change to take effect by running firebase deploy --only functions
```

변경 사항을 적용하기 위해 기능을 다시 구축하겠습니다.

```coffeescript
npm run deploy
```

## Firebase 구성 설정

이제 서비스 계정 정보를 Firebase 환경 변수로 지정했으므로 구성 파일을 설정하겠습니다. 우리는 `/functions/src` 디렉토리에 `config`라는 이름의 새 폴더를 만들고 그 안에는 `firebase.ts`라는 이름의 새 파일을 만들 것이다. 구조는 다음과 같습니다.

```bash
└── functions
    └── src
        └── config
           ├── firebase.ts
```

다음 코드를 `firebase.ts` 파일에 붙여 넣습니다.

```js
import * as admin from 'firebase-admin'
import * as functions from 'firebase-functions'

admin.initializeApp({
  credential: admin.credential.cert({
    privateKey: functions.config().private.key.replace(/\\n/g, '\n'),
    projectId: functions.config().project.id,
    clientEmail: functions.config().client.email
  }),
  databaseURL: 'https://[your_project_id].firebaseio.com'
})

const db = admin.firestore()
export { admin, db }
```

앱 초기화를 위한 Firebase 관리자와 Firebase 환경 변수에 액세스하기 위한 Firebase 기능을 가져오는 것으로 시작했습니다. 우리는 계속해서 `관리자`를 불렀다.에서는 App() 기능을 초기화하고 서비스 계정 자격 증명이 포함된 개체 인수를 제공했습니다. 또한 `functions.config(.name_of_variable)`를 사용하여 환경 변수에 액세스했습니다.

개인 키에 .replace() 메서드를 추가했습니다. 그러면 개인 키 문자열의 \\n이 \n으로 바뀝니다. 그렇지 않으면 변수를 사용할 때 잘못된 PEM 형식 오류 메시지가 표시됩니다. 우리는 또한 credential 뒤에 databaseUrl 키를 추가했다.

[your_project_id]를 Firebase 프로젝트를 만들 때 제공한 것으로 바꾸어야 합니다. 다음으로, 우리는 Firestore 데이터베이스인 db에 대한 변수를 만들어 admin 값을 주었다.소방서 마지막으로 firebase.ts 파일에서 admin과 db를 내보냈습니다.

## Firestore 작업

Firestore는 모바일, 웹 및 서버 개발을 위한 NoSQL 데이터베이스입니다. Firebase 클라우드 기능과의 원활한 통합을 제공하며, 유연하고 확장 가능한 특성이 있어 웹 API 구축에 적합합니다. 모든 NoSQL 데이터베이스와 마찬가지로 Firestore는 비관계형이며 문서와 컬렉션으로 구성되어 있습니다. 이러한 문서에는 일련의 키-값 쌍이 포함되어 있으며 컬렉션에 저장됩니다.

Firestore에서는 Firebase 콘솔에서 또는 클라우드 기능을 통해 문서와 컬렉션을 만들 수 있습니다. 클라우드 기능에서 문서 또는 컬렉션을 생성하려면 Firebase 구성 파일에서 내보낸 `db`를 사용합니다.

```bash
db.collection('entries').doc().create({ document_object_here })
```

위의 코드 줄에는 `항목`이라는 이름의 문서가 새로 작성되며, 해당 집합이 존재하지 않을 경우 제공된 이름을 사용하여 새 문서가 작성됩니다. 또한 빈 문서를 작성한 다음 나중에 해당 문서에 대한 키-값 쌍을 제공할 수 있습니다.

```undefined
const entry = db.collection('entries').doc()

const entryObject = {
  id: entry.id,
  title: 'entry title here',
  text: 'entry text here'
}

entry.set(entryObject)
```

방화벽 문서에는 업데이트할 수 있는 .set() 메서드가 있습니다. 진행하면서 몇 가지 다른 방법을 알아보도록 하겠습니다.

### Firestore 데이터베이스에 항목 추가

데이터베이스 구성이 설정되어 있으면 저널 항목을 만드는 기능을 추가할 수 있습니다. //functions/src 디렉토리 내에서 `entryController.ts`라는 새 파일을 생성해 보겠습니다. 이 파일을 사용하여 입력용 컨트롤러를 모두 저장할 수 있습니다. entryController.ts 파일의 경우 먼저 express에서 response 유형을 가져오고 /config/firebase에서 db를 가져옵니다.

```coffeescript
import { Response } from 'express'
import { db } from './config/firebase'
```

다음으로, 항목의 유형을 정의한 후 이를 사용하여 `요청` 유형을 작성합니다.

```undefined
...
type EntryType = {
  title: string,
  text: string,
  coverImageUrl: string
}

type Request = {
  body: EntryType,
  params: { entryId: string }
}
```

Request 타입의 경우 entryId를 params의 속성으로 포함시켰습니다. 항목을 편집할 때 이 속성을 사용합니다. 다음으로 `AddEntry` 기능을 생성해 보겠습니다.

```undefined
...
const addEntry = async (req: Request, res: Response) => {
  const { title, text } = req.body
  try {
    const entry = db.collection('entries').doc()
    const entryObject = {
      id: entry.id,
      title,
      text,
    }

    entry.set(entryObject)

    res.status(200).send({
      status: 'success',
      message: 'entry added successfully',
      data: entryObject
    })
  } catch(error) {
      res.status(500).json(error.message)
  }
}
```

우리의 addEntry 기능에서 우리는 요청기관에서 제목과 텍스트를 파괴하는 것으로 시작했다. 다음으로, `db.collection(.doc()` 방법을 사용하여 새로운 Firebase 문서를 만든 다음 `entryObject`로 문서를 업데이트했습니다. 우리의 새 문서에는 id가 포함되어 있다. 쉽게 접근할 수 있도록 이 id를 키-값 쌍의 일부로 저장해야 할 것이다.

`res.status(200.send()`는 API 요청이 성공한 경우 사용자에게 응답을 반환합니다. 또한 데이터베이스 요청에 오류가 있을 경우 "500" 오류와 오류 개체로부터의 메시지로 응답할 수 있도록 데이터베이스 작업을 try…catch" 문으로 마무리했습니다.

500 HTTP 상태 코드는 사용자 입력 오류 또는 다른 유형의 오류를 포함하지 않으므로 데이터베이스 요청을 하거나 미들웨어를 사용하기 전에 해당 사례를 처리해야 합니다.

`AddEntry` 기능을 사용하려면 `entryController.ts` 파일에서 내보냅니다.

```bash
...
export { addEntry }
```

다음으로 `/functions/src/index.ts` 파일로 가져올 것입니다.

```coffeescript
...
import { addEntry } from './entryController'
```

그런 다음 `express`를 초기화한 라인 아래에 다음 코드를 추가합니다.

```bash
app.post('/entries', addEntry)
```

이제 `index.ts` 파일은 다음과 같습니다.

```coffeescript
import * as functions from 'firebase-functions'
import * as express from 'express'
import { addEntry } from './entryController'

const app = express()

app.get('/', (req, res) => res.status(200).send('Hey there!'))
app.post('/entries', addEntry)

exports.app = functions.https.onRequest(app)
```

앱을 다시 배포하려면 터미널의 기능 디렉터리로 돌아가서 다음 명령을 실행합니다.

```coffeescript
npm run deploy
```

구현이 성공적이면 다음과 같은 응답을 받아야 합니다.

```undefined
+  functions: Finished running predeploy script.
i  ...
+  functions[app(us-central1)]: Successful update operation.

+  Deploy complete!
```

API를 테스트하려면 Firebase 콘솔의 기능 섹션으로 이동하십시오. 배포된 함수 이름 `app`과 해당 URL을 확인해야 합니다. Firestore 데이터베이스에 새 항목을 추가하려면 URL에 `/entry` 경로를 추가합니다. https://us-central1-project-id-here.cloudfunctions.net/app/entries은 이렇게 보여야 한다.

그런 다음 포스트맨이나 인섬니아 같은 API 클라이언트를 사용하는 POST 요청을 요청 본문에 있는 제목과 텍스트와 함께 URL로 보낼 것입니다.

```undefined
{
  "title": "My first entry",
  "text": "Hey there! I'm awesome!"
}
```

이와 유사한 답변을 받아야 합니다.

```undefined
{
    "status": "success",
    "message": "entry added successfully",
    "data": {
        "id": "g8fBsSVQWlkby9GMf0LC",
        "title": "My first entry",
        "text": "Hey there! I'm awesome!"
    }
}
```

### Firestore 데이터베이스에서 항목 가져오기

이제 클라우드 기능을 사용하여 Firestore 데이터베이스에 항목을 추가하는 방법에 대해 알아보았으니 데이터를 어떻게 가져올 수 있는지 알아보겠습니다. 이를 위해 `entryController.ts` 파일에 `getAllEntries`라는 새 함수를 만듭니다.

```undefined
...
const getAllEntries = async (req: Request, res: Response) => {
  try {
    const allEntries = await db.collection('entries').get()
    return res.status(200).json(allEntries.docs)
  } catch(error) { return res.status(500).json(error.message) }
}

export { addEntry, getAllEntries }
```

"getAllEntries" 기능에서는 "db.collection(`entrys`).get() 방법을 사용하여 "entry" 컬렉션의 모든 데이터를 검색한 다음 "docs" 속성을 사용하여 응답 문서의 문서에 액세스했습니다. 이 기능을 사용하여 API 클라이언트를 통해 항목을 검색하려고 하면 개인 키와 같은 중요한 정보가 포함된 응답을 받게 됩니다. 입력 데이터만 반환하도록 기능을 업데이트하여 이 문제를 방지하십시오.

```js
const getAllEntries = async (req: Request, res: Response) => {
  try {
    const allEntries: EntryType[] = []
    const querySnapshot = await db.collection('entries').get()
    querySnapshot.forEach((doc: any) => allEntries.push(doc.data()))
    return res.status(200).json(allEntries)
  } catch(error) { return res.status(500).json(error.message) }
}
```

getAllEntries 함수에서 우리는 `Entry` 유형의 `allEntries`라는 배열을 만드는 것으로 시작했다.유형[] 다음으로, `db.collection(.get()` 메서드의 데이터를 `querySnapshot`이라는 변수에 할당했다. Firestore에서 `QuerySnapshot`에는 쿼리의 결과가 포함됩니다. 각각에 대한 방법은 쿼리 스냅숏을 통해 루프되고 문서 데이터를 모든 항목 배열로 푸시합니다.

GetAllEntries 기능을 사용하려면 `//functions/src/index.ts` 파일로 가져온 다음 GET 요청 방법에 추가합니다.

```coffeescript
...
import { addEntry, getAllEntries } from './entryController'

const app = express()

app.get('/', (req, res) => res.status(200).send('Hey there!'))
app.post('/entries', addEntry)
app.get('/entries', getAllEntries)
```

앱을 다시 배포하고 GET 요청을 `/entry`에 넣읍시다. 다음과 같은 답변을 받아야 합니다.

```undefined
[
    {
        "text": "Hey there! I'm awesome!",
        "id": "nS4so0TiUI0YLQH3xZLT",
        "title": "My first entry"
    }
]
```

### 항목 업데이트 및 삭제

이제 Firebase 클라우드 기능에서 Firestore 데이터베이스로 POST 및 GET 요청을 만드는 방법을 확인했으므로 항목을 어떻게 업데이트하고 삭제할 수 있는지 알아보겠습니다. 항목 업데이트 기능을 추가하려면 `entryController.ts` 파일에 `updateEntry`라는 새 함수를 만듭니다.

```undefined
...
const updateEntry = async (req: Request, res: Response) => {
  const { body: { text, title }, params: { entryId } } = req

  try {
    const entry = db.collection('entries').doc(entryId)
    const currentData = (await entry.get()).data() || {}
    const entryObject = {
      title: title || currentData.title,
      text: text || currentData.text,
    }

    await entry.set(entryObject).catch(error => {
      return res.status(400).json({
        status: 'error',
        message: error.message
      })
    })

    return res.status(200).json({
      status: 'success',
      message: 'entry updated successfully',
      data: entryObject
    })
  }
  catch(error) { return res.status(500).json(error.message) }
}
```

우리의 `updateEntry` 기능에서는 요청서에 제공된 ID로 요청 문서를 가져오는 것으로 시작했습니다. 이를 위해 `db.collection(.doc()` 방법을 사용했습니다. 다음으로, 우리는 현재 문서 데이터 객체를 `.data() 방식으로 받아서 항목을 `.set()` 방식으로 업데이트할 때 사용자가 제공하지 않는 모든 필드를 현재 입력 데이터로 바꿀 수 있습니다.

항목 삭제 기능도 생성하겠습니다.

```undefined
...
const deleteEntry = async (req: Request, res: Response) => {
  const { entryId } = req.params

  try {
    const entry = db.collection('entries').doc(entryId)

    await entry.delete().catch(error => {
      return res.status(400).json({
        status: 'error',
        message: error.message
      })
    })

    return res.status(200).json({
      status: 'success',
      message: 'entry deleted successfully',
    })
  }
  catch(error) { return res.status(500).json(error.message) }
}
```

또한 당사의 `deleteEntry` 기능은 요청 매개변수의 `entryId`와 `db.collection(.doc()` 메서드를 사용하여 문서를 가져옵니다. 다음으로, 우리는 `삭제` 방법을 사용하여 항목을 삭제합니다. 또한 데이터베이스 작업이 실패할 경우 오류를 감시하고 응답을 반환하는 `catch` 방식도 추가했습니다.

새 기능을 사용하려면 `entryController.ts` 파일에서 해당 기능을 내보냅니다.

```bash
...
export { addEntry, getAllEntries, updateEntry, deleteEntry }
```

이제 `/functions/src/index.ts` 파일로 가져올 수 있습니다.

```coffeescript
...
import { addEntry, getAllEntries, updateEntry, deleteEntry } from './entryController'
```

가져온 함수를 사용하여 패치 및 삭제 경로를 추가하십시오.

```coffeescript
...
app.patch('/entries/:entryId', updateEntry)
app.delete('/entries/:entryId', deleteEntry)
```

이제 `index.ts` 파일은 다음과 같습니다.

```coffeescript
import * as functions from 'firebase-functions'
import * as express from 'express'
import { addEntry, getAllEntries, updateEntry, deleteEntry } from './entryController'

const app = express()

app.get('/', (req, res) => res.status(200).send('Hey there!'))
app.post('/entries', addEntry)
app.get('/entries', getAllEntries)
app.patch('/entries/:entryId', updateEntry)
app.delete('/entries/:entryId', deleteEntry)

exports.app = functions.https.onRequest(app)
```

새로운 기능을 테스트하기 위해 `npm run deploy` 명령을 사용하여 앱을 다시 배포한 다음 Patch 또는 `DELETE` 요청을 Postman과 같은 API 클라이언트를 사용하여 `/entry/:entryId`로 전송합니다. 항목을 작성할 때 항목 본문에 `entryId`를 포함시켰음을 기억하십시오. 해당 ID는 GET 요청을 항목에 전송하면 알 수 있다.

## 결론

이 기사에서는 Firebase를 설정하고 클라우드 기능을 사용하여 TypeScript 및 Firestore를 사용하여 REST API를 구축하는 방법을 살펴봤습니다. 이 데모에서는 모든 Firestore 방법을 다루지는 않았지만, 공식 문서에서 찾을 수 있으며, 이 기사를 통해 알게 되셨다면 쉽게 이해할 수 있을 것입니다.

프로덕션 지원 앱에서는 많은 사례를 처리할 수 있도록 미들웨어를 설정해야 합니다. 예를 들어, 업데이트하기 전에 항목이 존재하는지 확인하는 미들웨어가 있을 수 있습니다. 또한 컨트롤러가 작동하기 직전에 인증 및 인증 미들웨어(Firebase React 앱으로 인증을 처리하는 좋은 방법)를 사용할 수 있습니다. 앱 구조상 미들웨어 추가는 간단할 것입니다.

여기 데모 앱의 GitHubrepo 링크가 있습니다. 도움이 필요하면 언제든지 LinkedIn으로 연락하십시오!