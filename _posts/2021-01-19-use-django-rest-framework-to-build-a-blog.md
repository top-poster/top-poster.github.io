---
layout: post
title: "Django REST Framework를 사용하여 블로그 작성
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/django-rest-framework-api-blog.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/django-rest-framework-api-blog.png?fit=730%2C487&ssl=1)

API 서비스를 사용하면 애플리케이션이 JSON 인코딩 데이터를 사용하여 다른 애플리케이션에 연결할 수 있습니다.
 API를 한 번 만들고 API 클라이언트 또는 프런트 엔드 애플리케이션에서 사용합니다.
 

Django REST Framework는 Django로 REST API를 빌드하기위한 툴킷입니다.
 이 자습서에서는 Django REST Framework를 사용하여 블로그 API를 빌드합니다.
 이 API에는 사용자, 블로그 게시물, 댓글 및 카테고리에 대한 엔드 포인트가 있습니다.
 

또한 인증 된 사용자 만 앱의 데이터를 수정할 수 있도록 사용자 작업을 인증하는 방법을 알아 봅니다.
 

이 API 프로젝트는 다음 기술을 보여줍니다.
 

- API에 신규 및 기존 Django 모델 추가
 
- 공통 API 패턴을위한 내장 직렬 변환기를 사용하여 이러한 모델 직렬화
 
- 보기 및 URL 패턴 만들기
 
- 다 대일 및 다 대다 관계 정의
 
- 사용자 작업 인증
 
- Django REST Framework 탐색 가능한 API 사용
 

## Django REST Framework 사용을위한 필수 구성 요소
 

시스템에 Python 3이 설치되어 있어야하며 REST API와 상호 작용 한 경험이 있어야합니다.
 또한 기본 및 외래 키, 데이터베이스 모델, 마이그레이션, 다 대일 및 다 대다 관계를 포함한 관계형 데이터베이스에 익숙해야합니다.
 

또한 Python 및 Django에 대한 경험이 필요합니다.
 

## Python 환경 설정
 

새 API 프로젝트를 만들려면 먼저 작업 디렉터리에 Python 환경을 설정합니다.
 터미널에서 다음을 실행하십시오.
 

```bash
python3 -m venv env
source env/bin/activate
```

> Windows에서는 대신`source env \ Scripts \ activate`를 실행하십시오.
 

이 가상 환경에서이 튜토리얼의 모든 명령을 실행해야합니다 (터미널의 입력 행 시작 부분에`(env)`가 있는지 확인하십시오).
 

> 이 환경을 비활성화하려면`deactivate`를 실행하세요.
 

다음으로 Django 및 Django REST 프레임 워크를 가상 환경에 설치합니다.
 

```undefined
pip install django
pip install djangorestframework
```

그런 다음`blog`라는 새 프로젝트와`api`라는 앱을 만듭니다.
 

```bash
django-admin startproject blog
cd blog
django-admin startapp api
```

루트`blog` 디렉토리 (`manage.py` 파일이있는 위치)에서 초기 데이터베이스를 동기화합니다.
 이렇게하면`admin`,`auth`,`contenttypes` 및`sessions`에 대한 마이그레이션이 실행됩니다.
 

```css
python manage.py migrate
```

Django 관리 사이트 및 탐색 가능한 API와 상호 작용하려면`admin` 사용자가 필요합니다.
 터미널에서 다음을 실행합니다.
 

```css
python manage.py createsuperuser --email admin@example.com --username admin
```

원하는 암호를 설정하십시오 (최소 8 자 이상이어야 함).
 비밀번호를 `password123`과 같이 설정하면 비밀번호가 너무 일반적이라는 오류가 발생할 수 있습니다.
 

Django REST Framework API를 설정하려면`rest_framework` 및`api` 앱을`blog / blog / settings.py`에 추가하세요.
 

```undefined
INSTALLED_APPS = [
    # code omitted for brevity
    'rest_framework',
    'api.apps.ApiConfig',
]
```

`ApiConfig` 객체를 추가하면 앱에 다른 구성 옵션을 추가 할 수 있습니다 (AppConfig 문서 참조).
 이 가이드를 완료하기 위해 다른 옵션을 지정할 필요가 없습니다.
 

마지막으로 터미널에서 다음 명령을 사용하여 로컬 개발 서버를 시작합니다.
 

```css
python manage.py runserver
```

