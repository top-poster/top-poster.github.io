---
layout: post
title: "GraphQL 서버를 릴레이와 호환되도록 설정"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/makingagraphqlservercompatiblewithrelay.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/makingagraphqlservercompatiblewithrelay.png?fit=730%2C487&ssl=1)

GraphQL은 더 나은 API를 구축할 수 있는 매우 강력한 기술입니다. GraphQL은 서버에 대한 불필요한 요청을 피하고, 과도한 가져오기 및 언더페치 요청을 줄이고, 필요한 데이터만 정확하게 가져올 수 있도록 도와주는 쿼리 언어이다. 기술이 점점 더 많이 사용되고 있고, GraphQL을 다른 언어로 채택하는 것이 점점 더 커지고 있으며, GraphQL API를 구축하고자 하는 사람들에게는 미래가 밝습니다.

Relay는 GraphQL과 함께 선언적으로 작업하고 데이터 중심 React 애플리케이션을 구축하기 위한 강력한 JavaScript 프레임워크이다. 커뮤니티에서 가장 많이 사용하는 GraphQL 클라이언트는 아니며 몇 가지 이유가 있습니다. 그 중 하나는 릴레이가 다른 프레임워크보다 더 체계적이고 더 많은 의견을 제시한다는 것입니다. 설명서는 매우 직관적이지 않으며 커뮤니티 자체도 그리 크지 않습니다. 아폴로는 주로 커뮤니티와 문서와 관련된 릴레이에 비해 몇 가지 장점이 있지만, 릴레이는 이를 매우 특별하게 만드는 몇 가지 장점을 가지고 있습니다.

릴레이는 수백만의 사용자에게 쉽게 확장할 수 있는 보다 구조적이고 모듈적이며 미래 지향적인 애플리케이션을 위해 프런트 엔드에서 사용하는 것이 좋습니다. 릴레이를 사용하기 위해 가장 먼저 구현해야 할 것은 릴레이 호환 그래프QL 서버를 만드는 것이다. 그것이 우리가 지금 할 일입니다.

## 그래프QL 서버 사양

릴레이는 캐싱 및 데이터 가져오기를 처리할 때 우아한 방식으로 작동하며, 다른 GraphQL 클라이언트에 비해 가장 큰 장점 중 하나입니다.

릴레이에는 설명서에 GraphQL Server 사양이라는 것이 있습니다. 이 가이드에는 올바르게 작동하기 위해 릴레이가 GraphQL 서버에 대해 만든 규칙이 나와 있습니다.

릴레이와 함께 사용할 새 GraphQL 서버를 생성할 때는 다음 원칙을 따라야 합니다.

## '노드' 개체

Node 인터페이스는 설명서에 나와 있듯이 객체를 다시 세팅하는 데 사용됩니다.

> 서버는 'Node'라는 인터페이스를 제공해야 합니다. 이 인터페이스에는 Null이 아닌 ID를 반환하는 ID라는 필드가 하나만 있어야 합니다.이 'id'는 이 개체에 대해 전역적으로 고유한 식별자여야 하며, 이 'id'만 주어지면 서버는 해당 개체를 다시 가져올 수 있어야 합니다.

노드 인터페이스는 GraphQL 스키마가 ID를 사용하여 객체를 요청하는 표준 방식을 갖는 데 매우 중요하다.

노드 루트 필드도 구현해야 한다. 이 필드는 Null이 아닌 전역 고유 `ID`를 하나의 인수로만 사용합니다.

> 쿼리가 'Node'를 구현하는 객체를 반환하는 경우, 'Node'의 'id' 필드에서 서버가 반환한 값이 'id' 매개 변수로 'node' 루트 필드에 전달될 때 이 루트 필드는 동일한 객체를 다시 가져와야 한다.

## 연결을 통해 호출하는 방법

페이지화는 항상 API의 문제였고 올바르게 구현하는 표준 방법은 없습니다. 릴레이는 다음 그래프QL 커서 연결 규격을 사용하여 이 문제를 매우 잘 처리합니다.

