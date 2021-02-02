---
layout: post
title: "GraphQL 모듈 자습서 : GraphQL 스키마를 모듈화하는 방법
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/Screen-Shot-2021-01-28-at-10.37.23-AM.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Screen-Shot-2021-01-28-at-10.37.23-AM.png?fit=730%2C484&ssl=1)

이 튜토리얼에서는 GraphQL 모듈을 사용하여 GraphQL 애플리케이션을 단순하고 재사용 가능한 기능 기반 모듈로 나누는 방법을 보여줍니다.
 

다음 내용을 다룹니다.
 

- GraphQL 모듈이란 무엇입니까?
 
- GraphQL 모듈을 사용하는 이유는 무엇입니까?
 
- GraphQL 모듈의 작동 원리
 
- GraphQL 모듈과 스키마 병합
 
- 의존성 주입
 

## GraphQL 모듈이란 무엇입니까?
 

GraphQL 모듈은 재사용, 유지 관리, 테스트 및 확장 가능한 모듈을 만드는 데 도움이되도록 설계된 라이브러리 및 지침의 집합입니다.
 GraphQL 모듈의 기본 개념은 GraphQL 서버를 더 작고 독립적 인 기능 기반 모듈로 분리하는 것입니다.
 

GraphQL 모듈은 GraphQL에서 관심사 디자인 패턴의 분리를 구현하는 데 도움이되므로 필요한 작업 만 수행하는 간단한 모듈을 작성할 수 있습니다.
 이렇게하면 코드를 더 쉽게 작성, 테스트 및 유지 관리 할 수 있습니다.
 

GraphQL 서버의 초기 기본 구현 구조는 다음과 같습니다.
 

GraphQL 애플리케이션이 성장함에 따라 스키마 및 리졸버의 복잡성도 증가하여 스키마 유지 관리가 어려워 질 수 있습니다.
 코드를 구성하려면 스키마 유형 및 관련 리졸버를 여러 파일 또는 모듈로 분리하고 모듈 아래에 앱의 특정 부분과 관련된 모든 항목을 추가해야합니다.
 graphql-modules 라이브러리는 이러한 점진적 프로세스를 용이하게합니다.
 

`graphql-modules`를 사용하면 앱이 다음과 같이 구성됩니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/GraphQl-Modules-Library-App-Structure.png?resize=496%2C601&ssl=1)

## GraphQL 모듈을 사용하는 이유는 무엇입니까?
 

GraphQL 모듈을 사용하는 대부분의 개발자는 재사용 가능한 모듈, 확장 가능한 구조, 명확한 성장 경로 및 테스트 가능성을위한 도구 세트를 선택합니다.
 개발자가 GraphQL 앱을 위해 재사용 가능하고 확장 가능한 모듈을 작성하기 위해 GraphQL 모듈로 전환하는 몇 가지 이유를 자세히 살펴 보겠습니다.
 

- 재사용 가능한 모듈 : 모듈은 GraphQL 스키마에 의해 더 작은 조각으로 정의되며 나중에 이동하고 재사용 할 수 있습니다.
 
- 확장 가능한 구조 : 각 모듈에는 명확하게 정의 된 경계가있어 여러 팀과 기능, 여러 마이크로 서비스 및 서버를 훨씬 쉽게 관리 할 수 있습니다.
 모듈은 사용자 지정 메시지를 사용하여 서로 통신 할 수 있습니다.
 
- 명확한 성장 경로 : 기능을 모듈로 분리하여 애플리케이션이 매우 간단하고 빠른 단일 파일 모듈에서 확장 가능한 다중 파일, -team, -repo 및 -server 모듈로 어떻게 성장할 수 있는지 쉽게 확인할 수 있습니다.
 
- 테스트 가능성 : 작은 코드 조각을 테스트하는 것이 큰 코드 청크를 테스트하는 것보다 쉽습니다.
 GraphQL 모듈은 애플리케이션을 테스트하고 모의하기위한 풍부한 도구 세트를 제공합니다.
 

## GraphQL 모듈의 작동 원리
 

간단한 라이브러리 애플리케이션을 사용하여 GraphQL 애플리케이션을 모듈로 나누는 방법을 보여 드리겠습니다.
 애플리케이션은 `Book`, `Author`, `Genre`모듈을 가지며 각 모듈은 유형 정의와 리졸버 함수로 구성됩니다.
 

### GraphQL 모듈 만들기
 

`graphql-modules` 사용을 시작하려면 패키지와`graphql`을 설치하기 만하면됩니다.
 

