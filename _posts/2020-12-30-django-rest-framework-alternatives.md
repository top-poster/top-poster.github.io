---
layout: post
title: "Django REST 프레임워크 대안"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/django-rest-framework-alternatives.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/django-rest-framework-alternatives.png?fit=730%2C487&ssl=1)

이 튜토리얼에서는 웹 API 구축을 위한 Django REST 프레임워크에 대한 몇 가지 대안을 소개합니다. HTTP 요청을 사용하여 데이터에 액세스하고 사용하는 RESTful API를 사용하여 애플리케이션 확장을 지원하는 세 가지 라이브러리를 중점적으로 살펴보겠습니다. 짱고 맛있는 파이, 안절부절못하는 짱고 JSON 뷰.

다음 사항에 대해 자세히 설명합니다.

- 짱고 뭐야?
- REST API란?
- Django REST 프레임워크란?
- Django REST 프레임워크를 사용해야 합니까?
- 짱고맛파이
- 짱고 안절부절못함
- 짱고 짱고 짱고 짱구경

## 짱고 뭐야?

짱고(Django)는 파이썬 기반의 자유 오픈 소스 웹 프레임워크로, 모델 템플릿 뷰 아키텍처 패턴을 따른다. 웹 개발로 인한 번거로움을 줄여주므로 바퀴를 다시 만드는 대신 앱 작성에 집중할 수 있습니다.

## REST API란?

REST API는 시스템이 유용한 기능과 데이터를 노출하는 인기 있는 방법입니다. 대표 상태 전송을 의미하는 REST는 지정된 URL에서 액세스할 수 있는 하나 이상의 리소스로 구성될 수 있으며 JSON, 이미지, HTML 등과 같은 다양한 형식으로 반환될 수 있다.

RESTful API는 HTTP 요청을 사용하여 데이터에 액세스합니다. 이 데이터는 GET, PUT, POST, DELETE 데이터 유형에 사용될 수 있으며, 이는 리소스 관련 작업을 읽고 업데이트하고 생성하고 삭제하는 것을 의미한다. 이러한 작업을 CRUD 작업이라고 합니다. REST API의 데이터 형식에는 응용 프로그램, JSON 응용 프로그램, XML 등이 포함될 수 있습니다.

## Django REST 프레임워크란?

DFR(Django REST Framework)은 웹 API를 구축하기 위한 강력하고 유연한 툴킷이다. 그것의 주된 이점은 그것이 연재화를 훨씬 더 쉽게 만든다는 것이다.

Django REST 프레임워크는 Django의 클래스 기반 뷰를 기반으로 하기 때문에 Django에 대해 잘 알고 있다면 훌륭한 옵션입니다. 클래스 기반 보기, 양식, 모델 검증자, QuerySet 등과 같은 구현을 채택한다.

## Django REST 프레임워크를 사용해야 합니까?

Django 원칙이 익숙하지 않다면 새로운 웹 프레임워크를 배우기 전에 다른 옵션을 살펴보는 것이 좋습니다. 이 가이드에서는 Django REST 프레임워크에 대한 최선의 대안 방법을 검토합니다.

증명하기 위해, 저는 제 친구들의 이름과 나이를 저장할 수 있는 작고 기본적인 어플리케이션을 만들었습니다. RESTful API 프레임워크를 이 애플리케이션과 통합할 것입니다.

GitHub에서 데모 Django 애플리케이션의 코드를 복제할 수 있습니다. 코드 조각은 기존 프로젝트에서도 원활하게 작동합니다.

## Django REST 프레임워크 대안

대표적인 짱고 REST 프레임워크 대안으로는 짱고맛파이와 짱고맛파이와 짱고맛손뷰가 있다. 우리는 각각 자세히 살펴볼 것이다.

### 짱고맛파이

짱고 테이스티파이(Django Tastypie)는 짱고를 위한 웹 서비스 API 프레임워크로, REST 스타일의 인터페이스를 만들 수 있는 편리하면서도 강력하며 사용자 정의가 가능한 추상화를 제공한다. 다음과 같은 경우 완벽한 솔루션입니다.

