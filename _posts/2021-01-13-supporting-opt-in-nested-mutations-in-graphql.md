---
layout: post
title: "그래프에서 옵트인 중첩 돌연변이 지원QL"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/opt-in-nested-mutations-graphql.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/opt-in-nested-mutations-graphql.png?fit=730%2C487&ssl=1)

내포 돌연변이는 그래프QL의 루트 유형이 아닌 다른 유형에 대해 돌연변이를 수행할 수 있는 기능을 제공한다. 예를 들어, 이 표준 돌연변이는 최상위 수준에서 실행되었다.

```undefined
mutation {
  updatePost(id: 5, title: "New title") {
    title
  }
}
```

또한 `Post` 유형에서 이 중첩 돌연변이를 통해 실행될 수 있습니다.

```undefined
mutation {
  post(id: 5) {
    update(title: "New title") {
      title
    }
  }
}
```

제가 기사에서 주장한 바와 같이, "JavaScript 대 GraphQL 서버 코딩"입니다. WordPress" 중첩 돌연변이는 Graphql-js(참조 GraphQL 서버 구현)가 지원하지 않기 때문에 GraphQL 규격의 일부가 되지 않는다.

내포된 돌연변이를 지원해야 하는 경우 이 규격은 필드를 병렬로 해결하는 기능을 제거해야 하는데, 이 기능은 제거할 수 없습니다. 트레이드오프는 그럴 가치가 없는데, 이는 내포된 돌연변이로 인해 스키마가 덜 비대해지고 이해하기 쉽기 때문에 불행한 일이다. 저는 이것이 내포된 돌연변이를 매력적인 특징으로 만드는, 가질만한 가치가 있는 이점이라고 생각합니다.

이제 자바스크립트가 약속을 통해 제공할 수 있듯이 모든 프로그래밍 언어가 자연스럽게 필드 병렬 해결을 지원하는 것은 아니다. 이러한 경우 중첩 돌연변이를 지원하는 데 그래프ql-js와 동일한 트레이드오프가 필요하지 않으며 구현이 가능할 수 있다.

중첩된 돌연변이를 지원하는 GraphQL 서버는 표준 동작을 대체하기 위해 이 기능을 사용하지 말고 다른 옵션으로 제공해야 합니다.

- 기본적으로 표준 동작 사용
- 명시적으로 활성화된 경우에만 중첩된 돌연변이 사용

따라서, 중첩 돌연변이는 개발자가 이것이 표준 GraphQL 동작이 아니며 이 GraphQL 서버에서 작동하는 쿼리가 다른 곳에서 작동하지 않을 가능성이 높다는 것을 인식하도록 하기 위해 옵트인 기능으로 만들어야 한다.

이 기능을 지원해도 아무런 문제가 없습니다. 승산이 있거나 변화가 없습니다.

WordPress용 GraphQL API는 PHP로 구현된다. 이벤트 기반 라이브러리를 사용하여 필드를 병렬로 해결할 수 없기 때문에, 아무런 단점 없이 중첩된 돌연변이를 지원할 수 있습니다. 이 문서에서는 이 서버가 단일 코드 소스를 사용하여 그래프QL 규격에서 예상되는 표준 동작과 중첩된 돌연변이를 모두 옵트인 기능으로 지원하는 방법에 대해 설명하겠습니다.

## QueryRoot 및 MutationRoot 또는 단지 "Root"

표준 동작의 경우 쿼리 루트(QueryRoot)와 뮤티션 루트(MutationRoot)의 두 가지 루트 유형을 통해 쿼리와 돌연변이가 별도로 처리된다. 이러한 분리를 통해 MutationRoot에서 돌연변이 필드를 연속적으로 실행할 수 있으며, 다른 모든 필드(QueryRoot 및 다른 모든 엔티티)는 병렬로 해결할 수 있다.

이 배열에서 `MutationRoot`는 전체 GraphQL 스키마에서 돌연변이 필드를 포함할 수 있는 유일한 유형이다. 예를 들어, 아래 스키마에서 돌연변이 필드(`createPost` 및 `updatePost`)는 `MutationRoot` 아래에만 나타날 수 있습니다.

