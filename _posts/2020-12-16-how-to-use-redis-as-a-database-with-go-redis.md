---
layout: post
title: "Redis를 go-redis가 있는 데이터베이스로 사용하는 방법"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/howtouseredisasadatabase.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/howtouseredisasadatabase.png?fit=730%2C487&ssl=1)

Redis는 데이터베이스, 캐시 또는 메시지 브로커로 사용되는 메모리 내 데이터 저장소입니다. Go-redis/redis는 Pub/Sub, Sentinel 및 파이프라인과 같은 기능을 지원하는 Go용 타입 세이프, Redis 클라이언트 라이브러리이다.

> 참고: 클라이언트 라이브러리를 "고레디스"라고 지칭하여 Redis 자체와 차별화할 수 있도록 지원합니다.

이 기사에서는 Go-redis를 살펴보고 파이프라인 기능을 사용하여 리더보드 API를 구축한다. API는 후드 아래에서 Gin과 Redis의 정렬된 세트를 사용합니다. 다음과 같은 엔드포인트가 노출됩니다.

- `GET/points/:username` - 전체 리더 보드에서 사용자 점수 및 순위를 가져옵니다.
- `POST/points` - 사용자 및 사용자 점수를 추가하거나 업데이트합니다. 이 끝점은 사용자의 새 순위도 반환합니다.
- `GET/leaderboard` — 사용자가 순위를 오름차순으로 정렬된 현재 리더보드를 반환합니다.

## 전제조건

이 게시물을 따라가려면 다음이 필요합니다.

- 모듈을 지원하는 Go 설치
- 로컬 컴퓨터에 빨간색이 설치되어 있습니다(도커가 설치된 경우 도커 이미지를 사용할 수도 있습니다).
- Go 쓰기 체험

## 시작 중

시작하려면 원하는 위치에 프로젝트 폴더를 생성하고 Go 모듈을 초기화하십시오.

```shell
$ mkdir rediboard && cd rediboard
$ go mod init gitlab.com/idoko/rediboard
```

아래 명령을 사용하여 응용 프로그램 종속성(gin-gonic/gin 및 go-redis/redis)을 설치합니다.

```shell
$ go get github.com/gin-gonic/gin github.com/go-redis/redis
```

그런 다음 main.go 파일을 만들어 프로젝트의 진입점 역할을 합니다. 또한 프로젝트 루트 디렉터리에 db 폴더를 생성하여 Redis와의 상호 작용을 담당하는 코드를 보유합니다.

```shell
$ touch main.go
$ mkdir db
```

## 고레디스 사용법 숙지

애플리케이션 비계를 설치한 상태에서 몇 가지 기본 사항을 살펴보도록 하겠습니다. Redis 데이터베이스에 대한 연결은 "클라이언트"에 의해 처리되며, 스레드 안전 값은 여러 개의 루틴에서 공유할 수 있으며 일반적으로 애플리케이션의 수명 동안 유지됩니다. 아래 코드는 새 클라이언트를 생성합니다.

```cpp
client := redis.NewClient(&redis.Options{
   Addr:     "localhost:6379", // host:port of the redis server
   Password: "", // no password set
   DB:       0,  // use default DB
})
```

Go-redis는 `redis`를 통해 많은 구성 옵션을 제공합니다.옵션 매개 변수입니다. 옵션 중에는 최대 연결 수를 설정하는 `풀 크기`와 TLS 보호 Redis 서버에 연결하는 `TLS 구성`이 있습니다.

그런 다음 클라이언트는 명령을 수신기 메서드로 표시합니다. 예를 들어 이 코드는 Redis 데이터베이스에서 값을 설정하고 가져오는 방법을 보여줍니다.

```undefined
ctx := context.TODO()
client.Set(ctx, "language", "Go", 0)
language := client.Get(ctx, "language")
year := client.Get(ctx, "year")

fmt.Println(language.Val()) // "Go"
fmt.Println(year.Val()) // ""
```