- RESTful이고 HTTP를 잘 사용하는 API 필요
- 깊은 관계를 지원하기를 원합니다.
- 출력을 올바르게 만들기 위해 직접 직렬화기를 작성하지 마십시오.
- 마법이 거의 없고 유연하며 문제 영역에 잘 매핑되는 API 프레임워크를 원합니다.
- JSON과 동일하게 처리되는 XML 직렬화 필요(그리고 YAML도 있음)

Django Tastypie를 설치하려면:

```shell
$ pip install django-tastypie
```

짱고 테이스티파이 코드 샘플은 다음과 같습니다.

```python
# api/tastypie_resources.py
from tastypie.resources import ModelResource
from .models import Friend

class FriendResource(ModelResource):
    class Meta:
        queryset = Friend.objects.all()


# urls.py
from django.urls import path, include

# Tastypie
from tastypie.api import Api
from api.tastypie_resources import FriendResource
v1_api = Api(api_name='friends')
v1_api.register(FriendResource())

urlpatterns = [
    path(r'api/v1/', include(v1_api.urls)),
]
```

API에서 JSON 응답을 보려면 localhost를 방문하십시오.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/json-response-api.png?resize=422%2C417&ssl=1)

### 짱고 안절부절못함

Django Nestless는 Python을 위한 경량 REST 미니 프레임워크이다. 짱고, 플라스크, 피라미드, 토네이도와 함께 잘 작동하며 다른 많은 파이썬 웹 프레임워크에 유용하다. Neatless는 작고 유연하며 빠른 코드베이스를 가지며, 기본 출력은 JSON입니다.

Django Nestress는 REST 프레임워크에 대한 새로운 정보를 제공합니다. 다른 프레임워크는 매우 완벽하거나, 특수 기능을 포함하거나 ORM과 깊이 연결하려고 시도하지만, Nestless는 기본 사항에 초점을 맞춥니다.

Django를 설치하려면:

```shell
$ pip install restless
```

다음은 Django Nestless 코드 샘플입니다.

```python
# api/restless_resources.py
from restless.dj import DjangoResource
from restless.preparers import FieldsPreparer
from .models import Friend

class FriendResource(DjangoResource):
    preparer = FieldsPreparer(fields={
        'id': 'id',
        'age': 'age',
        'name': 'name',
    })
    # GET /api/v2/friends/
    def list(self):
        return Friend.objects.all()
    # GET /api/v2/friends/<pk>/ 
    def detail(self, pk):
        return Friend.objects.get(id=pk)

# urls.py
from django.urls import path, include

# Restless
from api.restless_resources import FriendResource

urlpatterns = [
    path(r'api/v2/friends/', include(FriendResource.urls())),
]
```

로컬 호스트에서 JSON 응답을 미리 볼 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/preview-json-response.png?resize=420%2C268&ssl=1)

### 짱고존관

짱고-jsonview는 파이썬 객체를 JSON으로 변환하여 뷰가 항상 JSON을 반환하도록 하는 간단한 장식기이다.

짱고json view는 보기 방법에 데코레이터만 추가하면 되고 JSON이 반환됩니다.

django-jsonview를 설치하려면:

```shell
$ pip install django-jsonview
```

다음은 `짱고-jsonview` 코드의 예입니다.

```python
# api/views.py

from jsonview.decorators import json_view

@json_view
def my_view(request):
    return {
        'foo': 'bar',
    }

# urls.py
from django.urls import path, include

# JSON View
from api.views import my_api_view
urlpatterns = [
    path(r'api/v3/friends/', my_api_view)
]
```

로컬 호스트에서 API 응답을 미리 볼 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/preview-api-response.png?resize=404%2C102&ssl=1)

## 결론

이 기사에서는 Django를 사용하여 RESTful API를 구축하기 위한 세 가지 솔루션을 살펴 보았다.

API 및 REST API 구축을 위한 Django 패키지에 대한 자세한 내용은 Django 문서를 참조하십시오.