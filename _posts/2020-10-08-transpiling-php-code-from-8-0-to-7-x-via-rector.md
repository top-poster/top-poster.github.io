---
layout: post
title: "Rector를 통해 PHP 코드 8.0에서 7.x로 변환"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/transpiling-php-code-rector.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/transpiling-php-code-rector.png?fit=730%2C487&ssl=1)

PHP 8.0은 올해 말에 출시될 것이다. 우리 프로젝트에 바로 도입이 가능한가요? 아니면 예를 들어, 레거시 코드와 함께 프레임워크나 CMS를 사용하기 때문에 이 작업을 수행할 수 없을까요?

이러한 우려는 Laravel, Symfony, Drupal, banilla PHP를 기반으로 하거나 하지 않는 모든 PHP 기반 프로젝트에 영향을 미치지만, 특히 WordPress에 대한 요구가 높으며, 해당 커뮤니티는 현재 솔루션을 찾고 있습니다.

올해 12월에 출시될 새 릴리즈에서 WordPress는 최소 요구 PHP 버전을 5.6에서 7.1로 업그레이드해야 한다. 그러나 설치의 거의 24%가 여전히 PHP 5.6 또는 7.0에서 실행되기 때문에 PHP 버전 범프를 일시적으로 취소하기로 결정했습니다.

이러한 상황에서는 최소 버전을 업그레이드하기 위한 고정 일정을 시작하는 것이 제안되었으며, 이전 버전에 대한 보안 패치를 제공하는 동시에 새 PHP 버전으로 업그레이드하는 것 사이에 절충안이 제공됩니다.

이 고정된 일정이 승인되었든 안 되었든, PHP의 최신 개선 사항을 사용하고자 하는 개발자들에게 상황은 더 심각해 보인다. 테마와 플러그인은 WordPress에 의한 PHP 요구 사항에 의해 제한되지 않기 때문에 이미 버전 7.1 이상이 필요할 수 있다. 그러나, 그렇게 하는 것은 그들의 잠재적인 도달 범위를 제한한다.

예를 들어, 현재 PHP 7.4에서 실행되는 설치는 10.7%에 불과하며, 출시 즉시 PHP 8.0에서 실행되는 설치는 이보다 더 적을 것으로 예상할 수 있습니다. 이러한 수치들은 다른 중요한 특징들 중에서 코드베이스에 타이핑된 속성이나 유니언 타입을 도입하는 것을 매우 어렵게 만든다.

개발자의 이 발언은 다음과 같은 절망감을 전달한다.

> 따라서 이는 모든 WordPress 버전을 출시 후 3년이 지난 2023년 12월까지 지원하려면 테마/플러그인에서 PHP 8 구문을 사용할 수 없다는 것을 의미한다. 매우 실망입니다.

오늘 상황을 개선하기 위해 할 수 있는 일이 있나요? 아니면 WordPress 테마 및 플러그인에 PHP 8 코드를 사용하려면 3년을 기다려야 합니까? (그때쯤이면 수명을 다했을 거야!)

## 바벨이 길을 알려준다.

트랜슬러는 "프로그래밍 언어로 작성된 프로그램의 소스 코드를 입력으로 받아 같은 또는 다른 프로그래밍 언어로 동등한 소스 코드를 생성하는 번역기의 한 종류"이다.

번역을 위한 모범 모델은 바벨(Babel)로, ECMAScript 2015+ 코드를 JavaScript의 역호환 버전으로 변환할 수 있는 도구 체인이다. 바벨 덕분에 개발자들은 새로운 자바스크립트 언어 기능을 사용하여 소스 코드를 이전 브라우저에서 실행할 수 있는 자바스크립트 버전으로 변환할 수 있다.

예를 들어, Babel은 ES2015 화살표 함수를 다음과 같은 ES5로 변환합니다.

```js
// Babel Input: ES2015 arrow function
[1, 2, 3].map((n) => n + 1);

// Babel Output: ES5 equivalent
[1, 2, 3].map(function(n) {
  return n + 1;
});
```

ES2015의 선두에 따라, PHP 7.4는 또한 익명 함수보다 통사당으로서 화살표 함수를 도입하였으며, 이는 PHP 5.3 이후부터 지원되었다.

```undefined
// PHP 7.4: arrow function
$nums = array_map(fn($n) => $n + 1, [1, 2, 3]);

// PHP 5.3: anonymous function
$nums = array_map(
  function ($n) {
    return $n + 1;
  },
  [1, 2, 3]
);
```