```js
type Post {
  id: ID!
  title: String
  content: String
}

type QueryRoot {
  posts: [Post]
  post(id: ID!): Post
}

type MutationRoot {
  createPost(title: String, content: String): Post
  updatePost(id: ID!, title: String, content: String): Post
}

schema {
  query: QueryRoot
  mutation: MutationRoot
}
```

게시물을 업데이트하는 유일한 방법은 다음과 같습니다.

```undefined
mutation {
  updatePost(id: 5, title: "New title") {
    title
  }
}
```

그러나 이러한 상황은 내포된 돌연변이를 도입할 때 변이되는데, 그 때 모든 단일 유형이 돌연변이(루트형뿐만 아니라)를 실행할 수 있고 쿼리의 모든 수준(위쪽에만 있는 것이 아님)에서 변이될 수 있기 때문이다.

예를 들어, `포스트`에 돌연변이 필드 `업데이트`를 추가하는 경우:

```shell
type Post {
  # previous fields
  # ...
  update(title: String, content: String): Post
}
```

그런 다음 다음과 같이 게시물을 업데이트할 수 있습니다.

```cpp
mutation {
  post(id: 5) {
    title
    update(title: "New title") {
      newTitle: title
    }
  }
}
```

> 참고: 'root.updatePost' 필드가 'Post.update'로 어떻게 수정되었는지, 더 이상 'ID' 매개 변수를 받을 필요가 없으므로 스키마가 간단해집니다.

우리는 또한 아래의 쿼리에서처럼 다른 돌연변이의 결과에 대한 돌연변이를 실행할 수 있다. 여기서 돌연변이 `루트.createPost`에 의해 생성된 물체에 돌연변이 `Post.update`가 적용된다.

```cpp
mutation {
  createPost(title: "First title") {
    id
    update(title: "Second title", content: "Some content") {
      title
      content
    }
  }
}
```

이러한 맥락에서 Post 유형에는 쿼리 필드와 돌연변이 필드(예: 전자의 경우 제목, 후자의 경우 업데이트)가 모두 포함됩니다. 내가 아래에 설명하는 이유 때문에, 나는 그것에 대한 방법이 없다고 믿는다.

다른 모든 유형에도 쿼리루트와 뮤티션루트를 통해 수행된 것처럼 쿼리 및 돌연변이를 처리하기 위해 두 가지 유형을 유지하는 아이디어를 복제한다고 가정합시다. 목적은 다음과 같이 병렬 및 돌연변이 필드에서 쿼리 필드를 순차적으로 해결하기 위해 모든 쿼리와 돌연변이를 자체 별도의 분기를 통해 실행하는 것이다.

- 질의에 대한 질의 루트 = > 게시물 = > 댓글 = > 등
- 돌연변이에 대한 변성근 => 변성포스트 = 변성댓글 = 등

그런 다음 포스트 유형을 엔터티 포스트(쿼리만 처리하기 위해)와 뮤티션 포스트(변이를 처리하기 위해)로 나눕니다. 그러나 아래 쿼리의 post 필드에는 post가 아닌 mutationpost 유형이 반환되어 필드 update를 결과에 적용합니다.

```undefined
mutation {
  post(id: 5) {
    update(title: "New title") {
      newTitle: title
    }
  }
}
```

그렇다면 포스트(Root.post 등)를 반환하는 모든 필드를 복제해 돌연변이 지점(MutationRoot.post 등)의 뮤턴트포스트(MutationPost)를 반환해야 한다.

```css
type QueryRoot {
  posts: [Post]
  post(id: ID!): Post
}

type MutationRoot {
  posts: [MutationPost]
  post(id: ID!): MutationPost
}
```

결과적으로, 스키마는 스키마를 단순화하기 위해 내포된 돌연변이를 도입하는 목적을 물리치고 비대해지고 관리할 수 없게 될 것이다.

