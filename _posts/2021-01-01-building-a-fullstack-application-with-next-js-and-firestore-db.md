---
layout: post
title: "Next.js 및 Firestore DB를 사용하여 전체 스택 애플리케이션 구축"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/firebase-next.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/firebase-next.png?fit=730%2C487&ssl=1)

## Cloud Firestore 설정

먼저 파이어베이스 프로젝트부터 시작하겠습니다. 이 링크로 이동하여 새 프로젝트를 만듭니다. Firebase는 다양한 서비스를 제공하지만, 이 기사에서는 NoSQL 데이터베이스 솔루션인 Cloud Firestore 서비스에 초점을 맞출 예정입니다.

사이드바의 개발 탭에서 클라우드 Firestore를 찾아 새 데이터베이스를 생성합니다. 메시지가 나타나면 프로덕션 모드에서 시작 옵션을 선택합니다. 이제 데이터베이스가 설정됩니다. 다음 단계는 Next.js 앱에서 데이터베이스를 초기화하고 액세스하는 데 사용할 자격 증명을 생성하는 것입니다.

사이드바 상단에 있는 `프로젝트 개요` 옆의 기어 아이콘을 클릭하고 프로젝트 설정으로 이동합니다. 그런 다음 서비스 계정 탭으로 이동하여 새 개인 키 생성 버튼을 클릭하여 동일한 페이지에 표시된 예제 코드와 함께 사용할 JSON 파일을 생성합니다.

### Next.js 앱 설정

이제 Next.js 앱을 생성하고 데이터베이스 연결을 구성할 수 있습니다.

Next.js를 시작하는 가장 쉬운 방법은 지정된 폴더에 Next.js 프로젝트를 생성하는 다음 명령을 사용하는 것입니다. 이 경우 데모 앱입니다.

```undefined
npx create-next-app demo-app
```

콘솔의 `demo-app` 디렉토리로 이동하여 `npm run dev`를 실행하여 개발 서버를 시작하십시오.

브라우저에서 localhost:3000을 방문하면 환영 페이지가 나타납니다.

## 데이터베이스에 연결 중

다음 단계는 데이터베이스와 통신할 유틸리티 기능을 만드는 것입니다. 첫째, 의존성으로 "firebase-admin"을 설치합니다. 그런 다음 프로젝트의 루트 디렉터리에 상대적인 utils/db/index.js를 생성하십시오. 이제 이전 단계에서 다운로드한 JSON 파일을 복사하여 `utils/db` 폴더에 붙여 넣고 `serviceAccountKey.json`의 이름을 바꿉니다.

다음 코드 블록을 `utils/db/index.js`에 복사하여 붙여넣고 `database`를 바꿉니다.서비스 계정 탭에서 확인할 수 있는 고유한 데이터베이스 URL을 가진 URL 키 값. 개인 키 JSON 파일을 생성하기 위해 이 탭으로 이동했습니다. 해당 탭의 코드 조각에는 데이터베이스의 URL이 포함되어 있습니다.

```coffeescript
import admin from 'firebase-admin';
import serviceAccount from './serviceAccountKey.json';

if (!admin.apps.length) {
  try {
    admin.initializeApp({
      credential: admin.credential.cert(serviceAccount),
      databaseURL: "YOUR_DB_URL"
    });
  } catch (error) {
    console.log('Firebase admin initialization error', error.stack);
  }
}
export default admin.firestore();
```

## 엔드포인트 생성

next.js에는 `/page/api` 폴더에 API 끝점을 만드는 데 도움이 되는 매우 편리한 API 경로 솔루션이 있습니다.

