---
layout: post
title: "JavaScript를 사용하여 문자열에서 모음 수를 찾는 방법
 "
author: 'Code Tower'
thumbnail: https://www.freecodecamp.org/news/content/images/size/w600/2021/01/Screen-Shot-2021-01-17-at-6.16.19-PM.png
tags: undefined
---


이 자습서에서는 JavaScript를 사용하여 문자열에서 모음 수를 찾는 방법을 알아 봅니다.
 이것은 주니어 개발자 면접에서 질문 할 수있는 문제이며 CodeWars 문제이기도합니다.
 

코딩을 시작하기 전에 문제 설명을 자세히 읽어 보겠습니다.
 

주어진 문자열에서 모음 수 (개수)를 반환합니다.
 우리는 a, e, i, o 및 u를 모음으로 간주하지만 y는 고려하지 않습니다.
 입력 문자열은 소문자 및 / 또는 공백으로 만 구성됩니다.
 

## 1 단계 : 문제 해결을위한 계획 수립
 

이 문제의 경우 문자열을 입력으로 받아 해당 문자열에있는 모음의 개수를 출력으로 반환하는`getCount `라는 함수를 만듭니다.
몇 가지 예를 살펴 보겠습니다.
 

![image](https://lh4.googleusercontent.com/0NnD6g02UboUYJkZ0KOJMw7abNXVH-e9iuq9kv1qg-OFzJ_k8t3ZVfMzj6MkPE45fjQxVBIshpJJNxF_e6KGDWSCdwp7BWd8vVasgeiJ1nYiK-7ufFJz1XuyIXcHNApmtBhn7Kk9)

첫 번째 예에서 우리는 함수가 5를 반환하는 것을 볼 수 있는데, 이는 모음이 문자열`abracadabra`에 나타나는 횟수입니다.
 `abc`문자열을 사용하면 모음 (a)이 하나만 표시되므로 1 만 반환됩니다.
 

이 문제를 해결하기 위해 문자열에 모음이 몇 개인 지 추적하는`vowelsCount` 변수를 만듭니다.
 

또한 모든 모음을 포함하는 배열, 모음을 만듭니다.
 문자열의 각 문자를 살펴 보겠습니다.
 캐릭터가 모음이면`vowelsCount` 변수를 늘립니다.
 

마지막으로`vowelsCount` 변수를 반환합니다.
시작하자!
 

## 2 단계 : 문제 해결을위한 코드 작성
 

먼저 함수`getCount`를 작성합니다.
 다음으로 변수`vowelsCount`를 만들고`0`으로 설정합니다.
 

![image](https://lh4.googleusercontent.com/3C2OuHNi9S9SL-SUEYzM8PSodXO1bYULEd9LLec7clus1o5TEvqBBgVy1STfDUoq3hFLT85VLVGAAzL8h949fazt9_36S54Oe97U39IjJhl9LBDTWCpSFd9w9wMFpkHdfSbeFpAq)

다음에는 모음 배열을 만들 것입니다.
 이를 통해 모든 모음을 한 곳에 모을 수 있으며 나중에이 배열을 사용할 수 있습니다.
 

![image](https://lh6.googleusercontent.com/g6F__ll7kJNmOq6c3kT6Z7X_zcslPkO8AuF5kUDYFcLcnJ9v-rpf3bm1NUSDPCAVWWnfpq9GS7cADMuN5GS3CdiTbAfun9Gth0CBUFGFl5vhviLMrKHKfQa9KPfWkujtV1_SLWHG)

이제 입력 문자열`str`의 모든 문자를 살펴 봐야합니다.
 모음인지 아닌지 확인할 수 있도록 문자열의 모든 문자를 살펴 보거나 살펴 봐야합니다.
 

이를 위해 문자열에서 작동하는 `for ... of`문을 사용할 수 있습니다.
 여기에서 자세한 내용을 읽을 수 있습니다.
 

![image](https://lh3.googleusercontent.com/mMSkKHhYAhJxh9F71Ccs4B9MyKpjHNlyIumJwJ9n7bTo-o6eR1YQLHsPe13VCVx7XlFU20TQHr2B5bXv52cbIHvTs2Jl2xIwBPo5hD0-ILOAW-o66sG2uyxUF5WljDTgDrsqgP7X)

이제 for 루프 내에서 문자열의 각 문자를보고 살펴볼 수 있습니다.
다음으로 각 문자가 모음인지 확인하고 싶습니다.
 

이를 위해`includes` 메소드를 사용할 수 있습니다.
 `includes ()`메소드는 배열이 항목 중 특정 값을 포함하는지 여부를 결정합니다.
 그렇다면 true를 반환하고 그렇지 않으면 false를 반환합니다.
 

`includes`를 사용하여 모음 배열에 현재 루프에서 반복중인 문자가 포함되어 있는지 확인합니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-89.png)

현재 문자가 모음인지 확인하기 위해 `if 문`을 만들었습니다.
 문자가 모음이면`vowelsCount` 변수를 늘리고 싶습니다.
 이를 위해 JavaScript에서 증가 연산자를 사용할 수 있습니다.
 

![image](https://lh4.googleusercontent.com/YELFhUaEOI51eOBznA9delrQlT5_brpGzM71vXiO6S1ARcy-IAbM06mYgPr6zQVC-0eytb87eQX8_5UBcZ0rMPLfTpf3uGHbJhpTWymoXGwLMDscQbp9BR1SIzbsrQSssmH689t2)

코드의이 시점에서 문자열의 각 문자를 살펴보고 모음인지 아닌지 확인한 다음 `vowelsCount`에 저장 한 숫자를 늘 렸습니다.
 

마지막으로 함수가 `모음 개수`변수를 반환하도록하기 만하면됩니다.
 루프 외부에서 변수를 반환하여이를 수행 할 수 있습니다.
 

![image](https://lh6.googleusercontent.com/4U_WmVuqES_Z5Tb79te7k7nCorSGuIvsKoWVXPjV1e7dug-pSylt7GMa7MNvkDBX-1PT0EtfFmCi0n-pqN0YGpo2Rs7xntRQViCzLBEYuVi0rDJOQsQJxkgScPdGHXT8ThDLvn5I)

거기에 있습니다.
 

## 그게 다야!
 

이제 문자열을 입력으로 받아 문자열에 모음이 나타난 횟수를 출력으로 반환하는 함수를 작성했습니다.
 

### 이 게시물이 마음에 드 셨다면 매주 일요일 코딩 과제를 함께 해결하는 코딩 클럽에 가입하세요.
이 게시물에 대한 의견이나 질문이 있으시면 @madisonkanna 트윗을 해주시기 바랍니다.
 