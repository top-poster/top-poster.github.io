---
layout: post
title: "Django REST 프레임 워크 : 15 분 만에 API 빌드
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/django-rest-framework-build-api.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/django-rest-framework-build-api.png?fit=730%2C487&ssl=1)

Django REST 프레임 워크 (DRF)는 웹 API를 빌드하기위한 강력하고 유연한 툴킷입니다.
 이 가이드에서는 Django REST 프레임 워크를 사용하여 단 15 분 만에 CRUD API를 빌드하는 방법을 보여줍니다.
 

다음 내용을 다룹니다.
 

- 장고는 무엇입니까?
 
- REST API 란 무엇입니까?
 
- Django REST 프레임 워크 란 무엇입니까?
 
- Django REST 프레임 워크 설정
 
- RESTful 구조
 
- Django 앱용 모델 만들기
 
- Django에서 API보기 만들기 : 목록보기 및 자세히보기
 

시연하기 위해 샘플 할 일 애플리케이션 용 CRUD API를 빌드합니다.
 Django 프로젝트에서 Django REST 프레임 워크를 설정하는 것으로 시작하고 Django REST 프레임 워크로 CRUD REST API를 만드는 방법에 대한 전체 자습서가 이어집니다.
 

## 장고는 무엇입니까?
 

Django는 model-template-views 아키텍처 패턴을 따르는 Python 기반의 무료 오픈 소스 웹 프레임 워크입니다.
 웹 개발과 관련된 번거 로움을 줄여 주므로 휠을 재발 명하는 대신 앱 작성에 집중할 수 있습니다.
 

## REST API 란 무엇입니까?
 

REST API는 시스템에서 유용한 기능과 데이터를 노출하는 데 널리 사용되는 방법입니다.
 표현 상태 전송을 나타내는 REST는 주어진 URL에서 액세스 할 수 있고 JSON, 이미지, HTML 등과 같은 다양한 형식으로 반환 될 수있는 하나 이상의 리소스로 구성 될 수 있습니다.
 

## Django REST 프레임 워크 란 무엇입니까?
 

Django REST 프레임 워크 (DFR)는 웹 API를 빌드하기위한 강력하고 유연한 툴킷입니다.
 주요 이점은 직렬화를 훨씬 쉽게 만든다는 것입니다.
 

Django REST 프레임 워크는 Django의 클래스 기반 뷰를 기반으로하므로 Django에 익숙하다면 훌륭한 옵션입니다.
 클래스 기반 뷰, 양식, 모델 유효성 검사기, QuerySet 등과 같은 구현을 채택합니다.
 

## Django REST 프레임 워크 설정
 

프로젝트에 Django 및 Django REST 프레임 워크를 설치하여 시작하십시오.
 

```undefined
pip install django
pip install django_rest_framework
```

다음 명령을 사용하여`todo`라는 앱을 만듭니다.
 

```css
python manage.py startapp todo
```

다음으로`settings.py` 파일 내의`INSTALLED_APPS`에`rest_framework` 및`todo`를 추가합니다.
 

```undefined
# settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'todo'
]
```

`api` 폴더를 만들고 아래 디렉토리 구조에 구성된대로 새 파일을 추가합니다.
 

```css
├── drf_guide
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
├── db.sqlite3
├── manage.py
└── todo
    ├── admin.py
    ├── api
    │   ├── __init__.py
    │   ├── serializers.py
    │   ├── urls.py
    │   └── views.py
    ├── __init__.py
    ├── models.py
    ├── urls.py
    └── views.py
```

또한 기본`urls.py` 파일에 아래와 같이`rest_framework` 및 URL을 포함합니다.
 

```python
# drf_guide/urls.py : Main urls.py
from django.conf.urls import url
from django.urls import path, include
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    path('api-auth/', include('rest_framework.urls')),
    path('todos/', include('todo.urls')),
]
```

수퍼 유저를 만듭니다.
 나중에 다시 설명하겠습니다.
 

```css
python manage.py createsuperuser
```

또한 todo`urls.py` 안에 API URL을 포함합니다.
 

```coffeescript
# todo/urls.py : App urls.py
from django.conf.urls import url
from django.urls import path, include

urlpatterns = [
    path('api/', include('todo.api.urls')),
]
```

## RESTful 구조
 

RESTful API에서 엔드 포인트는 다음 HTTP 메서드를 사용하여 구조와 사용법을 정의합니다.
 

- `GET`
 
- `POST`
 
- `PUT`
 
- `삭제`
 

이러한 방법을 논리적으로 구성해야합니다.
 

