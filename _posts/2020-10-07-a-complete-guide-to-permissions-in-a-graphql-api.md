---
layout: post
title: "GraphQL API의 사용 권한에 대한 전체 가이드"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/GraphQL.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/GraphQL.png?fit=730%2C412&ssl=1)

## 도입

GraphQL은 API 개발을 위한 새로운 표준이 되었다. 그것은 나름대로의 장점과 유연성을 가지고 있다. 이러한 장점 중 하나는 API에서 사용 권한 및 세분화된 액세스 제어를 구현할 수 있다는 것입니다.

대규모 REST API에서는 세분화된 액세스 제어가 매우 어렵습니다. 그래프QL에서는 매우 쉽게 세분화를 달성할 수 있습니다.

이 문서에서는 GraphQL API에서 사용 권한을 구현하기 위한 다양한 패턴을 볼 수 있습니다.

### 액세스 제어 및 권한의 기본 사항

액세스 제어 – 사용자가 API에 액세스할 수 있는 권한이 있는지 확인합니다.

일반적으로 사용자가 로그인하지 않았지만 API 끝점에 로그인한 사용자가 필요한 경우 API에서 인증 오류를 발생시킵니다.

사용자가 로그인했지만 작업을 수행할 수 있는 충분한 권한이 없는 경우 API가 금지된 오류 또는 무단 오류를 발생시킵니다.

사용 권한 – 사용 권한은 사용자가 특정 API에 액세스할 수 있는지 여부를 결정하는 데 도움이 되는 규칙 집합입니다.

사용 권한에 대한 몇 가지 사용 사례를 살펴보겠습니다.

Twitter 앱을 고려하십시오.

- 사용자는 트윗을 쓸 수 있습니다.
- 트윗을 작성한 작성자는 삭제할 수 있습니다.
- 다른 사용자가 트윗을 좋아하거나, 리트윗하거나, 공유할 수 있음
- 다른 사용자가 트윗을 보고할 수 있음
- 트윗의 작성자와 다른 사용자 모두 트윗의 주석 스레드에 참여할 수 있습니다.

보시다시피 권한 수준은 동일하지만 작성자와 다른 사용자 모두 권한 수준이 다릅니다. 모든 사용자 권한이 있지만 활동에 따라 접근 권한이 다릅니다.

## 그래프에서 사용 권한QL

GraphQL API에서는 다양한 방법으로 사용 권한을 구현할 수 있습니다. 예를 들어 스키마에서 GraphQL 지시사항을 통해 사용 권한을 구현하거나 GraphQL 미들웨어(예: 해결 프로그램)에서 사용 권한을 확인하여 사용 권한을 구현할 수 있습니다.

권한의 깊이

그래프QL에서는 사용 권한을 구현하기 위해 모든 깊이로 이동할 수 있습니다.

쿼리 수준 사용 권한, 개체 수준 사용 권한 및 필드 수준 사용 권한을 수행할 수 있습니다.

그래프에서 사용 권한을 구현하는 다양한 방법QL

이러한 권한을 구현하기 위한 다양한 기법이 있다: 지시어, 미들웨어 해결사, GraphQL 실드 라이브러리.

우리는 우리의 예에서 이 모든 기술들을 볼 것이다. 이제 간단한 GraphQL 서버 예를 들어 보겠습니다.

## 간단한 GraphQL API 구축

먼저 새 npm 프로젝트를 만듭니다.

```coffeescript
npm init
```

express, apollo-server-express 및 graphql 패키지를 추가합니다.

```coffeescript
npm i express apollo-server-express
```

그런 다음 샘플 GraphQL 서버로 `index.js` 파일을 생성합니다.

```js
const express = require('express');
const { ApolloServer, gql } = require('apollo-server-express');

// Construct a schema, using GraphQL schema language
const typeDefs = gql`
  type Query {
    hello: String
  }
