---
layout: post
title: "개츠비의 헤드리스 컨텐츠 관리를 위한 Sanity CMS"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/sanity-gatsby.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/sanity-gatsby.png?fit=730%2C487&ssl=1)

개츠비는 웹사이트와 앱을 만들기 위한 가장 인기 있는 리액션 기반 프레임워크 중 하나이다. 카일 매슈스(대표 개츠비)는 어떤 배포 환경에서도 속도가 빠르다고 칭찬했지만, 최근 빌드 시간은 개츠비 클라우드에 대한 증분 빌드 릴리스로 인해 부정적인 영향을 받을 수 있다고 경고했습니다.

만약 여러분이 개츠비나 다른 SSG를 그 문제에 사용했다면, 여러분은 사이트가 커질수록 빌드 시간이 증가하는 경향이 있다는 것을 알고 있습니다. 이는 응용 프로그램 크기가 증가된 결과로, 응용 프로그램이 수용하는 콘텐츠 양과 렌더링이 얼마나 이루어져야 하는지에 따라 달라집니다. 사이트 성능을 최적화하는 방법은 여러 가지가 있습니다. 그 중 하나는 백엔드 전용("머리 없는") 컨텐츠 관리 시스템을 사용하는 것입니다.

이 기사에서는 콘텐츠 관리에 대한 구조화된 접근 방식을 통해 사이트 효율성, 생산성 및 속도를 향상시키기 위해 헤드리스 CMS인 Sanity를 개츠비와 함께 사용하는 것에 대해 논의할 것이다.

## 개츠비와 함께 Sanity CMS 사용

Gatsby는 데이터 소스에 구애받지 않으므로 API, 데이터베이스, CMS, 정적 파일 및 여러 소스 등 어디에서나 데이터를 한 번에 가져올 수 있습니다. 이 문서에서는 Sanity CMS를 데이터 저장소로 사용할 것입니다.

Sanity는 웹 앱 성능을 향상시키는 콘텐츠에 대한 구조화된 접근 방식을 목표로 컨텐츠를 데이터처럼 취급하고 이미지(이미지 파이프라인), 텍스트(휴대용 텍스트) 및 설계를 관리하는 간결한 기능을 제공합니다. 또한, Sanity Studio는 개발자를 위해 React.js로 구축한 완벽한 기능, 사용자 지정 및 확장 가능한 편집기를 제공합니다.

다음 섹션에서는 컨텐츠 관리를 전적으로 담당하는 프런트 엔드 개츠비 기반 웹 애플리케이션과 헤드리스 CMS 백엔드를 구축한다. 결국 API를 통해 Sanity와 Gatsby를 연결함으로써 Sanity로 콘텐츠를 관리하는 방법과 데이터로 가져오는 방법에 대해 배우게 된다.

## Sanity 시작하기

Sanity를 시작하려면 Sanity CLI 또는 모든 시작 프로젝트를 사용할 수 있습니다.

### 1. Sanity CLI 설치

Sanity CLI를 설치하기 전에 `Node` 및 `npm`이 설치되어 있는지 확인하십시오. 그런 다음 Sanity 계정이 있는지(또는 계정이 생성되었는지) 확인합니다.

설치할 준비가 되었으면 터미널에서 다음 명령을 실행하여 Sanity CLI를 전체적으로 설치하십시오.

```coffeescript
npm install -g @sanity/cli
```

이렇게 하면 CLI를 통해 Sanity와 작업하는 데 필요한 도구가 설치됩니다.

### 2. Sanity 프로젝트 생성

Sanity CLI를 설치한 후 다음 명령을 실행하여 새 Sanity 프로젝트를 생성합니다.

```shell
>sanity init
```

이 명령을 실행하면 아래 이미지와 유사한 출력이 나타나 대화형 Q를 안내합니다.

메시지가 나타나면 아래의 패턴을 따릅니다.

- 사용할 프로젝트 선택 → 새 프로젝트 생성
- 기본 데이터 세트 구성을 사용하시겠습니까? → 예
- 프로젝트 템플릿 선택 → 미리 정의된 스키마가 없는 프로젝트 정리

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/sanity-output.png?resize=706%2C652&ssl=1)

### 3. 프로젝트 실행

프로젝트의 루트에서 명령을 실행하여 Sanity Studio를 시작합니다(포트 3333)

```undefined
sanity start -p 3333
```

