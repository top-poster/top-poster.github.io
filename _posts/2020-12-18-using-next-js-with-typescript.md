---
layout: post
title: "TypeScript와 함께 Next.js 사용"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/using-nextjs-typescript.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/using-nextjs-typescript.png?fit=730%2C487&ssl=1)

Next.js를 사용하면 React를 사용하여 정적 및 동적 앱을 만들 수 있습니다. API Routes, 자동 코드 분할, 국제화, 이미지 최적화 등과 같은 편리한 기능을 제공합니다. TypeScript는 형식에 따라 코드 품질을 높이는 JavaScript의 상위 집합입니다.

TypeScript와 Next.js는 천생연분이다. Next의 기능을 통해 전체 스택 Retact 앱을 어느 때보다 쉽게 구축할 수 있으며, TypeScript의 유형 시스템을 통해 개발 중에 오류를 파악할 수 있습니다. 유효한 JavaScript 코드도 유효한 TypeScript 코드이기 때문에 TypeScript에서 형식을 사용하는 것은 선택 사항입니다. TypeScript 컴파일러는 변수와 함수의 유형을 제공합니다.

이 튜토리얼에서는 Next.js를 TypeScript와 함께 사용하는 방법을 시연하고 고품질의 검색 최적화 예측 가능한 앱을 구축하기 위한 최신 스택을 소개합니다.

다음 사항에 대해 자세히 설명합니다.

- Next.js란 무엇입니까?
- TypeScript란?
- Next.js 앱에서 TypeScript 사용
- Next.js 앱 테스트

Next.js 및 TypeScript를 실제로 표시하기 위해 간단한 아티클 관리자 앱을 구축하는 방법에 대해 살펴보겠습니다. JSON 자리 표시자로부터 데이터를 검색하는 예제 앱입니다.

## Next.js란 무엇입니까?

Next.js는 React와 Node.js 위에 구축된 운영 준비 프레임워크이다. React 앱을 즉시 시작하고 실행하는 데 필요한 모든 기능이 함께 제공됩니다.

클라이언트 측 렌더링과 서버 측 렌더링을 모두 지원하므로 Next.js를 사용하여 정적 또는 동적 앱을 구축할 수 있습니다. next.js 9는 API 경로를 도입하여 Node.js, Express.js, GraphQL 등으로 구축된 실제 백엔드(서버리스)를 사용하여 Next.js 앱을 확장할 수 있도록 합니다. 작성 당시 가장 최근 버전은 Next.js 10이다.

next.js는 자동 코드 분할(지속적 로드)을 사용하여 웹 사이트에 필요한 JavaScript만 렌더링합니다. 추가된 보너스로 Next.js는 SEO에 적합합니다.

## TypeScript란?

TypeScript는 마이크로소프트가 만들고 유지하는 인기 있는 언어이다. 자바스크립트의 상위 집합이므로 모든 기능을 선택할 수 있습니다.

기존 JavaScript 앱을 TypeScript로 변환할 수 있으며, 코드가 유효한 JavaScript인 경우 예상대로 작동합니다. TypeScript를 사용하면 변수와 함수에 대한 유형을 설정하여 정적으로 코드를 입력하고 컴파일 시 오류를 파악할 수 있습니다.

또한 JavaScript에서 아직 지원되지 않는 최신 기능을 사용할 수도 있습니다. 그리고 브라우저 지원에 대해 걱정하지 마십시오. TypeScript는 일반 JavaScript로 컴파일됩니다. 즉, TypeScript 코드는 브라우저에 표시되지 않습니다.

## Next.js 앱에서 TypeScript 사용

새 Next.js 앱을 생성하려면 Create Next App을 사용하면 됩니다.

먼저 CLI(명령줄 인터페이스)를 열고 아래 명령을 실행합니다.

```undefined
    npx create-next-app next-typescript-example
```

이 명령은 새 Next.js 앱을 생성합니다. 이제 프로젝트를 다음과 같이 구성하겠습니다.