/page/api/entry/[id]를 생성하겠습니다.js` 및 `/page/api/entry/index.js`입니다.

첫 번째 것은 요청에서 전송된 id를 캡처하여 함수에서 사용할 수 있도록 합니다. 그건 나중에 얘기하죠.

우리는 네 가지 다른 요청 방법을 가질 것입니다. POST GET PUT DELETE 등이 그것이다.

- `POST`는 새 데이터베이스 항목을 추가하기 위한 것입니다.
- `PUT`는 기존 데이터베이스 항목을 업데이트하기 위한 것입니다.
- `GET`는 기존 데이터베이스 항목을 가져오기 위한 것입니다.
- `DELETE`는 기존 데이터베이스 항목을 삭제하는 것입니다.

먼저 POST 방법부터 시작하겠습니다. 여기서 POST 컬렉션에 새 항목을 추가합니다.

해당 끝점으로 보낸 다음 요청을 예로 들 수 있습니다.

```bash
axios.post('/api/entry', { title: Foo Bar, slug: foo-bar, body: lorem ipsum });
```

이것은 `/page/api/entry/index.js`의 내용입니다.

```js
import db from '../../../utils/db';

export default async (req, res) => {
  try {
    const { slug } = req.body;
    const entries = await db.collection('entries').get();
    const entriesData = entries.docs.map(entry => entry.data());

    if (entriesData.some(entry => entry.slug === slug)) {
      res.status(400).end();
    } else {
      const { id } = await db.collection('entries').add({
        ...req.body,
        created: new Date().toISOString(),
      });
      res.status(200).json({ id });
    }
  } catch (e) {
    res.status(400).end();
  }
}
```

db 오브젝트를 데이터베이스와의 통신 설정에 사용하기 위해 /utils/db에서 db 오브젝트를 가져왔습니다.

소방서에는 소장품이 있고, 각 소장품에는 여러 개의 문서가 있습니다. 문서는 JSON 구조와 마찬가지로 필드 또는 컬렉션을 다시 포함합니다.

위의 함수에서 "/api/entry/index.js"에서 내보내는 "/api/index.js"에서는 "slug" 키의 값을 사용하여 동일한 "slug" 값을 가진 다른 항목이 이미 있는지 확인합니다. 그렇다면 400의 상태 코드로 요청을 끝냅니다. 다른 상태 코드를 지정하거나 메시지를 전달하여 UI에 표시할 수 있습니다. 단순성을 위해 데이터 전송 없이 요청만 끝내도록 하겠습니다.

우리는 컬렉션 이름과 추가할 데이터를 지정합니다. 컬렉션이 아직 생성되지 않은 경우 지정한 이름으로 자동으로 생성됩니다. 또한 임의의 id가 포함된 문서가 추가됩니다. 그러면 이 문서에는 전달한 데이터가 포함됩니다.

슬러그가 고유한지 확인한 후 제목, 슬러그, 본문 등을 포함하는 요청기구를 추가해 달라고 다시 요청한다.

```undefined
// Example data
{
  title: 'Foo bar',
  slug: 'foo-bar', // This is auto generated on the client side
  body: 'Lorem ipsum',
}
```

우리는 그 객체를 다른 객체로 퍼트리고 현재 타임스탬프를 값으로 하여 `생성` 키도 추가한다.

이제 PUT, GET, DELETE 끝점을 만들어야 한다. /api/entry/[id]에서 처리됩니다.js의

이러한 요청의 예는 다음과 같습니다.

```js
await axios.get(`/api/entry/${id}`);
await axios.delete(`/api/entry/${id}`);
await axios.put(`/api/entry/${id}`, {
  slug: 'foo-bar',
  title: 'Foo Bar',
  body: 'Lorem ipsum'
});
```

끝점 경로에 `id` 변수가 어떻게 사용되는지 알 수 있습니다. 파일명이 `id`인 대괄호를 사용하기 때문에 `req.query`에서 사용할 수 있게 된다.

아래에서는 req 객체의 method 키를 확인하여 어떤 요청인지 파악한다.

PUT 방식이면 id가 항목 컬렉션에서 문서를 찾은 다음 객체로 업데이트합니다. 또한 현재 타임스탬프와 함께 `업데이트`라는 다른 키를 추가할 예정입니다. 이렇게 하면 항목이 업데이트되는 시기를 표시할 수 있습니다. 이것은 물론 선택적인 특징에 대한 시연일 뿐이다.

GET 방식이면 id로 문서를 다시 찾은 다음 doc.data()를 응답으로 반환합니다.

삭제 방법일 경우 id로 문서를 다시 찾은 다음 삭제하기만 하면 됩니다.

/page/api/entry/[id]의 내용입니다.js:

```js
import db from '../../../utils/db';

