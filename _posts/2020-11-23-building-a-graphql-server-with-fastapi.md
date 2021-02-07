---
layout: post
title: "빠른 GraphQL 서버 구축API"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/building-graphql-server-fastapi.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/building-graphql-server-fastapi.png?fit=730%2C487&ssl=1)

FastAPI는 파이썬으로 웹 API를 구축하기 위한 고성능 프레임워크이다. 단순하고 직관적인 특성으로 매우 적은 보일러 플레이트 코드를 사용하여 강력한 웹 API를 신속하게 개발할 수 있습니다. 이 기사에서는 Fast를 소개합니다.API 및 이 API로 GraphQL 서버를 설정하는 방법.

공식 문서에서는 FastAPI로 웹 애플리케이션을 구축하면 개발자가 초래하는 오류의 약 40%가 감소하며, 이는 파이썬 3.6형 선언을 통해 가능하다. 인터랙티브 API 설명서 자동 생성을 포함한 모든 기능을 갖춘 파이썬으로 웹 앱을 구축하는 것이 어느 때보다 쉬워졌습니다.

## 앱 설정

시작하기 전에 터미널에서 다음 명령을 실행하여 Python 3.6+가 설치되어 있는지 확인해 보겠습니다.

```undefined
python --version
```

오류가 반환되면 여기를 클릭하여 로컬 컴퓨터에 Python을 다운로드하여 설치하십시오. Python 3.4+ 설치는 기본적으로 pip과 함께 제공되지만 FastAPI를 실행하려면 Python 3.6+가 필요합니다. Pip은 선호되는 Python 패키지 관리자로, FASAPI 앱의 지원 모듈을 설치하는 데 사용됩니다.

Python과 pip이 설치된 상태에서 Fast를 추가하겠습니다.터미널에서 다음 명령을 실행하여 시스템에 대한 API를 제공합니다.

```undefined
pip install fastapi
```

또한 앱을 서비스하려면 ASGI(동기식 서버 게이트웨이 인터페이스) 서버인 Uvicorn이 필요합니다. 터미널에서 다음 명령을 실행하여 설치하겠습니다.

```undefined
pip install uvicorn
```

이렇게 하면 앱에 대한 새 디렉터리를 만들 수 있습니다. fastapi-graphql이라고 이름 짓자. 새 디렉터리 안에 `main.py`이라는 이름의 새 파일을 만들 것입니다. 이 파일은 서버의 인덱스 파일이 됩니다.

## 그래프QL 기본 사항: 쿼리 및 스키마

### 그래프QL 쿼리

그래프QL에서 쿼리는 REST API 아키텍처의 GET 요청과 마찬가지로 데이터를 가져오는 데 사용된다. 그러나 GraphQL 쿼리로 우리는 우리가 원하는 것을 정확히 요청할 수 있다. 예를 들어, 교육 과정에 대한 API가 있다고 가정해 보겠습니다. API에 대한 쿼리는 다음과 같습니다.

```undefined
{
  getCourse {
    id
    title
    instructor
    publishDate
  }
}
```

이 쿼리를 보낼 때 API 및 속성에서 id, title, designator, publish date 순으로 응답을 받아야 합니다.

```undefined
{
  "data": {
    "getCourse": [
      {
        "id": "1",
        "title": "Python variables explained",
        "instructor": "Tracy Williams",
        "publishDate": "12th May 2020"
      },
      {
        "id": "2",
        "title": "How to use functions in Python",
        "instructor": "Jane Black",
        "publishDate": "9th April 2018"
      }
    ]
  }
}
```

선택적으로 API에 과정 목록을 반환하도록 요청할 수 있지만 이번에는 `제목` 속성만 있으면 됩니다.

```undefined
{
  getCourse {
    title
  }
}
```

다음과 같은 응답을 받아야 합니다.

```undefined
{
  "data": {
    "getCourse": [
      {
        "title": "Python variables explained"
      },
      {
        "title": "How to use functions in Python"
      }
    ]
  }
}
```

이러한 유연성은 GraphQL로 구축된 애플리케이션을 매우 확장 가능하게 하는 것 중 하나이며, GraphQL의 형식 선언을 통해 가능하다.

### 그래프QL 스키마

