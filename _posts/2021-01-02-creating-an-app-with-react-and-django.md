---
layout: post
title: "반응 및 짱고: 앱 생성 가이드"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2019/12/unnamed-file.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/12/unnamed-file.png?fit=730%2C412&ssl=1)

편집자 참고: 이 튜토리얼은 2021년 1월 27일에 업데이트되었습니다.

Django는 이용 가능한 가장 완벽한 웹 개발 프레임워크 중 하나이다. 빠르고 안전하며 확장성이 뛰어납니다.

Python의 힘으로, 우리는 거의 순식간에 애플리케이션을 가동하고 실행할 수 있습니다.

데이터베이스에서 클라이언트로 전송된 최종 HTML까지 모든 것을 관리합니다.

그러나 단일 페이지 애플리케이션(SPA)의 출현으로, 가장 다양한 JavaScript 프레임워크에서 개발된 애플리케이션이 소비하는 JSON 데이터에 대응하는 API를 제공하기 위해 Django를 사용하는 애플리케이션을 만드는 것이 점점 더 일반화되고 있다.

이것은 사실 대부분의 언어들이 따르고 있는 추세입니다.

(전면과 백엔드를 분리하는) 이 아키텍처는 완전히 독립적으로 개발할 수 있는 팀과 함께 둘 모두를 더 잘 분리할 수 있게 한다.

또한 여러 클라이언트 앱이 동일한 API와 상호 작용하는 동시에 데이터 무결성 및 비즈니스 규칙, 다양한 사용자 인터페이스를 보장합니다.

반면, 두 개의 서로 다른 프로젝트는 훨씬 더 많은 작업을 생성합니다. 두 개의 개별 배포, 두 개의 서로 다른 환경을 구성할 수 있습니다.

이를 단순화하는 한 가지 방법은 Django 고유의 기능을 사용하여 정적 파일을 처리하는 것입니다. 결국 프런트 엔드 애플리케이션은 이 유형의 파일 세트에 지나지 않습니다.

이 기사에서는 CORS 공통 이슈에서 벗어난 Django와 유명한 Django REST Framework를 사용하여 간단한 CRUD API를 생성하고, 이를 React 앱과 통합하는 방법에 대해 개략적으로 설명하겠습니다. 설정 및 구성부터 프런트 엔드 구성 요소 및 백엔드 API의 사용자 지정에 이르기까지 모든 것을 다룹니다.

Django를 사용하면 다양한 방법으로 API를 노출할 수 있습니다. 그래프QL은 안전하지만 기존의 REST 끝점을 사용할 것입니다.

이 튜토리얼이 끝날 때쯤에는 최종 출력이 될 것입니다.

### Python 및 Django 설정

예를 들어 Python과 같은 기본 도구의 설치는 다루지 않습니다.

다음은 이 문서를 수행하기 전에 컴퓨터에 설정해야 하는 항목 목록입니다.

- Python 3(Linux를 사용하는 경우 이미 설치되어 있을 수 있습니다. *python3 -V* 명령을 실행하여 확인)
- Pip(기본 Python 패키지 설치 관리자)
- 노드JS(버전 6 이상) 및 npm(5.2+)

기사에서는 유용한 파이썬 기능인 venv도 활용하겠습니다.

Python Virtual Environment의 약자로, 개발자는 기본적으로 특정 Python 환경과 똑같이 작동하는 폴더를 만들 수 있습니다.

즉, 특정 패키지 및 모듈 또는 개인 라이브러리 버전을 추가하고 이를 로컬에서 서로 다른 Python 프로젝트 간에 혼합하지 않으려는 경우 venv를 사용하여 각 가상 환경에 대해 이를 생성하고 관리할 수 있습니다.

그럼 먼저 컴퓨터에 설치하는 것으로 시작하겠습니다. 다음 명령 실행(리눅스용):

```undefined
sudo apt install -y python3-venv
```

그런 다음 원하는 폴더로 이동하여 다음 폴더를 만듭니다.

```undefined
mkdir environments
```

이 폴더 내에서 명령을 실행하여 venv를 생성해 보겠습니다(항상 venv에 이름을 지정해야 함).