```cpp
{
  podcasts {
    name
    episodes(first: 10) {
      totalCount
      edges {
        node {
          name
        }
        cursor
      }
      pageInfo {
        endCursor
        hasNextPage
      }
    }
  }
}
```

규격은 연결이라는 패턴을 제안하며 쿼리에서 연결은 결과를 슬라이싱하고 페이징하는 표준 방법을 제공합니다. 또한 연결은 커서를 제공하는 응답을 위한 표준 방식으로, 더 많은 결과를 사용할 수 있을 때 클라이언트에 알리는 방법을 제공합니다.

## 예측 가능한 돌연변이

돌연변이는 표준 방식으로 예측 가능하고 구조화하기 위해 `입력` 유형을 사용해야 한다.

```bash
input AddPodcastInput {
  id: ID!
  name: String!
}

mutation AddPodcastMutation($input: AddPodcastInput!) {
  addPodcast(input: $input) {
    podcast {
      id
      name
    }
  }
}
```

GraphQL 서버를 Relay와 호환되게 하면 수백만 명으로 쉽게 확장할 수 있는 잘 구조화되고 성능적이며 확장 가능한 GraphQL API를 확보할 수 있다.

## 시작 중

이제 Relay가 제공하는 그래프QL 서버가 제공하는 세 가지 원칙이 무엇인지 알았으니, GraphQL 서버 사양을 따르는 예를 만들어 실제로 어떻게 작동하는지 살펴보겠습니다.

먼저 새 프로젝트를 생성하고 몇 가지 종속성을 설치하는 것입니다.

```undefined
yarn add @koa/cors @koa/router graphql graphql-relay koa koa-bodyparser koa-graphql koa-helmet koa-logger nodemon
```

이제 몇 가지 개발 종속성을 추가하겠습니다.

```undefined
yarn add --dev @types/graphql-relay @types/koa-bodyparser @types/koa-helmet @types/koa-logger @types/koa__cors @types/node ts-node typescript
```

이러한 종속성을 모두 설치한 후 이제 GraphQL 서버를 생성할 준비가 되었습니다. 우리는 `src`라는 폴더를 만들 것이고 다음은 우리의 예제 앱의 파일들입니다.

```undefined
-- src
  -- graphql.ts
  -- index.ts
  -- NodeInterface.ts
  -- schema.ts
  -- types.ts
  -- utils.ts
-- nodemon.json
-- tsconfig.json
```

이 프로젝트에서 TypeScript를 사용하기 위해 `tsconfig.json`이라는 파일을 만들어 보겠습니다. 다음 코드를 이 파일에 넣겠습니다.

```undefined
{
  "compilerOptions": {
    "lib": ["es2016", "esnext.asynciterable"],
    "target": "esnext",
    "module": "commonjs",
    "moduleResolution": "node",
    "sourceMap": true,
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true,
    "esModuleInterop": true,
    "resolveJsonModule": true,
    "baseUrl": "."
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist", "**/*.spec.ts"]
}
```

이 예에서는 nodemon을 사용하여 `src` 디렉토리를 확인한 후 디렉토리 내에서 변경이 탐지될 때마다 서버를 재시작합니다. nodemon.json 파일에 다음 코드를 입력합니다.

```undefined
{
  "watch": ["src"],
  "ext": "ts",
  "ignore": ["src/**/*.spec.ts", "src/types/**/*.d.ts"],
  "exec": "ts-node ./src/index.ts"
}
```

index.ts 파일에 다음 코드를 넣겠습니다.

```js
import Koa from "koa";
import cors from "@koa/cors";
import Router from "@koa/router";
import bodyParser from "koa-bodyparser";
import logger from "koa-logger";
import helmet from "koa-helmet";
import graphqlHTTP from "koa-graphql";

import schema from "./schema";

const app = new Koa();
const router = new Router();

const graphqlServer = graphqlHTTP({ schema, graphiql: true });
router.all("/graphql", bodyParser(), graphqlServer);

app.listen(5000);
app.use(graphqlServer);
app.use(logger());
app.use(cors());
app.use(helmet());
app.use(router.routes()).use(router.allowedMethods());
```