스키마는 그래프QL 서비스, 포함된 데이터 및 해당 데이터의 형식을 설명합니다. 질의에서 우리는 어떤 데이터가 우리에게 보내질지 그리고 그 데이터가 어떻게 표시되기를 원하는지 지정할 수 있다는 것을 알았습니다. 이것은 우리의 GraphQL API가 이미 모든 데이터에 대한 스키마를 포함하고 있고 그 스키마는 사용 가능한 모든 필드, 하위 필드 및 데이터 유형을 포함하기 때문이다.

이를 입증하기 위해 스키마를 만들겠습니다.모든 데이터 필드를 저장하는 데 사용할 루트 디렉터리의 py` 파일입니다. 코스 종류부터 시작하겠습니다. 이 과정에는 특정 과정에 대한 모든 정보가 포함되어야 하며 위의 예제 질의에서 과정은 ID, 제목, 강사 및 publish_date 필드를 포함합니다. 우리는 우리의 계획을 업데이트 할 것이다.다음을 포함한 py` 파일:

```python
from graphene import String, ObjectType

class CourseType(ObjectType):
  id = String(required=True)
  title = String(required=True)
  instructor = String(required=True)
  publish_date = String()
```

우리 스키마에.py 파일, 우리는 `그래핀`에서 `String`과 `ObjectType`을 가져오는 것부터 시작했습니다. 그래핀(Graphene)은 GraphQL 스키마와 유형을 구축하기 위한 파이썬 라이브러리이다. 터미널에서 다음 명령을 실행하여 설치하겠습니다.

```undefined
pip install graphene
```

Graphene에서 String과 ObjectType을 성공적으로 가져오고 나서 괄호 안에 가져온 ObjectType을 사용하여 클래스 CourseType을 정의했습니다. 거의 모든 그래프QL 유형을 개체 유형으로 선언합니다.

다음으로 우리가 한 일은 코스 타입의 다른 필드를 만든 것이고, 우리는 각 필드마다 스트링 타입을 사용했다. 그래핀의 다른 종류로는 인트(Int), 숫자(Enum), 날짜(Date), 리스트(List), 부울리언(Boolean) 등이 있다.

또한 ID, 제목, 강사 등에 대한 문자열 형식의 필수 주장을 추가했습니다. 따라서 질의할 때 해당 필드를 포함하지 않고서는 GraphQL 서비스에 과정을 추가할 수 없습니다.

## 임시 데이터베이스 설정

이제 스키마가 생겼으므로 과정 데이터를 저장하고 검색할 장소도 필요합니다. 이 데모에서는 JSON 데이터베이스를 사용합니다. 그러나 FastAPI는 Postgre와 같은 관계형 및 비관계형 데이터베이스를 모두 지원합니다.SQL, MySQL, MongoDB, ElasticSearch 등.

루트 디렉터리에 `cours.json` 파일을 생성하고 다음 코드 블록을 붙여넣어 보겠습니다.

```undefined
[
  {
    "id": "1",
    "title": "Python variables explained",
    "instructor": "Tracy Williams",
    "publish_date": "12th May 2020"
  },
  {
    "id": "2",
    "title": "How to use functions in Python",
    "instructor": "Jane Black",
    "publish_date": "9th April 2018"
  },
  {
    "id": "3",
    "title": "Asynchronous Python",
    "instructor": "Matthew Rivers",
    "publish_date": "10th July 2020"
  },
  {
    "id": "4",
    "title": "Build a REST API",
    "instructor": "Babatunde Mayowa",
    "publish_date": "3rd March 2016"
  }
]
```

GraphQL API를 사용하여 이 파일에서 데이터를 수정하고 검색할 수 있습니다.

## 쿼리 해결 프로그램 만들기

해결사는 당사의 GraphQL 서비스가 스키마 및 데이터 소스와 상호 작용하기 위해 사용하는 것입니다. 과정 가져오기를 위한 쿼리 해결사를 만들려면 먼저 `main.py` 파일의 `fastapi`를 가져오는 것부터 시작합니다.

```coffeescript
from fastapi import FastAPI
```

다음으로, 스키마에서와 같이 쿼리 해결기에서 사용할 관련 유형을 가져오도록 하겠습니다.py` 파일:

```coffeescript
...
from graphene import ObjectType, List, String, Schema
```

코스 타입의 래퍼로 사용할 리스트(List)와 작업 실행에 사용할 스키마(Schema) 등 두 가지 새로운 유형을 추가했습니다.

우리는 또한 `graphql.execution`에서 `Asyncio Executor`를 가져올 것이다.GraphQL 서비스에서 비동기식 호출을 할 수 있는 executers.asyncio, starlett.graphql의 GraphQLapp, 스키마의 CourseType을 사용할 수 있게 한다.py` 파일:

```coffeescript
...
from graphql.execution.executors.asyncio import AsyncioExecutor
from starlette.graphql import GraphQLApp
from schemas import CourseType
```

마지막으로 `cours.json` 파일로 작업할 수 있도록 내장된 `json` 패키지를 가져오도록 하겠습니다.

```coffeescript
...
import json
```

최종 수입 블록은 다음과 같습니다.

```js
from fastapi import FastAPI
from graphene import ObjectType, List, String, Schema
from graphql.execution.executors.asyncio import AsyncioExecutor
from starlette.graphql import GraphQLApp
from schemas import CourseType
import json
```

다음으로, 다음 코드 블록을 `main.py` 파일에 추가하여 쿼리를 만들어 보겠습니다.

```python
...
class Query(ObjectType):
  course_list = None
  get_course = List(CourseType)
  async def resolve_get_course(self, info):
    with open("./courses.json") as courses:
      course_list = json.load(courses)
    return course_list
```

위의 블록에서 개체 유형에서 클래스 `쿼리`를 생성한 다음 `라인 3`에서 `과정_리스트` 변수를 초기화하였다. 과정 데이터를 저장할 위치입니다.

목록의 여러 코스 객체를 반환하기 때문에, 우리는 그래핀에서 가져온 리스트 타입을 사용하여 코스 타입을 포장한 다음 4호선 get_course 변수에 할당했다. 이것이 저희 질의의 이름이 될 것입니다.

GraphQL 클라이언트에서 쿼리를 만들 때 낙타 케이스, 즉 get_course 대신 getCourse라는 이름을 제공해야 한다는 점에 유의해야 합니다.

다음으로, "get_course" 쿼리에 대한 "라인 5"에 해결 방법을 만들었습니다. 해결 방법 이름은 접두사 `resolve`로 시작하고 쿼리 이름(이 경우 `get_course`) 다음에 밑줄로 구분해야 합니다.

해결 방법에서는 또한 두 가지 위치 인수를 예상하는데, 이 인수를 메서드 정의에 포함시켰습니다. lines 6-8에서는 cours.json 파일에서 데이터를 로드하여 cours_list에 할당한 다음 변수를 반환합니다.

## FastAPI 기반 GraphQL 서버 시작

이제 GraphQL 쿼리를 만들었으므로 Fast를 초기화하겠습니다.API를 통해 GraphQL 서비스를 인덱스 경로에 할당합니다.

```undefined
...
app = FastAPI()
app.add_route("/", GraphQLApp(
  schema=Schema(query=Query),
  executor_class=AsyncioExecutor)
)
```

다음으로 터미널에서 다음 명령을 실행하여 Fast를 시작하십시오.API 앱:

```css
uvicorn main:app --reload
```

이와 유사한 성공 메시지가 표시됩니다.

```undefined
Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
Started reloader process
Started server process
Waiting for application startup.
Application startup complete.
```

GraphQL 서버는 http://127.0.0.1:8000으로 이동하여 테스트할 수 있습니다. 다음과 같은 페이지가 나타납니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/blank-graphql-server-page.png?resize=730%2C242&ssl=1)

왼쪽 창에 다음 쿼리를 붙여넣고 실행 버튼을 클릭하여 API 호출을 해보겠습니다.

```undefined
{
  getCourse {
    id
    title
    instructor
    publishDate
  }
}
```

다음과 같은 응답을 받아야 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/graphql-server-page-response.png?resize=730%2C277&ssl=1)

## 과정 하나만 가져오기

이제 한 과정에서만 데이터를 얻을 수 있도록 쿼리를 만드는 방법을 살펴보겠습니다. 질의에 `id` 매개 변수를 추가하여 GraphQL 서버에 해당 `id`와 일치하는 과정만 원한다는 것을 알리겠습니다. 이 작업을 수행하려면 `쿼리` 클래스를 아래 코드로 바꾸십시오.

```python
class Query(ObjectType):
  course_list = None
  get_course = Field(List(CourseType), id=String())
  async def resolve_get_course(self, info, id=None):
    with open("./courses.json") as courses:
      course_list = json.load(courses)
    if (id):
      for course in course_list:
        if course['id'] == id: return [course]
    return course_list
```