export default async (req, res) => {
  const { id } = req.query;

  try {
    if (req.method === 'PUT') {
      await db.collection('entries').doc(id).update({
        ...req.body,
        updated: new Date().toISOString(),
      });
    } else if (req.method === 'GET') {
      const doc = await db.collection('entries').doc(id).get();
      if (!doc.exists) {
        res.status(404).end();
      } else {
        res.status(200).json(doc.data());
      }
    } else if (req.method === 'DELETE') {
      await db.collection('entries').doc(id).delete();
    }
    res.status(200).end();
  } catch (e) {
    res.status(400).end();
  }
}
```

지금까지 이 모든 엔드포인트는 단일 항목을 제어했습니다. 그러나 나열할 모든 항목을 가져올 수 있는 끝점도 필요합니다.

이를 위해 `/api/entry.js`라는 이름의 끝점을 하나 더 만들 예정입니다.

아래에서는 항목 컬렉션에 있는 모든 문서를 가져오고 동시에 POST 방법의 끝점을 구현할 때 추가한 생성 키를 사용하여 문서를 주문합니다.

그런 다음 반환된 값을 맵핑하고 항목 데이터를 분산하는 새 개체 배열을 반환하고 문서 `id`도 추가합니다. GET, DELETE, PUT를 사용하려면 이 id가 필요하다.

이것은 /page/api/entry의 내용입니다.js:

```js
import db from '../../utils/db';

export default async (req, res) => {
  try {
    const entries = await db.collection('entries').orderBy('created').get();
    const entriesData = entries.docs.map(entry => ({
      id: entry.id,
      ...entry.data()
    }));
    res.status(200).json({ entriesData });
  } catch (e) {
    res.status(400).end();
  }
}
```

이 끝점은 다음과 같이 사용할 예정입니다.

```undefined
await axios.get('/api/entries');
```

이제 엔드포인트를 모두 사용할 수 있습니다. 그 다음 해야 할 일은 페이지를 구현하는 것이다.

## Next.js의 경로

가장 간단한 형태의 경우, "/admin" 경로와 "/posts" 경로가 필요합니다. 이것들은 물론 임의적인 이름들이다.

/admin 경로에서는 항목을 게시하고 모든 항목을 나열할 수 있어야 하나를 선택하고 편집할 수 있습니다. /posts 경로에서는 모든 항목을 나열하고 특정 항목을 탐색할 수 있어야 합니다.

따라서 `/페이지` 폴더의 최종 구조는 다음과 같습니다.

```undefined
|-- api
|-- admin
    |-- post.js
    |-- edit
        |-- index.js
        |-- [id].js
|-- posts
    |-- index.js
    |-- [slug].js
```

다시, 우리는 각각 /admin/edit 및 /posts 경로에서 id와 slug가 있는 대괄호를 사용할 것이다. 이러한 동적 경로를 통해 동일한 페이지 템플리트를 사용하여 서로 다른 내용을 가진 다른 경로를 만들 수 있습니다.

## '/관리' 경로

/admin 경로부터 살펴보겠습니다. `/admin` 라우트를 안전하게 만들기 위해 인증 뒤에 두지 않았습니다. 실제 응용 프로그램에서는 이 작업이 분명히 필요합니다.

### '/admin/post.js'

여기에는 하나의 상태 변수, 즉 `제목`과 `본문` 속성을 가진 개체가 있습니다.

제목과 본문 객체를 설정한 후 이 값을 항목으로 보낼 수 있습니다.

onSubmit 기능은 /api/entry에 제목, 본문, 슬러그 등을 포함한 객체를 호출한다. 제목 변수에서 대시파일을 통해 슬러그가 생성된다.

```coffeescript
import { useState } from 'react';
import dashify from 'dashify';
import axios from 'axios';