`;

// Provide resolver functions for your schema fields
const resolvers = {
  Query: {
    hello: () => 'Hello world!',
  },
};

const server = new ApolloServer({ typeDefs, resolvers });
const app = express();
server.applyMiddleware({ app });

app.listen({ port: 4000 }, () =>
  console.log(`🚀 Server ready at http://localhost:4000${server.graphqlPath}`)
);
```

서버를 로컬로 실행할 데몬을 추가하지 않습니다. Nodemon은 개발 중에 변경된 파일을 확인하여 서버를 다시 로드하는 데 도움이 됩니다.

```coffeescript
npm i -D nodemon
```

` 패키지에서 서버를 실행할 스크립트를 추가합니다.json 파일:

```bash
"scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js"
},
```

터미널에서 서버를 시작합니다.

```coffeescript
npm run dev
```

서버는 포트 `4000`에서 열립니다.

파일을 몇 개 더 만들고 코드를 나누자. GraphQL 유형 정의에 `schma.js` 파일을 사용합니다.

```js
// schema.js

const { gql } = require("apollo-server-express");
// Construct a schema, using GraphQL schema language
module.exports = gql`
  type Query {
    hello: String
  }
`;
```

그리고 해결사의 `resolver.js` 파일:

```js
// schema.js

const { gql } = require("apollo-server-express");
// Construct a schema, using GraphQL schema language
module.exports = gql`
  type Query {
    hello: String
  }
`;
```

간단한 트윗 응용 프로그램에 대한 데이터 및 해당 쿼리 및 해결 프로그램을 만들어 보겠습니다.

data.js 파일은 다음과 같습니다.

```undefined
// data.js

module.exports = [
  {
    id: 0,
    content: "HTML is a programming language",
    author: "Param",
  },
  {
    id: 1,
    content: "JavaScript programmers are beginners",
    author: "Param",
  },
  {
    id: 2,
    content: "HTML and CSS pages are enough to build a bank project",
    author: "Joshua",
  },
  {
    id: 3,
    content: "React Js can prove earth as flat in 2025",
    author: "Joshua",
  },
];
```

이제 이 Tweet 데이터를 표시하기 위해 쿼리 및 해결사를 만듭니다.

```js
// schema.js

const { gql } = require("apollo-server-express");
// Construct a schema, using GraphQL schema language
module.exports = gql`
  type Query {
    hello: String
    tweets: [Tweet]!
    tweet(id: Int!): Tweet!
  }

  type Tweet {
    id: Int!
    content: String!
    author: String!
  }
`;
```

tweets 및 tweet 쿼리의 해결사:

```js
// resolvers.js

const { ApolloError } = require("apollo-server-express");
const tweets = require("./data");

// Provide resolver functions for your schema fields
module.exports = {
  Query: {
    hello: () => "Hello world!",
    tweets: () => {
      return tweets;
    },
    tweet: (_, { id }) => {
      const tweetId = tweets.findIndex((tweet) => tweet.id === id);
      if (tweetId === -1) return new ApolloError("Tweet not found");
      return tweets[tweetId];
    },
  },
};
```

이제 기본 GraphQL 서버가 준비되었습니다. 예를 통해 그래프QL 사용 권한에 대해 살펴보겠습니다.

### 쿼리 수준 사용 권한: 로그인한 사용자만 트윗에 액세스할 수 있습니다.

> 이 예들은 가상적이고 오직 학습 목적만을 위한 것이다. 실제 애플리케이션에서는 이러한 시나리오의 대부분이 이치에 맞지 않습니다.

이 예에서는 먼저 HTTP 요청에서 로그인한 사용자의 세부 정보를 가져와야 합니다.

이 권한은 다양한 방법으로 구현될 수 있습니다.

해결사 내부의 순진한 솔루션

먼저 사용자를 요청 컨텍스트에 추가합니다. 단순성을 위해 헤더 사용자를 요청에 전달합니다.

```coffeescript
// index.js
...
const server = new ApolloServer({
  typeDefs,
  resolvers,
  context: ({ req }) => {
    return {
      user: req.headers.user || "",
    };
  },
});
...
```

