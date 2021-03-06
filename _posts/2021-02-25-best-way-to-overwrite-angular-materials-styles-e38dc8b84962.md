---
layout: post
title: "각진 재료 스타일을 덮어쓰는 3가지 올바른 방법"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![Various fabrics](https://miro.medium.com/max/10262/0*bTUq0wJv-Dye6ipm)

각 프로젝트의 초기 단계에서 모든 프런트 엔드 개발자는 항상 UI 구성 요소 라이브러리를 사용할지 여부에 대한 문제에 직면합니다.

오늘날, 우리의 개발 과정을 개선하고 촉진하는 많은 UI 라이브러리를 시중에서 이용할 수 있다. 가장 인기 있는 것들 중에서, 우리는 Angular Material, Ngxbootstrap, Ngbootstrap, PrimeNG, Semantic UI 등을 식별할 수 있다.

어느 날, 저는 Google 공장에서 바로 Angular Material(즉, Angular Material) 프로젝트를 위해 라이브러리를 사용하기로 결정했습니다. 입력, 확장 패널, 대화, 탭 등 프로젝트에 많은 구성 요소를 사용해야 했습니다.

불행히도 한 가지 단점이 있었다. UI 디자이너가 하이파이 모의고사에서 준비한 부품의 외관은 앵글머티리얼이 제공하는 것과는 조금 달랐다. 그러나, 이것은 경험이 풍부한 프런트 엔드 개발자에게 분명히 어려운 일이 아니다. 결국, 고객이 원하는 구성 요소의 모양과 동작을 만들기 위해 여기에 있는 것입니다.

하지만 AM 도서관은 그렇게 쉽지가 않아요.

이 문서에서는 단순 AM 탭 구성 요소의 일부 스타일을 덮어씁니다. 탭 표시기의 `배경색`을 변경해 보겠습니다.

![Angular Material Tabs component](https://miro.medium.com/max/2468/1*h9NAbGL22PrCKJIWds2Plw.png)

먼저 "배경색"을 담당하는 스타일을 포함하는 DOM 요소를 검사합니다.

![Tab indicator background colour — inspect mode](https://miro.medium.com/max/3684/1*zWPOrAe3Lv5l41qDlXE8NA.png)

구성 요소의 SCSS 파일에서 일반적으로와 같이 스타일을 덮어씁니다.

```js
//component.scss
.mat-tab-group.mat-primary.mat-ink-bar {
배경색: 빨간색;
}
```

아래와 같이 배경색은 변하지 않았다. 이게 왜 그럴까?

답은 간단합니다. Angular는 View Encapsulation 모드를 사용합니다. 이 모드는 각 DOM 요소에 추가 속성(`_ng-content-***-**`)을 연결하여 전체 SCSS 코드를 구성 요소의 고유 속성에 묶습니다.

보다 쉽게 하려면 기본 HTML 단추를 사용합니다.

```js
app.component.component
<버튼>예 버튼</버튼>
```

스타일링:

```js
//app.component.scss
{버튼
배경색: 빨간색;
}
```

검사 모드에서 확인할 수 있는 내용:

![Button background colour — inspect mode](https://miro.medium.com/max/2368/1*eySib-UoTiZULyqKp583_A.png)

고유한 특성이 보기 캡슐화 모드에 의해 자동으로 추가되었습니다. 탭이 있는 예제로 돌아가 보면 AM이 항상 각 DOM 요소에 특수 속성을 추가하는 것은 아니라는 것을 알 수 있습니다.

![Tab indicator — inspect mode](https://miro.medium.com/max/2412/1*66frRSAePwYuYNPsVoSG0w.png)

구성 요소를 범위 모드로 스타일링하려고 하면 브라우저가 다음과 같이 스타일을 읽기 때문에 결과가 표시되지 않습니다.

```js
.mat-tab-group.mat-primary[_ng-content-***-***].mat-ink-bar {
배경색: 빨간색;
}
// -**-***는 고유 번호입니다.
```

# 그렇다면 AM 스타일을 덮어쓰는 올바른 방법은 무엇일까요?

웹에는 AM 스타일을 덮어쓰는 방법에 대한 많은 예가 있지만 이러한 방법은 사용하기 위험하거나 더 이상 사용되지 않습니다.

## 1. View 캡슐화 모드 해제(고유 특성 제거)

가장 일반적인 대답은 보기 캡슐화 모드를 끄는 것입니다.

이걸 어떻게 달성할 수 있을까요?

![Turning off view encapsulation mode](https://miro.medium.com/max/2816/1*1ifr8UOtT0J6p-VQOgfz2Q.png)

"@각선/코어"에서 "ViewEncapsulation"을 가져오고 "@Component" 메타데이터 내에서 "캡슐레이션: 캡슐화를 봅니다.없습니다.

앞으로는 구성 요소의 DOM 요소에 고유한 특성이 없으며 스타일이 글로벌해질 것입니다.

![Tab indicator background colour — inspect mode](https://miro.medium.com/max/2652/1*9e8B9AAIGr3d79cwz5Y3Zg.png)

작동하지만, 이제 나머지 애플리케이션에 영향을 미치기 때문에 상당히 위험합니다. 앞에서 설명한 격리, 범위 지정 규칙 및 고유 특성이 더 이상 작동하지 않습니다. 모든 탭 구성 요소가 이 규칙을 적용합니다. 우리는 이렇게 하고 싶지 않아요.

## 2.:호스트

사용되지 않는 의사 클래스:ng-deep를 구현하고 특수 선택기:host를 덮어쓰는 것도 매우 자주 나타나는 대답이다.

한 번 해 보죠.

![Tab indicator background color — inspect mode](https://miro.medium.com/max/2548/1*Jpxz1lHfunBPpugehiDytQ.png)

이게 어찌된 일이냐?

오, 됐다! 그러나 잠시만요… Angular 웹 사이트에 다음과 같은 정보가 있습니다.

위의 예제와 같이 이 솔루션은 보기 캡슐화를 해제하므로 스타일이 글로벌 상태가 됩니다.

다른 하나는 Angular 11의 ":ng-deep"을 사용하고 있지만, 향후 Angular 버전에서 제거할 수도 있습니다. 시간이 지나면 클라이언트가 프로젝트를 최신 Angular 버전으로 업데이트할 수 있으며 사용되지 않는 이 의사 선택기를 사용하는 모든 스타일을 덮어쓰기가 어렵습니다.

## 3. AM 스타일을 별도의 글로벌 스타일로 덮어씁니다. 범위가 지정되지 않습니다!

심층 분석 후, 어떠한 위험이나 더 이상 사용되지 않는 방법 없이 적절한 SCSS 구조로 AM 스타일을 덮어쓰는 방법을 발견했다.

각 복잡한 프로젝트에서 SAS 7-1 패턴을 사용하여 SCSS 파일을 정렬합니다.

```js
| — scss
| — 베이스
| — 구성요소
| — 암-성분
| — 레이아웃
| — 페이지
| - 테마
| — utils
| — 공급업체
```

보시는 것처럼 am-components라는 폴더가 am-components 폴더에 am-components가 추가되었습니다. 여기에 AM 구성 요소만 담당하는 `.scss` 파일을 모두 저장합니다.

이제 다음 항목을 생성해 보겠습니다.

![SCSS structure](https://miro.medium.com/max/1260/1*3T7OIy13x_6tpJO1eaPG_Q.png)

주 `style.scss` 파일로 가져온 `_ab-tabs.scss` 파일을 만들었습니다.

```js
//style.scss
@가져오기 "scss/contents/am-contents/am-contents";
```

그러면 AM 탭 구성 요소 스타일을 어떻게 덮어쓸 수 있을까요?

Angular Material은 구성 요소를 쉽게 재정의할 수 있도록 가능한 최소 사양의 선택기를 사용합니다. 더 구체적인 스타일이 덜 구체적인 스타일보다 우선합니다. 따라서 우리의 경우, 우리는 AM 스타일을 덮어쓰려면 더 많은 특수성을 추가해야 합니다. 가장 좋은 방법은 구성 요소의 셀렉터 이름을 사용하는 것입니다.

![Tab component- inspect mode](https://miro.medium.com/max/2460/1*YbO5TWXoL8vmMJCphs9d9Q.png)

```js
//_am-discs.scss
매트 탭 그룹 {
```

현재, 우리는 `:ng-deep` 의사 선택자와 같은 결과를 얻었지만, 이것은 결코 비난받지 않을 것이다!

다른 탭을 다르게 스타일링하려면 어떻게 해야 합니까?

번거로움 없이 구성 요소에 고유한 클래스를 추가합니다.

![HTML of tab component](https://miro.medium.com/max/1872/1*lPYKQ3fhLxeoHOYAHGnWew.png)

이제 `.my-tab-class`를 사용하는 AM 탭 구성 요소에는 녹색 표시등이 있습니다.

![Tab indicator background color — inspect mode](https://miro.medium.com/max/2728/1*nODczzsJHGLNza-U5k4aNw.png)

# 결론

보시다시피, 우리의 AM 스타일을 덮어쓸 수 있는 많은 옵션들이 있습니다. 불행히도 이들 중 대부분은 프로젝트에 몇 가지 문제를 일으킬 수 있습니다(예: `:ng-deep` 또는 뷰 캡슐화 모드 해제). 구성 요소를 아름답게 만드는 가장 좋은 방법은 7-1과 같이 견고하고 선명한 패턴의 고전적인 글로벌 스타일을 사용하고 보다 구체적인 스타일로 덮어쓰는 것입니다.