```undefined
// NPM
npm install --save graphql graphql-modules

//Yarn 

yarn add graphql graphql-modules
```

모듈을 생성하려면`createModule`을 사용하십시오.
 

```coffeescript
import { createModule } from 'graphql-modules';

export const myModule = createModule({
  id: 'my-module',
  dirname: __dirname,
  typeDefs: [
    `type Query {
      hello: String!
    }`,
  ],
  resolvers: {
    Query: {
      hello: () =&gt; 'world',
    },
  },
});
```

고유 한`id`를 모듈에 추가하는 것은 유형 정의에서 문제를 찾는 데 도움이되기 때문에 중요합니다.
 `dirname`은 선택 사항이지만 예외가 발생할 때 올바른 파일을 일치시키는 것이 더 간단합니다.
 

### 유형 정의 및 해석기
 

스키마를 정의하기 위해 GraphQL 모듈은 GraphQL 스키마와 마찬가지로 SDL (스키마 정의 언어)을 사용합니다.
 

GraphQL 모듈은 다른 GraphQL 구현과 동일한 방식으로 리졸버를 구현합니다.
 GraphQL 모듈 문서에 따르면 GraphQL 모듈로 생성 된 모듈은 중복되거나 부정확하거나 오래된 리졸버 (예 : 유형 정의 또는 확장과 일치하지 않는 리졸버)를 감지 할 수 있습니다.
 

### 책 모듈 만들기
 

GraphQL 앱용 책 모듈을 만드는 방법은 다음과 같습니다.
 

```coffeescript
// book.type.graphql
import { gql } from 'graphql-modules';

export const Book = gql`
  type Query {
      book(id: ID!): Book
  }
  type Book {
    id: String
    title: String
    author: Author
    summary: String
    isbn: String
    genre: [Genre]
    url: String
  }
`;


// book.resolver.graphql

export const BookResolver = {
    Query: {
      book(root, { id }) {
        return {
          _id: id,
          title: "To The Lighthouse",
          author: "Virginia Woolf",
          summary:"Book summary",
          isbn: "12345678EDB"
          genre: ["ficton"],
          url: "http://lighouse.com"
        };
      },
    },
    Book: {
      id(book) {
        return book._id;
      },
      title(book) {
        return book.title;
      },
      author(book) {
        return book.author;
      },
      summary(book) {
        return book.summary;
      },
      isbn(book) {
        return book.isbn;
      },
      genre(book) {
        return book.genre;
      },
      url(book) {
        return book.url;
      },
    },
  },



// book.module.graphql.ts
import{ Book } from './book.type.graphql';
import { BookResolver } '.book.resolver.graphql';
import { createModule } from 'graphql-modules';

export const BookModule = createModule({
  id: 'book-module',
  dirname: __dirname,
  typeDefs: [Book],
  resolvers: [BookResolvers]
});
```

### 기타 모듈
 

예제 GraphQL 라이브러리 앱에 대한 저자 및 장르 모듈을 생성하려면 다음 단계를 따르십시오.
 

```js
// author.module.graphql.ts
import{ Author } from './author.type.graphql';
import { AuthorResolver } '.author.resolver.graphql';
import { createModule } from 'graphql-modules';

export const AuthorModule = createModule({
  id: 'book-module',
  dirname: __dirname,
  typeDefs: [Author],
  resolvers: [AuthorResolvers]
});


// genre.module.graphql.ts
import{ Genre } from './genre.type.graphql';
import { GenreResolver } '.genre.resolver.graphql';
import { createModule } from 'graphql-modules';

export const GenreModule = createModule({
  id: 'book-module',
  dirname: __dirname,
  typeDefs: [Genre],
  resolvers: [GenreResolvers]
});
```

## GraphQL 모듈과 스키마 병합
 

우리가 정의한 각 모듈은 전체 스키마의 작은 부분에 기여합니다.
 

스키마를 병합하려면 GraphQL Modules의`createApplication`을 사용하여`application`을 만듭니다.
 

```js
import { createApplication } from 'graphql-modules';
import { GenreModule } from './genre/genre.module.graphql';
import { BookModule } from './book/book.module.graphql';
import { AuthorModule } from './author/author.module.graphql';

export const application = createApplication({
  modules: [BookModule, AuthorModule, GenreModule],
});
```

`application`에는 GraphQL 스키마와 그 구현이 포함됩니다.
 GraphQL 서버에서 소비 가능하게 만들어야합니다.
 