Django REST 프레임 워크로 RESTful 앱을 빌드하는 방법을 보여주기 위해 예제 할 일 API를 만들겠습니다.
 아래 표와 같이 각각의 HTTP 메서드와 함께 두 개의 엔드 포인트를 사용합니다.
 

## Django 앱용 모델 만들기
 

해야 할 일 목록에 대한 모델을 만들어 보겠습니다.
 

```python
# todo/models.py
from django.db import models
from django.contrib.auth.models import User

class Todo(models.Model):
    task = models.CharField(max_length = 180)
    timestamp = models.DateTimeField(auto_now_add = True, auto_now = False, blank = True)
    completed = models.BooleanField(default = False, blank = True)
    updated = models.DateTimeField(auto_now = True, blank = True)
    user = models.ForeignKey(User, on_delete = models.CASCADE, blank = True, null = True)

    def __str__(self):
        return self.task
```

모델을 만든 후 데이터베이스로 마이그레이션합니다.
 

```css
python manage.py makemigrations
python manage.py migrate
```

### 모델 시리얼 라이저
 

`Model` 객체를 JSON과 같은 API에 적합한 형식으로 변환하기 위해 Django REST 프레임 워크는`ModelSerializer` 클래스를 사용하여 모든 모델을 직렬화 된 JSON 객체로 변환합니다.
 

```python
# todo/api/serializers.py
from rest_framework import serializers
from todo.models import Todo

class TodoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Todo
        fields = ["task", "completed", "timestamp", "updated", "user"]
```

## Django에서 API보기 만들기
 

이 섹션에서는 목록보기와 자세히보기의 두 가지 API보기를 만드는 방법을 살펴 봅니다.
 

### 목록보기
 verified_user

첫 번째 API 뷰 클래스는`todos / api /`엔드 포인트를 다룹니다. 여기서는 주어진 요청 된 사용자의 모든 할 일을 나열하는`GET`과 새 할 일을 생성하는`POST`를 처리합니다.
 인증 된 사용자 만 허용하는 `permission_classes`가 추가되었습니다.
 

```python
# todo/api/views.py
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from rest_framework import permissions
from todo.models import Todo
from .serializers import TodoSerializer

class TodoListApiView(APIView):
    # add permission to check if user is authenticated
    permission_classes = [permissions.IsAuthenticated]

    # 1. List all
    def get(self, request, *args, **kwargs):
        '''
        List all the todo items for given requested user
        '''
        todos = Todo.objects.filter(user = request.user.id)
        serializer = TodoSerializer(todos, many=True)
        return Response(serializer.data, status=status.HTTP_200_OK)

    # 2. Create
    def post(self, request, *args, **kwargs):
        '''
        Create the Todo with given todo data
        '''
        data = {
            'task': request.data.get('task'), 
            'completed': request.data.get('completed'), 
            'user': request.user.id
        }
        serializer = TodoSerializer(data=data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)

        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```

이 방법은 무엇을합니까?
 

`get ()`메소드는 먼저 요청 된 사용자 ID로 필터링하여 모델에서 모든 객체를 가져옵니다.
 그런 다음 모델 객체에서 JSON 직렬화 된 객체로 직렬화합니다.
 다음으로 직렬화 된 데이터와 상태가 `200_OK`인 응답을 반환합니다.
 

`post ()`메소드는 요청 된 데이터를 가져 와서`data` 사전에 요청 된 사용자 ID를 추가합니다.
 다음으로 직렬화 된 개체를 만들고 유효한 경우 개체를 저장합니다.
 유효한 경우 `201_CREATED`상태로 새로 생성 된 객체 인 `serializer.data`를 반환합니다.
 그렇지 않으면 `400_BAD_REQUEST`상태로 `serializer.errors`를 반환합니다.
 

위의 클래스 기반보기에 대한 끝점을 만듭니다.
 

```coffeescript
# todo/api/urls.py : API urls.py
from django.conf.urls import url
from django.urls import path, include
from .views import (
    TodoListApiView,
)

urlpatterns = [
    path('', TodoListApiView.as_view()),
]
```

Django 서버를 실행합니다.
 

```css
python manage.py runserver
```

이제 첫 번째 테스트를 할 준비가되었습니다.
 `http : //127.0.0.1 : 8000 / todos / api /`로 이동합니다.
 수퍼 유저 자격 증명으로 로그인했는지 확인하십시오.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/todo-list-api.png?resize=730%2C378&ssl=1)

다음을 게시하여 새 할 일을 만들 수 있습니다.
 

```undefined
{
    "task": "New Task",
    "completed": false
}
```