그런 다음 로직을 추가하여 로그인한 사용자만 트윗 쿼리를 볼 수 있도록 합니다.

```coffeescript
// resolvers.js
const { ApolloError, ForbiddenError } = require("apollo-server-express");

...
tweet: (_, { id }, { user }) => {
      // Check whether user is logged-in
      if (!user) return new ForbiddenError("Not Authorized");
      
      const tweetId = tweets.findIndex((tweet) => tweet.id === id);
      if (tweetId === -1) return new ApolloError("Tweet not found");
      return tweets[tweetId];
}
...
```

놀이터에서 사용자를 지나가면 다음과 같은 트윗이 표시됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/localhost.png?resize=730%2C400&ssl=1)

헤더에 전달된 사용자가 없으면 `Forbided Error`가 발생합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/tweet-errors.png?resize=730%2C375&ssl=1)

이러한 보호 방식은 확장성과 재사용이 불가능합니다. 하지만 여전히 이 사용 사례에 효과적입니다.

### 그래프QL 지시어 사용

GraphQL 지시어 `isLoggedin`을 작성하여 `tweet` 쿼리에 적용합니다.

```java
// schema.js

const { gql } = require("apollo-server-express");

// Construct a schema, using GraphQL schema language
module.exports = gql`
  directive @isLoggedin on FIELD_DEFINITION
  type Query {
    hello: String
    tweets: [Tweet]!
    tweet(id: Int!): Tweet! @isLoggedin
  }
  ...
`;
```

그런 다음 GraphQL 서버에 지시문의 로직을 추가합니다. `directions.js` 파일 생성:

```js
// directives.js

const {
  ForbiddenError,
  SchemaDirectiveVisitor,
} = require("apollo-server-express");
const { defaultFieldResolver } = require("graphql");

class isLoggedinDirective extends SchemaDirectiveVisitor {
  visitFieldDefinition(field) {
    const originalResolve = field.resolve || defaultFieldResolver;
    field.resolve = async function (...args) {
      const context = args[2];
      const user = context.user || "";
      if (!user) {
        throw new ForbiddenError("Not Authorized");
      }
      const data = await originalResolve.apply(this, args);
      return data;
    };
  }
}

module.exports = { isLoggedinDirective };
```

지시문을 만드는 구문은 좀 터무니없다. 당신은 아폴로 문서에서 더 많은 것을 탐험할 수 있습니다. 서버에 대한 지시사항을 연결해야 합니다.

```js
// index.js

const { isLoggedinDirective } = require("./directives");

...
const server = new ApolloServer({
  typeDefs,
  resolvers,
  schemaDirectives: {
    isLoggedin: isLoggedinDirective,
  },
  context: ({ req }) => {
    return {
      user: req.headers.user || "",
    };
  },
});
...
```

서버를 실행한 후 GraphQL 놀이터에서 With 및 With user(사용자 없음) 헤더를 확인합니다. 그 지시문은 우리의 순진한 해결책과 똑같이 작동하겠지만, 논리가 분리되어 있기 때문에 여러 장소에서 쉽게 재사용할 수 있다.

### 그래프QL 미들웨어 사용

미들웨어도 해결 방법입니다. 쿼리를 직접 확인하는 대신 먼저 미들웨어 해결사에서 검사가 수행된 후 다음 해결사로 전달됩니다. 이 기술을 사용하여 여러 해결사를 결합할 수 있습니다.

이를 위해 graphql-resolvers라는 패키지를 사용할 예정입니다.

```coffeescript
npm i graphql-resolvers
```

`middlewares.js` 파일에 `isLoggedin` 미들웨어 해결사를 생성해 보겠습니다.

```js
// middlewares.js

const { ForbiddenError } = require("apollo-server-express");

// Middleware resolver
const isLoggedin = (parent, args, { user }, info) => {
  if (!user) throw new ForbiddenError("Not Authorized");
};

module.exports = { isLoggedin };
```

이제 이 미들웨어를 tweet 해결사에 추가할 수 있습니다.

