---
layout: post
title: "각도에서 AOT 컴파일에 대한 간략한 소개"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![old-fashioned alarm clock pointing to seven a.m.](https://miro.medium.com/max/11732/0*oJUyARXAtnDEPY-z)

# 도입

Angular 코드는 주로 HTML 템플릿과 TypeScript 컴포넌트로 구성되지만, Angular 애플리케이션이 브라우저에서 실행되려면 자바스크립트 코드에 컴파일되어야 한다.

Angular 코드를 JavaScript로 컴파일하는 방법에는 크게 두 가지가 있다.

Angular 버전 9부터 기본 컴파일은 AOT입니다. 즉, `ngbuild --prod` 명령으로 Angular 애플리케이션을 제작할 때마다 애플리케이션도 빌드 프로세스의 일부로 컴파일됩니다.

참고: Angular 버전 8 이상에서는 `ngbuild --prod --aot=true` 명령을 사용하여 AOT=true` 명령을 사용하십시오.

# AOT 컴파일의 장점

이 단계에서는 AOT가 제공하는 것이 무엇인지 궁금할 수 있습니다. 다음은 JIT 컴파일에 비해 AOT 컴파일의 몇 가지 장점입니다.

# AOT 작동 방식

Angular Ivy는 Angular 버전 9 이상의 새로운 컴파일 및 렌더링 파이프라인이다. Angular Ivy는 이전 View Engine에 비해 매우 빠르고 효율적입니다.

컴파일 중에 발생할 주요 일은 트리 흔들기, 번들링, ugulation 및 코드 축소입니다. 또한 컴파일러는 Angular specific decorator, 생성자 파라미터 및 사용되지 않은 코드를 제거한다.

컴파일은 다음과 같이 세 가지 주요 단계로 이루어집니다.

## 1. 코드분석

여기서 컴파일러는 `@Component() 및 `@Input()와 같은 각도별 메타데이터를 분석합니다. 메타데이터는 Angular가 애플리케이션의 인스턴스를 구성하는 데 사용하는 필수 정보(예: 구성 요소를 생성하고 이를 시각적으로 표현하는 방법)를 제공합니다. Angular는 `.metadata.json` 파일 내에 있는 장식자 메타데이터의 전체 구조를 나타냅니다.

이 단계에서는 메타데이터 구문 위반 오류도 탐지 및 기록됩니다.

코드 분석 단계의 주요 출력 중 하나는 확장자가 `.d.ts`인 형식 정의 파일이다. AOT 컴파일러는 이 파일들을 사용하여 응용 프로그램 코드를 생성한다. 샘플 `.d.ts` 파일은 다음과 같습니다.

![Image for post](https://miro.medium.com/max/1600/1*533MOpza2n62RROmKRFbXQ.png)

## 2. 코드 생성

컴파일러는 두 번째 단계의 컴파일러에서 위의 1단계에서 생성된 .metadata.json 파일의 출력을 해석한다. 또한 메타데이터의 의미론이 컴파일러 규칙을 준수하는지 여부를 점검한다.

메타데이터 다시 쓰기는 이 단계에서 발생하는 또 다른 중요한 단계입니다. 예를 들어, 화살표 함수가 메타데이터 표현식에서 발견된다면, 코드 생성 단계는 그 함수를 컴파일러에 더 친숙한 형태로 다시 작성할 것이다.

## 3. 템플릿 유형 확인

컴파일의 마지막 단계는 Angular 템플릿, 즉 HTML 코드를 포함하는 파일과 많은 관련이 있다. 이 단계에서 컴파일러는 런타임에 충돌이 발생하지 않도록 식을 유형 검사한다.

Angular 컴파일러는 또한 TypeScript 컴파일러를 사용하여 템플릿의 바인딩 식을 검증한다. 유형 오류가 감지되면 템플릿 유효성 검사에서 적절한 오류 메시지가 생성됩니다.

참고: Angular Ivy의 경우 템플릿 확인기가 이전 View Engine보다 약간 엄격합니다. 따라서 View Engine에서 컴파일되는 형식 확인 오류가 있는 일부 템플릿은 Angular Ivy에서 그렇지 않을 수 있습니다.

# AOT의 사소한 단점

AOT 컴파일이 제공하는 장점은 단점보다 훨씬 크다. 하지만, AOT와 관련된 몇 가지 사소한 문제들을 아는 것은 좋습니다.

# 결론

Angular Ivy를 사용한 AOT 컴파일은 매우 빠르고 개발자의 워크플로우에 많은 이점을 제공합니다.

마지막으로, AOT는 강력한 보안 및 린 애플리케이션으로 이어지는 생산을 위한 Angular 코드를 효율적으로 컴파일하는 방법을 제공합니다.