---
layout: post
title: "오프라인 데이터 동기화에 수박DB 사용"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/12/watermelondb-offline-data-sync.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/watermelondb-offline-data-sync.png?fit=730%2C487&ssl=1)

## 씌우다

이 게시물은 리액트 네이티브로 오프라인 웨이트 트래킹 앱을 만드는 것에 대해 내가 썼던 이전 게시물의 속편으로 볼 수 있다. 하지만, 그것이 당신이 먼저 그 게시물을 전부 읽을 필요가 있다는 것을 의미하지는 않습니다.

그 게시물 전체에 우리가 쓴 코드를 이 게시물의 출발점으로 삼을 것입니다. 하지만 이 게시물의 목적은 수박의 연결 방법을 보여주는 것입니다.AdonisJs 서버 측 API 응용 프로그램과 DB의 동기화 기능.

## 수박 탐험DB의 오프라인 기능

위에서 언급한 바와 같이, 우리는 다른 블로그 게시물에 우리가 만든 앱을 사용할 것입니다. 계속하기 전에 React Native 및 AdonisJs 앱이 모두 포함된 폴더를 생성한 후 여기에서 리포지토리를 복제하고 분기 v1을 체크 아웃하십시오.

```bash
mkdir weightress && cd $_
git clone <https://github.com/foysalit/weightress-app-blog.git> app && cd $_
git checkout v1
yarn
```

코드와 모든 종속성을 설치한 후에는 장치 또는 에뮬레이터를 사용하여 원하는 플랫폼에서 앱을 실행할 수 있습니다. 테스트를 위해 물리적 Android 장치를 사용하고 있기 때문에 해당 사례에 대한 단계를 작성하겠습니다. 하지만 다른 경우에도 마찬가지여야 합니다.

단말기를 연결한 후 단말기의 한 창에서 yarn start(야른 시작)를 실행하고 다른 창에서 yarn Android(야른 안드로이드)를 실행하기만 하면 된다. 그러면 장치에서 앱이 시작됩니다.

이미 데이터를 동기화하고 싶은 수박DB 앱이 있다면 얼마든지 사용하되, 데이터 구조가 우리와 다를 것이라는 점을 명심하세요. 따라서, 테이블, 스키마 등은 그에 따라 조정되어야 할 것이다.

현재 상황을 요약하자면, 우리는 `weights`라는 테이블에 데이터를 입력할 수 있는 앱이 있는데, 우리는 이 데이터를 REST API 호출을 통해 어딘가에 있는 서버와 동기화하고 싶다. 이 기능은 장치 외부에서 데이터를 동기화할 수 있도록 오프라인 앱에 대한 매우 일반적인 요구 사항이기 때문에 수박DB는 이 기능을 즉시 사용할 수 있습니다.

그걸 어떻게 활용할 수 있는지 봅시다. `data/` 폴더 내에 `sync.ts`라는 파일을 만들어 다음 코드를 넣어보자.

```js
import {synchronize} from '@nozbe/watermelondb/sync';
import {database} from './database';
// your_local_machine_ip_address usually looks like 192.168.0.x
// on *nix system, you would find it out by running the ifconfig command
const SYNC_API_URL = 'http://<your_local_machine_ip_address>:3333/sync';
export async function sync() {
  await synchronize({
    database,
    pullChanges: async ({lastPulledAt}) => {
      const response = await fetch(SYNC_API_URL, {
        body: JSON.stringify({lastPulledAt}),
      });
      if (!response.ok) {
        throw new Error(await response.text());
      }

      const {changes, timestamp} = await response.json();
      return {changes, timestamp};
    },
    pushChanges: async ({changes, lastPulledAt}) => {
      const response = await fetch(
        `${SYNC_API_URL}?lastPulledAt=${lastPulledAt}`,
        {
          method: 'POST',
          body: JSON.stringify(changes),
        },
      );
      if (!response.ok) {
        throw new Error(await response.text());
      }
    },
  });
}
```

SeambonDB(데이터베이스 인스턴스 필요)의 동기화 기능, pullChanges 기능, pushChanges 기능을 사용하고 있습니다. 데이터베이스 인스턴스를 이미 database.ts 파일에서 내보냈기 때문에 이를 가져와 전달합니다.