지금은 Koa를 사용하여 간단한 GraphQL API를 만들었습니다. 우리는 `schema.ts` 파일에서 GraphQL 스키마를 가져왔으며, 이제 서버가 제대로 작동하기 위해 필요한 유형을 생성하려고 합니다.

type.ts 내부에는 다음 코드를 넣을 수 있습니다.

```cpp
export class IUser {
  id: string;
  firstName: string;
  lastName: string;
  constructor(data) {
    this.id = data.id;
    this.firstName = data.firstName;
    this.lastName = data.lastName;
  }
}
```

또한 utils.ts라는 파일을 만들고 users라는 빈 배열을 만들 것입니다.

```cpp
export const users = [];
```

방금 사용자를 위한 유형을 생성했습니다. 이제 `schema.ts` 파일 안에 GraphQL 스키마를 만들겠습니다. 다음 코드를 이 파일에 넣겠습니다.

```js
import { GraphQLSchema } from "graphql";
import { QueryType, MutationType } from "./graphql";

const schema = new GraphQLSchema({
  query: QueryType,
  mutation: MutationType,
});

export default schema;
```

graphql.ts 파일 안에 우리는 우리의 질의를 만들 것이다. 쿼리 유형이라는 변수를 만들고 `NodeInterface.ts` 파일에서 `NodeField`를 가져오도록 하겠습니다.

```coffeescript
import {
  GraphQLObjectType,
  GraphQLInt,
  GraphQLString,
  GraphQLNonNull,
} from "graphql";

import { NodeField, NodesField } from "./NodeInterface";

export const QueryType = new GraphQLObjectType({
  name: "Query",
  description: "The root of all... queries",
  fields: () => ({
    node: NodeField,
    nodes: NodesField,
  }),
});
```

이제 GraphQL 서버의 기본이 되었습니다. 노드 필드를 구현하고 작동 방식을 살펴보겠습니다. 이제 `NodeInterface.ts` 파일로 이동하여 코드를 입력하십시오.

graphql-ray, nodeDefinitions, globalId에서 함수를 가져올 예정입니다.

- `노드 정의` 기능은 전역적으로 정의된 ID를 실제 데이터 객체에 매핑하는 데 도움이 됩니다. 이 함수의 첫 번째 인수는 `fromGlobalId` 함수를 수신하고 두 번째 인수는 `fromGlobalId` 함수를 사용하여 개체 유형을 읽는 데 사용됩니다.
- fromGlobalId 함수는 전역 ID를 사용하여 객체를 검색합니다.

NodeInterface.ts 파일은 다음과 같습니다.

```js
import { nodeDefinitions, fromGlobalId } from "graphql-relay";
import { UserType } from "./graphql";
import { users } from "./utils";
import { IUser } from "./types";

const { nodeField, nodesField, nodeInterface } = nodeDefinitions(
  async (globalId: string) => {
    const { id: userGlobalID, type } = fromGlobalId(globalId);
    if (type === "User")
      return await users.find(
        ({ id }: IUser) => (id as string) === userGlobalID
      );
    return null;
  },
  (obj) => {
    if (obj instanceof IUser) return UserType;
    return null;
  }
);

export const NodeInterface = nodeInterface;
export const NodeField = nodeField;
export const NodesField = nodesField;
```

이제 GraphQL 서버에 대한 사용자 유형을 생성하겠습니다. graphql.ts 파일에 몇 가지를 가져와 사용자 유형이라는 변수를 만들어 보겠습니다. 이제 전체 graphql.ts는 다음과 같이 표시됩니다.

```coffeescript
import { GraphQLObjectType, GraphQLString, GraphQLNonNull } from "graphql";

import { NodeField, NodesField } from "./NodeInterface";

export const UserType: GraphQLObjectType = new GraphQLObjectType({
  name: "User",
  description: "User",
  fields: () => ({
    firstName: {
      type: GraphQLNonNull(GraphQLString),
      resolve: ({ firstName }) => firstName,
    },
    lastName: {
      type: GraphQLNonNull(GraphQLString),
      resolve: ({ lastName }) => lastName,
    },
  }),
});

export const QueryType = new GraphQLObjectType({
  name: "Query",
  description: "The root of all... queries",
  fields: () => ({
    node: NodeField,
    nodes: NodesField
  }),
});
```