`http://127.0.0.1:8000/admin`으로 이동하고 로그인하여 Django 관리 사이트를 확인합니다.
 사용자를 클릭하여 새 관리자를 보거나 한두 명의 새 사용자를 추가합니다.
 

## Django REST 프레임 워크 용 사용자 API 만들기
 

이제`admin` 사용자와 한두 명의 다른 사용자가 있으므로 사용자 API를 설정합니다.
 이렇게하면 API 끝점 집합에서 사용자 목록과 단일 사용자에 대한 읽기 전용 액세스가 허용됩니다.
 

### 사용자 직렬 변환기
 

Django REST Framework는 serializer를 사용하여 쿼리 세트 및 모델 인스턴스를 JSON 데이터로 변환합니다.
 직렬화는 또한 API가 클라이언트에 대한 응답으로 반환하는 데이터를 결정합니다.
 

Django의 사용자는`django.contrib.auth`에 정의 된`User` 모델에서 생성됩니다.
 

`User` 모델에 대한 직렬 변환기를 만들려면`blog / api / serializers.py`에 다음을 추가합니다.
 

```python
from rest_framework import serializers
from django.contrib.auth.models import User

class UserSerializer(serializers.ModelSerializer):
    class Meta:
        model = User
        fields = ['id', 'username']
```

이 예제에서 볼 수 있듯이 Django REST 프레임 워크의 직렬 변환기 컬렉션과 함께 Django에서`User` 모델을 가져옵니다.
 

이제`ModelSerializer` 클래스에서 상속해야하는`UserSerializer` 클래스를 만듭니다.
 

이 serializer와 연결되어야하는 모델을 정의합니다 (`model = User`).
 `fields` 배열은 모델의 어떤 필드가 직렬 변환기에 포함되어야 하는지를 나타냅니다.
 예를 들어`first_name` 및`last_name` 필드를 추가 할 수도 있습니다.
 

`ModelSerializer` 클래스는 해당 모델의 필드를 기반으로하는 직렬 변환기 필드를 생성합니다.
 즉, 이러한 속성은 모델 자체에서 가져 오므로 serializer 필드에 대한 속성을 수동으로 지정할 필요가 없습니다.
 

이 serializer는 간단한 create () 및 update () 메서드도 만듭니다.
 필요한 경우 재정의 할 수 있습니다.
 

> `ModelSerializer`의 작동 방식과 데이터에 대한 더 많은 제어를 위해 다른 직렬 변환기를 사용하는 방법에 대한 자세한 내용은 직렬 변환기를 참조하세요.
 

### 사용자보기
 

Django REST Framework에서 뷰를 만드는 방법에는 여러 가지가 있습니다.
 재사용 가능한 기능과 DRY 코드를 유지하려면 클래스 기반 뷰를 사용하십시오.
 

Django REST Framework는`APIView` 클래스를 기반으로하는 몇 가지 일반 뷰 클래스를 제공합니다.
 이러한보기는 가장 일반적으로 사용되는 API 패턴을위한 것입니다.
 

예를 들어`ListAPIView`는 읽기 전용 엔드 포인트에 사용되며`get` 메소드 핸들러를 제공합니다.
 `ListCreateAPIView` 클래스는 읽기-쓰기 엔드 포인트에 사용되며`get` 및`post` 메소드 핸들러를 제공합니다.
 

사용자 목록에 대한 읽기 전용보기와 단일 사용자에 대한 읽기 전용보기를 만들려면`blog / api / views.py`에 다음을 추가하세요.
 

```python
from rest_framework import generics
from api import serializers
from django.contrib.auth.models import User

class UserList(generics.ListAPIView):
    queryset = User.objects.all()
    serializer_class = serializers.UserSerializer

class UserDetail(generics.RetrieveAPIView):
    queryset = User.objects.all()
    serializer_class = serializers.UserSerializer
```

이 코드에서 알 수 있듯이 이전 단계에서 정의한`User` 모델 및`UserSerializer`와 함께 Django REST Framework의`generics` 뷰 컬렉션을 가져옵니다.
 `UserList`보기는 사용자 목록에 대한 읽기 전용 액세스 (`get`을 통해)를 제공합니다.
 `UserDetail` 뷰는 단일 사용자에게 읽기 전용 액세스 (`get`을 통해)를 제공합니다.
 

뷰 이름은 각각 개체 목록 및 단일 개체의 경우`{ModelName} List` 및`{ModelName} Detail` 형식이어야합니다.
 