```undefined
python3 -m venv logrocket_env
```

생성된 폴더에 들어가면 Bin, lib, share 등의 다른 폴더가 표시되어 Python 구성의 분리된 컨텍스트에 있음을 보장할 수 있습니다.

하지만 사용하기 전에 활성화해야 합니다.

```bash
source logrocket_env/bin/activate
```

그러면 명령행은 다음과 같습니다. (괄호 안의 이름은 venv에 있다는 것을 확인하는 것입니다.)

```coffeescript
(logrocket_env) diogo@localhost: _
```

참고: venv 내부에서는 pip 또는 python 명령을 정상적으로 사용할 수 있습니다. 당신이 빠지려면 pip3와 python3을 먹어야 합니다.

바로 그거예요. 정맥주사랑 같이 가시는 게 좋아요

venv에서 다음 명령을 실행하여 Django의 설치로 이동하겠습니다.

```undefined
pip install django djangorestframework django-cors-headers
```

API에 대한 종속성을 두 개 더 설치하고 있습니다.

- Django REST Framework: 웹 API 구축을 위한 강력하고 유연한 툴킷
- django-cors-headers: 교차 오리진 리소스 공유(CORS)에 필요한 서버 헤더를 처리하기 위한 앱

다른 응용 프로그램에서 API에 액세스하려고 할 때 유용합니다(Ract). 짱고와 리액션을 연결하는 데 도움이 됩니다.

또한 다음 두 가지 Django의 기능을 사용하여 보일러 플레이트 구성을 지원합니다.

- django-admin Django의 자동 관리자 인터페이스입니다. 기본적으로 Django로 편리한 작업을 수행할 수 있는 명령줄 유틸리티입니다.
- manage.py: 모델, 마이그레이션 및 버전 관리뿐만 아니라 프로젝트의 적절한 생성까지 데이터베이스 관리에 도움이 되는 스크립트입니다.

이제 다음 명령을 실행하여 API 프로젝트를 만듭니다(venv 안에 있어야 함).

```undefined
django-admin startproject django_react_proj
```

프로젝트가 생성된 후 루트 폴더에 대한 관리를 확인하십시오.py 파일입니다. 나머지 파일은 더 자세히 살펴보겠습니다.

설정을 기준으로 구성을 시작하겠습니다.py는 `django_twin_proj/` 폴더에 있습니다.

열면 많은 구성이 표시되지만 `INSTALLED_`우리에게 중요한 것은 APPS입니다.

다음 세 줄을 배열에 추가합니다.

```undefined
INSTALLED_APPS = [
   ...
    'rest_framework',
    'corsheaders',
    'students'
]
```

이러한 종속성은 이전에 설치한 API 폴더 이름과 함께 생성됩니다.

이제 MINDWARE 어레이에 다음을 추가합니다.

```undefined
MIDDLEWARE = [
    ....
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.common.CommonMiddleware',
]
```

그들은 우리 애플리케이션의 모든 요청을 가로채서 그들에게 CORS 논리를 적용하는 필터에 대응한다.

그러나 전체 로컬 호스트를 작동하므로 다음 사항을 동일한 파일에 추가하여 CORS 기능을 사용하지 않도록 설정합니다.

```undefined
CORS_ORIGIN_ALLOW_ALL = True
```

좋습니다! 이제 애플리케이션의 모델과 뷰로 넘어갑시다.

미리 설정된 파일을 만들기 위해 manage를 사용합니다.다시 한 번 py의 대본 이번에는 다음을 실행합니다.

```css
python manage.py startapp students
```

이후 models.py, views.py과 함께 내용이 거의 없는 폴더 `content/`가 생성된다.

그럼 먼저 models.py 파일에 모델을 추가해 보겠습니다.

따라서 파일에서 모든 항목을 제거하고 다음과 같이 바꾸십시오.

```python
from django.db import models

class Student(models.Model):
    name = models.CharField("Name", max_length=240)
    email = models.EmailField()
    document = models.CharField("Document", max_length=20)
    phone = models.CharField(max_length=20)
    registrationDate = models.DateField("Registration Date", auto_now_add=True)

    def __str__(self):
        return self.name
```