현재로서는 릴레이가 사용할 것으로 예상하는 글로벌 ID 원칙을 사용하지 않고 있습니다. graphql-릴레이에서 globalIdField를 가져와 firstName 필드 앞에 다음과 같이 전달할 예정입니다.

```coffeescript
import { GraphQLObjectType, GraphQLString, GraphQLNonNull } from "graphql";
import { globalIdField } from "graphql-relay";

import { NodeField, NodesField } from "./NodeInterface";

export const UserType: GraphQLObjectType = new GraphQLObjectType({
  name: "User",
  description: "User",
  fields: () => ({
    id: globalIdField("User"),
    firstName: {
      type: GraphQLNonNull(GraphQLString),
      resolve: ({ firstName }) => firstName,
    },
    lastName: {
      type: GraphQLNonNull(GraphQLString),
      resolve: ({ lastName }) => lastName,
    },
  }),
});

export const QueryType = new GraphQLObjectType({
  name: "Query",
  description: "The root of all... queries",
  fields: () => ({
    node: NodeField,
    nodes: NodesField
  }),
});
```

이제 `인터페이스`라는 사용자형 내부에 새로운 자산을 만들어 노드인터페이스(NodeInterface)를 전달할 예정이다. 먼저 `NodeInterface.ts` 파일에서 `NodeInterface`를 가져오고, 이제 `UserType`에 새 속성을 추가하려고 합니다. 다음과 같습니다.

```coffeescript
import { GraphQLObjectType, GraphQLString, GraphQLNonNull } from "graphql";
import { globalIdField } from "graphql-relay";

import { NodeField, NodesField, NodeInterface } from "./NodeInterface";

export const UserType: GraphQLObjectType = new GraphQLObjectType({
  name: "User",
  description: "User",
  fields: () => ({
    id: globalIdField("User"),
    firstName: {
      type: GraphQLNonNull(GraphQLString),
      resolve: ({ firstName }) => firstName,
    },
    lastName: {
      type: GraphQLNonNull(GraphQLString),
      resolve: ({ lastName }) => lastName,
    },
  }),
  interfaces: () => [NodeInterface],
});

export const QueryType = new GraphQLObjectType({
  name: "Query",
  description: "The root of all... queries",
  fields: () => ({
    node: NodeField,
    nodes: NodesField
  }),
});
```

이제 `graphql-ray`에서 `connection Definitions`를 가져와 `UserConnection`이라는 새 변수를 만들어 보겠습니다. 이 `UserConnection` 변수를 사용하여 사용자의 연결 유형을 생성하겠습니다. 우리는 graphql.ts 안에 사용자 연결을 만들 것이다.

또한 graphql.ts 파일 안에 사용자들을 `users`라고 부를 수 있는 새로운 질의를 만들 것이다. 우리는 사용자 연결을 전달할 것이고 논쟁으로 그래프ql-릴레이에서 연결 Args를 중요시하여 전달할 것이다. 우리의 결과를 제대로 파악하기 위해, 우리는 `Connection FromArray`를 가져와 우리의 사용자 배열과 우리의 주장을 전달할 것이다.

이러한 모든 변경 후에 `graphql.ts`는 다음과 같은 형태로 나타납니다.

```coffeescript
import { GraphQLObjectType, GraphQLString, GraphQLNonNull } from "graphql";
import {
  globalIdField,
  connectionDefinitions,
  connectionFromArray,
  connectionArgs,
  mutationWithClientMutationId,
} from "graphql-relay";

import { NodeField, NodesField, NodeInterface } from "./NodeInterface";
import { IUser } from "./types";
import { users } from "./utils";

export const UserType: GraphQLObjectType = new GraphQLObjectType<IUser>({
  name: "User",
  description: "UserType",
  fields: () => ({
    id: globalIdField("User"),
    firstName: {
      type: GraphQLNonNull(GraphQLString),
      resolve: ({ firstName }) => firstName,
    },
    lastName: {
      type: GraphQLNonNull(GraphQLString),
      resolve: ({ lastName }) => lastName,
    },
  }),
  interfaces: () => [NodeInterface],
});

export const UserConnection = connectionDefinitions({
  name: "User",
  nodeType: UserType,
});

export const QueryType = new GraphQLObjectType({
  name: "Query",
  description: "QueryType",
  fields: () => ({
    node: NodeField,
    nodes: NodesField,
    users: {
      type: GraphQLNonNull(UserConnection.connectionType),
      args: {
        ...connectionArgs,
      },
      resolve: (_, args) => connectionFromArray(users, args),
    },
  }),
});
```

