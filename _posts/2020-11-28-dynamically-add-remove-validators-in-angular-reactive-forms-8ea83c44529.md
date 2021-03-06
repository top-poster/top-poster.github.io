---
layout: post
title: "각도 반응형 형태의 검증기 동적 추가/제거"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![The Angular logo with the words “validating Reactive forms” and 3 bars, two with checkmarks and 1 with an X](https://miro.medium.com/max/2400/0*dI8ulj20RBlsrv4z.png)

Angular 애플리케이션을 개발하는 경우 양식 컨트롤에서 일부 검증자를 동적으로 추가하거나 제거해야 하는 상황이 발생할 수 있습니다. 예를 들어, 반응형 형식의 `카운티` 필드가 있고 사용자가 선택한 국가를 기준으로 필수 필드를 만들겠다고 가정해 보겠습니다. 이를 위해 선택한 국가를 기준으로 `카운티` 필드에서 `필수` 검증자를 추가 및 제거하는 것이 좋습니다.

글쎄요, 이런 시나리오에서는 Angular가 많은 도움을 줍니다. Angular에는 검증자를 조건부로 설정하는 데 사용할 수 있는 몇 가지 기능이 내장되어 있습니다. 반응형(reactive) 형태 덕분에 매우 쉽게 설정할 수 있습니다.

그럼 처음부터 시작해보죠.

나는 당신이 반응형식의 Angular 어플리케이션을 실행하기를 기대하고 있다. 저는 두 개의 필드를 가진 매우 단순한 형태를 가지고 있습니다. 하나는 `나라`를 위한 것이고 두 번째는 `카운티`를 위한 것이다.

나는 아래와 같이 템플릿과 해당 부품 코드를 가지고 있습니다.

우리가 여기서 무엇을 하고 있는지 차근차근 논의해 봅시다.

브라우저의 화면은 다음과 같습니다.

![a dialog box with Country A chosen and an empty dialog box labeled Required: false](https://miro.medium.com/max/790/0*NzKFNzZjUxB1hFim.png)

![a dialog box with Country Bchosen and an empty dialog box labeled Required: true](https://miro.medium.com/max/760/0*AuNZNcjaa66GJxJj.png)

그리고 그게 다야. Angular reactive 형태로 검증자를 추가하거나 제거하는 것이 매우 간단합니다.

# 기억해야 할 몇 가지 중요한 사항

1. `setValidators()` 메서드는 양식 컨트롤에서 이전/기본 검증자를 모두 제거합니다. 예를 들어, 양식 초기화 중에 `카운티`에 대해 `maxLength` 및 `minLength` 검증자를 설정한다고 가정합시다. 그러나 set Validators([Validators.required])가 실행되면 County에서 maxLength와 minLength를 제거하고 required(필수) 검증자만 설정합니다.

따라서 maxLength와 minLength를 모두 가지려면 다음과 같은 작업을 수행하십시오.

```js
검증자 설정([Validators.required, Validators.maxLength(10), 검증자.minLength(5)])
```

2. 검증자를 업데이트(추가 또는 제거)한 후에는 항상 `updateValueAndValidity()` 메서드를 사용하십시오. 컨트롤과 관련된 모든 변경 사항은 이 문, 즉 `updateValueAndValidity()`를 사용하는 경우에만 반영되기 때문입니다.

3. 유감스럽게도 Angular는 양식 제어에서 하나의 검증자만 제거할 수 있는 방법을 제공하지 않습니다. clear validators() 방법이 있지만 모든 validators가 제거됩니다.

이를 위해, 우리는 두 가지를 할 수 있습니다. 모든 검증자를 제거하는 데 `clear Validators()`를 사용하고 필요한 검증을 설정하는 데 `set Validators()`를 사용하거나, 필요한 검증자만(사용하지 않으려는 검증자 없이)`를 직접 사용할 수 있습니다.

이 기사는 여기까지입니다. 이게 도움이 됐으면 좋겠어요.

# 참고문헌

마지막 코드는 GitHub에서 사용할 수 있습니다.

읽어주셔서 감사합니다!