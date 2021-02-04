---
layout: post
title: "Angular 11 : 업그레이드시 예상되는 사항
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/angular-white.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/angular-white.png?fit=750%2C487&ssl=1)

## 소개
 

Angular는 웹 애플리케이션, 모바일 애플리케이션 및 데스크톱 애플리케이션을 빌드하기위한 Google의 TypeScript 기반 프레임 워크입니다.
 Google은 GitHub 문제 추적기에서 Angular 개발자 및 회사가 제기 한 문제를 검토 한 후 2020 년 11 월 14 일에 새로운 Angular 11 업데이트를 출시했습니다.
 

그래서 그들은 커뮤니티에서 제기 한 문제를 해결함으로써 개발자 경험을 개선하는 데 초점을 맞추고있는 다른 버전의 Angular를 출시하기로 결론을 내 렸습니다.
 

Angular 11의 릴리스는 단순히 개발 프로세스에 초점을 맞춘 몇 가지 개선 사항을 제공했습니다.
 또한 개발 측면에서 모든 Angular 사용자를 위해 일을 더 쉽게 만들었습니다.
 Angular 11은 인라인 글꼴과 TypeScript 4.0 지원을 통해 더 빠른 빌드 시스템을위한 최적화를 제공합니다.
 

CLI 및 구성 요소를 사용하여 프레임 워크로 작업 할 때 Angular 11이 플랫폼에 가져온 업데이트를 살펴 보겠습니다.
 또한 Angular가 출시 된 이후 변경 한 사항도 다룰 것입니다.
 

## Angular 11의 새로운 기능 및 변경 사항
 

### 구성 요소 테스트 장치
 

구성 요소 테스트 하네스는 Angular 버전 9의 릴리스와 함께 Angular에 추가 된 새로운 기능입니다. Angular 재료 구성 요소를 테스트하는 데 사용되었으며 재료 구성 요소를 테스트하는 동안 개발자를 돕기 위해 견고하고 읽기 쉬운 API 표면을 제공합니다.
 

구성 요소 테스트 하네스는 또한 개발자가 지원되는 API를 사용하여 Angular 재료 구성 요소와 상호 작용할 수있는 방법을 제공합니다.
 

새로운 Angular의 출시로 구성 요소 테스트 장치가 일부 업그레이드되었습니다.
 이제 개발자는 모든 구성 요소에서 구성 요소 테스트 장치를 사용할 수있는 자유를 얻었습니다.
 개발자는 더욱 강력한 테스트 스위트를 만들 수도 있습니다.
 

병렬 기능, 성능 향상 및 새로운 API는 Angular 11이 릴리스와 함께 가져온 일부 업데이트입니다.
 병렬 함수는 테스트에서 비동기 작업을 지원하므로 개발자가 구성 요소와 여러 비동기 상호 작용을 병렬로 수행 할 수 있습니다.
 

또한 수동 변경 감지를 통해 개발자는 단위 테스트에서 자동 변경 감지를 비활성화하고 변경 감지에 대한보다 세밀한 제어에 액세스 할 수 있습니다.
 

## TypeScript 4.0 지원
 

새로운 Angular 11은 버전 3.9에서 TypeScript에 대한 지원을 업그레이드했습니다.
 이제 Angular는 TypeScript 4.0도 지원합니다.
 이 업데이트의 목표는 빌드 속도를 개선하는 것입니다.
 따라서 새로운 Angular는 이전 버전에서 빌드 시스템 속도를 향상시키고 ngcc의 속도도 업데이트합니다.
 

## 웹팩 5 지원
 

webpack은 개발자가 더 큰 JavaScript 모듈을 컴파일 할 수있는 도구입니다.
 모듈 번 들러라고도합니다.
 많은 수의 파일을 컴파일하고 앱을 실행할 수있는 단일 파일을 생성합니다.
 새로운 웹팩 5는 지난달에 출시되었지만 사용하기에 완전히 안정적이지 않습니다.
 새로운 Angular 11은 최신 릴리스의 웹팩을 지원합니다.
 새로운 웹팩을 지원하는 이유는 Angular가 완전히 안정적 일 때 영구 디스크 캐싱과 작은 번들로 더 빠른 빌드를 달성하기를 원하기 때문입니다.
 

이를 실험하기 위해`package.json` 파일에 아래 코드 줄을 추가하거나`Yarn`을 사용할 수 있습니다.
 `npm`은 현재 해상도 속성을 지원하지 않습니다.
 

```bash
"resolutions": {
    "webpack": "5.4.0"
}
```

## ESLint로 이동
 