이제 서버에서 마지막으로 해야 할 일은 돌연변이를 만드는 것입니다. 사용자 생성이라는 돌연변이를 만들어 새로운 사용자를 만들어 어레이에 추가할 것입니다. 이제 `graphql.ts` 파일은 다음과 같습니다.

```coffeescript
import { GraphQLObjectType, GraphQLString, GraphQLNonNull } from "graphql";
import {
  globalIdField,
  connectionDefinitions,
  connectionFromArray,
  connectionArgs,
  mutationWithClientMutationId,
} from "graphql-relay";

import { NodeField, NodesField, NodeInterface } from "./NodeInterface";
import { IUser } from "./types";
import { users } from "./utils";

export const UserType: GraphQLObjectType = new GraphQLObjectType<IUser>({
  name: "User",
  description: "UserType",
  fields: () => ({
    id: globalIdField("User"),
    firstName: {
      type: GraphQLNonNull(GraphQLString),
      resolve: ({ firstName }) => firstName,
    },
    lastName: {
      type: GraphQLNonNull(GraphQLString),
      resolve: ({ lastName }) => lastName,
    },
  }),
  interfaces: () => [NodeInterface],
});

export const UserConnection = connectionDefinitions({
  name: "User",
  nodeType: UserType,
});

export const QueryType = new GraphQLObjectType({
  name: "Query",
  description: "QueryType",
  fields: () => ({
    node: NodeField,
    nodes: NodesField,
    users: {
      type: GraphQLNonNull(UserConnection.connectionType),
      args: {
        ...connectionArgs,
      },
      resolve: (_, args) => connectionFromArray(users, args),
    },
  }),
});

const UserCreate = mutationWithClientMutationId({
  name: "UserCreate",
  inputFields: {
    firstName: {
      type: new GraphQLNonNull(GraphQLString),
    },
    lastName: {
      type: new GraphQLNonNull(GraphQLString),
    },
  },
  mutateAndGetPayload: async ({ firstName, lastName }) => {
    const newUser = { firstName, lastName };
    users.push(newUser);
    return {
      message: "Success",
      error: null,
    };
  },
  outputFields: {
    message: {
      type: GraphQLString,
      resolve: ({ message }) => message,
    },
    error: {
      type: GraphQLString,
      resolve: ({ error }) => error,
    },
  },
});

export const MutationType = new GraphQLObjectType({
  name: "Mutation",
  description: "MutationType",
  fields: () => ({
    UserCreate,
  }),
});
```

GraphQL 서버 사양은 엄격한 규칙을 따르도록 하여 매우 모듈적이고 확장 가능하며 강력한 GraphQL API를 제공합니다. 프런트 엔드에서 릴레이를 사용하려면 백엔드에서 이러한 규칙을 구현해야 합니다.

사양을 준수하면 향후에 코드를 방탄하고 쉽게 확장할 수 있으므로 페이지 연결 및 인증과 같은 기능을 매우 쉽게 구현할 수 있습니다. 이 기사의 코드는 여기에서 찾을 수 있습니다.

## 결론

때로는 유연한 기술로 작업하는 것이 최선의 선택이 아닐 수도 있고, 불확실성으로 이어질 수도 있고 장기적으로 확장되지 않을 수도 있습니다. 릴레이는 매우 긍정적이고 구조적인 기술로, GraphQL 서버를 올바른 방식으로 사용하고 있으며 규칙을 따르면 문제가 발생하지 않습니다.