라이브러리에는 실행 중인 명령의 컨텍스트 기반 취소와 같은 것을 허용하기 위한 컨텍스트 매개 변수가 필요합니다. 여기서 제공하는 이점이 필요 없기 때문에 컨텍스트를 사용하여 빈 컨텍스트를 만듭니다.TODO() 다음으로 언어를 "Go"로 설정하고 만료 날짜를 지정하지 않습니다(값 0으로 전달). 언어와 연도의 값을 얻으려고 하지만 년도에 대한 값을 정하지 않았기 때문에 nil과 year이다.Val(`)은 빈 문자열을 반환합니다.

## Go로 Redis에 연결

응용 프로그램용 Redis 클라이언트를 생성하려면 앞에서 생성한 `db` 폴더에 새 `db.go` 파일을 생성하고 아래 코드 조각을 추가하십시오.

```undefined
package db

import (
   "context"
   "errors"
   "github.com/go-redis/redis/v8"
)

type Database struct {
   Client *redis.Client
}

var (
   ErrNil = errors.New("no matching record found in redis database")
   Ctx    = context.TODO()
)

func NewDatabase(address string) (*Database, error) {
   client := redis.NewClient(&redis.Options{
      Addr: address,
      Password: "",
      DB: 0,
   })
   if err := client.Ping(Ctx).Err(); err != nil {
      return nil, err
   }
   return &Database{
      Client: client,
   }, nil
}
```

위의 코드는 `데이터베이스` 구조를 만들어 redis 클라이언트를 감싸고 나머지 앱(라우터 등)에 노출시킵니다. 또한 두 가지 패키지 수준 변수인 "ErrNil"은 "Redis 작업 시 nil"이 반환되었음을 알리는 데 사용되는 호출 코드와 클라이언트와 함께 사용할 빈 컨텍스트인 Ctx"를 설정했다. 또한 클라이언트를 설정하고 PING 명령을 사용하여 연결이 활성 상태인지 확인하는 `New Database` 기능도 만들었습니다.

main.go 파일을 열고 아래 코드와 같이 `New Database()` 함수를 호출합니다.

```cpp
package main

import (
   "github.com/gin-gonic/gin"
   "gitlab.com/idoko/rediboard/db"
   "log"
   "net/http"
)

var (
   ListenAddr = "localhost:8080"
   RedisAddr = "localhost:6379"
)
func main() {
   database, err := db.NewDatabase(RedisAddr)
   if err != nil {
      log.Fatalf("Failed to connect to redis: %s", err.Error())
   }

   router := initRouter(database)
   router.Run(ListenAddr)
}
```

위의 조각은 데이터베이스에 연결을 시도하고 프로세스에서 발생하는 오류를 인쇄합니다. initRouter 기능도 참조한다. 우리는 다음 섹션에서 그것을 설정할 것이다.

## Gin을 사용한 API 경로

다음으로, 애플리케이션 경로 생성 및 등록을 위한 initRouter 기능을 생성한다. main.go에서 기존 `main` 함수 아래에 아래의 코드를 추가합니다.

```css
func initRouter(database *db.Database) *gin.Engine {
   r := gin.Default()
   return r
}
```

현재 함수는 gin의 인스턴스를 반환합니다.엔진. 나중에 경로별 처리기를 추가할 것입니다.

## 거래 파이프라인(고레디스)

Redis Transaction 대기열 작업은 모든 작업이 실행되거나 실행되지 않는다는 보장을 제공합니다. 또 다른 흥미로운 Redis 기능은 네트워크 최적화인 파이프라인으로, Redis 클라이언트는 응답을 기다리지 않고 동시에 모든 요청을 읽을 수 있도록 합니다.

Go-red는 트랜잭션과 파이프라인을 TxPipeline 방법으로 모두 차단합니다. 다음은 redis-cli에서 실행되는 샘플 트랜잭션 명령 집합입니다.

```css
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379> SET language "golang"
QUEUED
127.0.0.1:6379> SET year 2009
QUEUED
127.0.0.1:6379> EXEC
1) OK
2) OK
127.0.0.1:6379>
```

위의 명령을 아래의 Go 코드로 변환할 수 있습니다.

```undefined
pipe := db.Client.TxPipeline()
pipe.Set(Ctx, "language", "golang")
pipe.Set(Ctx, "year", 2009)
results, err := pipe.Exec()
```

## 정렬된 집합에 사용자 저장

db 폴더에 `user.go` 파일을 생성하고 아래 코드를 추가합니다.

```undefined
package db