각 뷰에 대해`queryset` 변수에는`User.objects.all ()`에서 반환 된 모델 인스턴스 목록이 포함됩니다.
 `serializer_class`는`User` 데이터를 직렬화하는`UserSerializer`로 설정해야합니다.
 

다음 단계에서 이러한보기에 대한 끝점 경로를 설정합니다.
 

### 사용자 URL 패턴
 

사용자를위한 모델, 시리얼 라이저 및 뷰 세트를 사용하여 마지막 단계는 각 뷰에 대한 엔드 포인트 경로 (Django에서 "URL 패턴"이라고 함)를 정의하는 것입니다.
 

먼저`blog / api / urls.py`에 다음을 추가합니다.
 

```coffeescript
from django.urls import path
from rest_framework.urlpatterns import format_suffix_patterns
from api import views

urlpatterns = [
    path('users/', views.UserList.as_view()),
    path('users/<int:pk>/', views.UserDetail.as_view()),
]

urlpatterns = format_suffix_patterns(urlpatterns)
```

여기에서 Django의`path` 함수와`api` 앱의 뷰 컬렉션을 가져 왔습니다.
 

`path` 함수는 Django가 앱에 페이지를 표시하는 데 사용하는 요소를 만듭니다.
 이를 위해 Django는 먼저 URL 패턴 (예 :`users /`)을 사용자가 요청한 URL과 일치시켜 올바른 요소를 찾습니다.
 그런 다음 해당 뷰 (예 :`UserList`)를 가져와 호출합니다.
 

`<int : pk>`시퀀스는 기본 키 (`pk`) 인 정수 값을 나타냅니다.
 Django는 URL의이 부분을 캡처하여 키워드 인수로 뷰에 보냅니다.
 

이 경우`User`의 기본 키는`id` 필드이므로`example.com / users / 1`은`id`가`1` 인 사용자를 반환합니다.
 

이러한 URL 패턴 (및이 자습서의 뒷부분에서 만들 패턴)과 상호 작용하려면 먼저 Django 프로젝트에 추가해야합니다.
 다음을`blog / blog / urls.py`에 추가하십시오.
 

```coffeescript
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('api.urls')),
]
```

이러한 부분이 제대로 작동하는지 확인하려면 브라우저에서 `http://127.0.0.1:8000/users`로 이동하여 앱 사용자 목록을 확인하세요.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/user-list.png?resize=730%2C319&ssl=1)

> 이 가이드에서는 Django REST Framework의 탐색 가능한 API를 사용하여이 가이드에서 만든 엔드 포인트를 설명합니다.
 이 GUI는 프런트 엔드 클라이언트를 모방 한 인증 및 양식을 제공합니다.
 원하는 경우`cURL` 또는`httpie`를 사용하여 터미널에서 API를 테스트 할 수도 있습니다.
 

`admin` 사용자의`id` 값을 기록하고 해당 사용자의 엔드 포인트로 이동합니다.
 예를 들어`id`가`1`이면`http : //127.0.0.1 : 8000 / users / 1`로 이동합니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/user-detail.png?resize=730%2C255&ssl=1)

요약하면 Django의 모델 클래스는`UserSerializer`를 사용하여 직렬화됩니다.
 이 시리얼 라이저는`users /`및`users / <int : pk> /`URL 패턴을 사용하여 액세스되는`UserList` 및`UserDetail` 뷰에 데이터를 제공합니다.
 

## Post API 생성
 

기본 사용자 API를 설정하면 이제 게시물, 댓글 및 카테고리에 대한 엔드 포인트를 사용하여 블로그에 대한 완전한 API를 만들 수 있습니다.
 Post API를 작성하여 시작하십시오.
 

### 포스트 모델
 

`blog / api / models.py`에서 Django의`Model` 클래스에서 상속하는`Post` 모델을 만들고 해당 필드를 정의합니다.
 

```python
from django.db import models

class Post(models.Model):
    created = models.DateTimeField(auto_now_add=True)
    title = models.CharField(max_length=100, blank=True, default='')
    body = models.TextField(blank=True, default='')
    owner = models.ForeignKey('auth.User', related_name='posts', on_delete=models.CASCADE)

    class Meta:
        ordering = ['created']
```

필드 유형은 관계형 데이터베이스에서 일반적으로 사용되는 필드 유형에 해당합니다.
 Django 모델 및 필드 유형에 대한 자세한 정보는 모델을 참조하십시오.
 

