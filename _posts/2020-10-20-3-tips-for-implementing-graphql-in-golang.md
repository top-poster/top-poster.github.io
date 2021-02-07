---
layout: post
title: "Golang에서 GraphQL 구현에 대한 3가지 팁"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/golang-logo.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/golang-logo.png?fit=730%2C487&ssl=1)

## 그래프QL이란?

백엔드 기술자들은 일반적으로 GraphQL이 REST APIs의 뒤를 이어 "블록 위의 빛나는 새로운 아이"로 등장했다고 말할 것이다. 처음에 Facebook에서 내부 사용을 위한 강력한 데이터 수집 API로 개발되었으며, 오늘날에는 매일 수십억 개의 API 호출을 지원합니다.

공식 문서는 GraphQL을 API의 데이터를 360도 볼 수 있는 API에 대한 쿼리 언어로 정의한다. 그러나 프리코드 캠프의 정의는 다음과 같다.

"GraphQL은 데이터를 요청하는 방법을 설명하는 구문으로, 일반적으로 서버에서 클라이언트로 데이터를 로드하는 데 사용됩니다. 이를 통해 고객은 요청에 필요한 데이터를 정확하게 지정할 수 있으므로 여러 소스에서 데이터를 쉽게 선택할 수 있습니다."

### 전제조건

이 튜토리얼에서는 Golang을 사용하여 GraphQL을 구현할 때 주의해야 할 몇 가지 주요 사항을 강조합니다. 따라서 다음 사항에 대한 실무 지식을 갖추는 것이 좋습니다.

- 프로그래밍 언어 이동
- 그래프QL

### 그래프를 다시 살펴보자.QL

그래프QL이 논의되는 모든 시나리오에서, 일부 키워드는 다른 키워드보다 더 자주 사용될 것이다. 따라서 반드시 다음과 같은 몇 가지 단어를 상기시켜야 합니다.

그래프QL 유형: 이러한 구성 요소는 GraphQL 스키마 정의의 가장 기본적인 구성 요소입니다. 이러한 필드는 서비스에서 가져올 수 있는 개체의 종류를 나타내는 JSON 필드입니다. GraphQL 유형을 자세히 이해하려면 이 튜토리얼을 참조하십시오.

예를 들어 다음과 같은 세 가지 필드가 있는 작성자의 샘플 유형 정의(이름, ID, book)가 있습니다.

```css
type author {
  name: String! //the `!` sign shows it’s required
  id: ID!
  book: String!
}
```

그래프QL 쿼리: 그래프QL 쿼리는 그래프QL 서버에서 데이터를 요청하거나 가져오는 방법입니다. 쿼리의 구조화 방법에 따라 데이터가 과다하게 수집되는지 또는 과소 추출되는지 여부가 결정됩니다. 이 튜토리얼에서는 쿼리에 대해 자세히 설명합니다.

다음은 위의 형식 정의에 따라 작성자의 이름에 대한 예제 쿼리입니다.

```undefined
query {                     
  authors {                 
    name      
  }
}
```

그래프QL 스키마: 스키마는 지도와 같다. 서버 내에서의 관계를 설명함으로써 데이터를 이해하는 데 도움이 됩니다. 이를 통해 고객은 데이터에 액세스할 때 요청을 구성하는 방법을 알 수 있습니다. SDL(Schema Definition Language)로 작성된다.

해결사: 서버에서 요청된 데이터를 검색합니다. 그것들은 현장 고유의 기능이다. 이것은 GraphQL이 쿼리 요청당 데이터를 반환하는 데 사용하는 유일한 메커니즘이다.

그래프QL 돌연변이: 돌연변이는 REST API의 PUT 및 POST와 유사한 GraphQL에서 객체를 만들고 수정하는 데 사용된다. 이들은 `변성`이라는 키워드로 시작해야 한다는 점을 제외하고는 쿼리와 유사한 구문을 따른다.