```coffeescript
// resolvers.js
...
const { combineResolvers } = require("graphql-resolvers");
const { isLoggedin } = require("./middlewares");

...
tweet: combineResolvers(isLoggedin, (_, { id }) => {
      const tweetId = tweets.findIndex((tweet) => tweet.id === id);
      if (tweetId === -1) return new ApolloError("Tweet not found");
      return tweets[tweetId];
})
...
```

combineResolvers를 사용하면 실제 쿼리를 해결하기 전에 여러 미들웨어 리졸버를 결합할 수 있다.

이 방법은 실제 해결 방법에서 로직을 추출하기 때문에 재사용할 수도 있습니다.

## 개체 수준 사용 권한

동일한 예제를 사용하여 전체 Tweet 유형에 지시어를 적용합니다. 그런 다음 개체 수준 사용 권한을 사용하도록 설정합니다. 쿼리만 보호하는 대신 모든 쿼리에서 전체 개체를 보호할 수 있습니다.

예를 들어 보자. 이 예는 스키마 지시어 예제와 유사합니다.

```java
// schema.js

const { gql } = require("apollo-server-express");

// Construct a schema, using GraphQL schema language
module.exports = gql`
  directive @isLoggedin on OBJECT
  
  type Query {
    hello: String
    tweets: [Tweet]!
    tweet(id: Int!): Tweet!
  }
  
  type Tweet @isLoggedin {
    id: Int!
    content: String!
    author: String!
  }
`;
```

지시어는 `트윗` 유형에 첨부되어 있습니다. 또한 만약 당신이 알아차린다면, 우리는 OBJECT에 대한 지시 선언을 변경하였습니다. 이를 위해서는 `directions.js`에서 약간의 수정이 필요하다.

```js
// directives.js

const {
  ForbiddenError,
  SchemaDirectiveVisitor,
} = require("apollo-server-express");
const { defaultFieldResolver } = require("graphql");

class isLoggedinDirective extends SchemaDirectiveVisitor {
  visitObject(obj) {
    const fields = obj.getFields();
    Object.keys(fields).forEach((fieldName) => {
      const field = fields[fieldName];
      const originalResolve = field.resolve || defaultFieldResolver;
      field.resolve = async function (...args) {
        const context = args[2];
        const user = context.user || "";
        if (!user) {
          throw new ForbiddenError("Not Authorized");
        }
        const data = await originalResolve.apply(this, args);
        return data;
      };
    });
  }
}

module.exports = { isLoggedinDirective };
```

로그인하지 않고 `트위츠` 쿼리를 확인하면 `트위츠` 쿼리가 일련의 트윗을 반환하기 때문에 오류가 발생합니다.

헤더에 사용자 없음:

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/error-messages.png?resize=730%2C400&ssl=1)

헤더에 사용자 포함:

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/data.png?resize=730%2C375&ssl=1)

## 필드 수준 권한

필드 수준의 세분화된 권한도 수행할 수 있습니다. 예를 들어 관리자 역할을 가진 사용자는 특정 필드를 볼 수 있습니다. 한편, 일반 액세스 권한을 가진 사용자는 이러한 필드를 볼 수 없습니다.

이러한 시나리오의 대표적인 예가 청구 및 구독 정보입니다. 관리자만 해당 정보를 볼 수 있고 나머지 사용자는 해당 정보를 볼 수 없습니다. GraphQL API에서 이와 같은 세분화된 사용 권한을 부여할 수 있습니다.

마지막 예에서는 지시어를 사용했습니다. 우리는 새로운 기술을 사용하여 현장 레벨 허가를 할 것이다. 앞서 말씀드린 것처럼 여러 가지 방법으로 권한을 구현할 수 있습니다. 하나를 고르는 것은 당신에게 달려 있고 그것은 프로젝트에 달려 있다.

### GraphQL 실드 라이브러리 사용

GraphQL 실드는 GraphQL API에서 매우 광범위하거나 철저한 권한 설정을 수행하는 데 사용될 수 있다. graphql-shield를 사용하여 세분화된 사용 권한을 얻는 방법에 대해 알아보겠습니다.