`ForeignKey`유형은 현재 모델과 첫 번째 인수에 표시된 모델 ( `auth.User`, 작업 한 `User`모델)간에 다 대일 관계를 생성합니다.
 

이 경우 한 사용자가 여러 게시물의 소유자가 될 수 있지만 각 게시물에는 한 명의 소유자 만있을 수 있습니다.
 `owner` 필드는 프런트 엔드 앱에서 사용자를 검색하고 사용자 이름을 게시물 작성자로 표시하는 데 사용될 수 있습니다.
 

`related_name` 인수를 사용하면 기본값 (`post_set`) 대신 현재 모델 (`posts`)에 대한 사용자 지정 액세스 이름을 설정할 수 있습니다.
 이 게시물 목록은 다 대일 관계를 완료하기 위해 다음 단계에서`사용자`직렬 변환기에 추가됩니다.
 

모델을 수정할 때마다 다음을 실행하여 데이터베이스를 업데이트하십시오.
 

```css
python manage.py makemigrations api
python manage.py migrate
```

이것들은 당신이 작업해온`User` 모델과 같은 Django 모델이기 때문에 여러분의 게시물은`blog / api / admin.py`에 등록하여 Django의 관리 사이트에서 수정할 수 있습니다.
 

```coffeescript
from django.contrib import admin
from api.models import Post

admin.site.register(Post)
```

나중에 찾아 볼 수있는 API에서 게시물을 만들 수 있습니다.
 

지금은`http : //127.0.0.1 : 8000 / admin`으로 이동하여 게시물을 클릭하고 몇 개의 게시물을 추가합니다.
 이 양식의`title` 및`body` 필드는`Post` 모델에 정의 된`CharField` 및`TextField` 유형에 해당합니다.
 

기존 사용자 중에서 `소유자`를 선택할 수도 있습니다.
 탐색 가능한 API를 사용하여 게시물을 만들 때 사용자를 선택할 필요가 없습니다.
 `소유자`는 현재 로그인 한 사용자로 자동 설정됩니다. 다음 단계에서 설정합니다.
 

### 포스트 시리얼 라이저
 

API에`Post` 모델을 추가하려면`User` 모델에서 따랐던 것과 유사한 프로세스를 따릅니다.
 

먼저 `Post`모델 데이터를 직렬화해야합니다.
 `blog / api / serializers.py`에 다음을 추가합니다.
 

```python
# code omitted for brevity
from api.models import Post

class PostSerializer(serializers.ModelSerializer):
    owner = serializers.ReadOnlyField(source='owner.username')

    class Meta:
        model = Post
        fields = ['id', 'title', 'body', 'owner']

class UserSerializer(serializers.ModelSerializer):
    posts = serializers.PrimaryKeyRelatedField(many=True, read_only=True)

    class Meta:
        model = User
        fields = ['id', 'username', 'posts']
```

이 예에서 볼 수 있듯이`api` 앱에서`Post` 모델을 가져오고`ModelSerializer` 클래스에서 상속되는`PostSerializer`를 만듭니다.
 이 직렬 변환기에서 사용할 모델 및 필드를 설정합니다.
 

`ReadOnlyField`는 수정없이 값을 반환하는 필드 클래스입니다.
 이 경우 기본 `id`필드 대신 소유자의 `username`필드를 반환하는 데 사용됩니다.
 

다음으로`UserSerializer`에`posts` 필드를 추가합니다.
 게시물과 사용자 간의 다 대일 관계는 이전 단계의 `Post`모델에서 정의했습니다.
 필드 이름 (`posts`)은`Post.owner` 필드의`related_field` 인수와 동일해야합니다.
 이전 단계에서`related_field` 값을 지정하지 않은 경우`posts`를`post_set` (기본값)으로 변경합니다.
 

`PrimaryKeyRelatedField`는이 다 대일 관계에있는 게시물 목록을 나타냅니다 (`many = True`는 하나 이상의 게시물이 있음을 나타냄).
 

`read_only = True`를 설정하지 않으면`posts` 필드에 기본적으로 쓰기 권한이 있습니다.
 즉, 사용자가 생성 될 때 사용자에게 속한 게시물 목록을 수동으로 설정할 수 있습니다.
 이것은 아마도 당신이 원하는 행동이 아닐 것입니다.
 

다시`http : //127.0.0.1 : 8000 / users`로 이동하여 각 사용자의`posts` 필드를 확인합니다.
 