import (
   "fmt"
   "github.com/go-redis/redis/v8"
)

type User struct {
   Username string `json:"username" binding:"required"`
   Points   int `json:"points" binding:"required"`
   Rank     int    `json:"rank"`
}

func (db *Database) SaveUser(user *User) error {
   member := &redis.Z{
      Score: float64(user.Points),
      Member: user.Username,
   }
   pipe := db.Client.TxPipeline()
   pipe.ZAdd(Ctx, "leaderboard", member)
   rank := pipe.ZRank(Ctx, leaderboardKey, user.Username)
   _, err := pipe.Exec(Ctx)
   if err != nil {
      return err
   }
   fmt.Println(rank.Val(), err)
   user.Rank = int(rank.Val())
   return nil
}
```

위의 코드는 "사용자" 구조를 만들어 리더보드에서 사용자를 감싸는 래퍼 역할을 합니다. 구조는 JSON으로 변환될 때 뿐만 아니라 Gin의 바인딩을 사용하여 HTTP 요청에서 변환될 때 필드를 표시하는 방법을 포함한다. 그런 다음 파이프라인을 사용하여 정렬된 집합에 새 멤버를 추가하고 멤버의 새 순위를 가져옵니다. 사용자 매개변수는 포인터이기 때문에, 사용자 저장() 함수에서 변환하면 순위 값이 보드에서 업데이트됩니다.

그런 다음 POST 요청을 받으면 main.go를 변경하여 위에서 선언한 SaveUser 함수를 /points로 호출합니다. main.go를 열고 아래의 경로 처리기를 initRouter 함수에 추가합니다(`reverter` line 바로 앞).

```undefined
r.POST("/points", func (c *gin.Context) {
   var userJson db.User
   if err := c.ShouldBindJSON(&userJson); err != nil {
      c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
      return
   }
   err := database.SaveUser(&userJson)
   if err != nil {
      c.JSON(http.StatusInternalServerError, gin.H{"error": err.Error()})
      return
   }
   c.JSON(http.StatusOK, gin.H{"user": userJson})
})
```

## 사용자 점수 및 순위 가져오기

마찬가지로 아래 코드를 user.go에 추가하여 단일 사용자의 순위와 점수를 가져옵니다.

```undefined
func (db *Database) GetUser(username string) (*User, error) {
   pipe := db.Client.TxPipeline()
   score := pipe.ZScore(Ctx, leaderboardKey, username)
   rank := pipe.ZRank(Ctx, leaderboardKey, username)
   _, err := pipe.Exec(Ctx)
   if err != nil {
      return nil, err
   }
   if score == nil {
      return nil, ErrNil
   }
   return &User{
      Username: username,
      Points: int(score.Val()),
      Rank: int(rank.Val()),
   }, nil
}
```

여기서는 사용자 이름을 핵심으로 하여 사용자의 점수 및 순위를 얻기 위해 파이프라인도 활용하고 있습니다.

또한 일치하는 기록이 발견되지 않은 경우(`ErrNil`을 사용하여) 호출자에게 신호를 보내 이러한 사례를 별도로 처리하는 것이 호출자의 몫(예: 404 응답을 표시하도록 선택할 수 있음)으로 한다.

다음으로 `main.go`에 다음과 같이 해당 경로 처리기를 추가합니다.

```undefined
r.GET("/points/:username", func (c *gin.Context) {
   username := c.Param("username")
   user, err := database.GetUser(username)
   if err != nil {
      if err == db.ErrNil {
         c.JSON(http.StatusNotFound, gin.H{"error": "No record found for " + username})
         return
      }
      c.JSON(http.StatusInternalServerError, gin.H{"error": err.Error()})
      return
   }
   c.JSON(http.StatusOK, gin.H{"user": user})
})
```

위의 코드 조각은 사용자 이름 경로 매개 변수를 검색하여 앞에서 선언한 `GetUser` 함수로 전달합니다. 또한 반환된 오류의 유형이 ErrNil인 경우도 확인하고 404 응답도 보여준다.

## 'ZRangeWithScores'로 전체 리더보드 가져오기

전체 리더 보드를 가져오기 위해 Redis는 ZRange 명령을 제공하며, 이 명령은 정렬된 세트의 멤버를 점수의 오름차순으로 검색하는 데 사용됩니다. ZRange는 또한 각 멤버의 점수를 반환하라는 선택적 WITSCORES 주장을 받아들인다. 반면 Go-red는 명령을 두 개로 분할하여 ZRange 및 ZRangeWithScores를 별도로 제공합니다.

다음 내용으로 `leaderboard.go`라는 이름의 `db` 폴더에 새 파일을 만듭니다.

```undefined
package db