GraphQL 모듈에는 Apollo, Express GraphQL 및 GraphQL Helix와 같은 인기있는 GraphQL 서버에 대한 다양한 구현이 있습니다.
 Apollo Server를 사용하는 경우`createSchemaForApollo`를 사용하여이 서버에 맞게 조정되고 완벽하게 통합되는 스키마를 얻을 수 있습니다.
 

```js
import { ApolloServer } from 'apollo-server';
import { application } from './application';

const schema = application.createSchemaForApollo();

const server = new ApolloServer({
  schema,
});

server.listen().then(({ url }) =&gt; {
  console.log(`🚀 Server ready at ${url}`);
});
```

## GraphQL '컨텍스트'
 

GraphQL에서`context` 인수는 특정 작업을 위해 실행중인 모든 리졸버에서 공유됩니다.
 `컨텍스트`를 사용하여 인증 데이터, 데이터 로더 인스턴스 등을 포함한 작업 별 상태를 공유 할 수 있습니다.
 

GraphQL 모듈에서 `context`는 각 리졸버의 세 번째 인수로 사용할 수 있으며 모듈간에 공유됩니다.
 

```cpp
const resolvers = {
  Query: {
    myQuery(root, args, context, info) {
      // ...
    },
  },
};
```

## 의존성 주입
 

애플리케이션이 확장됨에 따라 특정 비즈니스 로직을 해석기에서 서비스로 분리해야 할 수 있습니다.
 GraphQL 모듈은 리졸버에서 서비스를 사용할 수 있도록 도와주는 DI (종속성 주입)를 지원합니다.
 

종속성은 리졸버가 기능을 수행하는 데 필요한 서비스 또는 개체입니다.
 따라서 리졸버는 서비스를 생성하는 대신 외부 리소스에서 요청합니다.
 

종속성 주입은 코드베이스가 상당히 크고 빠르게 이동해야하는 경우에만 의미가 있다는 점에 유의해야합니다.
 

GraphQL Modules의 DI를 사용하려면 이해해야 할 몇 가지 용어가 있습니다.
 

- `injector`는 캐시에서 명명 된 종속성을 찾거나 구성된 공급자를 사용하여 종속성을 생성 한 다음 해당 범위에 따라 모듈 또는 전체 애플리케이션에서 사용할 수 있도록하는 DI 시스템의 객체입니다.
 
- `InjectionToken`은 종속성 주입 공간에서 객체 또는 값을 나타내는 심볼 (토큰) 또는 클래스 (서비스 클래스)입니다.
 
- `provider`는 값을 정의하고 `Token`또는 `Service`클래스와 일치시키는 방법입니다.
 특정 주입 토큰의 가치를 제공합니다.
 공급자는 리졸버 또는 기타 서비스에 주입됩니다.
 

공급자에는 클래스 공급자, 값 공급자 및 공장 공급자의 세 가지 종류가 있습니다.
 각각에 대해 자세히 살펴 보겠습니다.
 

### 클래스 제공자
 

클래스 공급자는 클래스의 인스턴스를 만들고 인젝터에서 사용할 수 있도록합니다.
 서비스 클래스를 공급자로 사용하려면`@Injectable ()`데코레이터로 데코 레이팅해야합니다.
 

```js
// book.service.ts
import { Injectable } from 'graphql-modules';

@Injectable()
export class BookService {}


// book.module.graphql.ts
import{ Book } from './book.type.graphql';
import { BookResolver } '.book.resolver.graphql';
import { createModule } from 'graphql-modules';

export const BookModule = createModule({
  id: 'book-module',
  dirname: __dirname,
  typeDefs: [Book],
  resolvers: [BookResolvers],
  providers: [BookService]
});
```

`context`개체는 모든 리졸버에서 사용할 수 있습니다.
 이`context` 객체는 해석기에서 제공된 서비스 클래스에 액세스하는 데 사용할 수있는`injector`라는 또 다른 객체를 포함합니다.
 

```js
// book.resolver.graphql
import { BookService } from './book.service.ts'
export const BookResolver = {
    Query: {
      book(root, { id }, context) {
        const bookService = context.injector.get(BookService);
        return {
          _id: id,
         // ...
        };
      },
    },
    Book: {
      id(book) {
        return book._id;
      },
      // ...
    },
  },
```

클래스에서 사용하려면 생성자에서 요청하면됩니다.
 

