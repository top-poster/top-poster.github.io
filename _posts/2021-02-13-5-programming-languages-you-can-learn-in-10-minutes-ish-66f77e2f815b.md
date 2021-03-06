---
layout: post
title: "10분 안에 배울 수 있는 프로그래밍 언어 5개"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![An hourglass](https://miro.medium.com/max/9792/1*GO5XXRB5X4CUX04gutlTkA.jpeg)

다른 언어를 적극적으로 사용하지 않을 계획이라도 다른 언어를 배우는 것은 모든 개발자에게 좋은 방법입니다. 대부분의 시간을 한 기술에 집중하면 다른 곳에서 발견할 수 있는 잠재적 개선 사항을 놓치게 되기 때문입니다.

오해하지 마세요. 이것은 모든 언어를 마스터하는 것에 관한 것이 아닙니다. 그것은 실용적이지도 않고 가능하지도 않습니다. 하지만 지금까지 객체 지향적인 대안에만 초점을 맞추었기 때문에 기능 프로그래밍 방식을 빠뜨리지 않았다고 누가 말할 수 있을까요?

그래서 제가 가장 좋아하는 프로그래밍 책들 중 한 권의 정신으로, 저는 여러분이 검토할 5개의 프로그래밍 언어를 골랐습니다.

그리고 비록 그것들이 여러분의 하루에 적용되지 않더라도, 그것들을 확인하고 그들이 무엇을 할 수 있는지 보세요. 놀래킬지도 몰라.

# 자바스크립트

웹 분야에서 모두가 일하는 것은 아니기 때문에 자바스크립트를 첫 번째로 선택했습니다. 내부에서도 모든 백엔드 개발자가 Node와 함께 작동하는 것은 아닙니다.

자바스크립트는 시간이 지남에 따라 커뮤니티가 여러 아키텍처에 대해 서로 다른 인터프리터와 실행 시간을 만들었기 때문에 현재 매우 인기 있는 언어이다. 따라서 웹 개발 산업에서의 역할로 널리 알려져 있지만, JavaScript는 다음과 같은 경우에도 사용할 수 있습니다.

## 자바스크립트의 주요 특성

JavaScript의 가장 매력적이고 흥미로운 특징 중 일부는 다음과 같습니다.

동적 타이핑

JavaScript는 동적 유형을 지원하므로 데이터 유형에 상관 없이 런타임 중에 변수의 내용을 변경할 수 있습니다.

위의 예제를 살펴보십시오. 동적 유형을 활용하는 방법은 분명 아닙니다. 하지만 JavaScript의 유연성 덕분에 가능합니다.

다이내믹 타이핑 시스템의 주요 이점은 다음과 같습니다.

다중 패러다임 지원

자바스크립트는 단일 프로그래밍 패러다임에 초점을 맞춘 다른 언어들과 달리 객체 지향 세계의 최고를 기능 세계와 혼합하려고 한다.

언어는 클래스, 메서드, 메서드 재정의 및 공개 및 개인 구성원(적어도 곧 개인 구성원을 지원함)을 지원합니다.

동시에 자바스크립트에는 일급 시민으로서의 기능 개념이 있습니다. 따라서 함수를 결합하고 더 많은 선언적 코드, 커링 함수 및 기타 FP 패턴(예: reduce, map 등)을 작성할 수 있습니다.

비동기 I/O

비동기 코드는 JavaScript에서 기본 제공되므로 언어 자체가 스레드나 기타 복잡한 메커니즘에 대해 걱정할 필요 없이 차단되지 않는 I/O 작업을 쓰는 데 사용할 수 있는 몇 가지 놀라운 구조를 제공합니다. 물론 콜백 기능과 약속, 그리고 `sync/`wait`에 대해 얘기하고 있습니다.

위의 코드를 주목하십시오. 쉽게 읽을 수 있으며 누가 요청을 하고 있는지 또는 언제 응답을 받았는지를 정신적으로 추적할 필요가 없습니다. `sync/`wait` 콤보는 비동기 코드를 동기 코드처럼 쓸 수 있게 해줍니다.

해석된 언어에 매우 효과적임

자바스크립트에 대한 한 가지 오해는 그것이 컴파일되지 않았기 때문에 성능이 매우 좋을 수 없다는 것이다. 그러나 대부분의 JS 런타임은 JIT 컴파일러를 구현하여 실행 속도가 매우 빠르다. 실제로, 코드가 오래 실행될수록 JIT 컴파일러가 실행을 최적화할 수 있는 기회가 많아진다.

# 이오

Io는 SmallTalk, Lisp, Lua와 같은 다른 언어로부터 몇 가지 단서를 얻는 순수한 객체 지향 언어이다. 자바스크립트(및 기타)와 마찬가지로 프로토타입 기반 객체 모델을 가지고 있으며 클래스와 객체를 구분하는 데 관심이 없다. Io에서는 모든 것이 물건이다.

작은 가상 머신에서 실행되므로 임베디드 사용 사례에 적합합니다. Io는 Pixar에서 Python으로 전환하기 전에 RenderMan 렌더링 파이프라인에 사용했었다. 그리고 저자가 이 위성이 인공위성의 일부로 사용된다는 소문이 있다고 인용되었지만, 저는 개인적으로 그 증거를 찾을 수 없었습니다.

말하자면, Io의 모델은 메시지와 사물을 기반으로 하기 때문에 매우 흥미롭습니다. 말씀드렸듯이, Io의 모든 것은 물건이고, 메시지는 그들과 상호작용하는 방법입니다. 기본적인 "Hello, World!" 예제를 살펴보겠습니다.

```undefined
"안녕하세요, 삐약삐약!" 인쇄물
```

특히 다른 C형 언어에서 온 경우에는 좀 익숙해져야 하는데, 이 예에서는 문자열 객체에 `인쇄`프린트` 메시지를 전달합니다.

숫자 목록을 다루는 또 다른 예를 살펴보겠습니다.

```undefined
d:= 클론 추가 목록(30, 10, 5, 20)
d프린트
```

이것은 메시지 전달의 메커니즘을 보여주기 때문에 흥미로운 것입니다. 위의 내용은 다음과 같이 읽을 수 있습니다.

```undefined
d:= (List.clone()) 추가(30, 10, 5, 20)
d.인쇄물
```

가장 먼저 해야 할 일은 목록을 복제하는 것입니다. 바로 IO에서 새로운 개체를 생성하는 방법입니다. 그런 다음 새 인스턴스에서 추가 메서드를 호출합니다. 최종 결과에서 숫자를 정확하게 정렬할 수 있도록 `sort(정렬)` 방법에 전화를 어떻게 추가할 수 있다.

다음과 같은 경우:

```undefined
d:= 클론 추가 목록(30, 10, 5, 20) 정렬
d프린트
```

## 그럼 Io는 뭐가 그렇게 특별해?

독특한 구문을 제외하고, Io는 다음과 같은 몇 가지 매우 흥미로운 특징을 가지고 있습니다.

게으른 평가, 쓰레기 증분 수집, 예외 처리 등과 같은 다른 것들이 있습니다. IO는 완벽한 기능 언어이며, 자세한 내용을 알고 싶다면 해당 문서를 참조할 수 있습니다.

이 흥미로운 샘플에 대해 마지막으로 생각해본 결과, 메타프로그래밍의 능력을 한번 보여드리고자 합니다. 여기서 우리는 `+` 피연산자를 재정의할 것이다. 그것은 매우 기본적인 것이기 때문에 매우 적은 언어들이 그것을 허락합니다.

```undefined
= 번호 getSlot("+")
숫자 + := 방법(i, i > 10, self originPlus(i), "그건 미친 짓이야!")
(1+2) println
(2 + 100) println
```

위의 코드는 다음과 같습니다.

물론 이 스크립트의 출력은 다음과 같습니다.

```undefined
3
그건 미친 짓이야!
```

이 라이브 REP에서는 코드와 더 많은 IO를 사용할 수 있습니다.

# 서언, 머리말

프롤로그는 제가 가장 좋아하는 "언젠가 사용되지 않지만 잠재적으로 매우 유용한" 언어 중 하나입니다. 그 이면의 패러다임은 그저 잠재력이 너무 풍부해서 더 인기가 없는 것 같아 당황스럽다. 나는 과거에 그것에 대한 모든 기사를 썼지만, 그 뒤의 요지는 다음과 같다.

프롤로그는 논리 프로그래밍 언어이다. 이는 IT 산업에서 우리가 해야 할 일이 무엇인지 알고 있는 대부분의 경험과 크게 어긋납니다. 그래서 우리는 컴퓨터에 무엇을 해야 하는지, 심지어 어떤 상황에서는 어떻게 해야 하는지까지 말합니다.

하지만 프롤로그는 다르다. 논리 프로그래밍을 통해 우주를 설정하고 컴퓨터에 자신의 현실이 어떻게 보이는지 설명해야 합니다. 그런 다음 질문을 합니다. 수행할 작업과 실행 방법은 런타임에 따라 다릅니다.

흥미롭죠, 그렇지 않나요?

다음 코드를 예로 들어 보겠습니다.

```undefined
매직넘버(3)
매직넘버(5)
매직넘버(7)
매직넘버(9)
? - 매직넘버(X), 매직넘버(Y), 더하기(X, Y, 12)
```

첫 네 줄은 우주를 세팅하고 있습니다. 우리는 통역에게 네 개의 마법의 숫자를 가지고 있다고 말하고 있습니다. 그리고 나서 우리는 질문을 던집니다. X의 매직 넘버와 Y의 매직 넘버로 매직 넘버를 합치면 12를 반환하는 매직 넘버는?

결과는 물론 다음과 같습니다.

```undefined
3, 9
5, 7
7, 5
9, 3
```

내가 어떻게 그것들을 반복하지 않았는지, 아니면 여러 가지 조합이나 다른 논리를 찾을 필요가 없었는지 주목하라. 방금 질문을 했어요.

## 그럼 프롤로그는 뭐가 그렇게 멋있어?

제 의견에 동의하지 않으시겠지만, Prolog를 매우 흥미롭게 만드는 가장 중요한 기능은 논리입니다.

여러분은 형성되는 동안 진리 표를 연구했을지도 모릅니다. 그리고 여러분은 수학 문제를 증명하기 위해 논리를 사용하는 것에 대해 한두 가지를 보았을지도 모릅니다. 그러나 프로그래밍 문제를 해결하는 데 적용되는 논리를 보지 못했다면, 놓치고 있었던 것입니다.

Prolog는 문제를 해결하기 위해 순수하고 단순한 논리를 사용합니다. 솔루션을 코드화하는 대신 Prolog를 사용하여 솔루션을 찾습니다.

다음 시나리오를 그려 보십시오. 여러분은 학생, 선생님, 그리고 과목들의 집합을 모형화해야 합니다. 각 학생은 여러 과목을 공부할 수 있고, 각 선생님은 한 과목을 가르칠 수 있다.

전통적인 프로그래밍 기술을 사용하여 이 모델을 만든다면, 데이터베이스를 사용하여 "X학생은 어떤 과목을 공부합니까?"와 같은 것을 쿼리할 생각을 했을 것입니다. 아니면 "누가 X 과목을 공부하고 있나요?"

Prolog를 사용하면 사실과 규칙을 사용하여 문제를 모델링하면 다음과 같은 작업을 수행할 수 있습니다.

```undefined
스터디(csc, csc135).
스터디(csc, csc135).
스터디(csc, csc1000)입니다.
스터디(잭, CSC)
연구(csc, csc134)

티쳐(kirke, csc135).
티처(teachers, CSC²)
티처(teachers, CSC²)
티칭(주니퍼, csc134)입니다.
교수(X, Y) : - 교수(X, C), 연구(Y, C)
```

참고: http://athena.ecs.csus.edu/~mei/logicp/prolog/programming-dister.glass에서 가져온 예제입니다.

여러분은 누가 무엇을 공부하고 누가 어떤 과목을 가르치고 있는지를 기술하는 사실을 알고 있습니다. 마지막 줄은 `Y`의 X교수는 다른 사람이 공부하는 과목을 가르치는 사람이라는 규칙을 정하는 것이다.

그럼, 넌 끝장이야. 이것은 새 데이터베이스이며 다음과 같은 질문을 할 수 있습니다.

```undefined
스터디(What, csc134)입니다. //'csc135'를 연구하는 사람
=> What=➡
교수(커크, 뭐) // 키르케 교수는 누구예요?
=> What=➡
=> What=➡
```

보시다시피, 논리엔진은 무거운 부분을 담당합니다. 여러분의 일은 기본적으로 우주를 올바르게 설정함으로써 여러분이 올바른 질문을 할 수 있도록 구성되어 있습니다.

이것은 왜 Prolog가 IBM의 Watson과 같은 프로젝트나 GeneXus와 같은 코드 생성 도구에 AI에 사용되는지에 대한 놀라움입니다.

이 REF를 사용하여 온라인으로 Prolog를 시도할 수 있습니다.

# 클로저

만약 여러분이 이상하게 생긴 언어에 대해 말하고 싶다면, Clojure는 확실히 위에 있다.

Clojure는 대부분 기능적인 프로그래밍 언어로, 멀티스레딩을 위한 환상적인 지원과 함께 매우 높은 수준의 동적 동작을 제공합니다.

흥미로운 점은, 다른 C형 언어들과 다르게 보이는 주요 이유 중 하나는, Lisp를 기반으로 하기 때문에 모든 것이 목록이라는 것입니다.

```undefined
('헬로 월드' 정의)
(인쇄 a)
```

그 코드는 "헬로 월드"를 출력할 것이지만, 흥미로운 사실은 이 코드들이 단지 두 개의 목록에 불과하다는 것이다. Clojure의 두 괄호 사이의 모든 것은 목록이다. 그 때문에 목록의 첫 번째 항목은 항상 함수로 처리되므로, 실제로 값 목록을 선언하려면 `목록` 함수를 호출해야 합니다.

```undefined
(목록 1 2 3 4 5 6)
'(1 2 3 4 5)
```

두 번째 줄은 명백한 `list` 함수의 사용을 피하기 위한 짧은 표기법이다.

Clojure에 대해 알아야 할 또 다른 흥미로운 점은 상위 Lisp와 달리 JVM 위에서 실행된다는 것입니다. 즉, Java에 액세스할 수 있다는 뜻입니다. Clojure 코드에서 JVM의 유형, 클래스 및 기타 기능을 함께 사용할 수 있습니다. 즉, 다음과 같은 작업을 수행할 수 있습니다.

```undefined
(새로운 java.util.날짜 "2016/2/19")
```

실제로 사용할 유효한 Java 날짜 개체를 얻을 수 있습니다.

## 주요 특징

클로저는 규모와 업종에 관계없이 수백 개의 기업에서 활발하게 이용되고 있다.

목록은 계속됩니다. 여기서 모든 성공 사례를 확인할 수 있습니다.

# 녹

마지막으로, 러스트가 이 여행의 마지막 정거장이에요. 다른 것들과 매우 다른 점을 보여주기 때문에 저는 이것을 취재하고 싶었습니다.

Rust는 성능과 안전을 염두에 두고 Mozilla Research 프로그램 내에서 설계 및 개발되었습니다. 구문론적으로는 C++와 많이 닮았고, 이와 마찬가지로 실행 전에 컴파일해야 한다.

그러나 시각적으로 C 또는 C++와 많이 유사할 수 있는 경우에도 다른 언어에서 많은 단서를 얻어서 독특하고 강력하다. 예를 들어:

```undefined
fn 주 회로 {
x = 42로 한다.
lety = {
x_class = x * x;
x_cube = x_cube * x;
x_cube + x_cube + x // 식의 반환 값입니다.
};
인쇄!("y는 {:?}", y);
}
```

```undefined
x = 5로 한다.
x {와 일치시키다
1. =5 => 인쇄!("1 ~ 5",),
_ => println! ("다른 것"),
}
```

## 러스트가 왜 그렇게 멋있어?

```undefined
fn simple_adder(a: i32, b: i32) -> i32 {
결과 = a + b;
결과를 낳다
}
```

위의 작업 예는 이러한 새로운 구조를 사용하여 객체 지향 니즈를 해결하는 방법을 보여줍니다. 여기서 온라인 RELP를 사용하여 위의 예를 시도해 볼 수 있습니다.

정적으로 입력되는 세계에서 오거나 하드웨어에 대해 저희 업계의 다른 분야보다 더 가까이 접근하고 있다면 Rust에게 기회를 주고 싶을 것입니다.

# 결론

이 언어에 대한 간단한 개요는 여기까지입니다. 물론, 다른 사람들도 너무 많아서 아마 당신이 좋아하는 것을 목록에서 빼놓았을 거예요. 그러나, 이것의 요점은 다양한 프로그래밍 패러다임의 다양한 구현에 적용되어 언어가 얼마나 다재다능한지를 보여주는 것이었습니다.

이 옵션들 중 적어도 한 가지에 대한 당신의 관심을 끌 수 있다면, 저는 제 일을 이미 끝난 것처럼 생각할 것입니다.