---
layout: post
title: "빠른 구축Vercel에 대한 API 애플리케이션"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/deploying-fastapi-applications-vercel.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/deploying-fastapi-applications-vercel.png?fit=730%2C487&ssl=1)

## 도입

애플리케이션을 웹 또는 클라우드 호스팅 플랫폼에 배포하는 것은 대개 개발 주기의 마지막 단계로, 사용자가 마침내 애플리케이션에 액세스할 수 있게 해줍니다. 이를 실현하기 위한 툴은 여러 가지가 있지만, 이 기사에서는 Fast를 구현하는 방법에 대해 알아보겠습니다.Vercel에 대한 API 애플리케이션

FastAPI는 백엔드 API 애플리케이션을 구축하기 위한 현대적이고 빠른 파이썬 웹 프레임워크이다. FastAPI는 스웨거, 보안 모듈 및 코드 정확성을 보장하기 위한 유형 검사와 함께 제공되는 API 설명서입니다.

### 전제조건

- 파이썬의 기본 이해
- 깃의 기본 이해
- 시스템에 설치된 Postman, virtualenv 또는 이와 동등한 기능
- Vercel을 사용한 계정

### 우리가 무엇을 만들 것인가

얼마나 빠른지 보여주는 방법Vercel에 API 어플리케이션이 구축되어, 우리는 간단한 노트 앱을 만들 것이다.

이 시점부터 Python과 Virtualenv가 설치되어 있다고 가정하겠습니다. 아래 명령을 실행하여 확인합니다.

```shell
$ python3 --version
```

그런 다음 실행:

```shell
$ virtualenv --version
```

## 세우다

너무 깊이 들어가기 전에 먼저 프로젝트 구조와 애플리케이션에 필요한 종속성 설치를 계획해 보겠습니다. 프로젝트 폴더를 만드는 것부터 시작합니다.

```shell
$ mkdir fastapi-notes-app && cd fastapi-notes-app
$ mkdir server
$ touch {main,server/api,server/routes,server/__init__}.py
```

그런 다음 기본 디렉토리에 가상 환경을 생성하고 필요한 종속성을 설치합니다.

```shell
$ virtualenv -p python3.8 venv
```

다음으로, 애플리케이션의 격리된 부분인 가상 환경을 활성화하여 앱의 종속성을 설치합니다. 이렇게 하려면 아래 명령을 실행합니다.

```shell
$ source venv/bin/activate
```

가상 환경을 구축한 상태에서 Fast를 설치합니다.API 및 Uvicorn:

```shell
(venv)$ pip3 install fastapi uvicorn
```

Uvicorn은 애플리케이션을 실행할 수 있게 해주는 ASGI(동기화 서버 게이트웨이 인터페이스) 서버입니다.

이제 FastAPI와 Uvicorn의 설치가 성공적이었는지 확인하기 위한 기본 경로를 만들어 보겠습니다.

```undefined
server/api.py
```

Fast를 가져오는 것부터 시작API 및 클래스 메소드를 변수 `app`로 초기화합니다.

```coffeescript
from fastapi import FastAPI

app = FastAPI()
```

그런 다음 경로를 정의합니다.

```undefined
@app.get("/", tags=["Root"])
async def read_root():
  return { 
    "message": "Welcome to my notes application, use the /docs route to proceed"
   }
```

응용 프로그램을 실행하려면 `main.py` 파일에 진입점을 정의해야 합니다. 시작 지점에서는 앞에서 설명한 대로 Uvicorn을 사용하여 서버를 실행합니다.

```cpp
//main.py
import uvicorn

if __name__ == "__main__":
  uvicorn.run("server.api:app", host="0.0.0.0", port=8000, reload=True)
```

메인 블록에서는 Uvicorn에서 run 메소드를 호출하고 다음 매개 변수를 취한다.

- 패스트의 위치API 인스턴스
- 호스트 주소
- 좌현
- 부울 재로드 값

`main.py` 파일을 실행합니다.

```shell
(venv)$ python3 main.py
```

위 명령은 다음 명령줄에 있는 것과 같은 출력을 반환해야 합니다.

```undefined
INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)
INFO:     Started reloader process [20586] using statreload
INFO:     Started server process [20588]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
```

앱은 `http://0.0.0:8000`에서 볼 수 있다. 애플리케이션 엔드포인트를 테스트하기 위해 포스트맨/인섬니아를 사용할 것입니다.

> 이 중 어느 것이든 빠른 것으로 바꾸십시오.API의 대화형 문서 'http://0.0.0.0:8000/docs'입니다.

다음으로, GET 요청을 우체부(또는 인섬니아)의 `http://0.0.0.0:8000`에 보냅니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/get-request.png?resize=730%2C377&ssl=1)

