---
layout: post
title: "엔트리에 대한 소개"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/ent3.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/ent3.png?fit=730%2C487&ssl=1)

데이터베이스 시스템은 소프트웨어 개발에 필수적인 부분입니다. 소프트웨어 개발자는 선택한 프로그래밍 언어와 관계없이 데이터베이스 작업에 능숙해야 합니다. 대부분의 프로그래밍 언어에는 개발자가 데이터베이스 관리 시스템을 쉽게 사용할 수 있는 다양한 도구/패키지가 있습니다. 이러한 도구 중 일부는 프로그래밍 언어에 기본이 되며, 다른 도구는 언어 주변의 개발자 커뮤니티에 의해 구축/유지 관리되며 무료로 사용할 수 있다.

바둑 프로그래밍 언어에 대한 그래프 기반 ORM(Object-Relational Mapping)의 부족은 페이스북의 개발자들로 하여금 엔트리를 만들도록 이끌었다. Ent는 일반적으로 그래프 기반 구조에서 데이터를 모델링하는 데 사용되는 엔티티 프레임워크이다. 엔트 프레임워크는 구조 태그로 데이터를 모델링하는 다른 많은 ORM과 달리 데이터를 Go 코드로 모델링할 수 있는 능력을 자랑한다. 엔트 프레임워크의 그래프 기반 구조 때문에 데이터베이스에 저장된 데이터를 쉽게 쿼리할 수 있으며 그래프 트래버설의 형태를 취한다. ent는 코드 스키마를 자동으로 생성하고 스키마의 시각적 표현을 얻는 데 사용할 수 있는 명령줄 도구와 함께 제공됩니다.

이 게시물에서는 엔트 프레임워크의 모든 멋진 기능을 탐색하고 엔트의 다양한 기능을 활용하는 간단한 CRUD API를 구축한다.

## 전제조건

이 기사를 읽는 동안 계속 진행하려면 다음이 필요합니다.

- 이동(버전 1.14 이상)
- 선택한 텍스트 편집기
- 바둑의 기본 지식
- 도커가 설치됨

## 엔트 시작하기

엔트 프레임워크와 함께 작업하기 위한 첫 번째 단계는 그것을 우리의 프로젝트에 설치하는 것입니다. 설치하려면 go get github.com/facebook/ent/cmd/entc 명령을 실행합니다. 명령어는 ent 패키지에 대한 명령줄 도구를 설치합니다.

이 기사 전반에 걸쳐, 우리는 ent를 활용하는 간단한 CRUD(Create, Read, Update, Delete) API를 구축할 것이다. API에는 5개의 끝점이 포함되며, 이 API를 구축하는 목적은 ent를 사용하여 데이터베이스에서 일반적인 생성, 읽기, 업데이트 및 삭제 작업을 수행하는 방법을 보여주는 것입니다.

시작하려면 아래 트리 구조와 일치하는 파일 및 폴더를 만드십시오.

```undefined
├── handlers/
│ ├── handler.go
├── database/
│ ├── db.go
└── main.go
```

- main.go 파일에는 API용 서버 생성과 관련된 모든 논리가 포함됩니다. 우리는 API 끝점을 빠르게 연결하기 위해 Go를 위한 익스프레스 스타일 프레임워크인 fiber를 사용할 것이다. 이 기사는 섬유에 대한 훌륭한 출발이다.
- 데이터베이스 디렉토리의 `db.go` 파일에는 데이터베이스 연결 및 클라이언트 생성과 관련된 코드가 포함됩니다.
- handler.go 파일에는 API 처리기가 포함됩니다.

다음 섹션에서는 API 및 탐색기 구축을 시작할 것입니다.

## 엔트리에 깊이 뛰어들다.

