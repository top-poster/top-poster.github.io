---
layout: post
title: "Rector 및 GitHub Actions를 통해 PHP 7.4로 코딩하고 7.1로 배포"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/coding-php-7-4-deploying-7-1-rector.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/coding-php-7-4-deploying-7-1-rector.png?fit=730%2C487&ssl=1)

PHP 개발자들은 언어의 최신 기능에 접근하기를 원하지만, 여러 가지 이유로 그들은 접근하지 못할 수도 있다. 클라이언트의 서버가 이전 버전에서 실행되어 업그레이드할 수 없거나, CMS가 레거시 코드를 지원해야 하거나, 사용자 기반이 크게 축소되거나, 다른 사용자가 업그레이드할 수 있습니다.

하지만 해결책이 있습니다. 우리는 트랜슬러를 사용하여 기존 구문으로 코드를 변환할 수 있습니다. 트랜스필러는 두 가지 장점을 모두 제공합니다. 개발자는 최신 기능을 사용하여 코드를 작성하고 이전 버전의 언어와 함께 작동하는 프로덕션 자산을 생성할 수 있습니다.

이전 기사에서는 PHP를 위한 재구성 도구인 Reector를 소개했습니다. 이제 그것을 실천해 봅시다. 이 기사에서는 PHP 7.4 코드를 사용하여 WordPress 플러그인을 개발하는 방법과 Rector 및 GitHub Actions를 통해 PHP 7.1 이하의 코드가 포함된 플러그인을 릴리스하는 방법에 대해 알아보겠습니다.

## PHP 7.1의 이유

WordPress가 현재 5.6인 최소 PHP 버전을 범프하지 않기로 결정한 결과로 WordPress 플러그인을 번역하기 시작했습니다. 그러면 당신은 내가 왜 PHP 5.1로 전이되고 PHP 5.6으로 전이되는가?라고 생각할 수 있다.

여기에는 두 가지 이유가 있다. 첫째, Rector는 ArrowFunctionToAnonymous와 같은 규칙을 기반으로 변환을 수행합니다.FunctionRector: PHP 7.4 이하에서 익명 함수로 코드를 다운그레이드합니다.

```undefined
class SomeClass
 {
     public function run()
     {
         $delimiter = ",";
-        $callable = fn($matches) => $delimiter . strtolower($matches[1]);
+        $callable = function ($matches) use ($delimiter) {
+            return $delimiter . strtolower($matches[1]);
+        };
     }
 }
```

현재까지 구현된 약 20개의 다운그레이드 규칙으로부터, 소수만이 PHP 7.1에서 7.0까지이고, 7.0에서 5.6까지는 없다. 따라서 7.0에 도달하기 위한 지원은 제한적이며 5.6을 목표로 하는 지원은 아직 없습니다.

이것은 Rector가 PHP 5.6을 지원할 수 없다는 것을 의미하지는 않지만, 작업이 완료되어야 한다. 규칙이 결국 구현되면(WordPress가 최소 버전을 7.1로 충돌하기 전에, 그렇지 않으면 더 이상 필요하지 않을 것) 더 낮은 PHP 버전을 대상으로 지정할 수 있습니다.

두 번째 이유는 타사 PHP 종속성과 관련이 있습니다. 또한 이러한 정보는 당사의 애플리케이션 코드와 함께 전송되어야 하며, 이를 위해 상당한 노력이 필요할 수 있습니다.

예를 들어 종속성이 PHP 7.1을 필요로 하고 PHP 7.1을 목표로 하는 경우 종속성은 직접 지원되므로 코드를 전송할 필요가 없습니다. 하지만 내가 만약 PHP 7.0이나 5.6을 목표로 한다면, 나는 그것을 전송해야 한다.

타사의 종속성을 제거하는 것은 내 통제 하에 있지 않기 때문에 어려운 일이 될 수 있습니다. 코드 검색만으로는 충분하지 않습니다. 종속성의 모든 PHP 7.1 코드가 전송될 수 있도록 철저한 조사를 해야 합니다. 내 주의를 끌지 못하는 단일 기능으로 인해 런타임에 응용 프로그램이 실패할 수 있습니다.