pullChanges는 원격 소스에서 데이터를 가져오고 원격 데이터로 로컬 DB를 업데이트합니다. 함수 본문에서는 SYNC_API_URL 끝점에 수박DB에서 사용할 수 있도록 제공된 마지막 PulledAT 타임스탬프를 사용하여 GET 요청을 한다. URL은 `<your_local_machine_ip_address>:3333/sync`를 가리키며, 아도니스 앱을 빌드하면 사용할 수 있습니다.

> 참고: 앱이 다른 장치에서 실행되고 해당 장치의 로컬 호스트가 Adonis 앱이 실행 중인 시스템의 로컬 호스트와 동일하지 않기 때문에 React Native 코드에서 'localhost:3333'을 사용할 수 없습니다. 그렇기 때문에 IP 주소를 대신 사용해야 합니다.

요청 응답에서는 변경 사항 및 타임스탬프 속성이 있는 개체를 수신해야 합니다. 수박과 호환되려면 `풀 체인지` 함수 호출에서 반환해야 하는 내용입니다.DB의 동기화 구현입니다.

push changes는 우리의 로컬 데이터를 원격 소스로 보내는 기능입니다. 당연히 우리는 우리의 `SYNC_API_URL`에 `POST` 요청을 하고, 요청 본문에는 수박DB가 우리에게 제공하는 `변경` 변수를 첨부한다.

함수에서 아무것도 반환하지 않지만 API 요청이 OK 상태를 반환하지 않을 경우 오류를 발생시킵니다. 이는 오류를 발생시킴으로써 서버 엔드에서 어떻게든 푸시가 깨지면 수박DB가 데이터 무결성을 유지하기 위해 제 역할을 할 수 있도록 하기 때문에 중요하다.

## AdonisJs를 사용하여 API 앱을 생성하는 중

아도니스에는 편리한 보일러 판 발전기 명령이 딸려 있다. 이 기능을 실행하려면 `weightress/` 폴더 내에 있는지 확인하고 `yann create donis-ts-app api` 명령을 실행하십시오. 몇 가지 질문을 받을 수 있습니다. 아래 답변과 같은 답변을 제공하면 `api`라는 이름의 디렉토리가 남게 됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/adonisjs-boilerplate-generator.png?resize=730%2C395&ssl=1)

Adonis는 CLI 명령을 작성하고 실행하기 위해 Ace라는 패키지를 사용합니다. 그러나 이 기능을 사용하려면 먼저 코드를 컴파일해야 합니다. 걱정하지 마세요. 이것은 실 명령어를 실행하는 것만큼 간단합니다. cdapi를 실행하기만 하면 됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/compiling-adonisjs-code-ace-e1607522635806.png?resize=730%2C120&ssl=1)

이 앱에서 가장 먼저 필요한 것은 수박DB에 있는 데이터 구조를 서버 데이터베이스로 복제하는 것입니다. 이 경우, 데이터베이스와 통신하기 위해 MySQL을 데이터베이스로, Lucid를 ORM으로 선택합니다.

다시, AdonisJs는 데이터베이스와 관련된 모든 것을 부트스트랩하는 데 유용하다. 다음 명령을 실행하여 모든 명령을 함께 가져옵니다.

```undefined
yarn add @adonisjs/lucid@alpha
node ace invoke @adonisjs/lucid
yarn build
```

두 번째 명령은 DB 공급자를 선택하라는 메시지를 표시합니다. 저는 MySQL을 선택하고 있지만, 이 게시물의 맥락에 큰 차이가 없을 것이기 때문에 원하는 DB를 자유롭게 선택해 주세요. 터미널 창의 출력입니다. 참조:

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/choosing-mysql-db-provider.png?resize=730%2C164&ssl=1)

이제 MySQLDB에 연결하려면 자격 증명을 env 변수로 추가해야 합니다. 아도니스는 그것들을 다루기가 꽤 쉽죠. .env 파일을 열면 다음과 같은 내용이 표시됩니다.

```undefined
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_USER=lucid
DB_PASSWORD=
DB_NAME=lucid
```

로컬 또는 원격 MySQL 데이터베이스의 인증 정보와 일치하도록 이러한 변수의 값을 조정하여 앱이 이 데이터베이스에 연결할 수 있도록 합니다.

