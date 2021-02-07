---
layout: post
title: "래러벨 대 레이브 AdonisJs: 당신은 어떤 것을 사용해야 합니까?"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/adonis-laravel.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/adonis-laravel.png?fit=730%2C487&ssl=1)

인기 있는 MVC(모델 뷰 컨트롤러) 기반 프레임워크인 Laravel은 웹 응용 프로그램을 만드는 데 필요한 모든 것과 모든 것을 갖추고 있습니다. 인증, 라우팅, 데이터베이스 마이그레이션, 모델, 도우미 등에 대해 자세히 설명합니다. 그러나 이는 소규모 또는 단순 애플리케이션의 경우 과도한 작업일 수 있습니다.

한편, AdonisJs는 Node.js 위에 구축된 MVC 기반 프레임워크이기도 하다. 또한, 개발자의 인체공학, 안정성 및 신뢰성에 매우 중점을 두고 있습니다. 그것은 라라벨과 같은 개념에 바탕을 두고 있다. Laravel에서 영감을 받아 Provider 및 Dependency Injection과 같은 장점을 통해 코드를 보다 유연하게 구성하고 구성할 수 있습니다.

이 기사에서는 라라벨과 아도니스Js를 비교할 것이다. 우리는 각 프레임워크의 설정, 보기, ORM, 라우팅, 데이터베이스, CLI, IOC 및 서비스 공급자, 파일 시스템, 커뮤니티 강점 및 성능을 분석할 것입니다.

## 세우다

래벨 설치는 쉽습니다. 개발 환경인 홈스테드를 사용하면 모든 요구사항이 해결됩니다. 그러나 홈스테드를 사용하지 않으려면 서버가 몇 가지 요구 사항을 지원하는지 확인해야 합니다. 여기에는 Tokenizer PHP Extension, PDO PHP Extension, JSON PHP Extension 등이 포함된다.

`larve new`는 지정한 디렉터리에 새로 설치됩니다.

```coffeescript
laravel new project
```

또는 Composer Create-Project를 통해 새 설치를 생성할 수 있습니다.

```undefined
composer create-project --prefer-dist laravel/laravel project
```

라벨은 자유로운 구성이 가능하고 프로젝트 구조를 강제하지 않아 유연하다. 설치가 완료되면 다음 명령을 사용하여 응용 프로그램을 실행할 수 있습니다.

```undefined
php artisan serve
```

아도니스는 설치하기도 쉽다. 설치는 Adonis 명령을 설치 또는 실행하는 데 도움이 되는 도구인 Adonis CLI를 통해 수행됩니다. 먼저 CLI를 전체적으로 설치해야 합니다.

```coffeescript
npm i -g @adonis/cli
```

설치가 완료되면 `adonis new` 명령을 사용하여 지정한 디렉토리에 새 설치를 수행할 수 있습니다.

```coffeescript
adonis new project
```

또 다른 대안으로는 Git을 통해 Github에서 리포트를 복제하는 것과 관련이 있습니다. API 전용, 풀 스택 및 슬림 보일러 플레이트입니다.

설치가 완료되면 응용 프로그램을 시작하고 다음을 개발합니다.

```undefined
adonis serve --dev
```

### 보기

Laravel은 Blade라고 불리는 강력한 템플릿 엔진을 가지고 있다. 그것은 Twig와 꽤 비슷하다. 실제 거래는 보기 내에서 직접 PHP 코드를 사용하는 데서 비롯됩니다. 즉, 다음과 같은 요청이 있을 때 서버에서 PHP 코드로 컴파일된 블레이드 보기가 처리됩니다.

```xml
<!-- Stored in resources/views/layouts/app.blade.php -->

<html>
    <head>
        <title>App Name - @yield('title')</title>
    </head>
    <body>
        @section('sidebar')
            This is the master sidebar.
        @show

        <div class="container">
            @yield('content')
        </div>
    </body>
</html>
```

아도니스는 매우 빠른 에지 템플릿 엔진을 탑재하고 있다. Chrome DevTools에서 런타임 디버깅, 논리 태그가 있는 텍스트 중요도에 대한 물리적 태그, 레이아웃 및 부분, 구성요소 스타일을 지원합니다. Blade와 마찬가지로 보기 내에 JavaScript를 작성할 수도 있습니다.

```coffeescript
@if(hours < 12)
  <h2> Good Morning </h2>
@elseif(hours < 18)
  <h2> Good Afternoon </h2>
@else
  <h2> Good Evening </h2>
@endif
```

