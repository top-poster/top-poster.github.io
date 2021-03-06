---
layout: post
title: "Promise 풀을 사용하여 Node.js 성능 향상"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![Image for post](https://miro.medium.com/max/2904/1*XFON-YgJCak5CN2ooi13-g.png)

필수 구성 요소: 이 기사를 따라가기 위해서는 약속의 기본을 알아야 합니다.

Node.js에서 동시성을 달성하는 일반적인 방법은 `약속` 방법 중 하나인 `약속` 방법 중 하나를 사용하는 것입니다. 가장 일반적인 방법은 `약속.all()입니다.

사용자 목록에 대한 ID 목록이 있는 데이터베이스를 쿼리하고 반환된 데이터에 대해 작업을 수행하려고 합니다(예: ID가 사용자를 반환하지 않은 경우 새 사용자 만들기).

이를 위한 한 가지 방법은 다음과 같습니다.

이 예에서는 각 `find OrCreateUser`가 순차적으로 호출됩니다. 그다지 효율적이지 않다.

`약속.모두()`를 사용하여 이를 개선해 봅시다.

각 `Find OrCreateUser` 약속이 사용자로 확인되기를 기다리고 있지 않습니다. 대신, 우리는 `약속`을 사용하여 그들 모두가 해결되기를 기다린다.모두()이것은 동시에 실행할 수 있게 하는 효과가 있습니다.

하지만 다음과 같은 문제가 있습니다. 동시성을 제어하려고 한다고 가정합니다(예: 데이터베이스의 제약을 피하기 위해 4개의 약속만 동시에 실행하도록 허용). 어떻게 이럴 수 있죠?

이 점에서 약속은 한정되어 있다.

해결책은 약속 풀을 사용하는 것입니다.

많은 약속 풀 라이브러리가 있지만 @supercharge/promise-pool은 내가 다른 것들보다 더 선호하는 좋은 API를 제공합니다.

사용 방법은 다음과 같습니다.

이 예에서, 저는 100의 동시성을 지정했습니다. 따라서 약속의 양은 주어진 시간 내에 해결될 것입니다.

우리는 각각 1과 "idList.length"의 동시성을 지정하여 이전의 예들을 시뮬레이션할 수 있다.

내가 `약속`을 사용하지 말라고 설득했었나?모두 수영장을 약속한다고요? 그렇지 않다면, 왜? 저는 당신의 의견을 알고 싶습니다.

읽어주셔서 감사합니다!