> `posts` 목록은 실제로 post`id` 값의 목록입니다.
 대신`HyperlinkedModelSerializer`를 사용하여 URL 목록을 반환 할 수 있습니다.
 

### 조회수 게시
 

다음 단계는 Post API에 대한보기 세트를 만드는 것입니다.
 다음을`blog / api / views.py`에 추가합니다.
 

```python
# code omitted for brevity
from api.models import Post

class PostList(generics.ListCreateAPIView):
    queryset = Post.objects.all()
    serializer_class = serializers.PostSerializer

    def perform_create(self, serializer):
        serializer.save(owner=self.request.user)

class PostDetail(generics.RetrieveUpdateDestroyAPIView):
    queryset = Post.objects.all()
    serializer_class = serializers.PostSerializer

# code omitted for brevity
```

`ListCreateAPIView`와`RetrieveUpdateDestroyAPIView`는 함께 가장 일반적인 API 메소드 핸들러를 제공합니다. 목록에 대한`get` 및`post` (`ListCreateAPIView`)와 단일 엔티티에 대한`get`,`update` 및`delete` (
 `RetrieveUpdateDestroyAPIView`).
 

또한 기본 `perform_create`함수를 재정 의하여 `owner`필드를 현재 사용자 ( `self.request.user`값)로 설정해야합니다.
 

### URL 패턴 게시
 

Post API에 대한 엔드 포인트를 완료하려면 Post URL 패턴을 작성하십시오.
 `blog / api / urls.py`의`urlpatterns` 배열에 다음을 추가합니다.
 

```undefined
# code omitted for brevity

urlpatterns = [
    # code omitted for brevity
    path('posts/', views.PostList.as_view()),
    path('posts/<int:pk>/', views.PostDetail.as_view()),
]
```

이러한 URL 패턴과 뷰를 결합하면`get posts /`,`post posts /`,`get posts / <int : pk> /`,`put posts / <int : pk> /`및`delete posts /가 생성됩니다.
 <int : pk> /`끝점.
 

이러한 엔드 포인트를 테스트하려면`http : //127.0.0.1 : 8000 / posts / 1`과 같은 단일 게시물로 이동하고 삭제를 클릭합니다.
 게시물 제목을 변경하려면`title` 필드 값을 변경하고 PUT을 클릭하여 업데이트합니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/post-detail.png?resize=730%2C450&ssl=1)

`http : //127.0.0.1 : 8000 / posts`로 이동하여 기존 게시물 목록을 보거나 새 게시물을 만듭니다.
 게시물의 소유자가 현재 사용자로 설정되어 있으므로 게시물 작성을 시도 할 때 로그인했는지 확인하십시오.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/post-list.png?resize=730%2C474&ssl=1)

## 권한 설정
 

편의를 위해 `blog / urls.py`에 다음 경로를 추가하여 탐색 가능한 API에 로그인 버튼을 추가 할 수 있습니다.
 

```undefined
# code omitted for brevity

urlpatterns = [
    # code omitted for brevity
    path('api-auth/', include('rest_framework.urls')),
]
```

이제 다른 사용자 계정에 로그인 및 로그 아웃하여 권한을 테스트하고 탐색 가능한 API를 사용하여 게시물을 수정할 수 있습니다.
 

현재 로그인 한 상태에서는 게시물을 작성할 수 있지만 게시물을 삭제하거나 수정하기 위해 로그인 할 필요는 없습니다. 본인 소유가 아닌 게시물도 마찬가지입니다.
 다른 사용자 계정으로 로그인 해보십시오.
 `admin`이 소유 한 게시물을 수정하거나 삭제할 수 있어야합니다.
 

사용자를 인증하고 게시물의 소유자 만 기존 게시물을 업데이트하거나 삭제할 수 있도록하려면 API에 권한을 추가해야합니다.
 

먼저`blog / api / permissions.py`에 다음을 추가합니다.
 

```python
from rest_framework import permissions

class IsOwnerOrReadOnly(permissions.BasePermission):
    def has_object_permission(self, request, view, obj):
        if request.method in permissions.SAFE_METHODS:
            return True

        return obj.owner == request.user
```

> 이 권한에 대한 코드는 Django REST Framework 문서에서 가져 왔습니다.
 

사용자 지정`IsOwnerOrReadOnly` 권한은 요청하는 사용자가 지정된 개체의 소유자인지 여부를 확인합니다.
 이 경우 소유자 만 게시물 업데이트 또는 삭제와 같은 작업을 수행 할 수 있습니다.
 읽기 전용 작업이므로 비 소유자가 게시물을 검색 할 수 있습니다.
 

