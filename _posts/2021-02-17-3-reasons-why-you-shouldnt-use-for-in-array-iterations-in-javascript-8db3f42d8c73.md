---
layout: post
title: "JavaScript에서 'for…in' 어레이 반복을 사용하면 안 되는 3가지 이유"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![Woman holding out her hand as a stop signal](https://miro.medium.com/max/7896/0*23rWzVy1qcQTyfCO)

어레이를 반복하려면 다음 코드를 사용해야 하는 이유는 무엇입니까?

몇 가지 이유가 있습니다. 하나는 이러한 목적을 위해 "for… of" 성명서(그리고 "for…" 방법 및 고전적인 "for" 루프)가 공식적으로 권장된다는 것이다. 그러나 어레이 반복에 `for…in`을 사용하는 데 문제가 있는 것은 무엇인가?

이 질문에 답하기 위해서는 "for…in"이 실제로 무엇을 하는지 알아야 한다. MDN Web Docs는 이에 대해 다음과 같이 말합니다.

열거형 속성에 `true`인 내부 열거형 플래그가 있습니다. 예를 들어 단순 할당을 통해 생성된 속성의 경우가 이에 해당합니다. 속성을 열거할 수 있는 기본 설정에 대한 자세한 내용은 이 MDN 항목을 참조하십시오.

# 문제 #1: 정의되지 않은 주문

첫 번째 문제는 반복 순서가 구현에 따라 달라지기 때문에 요소가 특정 순서로 반복된다는 보장이 없다는 것이다. 일반적으로 어레이 반복에 대해 추론할 때, 침묵적 가정은 반복이 지정된 순서대로 발생한다는 것입니다(일반적으로 첫 번째 항목에서 마지막 또는 반대 방향으로).

# 문제 #2: 확장 어레이

JavaScript에서 배열의 현재 크기를 초과하는 인덱스에 할당할 경우 배열이 자동으로 확장됩니다. letar = [`a`, `b`, `c`]` 배열이 세 가지 요소로 구성되어 있으며 인덱스 5에서 새 값을 할당한다고 가정합니다. arr[5] = `d`; JavaScript는 `정의되지 않음`으로 공백을 메웁니다. 따라서 이제 어레이는 `[`a`, `b`, `c`, 정의되지 않음, 정의되지 않음, `d`]와 같이 표시됩니다. 지금까지, 너무 좋아.

하지만 왜 그것이 반복 기술에 문제가 되는가? 그렇다면 `for…of`와 `for…in`의 생산량을 비교해보자.

그 이유는 인덱스 3과 4가 배열의 열거할 수 있는 속성이 아니기 때문입니다. 인덱스 5는 직접 할당되었기 때문에 열거할 수 있는 속성입니다.

사용 사례에 따라 이것이 원하는 것일 수도 있고 아닐 수도 있습니다. 그러나 일반적인 어레이 반복과 다르게 동작하므로 놀랄 수 있습니다. 인쇄된 요소의 수가 더 이상 배열의 길이와 일치하지 않습니다. 만약 당신이 무엇을 하고 있는지 알고 있고 이것이 당신이 원하는 바로 그 사용 사례라면, "에 대해"를 사용하세요.단지 그것의 행동을 알아두세요.

# 문제 #3: 사용자 지정 속성

어떤 이유로 인해 어레이의 프로토타입에 속성이 추가될 경우 또 다른 문제가 발생할 수 있습니다. 이 방법이 바람직한지 아닌지는 서로 다른 논의이지만 코드가 실행 중인 환경을 완전히 제어하지 못할 수도 있습니다.

예를 들어 다른 사용자가 사용할 수 있는 라이브러리를 작성하는 경우 일부 다른 프로그래머가 다음과 같은 배열 방법을 추가하는 것이 유용할 수 있다고 생각하는 상황을 고려해 보십시오.

출력:

```undefined
그
b
c
() => {...}
```

이게 어찌된 일이냐? 흠, `플랫텐`은 이제 우리 어레이의 열거할 수 있는 재산이기 때문에 `for…in`은 그것을 반복한다. 이것은 확실히 당신이 원하는 것이 아니다.

# 결론

"for…in"은 일반적인 의미에서 배열을 반복하려는 의도가 아니다. 그래서 대부분의 경우, 그것은 잘못된 도구입니다. 하지만, 그것을 그 목적으로 사용하는 것이 좋은 생각이 되는 드문 상황이 있을 것입니다. 단지 의식적으로 그것을 하고 정확한 행동을 인식하여 당신의 코드에 어떠한 버그도 도입하지 않도록 하세요.

# 참고문헌