var leaderboardKey = "leaderboard"

type Leaderboard struct {
   Count int `json:"count"`
   Users []*User
}

func (db *Database) GetLeaderboard() (*Leaderboard, error) {
   scores := db.Client.ZRangeWithScores(Ctx, leaderboardKey, 0, -1)
   if scores == nil {
      return nil, ErrNil
   }
   count := len(scores.Val())
   users := make([]*User, count)
   for idx, member := range scores.Val() {
      users[idx] = &User{
         Username: member.Member.(string),
         Points: int(member.Score),
         Rank: idx,
      }
   }
   leaderboard := &Leaderboard{
      Count: count,
      Users: users,
   }
   return leaderboard, nil
}
```

`leaderboard Key`는 당사의 Redis 데이터베이스에서 세트를 식별하는 데 사용되는 키를 나타냅니다. 지금은 단일 명령만 실행 중이므로(ZRangeWithScores) 더 이상 명령을 트랜잭션 파이프라인과 함께 일괄 처리할 필요가 없으므로 결과를 `점수` 변수에 직접 저장합니다. `점수`에 저장된 값은 바둑 맵의 한 조각을 포함하며, 바둑 맵의 길이는 집합에 저장된 멤버의 수입니다.

애플리케이션을 실행하려면 Redis가 설치되어 실행 중인지 확인하십시오. 또는 아래의 명령으로 Redis Docker 이미지를 당겨서 실행할 수 있습니다.

```shell
$ docker run --name=rediboard -p 6379:6379 redis
```

이제 아래의 명령으로 `main.go` 파일을 빌드하고 실행(또는 직접 실행)하여 샘플 프로젝트를 테스트할 수 있습니다.

```shell
$ go build ./main.go
$ ./main
```

다음은 샘플 cURL 명령과 응답입니다.

> cURL, Postman, HTTPie 또는 좋아하는 API 클라이언트로 API를 사용해 보십시오.

#### cURL 명령:

```undefined
$ curl -H "Content-type: application/json" -d '{"username": "isa", "points": 25}' localhost:8080/points
```

#### 응답:

```undefined
{
  "user": {
    "username": "isa",
    "points": 25,
    "rank": 3
  }
}
```

#### cURL 명령:

```shell
$ curl -H "Content-type: application/json" localhost:8080/points/mchl
```

#### 응답:

```undefined
{
  "user": {
    "username": "jude",
    "points": 22,
    "rank": 0
  }
}
```

#### cURL 명령:

```shell
$ curl -H "Content-type: application/json" localhost:8080/leaderboard
```

#### 응답:

```undefined
{
  "leaderboard": {
    "count": 7,
    "Users": [
      {
        "username": "ene",
        "points": 22,
        "rank": 0
      },
      {
        "username": "ben",
        "points": 23,
        "rank": 2
      },
      {
        "username": "isa",
        "points": 25,
        "rank": 3
      },
      {
        "username": "jola",
        "points": 39,
        "rank": 5
      }
    ]
  }
}
```

다음은 터미널에서 실행 중인 앱과 cURL 응답의 스크린샷입니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/s_F95FF86A2CADD063FA35ED2113AB093500CE08A6E8ACA31D659D295FA0DB8265_1608027386872_file.jpeg?resize=730%2C585&ssl=1)

## 결론

더 자세히 살펴보려면 Redis 및 Go-redis의 문서를 참조하십시오. 지원되지 않는 명령의 경우 G-redis는 일반적인 `보내기()` 및 `실행() 메서드도 제공합니다.

이 기사에서는 Go-redis 라이브러리를 사용하여 Redis 데이터베이스와 상호 작용하는 방법에 대해 살펴봤습니다. 샘플 프로젝트의 코드는 GitLab에서 사용할 수 있습니다.