앱 코드 작성을 시작하기 전에 앱의 데이터를 지원하도록 데이터베이스가 모두 설정되어 있는지 확인합니다. 스키마 관리에 대해서는 마이그레이션을 통해 수행하는 것이 항상 더 낫고 쉬우므로 먼저 node ace make: migration weights 중 하나를 생성하겠습니다. 그러면 `데이터베이스/마이그레이션/` 폴더에 새 파일이 삭제됩니다. 파일을 열고 다음 코드로 내용을 바꿉니다.

```undefined
import BaseSchema from '@ioc:Adonis/Lucid/Schema'

export default class Weights extends BaseSchema {
  protected tableName = 'weights'

  public async up () {
    this.schema.createTable(this.tableName, (table) => {
      table.increments('id')
      table.double('weight')
      table.string('note')
      table.timestamps(true)
    })
  }

  public async down () {
    this.schema.dropTable(this.tableName)
  }
}
```

마이그레이션이 실행되면 자동으로 증가하는 id 열, 부동 소수점 숫자를 저장할 수 있는 가중치 열, note라는 문자열 열, Adon이 자동으로 추가한 timestamps 열로 구성된 가중치라는 새 테이블이 MySQL 데이터베이스에 생성됩니다.

보시다시피, 이것은 Primary KEY 칼럼과 같은 MySQL 고유의 것을 제외하고 수박DB의 가중치 테이블 스키마에 대해 우리가 가지고 있는 것과 거의 똑같은 구조이다. React Native 앱의 코드에서 `data/schema.ts` 파일의 스키마 코드와 일치시킬 수 있습니다.

마지막으로, `실 쌓기`를 하는 것을 잊지 마세요.

이제 아도니스 앱에서 이 데이터를 전달/조작하려면 모델이 필요합니다. node ace make:model weight를 실행하여 app/model/weight.ts에서 새 파일을 생성하도록 하자. 이 파일의 코드를 다음과 같이 바꿉니다.

```java
import { DateTime } from 'luxon'
import { BaseModel, column } from '@ioc:Adonis/Lucid/Orm'

export default class Weight extends BaseModel {
  @column({ isPrimary: true })
  public id: number

  @column()
  public weight: number

  @column()
  public note: string

  @column.dateTime({ autoCreate: true })
  public createdAt: DateTime

  @column.dateTime({ autoCreate: true, autoUpdate: true })
  public updatedAt: DateTime
}
```

새로운 행은 가중치와 노트 열 정의뿐이다. 이렇게 하면 두 속성 모두 이 모델에서 코드로 공개적으로 액세스할 수 있습니다. 이것은 또한 우리가 수박에 대해 가지고 있던 모델 정의와 꽤 비슷하다.DB. 파일 `data/weight.ts`와 일치시킬 수 있습니다.

## 동기화를 위한 REST API 엔드포인트 구축

지금까지 우리는 API에 "최하위 기계"를 구축해 왔습니다. 이제 이 기계를 사용하여 REST API 끝점을 동기화에 구축해야 합니다. React Native 앱 쪽에서 이를 기대하고 있습니다. 기억하신다면, 우리는 `/sync`에 하나의 끝점만 있으면 되지만, 그것은 `GET`와 `POST` 요청을 모두 처리할 수 있어야 합니다.

먼저 `start/route.ts` 파일에 다음 두 줄을 추가합니다.

```undefined
Route.get('sync', 'SyncsController.push');
Route.post('sync', 'SyncsController.pull');
```

이는 아도니스가 `/sync` 끝점을 사용할 수 있도록 하고, `GET` 요청에 푸시(push) 방식, `POST` 요청에 풀(pull) 방식을 사용하라는 것으로, 둘 다 의문의 `싱크 컨트롤러` 클래스에 속한다. 지금 바로 컨트롤러로 이동하겠지만, 여기서는 이름이 좀 헷갈릴 수 있습니다.

그러나 API 서버의 데이터베이스 관점에서 생각하면 GET 요청은 이 데이터베이스의 POV의 원격 데이터인 수박DB의 "PullChanges" 호출에 의해 생성됩니다. 이쪽에서 데이터를 원격 소스로 밀어내는 것과 같습니다.

마찬가지로 수박DB의 push Changes 호출은 서버 데이터베이스의 POV의 pull 작업과 유사한 POST 요청을 생성한다. 좀 아찔하니까 너무 어렵게 생각하지 마세요.