내장 된`IsAuthenticatedOrReadOnly` 권한도 있습니다.
 이 권한이 있으면 인증 된 사용자는 모든 요청을 수행 할 수있는 반면, 인증되지 않은 사용자는 읽기 전용 요청 만 수행 할 수 있습니다.
 

다음 권한을 게시물보기에 추가합니다.
 

```python
# code omitted for brevity
from rest_framework import permissions
from api.permissions import IsOwnerOrReadOnly

class PostList(generics.ListCreateAPIView):
    queryset = Post.objects.all()
    serializer_class = PostSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly]

    def perform_create(self, serializer):
        serializer.save(owner=self.request.user)

class PostDetail(generics.RetrieveUpdateDestroyAPIView):
    queryset = Post.objects.all()
    serializer_class = PostSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly,
                          IsOwnerOrReadOnly]

# code omitted for brevity
```

`PostList`보기에는 `IsAuthenticatedOrReadOnly`권한 만 필요합니다. 사용자가 게시물을 작성하려면 인증을 받아야하고 모든 사용자가 게시물 목록을 볼 수 있기 때문입니다.
 

게시물의 업데이트 및 삭제는 게시물의 소유자이기도 한 인증 된 사용자에게만 허용되어야하므로 `PostDetail`에는 두 가지 권한이 모두 필요합니다.
 단일 게시물 검색은 읽기 전용이며 권한이 필요하지 않습니다.
 

다시`http : //127.0.0.1 : 8000 / posts`로 이동합니다.
 `admin` 계정 및 기타 사용자 계정에 로그인하여 인증 된 사용자와 인증되지 않은 사용자가 수행 할 수있는 작업을 테스트합니다.
 

로그 아웃하면 게시물을 작성, 삭제 또는 업데이트 할 수 없습니다.
 한 사용자로 로그인 한 경우 다른 사용자가 소유 한 게시물을 삭제하거나 업데이트 할 수 없습니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/post-detail-auth.png?resize=730%2C283&ssl=1)

> Django REST Framework의 권한에 대한 자세한 내용은 권한을 참조하세요.
 

## 댓글 API 만들기
 

이제 기본 post API가 있습니다.
 이제 게시물에 댓글 시스템을 추가 할 수 있습니다.
 

댓글은 사용자가 게시물에 대한 응답으로 추가하고 개별 사용자에게 속한 텍스트입니다.
 사용자는 자신의 게시물을 포함하여 모든 게시물에 많은 댓글을 달 수 있으며 게시물에는 다른 사용자의 댓글이 많이있을 수 있습니다.
 즉, 댓글과 사용자 사이에 하나, 댓글과 게시물 사이에 하나씩 두 개의 다 대일 관계를 설정하게됩니다.
 

### 코멘트 모델
 

먼저`blog / api / models.py`에 댓글 모델을 만듭니다.
 

```python
# code omitted for brevity

class Comment(models.Model):
    created = models.DateTimeField(auto_now_add=True)
    body = models.TextField(blank=False)
    owner = models.ForeignKey('auth.User', related_name='comments', on_delete=models.CASCADE)
    post = models.ForeignKey('Post', related_name='comments', on_delete=models.CASCADE)

    class Meta:
        ordering = ['created']
```

`Comment` 모델은`Post` 모델과 유사하며`owner` 필드를 통해 사용자와 다 대일 관계를 갖습니다.
 또한 댓글은 `post`필드를 통해 단일 게시물과 다 대일 관계를 갖습니다.
 

이전과 같이 데이터베이스 마이그레이션을 실행하십시오.
 

```css
python manage.py makemigrations api
python manage.py migrate
```

### 코멘트 시리얼 라이저
 

댓글 API를 생성하려면 먼저 `PostSerializer`및 `UserSerializer`에 `Comment`모델을 추가하여 관련 댓글이 다른 게시물 및 사용자 데이터와 함께 전송되도록합니다.
 

이 코드를`blog / api / serializers.py`에 추가합니다.
 