새로운 쿼리 클래스의 라인 3에서는 쿼리 매개 변수를 추가할 수 있는 필드 형식으로 get_course 값을 묶었다. 여기에 문자열 유형의 id 매개 변수를 추가했습니다. resolve_get_course() 방법에 id 매개 변수도 포함시켰고, 옵션으로 만들기 위해 기본값인 none(없음)을 부여했다.

7-9행에는 id와 일치하는 코스만 반환하는 조건문을 추가했다. 또한 다음 단계로 넘어가기 전에 `그래핀` 수입품에 `필드` 타입을 추가해야 합니다.

```js
from graphene import ObjectType, List, String, Schema, Field
```

이제 다음 쿼리와 `id`와 일치하는 과정을 하나만 가져올 수 있습니다.

```undefined
{
  getCourse(id: "2") {
    id
    title
    instructor
    publishDate
        }
}
```

우리는 이것을 우리의 대응으로 받아들여야 한다.

```undefined
{
  "data": {
    "getCourse": [
      {
        "id": "2",
        "title": "How to use functions in Python",
        "instructor": "Jane Black",
        "publishDate": "9th April 2018"
      }
    ]
  }
}
```

## 그래프QL 돌연변이

FastAPI로 GraphQL 서버를 설정하고 이 서버에서 데이터를 가져오는 방법을 확인했습니다. 이제 GraphQL 돌연변이를 사용하여 데이터 저장소에 새 과정을 추가하거나 기존 과정을 업데이트하는 방법을 살펴보겠습니다. 먼저 `그래핀` 수입품에 `뮤티션` 타입을 추가해 보겠습니다.

```js
from graphene import ObjectType, List, String, Schema, Field, Mutation
```

이제 Mutation 유형에서 CreateCourse라는 클래스를 만들 수 있습니다.

```python
class CreateCourse(Mutation):
  course = Field(CourseType)

  class Arguments:
    id = String(required=True)
    title = String(required=True)
    instructor = String(required=True)
```

`Create Course` 수업에서는 `Field` 클래스로 포장한 코스의 변수를 만드는 것으로 시작했습니다. 과정을 성공적으로 작성하면 사용자에게 반환됩니다.

그리고 나서 우리는 돌연변이 논쟁을 위한 클래스를 만들었습니다. 여기서 우리의 주장은 id, 제목, 강사이다. "Create Course" 돌연변이를 만들 때 이 정보를 제공해야 합니다.

다음으로, 우리는 과정을 만들기 위한 `변종` 방법이 필요할 것이다. 여기서 사용자가 제공한 인수를 사용하여 데이터 저장소에 새 과정을 작성합니다. 이 경우 `cours.json` 파일을 음소거합니다. 그러나 프로덕션 애플리케이션에서는 대신 데이터베이스가 필요할 수 있으며 앞에서 언급한 것처럼 FastAPI는 관계형 및 비관계형 데이터베이스를 모두 지원합니다.

돌연변이 방법을 만들어 봅시다. GraphQL 서비스는 다음과 같이 예상하므로 `변종`이라는 이름을 붙여야 합니다.

```python
class CreateCourse(Mutation):
  ...
  async def mutate(self, info, id, title, instructor):
    with open("./courses.json", "r+") as courses:
      course_list = json.load(courses)
      course_list.append({"id": id, "title": title, "instructor": instructor})
      courses.seek(0)
      json.dump(course_list, courses, indent=2)
    return CreateCourse(course=course_list[-1])
```

돌연변이 방법의 반환문에서 우리는 "Create Course" 클래스로 불렀고 괄호 안에 새로 만든 코스를 앞서 선언한 "Course" 변수에 할당했다. 이것은 우리의 GraphQL API가 돌연변이 요청에 대한 응답으로 사용자에게 반환되는 것이다.

이제 `CreateCourse` 클래스가 생겼으니 `Mutation`이라는 이름의 `ObjectType`에서 새로운 클래스를 만들어 보겠습니다. 모든 돌연변이를 여기에 저장하겠습니다.

```python
class Mutation(ObjectType):
  create_course = CreateCourse.Field()
```

이렇게 하면, 우리는 "app.add_route() 함수 호출에서 "Schema"에 "Mutation" 클래스를 추가할 수 있습니다.