그래프QL 구독: 이 기능은 서버가 구성된 모든 특정 이벤트에 대해 클라이언트로 데이터를 전송할 수 있는 기능입니다. 이를 위해 서버는 이 서버에 가입한 클라이언트와 안정적인 연결을 유지합니다.

## 왜 Golang과 GraphQL인가?

Go 프로그래밍 언어(Go programming lang, Golang)는 구글이 처음 고안한 범용 프로그래밍 언어이다. 골랑은 다른 많은 아름다운 특징들 중에서 단순함, 동시성, 그리고 빠른 성능으로 최고의 자리를 차지하고 있습니다. 따라서 GraphQL을 구현할 때 가장 중요한 옵션 중 하나로 남아 있다.

많은 사람들이 이 듀오(GraphQL과 Golang)의 잠재력을 깨닫게 되자, 수년 동안 소프트웨어 엔지니어들은 GraphQL을 Golang과 원활하게 통합하기 위한 라이브러리를 구축하기 시작했다.

다음은 원활한 구현을 위한 몇 가지 라이브러리입니다.

- Graph-gopers/graphql-go : GraphQL Schema Language를 구문 분석하는 동시에 스키마 및 해결사의 문제를 찾기 위해 런타임까지 기다릴 필요 없이 컴파일 시간에 해결사 정보를 제공한다.
- 그래프QL-go/graphql : 다른 라이브러리와 마찬가지로 쿼리, 돌연변이 및 구독을 지원합니다. 또한 GraphQL-js 라이브러리의 공식 구현을 모델링한다.
- Gqlgen : 이것은 유형 안전의 우선 순위를 정하고 스키마 퍼스트 접근법에 기초한다.

우리는 이 튜토리얼에서 이러한 라이브러리의 구현에 초점을 맞추지 않을 것이다. 그러나 각 라이브러리에는 이러한 라이브러리가 어떻게 구현되는지 직접 보기 위해 탐색할 수 있는 `예` 폴더가 있습니다.

### 여기 전구의 순간들이 온다.

Golang과 GraphQL을 통합할 때 경험할 수 있는 몇 가지 시나리오와 솔루션이 무엇인지 살펴보겠습니다.

### 팁 #1: 스키마 동질성

graphql-go/graphql은 Golang에서 GraphQL의 완전한 기능을 구현하여 쿼리, 돌연변이 및 구독을 지원한다.

이 라이브러리는 프로덕션용 GraphQL 서버를 구축하는 데 도움이 되는 다른 라이브러리를 활용하지만 GraphQL.js의 공식 참조 구현입니다. 다시 말해 Golang 버전의 GraphQL.js 라이브러리이다.

GraphQL.js에 대한 거의 동일한 API의 이점을 통해 Node.js/JavaScript를 위해 작성된 다른 GraphQL 스키마를 쉽게 읽을 수 있으며 Golang을 위해 다시 쓸 수 있습니다. 한 번 봅시다.

Graphql-go를 사용한 Golang의 그래프QL 스키마 정의

우리는 `graphql-go` 패키지를 가져오고 `hello` 필드를 사용하여 `BaseQuery`라는 GraphQL 개체 유형을 정의하여 스키마를 생성한다.

```undefined
package main
 
import (
   "log"
   "github.com/graphql-go/graphql"
)
 
var queryType = graphql.NewObject(graphql.ObjectConfig{
   Name: "BaseQuery",
   Fields: graphql.Fields{
       "hello": &graphql.Field{
           Type: graphql.String,
           Resolve: func(p types.ResolveParams) interface{} {
               return "World!"
           },
       },
   },
})
 
var Schema, err = graphql.NewSchema(graphql.SchemaConfig{
   Query: queryType,
})
if err != nil {
   log.Fatalf("failed to create new schema, error: %v", err)
}
```

스키마 생성 후 GraphQL 서버를 생성하는 것은 여기의 공식 `graphql-go` 설명서에 강조 표시된 일반적인 단계를 따릅니다.