PHP를 위한 전송 도구를 사용하여, 우리는 PHP 7.4 화살표 함수를 작성하고 5.3부터 시작하는 모든 버전의 PHP에서 실행할 수 있는 동등한 익명 함수로 변환할 수 있었다.

이를 통해 개발자는 WordPress 테마와 플러그인에 PHP 7.4의 기능을 사용할 수 있을 뿐만 아니라 PHP 7.1과 같은 이전 버전을 실행하는 사용자도 소프트웨어를 설치할 수 있게 된다.

## 개발 툴체인 업그레이드

전송의 또 다른 이점은 개발에 사용되는 새로운 버전의 라이브러리에 접근할 수 있다는 것이다.

테스트를 위한 프레임워크인 PHPUit이 그렇다. 오늘날 PHP 5.6과 함께 존재하기 때문에 WordPress는 PHPunit의 버전 7.x를 능가할 수 없으며, 결과적으로 테스트 제품군은 PHP 8에 대해 테스트할 수 없다.

PHP 7.3+(또는 PHP 7.1+)로 코딩한 다음, 프로덕션용 코드를 번역하면 PHPunit의 버전 9.x(또는 8.x)로 업그레이드하고 테스트 제품군을 현대화할 수 있다.

## 새 기능을 전송할 수 있는지 여부 평가

새로운 PHP 버전에 도입된 기능들은 대략 다음과 같이 분류될 수 있다.

- 일부 기존 기능에 대한 구문 슈가로서의 새로운 구문
- 새 기능에 대한 새 구문
- 새로운 기능, 클래스, 인터페이스, 상수 및 예외 구현

위에서 설명한 PHP 7.4에 도입된 화살표 함수는 이미 존재하는 기능에 대한 새로운 구문 예제이다. 구문을 새 버전에서 이전 버전으로 변환하면 동일한 기능이 실행되므로, 이러한 기능은 전송될 수 있으며, 결과 코드는 단점이 없습니다.

다른 사례들을 분석해보자.

### 개발에 사용할 수 있는 새로운 기능 만들기

Type 속성(PHP 7.4에 도입됨)과 유니언 유형(PHP 8.0에 도입됨)은 새로운 기능을 위한 새로운 구문을 도입한다.

```undefined
class User
{
  // Typed properties
  private int $id;
  private string $name;
  private bool $isAdmin;

  // Union types (in params and return declaration)
  public function getID(string|int $domain): string|int
  {
    if ($this->isAdmin) {
      return $domain . $this->name;
    }
    return $domain . $this->id;
  }
}
```

이러한 기능은 이전 PHP 버전에서는 직접 복제할 수 없습니다. 전송 코드에서 가장 근접한 것은 완전히 제거하고 문서 블록 태그를 사용하여 다음과 같은 특성을 설명하는 것입니다.

```undefined
class User
{
  /** @var int */
  private $id;
  /** @var string */
  private $name;
  /** @var bool */
  private $isAdmin;

  /**
   * @param string|int $domain
   * @return string|int
   */
  public function getID($domain)
  {
    if ($this->isAdmin) {
      return $domain . $this->name;
    }
    return $domain . $this->id;
  }
}
```

이 두 가지 기능을 포함하는 코드의 경우, 번역된 코드는 PHP 7.3 이하에서 컴파일되지만, 새로운 기능은 존재하지 않는다.

그러나, 그들의 부재는 중요하지 않을 것이다. 이러한 기능은 개발 중에 대부분 우리 코드의 정확성을 검증하는 데 유용하다(테스트를 위한 PHPUit, 정적 분석을 위한 PHPStan과 같은 추가 도구에 의해 지원됨). 만약 우리의 코드가 오류가 있고 그것이 생산에서 실패한다면, 그것은 이러한 새로운 기능들을 포함하거나 포함하지 않고 실패할 것이다; 최대 에러 메시지는 다를 것이다.

따라서, 코드의 불완전한 변환은 여전히 우리의 요구를 만족시키기에 충분하며, 이 코드는 생산을 위해 전송될 수 있다.

### 런타임에 필요한 기능 방지

이전 버전에서 동등하지 않고 런타임(프로덕션)에 필요한 새 기능을 제거할 수 없으며 그렇지 않으면 응용 프로그램이 다르게 동작합니다.