Angular는 항상 기본적으로 TSLint를 사용하여 Linting을 구현했으며 새 버전이 출시되기 전에 가장 인기있는 Linting 도구 중 하나였습니다.
 새 버전은 인기있는 TSLint가 현재 가치가 하락하고 있으므로 ESLint를 사용합니다.
 이러한 이유로 TSlint Linting에 대한 Angular 구현은 더 이상 사용할 수 없으며 Linting 용도로 TSLint를 사용할 수 없습니다.
 

이 업그레이드는 James Henry와 함께 Angular 커뮤니티 회원의 도움으로 이루어졌습니다.
 그들은 typescript-eslint, angular-eslint 및 tslint-to-eslint-config로 구축 된 타사 마이그레이션 경로를 개발했습니다.
 

마이그레이션하려면 프로젝트 종속성에 아래 코드를 추가하세요.
 

```css
ng add @angular-eslint/schematics
```

프로젝트에서 아래 회로도를 실행하십시오.
 

```coffeescript
ng g @angular-eslint/schematics:convert-tslint-to-eslint {YOUR_PROJECT_NAME_GOES_HERE}
```

마지막으로, ESLint 설정이 끝나면`tslint.json`을 삭제하고 프로젝트에서 TSLint를 제거합니다.
 

### 업데이트 된 HMR (Hot Module Replacement) 지원
 

핫 모듈 교체는 전체 브라우저를 새로 고치지 않고도 모듈을 교체 할 수있는 메커니즘입니다.
 Angular 개발자에게는 오래된 개념이며 Angular 11의 릴리스는 HMR을 구성하는 데 필요한 노력을 줄였습니다.
 Angular 11을 사용하면 CLI가`ng serve`를 사용하여 애플리케이션을 시작하는 동안 HMR을 활성화 할 수 있습니다.
 

시작하려면 터미널에서 아래 명령을 실행하십시오.
 

```undefined
ng serve --hmr
```

위의 명령을 실행 한 후 로컬 서버가 실행되기 시작하면 콘솔을 엽니 다.
 HMR이 현재 활성화되었음을 확인하는 메시지가 표시됩니다.
 

### 업데이트 된 언어 서비스 미리보기
 

Angular 11이 출시되기 전에 Angular는 항상 뷰 엔진 서비스를 기반으로했습니다.
 현재 버전의 Angular에서도 지원합니다.
 이 언어 서비스는 Angular가 Angular를 재미있게 만드는 데 도움이되는 도구를 제공하는 데 사용됩니다.
 

Angular 11의 출시로 곧 출시 될 새로운 언어 서비스가 도입되었습니다.
 이 서비스는 아직 개발 단계에 있지만 Ivy 기반 언어 서비스를 기반으로합니다.
 

새로운 서비스를 통해 개발자는 더 나은 엔진 및 렌더러보기에서 어떻게 작동하는지 미리 볼 수 있습니다.
 또한 언어는 TypeScript 컴파일러처럼 템플릿 내에서 제네릭 유형을 올바르게 추론 할 수 있습니다.
 

### 자동 글꼴 인라인
 

Angular 11이 출시되면서 자동 글꼴 인라인이 도입되었습니다.
 Angular 11 CLI는 응용 프로그램에서 사용 및 연결되고 컴파일 할 때 최적화되는 인라인 글꼴을 다운로드합니다.
 인라인 글꼴의 최적화로 이제 앱의 속도가 향상되었습니다.
 

이러한 기능은 Angular 11의 프로덕션 구성에 대해 기본적으로 활성화됩니다.
 

### 향상된 로깅 및보고
 

Angular 11의 릴리스는 로그와 보고서를 이전 버전보다 더 쉽게 읽고 이해할 수 있도록함으로써 개발 중에보고하는 빌더 단계 정보를보다 사용자 친화적으로 만들었습니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/code-snippet.png?resize=730%2C427&ssl=1)

### 버전 11로 업데이트
 

준비가되어 있고 이미 시스템에 설치된 Angular의 이전 버전을 새 버전으로 업데이트하려면 아래 명령을 실행하여 업데이트 할 수 있습니다.
 

```undefined
ng update @angular/cli @angular/core
```

## 결론
 

이 기사에서는 Angular 11의 새로운 기능, 이점 및 새 릴리스에서 수정 된 버그를 살펴 보았습니다.
 새로운 기능과 업데이트를 살펴보면 Angular 11이 많은 흥미로운 기능과 함께 제공되는 매우 큰 릴리스임을 알 수 있습니다.
 특히 더 빠른 빌드와 인라인 글꼴.
 이러한 기능이 가장 흥미 롭습니다.
 

더 많은 업데이트 정보를 보려면 언제든지 update.angular.io를 방문하여 자세한 내용을 확인할 수 있습니다.
 