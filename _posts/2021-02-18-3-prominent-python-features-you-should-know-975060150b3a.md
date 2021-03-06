---
layout: post
title: "알아야 할 3가지 Python 기능"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![Mountains](https://miro.medium.com/max/11232/0*YRm4JWM23wUPiNZQ)

오늘날 파이썬은 가장 선호하는 프로그래밍 언어 중 하나가 되었다. 구문이 명확하고 간단하며 학습 곡선이 합리적입니다. 파이썬은 데이터 과학, 통계, 분석, 웹 개발, 스크립트를 통한 자동화 등의 분야에서 사용될 수 있다. 공동체가 강해 공통적인 문제를 해결할 수 있는 도서관과 도구가 이미 많이 있다.

이 모든 것들이 파이썬을 까다로운 언어로 만든다. 분명히 고려해야 할 몇 가지 고급 개념들이 있다. Python의 기본 사항에 대해 이미 잘 알고 있는 경우 다음 기능을 확인하십시오. 학습 과정을 더 진전시킬 수 있습니다.

# 데코레이터 패턴

장식자 패턴은 소프트웨어 개발에서 디자인 패턴이다. 개체의 동작을 변경하지 않고 확장할 수 있습니다. 구현은 프로그래밍 언어에 따라 달라질 수 있습니다. 이 패턴의 실제 예로는 패키지 발송 전에 대상 레이블을 인쇄하고 네트워크 호출에 인증 데이터를 추가하는 것이 있습니다.

이 패턴은 파이썬에서 응용 프로그램을 찾았습니다. 현재 다른 프레임워크와 라이브러리에서 성공적으로 사용되고 있습니다. 어떤 구성인지 살펴보도록 하겠습니다.

위의 예에서, 우리는 `print_address()라는 함수를 선언한다. 사실상, 그것은 `decorator()` 중첩 함수를 반환하는 것 외에는 아무 것도 하지 않는다. `원함수`는 `원함수`라는 주장을 받습니다. 이 인수는 확장하려는 동작의 초기 함수입니다. 호출하기 전이나 후에 다른 작업을 수행하는 것이 장식자 패턴의 작동 방식입니다.

자, 그걸 어떻게 사용하겠습니까?

파이썬은 `디코레이터` 함수를 호출하는 특별한 구문을 가지고 있다. 원래 함수 바로 위에 `디코레이터` 함수의 이름을 쓰십시오. @로 접두사 붙이는 것도 잊지 마세요. 파이썬의 좋은 부분입니다.

파이썬이 어떻게 해석하는지 궁금하시죠? 이 특수 구문에서는 print_address() 함수가 decorator() 함수를 반환하기 때문에 ship_package() 함수를 decorator() 함수에 인수로 전달한다.

또는 다음과 같이 보일 수 있습니다.

```js
print_address("Munich, Germany") (ship_package)
```

예제에서 코드를 실행한 후 대상 주소의 출력과 패키지가 발송되었다는 메시지를 받게 됩니다.

# 제너레이터 기능

생성기 함수는 파이썬에서 특별한 유형의 함수이다. 내부 상태가 있습니다. 이 내부 상태에서 제너레이터 함수는 중단되지 않고 중간 결과를 반환할 수 있습니다. 그런 다음 실행을 계속할 수 있습니다. 이것은 정규 함수와의 주요 차이점입니다. 다음 예제를 확인하십시오.

read_caracters() 함수가 생성기 함수임을 알 수 있다. 아무것도 돌려주지 않고 수익률 키워드를 쓴다. 이 함수는 문자열을 수신하고 문자열의 문자를 하나씩 반환합니다.

일부 변수에 함수를 할당하고 인쇄하면 다음과 같은 출력이 표시됩니다.

```js
< 0x1032bd0b0에서 object read_filers를 생성함
```

할당된 변수는 함수에 대한 참조이지만 정규 함수에 대해 살펴보는 데 익숙한 것처럼 함수의 결과는 아닙니다. 우리가 발전기 기능을 반복하기 시작할 때 마법이 찾아옵니다.

다음 출력이 생성됩니다.

```js
항복 문자 L...
현재 문자 L
항복 카로...
현재 charo
항복도...
현재 문자
```

생성기 기능은 첫 번째 문자를 반환하고 인쇄합니다. 그런 다음 다음 통화 시 두 번째 문자 등을 반환합니다. 내부 상태를 유지하고 그 동안 결과를 반환합니다.

제너레이터 기능의 주요 적용은 메모리에 보관할 수 없는 빅데이터 세트를 처리하는 것입니다. 우리는 각각 일괄적으로 검토하고 일괄 처리하겠습니다. 이렇게 하면 메모리가 부족해지는 것을 방지할 수 있습니다.

# 목록 이해

목록 이해는 다른 목록에서 목록을 만드는 방법입니다. 이것은 "map() 함수"처럼 들리고, 그것은 사실입니다. 결과는 같을 것입니다. 우리는 또한 간단한 루프를 할 수 있고 새로운 컬렉션을 만들 수 있습니다. 그러나 목록 이해는 코드를 우아하게 보이기 때문에 파이썬 프로젝트에서 널리 사용된다.

위의 예에서, 우리는 리뷰가 있는 리스트를 가지고 있습니다. 4.0 이상의 리뷰만 유지하기 위해 필터링하고 싶습니다. 목록 이해와 함께, 이것은 한 줄로 달성될 수 있다.

구문은 처음에는 혼란스러워 보일 수 있지만, 모두 이치에 맞는다. 전체 식은 대괄호로 묶입니다. 그 안에 `for` 루프를 넣고 그 앞에 루프 변수를 배치한다. 루프 변수는 필요에 따라 수정할 수 있습니다. 또한 if 조건은 선택 사항이며 결과를 필터링하는 데 사용할 수 있습니다.

목록 이해는 목록뿐만 아니라 사전과 세트에도 적용할 수 있다. 구문 차이는 포장을 위해 대괄호 대신 곱슬곱슬한 대괄호를 사용하는 것입니다.

보다시피 사전적 이해는 목록적 이해와 비슷하게 보입니다. 루프는 사전을 통과하여 등급이 필터링된 다른 사전을 생성합니다.

# 결론

일단 파이썬의 기본을 익히면, 다음 단계는 더 성장하는 것이다. 이 기사의 주제들은 다양한 라이브러리와 틀에서 사용되므로, 이 뛰어난 프로그래밍 언어로 여러분의 기술을 마스터하기 위한 훌륭한 출발점이다.