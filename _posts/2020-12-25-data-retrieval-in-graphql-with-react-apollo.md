---
layout: post
title: "반응 아폴로를 사용한 그래프QL에서의 데이터 검색"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/graphql.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/graphql.png?fit=730%2C487&ssl=1)

이 기사에서는 그래프QL 쿼리 언어를 아폴로 클라이언트와 함께 사용하여 서버에서 데이터를 효율적이고 원활하게 가져오는 방법에 대해 살펴본다. 읽기를 마치면 그래프QL과 아폴로 클라이언트의 사용 및 설치 방법과 act-apollo 내에서 쿼리 및 뮤테이션 작업을 수행하는 두 가지 접근 방식을 배우게 됩니다.

## 그래프QL

GraphQL은 정의된 시스템을 사용하여 사용자가 API에서 데이터를 요청하는 방법을 설명하는 쿼리 언어이다. 그래프QL을 사용하려면 사용자가 전송해야 하는 정확한 데이터를 지정해야 합니다(대개 서버에서). 그런 다음 클라이언트 측에 로드됩니다.

아폴로를 사용하여 특정 쿼리 작업을 수행할 수 있는 방법에 대해 알아보기 전에 GraphQL 쿼리 라이브러리 내에서 수행할 수 있는 몇 가지 기본 작업과 작업을 검토한다.

### 1. 질의

GraphQL 쿼리 작업은 서버 측에서 기본 데이터를 가져오는 작업과 관련이 있습니다.

```undefined
  {
    query getItems {
      Items {
        id
        chair
        stool
       }
     }
  }
```

위는 id, chair, stool을 불러오는 getItems라는 이름의 GraphQL 쿼리이다.

### 2. 돌연변이

`mutation` 키워드를 추가하면 GraphQL 애플리케이션에서 기본 CRUD(생성, 읽기, 업데이트 및 삭제) 작업이 수행됩니다.

```bash
{
    mutation addUser(name: String!, email: String!){
      addUser(name: $name, email: $email){
        id
        name
        email
        created_at
      }
    }
  }
```

위의 addUser 돌연변이는 사용자의 이름과 이메일을 지정하면 사용자의 이름, ID, e-메일, create_at 필드를 반환합니다.

### 3. 가입

이 작업은 특정 이벤트가 실행될 때 서버에서 클라이언트 측으로 데이터를 전송하는 이벤트 기반 작업입니다. GraphQL에서 구독을 생성하려면 구독 유형을 스키마에 추가해야 합니다.

```js
const typeDefs = gql`
  type Subscription {
    bookAdded: Book
  }
```

다음 단계는 해결사를 만드는 것이다. Pubsub은 돌연변이 내에서 이 사건의 발생을 통보받는다.

```js
const BOOK_ADDED = 'BOOK_ADDED';

const resolvers = {
  Subscription: {
    bookAdded: {
      subscribe: () => pubsub.asyncIterator([BOOK_ADDED]),
    },
  },

  Mutation: {
    addBook(root, args, context) {
      pubsub.publish(BOOK_ADDED, { bookAdded: args });
      return postController.addBook(args);
    },
  },
};
```

이 구독은 돌연변이 내부의 공개 호출에 의해 촉발된다.

## 아폴로

Apollo는 GraphQL 애플리케이션에서 로컬 및 원격 데이터 스토리지를 위한 상태 관리 라이브러리 역할을 한다. Apollo의 도움으로 수행할 수 있는 기본적인 작업으로는 fetching, local state storage, caching 등이 있습니다. 또한 아폴로 클라이언트는 React와 통합되어 개발을 훨씬 쉽게 합니다.

아폴로 클라이언트의 몇 가지 기본 기능은 다음과 같습니다.

### 선언적 데이터 가져오기

아폴로 클라이언트는 사용자가 애플리케이션 프로그래밍 인터페이스에서 필요한 정확한 데이터를 지정할 수 있도록 한다. React는 그래프QL 애플리케이션에서 데이터 검색 및 데이터 가져오기 작업을 수행할 수 있는 `useQuery` 후크를 사용한다. 공식 설명서의 예는 다음과 같습니다.

```php
function Feed() {
    const { loading, error, data } = useQuery(GET_DOGS);
    if (error) return <Error />;
    if (loading || !data) return <Fetching />;

    return <DogList dogs={data.dogs} />;
  }