프로젝트를 시작하려면 프로젝트의 루트 디렉터리에서 go mod init을 실행하십시오. Go 모듈을 사용하여 새 프로젝트를 초기화합니다. 다음으로, 우리는 프로젝트 `github.com/gofiber/fiber/v2`의 루트 디렉토리에서 다음 명령을 실행하여 API 구축에 사용할 프레임워크인 fiber를 설치해야 한다.

가상 노트 테이킹 응용 프로그램을 위한 API를 구축하려면 다음과 같은 끝점이 필요합니다.

- /api/v1/create note
- /api/v1/readnote/
- /api/v1/searchnote/:contract
- /api/v1/메모/:id
- /api/v1/tftenote/:id

main.go 파일에서 다음 줄의 코드를 추가합니다.

```java
package main

import (
   "fmt"

   "github.com/gofiber/fiber/v2"
)

func Routes(app *fiber.App){
   api := app.Group("/api/v1")

   api.Get("/", func(c *fiber.Ctx) error {
      return c.SendString("Hello, World!")
   })
}

func main() {
   app := fiber.New()

   Routes(app)

   err := app.Listen(":3000")
   if err != nil {
      fmt.Println("Unable to start server")
   }
}
```

위의 코드는 간단한 웹 서버를 만듭니다. 단 한 개의 끝점만 배선되어 있는 현재, 다음 섹션에서는 모든 API 끝점이 제대로 작동하는지 확인하기 위해 `handler.go` 파일에서 작업할 예정입니다. 지금은 위의 파일을 실행하고 브라우저에서 `localhost:3000/api/v1/`를 방문하면 됩니다. 모든 것이 잘 되었다면, 여러분은 "안녕하세요 세계"가 인쇄된 것을 볼 수 있을 것입니다.

## 스키마 생성

위에서 설치한 명령줄 도구를 entc하여 엔트로 스키마를 쉽게 만들 수 있습니다. API의 경우 notes라는 스키마를 만들어 프로젝트 디렉토리의 루트에 `entcinNotes`를 실행하는 스키마를 만들 것이다. 이 명령은 자동으로 Notes 스키마를 생성합니다. 스키마와 관련된 코드는 `ent/schema/notes.go`에서 찾을 수 있습니다. 이 때 스키마가 비어 있고 필드가 없습니다. API의 경우 스키마에는 다음 네 개의 필드가 있습니다.

- 제목
- 내용
- 비공개
- 생성됨_at

스키마에 필드를 정의하기 위해, 우리는 `필드` 함수 안에서 엔트리에 의해 제공되는 필드 하위 패키지를 사용한다. 필드 유형을 호출하여 다음과 같이 원하는 스키마 필드의 이름을 전달합니다.

```undefined
field.String("Title")
```

API의 경우, 스키마의 속성으로 제목, 컨텐츠 및 개인 필드를 지정합니다. ent는 현재 모든 Go 숫자 유형, 문자열, boole 및 `time을 지원합니다.시간! 스키마에 필드를 추가한 후 `notes.go` 파일은 다음과 같습니다.

```undefined
package schema

import (
   "time"

   "github.com/facebook/ent"
   "github.com/facebook/ent/schema/field"
)

// Notes holds the schema definition for the Notes entity.
type Notes struct {
   ent.Schema
}

// Fields of the Notes.
func (Notes) Fields() []ent.Field {
   return []ent.Field{
      field.String("Title").
         Unique(),
      field.String("Content"),
      field.Bool("Private").
         Default(false),
      field.Time("created_at").
         Default(time.Now),
   }
}