## 모델 스키마 정의

응용 프로그램의 모델 스키마를 정의하겠습니다. 이는 애플리케이션에 데이터가 저장되는 방식을 나타냅니다. `app` 폴더에서 새 파일 `model`을 만듭니다.py:

```python
from typing import Optional
from pydantic import BaseModel

class NoteSchema(BaseModel):
  title: Optional[str]
  content: Optional[str]

  class Config:
    schema_extra = {
        "example": {
            "title": "LogRocket.",
            "content": "Logrocket is the most flexible publishing company for technical authors. From editors to payment, the process is too flexible and that's what makes it great."
        }
    }
```

위의 코드 블록에서, 우리는 애플리케이션의 임시 데이터베이스에 노트 데이터가 어떻게 저장될 것인지를 나타내는 Pydantic 스키마 `NoteSchema`를 정의했다. 하위 클래스 구성에는 대화형 문서에서 요청을 보낼 때 사용자를 안내하는 예제 요청 본문이 있습니다.

CRUD 작업에 대한 경로를 `route`에서다음 섹션에 있는 py` 파일입니다.

## 경로 정의

스키마가 설치된 상태에서 노트를 저장하고 검색할 수 있는 앱 내 데이터베이스를 만들고 노트 스키마를 가져오도록 하겠습니다.

### ➡➡➡파이의

빠른 가져오기부터 시작API의 `APIRouter` 클래스와 `NoteSchema` 클래스:

```coffeescript
from fastapi import APIRouter, Body
from fastapi.encoders import jsonable_encoder
from server.model import NoteSchema

router = APIRouter()
```

`router` 변수 바로 아래에 임시 데이터베이스 `notes`notes`:

```undefined
notes = {
    "1": {
        "title": "My first note",
        "content": "This is the first note in my notes application"
    },
    "2": {
        "title": "Uniform circular motion.",
        "content": "Consider a body moving round a circle of radius r, wit uniform speed v as shown below. The speed everywhere is the same as v but direction changes as it moves round the circle."
    }
}
```

그런 다음 GET 요청의 경로를 정의합니다.

```python
@router.get("/")
async def get_notes() -> dict:
    return {
        "data": notes
    }

@router.get("/{id}")
async def get_note(id: str) -> dict:
    if int(id) > len(notes):
        return {
            "error": "Invalid note ID"
        }

    for note in notes.keys():
        if note == id:
            return {
                "data": notes[note]
            }
```

위의 코드 블록에서 두 가지 경로를 정의했습니다.

- 사용 가능한 모든 노트를 반환하는 `/note` 경로
- 전달된 ID와 일치하는 ID를 가진 노트를 반환하는 `/note/{id}` 경로

경로 테스트를 진행하기 전에 다음과 같이 `api.py`의 글로벌 경로 처리기에 노트 라우터를 포함시키십시오.

```coffeescript
from server.routes import router as NoteRouter

...

app.include_router(NoteRouter, prefix="/note")
```

FastAPI(.include_router() 메서드는 글로벌 경로 처리기의 다른 파일에 선언된 경로를 포함하는 데 사용됩니다. 이 방법은 경로를 별도의 파일과 디렉터리로 분할하는 응용 프로그램에서 유용합니다.

## 항로 테스트

노트 경로가 설치된 상태에서 경로를 테스트해 보겠습니다.

- GET `/note`:
- GET `/note/{id}: 임시 데이터베이스에는 ID 1과 ID 2가 있는 노트 2개를 추가했습니다. `notes` 데이터베이스에 없는 ID를 전달하면 오류 응답이 반환됩니다. 다음 순서로 유효한 ID와 잘못된 ID를 모두 사용해 봅니다.

데이터베이스에 없는 ID의 경우:

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/id-not-in-database.png?resize=730%2C377&ssl=1)

그런 다음 새 노트를 추가할 POST 경로를 정의합니다.

```python
@router.post("/note")
async def add_note(note: NoteSchema = Body(...)) -> dict:
    note.id = str(len(notes) + 1)
    notes[note.id] = note.dict()

    return {
        "message": "Note added successfully"
    }
```

add_note 함수에서 우리는 노트를 우리의 모델인 노트 스키마 유형으로 설정하고, 그것을 본문(…)을 사용하여 필수 인수로 만들었다. 본문() 문의 줄임표는 이 요청 본문을 스키마 사양에 따라 채워야 함을 나타냅니다.

POST 경로를 테스트하려면 Postman/Insomnia에서 GET에서 POST로 요청 유형을 변경하고 URL 주소를 `http://0.0.0.0:8000/note`로 변경해야 합니다. 그런 다음 요청 본문을 JSON으로 설정하고 아래의 JSON 코드를 전달합니다.

```undefined
{
    "title": "Deploying FastAPI applications to Vercel",
    "content": "In this article, you will be learning how to build and in turn deploy a FastAPI application to Vercel."
}
```

이제 다음 요청을 보냅니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/send-request.png?resize=730%2C377&ssl=1)