내 경우 내 애플리케이션은 PHP 7.2를 필요로 하는 종속성이 하나 있고 PHP 7.1을 필요로 하는 종속성이 수십 개 있다(나중에 이것에 대해 더 자세히 설명). 저는 무제한의 자원을 가지고 있지 않기 때문에, 7.0을 목표로 하고 수십 개를 전송하기 보다는 PHP 7.1을 목표로 하고 하나의 종속성을 전송하기로 선택했습니다.

그 결과 워드프레스 5.6과 7.0을 실행하는 사용자는 내 워드프레스 플러그인을 사용할 수 없게 되지만, 그것은 내가 만족하는 보상이다.

## 지원되는 PHP 기능

응용 프로그램이 이제 PHP 7.4 코드를 사용할 수 있다고 말할 때, 이것은 반드시 PHP 7.4에 도입된 모든 기능을 사용할 수 있다는 것을 의미하지는 않는다. 오히려, 다운그레이드하기 위해 Rector 규칙이 있는 기능만 사용할 수 있다.

또한 일부 기능은 일부 이유로 인해 전송되지 않으며, 일부 기능은 전송되지 않습니다.

예를 들어, PHP 7.4에 도입된 새로운 상수 중 상수 `SO_LABEL`, `SO_PEERLABEL` 등은 FreeBSD 전용 소켓 옵션이다. 그것은 너무 구체적이기 때문에 나는 아무도 그들을 위해 렉터 규칙을 시행하지 않을 것으로 예상한다.

결과적으로, 응용 프로그램은 PHP 7.4를 완전히 지원하지 않을 것이다. (누군가나 상수 `SO_LABEL`을 필요로 한다면 그것은 없을 것이다.) 대신 PHP 7.1을 완전히 지원할 수 있고 PHP 7.2, 7.3 및 7.4의 기능으로 향상될 것이다.

아래 목록에는 PHP 7.1용 응용 프로그램을 릴리스하기 위해 현재 지원되는 기능이 나열되어 있습니다. 이 목록(커뮤니티에서 나머지 다운그레이드 규칙을 구현할 때 확장해야 함)에는 Symfony polyfill 패키지에서 지원되는 기능도 포함되어 있습니다.

일부 PHP 8.0 기능이 이미 지원된다는 사실을 알고 계십니까? 올해 말 PHP 8.0이 출시되는 즉시 PHP 7.1에 대한 지원을 중단하지 않고 애플리케이션 코드에서 유니언 유형을 즉시 사용할 수 있습니다. 얼마나 멋진 일입니까?

## 변속기 입력 및 출력

나는 나만의 WordPress용 GraphQL API와 그 패키지를 사용하여 Rector를 통해 WordPress 플러그인을 전송하는 방법을 시연할 것이다.

플러그인의 코드는 PHP 7.4, 7.3 및 7.2의 기능을 사용한다.

- 입력한 속성, 화살표 함수, null 병합 할당 연산자, 배열 내부 포장 풀기 및 PHP 7.4의 숫자 리터럴 구분 기호
- PHP 7.3의 어레이 파괴 및 유연한 Etheroc 구문에서 참조 할당
- PHP 7.2의 `개체` 반환 및 매개 변수 유형

전송 시 이러한 기능은 PHP 7.1에서 동등한 코드로 변환됩니다.

다음 표에는 소스 코드의 예와 프로덕션 자산을 생성할 때 Rector가 변환하는 예가 표시됩니다.

파일은 `src/` 폴더와 `vendor/` 폴더의 두 원본에서 가져옵니다.

`src/`는 응용 프로그램 코드가 저장되는 곳이기 때문에 완전히 제 통제 하에 있습니다. 따라서 이 코드는 앞에서 설명한 지원되는 PHP 기능만 포함할 수 있습니다.