const Post = () => {
  const [content, setContent] = useState({
    title: undefined,
    body: undefined,
  })
  const onChange = (e) => {
    const { value, name } = e.target;
    setContent(prevState => ({ ...prevState, [name]: value }));
  }
  const onSubmit = async () => {
    const { title, body } = content;
    await axios.post('/api/entry', { title, slug: dashify(title), body });
  }
  return (
    <div>
      <label htmlFor="title">Title</label>
      <input
        type="text"
        name="title"
        value={content.title}
        onChange={onChange}
      />
      <label htmlFor="body">Title</label>
      <textarea
        name="body"
        value={content.body}
        onChange={onChange}
      />
      <button onClick={onSubmit}>POST</button>
    </div>
  );
};

export default Post;
```

### '/admin/edit/index.js'

편집 페이지에서 모든 항목을 `/api/entry` 끝점에서 가져와 나열했습니다.

그런 다음, 우리는 `id` 키를 사용하여 특정 항목에 연결했는데, 이것은 `admin/edit/[id]와 일치한다.js의

```js
import { useEffect, useState } from 'react';
import Link from 'next/link'
import axios from 'axios';

const List = () => {
  const [entries, setEntries] = useState([]);
  useEffect(async () => {
    const res = await axios.get('/api/entries');
    setEntries(res.data.entriesData);
  }, []);

  return (
    <div>
      <h1>Entries</h1>
      {entries.map(entry => (
        <div key={entry.id}>
          <Link href={`/admin/edit/${entry.id}`}>
            <a>{entry.title}</a>
          </Link>
          <br/>
        </div>
      ))}
    </div>
  );
};

export default List;
```

### /관리/편집/[id]js의

여기서는 `/api/entry/[id]를 사용합니다.특정 항목의 데이터를 `id`로 가져오기 위해 GET 메서드를 사용하는 js 끝점.

그런 다음 페이지의 제목 및 본문 필드를 데이터베이스에서 가져온 데이터로 채웁니다.

"OnSubmit"과 "OnDelete" 방법이 있으며, 두 방법 모두 "/api/entry/[id]에 요청을 보냅니다.항목을 업데이트 및 삭제하기 위해 PUT와 DELETE 메서드가 있는 js.

```js
import { useEffect, useState } from 'react';
import { useRouter } from 'next/router'
import dashify from 'dashify';
import axios from 'axios';

const EditEntry = () => {
  const router = useRouter()
  const [content, setContent] = useState({
    title: undefined,
    body: undefined,
  })

  useEffect(async () => {
    const { id } = router.query;
    if (id) {
      const res = await axios.get(`/api/entry/${id}`);
      const { title, body } = res.data;
      setContent({
        title,
        body
      })
    }
  }, [router])

  const onChange = (e) => {
    const { value, name } = e.target;
    setContent(prevState => ({ ...prevState, [name]: value }));
  }

  const onSubmit = async (e) => {
    const { id } = router.query
    const { title, body } = content;
    console.log(id, title, body);
    await axios.put(`/api/entry/${id}`, {
      slug: dashify(title),
      title,
      body,
    });
  }

  const onDelete = async () => {
    const { id } = router.query;
    await axios.delete(`/api/entry/${id}`);
    router.back();
  }

  return (
    <div>
      <label htmlFor="title">Title</label>
      <input
        type="text"
        name="title"
        value={content.title}
        onChange={onChange}
      />
      <label htmlFor="body">Body</label>
      <textarea
        name="body"
        value={content.body}
        onChange={onChange}
      />
      <button
        type="button"
        onClick={onSubmit}
      >
        Submit
      </button>
      <button
        type="button"
        onClick={onDelete}
      >
        Delete
      </button>
    </div>
  );
};