GraphQL.js를 사용한 JavaScript용 Graphql 스키마 정의

아래 코드 조각에서, 우리는 GraphQL 패키지를 요구하고 `Hello` 필드를 사용하여 `BaseQuery`라는 GraphQL 개체 유형을 정의한다. 위의 스키마에서 강조 표시된 것과 유사한 프로세스를 따릅니다.

```js
var graphql = require('graphql');
 
var queryType = new graphql.GraphQLObjectType({
   name: 'BaseQuery',
   fields: {
       hello: {
           type: graphql.GraphQLString,
           resolve: function () {
               return 'World!';
           }
       }
   }
});
 
var schema = new graphql.GraphQLSchema({
   query: queryType
});
```

위의 두 가지 표현을 자세히 살펴보면, Graphql-go 라이브러리를 사용하여 Golang에서 GraphQL을 구현하는 것은 익숙한 프로그래밍 언어에 관계없이 공원에서 산책하는 것이라는 결론을 내릴 수 있다.

이는 `graphql-go` 라이브러리가 실제 선택한 라이브러리인 GraphQL.js의 공식 참조 구현을 밀접하게 반영하기 때문이다. 그 결과 GraphQL.js 라이브러리에 거의 동일한 API를 제공한다.

### 팁 #2: n+1 문제 해결

API 끝점당 하나의 리졸버(위의 정의 참조)를 사용하는 REST API와 달리 GraphQL은 필드당 하나의 리졸버를 실행합니다. 이는 요청당 특히 중첩된 데이터의 경우 너무 많은 수의 추가 해결사가 있을 수 있음을 의미합니다. 이것은 n+1 문제입니다.

이 문제를 해결하기 위한 예제 쿼리는 다음과 같습니다.

```undefined
query {                     
 authors {            # fetches authors - query 1
   name      
   book {             # fetches books for each author (N queries for N authors)
     title
   }
 }
}                     # N+1 round trips
```

쿼리의 응답은 다음과 같습니다.

```undefined
{
 "writers": [{
   "name": "Tom Wolfe",
   "book": {
     "title": "The Bonfire of the Vanities"
   }
 }, {
   "name": "ALice Walker",
   "book": {
     "title": "The Color Purple"
   }    
 }]
}
```

위의 예에서는 이 문제가 얼마나 빨리 증폭되어 문제가 될 수 있는지 보여 주지 않을 수 있습니다. 그러나 100명의 작성자에 대한 데이터를 가져와야 한다고 가정하면, 해결사는 응답을 가져오기 위해 100+1 트립을 진행합니다. 한편, 이 작업은 두 번의 여행, 즉 요청과 응답만으로 수행될 수 있었습니다.

이 문제의 해결 방법은 일괄 처리입니다.

일괄 처리 기능은 데이터 로더의 주요 기능입니다. 데이터 로더는 원래 2010년에 Facebook이 개발하여 GraphQL.js가 응용 프로그램의 데이터 가져오기 계층의 일부로 사용되었다. 배치 및 캐시를 통해 원격 데이터 소스에 대해 단순하고 일관된 API를 제공합니다. 그러나 `graphql-go`를 위해 다음과 같은 몇 개의 다른 데이터 로더 라이브러리가 개발되었다.

- Graph-Gopers/Data Loader : Golang에서 공식 Facebook 데이터 로더(JavaScript로 작성)를 복제한 것입니다.
- gqlgen 작성자의 데이터 로드 : Gglgen 라이브러리와 함께 사용할 수 있는 타입 세이프 데이터 로더를 Golang에서 생성한다. 또한 공식 데이터 로드 라이브러리에서 영감을 받았습니다.

그리고 다른 많은 사람들.