벤더/는 나와 타사가 소유한 모든 종속성(Composer를 통해 관리됨)을 포함합니다. 내 플러그인의 경우, 모든 전송 종속성(소유자 getpop, pop-schema, graphql-by-pop)도 내 것이므로 이 코드가 지원되는 기능만 포함한다는 것을 다시 한번 보장할 수 있다.

제외된 경로는 이미 알고 있는 PHP 7.1 이하의 코드만 포함하는 포함된 종속성에 해당합니다. 그래서 그들을 위해 전치할 것이 아무것도 없습니다. 그래서 저는 직접적으로 렉터를 거릅니다.

타사 종속성은 어떻게 됩니까? 왜 나는 그들 중 누구도 번역하지 않는 거지?

다행히도 그럴 필요가 없었어요. 그 이유는 이렇다.

### 타사 종속성 전송

타사 종속성이 PHP 7.1로 전송되어야 하는지 여부를 확인해야 합니다.

첫 번째 단계는 PHP 7.2 이상이 필요한 종속성을 찾는 것입니다. 이를 위해 변환된 코드를 실행할 것이기 때문에 프로덕션용 Composer 종속성을 설치합니다.

```undefined
composer install --no-dev
```

이제 다음을 실행하여 PHP 7.1을 지원하지 않는 종속성 목록을 얻을 수 있습니다.

```css
composer why-not php 7.1.33
```

제약 조건은 버전 7.1.33(PHP 7.1의 최신 버전)에 있고 직접 7.1에 있지 않습니다. 7.1이 7.1.0으로 해석돼 7.1.3이 필요한 패키지도 실패하기 때문이다.

내 플러그인의 경우 위의 명령을 실행하면 다음과 같은 종속성이 생성됩니다.

```java
symfony/cache                                 v5.1.6         requires  php (>=7.2.5)
symfony/cache-contracts                       v2.2.0         requires  php (>=7.2.5)
symfony/expression-language                   v5.1.6         requires  php (>=7.2.5)
symfony/filesystem                            v5.1.6         requires  php (>=7.2.5)
symfony/inflector                             v5.1.6         requires  php (>=7.2.5)
symfony/service-contracts                     v2.2.0         requires  php (>=7.2.5)
symfony/string                                v5.1.6         requires  php (>=7.2.5)
symfony/var-exporter                          v5.1.6         requires  php (>=7.2.5)
```

그래서 저는 왜 적어도 PHP 7.2.5가 필요한지 확인하기 위해 이 8개 패키지의 소스 코드를 검사해야 했습니다. 그리고 그 코드가 전송될 수 있는지 알아내야 했습니다.

6개 패키지(캐시-계약, 표현-언어, 파일시스템, 인플렉터, 서비스-계약, 문자열)는 PHP 7.1 코드 이하만 사용한다. 종속성 중 하나에 이러한 요구사항이 있기 때문에 PHP 7.2.5에 대한 요구사항이 있다.

psynony/cache의 종속성인 package `symony/var-exporter`에 PHP 7.2 코드가 포함되어 있는지 여부(PpArrayAdapter 및 PhpFilesAdapter) 및 autolo 및 psolo 때문에 PHP 7.2 코드가 포함되어 있는지 여부도 알 수 없습니다.

마지막으로 패키지 `symony/cache`는 클래스 `PdoAdapter`의 PHP 7.2 코드를 포함합니다. 이 코드를 전송할 수는 있지만(해당 다운그레이드 규칙이 있음) 그럴 필요는 없습니다. 응용 프로그램이 클래스 `PdoAdapter`에 액세스하지 않고 `PSR-4` 때문에 로드되지 않습니다.

이 8개의 패키지는 다소 작으며, PHP 7.2는 소수의 새로운 기능만 도입했기 때문에 PHP 7.2 코드의 발생을 찾는 것은 그리 어렵지 않았다. 그러나 더 큰 패키지를 가지고 있거나 더 많은 기능을 가진 PHP 버전을 목표로 하는 것은 작업을 더 어렵게 만들 것이다.

### 다운그레이드 세트

그런 다음 코드에 적용할 세트 또는 규칙을 정의합니다.