### ORM

라라벨은 액티브 레코드 패턴을 관찰하는 웅변 ORM과 함께 나온다. 이것은 일반 데이터베이스 쿼리와 달리 응용 프로그램의 데이터 흐름을 촉진하기 위한 효율적이고 강력한 API를 제공합니다. 애플리케이션의 데이터베이스 구성으로 사랑 받고 있습니다. 또한 가장 널리 사용되는 데이터베이스(MySQL, Postgre, SQLite 등)도 지원합니다.

데이터베이스 테이블과 통신하는 웅변 모델을 작성합니다. 모델을 사용하면 삽입 또는 쿼리별로 테이블과 상호 작용할 수 있습니다.

우리는 `장인` 명령을 사용하여 모델을 더 빨리 만듭니다.

```css
php artisan make:model User
```

모형에서 규약 등을 설정할 수 있습니다.

사용자 생성은 다음과 같이 작동합니다.

```xml
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use App\Models\User;
use Illuminate\Http\Request;

class UserController extends Controller
{
    /**
     * Create a new user instance.
     *
     * @param  Request  $request
     * @return Response
     */
    public function store(Request $request)
    {
        // Validate the request...

        $user = new User;

        $user->name = $request->name;


        $user->username = $request->username;


        $user->email = $request->email;


        $user->password = $request->password;

        $user->save();
    }
}
```

한 가지 아름다운 점은 관계를 관리하는 API입니다.

```php
return $this->hasOne('App\Models\Phone', 'foreign_key');
```

아도니스는 활성 레코드 패턴도 사용하는 Lucid ORM과 함께 제공됩니다. 날짜 형식 관리, 암호 해싱, 데이터 직렬화, 데이터 변환을 위한 getter/setter 등의 작업을 위한 라이프사이클 후크가 함께 제공됩니다.

모델을 만드는 것은 웅변 ORM과 거의 같습니다. 필요한 경우 다음과 같은 규칙도 유용합니다.

```css
adonis make:model User
```

```js
'use strict'

const Model = use('Model')

class User extends Model {
}

module.exports = User
```

사용자 생성은 웅변 ORM과 다를 바 없습니다.

```undefined
const User = use('App/Models/User')

const user = new User()

user.username = 'virk'
user.password = 'some-password'

await user.save()
```

또한 관계를 관리하기 위한 표현형 API를 가지고 있다.

```undefined
hasOne(relatedModel, primaryKey, foreignKey)
```

### 라우팅

Laravel과 마찬가지로 Adonis도 기본 경로 파일을 URL 및 Closure로 받아들이는데, 이는 경로를 정의하는 매우 간단한 방법입니다.

```coffeescript
Route.get('posts/:id', ({ params }) => {
  return `Hello  ${params.id}`
})
```

라우팅을 사용하도록 설정하는 다른 방법은 라우트 파일의 엔드포인트에 컨트롤러를 연결하는 것입니다. 라우트 파일은 프로젝트의 루트 폴더에 저장되는 Laravel과 달리 시작 폴더 아래에 있습니다.

라라벨

```undefined
Route::get('/user', [UserController::class, 'index']);
```

아도니스 Js

```undefined
Route.get('users', 'UserController.index').as('users.index')
```

또한 두 프레임워크 모두 엔드포인트를 그룹화하고 엔드포인트에 연결된 HTTP 동사를 할당합니다.

라라벨

```php
Route::prefix('admin')->group(function () {
    Route::get('users', function () {
        // Matches The "/admin/users" URL
    });
});
```

아도니스 Js

```undefined
Route.group(() => {
  Route.get('users', closure)   // GET /api/v1/users
  Route.post('users', closure)  // POST /api/v1/users
}).prefix('api/v1')
```

### 데이터베이스

현재 Laravel은 4개의 데이터베이스를 지원합니다. MySQL, PostgreSQL, SQLite 및 SQL Server. 데이터베이스에 대한 모든 구성은 `config/database.php`에 있습니다. 기본 데이터베이스 연결을 포함하여 이 파일에 정의되어 있습니다.

버전 제어와 같은 마이그레이션은 Laravel에서 지원됩니다. 이 규칙을 사용하면 데이터 모델 변경 내용을 쓸 수 있습니다.

Laravel과 마찬가지로 AdonisJs도 마이그레이션과 시드를 지원합니다. 시드는 데이터를 사용하여 테이블을 테스트하는 데 도움이 됩니다.