```

useQuery Hook의 도움으로, 우리는 GraphQL 서버에서 데이터를 가져올 수 있으며, 이는 변경 사항을 반영하기 위한 UI 업데이트로 이어진다.

### 캐싱

GraphQL로 수행할 수 있는 주요 가져오기 작업 외에도 지능형 데이터 캐싱 작업도 거의 또는 전혀 구성 없이 수행할 수 있습니다.

다음은 데이터 캐싱을 위한 아폴로 클라이언트 구성입니다.

```js
 import { ApolloClient, InMemoryCache } from '@apollo/client';

const client = new ApolloClient({
  cache: new InMemoryCache()
});
```

아폴로 고객과 함께 제공되는 다른 이점은 다음과 같습니다.

- 현대식 리액트 훅 지원
- 호환성.
- 강력한 커뮤니티 지원
- 페이지화

## GraphQL 및 Apollo Client의 설치 및 사용

대응 응용 프로그램을 설정하지 않은 경우 다음을 실행하는 것부터 시작하십시오.

```undefined
npx create-react-app
```

반응 애플리케이션에서 그래프QL 및 아폴로를 사용하려면 다음과 같은 종속성을 설치해야 합니다.

```coffeescript
npm install apollo-boost graphql react-apollo
```

이제 그래프QL을 사용한 반응 응용 프로그램이 설정되고 사용할 준비가 되었습니다!

연결을 만들려면 먼저 `index.js` 파일을 만들고 다음 조각을 추가하십시오.

```js
import ApolloClient from "apollo-boost";
import { ApolloProvider } from 'react-apollo';

const client = new ApolloClient({
    uri: //graghql server url should be here
  });
```

다음으로, 아폴로 프로바이더로 앱 구성 요소를 포장하여 클라이언트를 소품으로 전달합니다.

```js
ReactDOM.render(
    <ApolloProvider client={client}>
      <App />
    </ApolloProvider>,
    document.getElementById("root")
  );
```

처음에 우리는 GraphQL로 `쿼리`와 `음성`을 포함하여 수행할 수 있는 기본적인 작업에 대해 이야기했습니다. 이제 useQuery Hook 접근법과 렌더 소품 접근 방식을 사용하여 아폴로와 이러한 작업 중 일부를 수행하는 방법을 알아보자.

### 쿼리

그래프 QL 쿼리는 데이터 읽기 및 가져오기와 관련이 있습니다. 우리는 리액트 아폴로를 통해 이 작업을 수행할 때 두 가지 접근 방식을 취할 수 있다: `useQuery` Hook 접근법과 렌더 소품 접근법.

#### 쿼리 후크 사용 방법

먼저 GraphQL 쿼리를 만들고 아래와 같이 `getItems`로 이름을 지정합니다.

```js
import React from "react";
import { useQuery } from "react-apollo";
import { gql } from "apollo-boost";

const GET_ITEMS = gql`
  query GetItems {
    items {
      id
          oneitem
    }
  }
`;
```

그런 다음, 다음과 같이 기능 구성 요소를 생성하고 GraphQL 쿼리를 `useQuery` 후크에 전달합니다.

```js
function DisplayItems() {
  const { loading, error, data } = useQuery(GET_ITEMS);

  if (loading) return 'Loading...';
  if (error) return `Error! ${error.message}`;

  return (
    <select name="items">
      {data.items.map(item => (
        <option key={item.id} value={item.oneitem}>
          {item.oneitem}
        </option>
      ))}
    </select>
  );
}
```

새 데이터를 가져오거나 새 변경 사항을 반영하는 오류가 발생하면 응용 프로그램이 업데이트됩니다.

#### 렌더링 소품 접근 방식

렌더 소품 접근 방식의 경우, 이전에 설치한 `react-apollo`에서 쿼리를 추가로 가져오는 경우를 제외하고는 첫 번째 단계는 변경되지 않습니다.

```js
import React from "react";
import { Query } from "react-apollo";
import { gql } from "apollo-boost";

