---
layout: post
title: "예제 JS 코드를 사용한 자바 스크립트 배열 역방향 자습서
 "
author: 'Code Tower'
thumbnail: https://www.freecodecamp.org/news/content/images/size/w600/2021/01/js-reverse.png
tags: undefined
---


특정 제한 사항이있는 어레이를 뒤집는 것은 면접 및 코딩 퀴즈에서 찾을 수있는 가장 일반적인 과제 중 하나입니다.
 

이 튜토리얼에서는 사용할 수있는 코드 스 니펫과 함께`reverse` 메소드를 사용하거나 사용하지 않고 자바 스크립트에서 배열을 뒤집는 다섯 가지 방법을 보여줍니다.
 

## Reverse 메서드를 사용하여 JavaScript에서 배열을 반전하는 방법
 

자바 스크립트에서 배열을 뒤집어 야 할 때 `reverse`메서드를 사용하면 마지막 요소를 먼저 배치하고 첫 번째 요소를 마지막에 배치합니다.
 

그러나 `reverse`메서드는 원래 배열도 수정합니다.
 

일부 코딩 문제는 원본 배열을 보존하기를 원할 수 있으므로 원본을 변경하지 않고 배열을 뒤집을 수있는 방법을 살펴 보겠습니다.
 

## Spread 연산자를 사용하여 JavaScript에서 배열을 반전하는 방법
 

스프레드 연산자와`reverse` 방법의 조합을 사용하여 원본을 변경하지 않고 배열을 반전시킬 수 있습니다.
 

먼저 스프레드 구문을 대괄호`[]`로 묶어 스프레드 연산자에서 반환 된 요소를 새 배열에 넣습니다.
 

```undefined
[...numbers]
```

그런 다음 배열에서 `reverse`메서드를 호출합니다.
 이렇게하면 `reverse`메소드가 원본 대신 새 배열에서 실행됩니다.
 

참고 :`spread` 방법은 ES6 구문입니다.
 이전 브라우저를 지원해야하거나 ES5 구문을 사용하려는 경우`slice` 및`reverse` 메소드를 결합 할 수 있습니다.
 이제 살펴 보겠습니다.
 

## Slice 및 Reverse 메서드를 사용하여 JavaScript에서 배열을 반전하는 방법
 

`slice` 메서드는 선택한 요소를 새 배열로 반환하는 데 사용됩니다.
 인수없이 메서드를 호출하면 원본과 동일한 새 배열이 반환됩니다 (첫 번째 요소에서 마지막 요소까지).
 

다음으로 새로 반환 된 배열에서 `reverse`메서드를 호출합니다.
 이것이 원래 배열이 반전되지 않는 이유입니다.
 

## Reverse 메서드없이 JavaScript에서 배열을 반전하는 방법
 

때때로 면접은 `reverse`방법없이 배열을 뒤집도록 요구할 것입니다.
 문제 없어요!
 아래 예제와 같이`for` 루프와 배열`push` 메소드의 조합을 사용할 수 있습니다.
 

## JS에서 자신의 역방향 함수를 작성하는 방법
 

마지막으로, 복사본을 만들지 않고 배열을 반전해야하는 자체 역함수를 작성해야한다고 가정 해 보겠습니다.
 처음에는 복잡해 보일 수 있지만 실제로는 매우 쉽기 때문에 걱정하지 마십시오.
 

여기서해야 할 일은 배열의 첫 번째 요소와 마지막 요소를 바꾼 다음 두 번째 요소와 마지막 요소를 바꾸는 식으로 모든 요소를 바꿨을 때까지하는 것입니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/js-array-reverse-function-1.png)

이를 수행하는 함수를 작성해 보겠습니다.
 

`customReverse`함수를 작성하고 `array.length-1`을 변수로 사용하여 첫 번째 색인을 `0`에 저장하고 마지막 색인을 저장합니다.
 

```undefined
function customReverse(originalArray) {

  let leftIndex = 0;
  let rightIndex = originalArray.length - 1;
}
```

다음으로`leftIndex`가`rightIndex`보다 작은 한 실행되는`while` 루프를 만듭니다.
 

이 루프 내에서`leftIndex`와`rightIndex`의 값을 바꿉니다.
 값 중 하나를 임시 변수에 임시로 저장할 수 있습니다.
 

```undefined
while (leftIndex < rightIndex) {

  // Swap the elements
  let temp = originalArray[leftIndex];
  originalArray[leftIndex] = originalArray[rightIndex];
  originalArray[rightIndex] = temp;
}
```

마지막으로`leftIndex`를 위로 이동하고`rightIndex`를 아래로 이동합니다.
 `while` 루프가 반복되면 두 번째 요소와 마지막 요소에서 두 번째 요소가 서로 바뀝니다.
 

되돌릴 요소가 더 이상 없으면 루프가 바로 중지됩니다.
 홀수 크기 배열의 경우`leftIndex`와`rightIndex`의 값이 동일하므로 더 이상 스와핑하지 않습니다.
 짝수 크기의 경우`leftIndex`가`rightIndex`보다 큽니다.
 

함수를 테스트하여 다음과 같이 제대로 작동하는지 확인할 수 있습니다.
 

## 결론
 

축하합니다!
 JavaScript에서 배열을 뒤집는 방법뿐만 아니라 자신의 역함수를 작성하는 방법도 배웠습니다.
 

관심을 가질만한 JavaScript 튜토리얼은 다음과 같습니다.
 

- 자바 스크립트 배열을 문자열로 (쉼표 포함 및 제외)
 
- JavaScript로 배열을 필터링하는 방법
 
- JavaScript 감소 방법 이해
 
- 자바 스크립트 배열 길이 이해
 