이제 프로젝트가 http://localhost:3333에서 실행되고 있어야 합니다.

참고: 인증확인을 사용하여 콘텐츠를 쿼리할지 여부에 따라 로그인하라는 메시지가 표시될 수 있습니다.

### 4. 스키마 편집

이 때 스키마는 비어 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/empty-schema.png?resize=659%2C623&ssl=1)

스키마는 Sanity에서 구조화된 컨텐츠 모델링의 핵심이며 문서가 구성된 필드 유형(문서, 이미지, 객체, 참조 등)을 참조합니다.

예를 들어, 우리는 이름, 제목, 유형, 저자, 발매일 등의 속성을 가진 책 스키마를 만들 것이다.

북 스키마를 만들려면 스키마 폴더에 다음과 같이 `books.js` 파일을 만드십시오.

```js
// schemas are basically objects
export default {
   // The identifier for this document type used in the api's
  name: 'book',

  // This is the display name for the type
  title: 'Books',

  // Schema type of Document
  type: 'document',

  fields: [
    {
      name: 'name',
      title: 'Book Name',
      type: 'string',
      description: 'Name of the book',
    },
  ]
}
```

필드 속성은 스키마의 속성을 정의하는 개체 배열입니다. 첫 번째 필드는 문자열 유형으로 책 이름을 지정합니다.

이제 북 스키마가 생성되었으므로 `schema.js`의 스키마 목록에 추가해야 합니다.

```js
// Default imports from Sanity
import schemaTypes from 'all:part:@sanity/base/schema-type';
import createSchema from 'part:@sanity/base/schema-creator';

// Import the book schema
import book from './book';

export default createSchema({
  name: 'default',
  types: schemaTypes.concat([
    /* Append to the list of schemas */
    book
  ]),
});
```

### 5. Sanity Studio를 통해 게시

스키마를 생성했으므로 이제 Sanity Studio에서 업데이트된 변경 사항을 실행하고 있어야 합니다.

Sanity Studio에는 다음과 같은 세 가지 중요한 기능이 있습니다.

- 스키마 – 스키마 목록을 표시합니다(아래 1열).
- 문서 – 스키마에 작성된 문서(아래 2열)
- 편집 – 스키마에 작성된 필드(아래 3열)

등록하려면, 문서를 작성하십시오.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/schema-example-1024x682.png?resize=1024%2C682&ssl=1)

### 6. 추가 필드 만들기

우리는 더 많은 필드를 만들면 더 자세히 알 수 있습니다. 아래 예에서는 `schma.js`의 기존 `fields` 배열에 `autor`, `release date`, `category`를 추가합니다.

```bash
{
  name: 'author',
  title: 'Author Name',
  type: 'string',
  description: 'Name of the author',
},
{
  name: 'releaseDate',
  title: 'Release Date',
  type: 'date',
  options: {
    dateFormat: 'YYYY-MM-DD',
    calendarTodayLabel: 'Today',
  },
  description: 'Release Date of the book',
},
{
  name: 'category',
  title: 'Book Category',
  type: 'array',
  description: 'Category of the Book',
  of: [
    {
      type: 'reference',
      to: [
        {
          type: 'category',
        },
      ],
    },
  ],
},
```

### 7. 추가 스키마 생성

위의 블록에서 r`lease date는 `of` ~ date type 속성을 가지고 할당됩니다. 반면 category는 category에 `of` 속성이 할당된 참조 유형이지만, 개체 배열인 category 자체는 아직 스키마가 생성되지 않았다.

카테고리 스키마를 만들기 위해, 우리는 북 스키마에 대해 했던 것과 같은 접근 방식을 따를 것입니다.

먼저 `schema` 폴더에 다음과 같은 내용으로 `category.js`를 생성합니다.

```css
export default {
  name: 'category',
  title: 'Categories',
  type: 'document',
  fields: [
    {
      name: 'category',
      title: 'Book Category',
      type: 'string',
      description: 'Category of Book',
    },
  ],
};
```

둘째, `schema.js`의 스키마 목록에 가져와 추가합니다.

```js
// Sanity default imports
import book from './book';
import category from './category';