```undefined
src
├── components
|  ├── AddPost.tsx
|  └── Post.tsx
├── pages
|  ├── index.tsx
|  └── _app.tsx
├── styles
|  └── index.css
├── tsconfig.json
├── types
|  └── index.ts
├── next-env.d.ts
└── package.json
```

Next.js 앱에서 TypeScript를 사용하려면 프로젝트 루트에 `tsconfig.json` 파일을 추가하십시오. next.js는 파일을 인식하고 프로젝트에 TypeScript를 사용합니다.

이렇게 하면 확장자가 .ts 또는 .tsx인 파일을 만들 수 있습니다. next.js는 자바스크립트에 대한 TypeScript 코드의 컴파일을 처리한 후 브라우저에서 정상적으로 앱을 제공한다.

### 유형 생성

```undefined
// types/index.ts

export interface IPost {
  id: number
  title: string
  body: string
}
```

이 인터페이스는 `포스트` 개체의 모양을 반영합니다. id, 제목, 본문 속성을 기대한다.

### 구성 요소 생성

이제 이 TypeScript 유형을 사용할 준비가 되었으므로 React 구성 요소를 생성하고 유형을 설정합니다.

```xml
// components/AddPost.tsx

import * as React from 'react'
import { IPost } from '../types'

type Props = {
  savePost: (e: React.FormEvent, formData: IPost) => void
}

const AddPost: React.FC<Props> = ({ savePost }) => {
  const [formData, setFormData] = React.useState<IPost>()

  const handleForm = (e: React.FormEvent<HTMLInputElement>): void => {
    setFormData({
      ...formData,
      [e.currentTarget.id]: e.currentTarget.value,
    })
  }

  return (
    <form className='Form' onSubmit={(e) => savePost(e, formData)}>
      <div>
        <div className='Form--field'>
          <label htmlFor='name'>Title</label>
          <input onChange={handleForm} type='text' id='title' />
        </div>
        <div className='Form--field'>
          <label htmlFor='body'>Description</label>
          <input onChange={handleForm} type='text' id='body' />
        </div>
      </div>
      <button
        className='Form__button'
        disabled={formData === undefined ? true : false}
      >
        Add Post
      </button>
    </form>
  )
}

export default AddPost
```

보시는 바와 같이, 우리는 `IPost` 타입을 가져오는 것부터 시작합니다. 그 후, 구성요소가 매개 변수로 받은 소품을 반영하는 `Props`라는 다른 유형을 생성한다.

다음으로, 우리는 `useState` 후크에 `IPost` 타입을 설정한다. 그런 다음 폼 데이터를 처리하는 데 사용합니다. 일단 양식이 제출되면, 우리는 `포스트`의 배열에 데이터를 저장하는 `세이브포스트` 기능에 의존한다.

이제 새로운 게시물을 만들고 저장할 수 있습니다.

`Post` 개체를 표시하는 구성 요소로 넘어갑시다.

```js
// components/Post.tsx

import * as React from 'react'
import { IPost } from '../types'

type Props = {
  post: IPost
  deletePost: (id: number) => void
}

const Post: React.FC<Props> = ({ post, deletePost }) => {
  return (
    <div className='Card'>
      <div className='Card--body'>
        <h1 className='Card--body-title'>{post.title}</h1>
        <p className='Card--body-text'>{post.body}</p>
      </div>
      <button className='Card__button' onClick={() => deletePost(post.id)}>
        Delete
      </button>
    </div>
  )
}

export default Post
```

이 `Post` 구성 요소는 표시할 게시물 개체와 해당 개체를 삭제하는 기능을 소품으로 수신합니다. TypeScript를 만족시키려면 인수가 `Props`와 일치해야 합니다.

이제 게시물을 추가, 표시 및 삭제할 수 있습니다. 이제 `App.tsx` 파일로 구성 요소를 가져와 게시물을 처리할 수 있는 논리를 만들어 보겠습니다.