```php
  // here we can define what sets of rules will be applied
  $parameters->set(Option::SETS, [
    // @todo Uncomment when PHP 8.0 released
    // SetList::DOWNGRADE_PHP80,
    SetList::DOWNGRADE_PHP74,
    SetList::DOWNGRADE_PHP73,
    SetList::DOWNGRADE_PHP72,
  ]);
```

`SetList:`SetList:다운그레이드_PHP80` 라인? PHP 8.0이 출시되는 날, 해당 라인을 비컴파일하는 것만으로도 내 플러그인은 😎 union type을 사용할 수 있습니다.

세트가 실행되는 순서에 따라 코드를 상위 버전에서 하위 버전으로 다운그레이드해야 합니다.

- PHP 7.4 ~ 7.3
- PHP 7.3에서 7.2까지
- PHP 7.2에서 7.1로

현재 규칙의 경우, 이것은 차이를 만들지는 않지만 다운그레이드된 코드가 하위 PHP 버전의 다른 규칙에 의해 수정되는 경우이다.

예를 들어 null 병합 할당 연산자 `???PHP 7.4에 도입된 =는 다음과 같이 다운그레이드됩니다.

```undefined
 $array = [];
-$array['user_id'] ??= 'value';
+$array['user_id'] = $array['user_id'] ?? 'value';
```

그런 다음 PHP 5.6까지 다운그레이드하면 null 병합 연산자 `????`와 함께 코드가 변환됩니다.`도 다음과 같이 다운그레이드해야 합니다.

```undefined
 $array = [];
-$array['user_id'] = $array['user_id'] ?? 'value';
+$array['user_id'] = isset($array['user_id']) ? $array['user_id'] : 'value'; 
```

### 워드프레스 로드 중

WordPress는 Composer 자동 로드를 사용하지 않으므로 소스 파일에 대한 경로를 제공해야 합니다. 그렇지 않으면 Rector가 WordPress 코드를 발견할 때마다 오류가 발생합니다(예: WordPress 기능 실행, WordPress에서 클래스 확장).

```php
  // Rector relies on autoload setup of your project; Composer autoload is included by default; to add more:
  $parameters->set(Option::AUTOLOAD_PATHS, [
    // full directory
    __DIR__ . '/vendor/wordpress/wordpress',
  ]);
```

WordPress 소스 파일을 다운로드하기 위해 WordPress를 Composer 종속성(단, 개발 전용)으로 추가하고 해당 위치를 `벤더/워드프레스/워드프레스`로 사용자 지정합니다. 우리의 `composer.json`은 다음과 같이 보일 것이다.

```undefined
{
  "require-dev": {
    "johnpbloch/wordpress": ">=5.5"
  },
  "extra": {
    "wordpress-install-dir": "vendor/wordpress/wordpress"
  }
}
```

### 워드프레스 처리

WordPress의 자동 로드 경로를 포함하는 것만으로는 충분하지 않을 수 있습니다. 예를 들어, Rector를 실행할 때 이 오류가 발생합니다(내 코드가 클래스 `WP_Upgrader`를 참조하는 위치).

```undefined
PHP Warning:  Use of undefined constant ABSPATH - assumed 'ABSPATH' (this will throw an Error in a future version of PHP) in .../graphql-api-for-wp/vendor/wordpress/wordpress/wp-admin/includes/class-wp-upgrader.php on line 13
```

왜 이런 일이 일어나는지 깊이 파고들지는 않았지만 상수ABSPATH(wp-load.php)를 정의하는 워드프레스 코드가 어쩐지 실행되지 않은 것 같다. 그래서 저는 방금 제 Rector 구성에서 WordPress 원본 파일이 있는 위치를 가리키며 이 논리를 복제했습니다.

```php
  /** Define ABSPATH as this file's directory */
  if (!defined('ABSPATH')) {
    define('ABSPATH', __DIR__ . '/vendor/wordpress/wordpress/');
  }