export default createSchema({
  name: 'default',
  types: schemaTypes.concat([
    /* Append to the list of schemas */
    book,
    category,
  ]),
});
```

마지막으로, 카테고리에 대한 다른 문서를 작성합니다. 이 예에서, 저는 스릴러, 논픽션, 그리고 소설을 선택했습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/categories-schema.png?resize=1024%2C621&ssl=1)

### 8. Sanity 프로젝트 구축

Sanity는 컨텐츠를 API를 통해 데이터로 노출하고 GROQ(Graph Oriented Query Language)로 알려진 GraphQL과 유사한 쿼리 언어를 통해 액세스할 수 있도록 합니다.

Gatsby 데이터 계층은 GraphQL에 의해 작동되기 때문에 Sanity를 통해 데이터에 액세스할 수 있도록 지시하는 것이 쉽다. 이렇게 하려면 아래 명령을 실행하고 질문에 대한 확인을 수행합니다. GraphQL 놀이터를 사용하시겠습니까?

```undefined
sanity graphql deploy
```

그런 다음 Sanity 콘텐츠를 쿼리할 수 있는 GraphQL 놀이터에 배포 URL이 표시됩니다.

다음과 같이 쿼리를 실행하여 `allBook`을 사용하여 모든 책을 가져올 수 있습니다.

```undefined
query {
  allBook {
    name
  }
}
```

프로젝트를 진행하고 스키마를 변경할 때 변경 사항을 업데이트하려면 다시 배포해야 합니다.

만약 당신이 여전히 나와 함께 있다면, 당신은 개츠비로 자료를 가져올 준비가 되어 있습니다.

## 개츠비와 함께 시작하기

진행하기에 앞서, 다음은 개츠비에게 친숙한 몇 가지 뉘앙스입니다.

- 플러그인: 플러그인은 노드 프로젝트의 npm 패키지이다. 일반적으로 사용되는 기능의 코드를 다시 작성하지 않도록 개츠비 앱에서 사용할 플러그인을 설치합니다.
- gatsby-config.js=이 파일은 개츠비 파일 형식인 .gitignore, ESlint의 .eslintrc, Prettierrc와 같은 구성 파일이다.
- Gatsby-browser.js: 이것은 개츠비 사이트와 브라우저 간의 인터페이스입니다. 개츠비 플러그인을 설치할 때마다 gatsby-config.js로 구성합니다.

### 개츠비 사이트 만들기

새 개츠비 앱을 만들려면 개츠비 CLI가 설치되어 있어야 합니다.

```cpp
npm install -g gatsby-cli // Installs the gatbsy CLI globally
```

그리고 개츠비라는 이름의 새로운 개츠비 사이트를 만드세요.

```cpp
gatsby new gatsby // Creates a new gatbsy site named gatsby
```

디렉토리를 새 개츠비 사이트로 변경합니다.

```cpp
cd gatsby // Switch directory into the new gatsby site
```

마지막으로 사이트를 실행합니다.

```undefined
gatsby develop -p 3000 // Instruct Gatsby to run on port 3000
```

정상적으로 작동했다면 이 사이트는 http://localhost:3000:에서 실행되어야 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/gatsby-default.png?resize=694%2C461&ssl=1)

Gatsby GraphQL 작업을 탐색하기 위한 기본 IDE인 GraphiQL은 http://localhost:3000/_graphql에서도 찾을 수 있어야 합니다.

### 개츠비에서 데이터 가져오기

개츠비에서 데이터를 가져오는 것은 그 자체로 전용적인 주제를 다루어야 마땅하지만, 이 기사에서 가장 주목해야 할 것은 개츠비가 데이터 소스에 구애받지 않기 때문에 어디에서든 데이터를 로드할 수 있다는 것이다.

이 튜토리얼의 목적을 위해 개츠비의 그래프QL 데이터 레이어로 데이터를 소싱한 다음 해당 데이터를 쿼리할 것이다. 수동으로 또는 플러그인을 통해 수행할 수 있습니다. 우리의 목적을 위해 우리는 Sanity CMS 플러그인을 사용할 것이다.

### 개츠비의 Sanity CMS에서 데이터 소싱

개츠비-소스-샌티는 개츠비로부터 데이터를 끌어들이는 데 도움을 주는 플러그인이다. 개츠비 앱에서 다음 명령을 실행하여 설치합니다.

```undefined
npm install gatsby-source-sanity
```

그런 다음 `gatsby-config.js` 플러그인 어레이에서 구성합니다.

```undefined
plugins: [
  {
    resolve: 'gatsby-source-sanity',
  },
  // other plugins
]
```

### 플러그인 구성 업데이트 중

플러그인 요구에 대한 옵션 목록(필수 옵션 및 옵션)을 지정할 수 있습니다. 이러한 옵션 중 일부는 각 프로젝트별로 다르며 Sanity 대시보드에서 찾을 수 있는 반면, `watchMode`와 같은 옵션은 그렇지 않습니다.

다음을 사용하여 플러그인 구성 업데이트:

```undefined
plugins: [
  {
    resolve: 'gatsby-source-sanity',
    options: {
      projectId: 'your-project-id',
      dataset: 'your-dataset-name',
      watchMode: true, // Updates your pages when you create or update documents
      token: 'your-token',
    },
  },
]
```

아래 예제의 출력을 참조하십시오.

- `projectId` →에서 Sanity 프로젝트를 고유하게 식별합니다.
- → 이 경우 생산
- → `read-contract` 프로젝트 API를 쿼리합니다(토큰은 중요한 데이터이므로 하드 코딩되어서는 안 됩니다. 대신, 개츠비의 환경변수로부터 읽으세요.)

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/plugin-update.png?resize=637%2C950&ssl=1)

### Sanity에서 Gatsby로 데이터 조회

모든 자격 증명이 설정된 상태에서 Gatsby 서버를 다시 시작한 다음 GraphiQL로 이동하여 다음 쿼리를 실행하여 생성된 모든 책을 가져옵니다.

```undefined
query {
  allSanityBook {
    nodes {
      name
    }
  }
}
```

데이터 쿼리는 페이지 쿼리 또는 정적 쿼리(https://www.gatsbyjs.com/docs/recipes/querying-data/#Query-data-with-the-static 쿼리 구성 요소)를 통해 수행되거나 [StaticQuery](https://www.gatsbyjs.com/docs/recipes/querying-data/#Query-data-with-the-userstatic Query-hook) 후크를 사용하여 수행할 수 있습니다. 주요 차이점은 페이지 쿼리는 페이지에서 사용되는 반면 정적 쿼리는 페이지가 아닌 구성 요소에 사용된다는 점입니다.

페이지 쿼리가 `index.js`로 `index.js`의 Sanity 데이터 쿼리:

```js
import React from 'react';
import { graphql } from 'gatsby';