AdonisJs는 MySQL, Postgre 등 6개의 데이터베이스를 지원합니다.SQL, MariaDB, SQLite3, MSSQL 및 Oracle.

### CLI

아도니스Js와 라라벨이 자체 CLI를 들고 포장에서 나온다. 따라서 개발자는 마이그레이션 실행, 컨트롤러, 모델 생성 및 데이터베이스 시드, 사용자 정의 명령 작성 등 특정 작업에 대한 명령을 실행할 수 있습니다.

라라벨의 명령 도구는 아티산(Artisan)이라 불리며, 아도니스Js 명령 도구는 에이스이다.

### IOC 및 서비스 프로바이더

어떤 응용 프로그램에서도 종속성을 관리하기 어렵습니다. 그들은 독립적이거나 의존적일 수 있다. 제대로 처리되지 않으면 버그가 발생할 수 있습니다. Adonis는 IoC를 사용하여 이 문제를 해결하는 반면 Laravel은 일반적으로 클래스 종속성을 관리하고 종속성 주입을 수행하는 서비스 컨테이너라고 부른다.

서비스 공급자는 미들웨어, 이벤트 수신기, 경로 및 서비스 컨테이너 바인딩의 등록을 처리합니다. 대부분의 경우 응용 프로그램에서 사용할 미들웨어를 등록합니다. 이 패턴은 Laravel과 AdonisJs에 모두 적용된다.

### 파일 시스템

AdonisJs는 FlyDrive라고 하는 전용 드라이브 공급자(서비스 공급자에 등록되어야 함)를 사용합니다. 이것은 Amazon S3와 같은 로컬 및 원격 파일 시스템의 상호 작용을 돕습니다.

기본적으로 설치되지 않으므로 드라이브를 `npm`에서 빼는 것은 필수입니다.

```css
adonis install @adonisjs/drive
```

다음으로, 서비스 공급자에 등록합니다.

```undefined
const providers = [<br> '@adonisjs/drive/providers/DriveProvider'<br>]
```

Laravel은 FlySystem을 API로 사용하며, API가 그대로 유지되기 때문에 로컬 및 원격 파일 시스템(Amazon S3) 간에 유연하게 전환할 수 있다.

### 공동체 힘

현재 아도니스Js는 커뮤니티가 약해서, 커뮤니티가 꽤 작다는 것을 의미한다. 이로 인해 StackOverflow에 대한 기여자는 줄어들고 답변은 거의 없습니다. 대부분의 이슈는 절대 찾을 수 없기 때문에 깊이 파고들어야 할 수도 있다.

한편, 두 프레임워크의 문서는 훌륭하고 잘 설명되어 있습니다. 그들은 모든 것을 커버하고, 경험이 많고 새로운 개발자들에게 매우 도움이 된다.

### 성과

Laravel과 AdonisJs 모두 매우 빠르며, 대부분의 경우 서버에 배포할 때 웹 응용 프로그램이 빠릅니다. 프로그램에 응답 시간이 느릴 경우 비교할 필요가 없습니다. 대신 다음 작업을 수행하십시오.

- 사용하지 않는 패키지 제거
- 최신 버전의 AdonisJs 또는 Laravel 사용
- 캐시 라우터
- 캐시 구성 파일
- 캐시 뷰
- 보기를 줄이거나 줄입니다.
- 애저 로드 사용
- 주로 사용되는 데이터베이스 테이블 열 색인
- 더 많은 최적화 팁

이렇게 되면 성능 비교가 안 될 겁니다.

## 결론

이 기사에서는 라라벨과 아도니스Js의 기본적인 차이점을 살펴보고 그들의 가장 좋은 특징을 강조했다. 전반적으로, 어느 쪽도 다른 쪽보다 낫지 않습니다. 둘 다 원하는 목표를 달성합니다. 모두 사용 사례에 따라 다릅니다.

또한 라우팅, 마이그레이션, 모델, 컨트롤러, 미들웨어, 구성 파일 및 .env의 유사성까지 다양합니다. 그 밖에는, 두 가지 모두 빠르고 동일한 규모로 성능을 발휘합니다.

Node.js 개발자들에게, AdonisJs는 흥미로운 MVC 도구일 수 있다. PHP에서 Node.js로 마이그레이션하려는 사람들에게, AdonisJs는 올바른 선택이다.

두 가지 프레임워크 모두 견고하고 해결해야 할 문제를 해결할 수 있다는 점을 기억하는 것이 좋을 것이다. 또한 둘 다 오픈 소스입니다.