저희 수업은 Django의 Model 수업에서 확장되었습니다.

이렇게 하면 데이터베이스 테이블을 만드는 데 사용할 Django 모델 프레임워크에 직접 연결하기만 하면 생활이 더 쉬워집니다.

또한 모든 필드 유형 및 구성(필요한 경우 최대 길이, 설명, 자동 작성 등)을 적절하게 설정하는 것이 중요합니다.

이제 마이그레이션 Django 기능을 통해 모델을 데이터베이스로 내보내 보겠습니다.

마이그레이션은 데이터베이스 스키마에 필드 추가, 모델 삭제 등 모델에 대한 변경 사항을 전파하는 Django의 방법입니다.

대부분 자동으로 설계되지만 마이그레이션 시기, 실행 시기 및 발생할 수 있는 일반적인 문제를 알아야 합니다.

응용 프로그램의 루트로 이동하여 다음을 실행합니다.

```css
python manage.py makemigrations
```

이러한 변경사항의 버전화를 위해 작성된 파일 이름과 파일 위치가 표시됩니다.

그런 다음 데이터베이스 자체에 변경 사항을 적용해야 합니다.

```css
python manage.py migrate
```

다음 단계는 데이터 마이그레이션 파일 생성으로 구성됩니다.

이것은 데이터베이스에 데이터를 직접 조작하는 것을 나타냅니다.

다음 명령을 실행합니다.

```css
python manage.py makemigrations --empty --name students students
```

그런 다음 두 번째 파일이 표시됩니다(주문을 유지하기 위해 파일 끝에 있는 번호에 따라 버전 관리를 수행함).

그런 다음 `django_react_proj/학생/마이그레이션/` 폴더로 이동하여 내용을 다음과 같이 변경합니다.

```python
from django.db import migrations

def create_data(apps, schema_editor):
    Student = apps.get_model('students', 'Student')
    Student(name="Joe Silver", email="joe@email.com", document="22342342", phone="00000000").save()

class Migration(migrations.Migration):

    dependencies = [
        ('students', '0001_initial'),
    ]

    operations = [
        migrations.RunPython(create_data),
    ]
```

간단히 말해, `create_data` 방법은 API가 시작될 때 데이터베이스가 비어 있지 않도록 Student 모델 개체를 복구하고 초기 데이터를 생성합니다.

종속성 속성은 다른 파일을 마이그레이션 프로세스로 간주합니다.

`작업`은 기본적으로 마이그레이션이 트리거되면 Django가 수행해야 하는 작업이다.

이제 마이그레이션 명령을 다시 실행할 준비가 되었습니다.

따라서 `django_react_proj/` 폴더에서 다음을 실행합니다.

```css
python manage.py migrate
```

### 짱고 REST API

이제 앞서 말씀드린 대로 Django REST Framework 위에 구축할 REST API에 대해 살펴보겠습니다.

여기에서 두 가지 주요 세계, 즉 뷰와 URL을 확인할 수 있습니다. 보기는 URL에서 제공하는 특정 엔드포인트에서 수행된 요청의 초기 진입점입니다.

기능 자체를 엔드포인트에 연결하면 Django REST Framework에 의해 매핑됩니다. 우리는 또한 연재기를 사용할 것이다.

쿼리 세트 및 모델 인스턴스와 같은 복잡한 데이터를 네이티브 파이썬 데이터 유형으로 변환하여 JSON으로 쉽게 렌더링할 수 있게 한다. 거기서부터 시작하자.

새 파일 `serializers.py`을 `discount/` 폴더에 생성하고 다음 내용을 추가하십시오.

```python
from rest_framework import serializers
from .models import Student

class StudentSerializer(serializers.ModelSerializer):

    class Meta:
        model = Student 
        fields = ('pk', 'name', 'email', 'document', 'phone', 'registrationDate')
```

메타 클래스는 모델이 가지고 있는 메타데이터 정보(데이터베이스)를 정의하고 이를 학생 클래스로 변환해야 하므로 여기서 중요합니다.

다음으로, `django_twin_proj/` 폴더에 있는 `urls.py` 파일을 열고 내용을 다음과 같이 변경해 보겠습니다.