노트가 성공적으로 추가되었습니다. `/note` 끝점에서 GET 요청을 실행하여 다음을 확인합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/run-get-request.png?resize=730%2C377&ssl=1)

그런 다음 `업데이트` 및 `삭제` 경로를 정의합니다.

```python
@router.put("/{id}")
def update_note(id: str, note: NoteSchema):
    stored_note = notes[id]
    if stored_note:
        stored_note_model = NoteSchema(**stored_note)
        update_data = note.dict(exclude_unset=True)
        updated_note = stored_note_model.copy(update=update_data)
        notes[id] = jsonable_encoder(updated_note)
        return {
            "message": "Note updated successfully"
        }
    return {
        "error": "No such with ID passed exists."
    }


@router.delete("/{id}")
def delete_note(id: str) -> dict:
    if int(id) > len(notes):
        return {
            "error": "Invalid note ID"
        }

    for note in notes.keys():
        if note == id:
            del notes[note]
            return {
                "message": "Note deleted"
            }

    return {
        "error": "Note with {} doesn't exist".format(id)
    }
```

업데이트 경로에서 부분 업데이트를 수행하고 있습니다. 노트가 있는 경우에만 노트를 업데이트하고, 그렇지 않은 경우 오류 메시지를 반환합니다. 삭제 경로에도 동일한 논리를 적용합니다. 삭제하기 전에 먼저 노트가 존재하는지 확인하고, 그렇지 않으면 오류 메시지를 반환합니다. 경로를 테스트해 봅시다.

`업데이트` 경로는 다음과 같습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/update-route.png?resize=730%2C377&ssl=1)

이제 두 번째 노트를 삭제하여 `삭제` 경로를 테스트해 보겠습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/delete-route-test.png?resize=730%2C377&ssl=1)

경로를 설정하고 테스트하면 Vercel로 배포를 진행할 수 있습니다.

## NAT의 빠른 구현Vercel에 API 앱

이 섹션에서는 Vercel에 배포합니다. Vercel 명령줄 도구가 설치되어 있지 않은 경우 다음 명령을 실행하여 해당 도구를 가져올 수 있습니다.

```undefined
yarn global add vercel
```

다음으로, 로그온:

```undefined
vercel login
```

Vercel에 배포하려면 `vercel.json` 구성 파일이 필요합니다. 상위 디렉터리에 `vercel.json` 파일을 생성하고 다음 JSON 코드를 추가하십시오.

```undefined
{
  "builds": [
    {"src": "/server/api.py", "use": "@now/python"}
  ],
  "routes": [
    {"src": "/(.*)", "dest": "server/api.py"}
  ]
}
```

위의 코드 블록에서 빌드 키는 다른 개체를 포함하는 배열을 유지합니다. 이 개체에서 응용 프로그램의 진입점으로 가는 경로를 표시했습니다. 우리는 또한 "routs" 객체에서 우리의 앱을 만들 때 사용할 패키지를 명시했다. 모든 라우팅을 `server/api.py` 파일로 전달합니다.

배포를 진행하기 전에 `요구 사항`을 생성해 보겠습니다.애플리케이션 종속성이 포함된 txt 파일:

```cpp
//requirements.txt
fastapi
uvicorn
```

구성 및 요구 사항 파일이 있는 상태에서 Vercel을 초기화하겠습니다. 상위 디렉토리에서 다음 명령을 실행합니다.

```undefined
vercel .
```

콘솔의 프롬프트에 따라 유사한 화면이 나타납니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/vercel-prompt-in-console.png?resize=730%2C215&ssl=1)

Vercel에 애플리케이션을 성공적으로 구현한 것은 단 4단계에 불과합니다. 콘솔이나 Vercel 대시보드에서 링크를 클릭하여 배포된 애플리케이션을 미리 볼 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/viewing-deployed-application.png?resize=730%2C356&ssl=1)

이 문서에 배포된 애플리케이션은 여기에서 볼 수 있습니다.

## 결론

이 기사에서는 Fast를 구축하고 배포하는 방법에 대해 알아봤습니다.API 응용 프로그램. 공식 문서에서 FastAPI에 대한 자세한 내용을 볼 수 있으며 GitHub의 이 기사에서 사용된 코드를 찾을 수 있습니다.