### 자세한 내용
 verified_user

이제 첫 번째 엔드 포인트보기를 성공적으로 만들었으므로 두 번째 엔드 포인트`todos / api / <int : todo_id>`API보기를 만들어 보겠습니다.
 

이 API 뷰 클래스에서는 위에서 설명한대로 해당 HTTP 메서드를 처리하기위한 세 가지 메서드 인`GET`,`PUT`,`DELETE`를 만들어야합니다.
 

```python
# todo/api/views.py
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from todo.models import Todo
from .serializers import TodoSerializer
from rest_framework import permissions

class TodoDetailApiView(APIView):
    # add permission to check if user is authenticated
    permission_classes = [permissions.IsAuthenticated]

    def get_object(self, todo_id, user_id):
        '''
        Helper method to get the object with given todo_id, and user_id
        '''
        try:
            return Todo.objects.get(id=todo_id, user = user_id)
        except Todo.DoesNotExist:
            return None

    # 3. Retrieve
    def get(self, request, todo_id, *args, **kwargs):
        '''
        Retrieves the Todo with given todo_id
        '''
        todo_instance = self.get_object(todo_id, request.user.id)
        if not todo_instance:
            return Response(
                {"res": "Object with todo id does not exists"},
                status=status.HTTP_400_BAD_REQUEST
            )

        serializer = TodoSerializer(todo_instance)
        return Response(serializer.data, status=status.HTTP_200_OK)

    # 4. Update
    def put(self, request, todo_id, *args, **kwargs):
        '''
        Updates the todo item with given todo_id if exists
        '''
        todo_instance = self.get_object(todo_id, request.user.id)
        if not todo_instance:
            return Response(
                {"res": "Object with todo id does not exists"}, 
                status=status.HTTP_400_BAD_REQUEST
            )
        data = {
            'task': request.data.get('task'), 
            'completed': request.data.get('completed'), 
            'user': request.user.id
        }
        serializer = TodoSerializer(instance = todo_instance, data=data, partial = True)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_200_OK)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    # 5. Delete
    def delete(self, request, todo_id, *args, **kwargs):
        '''
        Deletes the todo item with given todo_id if exists
        '''
        todo_instance = self.get_object(todo_id, request.user.id)
        if not todo_instance:
            return Response(
                {"res": "Object with todo id does not exists"}, 
                status=status.HTTP_400_BAD_REQUEST
            )
        todo_instance.delete()
        return Response(
            {"res": "Object deleted!"},
            status=status.HTTP_200_OK
        )
```

이 방법은 무엇을합니까?
 

`get ()`메소드는 먼저 ID가`todo_id` 인 객체와 사용자를 to-do 모델에서 요청 사용자로 가져옵니다.
 요청 된 객체를 사용할 수없는 경우 `400_BAD_REQUEST`상태의 응답을 반환합니다.
 그렇지 않으면 모델 객체를 JSON 직렬화 된 객체로 직렬화하고`serializer.data`와 상태가`200_OK` 인 응답을 반환합니다.
 

`put ()`메서드는 데이터베이스에서 사용할 수있는 작업 개체를 가져오고 요청 된 데이터로 데이터를 업데이트하고 업데이트 된 데이터를 데이터베이스에 저장합니다.
 

`delete ()`메소드는 데이터베이스에서 사용할 수있는 작업 객체를 가져 와서 삭제하고 응답으로 응답합니다.
 

아래와 같이 API`urls.py`를 업데이트합니다.
 

```coffeescript
# todo/api/urls.py : API urls.py
from django.conf.urls import url
from django.urls import path, include
from .views import (
    TodoListApiView,
    TodoDetailApiView
)

urlpatterns = [
    path('', TodoListApiView.as_view()),
    path('<int:todo_id>/', TodoDetailApiView.as_view()),
]
```

이제`http://127.0.0.1:8000/todos/api/ <id> /`로 이동하면 세부 API보기 페이지가 표시됩니다.
 유효한 ID로 올바르게 이동했는지 확인하십시오.
 아래 스크린 샷에서는 ID로 `7`을 사용했습니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/todo-detail-api.png?resize=730%2C382&ssl=1)

## 결론
 

축하합니다. 완전한 기능을 갖춘 첫 번째 CRUD Django REST API를 성공적으로 구축했습니다.
 

RESTful API를 빌드하는 것은 복잡 할 수 있지만 Django REST 프레임 워크는 복잡성을 상당히 잘 처리합니다.
 Django REST 프레임 워크를 사용하여 새로운 API를 재미있게 빌드하세요!
 