```python
from django.contrib import admin
from django.urls import path, re_path
from students import views
from django.conf.urls import url

urlpatterns = [
    path('admin/', admin.site.urls),
    re_path(r'^api/students/$', views.students_list),
    re_path(r'^api/students/([0-9])$', views.students_detail),
]
```

`관리자`의 길은 이미 그곳에 있었다.

추가한 것은 학생 끝점뿐입니다.

각 기능은 생성될 보기 함수에 연결되어 있으므로 요청을 라우팅할 수 있습니다.

첫 번째 끝점은 작성(POST)과 목록(GET)을 모두 처리합니다.

두 번째 항목은 단일 학생의 데이터를 삭제(DELETE)하거나 업데이트(PUT)합니다. 간단하죠?

자, 이제 전망으로 가보겠습니다. `학생/뷰`를 엽니다.py` 파일 및 복사 코드는 다음과 같습니다.

```python
from rest_framework.response import Response
from rest_framework.decorators import api_view
from rest_framework import status

from .models import Student
from .serializers import *

@api_view(['GET', 'POST'])
def students_list(request):
    if request.method == 'GET':
        data = Student.objects.all()

        serializer = StudentSerializer(data, context={'request': request}, many=True)

        return Response(serializer.data)

    elif request.method == 'POST':
        serializer = StudentSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(status=status.HTTP_201_CREATED)
            
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

@api_view(['PUT', 'DELETE'])
def students_detail(request, pk):
    try:
        student = Student.objects.get(pk=pk)
    except Student.DoesNotExist:
        return Response(status=status.HTTP_404_NOT_FOUND)

    if request.method == 'PUT':
        serializer = StudentSerializer(student, data=request.data,context={'request': request})
        if serializer.is_valid():
            serializer.save()
            return Response(status=status.HTTP_204_NO_CONTENT)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    elif request.method == 'DELETE':
        student.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)
```

첫 번째 방법인 `student_list`는 API의 루트 끝점을 통해 GET 작업과 POST 작업을 모두 처리하는 것입니다.

즉, GET 및 POST HTTP 동사를 사용하여 http://localhost:8000/api/학생을 통해 요청을 할 때마다 이 방법을 실행합니다.

첫 번째는 우리 모델의 모든 학생들을 `학생`이라는 대상을 통해 끌어내는 것입니다.

전체 데이터베이스에 액세스할 수 있는 메서드와 함께 `개체`라는 암시적 개체를 제공합니다.

그런 다음 결과를 직렬화기에 전달하면 변환 프로세스가 처리되어 응답으로 반환됩니다.

POST 방법의 경우 먼저 직렬화기에서 `is_valid() 메서드를 호출하여 수신된 데이터가 우리의 모델과 일치하는지 확인합니다.

그렇지 않으면 직렬화기가 여기에 예외를 발생시킵니다. 문제가 없으면 데이터스토어에 저장합니다.

다음 PUT와 DELETE 작업은 HTTP 동사와 응답만 변경하면서 거의 동일합니다.

다 됐다! 더 이상 어쩔 수 없다!

이제 Django 애플리케이션을 실행하여 엔드포인트를 테스트해 보겠습니다. 루트 폴더에 다음 명령을 실행합니다.

```css
python manage.py runserver
```

서버가 실행 중임을 나타내는 로그를 확인한 후 브라우저로 이동하여 http://localhost:8000/api/students/에 액세스합니다. 다음과 같은 것을 볼 수 있습니다.