이제 Syncs Controller를 살펴보겠습니다. 컨트롤러 메서드는 지정된 끝점에서 요청이 경로 정의로 수신될 때 트리거됩니다. node ace make:controller sync를 실행하여 동기화 컨트롤러를 생성해 봅시다. 이 동기화는 `app/Controller/Http/SyncsController.ts`에서 새 파일을 생성합니다.

이제 해당 파일을 열고 다음 코드에 배치합니다.

```js
import { DateTime } from 'luxon'
import {RequestContract} from '@ioc:Adonis/Core/Request'
import { HttpContextContract } from '@ioc:Adonis/Core/HttpContext'
import Weight from 'App/Models/Weight'

const getSafeLastPulledAt = (request: RequestContract) => {
  const lastPulledAt = request.input('lastPulledAt')
  if (lastPulledAt !== 'null') {
    return DateTime.fromMillis(parseInt(lastPulledAt)).toString()
  }
  return DateTime.fromMillis(1).toString()
}

export default class SyncsController {
  public async pull ({ request }: HttpContextContract) {
    const changes = request.input('changes')

    if (changes?.weights?.created?.length > 0) {
      await Weight.createMany(changes.weights.created.map(remoteEntry => ({
        note: remoteEntry.note,
        weight: remoteEntry.weight,
        watermelonId: remoteEntry.id,
        createdAt: DateTime.fromMillis(parseInt(remoteEntry.created_at)),
      })))
    }

    if (changes?.weights?.updated?.length > 0) {
      const updateQueries = changes.weights.updated.map(remoteEntry => {
        return Weight.query().where('watermelonId', remoteEntry.id).update({
          note: remoteEntry.note,
          weight: remoteEntry.weight,
        })
      })
      await Promise.all(updateQueries)
    }

    if (changes?.weights?.deleted?.length > 0) {
      await Weight.query().where('watermelon_id', changes.weights.deleted).exec()
    }
  }
  public async push ({request}: HttpContextContract) {
    const lastPulledAt = getSafeLastPulledAt(request)
    const created = await Weight.query().where('created_at', '>', lastPulledAt).exec()
    const updated = await Weight.query().where('updated_at', '>', lastPulledAt).exec()
    return {
      changes: {
        weights: {
          created,
          updated,
          deleted: [],
        },
      },
      timestamp: Date.now(),
    }
  }
}
```

네, 여기 일이 많으니 한 번에 이 방법을 풀어요. pull 메서드는 개체인 changes라는 속성이 있는 개체를 수신합니다. 이 개체에는 마지막 동기화 이후 발생한 모든 DB 변경사항이 모든 테이블/컬렉션의 이름으로 표시됩니다.

따라서 우리의 경우 핵심 `중량`을 살펴봐야 한다. 각 테이블에 대해 세 가지 배열 속성(모든 새 항목을 포함하는 `created`, 수정된 모든 항목을 포함하는 `updated`, 마지막 동기화 이후 삭제된 모든 항목을 포함하는 `deleted`)이 있다.

따라서 `created` 입력에 항목이 있는 경우 어레이의 모든 항목을 매핑하고 `Weight.createMany() 방법을 사용하여 해당 모든 항목을 서버측 데이터베이스에 삽입합니다.

created_at을 DateTime 인스턴스로 변환하는 데 유의하십시오. 수박DB 타임스탬프는 숫자만 포함하는 Unix 타임스탬프로서 Lucid ORM의 datetime 필드와 직접 호환되지 않습니다. 우리는 수박DB에서 나오는 id 필드도 삽입하고 있으며, 다음 줄에서 왜 그것이 필요한지 알 수 있다.

서버 측에서 업데이트할 때 클라이언트 항목과 서버 항목 사이의 유일한 참조 지점은 수박DB 생성 ID이며, 따라서 생성된 항목을 동기화할 때 서버측 DB에 저장했습니다. 따라서 업데이트를 위해 `업데이트된` 입력 배열의 모든 항목을 순환하고 `가중치`를 사용하여 항목을 업데이트한다.추궁하다여기서(`watermelonId`, remoteEntry.id).update`.

업데이트된 어레이의 모든 항목에 대해 데이터베이스에 대해 하나의 업데이트 쿼리를 실행하고 있기 때문에 여기에 약간의 성능 문제가 있습니다. 이로 인해 사용자 기반이 확장됨에 따라 데이터베이스가 빠르게 정체될 수 있으므로, 이 점을 주시하십시오. 마지막으로, 이러한 모든 쿼리가 `약속.all`을 사용하여 완료될 때까지 기다립니다.

