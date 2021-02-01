---
layout: post
title: "PHP 오류를 표시하고 오류보고를 활성화하는 방법
 "
author: 'Code Tower'
thumbnail: https://images.unsplash.com/photo-1555861496-0666c8981751?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MXwxMTc3M3wwfDF8c2VhcmNofDIwfHxlcnJvcnN8ZW58MHx8fA&ixlib=rb-1.2.1&q=80&w=2000
tags: undefined
---


PHP 오류 처리에 대해 처음 배웠을 때를 아직도 생생하게 기억합니다.
 

프로그래밍 4 년차 였고 PHP로 2 년차 였고 지역 대행사에서 개발자 직책에 지원했습니다.
 애플리케이션에서 코드 샘플 (당시에는 존재하지 않았던 GitHub)을 보내야했기 때문에 전년도에 만든 간단한 사용자 지정 CMS를 압축하여 보냈습니다.
 

코드를 검토하는 사람으로부터받은 이메일은 지금까지도 여전히 내 뼈를 진정시킵니다.
 

"당신의 프로젝트에 대해 조금 걱정했지만 오류보고 기능을 끄고 나면 실제로 잘 작동하는 것 같습니다."
 

"PHP 오류보고"를 처음 검색하고이를 활성화하는 방법을 발견했으며 이전에 숨겨져 있던 오류 스트림을보고 내부에서 사망했습니다.
 

PHP 오류 및 오류보고는 언어를 처음 접하는 많은 개발자가 처음에는 놓칠 수있는 것입니다.
 많은 PHP 기반 웹 서버 설치에서 PHP 오류가 기본적으로 억제 될 수 있기 때문입니다.
 이는 아무도 이러한 오류를 보거나 인식하지 못함을 의미합니다.
 

이러한 이유로 특히 로컬 개발 환경에서 사용할 수있는 위치와 방법을 아는 것이 좋습니다.
 이렇게하면 코드에서 오류를 조기에 발견 할 수 있습니다.
 

## PHP 오류를 표시하는 방법
 

"PHP 오류"를 Google에 검색하면 표시되는 첫 번째 결과 중 하나는 error_reporting 함수 문서에 대한 링크입니다.
 

이 함수를 사용하면 PHP 스크립트 (또는 스크립트 모음)가 실행될 때 PHP 오류보고 수준을 설정하거나 PHP 구성에 정의 된대로 현재 수준의 PHP 오류보고를 검색 할 수 있습니다.
 

`error_reporting` 함수는 허용 할보고 수준을 나타내는 정수인 단일 매개 변수를받습니다.
 매개 변수로 아무것도 전달하지 않으면 현재 레벨 세트가 리턴됩니다.
 

매개 변수로 전달할 수있는 가능한 값의 긴 목록이 있지만 나중에 자세히 살펴 보겠습니다.
 

지금은 가능한 각 값이 PHP 사전 정의 상수로도 존재한다는 것을 아는 것이 중요합니다.
 예를 들어 상수`E_ERROR`의 값은 1입니다. 이는`1` 또는`E_ERROR`를 error_reporting 함수에 전달하여 동일한 결과를 얻을 수 있음을 의미합니다.
 

간단한 예로`php_error_test.php` PHP 스크립트 파일을 생성하면 현재 오류보고 레벨 세트를 볼 수있을뿐만 아니라 새 레벨로 설정할 수도 있습니다.
 

```undefined
<?php
// echo the current error reporting level
echo error_reporting();

```

```undefined
<?php
// report all Fatal run-time errors.
echo error_reporting(1);

```

## 오류보고 구성
 

이러한 방식으로`error_reporting` 함수를 사용하면 현재 작업중인 코드와 관련된 오류 만보고 싶을 때 유용합니다.
 

그러나 로컬 개발 환경에서보고되는 오류를 제어하고 코드를 작성할 때 검토 할 수 있도록 논리적 위치에 기록하는 것이 좋습니다.
 이는 PHP 초기화 (또는`php.ini`) 파일 내에서 수행 할 수 있습니다.
 

`php.ini` 파일은 PHP 동작의 모든 측면을 구성합니다.
 여기에서 PHP 스크립트에 할당 할 메모리 양, 허용 할 파일 업로드 크기, 환경에 대해 원하는`error_reporting` 수준 등을 설정할 수 있습니다.
 

`php.ini` 파일이 어디에 있는지 확실하지 않은 경우 알아내는 한 가지 방법은 phpinfo 함수를 사용하는 PHP 스크립트를 만드는 것입니다.
 이 함수는 PHP 설치와 관련된 모든 정보를 출력합니다.
 

```undefined
<?php
phpinfo();
```

내 phpinfo에서 볼 수 있듯이 현재`php.ini` 파일은`/ etc / php / 7.3 / apache2 / php.ini`에 있습니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/phpinfo.png)

`php.ini` 파일을 찾으면 원하는 편집기에서 열고 `오류 처리 및 로깅`섹션을 검색합니다.
 여기에서 재미가 시작됩니다!
 

## 오류보고 지시문
 

이 섹션에서 가장 먼저 보게되는 것은 모든 오류 수준 상수에 대한 자세한 설명이 포함 된 주석 섹션입니다.
 나중에 오류보고 수준을 설정하는 데 사용할 것이기 때문에 이것은 훌륭합니다.
 

다행히도 이러한 상수는 참조의 편의를 위해 온라인 PHP 매뉴얼에 문서화되어 있습니다.
 