![image](https://blog.logrocket.com/wp-content/uploads/2019/12/student-list-nocdn.png)

여기 보시는 것은 Django의 Browseable API입니다. 리소스를 쉽게 탐색할 수 있는 인간 친화적인 HTML 출력과 리소스에 데이터를 제출하는 양식을 제공합니다.

cURL이나 다른 UI 도구를 사용하지 않고도 엔드포인트를 쉽게 테스트할 수 있습니다.

이미지 하단의 양식을 통해 다른 HTTP 메소드를 사용할 수도 있습니다. 어서 가지고 놀아라.

### 리액트 앱 구축

이제 프런트 엔딩 시간입니다.

여기서는 Relative 세부 사항에 대해 자세히 설명하지 않습니다(초보자일 경우 LogRocket의 블로그에 관련 기사가 많이 있습니다).

이 글의 초점은 리액트 앱에서 짱고 API를 빠르게 소비하는 방법을 보여주는 것이다.

이 기사에서는 최신 버전의 React를 사용할 것입니다.

그러나 원하는 버전을 자유롭게 사용하십시오. 또한 API 사용 자체가 목적이기 때문에 Hooks나 React의 다른 측면 기능에 대해서는 논의하지 않겠습니다.

Node와 npm이 설치되면 Django 프로젝트의 루트 폴더에서 다음 명령을 실행하여 React 앱을 생성해 보겠습니다.

```undefined
npx create-react-app students-fe
```

`생성-반응-앱`을 모르면 여기로 가보는 게 좋을 것 같아요.

다음 그림과 같이 프런트 엔드를 몇 가지 작은 구성 요소로 나눕니다.

헤더는 헤더 정보, 로고 등을 저장합니다.

홈은 우리의 주요 컨테이너가 될 것입니다. 다른 구성 요소들, 예를 들어, 테이블에 학생들의 목록 같은 것들을 저장할 것입니다.

또한 양식을 위한 두 가지 구성 요소가 더 있습니다. 업데이트/추가 양식은 거의 동일한 구성 요소가 됩니다. 두 가지 기능은 현재 어떤 기능이 활성화되었는지에 따라 달라집니다(모달에 배치됨).

바로 시작합시다.

우선, 우리의 `학생-페` 프로젝트에 몇 가지 중요한 의존성을 더하여, 프로젝트에 `cd`를 추가하고 다음을 실행해 봅시다.

```undefined
npm install bootstrap reactstrap axios --save
```

왜냐하면 우리는 스타일링을 위해 Bootboot를 사용할 것이고, Retactstrap은 준비된 Bootboot 내장 부품을 사용하기 쉽기 때문에 매우 강력한 방법이기 때문입니다.

Axios는 Django API에 대한 HTTP 요청 호출에 사용할 약속 기반 HTTP 클라이언트입니다.

먼저 In`src/` 폴더에서 `constant`라는 다른 폴더를 생성한 다음 `index.js` 파일을 생성합니다.

반응 프로젝트의 유틸리티 상수를 저장합니다. API의 URL을 유지하기 위해 단일 상수를 추가합니다.

```cpp
export const API_URL = "http://localhost:8000/api/students/";
```

그런 다음 머리글부터 구성 요소 생성으로 이동하겠습니다.

구성 요소라는 다른 폴더를 만든 다음, JavaScript 파일인 `Header.js`를 만듭니다. 다음 내용을 추가하십시오.

```js
import React, { Component } from "react";

class Header extends Component {
  render() {
    return (
      <div className="text-center">
        <img
          src="https://logrocket-assets.io/img/logo.png"
          width="300"
          className="img-thumbnail"
          style={ marginTop: "20px" }
        />
        <hr />
        <h5>
          <i>presents</i>
        </h5>
        <h1>App with React + Django</h1>
      </div>
    );
  }
}

export default Header;
```

이것은 JSX에 의해 표현되는 정적 HTML이다. 여기선 별로 주목할 게 없어요.

다음으로 전략을 바꾸고 가장 내부적인 요소에서 외부 구성 요소로 다음 구성 요소를 구축하겠습니다.

동일한 폴더에 새 파일 `NewStudentForm.js`를 생성하고 다음을 추가하십시오.

```coffeescript
import React from "react";
import { Button, Form, FormGroup, Input, Label } from "reactstrap";

import axios from "axios";

import { API_URL } from "../constants";

class NewStudentForm extends React.Component {
  state = {
    pk: 0,
    name: "",
    email: "",
    document: "",
    phone: ""
  };

  componentDidMount() {
    if (this.props.student) {
      const { pk, name, document, email, phone } = this.props.student;
      this.setState({ pk, name, document, email, phone });
    }
  }

  onChange = e => {
    this.setState({ [e.target.name]: e.target.value });
  };

  createStudent = e => {
    e.preventDefault();
    axios.post(API_URL, this.state).then(() => {
      this.props.resetState();
      this.props.toggle();
    });
  };

  editStudent = e => {
    e.preventDefault();
    axios.put(API_URL + this.state.pk, this.state).then(() => {
      this.props.resetState();
      this.props.toggle();
    });
  };

  defaultIfEmpty = value => {
    return value === "" ? "" : value;
  };

  render() {
    return (
      <Form onSubmit={this.props.student ? this.editStudent : this.createStudent}>
        <FormGroup>
          <Label for="name">Name:</Label>
          <Input
            type="text"
            name="name"
            onChange={this.onChange}
            value={this.defaultIfEmpty(this.state.name)}
          />
        </FormGroup>
        <FormGroup>
          <Label for="email">Email:</Label>
          <Input
            type="email"
            name="email"
            onChange={this.onChange}
            value={this.defaultIfEmpty(this.state.email)}
          />
        </FormGroup>
        <FormGroup>
          <Label for="document">Document:</Label>
          <Input
            type="text"
            name="document"
            onChange={this.onChange}
            value={this.defaultIfEmpty(this.state.document)}
          />
        </FormGroup>
        <FormGroup>
          <Label for="phone">Phone:</Label>
          <Input
            type="text"
            name="phone"
            onChange={this.onChange}
            value={this.defaultIfEmpty(this.state.phone)}
          />
        </FormGroup>
        <Button>Send</Button>
      </Form>
    );
  }
}

export default NewStudentForm;
```

여기서는 몇 가지 중요한 작업이 진행 중입니다.

- 첫 번째 줄에서는 양식을 구성하는 양식, 버튼 등을 포함하여 처음으로 리액트 스트랩 구성 요소를 가져옵니다.
- 그런 다음, 우리는 Student 모델의 해당 속성을 가진 `state` 객체를 만들었습니다. 이것은 각각의 소품을 개별적으로 조작하는 데 유용할 것입니다.
- 구성 요소 DidMount 기능은 구성 요소가 시작된 후 실행되므로 여기서 상위 구성 요소(`this.props`)에서 학생의 소품을 복구하고 편집 시나리오를 위해 상태를 설정할 수 있습니다.
- `OnChange` 기능은 각 필드에 입력된 현재 값으로 각 상태 소프의 업데이트를 처리합니다.
- `create Student` 기능은 우리 양식의 HTTP POST 요청을 처리합니다. 제출 버튼을 누를 때마다 이 기능이 호출되어 축의 post() 기능이 트리거되고 요청 본문에 현재 상태가 전달됩니다.일단 완료되면, 우리는 `props` 기능을 `resetState`(테이블을 새로 고치기 위해)와 `toggle`(모달 닫기 위해)이라고 부를 것이다.
- 학생 편집 기능은 이전과 거의 비슷하게 작동하지만, 우리의 PUT 작전에 전화를 건다.
- defaultIf Empty` 함수는 각 필드의 현재 값을 확인하여 상태 값(있는 경우 편집용)으로 채워질지 여부를 결정하는 보조 함수로 생성되었습니다(새 학생 생성 시).
- "렌더" 기능은 리액트랩 구성부품의 도움으로 우리의 형태를 구성하게 될 것이다. 여기서는 학생이라는 소품 속성을 확인하는 onSubmit 속성 외에는 특별한 것이 없습니다. 있는 경우 제출 기능은 편집을 위한 것이며(상위 구성 요소가 전달한 값) 그렇지 않은 경우 생성을 위한 것입니다.

다음으로, 방금 만든 양식이 들어 있는 모달 구성 요소에 주의를 기울이겠습니다.

이를 위해 `NewStudentModal.js`라는 새 구성 요소 파일을 생성하고 아래 코드를 추가하십시오.

```js
import React, { Component, Fragment } from "react";
import { Button, Modal, ModalHeader, ModalBody } from "reactstrap";
import NewStudentForm from "./NewStudentForm";

class NewStudentModal extends Component {
  state = {
    modal: false
  };

  toggle = () => {
    this.setState(previous => ({
      modal: !previous.modal
    }));
  };

  render() {
    const create = this.props.create;

    var title = "Editing Student";
    var button = <Button onClick={this.toggle}>Edit</Button>;
    if (create) {
      title = "Creating New Student";

      button = (
        <Button
          color="primary"
          className="float-right"
          onClick={this.toggle}
          style={ minWidth: "200px" }
        >
          Create New
        </Button>
      );
    }

    return (
      <Fragment>
        {button}
        <Modal isOpen={this.state.modal} toggle={this.toggle}>
          <ModalHeader toggle={this.toggle}>{title}</ModalHeader>

          <ModalBody>
            <NewStudentForm
              resetState={this.props.resetState}
              toggle={this.toggle}
              student={this.props.student}
            />
          </ModalBody>
        </Modal>
      </Fragment>
    );
  }
}

export default NewStudentModal;
```

이번에는, 우리가 만드는 유일한 상태 소품이 모달의 상태이며, 모달의 상태를 확인하여 모달의 상태를 열어야 하는지 닫아야 하는지 여부를 확인합니다.

toggle 함수(우리 양식이 매개 변수로 받는 함수)는 호출될 때마다 현재 모달 값을 반대로 전환합니다.

`렌더` 기능에서는 먼저 상위 호출자로부터 `만들기` 부울이 매개 변수로 전달되었는지 확인하여 버튼이 편집 또는 작업 생성용인지 여부를 결정합니다.

버튼은 부모가 우리에게 한 말에 따라 동적으로 생성됩니다.

그런 다음 이러한 조건에 `모달` 구성 요소를 추가로 장착할 수 있습니다. 방금 만든 <새 학생 양식/> 구성 요소를 어디에 배치하고 있는지 주목해 주십시오.

`새 학생 모델` 구성 요소는 지금 생성하려는 `StudentList.js`에 배치됩니다.

```coffeescript
import React, { Component } from "react";
import { Table } from "reactstrap";
import NewStudentModal from "./NewStudentModal";

import ConfirmRemovalModal from "./ConfirmRemovalModal";

class StudentList extends Component {
  render() {
    const students = this.props.students;
    return (
      <Table dark>
        <thead>
          <tr>
            <th>Name</th>
            <th>Email</th>
            <th>Document</th>
            <th>Phone</th>
            <th>Registration</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          {!students || students.length <= 0 ? (
            <tr>
              <td colSpan="6" align="center">
                <b>Ops, no one here yet</b>
              </td>
            </tr>
          ) : (
            students.map(student => (
              <tr key={student.pk}>
                <td>{student.name}</td>
                <td>{student.email}</td>
                <td>{student.document}</td>
                <td>{student.phone}</td>
                <td>{student.registrationDate}</td>
                <td align="center">
                  <NewStudentModal
                    create={false}
                    student={student}
                    resetState={this.props.resetState}
                  />
                  &nbsp;&nbsp;
                  <ConfirmRemovalModal
                    pk={student.pk}
                    resetState={this.props.resetState}
                  />
                </td>
              </tr>
            ))
          )}
        </tbody>
      </Table>
    );
  }
}

export default StudentList;
```

여기서, 초점은 명시적으로 학생들의 목록이고 다른 것은 아무것도 아니다.

여기에 속하지 않는 다른 논리와 규칙이 섞이지 않도록 조심해라.

이 구성 요소의 핵심은 상위 구성 요소(홈)로부터 받게 될 학생 소품(`홈`)에 대한 반복입니다.

지도 기능은 각 값에 액세스할 수 있는 변수(`학생`)를 제공하여 반복을 관리합니다.

다시 한 번, 마지막 <td> 아래에 놓인 `신입생모형`과 `제거모형 확인` 구성 요소를 살펴보자.

다음은 제거 모델 확인 구성 요소의 내용입니다.

```js
import React, { Component, Fragment } from "react";
import { Modal, ModalHeader, Button, ModalFooter } from "reactstrap";

import axios from "axios";

import { API_URL } from "../constants";

class ConfirmRemovalModal extends Component {
  state = {
    modal: false
  };

  toggle = () => {
    this.setState(previous => ({
      modal: !previous.modal
    }));
  };

  deleteStudent = pk => {
    axios.delete(API_URL + pk).then(() => {
      this.props.resetState();
      this.toggle();
    });
  };

  render() {
    return (
      <Fragment>
        <Button color="danger" onClick={() => this.toggle()}>
          Remove
        </Button>
        <Modal isOpen={this.state.modal} toggle={this.toggle}>
          <ModalHeader toggle={this.toggle}>
            Do you really wanna delete the student?
          </ModalHeader>

          <ModalFooter>
            <Button type="button" onClick={() => this.toggle()}>
              Cancel
            </Button>
            <Button
              type="button"
              color="primary"
              onClick={() => this.deleteStudent(this.props.pk)}
            >
              Yes
            </Button>
          </ModalFooter>
        </Modal>
      </Fragment>
    );
  }
}

export default ConfirmRemovalModal;
```

이것은 매우 간단합니다. 제거 작업을 주관합니다.

이를 DELETE 끝점이라고 합니다.

이 또한 모달이기 때문에 우리는 국가의 `모달` 소품뿐만 아니라 `토글` 기능도 가지고 있어야 한다.

학생 삭제 기능은 지정된 학생을 삭제하기 위한 HTTP 호출을 처리합니다. 나머지 코드는 우리가 이미 본 것과 매우 유사하다.

이제 `Home.js` 구성 요소를 구축하겠습니다. 파일을 만들고 다음 파일을 추가합니다.

```coffeescript
import React, { Component } from "react";
import { Col, Container, Row } from "reactstrap";
import StudentList from "./StudentList";
import NewStudentModal from "./NewStudentModal";

import axios from "axios";

import { API_URL } from "../constants";

class Home extends Component {
  state = {
    students: []
  };

  componentDidMount() {
    this.resetState();
  }

  getStudents = () => {
    axios.get(API_URL).then(res => this.setState({ students: res.data }));
  };

  resetState = () => {
    this.getStudents();
  };

  render() {
    return (
      <Container style={ marginTop: "20px" }>
        <Row>
          <Col>
            <StudentList
              students={this.state.students}
              resetState={this.resetState}
            />
          </Col>
        </Row>
        <Row>
          <Col>
            <NewStudentModal create={true} resetState={this.resetState} />
          </Col>
        </Row>
      </Container>
    );
  }
}

export default Home;
```

여기서, 우리의 `주`는 서버에서 복구될 `학생`의 배열을 주최할 것이다.

리셋스테이트 기능(앞서 전화드렸음)은 getStudents라고 부르는데, getStudents는 API에서 GET 끝점에 전체 학생 목록을 호출한다.

나머지 리스트는 학생목록과 신학생모형 구성품을 사용한 것이다.

자유롭게 구성품 전시를 직접 준비하세요.

바로 이거예요. 앱을 테스트하기 전에 마지막으로 할 수 있는 거예요.

`Header` 및 `Home` 구성 요소를 `App.js` 파일로 가져옵니다.

```js
import React, { Component, Fragment } from "react";
import Header from "./components/Header";
import Home from "./components/Home";

class App extends Component {
  render() {
    return (
      <Fragment>
        <Header />
        <Home />
      </Fragment>
    );
  }
}

export default App;
```

이제 `npm start` 명령을 실행하면 Retact 앱이 http://localhost:3000/url에서 브라우저를 엽니다. Django API도 반드시 가동하고 실행하세요.

### 결론

여기서 이 프로젝트의 전체 소스 코드에 액세스할 수 있습니다.

물론, 이것은 단지 한 가지 방법일 뿐입니다.

React를 사용하면 여러 가지 방법으로 구성 요소를 구성할 수도 있고, 여러 가지 방법으로 구성 요소를 만들어 동일한 목표를 달성할 수도 있습니다.

SPA의 세계에서는 백엔드 API가 사실상 프런트엔드 클라이언트와 완전히 독립적입니다.

이렇게 하면 리액트 앱에 대한 부작용 없이 API의 전체 아키텍처(예: Django에서 Plask로 전환)를 유연하게 변경할 수 있습니다.

과제로, API/Ract 앱에 페이지 지정 시스템을 추가해 보십시오.

Django REST Framework는 사용자 지정 가능한 페이지 지정 스타일을 지원하며 React 다양한 libs도 지원합니다.