// Edges of the Notes.
func (Notes) Edges() []ent.Edge {
   return nil
}
```

필드 하위 패키지는 또한 위의 코드 조각에 표시된 필드 입력을 확인하기 위한 도우미 기능을 제공합니다. 모든 기본 제공 검증자의 포괄적인 목록은 여기에서 확인할 수 있습니다. 이제 필수 필드를 추가했으므로 데이터베이스 작업에 필요한 자산을 생성할 수 있습니다.

ent는 CRUD 구축업체 및 엔티티 개체를 포함하는 자산을 자동으로 생성합니다. 자산을 생성하려면 프로젝트 디렉토리 `go generate./ent`의 루트에서 다음 명령을 실행하면 프로젝트의 `/ent` 디렉토리에 여러 개의 파일이 추가됩니다. 추가된 파일에는 생성된 자산과 관련된 코드가 포함되어 있습니다. 다음 섹션에서는 이러한 생성된 자산 중 일부를 사용하여 CRUD 작업을 수행하고 노트 API를 계속 구축하는 방법에 대해 알아봅니다.

## 스키마 시각화

엔트 프레임워크의 명령줄 도구인 entc는 터미널에서 스키마의 시각적 표현을 얻을 수 있게 해준다. 스키마를 시각화하려면 프로젝트 디렉터리의 루트에서 다음 명령 `entc description./ent/schema`를 실행하면 아래 이미지와 유사한 노트 스키마가 시각적으로 표시됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/SchemaDescribe.png?resize=926%2C276&ssl=1)

## 데이터베이스에 연결

ent는 Postgre를 포함한 몇 개의 데이터베이스에 연결하기 위한 기능을 제공합니다.SQL.database.go 파일에서는 ent를 사용하여 데이터베이스에 연결하는 init 함수를 생성합니다.함수를 열고 `ent` 유형의 클라이언트를 반환합니다.고객입니다. 열기 함수는 데이터베이스 이름과 데이터베이스 연결 문자열을 사용합니다.

현재 구축 중인 API에 대해서는 Postgre를 사용할 예정입니다.SQL 데이터베이스. 시작하기 위해, Postgres의 도커 인스턴스를 회전시키고 세 가지 간단한 단계로 로컬 컴퓨터에서 연결할 것입니다.

계속하려면 로컬 컴퓨터에 도커가 설치되어 있어야 합니다.

- 터미널에서 다음 명령을 실행합니다.
도커 실행 -d -p 5432:5432 --name postgresDB -e POSTGES_PASSWORD=mySecretpassword postgres
위의 명령은 Postgres의 공식 도커 이미지를 다운로드하고 용기가 실행 중인지 확인합니다.
- 아래 명령을 실행하고 위의 명령이 실행된 직후에 "CREATE DABASE notesdb;"를 입력하여 컨테이너에 데이터베이스를 작성합니다.
도커 집행관-이것은 나의 포스트그레스 배시.
- 도커 집행관-이것은 나의 포스트그레스 배시.
- `\c`를 실행하여 데이터베이스 컨테이너에 연결하고 암호를 입력합니다.

데이터베이스 컨테이너를 연결했으므로, 다음으로 필요한 것은 Postgre용 드라이버를 가져오는 것입니다.프로젝트에 대한 부작용으로 SQL을 들 수 있습니다. 드라이버를 설치하려면 프로젝트 디렉토리의 루트에서 go get github.com/lib/pq을 실행하십시오. 모든 것을 설정한 상태에서 다음 줄의 코드를 `database.go` 파일에 추가합니다.

```cpp
var EntClient *ent.Client
func init() {
//Open a connection to the database
   Client, err := ent.Open("postgres","host=localhost port=5432 user=postgres dbname=notesdb password=mysecretpassword sslmode=disable")
   if err != nil {
      log.Fatal(err)
   }

   fmt.Println("Connected to database successfully")
   defer Client.Close()
// AutoMigration with ENT
   if err := Client.Schema.Create(context.Background()); err != nil {
      log.Fatalf("failed creating schema resources: %v", err)
   }
   EntClient = Client
}
```

## 데이터베이스에 저장

데이터베이스 생성/저장 작업을 쉽게 수행할 수 있는 엔트리 프레임워크입니다. 이 섹션에서는 데이터베이스에 새 노트를 저장하는 역할을 하는 노트 만들기 끝점을 추가합니다.

시작하기 위해 handler.go 파일에서 fibers 핸들러 인터페이스를 구현하는 createNotes라는 함수를 만듭니다. `createNotes` 함수 내에서는 fiber에서 제공하는 body parser 함수를 사용하여 요청 본문을 구문 분석한다.

ent에는 명령줄 도구인 `entc`에서 자동으로 생성된 도우미 메서드가 있습니다. 우리는 각각의 값을 형식 문자열로 전달하면서 `setTitle`과 `setContent` 방법을 호출한다. 마지막으로, 데이터가 저장되도록 하기 위해 컨텍스트 값으로 전달되는 `저장` 방법을 호출한다.

```undefined
func CreateNote(c *fiber.Ctx) error{
//Parse the request body
   note := new(struct{
      Title string
      Content string
      Private bool
   })

   if err := c.BodyParser(&note); err != nil {
      c.Status(400).JSON("Error  Parsing Input")
      return err
   }
//Save to the database
   createdNote, err := database.EntClient.Notes.
      Create().                      
      SetTitle(note.Title).
      SetContent(note.Content).
      SetPrivate(note.Private).
      Save(context.Background())  

   if err != nil {
      c.Status(500).JSON("Unable to save note")
      return err
   }
//Send the created note back with the appropriate code.
   c.Status(200).JSON(createdNote)

   return nil
}
```

이 시점에서, 우리는 모두 준비가 되었고 새로운 엔티티를 만들기 위한 논리를 추가했습니다. 위의 처리기를 등록하려면 `main.go` 파일에서 위에서 생성한 경로 함수에 다음 코드 줄을 추가하십시오.

```undefined
api.Post("/createnote", handlers.CreateNote)
```

애플리케이션을 시작하고 "localhost:3000/api/v1/create note"에 포스트 요청을 하면 노트의 제목과 내용을 전달하면 노트가 성공적으로 생성되었음을 나타내는 아래 이미지와 유사한 출력이 표시됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/CreateNote.png?resize=974%2C518&ssl=1)

## 데이터베이스에서 읽기

ent를 사용하면 데이터베이스 조회가 쉬워집니다. entc는 데이터베이스를 검색하는 데 유용한 자산이 포함된 각 스키마에 대한 패키지를 생성합니다. 자동으로 생성된 빌더와 상호 작용하기 위해 클라이언트에서 `쿼리` 기능을 호출한다. 이 함수는 스키마에 대한 쿼리 작성기를 반환하며, 작성자의 일부는 `어디`와 `선택`을 포함한다.

이 섹션에서는 다음 두 엔드포인트에 대한 로직을 코드화합니다.

- `/api/v1/readnotes/` – 이 끝점에서 데이터베이스의 모든 노트를 읽을 수 있습니다.
- `/search notes/:title` – 이 끝점을 통해 데이터베이스에서 제목별로 특정 노트를 검색할 수 있습니다.

먼저 `/api/v1/readnotes/` 끝점을 구축하겠습니다. `handler.go` 파일에서는 위의 `create note` 기능과 유사한 `ReadNotes`라는 처리기 함수를 생성하여 섬유 처리기 인터페이스를 구현한다. `ReadNotes` 함수에서 우리는 `EntClient` 변수에서 `Query`를 호출한다. 쿼리와 일치하는 모든 레코드를 원한다는 것을 지정하려면 쿼리 작성기에서 "모두"를 호출합니다. 이때 전체 `ReadNotes` 기능은 다음과 유사하게 표시되어야 한다.

```cpp
func ReadNote(c *fiber.Ctx) error{
   readNotes, err := database.EntClient.Notes.
      Query().
      All(context.Background())
   if err != nil {
      c.Status(500).JSON("No Notes Found")
      log.Fatal(err)
   }
   c.Status(200).JSON(readNotes)
   return nil
}
```

ReadNotes 핸들러 기능이 준비되었습니다. 이제 main.go의 Routes 기능에 다음 줄을 추가하여 서버에 등록할 수 있습니다.

```undefined
api.Get("/readnotes", handlers.ReadNote)
```

이제 응용 프로그램을 시작하고 /api/v1/readnotes/ 경로를 방문하여 테스트할 수 있습니다. 모든 작업이 잘 진행되었으면 아래 이미지와 같이 데이터베이스의 모든 노트가 포함된 배열이 표시됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/ReadNote.png?resize=963%2C517&ssl=1)

데이터베이스에 저장된 모든 노트를 읽을 수 있는 `읽기 노트` 끝점이 유선으로 연결되었으며, 다음에는 검색 쿼리와 제목이 일치하는 모든 노트를 데이터베이스로 검색하는 `검색 노트` 끝점을 연결하겠습니다. 이때까지 모든 핸들러를 대상으로 했던 것처럼 검색노트라는 기능을 만듭니다.

이 함수에서는 섬유 내장 `params` 방법을 사용하여 요청 매개 변수로 전달된 검색 쿼리를 검색한다. 다음으로, "ReadNotes" 함수에 대해 했던 것과 같이 클라이언트에서 "Query" Builder 방법을 호출한다. 검색 쿼리를 지정하려면 쿼리 작성기에 새 술어를 추가하는 `어디`라는 다른 메서드를 호출한다. entc에 의해 자동으로 생성된 제목 술어로 전달할 수 있는 `어디` 방법에 대한 주장으로,

```cpp
func SearchNotes(c *fiber.Ctx) error{
//extract search query
   query := c.Params("title")
   if query == "" {
      c.Status(400).JSON("No Search Query")
   }
 //Search the database
   createdNotes, err := database.EntClient.Notes.
      Query().
      Where(notes.Title(query)).
      All(context.Background())

   if err != nil {
      c.Status(500).JSON("No Notes Found")
      log.Fatal(err)
   }

   c.Status(200).JSON(createdNotes)
   return nil
}
```

마지막으로 main.go 파일에 다음 줄의 코드를 추가하여 SearchNotes 기능을 등록할 수 있습니다.

```undefined
api.Get("/searchnotes/:title", handlers.SearchNotes)
```

우리는 `search notes` endpoint를 완료했으며 애플리케이션을 시작하고 `localhost:3000/api/v1/search notes/Lorem`을 방문하여 테스트할 수 있습니다. 모든 것이 잘 되면, 데이터베이스에 있는 로렘이라는 이름의 노트가 반환되는 것을 볼 수 있습니다.

## 레코드 업데이트

API를 구축할 때는 비즈니스 논리에 맞는 데이터베이스의 레코드를 업데이트하는 기능을 제공하는 것이 중요합니다. ent를 사용하면 빌드 기능을 포함하는 생성된 모든 자산 덕분에 레코드를 쉽게 업데이트할 수 있습니다. 이 섹션에서는 노트 API의 업데이트 경로를 작성하고 엔트로 레코드를 업데이트하는 방법에 대해 알아봅니다.

시작하기 위해 handler.go 파일로 이동하여 업데이트 노트라는 함수를 만듭니다. 이 함수는 handler.go 파일의 다른 함수와 마찬가지로 fiber의 핸들러 인터페이스를 구현한다. `UpdateNotes` 기능에서는 요청 본문을 구문 분석하여 내용 필드만 업데이트할 수 있도록 합니다. 다음으로, 키로 `params` 함수를 호출하여 쿼리 매개 변수에서 업데이트할 레코드의 ID를 검색한다. 우리는 섬유를 형식 문자열로 하여 쿼리 매개 변수를 검색하므로, 검색된 매개 변수를 `스트롤프` 패키지에서 사용할 수 있는 `아토이` 함수를 사용하여 데이터베이스에 저장된 유형에 해당하는 Int로 변환해야 한다.

기록을 갱신하기 위해, 우리는 `UpdateOneId` 함수를 `UpdateOneId`라고 부르고 위의 사용자로부터 검색한 ID를 전달합니다. UpdateOneId 함수를 호출하면 지정된 ID에 대한 업데이트 작성기가 반환됩니다. 다음으로, 우리는 `set Content` 함수라고 부른다. setContent는 위에서 선언한 스키마와 필드를 기반으로 자동으로 생성되었습니다. setContent 함수는 스키마의 내용 필드에 대한 지정된 업데이트를 가져옵니다. 마지막으로, 다음과 같은 컨텍스트를 사용하여 `저장` 함수를 호출하여 업데이트된 레코드를 저장할 수 있습니다.

```undefined
func UpdateNote(c *fiber.Ctx) error{
//Parse the request Body
   note := new(struct{
      Content string
   })

   if err := c.BodyParser(&note); err != nil {
      c.Status(400).JSON("Error  Parsing Input")
      return err
   }
//Extract & Convert the request parameter
   idParam := c.Params("id")
   if idParam == "" {
      c.Status(400).JSON("No Search Query")
   }
   id, _ := strconv.Atoi(idParam)
//Update the note in the database
   UpdatedNotes, err := database.EntClient.Notes.
      UpdateOneID(id).
      SetContent(note.Content).
      Save(context.Background())

   if err != nil {
      c.Status(500).JSON("No Notes Found")
      log.Fatal(err)
   }

   c.Status(200).JSON(UpdatedNotes)
   return nil
}
```

노트 업데이트 기능이 이렇게 보이면, 우리는 가도 좋고, 다음 코드 라인을 경로 기능에 추가하여 핸들러를 등록할 수 있다.

```undefined
api.Put("/updatenote/:id", handlers.UpdateNote)
```

위의 경로에 대해 put 요청을 하고 유효한 레코드 ID를 제공하면 해당 레코드가 업데이트됩니다.

## 레코드 삭제

레코드를 삭제하는 것은 업데이트 작업과 유사하지만, 엔트로 레코드를 삭제하는 경우 다른 기능이 사용됩니다. 레코드를 삭제하려면 ID를 수신하는 `DeleteOneId` 기능이 지정된 사용자의 삭제 작성기를 반환합니다. 실행 기능도 호출합니다. `Exec`은 다음 컨텍스트를 사용하여 데이터베이스에서 삭제 작업을 실행합니다.

```undefined
func DeleteNotes(c *fiber.Ctx) error{
   idParam := c.Params("id")
   if idParam == "" {
      c.Status(400).JSON("No Search Query")
   }

   id, _ := strconv.Atoi(idParam)
//Delete the Record frm the databse
   err := database.EntClient.Notes.
      DeleteOneID(id).
      Exec(context.Background())

   if err != nil {
      c.Status(500).JSON("Unable to Perform Operation")
   }

   c.Status(200).JSON("Success")

   return nil
}
```

우리는 `handler.go` 파일의 `route` 함수에 다음 줄의 코드를 추가하여 위의 핸들러 함수를 등록할 수 있습니다.

```undefined
api.Delete("/deletenote/:id", handlers.DeleteNotes)
```

`삭제` 경로가 모두 설정되었습니다! 이제 데이터베이스의 모든 노트를 ID를 지정하여 삭제할 수 있습니다.

## 결론

지금까지, 우리는 포스트그레와 상호작용하기 위해 엔트 프레임워크를 사용하여 노트테이킹 애플리케이션을 위한 API를 구축했다.SQL 데이터베이스. ent와 entc가 생성한 자산 덕분에 우리는 SQL 조회를 작성하거나 CRUD 작업을 수행하기 위한 논리에 대해 크게 걱정할 필요가 없었습니다. 이 글은 엔트리를 가지고 당신을 일으켜 세우도록 하는 것을 목표로 한다. 참고 가이드로 공문서를 확인해 보시기를 강력히 권합니다. 이 프로젝트의 전체 소스 코드는 여기에서 찾을 수 있습니다.