const GET_ITEMS = gql`
  query GetItems {
    items {
      id
          oneitem
    }
  }
`;
```

다음으로 생성된 GraphQL 쿼리를 전달하여 다음과 같이 사용하여 쿼리(query)를 사용할 것이다.

```js
function DisplayItems() {

  if (loading) return 'Loading...';
  if (error) return `Error! ${error.message}`;

  return (
    <Query query={GET_ITEMS}>
    <select name="items">
      {data.items.map(item => (
        <option key={item.id} value={item.oneitem}>
          {item.oneitem}
        </option>
      ))}
    </select>
    </Query>
  );
}
```

후크 접근 방식에서와 마찬가지로, 응용프로그램은 새 데이터를 반영하도록 업데이트하거나 오류를 반환합니다.

### 돌연변이

돌연변이는 생성, 업데이트 및 삭제 작업을 수행합니다. 쿼리의 경우처럼, 우리는 돌연변이를 처리할 때 후크 접근 방식을 사용하고 소품 접근 방식을 렌더링할 수 있다.

#### 훅스 어프로치

우선 react-apolo에서 수입한 useMutation을 비롯한 의존성을 수입해 보자.

```coffeescript
import React, { useState } from 'react';
import { useMutation } from 'react-apollo';
import { gql } from 'apollo-boost';

const ADD_TODO = gql`
  mutation AddTodo($type: String!) {
    addTodo(type: $type) {
      id
      type
    }
  }
`;
```

다음은 `AddToDo`라는 이름의 기능 구성요소를 만들고 `ADD_TODO` 돌연변이를 `useMutation` 후크에 전달하는 것이다.

```js
function AddTodo() {
  let input;
  const [addTodo, { data }] = useMutation(ADD_TODO);

  return (
    <div>
      <form
        onSubmit={e => {
          e.preventDefault();
          addTodo({ variables: { type: input.value } });
          input.value = '';
        }
      >
        <input
          ref={node => {
            input = node;
          }
        />
        <button type="submit">Add Todo</button>
      </form>
    </div>
  );
}
```

#### 렌더링 소품 접근 방식

후크 접근법과 마찬가지로 우리는 필요한 의존성을 `react-apolo`에서 새로운 `mutation`을 수입할 필요가 있다.

```coffeescript
import React, { useState } from 'react';
import { useMutation } from 'react-apollo';
import { gql } from 'apollo-boost';

const ADD_TODO = gql`
  mutation AddTodo($type: String!) {
    addTodo(type: $type) {
      id
      type
    }
  }
`;
```

생성된 돌연변이를 완료하려면 다음과 같이 전달합니다.

```js
function AddTodo() {
  let input;

  return (
    <Mutation mutation={ADD_TODO}>
    <div>
      <form
        onSubmit={e => {
          e.preventDefault();
          addTodo({ variables: { type: input.value } });
          input.value = '';
        }
      >
        <input
          ref={node => {
            input = node;
          }
        />
        <button type="submit">Add Todo</button>
      </form>
    </div>
    </Mutation>
  );
}
```

## 결론

그래프QL은 RESTful API와 함께 발생하는 과도한 페치 및 언더페치 문제를 해결하므로 데이터 페치를 처리할 때 좋은 선택이다. Apollo Client를 사용하면 GraphQL을 서버에 쉽고, 원활하고, 효율적으로 연결할 수 있습니다.