export default EditEntry;
```

이러한 `/admin` 경로는 공용 경로가 아니므로 클라이언트에서 렌더링될 수 있기 때문에 서버측으로 렌더링되지 않습니다.

### '/우편물' 경로

이러한 경로는 공개되므로 성능 및 SEO 결과 개선을 위해 서버측으로 렌더링될 수 있습니다.

### '/disc/index.js'

이 페이지는 모든 항목을 나열하는 페이지입니다. Next.js 전용 기능인 getStaticProps를 내보냅니다. 이 페이지를 내보내면 Next.js가 빌드 시간에 해당 페이지를 서버측 렌더링하도록 지시합니다. getStaticProps 함수의 모든 것은 서버측 렌더링되며 클라이언트에 노출되지 않습니다. 마찬가지로, GetStaticProps에 사용되는 경우 이 함수 외부의 모든 가져오기 및 정의는 클라이언트에 전송되는 코드에 포함되지 않습니다.

서버에서 이 페이지를 생성할 때 항목을 나열할 수 있어야 하므로 가져와야 합니다. 따라서 우리는 db 객체를 가져온 다음 데이터베이스에 요청을 합니다. 우리가 입력을 받으면, 우리는 어떻게든 그 부품에서 그것들을 사용할 수 있게 만들어야 합니다.

getStaticProps에서 반환된 객체는 구성 요소에서 소품으로 사용할 수 있게 됩니다. 항상 `props` 키가 있는 개체를 반환해야 하며 여기서 가져온 데이터를 전달합니다.

```css
return {
  props: {
    entriesData
  }
}
```

이 방법은 정말로 정적 페이지에 적합합니다. 그러나, 우리의 경우, 새로운 항목을 게시할 때, 이 페이지가 새로운 항목 데이터로 다시 생성되도록 빌드를 트리거하거나, 또는 비용이 많이 들고 속도가 느린 `요청 시간`에 이 페이지를 서버측으로 렌더링해야 합니다. next.js에는 증분 정적 재생이라는 솔루션이 있습니다.

시간 초과 값을 초 단위로 가진 `재검증` 키를 추가하여 Next.js에 요청 시 이 페이지를 재생성하도록 지시합니다. 따라서, 처음 페이지를 방문한 사람에게는 앱이 배포될 때 만들어진 기존 페이지가 제공됩니다. 그러나 이 첫 번째 요청은 새 빌드를 트리거하고 페이지를 방문한 다음 사용자에게 새로 생성된 페이지가 제공됩니다.

기간은 서버가 요청 시 다른 재생성을 트리거할 때까지 기다려야 하는 시간을 결정합니다.

예를 들어 이 값을 10분에 해당하는 600으로 설정하면 해당 페이지에 대한 첫 번째 요청이 재생성을 트리거합니다. 10분 이내에 다가올 요청은 다른 재생성을 트리거하지 않으며, 첫 번째 요청 후 마지막으로 생성된 페이지가 제공됩니다. 10분 기간이 만료되면 첫 번째 요청이 오면 다른 재생성이 트리거되는 등의 작업을 수행합니다.

여기서는 클라이언트 측 경로 전환을 위해 Next.js에서 제공하는 `<Link/> 구성 요소도 사용한다. {entry}/posts/${entry}를 전달합니다.href 소품에 "/swrug/[swrug"로 "/swrug"합니다.js는 그것에 필적할 수 있다.

```js
import Link from 'next/link'
import db from '../../utils/db';

const Posts = (props) => {
  const { entriesData } = props;

  return (
    <div>
      <h1>Posts</h1>
      {entriesData.map(entry => (
        <div key={entry.id}>
          <Link href={`/posts/${entry.slug}`}>
            <a>{entry.title}</a>
          </Link>
          <br />
        </div>
      ))}
    </div>
  );
};

export const getStaticProps = async () => {
  const entries = await db.collection('entries').orderBy('created', 'desc').get();
  const entriesData = entries.docs.map(entry => ({
    id: entry.id,
    ...entry.data()
  }));
  return {
    props: { entriesData },
    revalidate: 10
  }
}

export default Posts;
```

### '/아주머니/[아주머니]js의