삭제하는 것은 약간 다릅니다. 수박DB는 삭제된 항목의 ID만 전송하므로 해당 ID와 일치하는 모든 항목을 삭제하기만 하면 됩니다.

push 끝점은 pull과 비슷한 인터페이스를 가지고 있다. 정부는 변경 부동산과 타임스탬프 부동산을 반환해야 할 것이다. 타임 스탬프(time stamp)의 속성은 매우 중요하다. 왜냐하면 이것은 고객으로부터 성공적으로 끌어낸 후에 수박DB가 구할 것이기 때문이다.

다음에 다른 풀링을 요청할 때 이전에 저장한 타임스탬프를 전송하여 마지막으로 동기화에 성공한 시기를 서버에 알립니다. 그래서 당길 때 수박DB가 `마지막 뽑기` 입력을 보내는 것이다. 첫 번째 동기화의 경우 null로 설정되므로 이 입력을 번역하기 위해 getSafeLastPullAt라는 도우미 기능이 있습니다.

변경사항 내에는 데이터베이스의 각 테이블/컬렉션에 대해 하나의 속성이 있어야 하므로, 이 경우 가중치만 포함됩니다. 각 속성은 생성됨, 업데이트됨 및 삭제됨으로 구성됩니다. 이 게시물의 목적상, 우리의 UI는 아직 앱 내의 항목 삭제를 허용하지 않기 때문에, 우리는 단지 `생성`과 `업데이트` 동기화만을 다룰 것이다.

마지막 PulledAt 타임스탬프를 사용하여 데이터베이스의 모든 항목을 쿼리하여 이후 업데이트되거나 생성된 항목을 찾아 응답으로 보냅니다. 서버측 앱에 필요한 것은 그것뿐입니다.

## 동기화 표시기 UX 구현

이 게시물의 시작 부분에서는 동기화 프로세스를 트리거하는 `sync` 기능만 정의했습니다. 그러나 이 모든 작업을 수행하려면 여전히 동기화 기능을 호출하여 프로세스를 트리거해야 합니다. 이 작업은 온 디맨드, 자동 또는 두 가지를 조합하여 수행할 수 있습니다.

이 게시물의 목적상 앱이 열릴 때마다 동기화를 트리거하는 방식으로만 자동 접근 방식을 구축하겠다. 동기화가 발생할 때 사용자에게 동기화가 진행 중이라는 표시도 보여줘야 합니다. 우리는 이 모든 것을 부품으로 만들 것입니다. `components/sync-indicator.tsx`에 새 파일을 생성하고 다음 코드를 입력합니다.

```js
import React, {useEffect, useState} from 'react';
import {View, Text} from 'react-native';
import {syncStyles} from './styles';
import {sync} from '../data/sync';

const SyncIndicator = () => {
  const [syncState, setSyncState] = useState<string>('Syncing data...');

  useEffect(() => {
    sync()
      .then(() => setSyncState(''))
      .catch(() => setSyncState('Sync failed!'));
  });

  if (!syncState) {
    return null;
  }

  return (
    <View style={syncStyles.container}>
      <Text style={syncStyles.text}>{syncState}</Text>
    </View>
  );
};

export default SyncIndicator;
```

이것은 단지 하나의 로컬 상태인 `syncState`를 가진 꽤 간단한 구성 요소이다. 처음에는 데이터 동기화로 설정되어 있습니다. 열린 상태에서는 이 구성 요소가 매번 로드되고 동기화가 트리거되기 때문입니다.

구성 요소가 뷰에 로드되는 즉시 use Effect를 사용하여 sync() 기능을 해제하고 오류 없이 완료되면 syncState를 빈 문자열로 설정하여 완료로 표시합니다. 동기화 약속에서 오류가 발생하면 사용자에게 알리기 위해 `syncState failed!(동기화 상태 실패!)를 설정합니다.

이러한 논리에 따라 동기화 상태가 비어 있으면 아무것도 렌더링하지 않습니다. 그렇지 않으면 style.ts 파일에서 가져온 일부 스타일과 함께 "syncState` 텍스트를 표시합니다. 스타일 정의를 살펴보겠습니다.

```js
export const syncStyles = {
  container: {
    paddingVertical: 5,
    alignItems: 'center',
    backgroundColor: primaryColor,
  },
  text: {
    color: '#FFFFFF',
  },
};
```