```bash
app.add_route("/", GraphQLApp(
  schema=Schema(query=Query, mutation=Mutation),
  executor_class=AsyncioExecutor)
)
```

이제 GraphQL 클라이언트에서 다음 쿼리를 실행하여 이를 테스트할 수 있습니다.

```undefined
mutation {
  createCourse(
    id: "11" 
    title: "Python Lists" 
    instructor: "Jane Melody"
  ) {
    course {
      id
      title
      instructor
    }
  }
} 
```

그리고 우리는 다음과 같은 답변을 받아야 합니다.

```undefined
{
  "data": {
    "createCourse": {
      "course": {
        "id": "11",
        "title": "Python Lists",
        "instructor": "Jane Melody"
      }
    }
  }
}
```

## 요청 오류 처리

기존 ID에 대한 유효성 검사를 추가하여 앱에서 오류를 처리하는 방법을 알아보겠습니다. 사용자가 데이터 저장소에 이미 있는 ID로 과정을 작성하려고 할 경우, GraphQL 서버는 "제공된 ID를 가진 과정이 이미 존재합니다!"라는 오류 메시지와 함께 응답해야 합니다.

`CreateCourse` 돌연변이 함수의 데이터 저장소에 새 과정을 추가하기 직전에 다음 코드를 붙여넣어 보겠습니다.

```python
...
for course in course_list:
  if course['id'] == id:
    raise Exception('Course with provided id already exists!')
```

위에서, 우리는 데이터 저장소인 `과정_리스트`를 순환해서 들어오는 과정과 동일한 `id`를 가진 기존 과정이 있는지 확인했다. 만약 우리가 시합을 한다면, 예외는 주어져야 한다.

애플리케이션에 대해 선택하는 데이터베이스와 ORM에 따라 기존 값이 있는지 확인하는 프로세스가 다를 수 있습니다. 그러나 예외가 발생하면 그래프QL은 항상 오류를 반환합니다. 코드 변경으로 인해 `CreateCourse` 돌연변이 클래스는 다음과 같이 변해야 합니다.

```python
class CreateCourse(Mutation):
  course = Field(CourseType)

  class Arguments:
    id = String(required=True)
    title = String(required=True)
    instructor = String(required=True)
    publish_date = String()

  async def mutate(self, info, id, title, instructor):
    with open("./courses.json", "r+") as courses:
      course_list = json.load(courses)

      for course in course_list:
        if course['id'] == id:
          raise Exception('Course with provided id already exists!')

      course_list.append({"id": id, "title": title, "instructor": instructor})
      courses.seek(0)
      json.dump(course_list, courses, indent=2)
    return CreateCourse(course=course_list[-1])
```

이제 우리는 기존의 `id`로 새로운 과정을 만들어보면서 이것을 시험해 볼 수 있다. GraphQL 클라이언트의 마지막 `CreateCourse` 요청에서 정확한 변이를 실행해 보겠습니다.

```undefined
mutation {
  createCourse(
    id: "11" 
    title: "Python Lists" 
    instructor: "Jane Melody"
  ) {
    course {
      id
      title
      instructor
    }
  }
}
```

우리는 이것을 우리의 응답으로 받아야 한다.

```undefined
{
  "data": {
    "createCourse": null
  },
  "errors": [
    {
      "message": "Course with provided id already exists!",
      "locations": [
        {
          "line": 2,
          "column": 3
        }
      ],
      "path": [
        "createCourse"
      ]
    }
  ]
}
```

## 결론

이 기사에서는 FastAPI의 기본 사항 및 이를 사용하여 GraphQL 서버를 설정하는 방법에 대해 알아봤습니다. 이 두 기술의 조합은 웹 개발에서 정말 흥미로운 경험을 가져다 줍니다.

GraphQL을 사용하면 복잡한 쿼리를 비교적 쉽게 작성할 수 있는 동시에 웹 클라이언트가 원하는 것을 정확히 요구할 수 있는 권한을 부여할 수 있습니다. 또한 FastAPI를 통해 우리는 매우 적은 보일러 플레이트 코드로 강력한 고성능 GraphQL 서버를 구축할 수 있습니다.

데모 강좌에 대한 레포 링크입니다. 도움이 필요하시면 언제든지 링크드인에 대해 연락 주십시오.