그러나 MutationPost 타입은 업데이트 필드를 해결할 수 있지만 필드 타이틀은 해결할 수 없기 때문에 이 솔루션은 더 이상 작동하지 않습니다.

```cpp
mutation {
  post(id: 5) {
    title
    update(title: "New title") {
      newTitle: title
    }
  }
}
```

그런 다음 `포스트` 유형에는 쿼리와 돌연변이 필드가 모두 포함되어야 합니다.

이제 쿼리 필드와 돌연변이 필드가 모두 동일한 유형에서 나란히 살기 때문에 뮤티션 루트 유형은 더 이상 이치에 맞지 않으며 쿼리 필드와 돌연변이 필드를 모두 처리하면서 쿼리 루트와 뮤티션 루트를 단일 유형인 루트로 병합할 수 있다.

```css
type Post {
  id: ID!
  title: String
  content: String
  update(title: String, content: String): Post
}

type Root {
  posts: [Post]
  post(id: ID!): Post
  createPost(title: String, content: String): Post
  updatePost(id: ID!, title: String, content: String): Post
}

schema {
  query: Root
  mutation: Root
}
```

이 상황에서, 스키마는 더 단순해졌다. 그리고 루트 유형에서 중복된 필드를 제거할 수 있기 때문에 더 이상 "Post.update"가 없기 때문에 더 이상 "Root.updatePost"가 필요하지 않고 스키마에서 꺼낼 수 있기 때문에 더 희박해질 수 있다.

## 작업 유형을 통한 돌연변이 검증

이제 쿼리 필드와 혼합되어 있기 때문에 어떤 수준에서든 쿼리에 돌연변이가 추가되어 언제든지 실행될 수 있다는 불만이 제기될 수 있습니다.

예를 들어 이 쿼리는 다음과 같습니다.

```undefined
query {
  post(id: 1459) {
    title
  }
}
```

다음 쿼리가 될 수 있습니다.

```undefined
query {
  post(id: 1459) {
    title
    addComment(comment: "Hi there") {
      id
      content
    }
  }
}
```

그리고 나서, 우리는 원래 쿼리를 실행하려고 했을 때 돌연변이를 실행하고 있을지도 모릅니다.

그러나 우리는 여전히 우리의 의도를 검증하기 위해 `query`와 `mutation`이라는 연산 유형을 사용할 수 있다. 그러면 위의 쿼리에 오류가 발생하여 쿼리가 작업 유형 `쿼리`를 사용하고 있기 때문에 돌연변이 `addComment`를 실행할 수 없음을 알 수 있습니다.

```undefined
{
  "errors": [
    {
      "message": "Use the operation type 'mutation' to execute mutations",
      "extensions": {
        "type": "Post",
        "id": 1459,
        "field": "addComment(comment:\"Hi there\")"
      }
    }
  ],
  "data": {
    "post": {
      "title": "A lovely tango, not with leo"
    }
  }
}
```

대신 이 쿼리는 제대로 작동합니다.

```undefined
mutation {
  post(id: 1459) {
    title
    addComment(comment: "Hi there") {
      id
      content
    }
  }
}
```

상위 수준(post 쿼리 필드만 해당)에서 더 이상 돌연변이가 발생하지 않더라도 작업 유형 `mutation`을 사용하고 있습니다. 괜찮습니다. MutationRoot(루트로 대체)를 제거했으므로 기본 동작에 대한 기대치가 더 이상 적용되지 않습니다.

## 두 솔루션 모두에 단일 코드 소스 사용

이미 언급한 바와 같이, 서버는 다음 두 가지 동작을 지원해야 합니다.

- 기본적으로 표준 동작
- 중첩된 돌연변이, 옵트인(Opt-In)

이에 따라 기본적으로 쿼리루트와 뮤턴루트를 노출하고 중첩된 돌연변이에 대해 단일 루트 유형을 노출하는 것으로 전환된다.

