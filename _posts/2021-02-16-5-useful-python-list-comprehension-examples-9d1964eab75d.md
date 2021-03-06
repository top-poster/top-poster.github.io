---
layout: post
title: "5가지 유용한 Python 목록 이해 예제"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![Festival lights](https://miro.medium.com/max/9796/1*s28nGW4hxVIqESihC4LliA.jpeg)

파이썬 리스트 캄프리헨션은 특정 술어 논리에 따라 새로운 목록을 만들거나 기존 목록을 수정하는 데 사용되는 구문론이다. 그것들은 종종 세 줄 이상의 파이썬 루프를 간결하고 우아하며 읽을 수 있는 한 줄로 줄이는 데 사용된다.

다음은 Python 목록 포괄성의 기본 구문입니다.

```undefined
[불가결한 항목에 대한 설명]
```

또한 다음 대체 구문을 사용하여 조건을 설정하고 중첩된 포괄성을 만들 수 있습니다.

![Examples of list comprehension syntax](https://miro.medium.com/max/1172/1*joi_j8kRYjpXQNKmEvEOqg.png)

Python 목록 포괄성을 사용하여 목록을 반복하고 다양한 방식으로 변환할 수 있다. 목록에서 요소를 찾거나 목록 목록을 평평하게 만드는 것에서부터 분할 목록에 이르기까지 목록 포괄으로 얻을 수 있는 수효는 실로 무한합니다.

다음 몇 섹션에서는 파이썬의 다양한 목록 이해 예를 살펴보겠습니다. 기사의 마지막 부분까지 컨셉을 익혀서 어떤 시나리오에서든 활용하실 수 있기를 바랍니다.

# 1. 목록에서 요소 제외

간단한 예부터 시작하겠습니다. 다음과 같은 방법으로 리스트에서 특정 요소(예: 홀수)를 제외할 수 있습니다.

```undefined
new_list = [x가 x%2 == 0인 경우 x일 경우 x일 경우]
```

슬라이스 연산자를 사용하여 할당을 설정할 수도 있습니다.

```undefined
new_list[:] = [x가 x%2 == 0인 경우 x일 경우 x일 경우
```

슬라이스에 할당하면 `new_list`에 대한 참조를 포함하는 다른 목록도 업데이트됩니다.

# 2. 기능 목록 호출

목록 이해의 표현 구성 요소는 새로 생성된 목록에서 반환되는 것입니다. 따라서, 우리는 그것을 활용하여 목록의 각 요소에 대한 함수를 호출할 수 있습니다.

```undefined
defilesToKm(x):
반환 x*1.198734
```

위의 변환 기능을 사용하여 마일 단위에서 지정한 요소 목록을 킬로미터로 변환하려고 합니다. 매우 간단한 방법으로 할 수 있습니다.

```undefined
kList = [milesToKm(m) 양식 in mList]
```

![Calling a list of functions using comprehensions in Python](https://miro.medium.com/max/1004/1*t3b3JuwZFpWSW6qWrqWKhQ.png)

# 3. Touple을 목록으로 정리

튜플은 개체의 순서 집합입니다. 우리는 종종 목록 두 개를 풀어야 하는 상황에 부딪힌다. 튜플을 목록으로 풀고 그룹화하고 싶다고 가정해 보겠습니다. 예를 들어:

```undefined
원래 = [(1,2), (3,4)]
결과 = [1, 2], [3, 4]
```

다음은 언팩 오퍼레이터를 활용하여 목록을 포괄하는 방법 중 하나입니다.

```undefined
결과 = [*x](원본의 x에 대한 결과)
```

이 경우에는 내장된 `zip(*list)` 기능을 사용하는 것이 더 읽기 쉽지만, 목록으로 변환하는 동안 튜플을 수정하려면 목록 이해 기능을 사용하는 것이 더 편리합니다.

```undefined
new_list = [['a', *x](원래의 x에 대한 경우)]
```

![Unpacking tuples into lists](https://miro.medium.com/max/918/1*x3Ijy9-oNVztNN-rGZSpiQ.png)

# 4. 목록 목록에서 목록 변환

우리는 이미 목록의 목록에 있는 튜플을 푸는 것이 얼마나 간단한지 알고 있다. 음, 우리는 목록을 같은 크기의 내부 리스트로 나눌 수도 있습니다.

다음 목록 이해는 목록을 목록 목록으로 변환하는 방법을 보여줍니다.

```undefined
#n은 각 청크의 크기입니다.
[0, len(lst), n) 범위의 i에 대한 lst[i:i + n]
```

![Converting a list into a list of lists in Python](https://miro.medium.com/max/1320/1*9fzprFm1avwWWw9CVRSh_A.png)

# 5. JSON 어레이 값을 파이썬 목록으로 디코딩

API에서 JSON 응답을 구문 분석하는 작업은 개발자에게 가장 사소한 작업입니다. 그러나 어떻게 JSON 어레이를 동등한 Python 목록으로 신속하게 변환할 수 있습니까? 목록 포괄사항을 다시 한 번 입력하세요.

```undefined
[x["first"]의 경우 xiny["first"]의 경우 my_json["root]의 y]의 경우 [x["first"]
```

다음은 위도와 경도를 여러 목록으로 구문 분석한 위치 주소의 JSON 샘플입니다.

![Python JSON array to list using comprehensions](https://miro.medium.com/max/1846/1*qWbb_Bx9A6IIIsAoLJJc4Q.png)

# 결론

Python의 목록 이해에는 다양한 사용 사례가 있습니다. 그것들은 피톤적인 코드 작성 방식입니다. 하지만, 사람들은 종종 그것들을 오용해서 결국 이해할 수 없는 코드를 쓰게 되는 경향이 있습니다.

다음 기사에서는 목록 이해를 사용하는 것이 최선의 방법이 아닌 몇 가지 경우를 볼 수 있습니다.

이것으로 마치겠습니다. 읽어주셔서 감사합니다!