이렇게 하면 동기화 상태 텍스트가 쉽게 표시되도록 배경색이 설정됩니다. 이제 이 구성 요소만 앱에 넣으면 됩니다. `App.tsx` 파일을 열고 `ScrollView` 영역 위에 구성 요소를 렌더링합니다.

```undefined
import Creator from './components/creator'; 
// import the sync indicator
import SyncIndicator from './components/sync-indicator';
// … previous code
<StatusBar /> 
<SafeAreaView>
  // render the sync indicator
  <SyncIndicator />
  <ScrollView contentInsetAdjustmentBehavior="automatic">
```

이제 앱을 열고 AdonisJs 앱이 실행 중인 터미널 창을 주시하면 화면 상단에 Syncing Data...라는 작은 텍스트가 표시되고 동기화가 성공했음을 의미하는 텍스트가 사라집니다. 또한 동기화 실패를 시뮬레이션하기 위해 `SYNC_API_URL`을 임의 URL로 변경할 수도 있습니다. 이 경우 동기화 실패! 메시지가 표시됩니다.

다음은 내 장치의 빠른 스크린샷입니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/syncing-data-message-screenshot.jpg?resize=554%2C1200&ssl=1)

## 오프라인 동기화 동작 테스트

시험 운전할 시간이다. 오프라인 지원 앱은 특히 다양한 시나리오에서 발생할 수 있는 에지 케이스가 너무 많기 때문에 테스트하기가 좀 어렵다. 다음은 기본 오프라인 동작이 작동하는지 테스트하고 확인하는 방법입니다.

장치의 모든 네트워크 연결을 끊으려면 비행기 모드를 켜십시오. 그런 다음 새 가중치 항목을 추가하고 항목이 차트에 나타나는지 확인합니다. 또한 새 항목을 서버와 동기화하려고 했기 때문에 Sync failed!(동기화 실패!) 오류가 나타납니다.

이제 앱을 닫고 비행기 모드를 끈 다음 앱을 다시 엽니다. Sync 표시기에 Sync Data… 메시지가 표시되고 결국 사라집니다. 서버측에서 데이터베이스를 확인하면 비행기 모드 중에 추가된 항목이 표시되어야 합니다.

전체 AdonisJs 앱은 여기에서 찾을 수 있습니다.

## 여기서 어디로 갈까요?

우선, 축하해요! 이제 여러분은 완전한 오프라인 지원을 받는 웨이트 트래킹 앱의 자랑스러운 소유자가 되었습니다. 그러나 Google Fit 또는 Apple Health를 이길 수 없다는 데 동의할 것입니다. 다행히도, 전문적으로 만들어진 체중 추적 플랫폼과 경쟁하기 위해 이것을 다듬을 수 있는 많은 것들이 있습니다.

다음은 이 앱을 개선하기 위해 구축할 수 있는 한 입 크기의 아이디어입니다.

- 오프라인 동기화는 오류가 발생하기 쉽습니다. 사용자에게 동기화에 실패했다고 말하는 것은 전혀 도움이 되지 않습니다. 사용자에게 동기화 중에 무엇이 잘못되었는지, 오류를 극복하여 데이터를 동기화하는 방법을 보여주는 자세한 오류 뷰어를 만들 수 있습니다.
- 오프라인 앱의 경우 서버와의 동기화도 좋지만, 결국 사용자는 데이터를 어딘가에 서버에 보관하지 않고 집으로 가져가고 싶어할 것으로 가정해야 합니다. 따라서 모든 데이터를 내보내고 앱으로 다시 가져올 수 있는 방법을 구축하십시오.
- 동기화가 부분적으로 실패할 수 있습니다. 예를 들어, 사용자가 오프라인 모드에서 5개의 항목을 추가하는 경우 동기화 중에 4개가 DB에 저장될 수 있지만 1개가 실패할 수 있습니다. 그러면 실패한 항목에 대해서만 동기화를 트리거하도록 사용자에게 요청하거나 실패한 항목을 다시 삽입하도록 요청할 수 있기 때문에 실패한 항목 목록이 있으면 유용합니다.

위에 언급한 내용 중 하나 또는 그 이상을 앱에 구축해 주신다면 언제든지 큰 소리로 알려 주시면 코드를 검토하겠습니다. 대박이다!