설치 방법:

```css
npm i graphql-shield graphql-middleware @graphql-tools/schema
```

그런 다음 `permissions.js` 파일을 만듭니다. 모든 요청을 거부하고 `안녕` 쿼리만 허용하겠습니다.

```java
// permissions.js

const { allow, deny, shield } = require("graphql-shield");

const permissions = shield({
  Query: {
    "*": deny,
    hello: allow,
  },
});

module.exports = permissions;
```

와일드카드는 모든 요청을 거부한 다음 `hello` 쿼리를 허용한다. 이제 서버에서 사용 권한을 적용합니다.

```js
// index.js

const express = require("express");
const { ApolloServer } = require("apollo-server-express");
const { applyMiddleware } = require("graphql-middleware");
const { makeExecutableSchema } = require("@graphql-tools/schema");

const typeDefs = require("./schema");
const resolvers = require("./resolvers");
const permissions = require("./permissions");

const schema = makeExecutableSchema({
  typeDefs,
  resolvers,
});

const server = new ApolloServer({
  schema: applyMiddleware(schema, permissions),
  resolvers,
  context: ({ req }) => {
    return {
      user: req.headers.user || "",
    };
  },
});

const app = express();
server.applyMiddleware({ app });

app.listen({ port: 4000 }, () =>
  console.log(`🚀 Server ready at http://localhost:4000${server.graphqlPath}`)
);
```

Tweets 또는 tweet 쿼리를 확인합니다. `Not Authorized!`를 던질 것이다.에러 hello 쿼리만 작동합니다.

이제 모든 쿼리를 허용하고 `트윗` 유형의 `작성자` 필드만 거부합니다.

```java
// permissions.js

const { allow, shield, deny } = require("graphql-shield");

const permissions = shield({
  Query: {
    "*": allow,
  },
  Tweet: {
    author: deny,
  },
});

module.exports = permissions;
```

작성자 필드가 있는 쿼리를 보내면 다음과 같은 오류가 표시됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/not-authorized.png?resize=730%2C400&ssl=1)

작성자 없이 요청을 보내면 다음과 같은 `트위트` 데이터가 표시됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/data-tweets.png?resize=730%2C375&ssl=1)

이제 완전히 거부하지 않고 사용자가 로그인한 경우에만 작성자를 표시하는 규칙을 추가합니다. 그렇지 않으면 `작성자` 필드에 대해 쿼리하는 요청을 거부합니다.

GraphQL 실드 라이브러리는 광범위한 방법으로 규칙을 만들 수 있습니다. wW는 `isLoggedin` 규칙을 만들 수 있습니다.

```js
// permissions.js

const { ForbiddenError } = require("apollo-server-express");
const { allow, shield, rule } = require("graphql-shield");

// Rule for shield
const isLoggedin = rule({ cache: "contextual" })(
  async (parent, args, { user }, info) => {
    if (user) return true;
    return new ForbiddenError("Not Authorized");
  }
);

const permissions = shield({
  Query: {
    "*": allow,
  },
  Tweet: {
    author: isLoggedin,
  },
});

module.exports = permissions;
```

필드가 요청되면 GraphQL 실드가 사용자를 확인합니다. 사용자가 헤더에 있으면 tweet 결과를 허용합니다. 그렇지 않으면 요청을 거부합니다.

헤더에 사용자 없음:

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/Without-user.png?resize=730%2C375&ssl=1)

머리글에 사용자 포함:

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/authorized-tweets..png?resize=730%2C400&ssl=1)

## 결론

GraphQL API에서 사용 권한 구현에 대해 다루었다. GraphQL API에서 사용 권한을 구현하는 데 유용한 기사가 발견되면 코멘트로 알려주세요.

소스 코드는 여기에서 찾을 수 있습니다. 각 예를 보려면 서로 다른 분기를 찾아야 합니다. 모두 쉽게 탐색할 수 있도록 숫자 앞에 숫자가 붙어 있습니다.