"getStaticProps"에서는 "*context*.params"에서 "slug"를 캡처하여 특정 "slug" 값과 일치하는 문서를 찾기 위해 "entry" 컬렉션을 필터링하는 데 사용한다. 그런 다음 이전 구성 요소에서와 마찬가지로 해당 항목의 데이터를 소품으로 반환합니다. 입력된 민달팽이가 존재하지 않는다면, 우리는 빈 `props` 객체만 반환하지 않는다. 왜냐하면 우리는 어느 쪽이든 `props` 열쇠가 있는 객체를 반환해야 하기 때문이다.

여기서 새로운 것은 get static paths의 사용이다. 페이지에 동적 경로가 있고 getStaticProps가 있는 경우 빌드 시 생성되는 경로 목록을 정의하는 getStaticPaths도 사용해야 합니다.

GetStaticPaths에서는 모든 항목을 가져옵니다. 각 항목의 `slug` 값을 사용하여 다음과 같은 구조로 객체 배열을 생성한다.

```css
params: {
  slug: ...
}
```

대괄호로 묶은 파일 이름에 slug를 사용했기 때문에 params 객체에도 slug라는 키네임을 사용해야 한다.

생성한 어레이는 getStaticPaths에서 반환되는 개체의 paths 키로 전달됩니다.

경로 키 외에도 폴백:true 키-값 쌍도 있습니다.

이 `폴백` 키를 설정해야 합니다. false로 설정된 경우 Next.js는 빌드 시간 동안 생성되지 않은 모든 경로에 대해 404 페이지를 반환합니다. 새 항목을 추가할 경우 빌드를 다시 실행하여 이러한 경로를 생성해야 합니다. 그러나 fallback을 true로 설정하면 백그라운드에서 정적 생성을 트리거합니다.

이를 감지하고 그에 따라 로드 페이지를 표시하기 위해 라우터 개체를 사용할 수 있습니다. router.isFallback은 생성 중에 true가 되고 경로가 생성되면 false가 됩니다.

아래에서 `router.isFallback`을 사용하여 `loading…` 텍스트를 반환하는 방법을 확인하십시오. 방문한 경로가 데이터베이스의 "슬러그"와 일치하지 않으면 "getStaticProps"는 빈 "props" 개체를 반환하고 "not found" 텍스트가 표시됩니다.

```js
import { useRouter } from 'next/router'
import db from '../../utils/db';

const Post = (props) => {
  const { entry } = props;
  const router = useRouter()
  if (router.isFallback) {
    return (
      <div>loading</div>
    )
  } else {
    if (entry) {
      return (
        <div>
          <h1>{entry.title}</h1>
          <h4>{entry.created}</h4>
          <p>{entry.body}</p>
        </div>
      );
    } else {
      return (
        <div>not found</div>
      )
    }
  }
};

export const getStaticPaths = async () => {
  const entries = await db.collection("entries").get()
  const paths = entries.docs.map(entry => ({
    params: {
      slug: entry.data().slug
    }
  }));
  return {
    paths,
    fallback: true
  }
}

export const getStaticProps = async (context) => {
  const { slug } = context.params;
  const res = await db.collection("entries").where("slug", "==", slug).get()
  const entry = res.docs.map(entry => entry.data());
  if (entry.length) {
    return {
      props: {
        entry: entry[0]
      }
    }
  } else {
    return {
      props: {}
    }
  }
}

export default Post;
```

## 결론

이 기사에서는 Firebase의 Cloud Firestore 서비스를 사용하여 데이터베이스를 생성하는 방법과 Next.js 애플리케이션을 통해 이 데이터베이스와 통신하는 방법에 대해 다루었습니다. Next.js에서는 API 끝점을 생성하고 다양한 구성 요소 내에서 이러한 끝점을 사용하는 방법을 배웠습니다. 또한 정적 페이지 생성을 위한 동적 라우팅 및 서버 측 렌더링과 기존 또는 존재하지 않는 정적 페이지를 동적으로 재생성하는 방법을 시연했다.