(라이브러리에 관계 없이) 데이터 로더는 해결사가 요청된 정확한 필드를 즉시 반환하지 않고 데이터를 요청하고 데이터에 대한 약속을 반환하도록 허용함으로써 n+1 문제를 해결한다. 그러면 요청의 모든 응답이 배치되고 한 번에 응답하므로 요청과 응답이라는 두 가지 트립이 발생합니다.

그러나 `graphql-gopers/dataloader` 라이브러리를 좀 더 자세히 살펴봅시다.

다음은 캐시 없이 라이브러리를 사용할 수 있는 방법에 대한 샘플 구현입니다. 자세한 예제를 보려면 라이브러리의 예제 디렉토리를 참조하십시오.

```undefined
package no_cache_test
 
import (
 "context"
 "fmt"
 
 dataloader "github.com/graph-gophers/dataloader/v6"
)
 
func ExampleNoCache() {
 // go-cache will automaticlly cleanup expired items on given diration
 cache := &dataloader.NoCache{}
 loader := dataloader.NewBatchedLoader(batchFunc, dataloader.WithCache(cache))
 
 result, err := loader.Load(context.TODO(), dataloader.StringKey("some key"))()
 if err != nil {
   // handle error
 }
 
 fmt.Printf("identity: %s", result)
 // Output: identity: some key
}
 
func batchFunc(_ context.Context, keys dataloader.Keys) []*dataloader.Result {
 var results []*dataloader.Result
 // do some pretend work to resolve keys
 for _, key := range keys {
   results = append(results, &dataloader.Result{Data: key.String()})
 }
 return results
}
```

### 팁 #3: 클라이언트-서버 데이터 스키마 불일치

일반적으로 GraphQL 백엔드를 처음부터 코딩하려면 데이터베이스 및 API 끝점에 대해 서로 다르지만 유사한 스키마를 상당히 많이 복제해야 한다.

이는 서로 다른 스키마가 문서 저장 방식에 영향을 미쳤기 때문입니다.

예: `데이터베이스`의 작성자에 대한 쿼리는 `작성자`를 반환합니다.ID 속성이지만 서버의 작성자에 대한 동일한 쿼리가 작성자 속성을 반환합니다. 따라서 클라이언트와 서버 간에 코드를 공유하여 코드베이스를 단순화하기가 어렵습니다.

```js
const fetchBookTitleClient = author => {
   return author.book.title
}
 
const fetchBookTitleServer = author => {
   const authorDeets = Books.findOne(author.bookId)
   return authorDeets.title
}
```

이에 대한 해결 방법은 서버-서버 쿼리입니다.

돌이켜 보면, 이것은 다음과 같은 GraphQL 함수에 GraphQL 스키마를 추가함으로써 이루어진다.

```cpp
graphql(
 schema: GraphQLSchema,
 requestString: string,
 rootValue?: ?any,
 contextValue?: ?any,
 variableValues?: ?{[key: string]: any},
 operationName?: ?string
): Promise<GraphQLResult>
```

그러나 구현 시 코드 내에서 다음과 같은 방식으로 서버 간 쿼리를 실행합니다.

```undefined
const result = await graphql(executableSchema, query, {}, context, variables);
```

이를 통해 거래실적 없이 코드를 대폭 간소화할 수 있다.

## 결론

GraphQL은 본 튜토리얼의 과정을 통해 살펴본 것처럼 단순한 API 쿼리 언어 그 이상이다. 데이터 및 API 쿼리 측면에서 게임을 완전히 바꿀 수 있는 잠재력을 가지고 있습니다.

이러한 잠재력을 가진 GraphQL의 모든 이점은 Golang과 같은 프로그래밍 언어에 의해 이용되어야 한다.

바둑 프로그래밍 언어는 동시성, 단순성, 빠른 성능 때문에 쉽게 상위권을 차지한다.

Golang에서 GraphQL을 실행할 때 Gotchas에 대한 이 게시물을 재미있게 읽으셨기를 바랍니다. 질문이나 피드백을 얼마든지 남겨주세요.