```python
# code omitted for brevity
from api.models import Comment

class PostSerializer(serializers.ModelSerializer):
    owner = serializers.ReadOnlyField(source='owner.username')
    comments = serializers.PrimaryKeyRelatedField(many=True, read_only=True)

    class Meta:
        model = Post
        fields = ['id', 'title', 'body', 'owner', 'comments']

class UserSerializer(serializers.ModelSerializer):
    posts = serializers.PrimaryKeyRelatedField(many=True, read_only=True)
    comments = serializers.PrimaryKeyRelatedField(many=True, read_only=True)

    class Meta:
        model = User
        fields = ['id', 'username', 'posts', 'comments']
```

여기서 프로세스는`UserSerializer`에`posts`를 추가하는 것과 유사합니다.
 다시 말하지만 이것은 댓글과 사용자 간의 다 대일 관계, 댓글과 게시물 간의 다 대일 관계의 "다"부분을 설정합니다.
 댓글 목록은 다시 읽기 전용이어야합니다 (`read_only = True` 설정).
 

이제 동일한 파일에`CommentSerializer`를 추가합니다.
 

```python
class CommentSerializer(serializers.ModelSerializer):
    owner = serializers.ReadOnlyField(source='owner.username')

    class Meta:
        model = Comment
        fields = ['id', 'body', 'owner', 'post']
```

여기서는 `post`필드를 맞춤 설정할 필요가 없습니다.
 `post` 필드를`fields` 배열에 직접 추가하면`ModelSerializer`에 따라 기본 방식으로 직렬화됩니다.
 이것은`post = serializers.PrimaryKeyRelatedField (queryset = Post.objects.all ())`를 정의하는 것과 같습니다.
 

즉, `post`필드에는 기본적으로 쓰기 권한이 있습니다. 사용자가 새 댓글을 작성할 때 댓글이 속한 게시물도 설정합니다.
 

### 댓글보기
 

마지막으로 댓글에 대한 사용자 지정보기 및 URL 패턴을 만듭니다.
 이 프로세스는 `Post`API에서 따랐던 프로세스와 유사합니다.
 

이 코드를`blog / api / views.py`에 추가합니다.
 

```python
from api.models import Comment

class CommentList(generics.ListCreateAPIView):
    queryset = Comment.objects.all()
    serializer_class = serializers.CommentSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly]

    def perform_create(self, serializer):
        serializer.save(owner=self.request.user)

class CommentDetail(generics.RetrieveUpdateDestroyAPIView):
    queryset = Comment.objects.all()
    serializer_class = serializers.CommentSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly,
                          IsOwnerOrReadOnly]
```

이러한보기는 `PostList`및 `PostDetail`보기와 유사합니다.
 

### 댓글 URL 패턴
 

댓글 API를 완료하려면`blog / api / urls.py`에서 URL 패턴을 정의하세요.
 

```undefined
# code omitted for brevity

urlpatterns = [
    # code omitted for brevity
    path('comments/', views.CommentList.as_view()),
    path('comments/<int:pk>/', views.CommentDetail.as_view()),
]

urlpatterns = format_suffix_patterns(urlpatterns)
```

이제`http : //127.0.0.1 : 8000 / comments`로 이동하여 기존 댓글 목록을보고 새 댓글을 작성할 수 있습니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/comment-list.png?resize=730%2C395&ssl=1)

탐색 가능한 API에서 새 댓글을 작성할 때 기존 게시물 목록에서 게시물을 선택해야합니다.
 

## 카테고리 API 생성
 

블로그 API의 마지막 부분은 카테고리 시스템입니다.
 

게시물에 하나 이상의 카테고리를 추가 할 수 있습니다.
 게시물은 여러 카테고리를 가질 수 있고 카테고리는 여러 게시물에 속할 수 있으므로 다 대다 관계를 정의해야합니다.
 

### 카테고리 모델
 

`blog / api / models.py`에서`Category` 모델을 만듭니다.
 

```python
class Category(models.Model):
    name = models.CharField(max_length=100, blank=False, default='')
    owner = models.ForeignKey('auth.User', related_name='categories', on_delete=models.CASCADE)
    posts = models.ManyToManyField('Post', related_name='categories', blank=True)

    class Meta:
        verbose_name_plural = 'categories'
```

여기서 `ManyToManyField`클래스는 현재 모델과 첫 번째 인수에 표시된 모델 사이에 다 대다 관계를 생성합니다.
 `ForeignKey` 클래스와 마찬가지로이 관계는 serializer에 의해 완료됩니다.
 

`verbose_name_plural`은 Django 관리 사이트와 같은 장소에서 모델 이름을 복수화하는 방법을 결정합니다.
 이렇게하면 `category`를 `categorys`로 복수화하는 것을 방지하고 복수형을 `categories`로 수동 설정합니다.
 