// Queried data gets passed as props
export default function IndexPage({ data }) {
  const books = data.allSanityBook.nodes
  return <h1>Index Page</h1>
}

// Query data
export const query = graphql`
  query BooksQuery {
    allSanityBook {
      nodes {
        name
      }
    }
  }

```

데이터 쿼리는 먼저 gatbsy에서 graphql을 가져온 다음 명명된 내보내기(export)로 쿼리를 쓰는 방식으로 수행됩니다. 그런 다음 쿼리에서 반환된 `데이터`는 페이지의 기본 내보내기 구성 요소인 `인덱스 페이지`로 전달됩니다. 책은 아래와 같이 페이지에서 사용하거나 다른 구성요소로 전달할 수 있는 일련의 책을 담고 있다.

`index.js`의 최종 업데이트는 다음과 같습니다.

```js
import React from 'react'
import { graphql } from 'gatsby'

export default function IndexPage({ data }) {
  const books = data.allSanityBook.nodes

  return (
    <div className="books-wrap">
      <div className="container">
        <h1 className="heading">Books</h1>
        <ul className="books">
          {books.map(book => (
            <li className="book-item" key={book.name}>
              <h2 className="title">{book.name}</h2>
              <p className="author">Author: {book.author}</p>
              <p className="release-date">Release Date: {book.releaseDate}</p>
              <span className="category">Category: {book.category[0].category}</span>
            </li>
          ))}
        </ul>
      </div>
    </div>
  )
}

export const query = graphql`
  query BooksQuery {
    allSanityBook {
      nodes {
        name
        author
        releaseDate
        category {
          category
        }
      }
    }
  }
```

최종 산출물은 다음과 같습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/allBooks-Sanity.png?resize=1024%2C967&ssl=1)

여기 전체 코드를 잡으세요.

## 결론

콘텐츠는 웹 사이트와 앱이 살아나게 하지만 빌드 속도와 효율성에 부정적인 영향을 주지 않도록 적절히 모델링하고 관리해야 합니다. 개발자는 개츠비와 함께 Sanity CMS를 사용하여 빌드 시간을 줄이고 콘텐츠를 데이터처럼 처리하는 프로그래밍 가능한 최신 플랫폼을 통해 웹 성능을 최적화할 수 있다.