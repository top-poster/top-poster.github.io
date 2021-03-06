---
layout: post
title: "GraphQL 가입(Go)"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![The words “GraphQL subscriptions with Go” on an abstract background](https://miro.medium.com/max/3000/1*6mWLbH3jFKM9tEy9ZJ8cDA.png)

이 기사에서는 Go로 GraphQL 구독을 구현하는 방법에 대해 설명하겠습니다.

# 종속성

필요한 종속성은 다음과 같습니다.

## 서버측

## 프런트 엔드 측

# 그래프QL 구독

GraphQL 구독을 사용하면 클라이언트에서 폴링하지 않고도 주기적으로 데이터를 가져올 수 있습니다. 헤드라인 등록은 WebSockets를 통한 활성 연결을 유지하며 업데이트를 탐지한 후 즉시 사용자에게 알립니다.

# 레디스 스트림

버전 2에서 Redis는 게시자와 구독자를 분리할 수 있는 Pub/Sub 기능을 제공하였다. 이를 통해 게시자는 어떤 구독자가 있는지 모르는 상태에서 채널의 구독자 수에 상관없이 메시지를 보낼 수 있습니다.

버전 5에서 Redis는 Redis Stream이라는 새로운 기능을 제공합니다.

Redis Stream의 아이디어는 Redis Pub/Sub와 관련이 있지만 데이터 사용 방법에는 몇 가지 근본적인 차이가 있습니다. 그들 사이의 가장 큰 차이점은 메시지를 저장하는 방법이다. Pub/Sub에 있는 동안에는 메시지가 전송되고 저장되지 않습니다. Stream에서는 모든 메시지가 스트림에 무한정 저장되며, 이는 다른 소비자가 마지막 메시지의 ID를 기억함으로써 새로운 메시지가 무엇인지 알 수 있게 한다.

예를 들어, 채팅 프로그램에서는 한낮에 방에 들어가더라도 처음부터 모든 메시지를 보고 어떤 메시지가 새 메시지인지 알 수 있습니다.

이 문서에서는 Redis 스트림을 사용하여 여러 연결을 통해 데이터를 공유할 것입니다.

# 구현 개요

# Redis 서버 설정

먼저 도커를 사용하여 Redis 서버를 생성하겠습니다.

`docker-compose.yml`에 구성을 씁니다.

```undefined
버전: '3'
서비스:
redis:
이미지: redis:5.0.7
볼륨:
- redis-data:/data
포트:
- '6379:6379'
볼륨:
재데이터:
드라이버: 로컬
```

그리고 다음 명령을 실행하여 서버를 시작합니다.

```undefined
도킹을 하다.
```

다음과 같은 메시지가 나타나면:

```undefined
...
경고 커널에서 THP(Transparent Big Pages) 지원을 사용하도록 설정했습니다. 이렇게 하면 Redis에서 대기 시간 및 메모리 사용 문제가 발생합니다. 이 문제를 해결하려면 'echo never > /sys/kernel/mm/transparent_hugage/enabled' 명령을 루트로 실행하고 /etc/rc.local에 추가하여 재부팅 후에도 설정을 유지합니다. THP를 사용하지 않도록 설정한 후 Redis를 다시 시작해야 합니다.
...
```

VM 시스템에서 Docker for Mac용 VM을 구성해야 합니다.

다음 명령을 실행하여 Mac용 도커에 VM을 입력합니다.

```undefined
도커 실행 -it --figged --pid=host debiansenter -t 1 -m -u -n -ish
```

터미널에 설치하세요.

```undefined
eco never > /sys/sys/contract/mm/content_contentpage/enable
eco never > /sys/twin/mm/twincent_twinterpage/twin
```

터미널을 종료하고 다시 시작합니다.

```undefined
도킹을 하다.
```

# 에코 서버 설정

다음으로 에코 패키지를 사용하여 서버를 설정할 것입니다.

main.go를 생성하고 다음과 같이 코드를 작성합니다.

시작 페이지는 http://localhost:8080:에서 확인할 수 있습니다.

![welcome page](https://miro.medium.com/max/5660/1*OHUy88oaD8mG9nXOXtpF3g.jpeg)

# 그래프 설정QL

다음으로 gqlgen을 사용하여 GraphQL을 설정하겠습니다.

패키지를 초기화하려면 다음 명령을 실행합니다.

```undefined
github.com/99designs/gqlgen에 접속하다
```

그러면 프로젝트에서 다음과 같은 레이아웃이 생성됩니다.

```undefined
➡-- go.mod
➡--가세요.sum
α-- gqlgen.yml
α--그래프
➡➡--생성됨
➡➡--생성.go
α--모형
models_gen.go
➡➡--해결사.go
➡-- 스키마.graphqls
➡--schema.tvers.go
➡--서버.go
```

server.go는 이 레이아웃의 진입점으로 간주되지만 이를 제거하고 대신 gqlgen 처리기를 main.go에 넣습니다.

http://localhost:8080/playground로 이동하면 다음과 같은 놀이터 페이지가 나타납니다.

![playground page](https://miro.medium.com/max/5668/1*keVkS2xTeTh4IFU26bYmOg.jpeg)

main.go 내부의 코드를 단순화하기 위해 라우팅과 관련된 코드를 infrastructure/router/router.go로 추출하여 다음과 같이 가져올 것입니다.

# 그래프에서 웹 소켓 CORS 사용QL

gqlgen에는 WebSocket용 전송 기능이 제공되므로 구성할 필요가 없습니다.

하지만 CORS 구현이 포함되어 있지 않기 때문에 수동으로 추가해야 합니다.

그러기 위해 다음과 같이 `인프라/graphql/server.go`를 추가하겠습니다.

이 게시물에서는 클라이언트로부터 들어오는 모든 요청을 수락합니다.

main.go로 가져오면 다음과 같습니다.

이제 서버에 웹 소켓을 연결할 준비가 되었습니다.

# Redis 클라이언트 구성

다음으로는 응용 프로그램에서 Redis 클라이언트를 구성합니다.

먼저 `인프라/데이터스토어/redis.go`에 다음과 같은 구성 파일을 생성합니다.

main.go로 가져오기:

# Redis 스트림 구현

다음으로, 애플리케이션에 Redis Stream을 구현하겠습니다.

graph/resolver.go에 다음과 같은 코드를 추가해 보겠습니다.

서브스크립션 레디스(Subscribe Redis)는 레디스 스트림(Redis Stream)을 청취하고 채널을 통해 데이터를 제공할 예정이다.

이 게시물에서는 데이터를 저장할 수 있는 `룸`이라는 스트림을 만들 것입니다. 주기적으로 스트림에 데이터를 가져오려면 "BLOCK" 옵션이 있는 "XREAD"를 통해 스트림을 들을 필요가 있습니다. 차단 옵션을 사용하면 스트림이 지정된 시간 동안 새 수신 메시지가 추가될 때까지 기다립니다. 이 경우 0을 지정하는데, 이는 메시지가 들어올 때까지 기다린다는 의미입니다.

데이터를 얻으려면 ID를 지정해야 합니다. 이 경우 ID에 대해 $를 지정하게 되는데 XREAD가 발급된 후에 메시지만 받게 된다는 의미입니다.

```undefined
스트림, 오류:=r.RedisClient.XRAD(
```

main.go로 이동하여 구독을 실행하십시오.

그리고 다음과 같은 `인프라/라우터/라우터.go`에서 서브스크립션 경로를 생성합니다.

# 그래프QL 분해능 구현

서버의 마지막 단계를 위해 스키마에 구현을 추가할 것입니다.

먼저 `graph/schema.graphqls`로 이동하여 다음과 같은 스키마를 추가합니다.

그리고 명령을 실행하여 해결사를 생성합니다.

```undefined
github.com/99designs/gqlgen에 접속하다
```

이로 인해 위에서 정의한 스키마를 기반으로 함수와 함께 `graph/schema.resolvers.go`가 생성되었습니다.

graph/schema.resolvers.go에 다음과 같은 구현을 추가하겠습니다.

이제 클라이언트와 정기적으로 메시지를 제공하기 위해 서버를 준비했습니다.

# 클라이언트 응용 프로그램 설정

신속하게 시작하기 위해 Create React App을 사용하여 React 애플리케이션을 생성하겠습니다.

루트에서 이 명령을 실행합니다.

```undefined
npx 생성-수정-app 프런트엔드 --수정 유형 스크립트
```

설치 후 다음 명령을 실행하여 dev 서버를 실행합니다.

```undefined
실타래를 치다
```

시작 페이지가 나타납니다.

![welcome page](https://miro.medium.com/max/5200/0*lbeiiY5MOKNONIqE.png)

# 아폴로 클라이언트 설정

우리는 그래프QL의 클라이언트 모듈로 아폴로 클라이언트를 사용할 것입니다.

필요한 패키지를 설치하겠습니다.

```undefined
cd 프런트 엔드
yarn add @contract/client graphql 구독-contract-ws
```

그리고 `frontend/src/lib/apoloClient.ts`에 아폴로 클라이언트를 생성합니다.

그리고 클라이언트와 함께 App 구성요소를 ApolloProvider로 포장합니다.

프런트 엔드 서버를 시작하면 HTTP 연결이 WebSockets를 성공적으로 업그레이드하는 것을 볼 수 있습니다.

# 구성 요소 구현

그런 다음 메시지를 표시할 구성 요소를 생성하겠습니다.

먼저 Chakra UI를 설치하여 다음 양식을 작성합니다.

```undefined
실 추가 @chakra-ui/thew @thew/thew @thewas @thewas @thewased @t
```

`src/Component.ts`를 생성하고 다음과 같은 코드를 입력합니다.

첫 번째 렌더에서는 쿼리로부터 모든 메시지를 가져오고 그 후에 구독을 시작하여 새 메시지를 가져옵니다.

그리고 `src/App.ts`에 포함:

이제 응용 프로그램을 테스트할 준비가 되었습니다.

브라우저에 두 개의 탭을 준비하고 메시지를 제출하겠습니다.

보시는 바와 같이, 두 개의 탭이 동시에 메시지를 표시합니다. 그리고 다시 로드한 후에는 Redis 스트림에 저장된 모든 메시지를 표시합니다.

이제 GraphQL 및 Redis Stream과 실시간 커뮤니케이션을 달성했습니다.

# 결론

Gqlgen 및 Redis Stream을 사용하여 GraphQL 서브스크립션을 구현하는 방법에 대해 설명했습니다. 프런트 엔드에서는 아폴로 클라이언트를 사용하여 실시간 채팅 서비스를 구현했습니다. 이 튜토리얼을 따라 차근차근 실행해 보면 그다지 복잡하지 않다는 것을 알 수 있습니다.

여기 최종 코드 베이스가 있습니다.

유용하게 쓰시길 바랍니다.