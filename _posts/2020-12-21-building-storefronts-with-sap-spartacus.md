---
layout: post
title: "SAP Spartacus를 통한 스토어 프런트 구축"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/Spartacus.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/Spartacus.png?fit=730%2C487&ssl=1)

스파르타쿠스는 SAP Commerce Cloud를 위한 온라인 전자상거래 스토어 프런트 구축을 위한 경량 오픈 소스 프레임워크이다. 스파르타쿠스는 자바스크립트와 앵글을 기반으로 하며 라이브러리로 출판된다. 개발자는 스파르타쿠스를 기존 애플리케이션으로 가져와 스토어 프런트용 툴이 즉시 소진되도록 할 수 있다.

이 기사에서는 스파르타쿠스의 몇 가지 기능을 소개하고, 스토어 프런트 애플리케이션을 시작하는 방법에 대해 설명하며, 몇 가지 팁과 요령에 대해 다룹니다.

## 스파르타쿠스의 특징

스파르타쿠스를 활용하는 두 가지 주요 이점은 확장성과 업그레이드 능력입니다. 오픈 소스 라이브러리는 약 2주에 한 번씩 자주 업데이트되므로 개발자는 업그레이드와 새로운 기능을 빠르게 활용할 수 있습니다. 스파르타쿠스는 순수하게 SAP Commerce Cloud에 대한 REST API 호출을 통해 작동하므로 모든 백엔드 아키텍처와 독립적으로 스토어프론트가 실행됩니다.

스파르타쿠스 프레임워크는 진행형 웹 애플리케이션(PWA)을 지원한다. PWA는 고객이 추가적인 프런트 엔드 기능을 프로그래밍하지 않고도 모바일 및 데스크톱 플랫폼에서 스토어 프런트를 활용할 수 있도록 지원하는 대응형 설계 요소를 통합합니다.

스파르타쿠스는 다양한 컨텐츠 관리 시스템(CMS)과 잘 연동되며, 개인화된 경험을 제공할 수 있도록 전폭적으로 지원된다. 스파르타쿠스는 전자상거래 매장 정면을 위해 설계되었으며 스마트편집과 쉽게 통합된다. SmartEdit에는 스파르타쿠스 사이트가 고객에게 나타날 것을 미리 볼 수 있는 기능이 포함되어 있습니다.

## 종속성

스파르타쿠스는 프런트 엔드의 Angular 플랫폼에 의해 지지된다. 또한 yarn과 Node.js를 사용합니다.

다른 버전의 스파르타쿠스는 Angular, Node.js 및 yarn의 특정 버전이 필요하므로 요구 사항을 확인하는 것이 가장 좋습니다.

백엔드에서 Spartacus는 SAP Commerce Cloud와 인터페이스합니다. SAP Commerce Cloud의 버전 2005가 권장되지만 Spartacus는 버전 1905 이상에서 작동합니다.

## 시작 중

스파르타쿠스를 설치하려면 라이브러리를 기존 JavaScript 또는 Angular 웹 응용 프로그램으로 가져오십시오. 아래 예제에서는 스키마를 사용하여 기존 Angular 프로젝트에 스파르타쿠스를 설치하는 방법에 대해 설명합니다.

### 전제조건

아래 워크플로의 경우 다음 필수 구성 요소가 필요합니다.

- Angular CLI 버전 9.1 이상 10.0 미만
- 각도 9 이상 10.0 미만
- 노드 버전이 13보다 작거나 같습니다.

## 워크플로우 및 예제

### Spartacus 라이브러리 추가

Spartacus 라이브러리를 Angular 프로젝트로 가져오려면 명령 프롬프트를 열고 디렉토리를 프로젝트 디렉토리로 변경합니다. 여기에서 프롬프트에 다음 명령을 입력합니다.

```css
ng add @spartacus/schematics
```

이 명령은 기본 구성을 추가하지만 사용자 지정을 위해 확장할 수 있습니다.

예를 들어 `baseUrl`에 대한 매개 변수를 추가하면 CX OCC 백엔드의 기본 URL이 설정됩니다. 통화 또는 언어와 같은 다른 매개변수는 스토어프론트의 기본값을 설정합니다. 사용 가능한 매개 변수의 전체 목록은 스파르타쿠스 문서와 함께 제공됩니다.

기본 명령을 매개 변수 이상으로 확장하여 응용 프로그램을 확장할 수 있습니다. 다음은 몇 가지 예입니다.

- `ng @twascus/schematics:add-thead` (서버 측 렌더링 구성)
- `ng @spartacus/schematics:add-pwa` (Spartacus용으로 사용자 정의된 PWA 모듈 포함)
- `ng @spartacus/schematics:add-cms-component`(CMS 구성 요소를 생성하고 구성 요소를 추가합니다.)

### 뒤에서 무슨 일이 일어나고 있는지

기본 명령이 실행되면 프로젝트에서 스파르타쿠스를 설정하기 위한 몇 가지 단계가 백그라운드에서 수행됩니다.

- 필요한 종속성이 프로젝트에 추가됩니다.
- 스파르타쿠스 모듈을 `app.module`로 가져오고 기본 구성이 설정됨
- 스파르타쿠스 스타일을 main.scss로 가져오기
- `cx-storefront` 구성 요소가 `app.component`에 추가됨

PWA 또는 SSR와 같은 매개 변수나 플래그를 추가하는 경우 추가 단계가 발생합니다(예: 서버 측 렌더링 종속성 및 구성에 필요한 추가 파일 추가).