예를 들어 PHP 7.4에서 도입된 `취약 참조` 클래스가 있는데, 이 클래스는 우리가 여전히 참조를 보유하는 객체를 파괴할 수 있다.

```undefined
$obj = new stdClass;
$weakref = WeakReference::create($obj);
var_dump($weakref->get());
unset($obj);
var_dump($weakref->get());
```

인쇄됩니다.

```undefined
object(stdClass)#1 (0) {
}
NULL
```

PHP 7.3을 사용하면 개체에 대한 모든 참조가 제거되지 않는 한 개체가 삭제되지 않습니다.

```undefined
$obj = new stdClass;
$array = [$obj];
var_dump($array);
unset($obj);
var_dump($array);
```

인쇄됩니다.

```undefined
array(1) {
  [0]=>
  object(stdClass)#412 (0) {
  }
}
array(1) {
  [0]=>
  object(stdClass)#412 (0) {
  }
}
```

따라서, 우리는 새로운 행동이 받아들일 수 있는지 없는지를 알아내야 합니다. 예를 들어, `취약 참조` 클래스로 변환된 응용 프로그램이 메모리를 더 많이 소비할 수 있으며, 이는 허용될 수 있지만, 우리의 논리가 개체를 설정 해제한 후 `null`이라고 주장해야 한다면, 그것은 실패할 것이다.

### 백포트 기능

마지막으로, 기능, 클래스, 인터페이스, 상수 및 예외와 같은 새로 구현된 기능의 경우가 있다.

더 간단한 솔루션은 하위 PHP 버전에 대해 동일한 구현을 제공하는 것이다.

예를 들어, PHP 8.0에 도입된 함수 `str_contains`는 다음과 같이 구현될 수 있다.

```undefined
if (!defined('PHP_VERSION_ID') || (defined('PHP_VERSION_ID') && PHP_VERSION_ID < 80000)) {
  if (!function_exists('str_contains')) {
    /**
     * Checks if a string contains another
     *
     * @param string $haystack The string to search in
     * @param string $needle The string to search
     * @return boolean Returns TRUE if the needle was found in haystack, FALSE otherwise.
     */
    function str_contains(string $haystack, string $needle): bool
    {
      return strpos($haystack, $needle) !== false;
    }
  }
}
```

Symfony에서 이미 폴리필 라이브러리로 사용할 수 있기 때문에 편리하게 백업 코드를 구현할 필요가 없습니다.

- 폴리필 PHP 7.1
- 폴리필 PHP 7.2
- 폴리필 PHP 7.3
- 폴리필 PHP 7.4
- 폴리필 PHP 8.0

## Rector를 통해 PHP 코드 전송

이론에서 실천으로 전환하고 우리의 PHP 코드를 번역할 때입니다.

Reector는 코드를 즉시 업그레이드하고 리팩토링하는 재구성 도구입니다. 널리 사용되는 PHP 파서 라이브러리를 기반으로 한다.

Rector는 다음 작업 시퀀스를 실행합니다.

- 구조와 내용을 조작할 수 있는 AST(Abstract Syntax Tree의 줄임말)로 PHP 코드 구문 분석
- AST의 선택한 노드에서 변환을 실행하기 위한 규칙 적용
- 새 AST를 파일에 다시 덤프하여 변환된 PHP 코드를 저장합니다.

이 순서에서 우리는 두 번째 단계인 변환 규칙을 Reector에게 제공하는 것에 대해서만 관심을 가질 것이다.

### 규칙 설명

규칙에는 노드를 A에서 B로 변환하는 것이 목적이다. 이 작업을 설명하기 위해 최종 결과에 적용된 diff 형식을 사용한다. 삭제(상태 `A`에 속하는)는 빨간색으로 표시되고 추가(상태 `B`에 속하는)는 녹색으로 표시된다.