이 목록 아래에는 공통 값의 두 번째 목록이 있습니다.
 여기에서는 기본값, 개발 환경에 대해 제안 된 값 및 프로덕션 환경에 대해 제안 된 값을 포함하여 일반적으로 사용되는 오류보고 값 조합 집합을 설정하는 방법을 보여줍니다.
 

```undefined
; Common Values:
;   E_ALL (Show all errors, warnings and notices including coding standards.)
;   E_ALL & ~E_NOTICE  (Show all errors, except for notices)
;   E_ALL & ~E_NOTICE & ~E_STRICT  (Show all errors, except for notices and coding standards warnings.)
;   E_COMPILE_ERROR|E_RECOVERABLE_ERROR|E_ERROR|E_CORE_ERROR  (Show only errors)
; Default Value: E_ALL & ~E_NOTICE & ~E_STRICT & ~E_DEPRECATED
; Development Value: E_ALL
; Production Value: E_ALL & ~E_DEPRECATED & ~E_STRICT
```

마지막으로, 모든 댓글의 맨 아래에는`error_reporting` 수준의 현재 값이 있습니다.
 로컬 개발의 경우 모든 오류를 볼 수 있도록 `E_ALL`로 설정하는 것이 좋습니다.
 

```undefined
error_reporting = E_ALL
```

이것은 일반적으로 새로운 개발 환경을 설정할 때 가장 먼저 설정 한 것 중 하나입니다.
 이렇게하면보고 된 모든 오류를 볼 수 있습니다.
 

error_reporting 지시문 뒤에 몇 가지 추가 지시문을 설정할 수 있습니다.
 이전과 마찬가지로 php.ini 파일에는 각 지시문에 대한 설명이 포함되어 있지만 아래에서 중요한 사항에 대해 간략하게 설명하겠습니다.
 

`display_errors` 지시문을 사용하면 PHP가 오류를 출력할지 여부를 전환 할 수 있습니다.
 일반적으로이 설정을 On으로 설정하여 오류가 발생하는 즉시 확인할 수 있습니다.
 

```undefined
display_errors=On
```

`display_startup_errors`는 PHP의 시작 시퀀스 중에 발생할 수있는 오류의 On / Off 토글 링을 허용합니다.
 이는 일반적으로 코드가 아닌 PHP 또는 웹 서버 구성의 오류입니다.
 문제를 디버깅하고 있고 원인이 확실하지 않은 경우이 옵션을 끄는 것이 좋습니다.
 

`log_errors` 지시문은 오류 로그 파일에 오류를 기록할지 여부를 PHP에 알려줍니다.
 기본적으로 항상 켜져 있으며 권장됩니다.
 

나머지 지시문은 기본값으로 둘 수 있습니다. 단,`error_log` 지시문을 제외하면`log_errors`가 켜져있는 경우 오류를 기록 할 위치를 지정할 수 있습니다.
 기본적으로 웹 서버가 오류를 기록하도록 정의한 모든 위치에 오류를 기록합니다.
 

## 사용자 지정 오류 로깅
 

Ubuntu에서 Apache 웹 서버를 사용하고 프로젝트 별 가상 호스트 구성은 다음을 사용하여 오류 로그의 위치를 결정합니다.
 

```undefined
ErrorLog ${APACHE_LOG_DIR}/project-error.log
```

즉,`project-error.log`라는 파일 아래의 기본 Apache 로그 디렉토리 인`/ var / log / apache2`에 기록됩니다.
 일반적으로`project`를 관련 웹 프로젝트의 이름으로 바꿉니다.
 

따라서 로컬 개발 환경에 따라 필요에 맞게 조정해야 할 수도 있습니다.
 또는 웹 서버 수준에서 변경할 수없는 경우`php.ini` 수준에서 특정 위치로 설정할 수 있습니다.
 

```undefined
error_log = /path/to/php.log
```

이것은 모든 PHP 오류를이 파일에 기록하고 이상적이지 않은 여러 프로젝트에서 작업하는 경우 주목할 가치가 있습니다.
 그러나 항상 하나의 파일에 오류가 있는지 확인하는 것이 더 효과적 일 수 있으므로 마일리지가 다를 수 있습니다.
 

## 해당 오류를 찾아 수정
 

최근에 PHP로 코딩을 시작했고 오류보고 기능을 사용하기로 결정했다면 기존 코드의 오류를 처리 할 준비를하십시오.
 예상하지 못한 몇 가지 사항이 표시되어 수정해야 할 수 있습니다.
 

그러나 장점은 이제 서버 수준에서이 모든 기능을 켜는 방법을 알고 있으므로 이러한 오류가 발생할 때이를 확인하고 다른 사람이보기 전에 처리 할 수 있다는 것입니다!
 

아, 그리고 궁금하다면이 게시물의 시작 부분에서 언급 한 오류는 상수 이름 주위에 따옴표를 추가하지 않고 상수를 잘못 정의했다는 사실과 관련이 있습니다.
 

```undefined
define(CONSTANT, 'Hello world.');
```

PHP는이를 허용했지만 (여전히 허용 할 수 있음) 알림을 트리거합니다.
 

```undefined
Notice: Use of undefined constant CONSTANT - assumed 'CONSTANT' 
```

이 알림은 내가 상수를 정의 할 때마다 트리거되었으며 해당 프로젝트의 경우 약 8 ~ 9 번이었습니다.
 누군가가 각 페이지 상단에 8 개 또는 9 개의 알림을 보는 것은 좋지 않습니다.
 