```js
import * as React from 'react'
import { InferGetStaticPropsType } from 'next'
import AddPost from '../components/AddPost'
import Post from '../components/Post'
import { IPost } from '../types'

const API_URL: string = 'https://jsonplaceholder.typicode.com/posts'

export default function IndexPage({
  posts,
}: InferGetStaticPropsType<typeof getStaticProps>) {
  const [postList, setPostList] = React.useState(posts)

  const addPost = async (e: React.FormEvent, formData: IPost) => {
    e.preventDefault()
    const post: IPost = {
      id: Math.random(),
      title: formData.title,
      body: formData.body,
    }
    setPostList([post, ...postList])
  }

  const deletePost = async (id: number) => {
    const posts: IPost[] = postList.filter((post: IPost) => post.id !== id)
    console.log(posts)
    setPostList(posts)
  }

  if (!postList) return <h1>Loading...</h1>

  return (
    <main className='container'>
      <h1>My posts</h1>
      <AddPost savePost={addPost} />
      {postList.map((post: IPost) => (
        <Post key={post.id} deletePost={deletePost} post={post} />
      ))}
    </main>
  )
}

export async function getStaticProps() {
  const res = await fetch(API_URL)
  const posts: IPost[] = await res.json()

  return {
    props: {
      posts,
    },
  }
}
```

이 구성 요소에서는 먼저 이전에 생성된 유형과 구성 요소를 가져옵니다. Next.js에서 제공하는 InferGetStaticPropsType을 사용하면 "getStaticProps" 메서드에서 형식을 설정할 수 있습니다. GetStaticProps가 반환하는 소품에 정의된 유형을 유추한다.

그 후, 우리는 `useState` 후크를 사용하여 `posts` 배열로 상태를 초기화한다. 다음으로, 우리는 게시물의 배열에 대한 데이터를 저장하기 위해 `AddPost` 함수를 선언합니다. deletePost 메서드는 게시물의 id를 인수로 수신하므로 배열을 필터링하고 게시물을 제거할 수 있습니다.

마지막으로, 우리는 예상되는 소품을 부품에 전달한다. 그런 다음 응답 데이터를 순환하여 `작업` 구성 요소를 사용하여 표시합니다. 데이터는 Next.js에서 제공하는 `getStaticProps` 메소드의 도움을 받아 JSON 자리 표시자 API에서 검색된다. 가져온 게시물은 IPost와 일치하는 배열이어야 합니다. 그렇지 않으면 TypeScript에서 오류가 발생합니다. 또는 `getServerSideProps` 메서드, Fetch 또는 라이브러리를 사용하여 데이터를 가져올 수 있습니다. Next.js 앱을 어떻게 렌더링할지에 대한 문제입니다.

## Next.js 앱 테스트

이 마지막 터치로 앱을 브라우저에서 테스트할 수 있습니다.

먼저 프로젝트의 루트를 탐색하고 다음 명령을 실행합니다.

```undefined
    yarn dev
```

또는 `npm`을 사용하는 경우:

```coffeescript
    npm run dev
```

모든 작업이 예상대로 진행되면 다음 앱이 `http://localhost:3000/`에 표시됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/app-preview.png?resize=730%2C497&ssl=1)

바로 그거야!

## 결론

본 튜토리얼에서는 기사 관리자 앱을 구축하여 Next.js와 함께 TypeScript를 사용하는 방법에 대해 다루었습니다. GitHub에서 완료된 프로젝트를 미리 볼 수 있습니다.

Next.js는 TypeScript를 매우 잘 지원하며 설정도 쉽습니다. 이렇게 하면 클라이언트 또는 서버에서 실행되는 Next.js 및 TypeScript를 사용하여 강력한 형식의 Retact 앱을 쉽게 구축할 수 있습니다. 쉽게 말해 Next.js 및 TypeScript는 다음 React 프로젝트를 시도하기 위한 매우 흥미로운 스택입니다.