해결 방법을 제공할 때 개발자에게 솔루션별로 하나씩 두 개의 해결 방법을 제공하도록 요구하지 않습니다. 루트의 필드를 확인하는 데 사용된 동일한 해결사로 쿼리루트와 뮤턴루트의 필드를 해결할 수도 있습니다. 이를 처리하기 위해 서버는 코드 우선 접근법을 따르는데, 이는 두 솔루션 중 하나에 대한 스키마를 동적으로 조정할 수 있기 때문이다.

WordPress용 GraphQL API가 기반이 되는 PHP의 CMS-애그노스틱 서버인 PoP에 의한 GraphQL에서 우리는 param `?closed_param`을 통해 엔드포인트에서 이 동작을 사용자 지정할 수 있으며, `standard`, `cals` 또는 `lean_nested`(루트 업데이트와 같은 루트의 중복된 돌연변이 필드가 있는 경우)를 통해 이 동작을 사용자 지정할 수 있다.

예를 들어 표준 끝점은 다음과 같습니다.

내포된 돌연변이 끝점은 다음과 같습니다.

서버는 필드해결기(FieldResolver)를 사용하여 필드를 확인하고 실제 돌연변이를 실행하기 위해 뮤티션해결기(MutationResolver)를 사용한다. 동일한 `Mutation Resolver` 객체는 서로 다른 필드를 구현하는 서로 다른 `Field Resolvers`에서 참조할 수 있기 때문에 SOLID 접근법에 따라 코드가 한 번만 구현되고 여러 곳에서 사용된다.

필드 해결사가 해당 필드의 뮤티션 해결사 개체를 선언하면 필드 해결사 클래스(resolveFieldMutation ResolverClass)를 통해 필드가 돌연변이인지 여부를 알 수 있다.

예를 들어 `Root.replyComment` 필드는 `AddCommentToCustomPostMutationResolver` 개체를 제공합니다. 이 개체는 Comment.reply 필드에서도 사용됩니다.

또한 필드 해결사를 코딩할 때 루트 필드는 루트 유형에만 추가됩니다. 표준 GraphQL 동작의 경우 서버는 이 구성을 검색하여 돌연변이 여부에 따라 이 필드를 자동으로 `MutationRoot` 또는 `QueryRoot`에 추가할 수 있습니다.

결과적으로, 우리는 표준 동작과 내포된 돌연변이에 모두 전원을 공급하는 코드에 단일 소스를 사용하기 때문에 추가 노력 없이 중첩된 돌연변이를 가진 쿼리를 실행할 수 있다.

이 쿼리에서 우리는 `Root.post`을 통해 포스트 엔티티를 얻은 다음, 거기서 돌연변이 `Post.addComment`를 실행하고 생성된 코멘트 객체를 얻는다. 마지막으로, 우리는 이 후자 개체에 대해 돌연변이 `Comment.reply`를 실행한다.

```undefined
mutation {
  post(id: 1459) {
    title
    addComment(comment: "Nice tango!") {
      id
      content
      reply(comment: "Can you dance like that?") {
        id
        content
      }
    }
  }
}
```

이 응답 생성:

```undefined
{
  "data": {
    "post": {
      "title": "A lovely tango, not with leo",
      "addComment": {
        "id": 117,
        "content": "<p>Nice tango!</p>\n",
        "reply": {
          "id": 118,
          "content": "<p>Can you dance like that?</p>\n"
        }
      }
    }
  }
}
```

기본 동작을 사용하여 여러 개체에 대해 돌연변이를 실행하려면 필드를 복제하여 (변이를 적용할 여러 개체를 나타내기 위해) `ids` 매개 변수를 전달해야 한다.

예를 들어 `MutationRoot` 필드가 있는 경우.포스트처럼 포스트를 "좋아요"하려면 필드를 뮤티션 루트로 복제해야 한다.여러 개의 게시물을 동시에 "좋아요"하는 포스트를 좋아합니다.

```bash
type MutationRoot {
  likePost(id: ID!): Post
  likePosts(ids: [ID!]!): [Post]
}
```

그러나 중첩된 돌연변이를 사용하면 여러 필드를 동시에 변이시킬 수 있습니다. 예를 들어 이 쿼리는 여러 게시물에 동일한 설명을 추가합니다.