```

## 러닝 로터

Rector 구성이 설정되었으니 일부 코드 전송을 시작하겠습니다!

Rector를 실행하려면 실행하는 플러그인의 루트 폴더에서 다음을 수행합니다.

```undefined
vendor/bin/rector process --dry-run
```

코드를 다운그레이드하고 원본 파일을 재정의하고 싶지 않기 때문에 --dry-run을 사용해야 합니다. "--dry-run"이 없는 프로세스는 생산용 자산을 생산할 때 지속적인 통합 프로세스 내에서 실행됩니다.

내 플러그인의 경우, Rector는 지정된 경로에 포함된 4,188개의 파일에 대해 16개의 다운그레이드 규칙을 처리하는 데 약 1분이 소요되며, 이후 173개의 파일의 코드가 변환되는 방법을 표시합니다.

## 전송 코드 테스트

일단 코드를 전송하고 나면, 어떻게 그것이 잘 작동하는지 알 수 있을까요? 즉, 만약 우리가 PHP 7.1을 목표로 한다면, 어떻게 PHP 7.2 이상의 모든 코드 조각이 다운그레이드되었는지 확인할 수 있는가?

제가 찾은 방법은 PHP 7.1을 사용하여 다운그레이드된 코드를 실행하는 것입니다. PHP 7.2 이상의 코드가 여전히 남아 있고 참조되는 경우 PHP 엔진이 이를 인식하지 못하고 오류를 발생시킵니다.

저는 지속적인 통합 프로세스의 일환으로 Travis와 함께 이 솔루션을 구현했습니다. 새 코드를 repo에 푸시할 때마다 해당 코드가 제대로 다운그레이드될 수 있는지 확인합니다. 이것을 주장하기 위해, 나는 단지 번역된 코드에서 PHPStan을 실행한다; 만약 PHPStan 프로세스가 오류 없이 종료된다면, 그것은 모든 번역된 코드가 PHP 7.1과 호환된다는 것을 의미한다.

이러한 결과(빨간색으로 변환된 코드 삭제 및 녹색으로 추가됨)를 생성하는 솔루션이 여기에 구현됩니다.

```undefined
language: php
os:
  - linux
dist: bionic

php:
  - 7.4

jobs:
  include:
    - name: "Test downgrading"
      script:
        - vendor/bin/rector process
        - composer config platform-check false
        - composer dumpautoload
        - phpenv local 7.1
        - vendor/bin/phpstan analyse -c phpstan.neon.dist src/
      after_script: skip

script:
  - vendor/bin/phpunit --coverage-text --coverage-clover=coverage.clover