```js
import { Injectable } from 'graphql-modules';
import { BookService } from './book';

@Injectable()
class AuthorService {
  constructor(private bookService: BookService) {}

  allbooks(authorId) {
     return this.bookService.books(authorId)
  }
}
```

### 가치 제공자
 

값 공급자는 바로 사용할 수있는 값을 제공합니다.
 `InjectionToken`또는 클래스 값을 나타내는 토큰이 필요합니다.
 

```js
import { InjectionToken } from 'graphql-modules';
const ApiKey = new InjectionToken&lt;string&gt;('api-key');
import { createModule } from 'graphql-modules';

export const BookModule = createModule({
  id: 'book-module',
  /* ... */
  providers: [
    {
      provide: ApiKey,
      useValue: 'my-api-key',
    },
  ],
});
```

### 공장 공급자
 

팩토리 공급자는 값을 제공하는 기능입니다.
 런타임까지 사용할 수없는 정보를 기반으로 동적으로 종속 값을 만들어야 할 때 유용합니다.
 응용 프로그램 상태의 특정 조건에 따라 반환 할 값을 정보에 입각 한 결정을 내릴 수 있습니다.
 

팩토리 공급자는 타사 라이브러리를 사용할 때와 같이 클래스의 인스턴스를 만드는데도 유용합니다.
 팩토리 함수에는 종속성이있는 선택적 인수가있을 수 있습니다.
 

```js
import { InjectionToken } from 'graphql-modules';
const ApiKey = new InjectionToken&lt;string&gt;('api-key');
import { createModule } from 'graphql-modules';

export const BookModule = createModule({
  id: 'book-module',
  /* ... */
  providers: [
    {
      provide: ApiKey,
      useFactory(config: Config) {
        if (context.environment) {
          return 'my-api-key';
        }

        return null;
      },
      deps: [Config],
    },
  ],
});
```

리졸버에서 토큰에 액세스하는 것은 서비스에 액세스하는 것과 유사합니다.
 `@ Inject` 데코레이터는 생성자의 토큰에 액세스하는 데 사용됩니다.
 

```coffeescript
import { Injectable } from 'graphql-modules';
import { BookService } from './book';

@Injectable()
class AuthorService {
  constructor(@Inject(ApiKey) private key: string, private bookService: BookService) {}

  allbooks(authorId) {
     return this.bookService.books(authorId)
  }
}
```

### GraphQL 범위
 

각 토큰 또는 공급자에는 수명주기를 정의하는 데 사용되는 범위가 있습니다.
 `범위`는 싱글 톤 또는 작업 일 수 있습니다.
 싱글 톤으로 정의 된 모든 공급자 또는 토큰은 한 번 생성되며 들어오는 모든 GraphQL 작업에 동일한 인스턴스를 사용할 수 있습니다.
 싱글 톤 제공자는 애플리케이션의 라이프 사이클 전체에 걸쳐 존재하며 GraphQL 모듈의 기본 및 권장 범위입니다.
 

반면에 작업 공급자는 실행 컨텍스트 또는이를 필요로하는 각 GraphQL 작업에 대해 생성됩니다.
 작업 공급자는이를 인스턴스화 한 GraphQL 작업의 수명 주기만 유지합니다.
 

```js
// genre.service.ts
import { Injectable, Scope } from 'graphql-modules';

@Injectable({
  scope: Scope.Operation,
})
export class GenreService {}


// genre.module.graphql.ts
import{ Genre } from './genre.type.graphql';
import { GenreResolver } '.genre.resolver.graphql';
import { createModule } from 'graphql-modules';
import { GenreService } from './genre.service.ts'

export const GenreModule = createModule({
  id: 'book-module',
  dirname: __dirname,
  typeDefs: [Genre],
  resolvers: [GenreResolvers],
  providers: [GenreService]
});
```

리졸버가`GenreService`를 호출하지 않으면 서비스가 생성되지 않습니다.
 

## 결론
 

유지 관리 가능한 코드는 쉽게 확장 할 수 있으며 GraphQL 모듈은이를 달성하는 데 도움을주는 것을 목표로합니다.
 

코드 분리 또는 모듈화가 개발 중에 만 발생한다는 것은 가치가 없습니다.
 그러나 런타임에는 통합 스키마가 계속 제공됩니다.
 

이제 GraphQL 모듈 사용을 시작하는 데 필요한 모든 것이 준비되어 있습니다.
 구독, 미들웨어, 실행 컨텍스트 및 수명주기 후크와 같은 다른 고급 개념에 대해 알아 보려면 GraphQL 모듈 문서를 확인하세요.
 