```cpp
mutation {
  posts(limit: 3) {
    title
    addComment(comment: "First comment on several posts") {
      id
      content
      reply(comment: "Response to my own parent comment") {
        id
        content
      }
    }
  }
}
```

이 응답을 생성합니다.

```undefined
{
  "data": {
    "posts": [
      {
        "title": "Scheduled by Leo",
        "addComment": {
          "id": 126,
          "content": "<p>First comment on several posts</p>\n",
          "reply": {
            "id": 129,
            "content": "<p>Response to my own parent comment</p>\n"
          }
        }
      },
      {
        "title": "COPE with WordPress: Post demo containing plenty of blocks",
        "addComment": {
          "id": 127,
          "content": "<p>First comment on several posts</p>\n",
          "reply": {
            "id": 130,
            "content": "<p>Response to my own parent comment</p>\n"
          }
        }
      },
      {
        "title": "A lovely tango, not with leo",
        "addComment": {
          "id": 128,
          "content": "<p>First comment on several posts</p>\n",
          "reply": {
            "id": 131,
            "content": "<p>Response to my own parent comment</p>\n"
          }
        }
      }
    ]
  }
}
```

자상하다!

## 스키마의 돌연변이 필드 시각화

유형이 이제 쿼리 및 돌연변이 필드를 포함하므로, 예를 들어 GraphiQL을 사용할 때 문서에서 어떤 필드를 명확하게 시각화할 수 있는지 확인할 수 있다.

아쉽게도 자기성찰을 할 때 __Field` 타입에 isMutation 플래그가 없기 때문에 제가 사용하는 솔루션은 조금 까다롭고 완전히 만족스럽지 못합니다. 여기서는 필드 설명에 "[Mutation]"이라는 레이블을 붙입니다.

WordPress용 GraphQL API는 이 데이터를 메타데이터인 것처럼 사용하기 위해 이 검사 쿼리에서처럼 사용자 지정 확장 데이터를 검색하기 위해 `__Field`( 스키마에 숨겨짐) 필드에 확장자 필드를 추가했습니다.

```undefined
query {
  __schema {
    queryType {
      fields {
        name
        # This field is not currently part of the spec
        extensions
      }
    }
  }
}
```

다음 결과를 생성합니다(`isMutation: true`가 있는 알림 항목).

```undefined
{
  "data": {
    "__schema": {
      "queryType": {
        "fields": [
          {
            "name": "addCommentToCustomPost",
            "extensions": {
              "isMutation": true
            }
          },
          {
            "name": "createPost",
            "extensions": {
              "isMutation": true
            }
          },
          {
            "name": "customPost",
            "extensions": []
          },
          {
            "name": "customPosts",
            "extensions": []
          },
          {
            "name": "loginUser",
            "extensions": {
              "isMutation": true
            }
          }
        ]
      }
    }
  }
}
```

## 결론

그래프QL 서버에서 중첩된 돌연변이를 지원하기 때문에 상위 수준에서 돌연변이를 실행하는 것으로 돌아가기가 어렵다.

이 스키마는 돌연변이 외에는 공통점이 없는 뮤티션 루트 패킹 필드(Mutation Root)를 사용하지 않고 여러 개체에 대해 동시에 돌연변이를 실행하는 필드를 복제하지 않고 점점 더 얇아졌다.

또한 GraphQL에서 쿼리하는 것처럼 데이터 모델에 정의된 관계를 탐색하는 것처럼 중첩 연산을 실행할 수도 있습니다.

내포된 돌연변이는 훌륭한 특징입니다. 불행히도 graphql-js는 이들을 지원할 수 없기 때문에, 그들은 결코 규격의 일부가 될 수 없을 것이다. 그러나 그렇다고 해서 GraphQL 서버가 이 기능을 옵트인(opt-in)으로 제공하는 것을 방해해서는 안 되며, 개발자는 이 기능을 애플리케이션에 사용할 수 있는지 여부를 결정할 수 있다.