```

이 솔루션이 어떻게 작동하는지 살펴보겠습니다.

우리는 먼저 `벤더/bin/reector process`를 실행하여 Rector를 통해 코드를 다운그레이드한다. 소스 파일에는 PHP 7.4 코드가 포함되어 있으므로, Rector를 실행하지 않으면 파일을 구문 분석할 때 PHP 엔진이 오류를 발생시킨다.

작곡가 v2(얼마 전 출시)는 플랫폼 점검을 도입했다. composer.json은 PHP 7.4를 요구하지만 PHP 7.1을 실행하게 되므로 이를 사용하지 않도록 설정해야 하며 그렇지 않으면 ppsstan을 실행하면 오류가 발생합니다. 이를 위해 먼저 `composer config statform-check false`를 실행한 다음 `composer dump auto load`를 실행하여 `vendor/composer/platform_check` 파일을 제거한다.php는 유효성 검사가 수행되는 지점입니다.

코드를 다운그레이드한 후 환경의 PHP 버전을 7.4에서 7.1로 전환한다. 이러한 이유로 우리는 Ubuntu 18.04 LTS, Bionic을 빌드 환경으로 사용하고 있는데, 이는 PHP 7.1이 사전에 설치되어 있기 때문이며, PHP 7.1을 실행함으로써 PHP 7.1로 전환할 수 있기 때문이다.

`vendor/bin/phpstan analysis -cphstan.neon.distsrc/` 명령은 다운그레이드된 코드에서 PHPStan을 실행합니다. 이 프로세스가 `0`으로 종료되면 다운그레이드가 성공했음을 의미하며, 그렇지 않으면 실패한 코드를 가리키는 오류 메시지가 표시됩니다.

내 플러그인은 PHP 7.3 이상이 필요한 최신 버전의 PHPUit(버전 9.4)을 사용합니다. 따라서 이 프로세스는 PHPU 유닛을 실행할 수 없거나 PHPU 유닛이 실패하므로 건너뜁니다. 그런 다음 Travis는 행렬을 사용하여 다른 테스트를 수행해야 하며 PHPUnit은 별도의 실행으로 실행됩니다.

## 특이점 처리

우리는 때때로 우리가 고쳐야 할 이상한 점들을 마주칠지도 모른다.

예를 들어, 나는 잠재적인 버그가 (가장 엄격한 모드, 레벨 `8` 사용) 일치하지 않도록 소스 코드에서 PHPStan을 실행한다. PHPStan은 현재 익명 함수를 `array_filter`에 전달하면 존재하지 않는 오류가 발생할 수 있지만 대신 화살표 함수를 전달하면 잘 작동하는 버그를 가지고 있다.

결과적으로, 화살표 함수를 포함하는 소스 코드와 익명 함수를 포함하는 전송 버전에 대한 PHPStan의 동작은 다를 수 있다. 플러그인의 경우 PHPStan은 다음 화살표 기능에 대한 오류를 표시하지 않습니다.

```php
$skipSchemaModuleComponentClasses = array_filter(
  $maybeSkipSchemaModuleComponentClasses,
  fn ($module) => !$moduleRegistry->isModuleEnabled($module),
  ARRAY_FILTER_USE_KEY
);
```

그러나 전송된 코드에 오류가 발생합니다.

```php
$skipSchemaModuleComponentClasses = array_filter(
  $maybeSkipSchemaModuleComponentClasses,
  function ($module) use ($moduleRegistry) {
      return !$moduleRegistry->isModuleEnabled($module);
  },
  ARRAY_FILTER_USE_KEY
);
```

이를 해결하기 위해 PHPStan을 구성하여 다운그레이드된 코드의 경우 오류를 무시하고 일치하지 않는 오류(소스 코드의 경우):

```bash
parameters:
  reportUnmatchedIgnoredErrors: false
  ignoreErrors:
    -
      message: '#^Parameter \#1 \$module of method GraphQLAPI\\GraphQLAPI\\Registries\\ModuleRegistryInterface::isModuleEnabled\(\) expects string, array\<int, class-string\> given\.$#'
      path: src/PluginConfiguration.php
```

이를 위해 소스 코드와 그 전송 버전이 프로세스를 실행할 때 항상 동일한 동작을 발생시켜 불쾌한 놀라움을 방지하는지 다시 확인해야 한다.

## GitHub Actions를 통해 생산을 위한 자산 생성

거의 다 됐어요. 지금쯤이면, 우리는 트랜스필링을 구성하고 테스트해 보았습니다. 이제 생산을 위해 자산을 생성할 때 코드를 변환하기만 하면 됩니다. 이 자산은 설치를 위해 배포될 실제 WordPress 플러그인이 됩니다.

플러그 인 코드가 GitHub에서 호스팅되기 때문에, 저는 코드 태그가 지정되면 전송된 자산을 생성하는 GitHub 동작을 만들었습니다. 액션에는 다음과 같은 내용이 있습니다.

```undefined
name: Generate Installable Plugin and Upload as Release Asset
on:
  release:
    types: [published]