이전과 같이 데이터베이스 마이그레이션을 실행하십시오.
 

```css
python manage.py makemigrations api
python manage.py migrate
```

### 카테고리 시리얼 라이저
 

범주 API를 만드는 프로세스는 이전 단계에서 수행 한 프로세스와 유사합니다.
 먼저 다음 코드를`blog / api / serializers.py`에 추가하여`Category` 모델에 대한 직렬 변환기를 만듭니다.
 

```python
# code omitted for brevity
from api.models import Category

class CategorySerializer(serializers.ModelSerializer):
    owner = serializers.ReadOnlyField(source='owner.username')
    posts = serializers.PrimaryKeyRelatedField(many=True, read_only=True)

    class Meta:
        model = Category
        fields = ['id', 'name', 'owner', 'posts']

class PostSerializer(serializers.ModelSerializer):
    owner = serializers.ReadOnlyField(source='owner.username')
    comments = serializers.PrimaryKeyRelatedField(many=True, read_only=True)

    class Meta:
        model = Post
        fields = ['id', 'title', 'body', 'owner', 'comments', 'categories']

class UserSerializer(serializers.ModelSerializer):
    posts = serializers.PrimaryKeyRelatedField(many=True, read_only=True)
    comments = serializers.PrimaryKeyRelatedField(many=True, read_only=True)
    categories = serializers.PrimaryKeyRelatedField(many=True, read_only=True)

    class Meta:
        model = User
        fields = ['id', 'username', 'posts', 'comments', 'categories']
```

`PostSerializer`및 `UserSerializer`의 필드 목록에 `categories`필드 이름을 추가해야합니다.
 `UserSerializer.categories`도`read_only = True`로 맞춤 설정해야합니다.
 이 필드는 사용자가 만든 모든 범주의보기 가능한 목록을 나타냅니다.
 

반면 `PostSerializer.categories`필드에는 기본적으로 쓰기 권한이 있습니다.
 기본값은`categories = serializers.PrimaryKeyRelatedField (many = True, queryset = Category.objects.all ())`설정과 동일합니다.
 이를 통해 사용자는 하나 이상의 기존 카테고리를 선택하여 새 게시물에 할당 할 수 있습니다.
 

### 카테고리보기
 

다음으로`blog / api / views.py`에서 카테고리 API에 대한보기를 만듭니다.
 

```python
# code omitted for brevity
from api.models import Category

class CategoryList(generics.ListCreateAPIView):
    queryset = Category.objects.all()
    serializer_class = serializers.CategorySerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly]

    def perform_create(self, serializer):
        serializer.save(owner=self.request.user)

class CategoryDetail(generics.RetrieveUpdateDestroyAPIView):
    queryset = Category.objects.all()
    serializer_class = serializers.PostSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly,
                          IsOwnerOrReadOnly]
```

이러한보기는 지금까지 만든 다른보기와 유사합니다.
 

### 카테고리 URL 패턴
 

마지막으로 카테고리 API를 완료하려면 다음 코드를`blog / api / urls.py`에 추가하십시오.
 

```undefined
# code omitted for brevity

urlpatterns = [
    # code omitted for brevity
    path('categories/', views.CategoryList.as_view()),
    path('categories/<int:pk>/', views.CategoryDetail.as_view()),
]

urlpatterns = format_suffix_patterns(urlpatterns)
```

이제`http : //127.0.0.1 : 8000 / categories`로 이동하여 하나 또는 두 개의 범주를 만들 수 있습니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/category-list.png?resize=730%2C348&ssl=1)

다음으로`http : //127.0.0.1 : 8000 / posts`로 이동하여 새 게시물을 만듭니다.
 게시물에 하나 이상의 카테고리를 추가 할 수 있습니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/create-post-with-categories.png?resize=730%2C262&ssl=1)

## 결론
 

축하합니다!
 이제 인증 기능이있는 블로그 API와 API 개발에서 가장 일반적인 많은 패턴이 있습니다.
 게시물, 댓글 및 카테고리를 검색, 작성, 업데이트 및 삭제하기위한 엔드 포인트를 작성했습니다.
 또한 이러한 리소스간에 다 대일 및 다 대다 관계를 추가했습니다.
 

API를 확장하거나 이에 대한 프런트 엔드 클라이언트를 만들려면 Django REST Framework 문서와 자습서 및 리소스를 참조하세요.
 