예를 들어, 규칙 다운그레이드 Null 병합 연산자의 차이점으로, `???`를 대체합니다.PHP 7.4에 소개된 =` 운영자:

```undefined
function run(array $options)
{
-  $options['limit'] ??= 10;
+  $options['limit'] = $array['limit'] ?? 10;

  // do something
  // ...
}
```

### Rector 규칙 목록 검색

Rector는 현재 적용할 수 있는 거의 600개의 사용 가능한 규칙을 적용할 수 있습니다. 그러나, 그들 대부분은 우리의 목표와 반대인 코드 현대화를 위한 것이다(예: PHP 7.1에서 PHP 7.4로).

우리가 사용할 수 있는 규칙은 "다운그레이드" 세트 아래에 있는 규칙:

- 다운그레이드php80
- 다운그레이드 Php74
- 다운그레이드 Php72
- 다운그레이드 Php71

이러한 세트의 각 규칙은 언급된 버전의 코드를 바로 이전 버전의 동등한 코드로 변환합니다. 그럼 다운그레이드 아래의 모든 것이PHP80은 코드를 PHP 8.0에서 7.4로 변환한다.

이러한 규칙들을 더하면, 현재 16개의 규칙이 있는데, 어느 정도까지는 PHP 8.0에서 PHP 7.0으로 코드를 변환할 수 있다.

PHP 8.0과 PHP 7.0 사이의 모든 새로운 기능에 대한 액세스를 잠금 해제하는 데 필요한 나머지 변환은 이미 문서화되어 있습니다. 누구나 오픈 소스 프로젝트에 기여하고 이러한 규칙 중 하나를 이행하는 것을 환영한다.

### 러닝 로터

Rector를 설치한 후에는 `Rector` 파일을 생성해야 합니다.php` (프로젝트의 루트에서 기본적으로) 실행할 규칙 집합을 정의하고 명령행에서 다음을 실행하여 실행합니다.

```undefined
vendor/bin/rector process src
```

이 경우 `src/` 아래에 있는 소스 코드가 변환과 함께 재정의되므로, 다운그레이드 코드를 연속 통합해야 새 자산을 생성할 수 있습니다(예: 배포 중).

변환을 적용하지 않고 미리 보려면 "--dry-run"으로 명령을 실행합니다.

```undefined
vendor/bin/rector process src --dry-run
```

`reector.php`를 구성하는 방법을 살펴보겠습니다. PHP 7.4에서 7.1로 코드를 다운그레이드하려면 `다운그레이드-php74` 및 `다운그레이드-php72`(현재 PHP 7.3에 대해 구현된 세트가 없음) 세트를 실행해야 합니다.

```undefined
<?php

declare(strict_types=1);

use Rector\Core\Configuration\Option;
use Symfony\Component\DependencyInjection\Loader\Configurator\ContainerConfigurator;
use Rector\Set\ValueObject\SetList;

return static function (ContainerConfigurator $containerConfigurator): void {
  // get parameters
  $parameters = $containerConfigurator->parameters();

  // paths to refactor; solid alternative to CLI arguments
  $parameters->set(Option::PATHS, [
    __DIR__ . '/src',
  ]);

  // here we can define, what sets of rules will be applied
  $parameters->set(Option::SETS, [
    SetList::DOWNGRADE_PHP74,
    SetList::DOWNGRADE_PHP72,
  ]);

  // is your PHP version different from the one your refactor to? [default: your PHP version]
  $parameters->set(Option::PHP_VERSION_FEATURES, '7.1');
};
```

"--dry-run"으로 명령을 실행하면 결과가 diff 형식(삭제: 빨간색, 추가: 녹색):

최종 결과는 PHP 7.4의 기능을 사용하여 작성되었지만 PHP 7.1에 배포될 수 있는 코드로 변환된 코드이다.

## 결론

가능한 한 많은 환경에 설치할 수 있는 소프트웨어를 만들어 광범위한 사용자 기반을 대상으로 하는 필요성과 함께 최신 툴 및 언어 기능에 액세스하고 코드의 품질을 개선하고자 하는 개발자의 욕구를 어떻게 절충할 수 있습니까?

전치사는 해결책입니다. 이것은 새로운 개념이 아니다. 우리가 웹사이트를 만든다면, 우리는 자바스크립트 코드를 알지 못하더라도 그것이 어떤 프레임워크에 통합될 수 있기 때문에 이미 바벨을 사용하여 자바스크립트 코드를 전송하고 있을 가능성이 크다.

우리가 몰랐을 수도 있는 것은 Rector라고 불리는 PHP 코드를 전송하는 도구가 있다는 것입니다. 이 도구를 사용하면 PHP 8.0 기능을 포함하는 코드를 작성하고 PHP 7.0까지 하위 버전을 실행하는 환경에 배포할 수 있다. 정말 멋지네요.

기분 전환!