jobs:
  build:
    name: Build, Downgrade and Upload Release
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      - name: Downgrade code for production (to PHP 7.1)
        run: |
          composer install
          vendor/bin/rector process
          sed -i 's/Requires PHP: 7.4/Requires PHP: 7.1/' graphql-api.php
      - name: Build project for production
        run: |
          composer install --no-dev --optimize-autoloader
          mkdir build
      - name: Create artifact
        uses: montudor/action-zip@v0.1.0
        with:
          args: zip -X -r build/graphql-api.zip . -x *.git* node_modules/\* .* "*/\.*" CODE_OF_CONDUCT.md CONTRIBUTING.md ISSUE_TEMPLATE.md PULL_REQUEST_TEMPLATE.md rector.php *.dist composer.* dev-helpers** build**
      - name: Upload artifact
        uses: actions/upload-artifact@v2
        with:
            name: graphql-api
            path: build/graphql-api.zip
      - name: Upload to release
        uses: JasonEtco/upload-to-release@master
        with:
          args: build/graphql-api.zip application/zip
        env:
          GITHUB_TOKEN: ${ secrets.GITHUB_TOKEN }
```

저는 이미 블로그에 모든 Composer 종속성이 포함된 새 `.zip` 파일을 생성하는 방법 및 GitHubrepo에 릴리스 자산으로 업로드하는 방법 등 이 작업의 대부분의 단계를 문서화했습니다.

코드를 다운그레이드하는 단계만 새로 추가되면 다음과 같은 상황이 발생합니다.

```undefined
      - name: Downgrade code for production (to PHP 7.1)
        run: |
          composer install
          vendor/bin/rector process
          sed -i 's/Requires PHP: 7.4/Requires PHP: 7.1/' graphql-api.php
```

동작 내에서 컴포터 인스톨을 두 번 실행하는 방법을 알아두십시오. 즉, Rector가 Devident로 설치되기 때문에 "--no-dev"가 없으면 "--no-dev"를 사용하여 자산을 생성하기 전에 "vendor/"에서 모든 Devident를 제거합니다.

종속성을 설치한 후 `벤더/bin/reector process`를 실행하여 코드를 전송합니다. 여기에는 --dry-run이 없으므로, Rector는 변환만 표시할 것이 아니라 입력 파일에 적용할 것이다.

그런 다음, 우리는 플러그인의 메인 파일(WordPress가 플러그인 설치 가능 여부를 검증하는 데 의존하는)에서 PHP 필요 헤더를 7.4에서 7.1로 수정해야 한다. 우리는 `sed -i`s/requires PHP: 7.4/Requires PHP: 7.1/` graphql-api.php`를 실행하여 이를 수행한다.

이 마지막 단계는 세부 사항으로 보일 수 있습니다. 결국, 나는 소스 코드에 이미 있는 `필수 PHP: 7.1` 헤더를 정의할 수 있었다. 그러나, 우리는 또한 repo에서 직접 플러그인을 설치할 수 있다(실제로, 그것은 개발의 경우이다. 따라서 일관성을 위해 소스 코드와 생성된 `.zip` 파일 플러그인은 각각 PHP 버전을 표시해야 합니다.

마지막으로, `.zip` 파일을 생성할 때 파일 `reector`를 제외해야 합니다.php`(제외할 다른 모든 파일과 함께):

```undefined
      - name: Create artifact
        uses: montudor/action-zip@v0.1.0
        with:
          args: zip -X -r build/graphql-api.zip . -x rector.php ...
```

이 GitHub 작업이 트리거되면 플러그인 자산 `graphql-api.zip`을 생성하여 릴리스 페이지에 업로드합니다.

자산생성이 성공적이었는지 확인해 보자. 이를 위해 변환된 플러그인 `graphql-api.zip`을 다운로드하여 PHP 7.1을 실행하는 WordPress 사이트에 설치한 다음 해당 기능을 호출합니다(이 경우 GraphQL 쿼리 실행).

됐다!

## 결론

플러그인은 PHP 7.4의 기능을 사용하여 코딩되었으며, PHP 7.1을 실행하는 WordPress에 설치할 수 있습니다. 목표는 🙏입니다.

PHP 코드를 전송하면 애플리케이션 자체에서 애플리케이션 개발을 분리할 수 있으므로 클라이언트나 CMS가 이를 지원할 수 없어도 최신 PHP 기능을 사용할 수 있다. PHP 8.0이 바로 코앞에 있다. 조합 유형을 사용하시겠습니까? 이제 넌 할 수 있어!