### CMS 구성 요소 설정

새로운 전자상거래 사이트를 설정하는 가장 중요한 단계 중 하나는 인벤토리를 포함하는 것이므로 스파르타쿠스 체계는 CMS 구성 요소 추가를 지원한다.

`add-cms-component`는 거의 모든 CMS 설정을 처리하지만, 이 중 대부분은 CMDB 구성 요소 데이터와 관련된 특정 사용자 지정도 사용할 수 있습니다.

- `--CmsModule 선언`: 새 CMS 구성 요소가 추가된 위치를 식별합니다. 기존 모듈을 지정하지 않으면 새 모듈을 생성합니다.
- `--cmsComponentData`(또한 `--cms`): 기본값은 `true`입니다. 이 옵션은 새 구성 요소에 `CmsComponentData`를 추가합니다.
- `--cmsComponentDataModel`(`--cms-model`도 해당): `CmsComponentData`의 모델 클래스를 식별합니다. 이 인수는 `--cmsComponentData`가 `true`로 설정된 경우 필요합니다.
- `--cmsComponentDataModelPath`(`--cms-model-path`도 해당): `CmsComponentData`를 가져올 경로를 지정합니다. 기본값은 `@spartacus/core`로 설정됩니다.

다음은 `add-cms-component`를 `--declareCmsModule`과 함께 사용하는 예입니다.

```undefined
ng g @spartacus/schematics:add-cms-component mySiteCms
--cms-model=MySiteModel --module=app
--declareCmsModule=my-site-cms-path/my-site-cms
```

위의 명령문은 필요한 `component.ts` 파일을 생성하고 `my-site-cms-path/my-site-cms.module.ts`의 사용자 지정 모듈 위치에 파일을 추가합니다. my-site-cms.module.ts도 app.module.ts로 불러온다.

사이트가 설정된 후에도 CMS는 다양한 JavaScript, Angular 및 CSS 변경을 통해 맞춤형으로 사용자 정의할 수 있습니다. 스파르타쿠스 설명서에는 CMS 구성 요소 사용자 지정에 대한 추가 정보가 포함되어 있다.

### 스파르타쿠스 사용자 정의

#### 페이지 사용자 정의

스파르타쿠스는 단일 페이지 애플리케이션(SPA)임에도 불구하고 페이지를 지원한다. 스파르타쿠스의 페이지는 CMS에 의해 제어되므로 페이지에 대한 추가 또는 편집은 CMS 측에서 이루어집니다.

#### 배너

스파르타쿠스는 배너 구성요소를 활용하여 CMS에서 생성된 배너를 렌더링한다. 배너는 하나 이상의 이미지를 가질 수 있으며 헤더와 같은 선택적 콘텐츠를 포함할 수 있다. 추가 콘텐츠를 연결하는 데 사용되는 경우가 많습니다.

배너는 다음과 같이 스파르타쿠스로 매핑할 수 있습니다.

```css
    SimpleResponsiveBannerComponent: {
      component: BannerComponent
    },
    BannerComponent: {
      component: BannerComponent
    },
    SimpleBannerComponent: {
      component: BannerComponent
    }
  }
}
```

#### 검색 상자

검색 상자 구성 요소를 추가하면 사용자 검색에 대한 두 가지 중요한 기능, 즉 페이지를 벗어나지 않고 제품 카탈로그 자동 완성 및 검색을 수행할 수 있습니다. 검색 상자 구성 요소는 다음과 같이 JavaScript 구성 요소에 추가됩니다.

```css
        SearchBoxComponent: {
          component: SearchBoxComponent
        }
    }
}
```

검색 기능을 사용자 정의하기 위해 편집할 수 있습니다.

## 요령과 요령

### 레시피 모듈

스파르타쿠스는 기본 구현을 제공하는 `src/app/app.module.ts`에 레시피 모듈을 설정합니다. 현재 B2cStorefront Module이 가장 인기 있는 레시피 모듈입니다. 그러나 보다 심층적인 사용자 정의를 위해 추가적인 유연성이 필요한 경우 Storefront Module 또는 Storefront Foundation Module과 같은 다른 Recipe 모듈을 사용하는 것이 귀하의 사이트에 더 적합할 수 있습니다. 이러한 정보는 귀하의 사이트에 가장 적합한지 조사할 가치가 있습니다.

### 라이브러리 참조

추가 사용자 지정을 위해 스파르타쿠스 라이브러리를 참조하는 것이 좋다. 프로젝트에 스파르타쿠스 소스 코드를 복사하여 붙여넣는 것이 바람직할 수 있지만, CMS 컨텐츠 편집과 같이 스토어를 보다 깔끔하게 사용자 정의할 수 있는 다른 방법이 있습니다.

## 결론

스파르타쿠스(Spartacus)는 SAP Commerce Cloud를 사용하는 상점용 오픈 소스 라이브러리이다. 이 기사에서는 스파르타쿠스의 몇 가지 기본 기능뿐만 아니라 기존 웹 사이트에 통합하는 방법에 대해서도 다루었다. CMS, 배너 및 검색 기능을 위해 스파르타쿠스를 사용자 지정하는 방법에 대한 몇 가지 예를 다루었다. 우리는 또한 스파르타쿠스와 함께 일하기 위한 몇 가지 요령과 요령을 다루었다.

스파르타쿠스 사용 및 상점 앞 맞춤화에 대한 자세한 내용은 스파르타쿠스 공식 설명서를 참조하십시오.