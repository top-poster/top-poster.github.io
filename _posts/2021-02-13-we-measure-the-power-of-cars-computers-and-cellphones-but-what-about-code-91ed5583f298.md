---
layout: post
title: "우리는 자동차, 컴퓨터, 휴대전화의 힘을 측정합니다. 하지만 코드는 어떨까요?"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![A device to measure voltage.](https://miro.medium.com/max/3840/1*17aMnyj2niaMmHScnOOkbw.jpeg)

저는 거짓말을 하지 않을 것입니다. 벤치마킹은 저의 가장 큰 장점 중 하나가 아닙니다. 나는 내가 원하는 만큼 자주 그것을 하지 않는다. 하지만 바둑을 제 1외국어로 삼으면서부터 점점 잦아지고 있어요. 그 이유 중 하나는 Go가 벤치마킹에 대한 훌륭한 지원을 가지고 있기 때문입니다.

Go를 통해 개발자는 테스트 패키지를 사용하여 벤치마킹할 수 있습니다. 따라서 테스트 패키지에는 벤치마킹 기능이 포함되어 있습니다. 멋지다!

이 기사에서는 벤치마크를 좀 더 자세히 살펴보고자 합니다. 하지만 처음부터 다시 시작하겠습니다. 이 글을 읽고, 제가 벤치마크를 조금 더 잘 이해했기를 바랍니다.

이제 벤치마킹에 대해 살펴보겠습니다. 소프트웨어 개발의 벤치마킹은 우리가 작성하는 코드의 성능을 테스트하는 것이다.

벤치마킹을 통해 측정된 속도를 비교하여 다양한 솔루션을 채택하고 성능을 테스트할 수 있습니다. 이는 개발자로서 보유해야 할 훌륭한 지식이며, 특히 애플리케이션을 보유한 경우 속도를 높이고 최적화해야 합니다.

개발의 황금률을 기억하는 것이 중요합니다. 너무 일찍 최적화하지 마십시오. 벤치마킹 방법을 배운다고 해서 모든 코드를 실행하고 벤치마킹할 것을 제안하는 것은 아닙니다. 벤치마킹은 성능 문제에 직면하거나 순수한 호기심으로 인해 죽을 때 사용하는 도구라고 생각합니다.

주니어 개발자들이 어떤 것이 최선인지 묻는 다른 코드 솔루션에 대해 인터넷에 올린 글을 보는 것은 드문 일이 아니다. 하지만 코드를 말할 때 어떤 말을 하는 것이 가장 좋습니다.

더 느린 코드는 더 쉽게 유지되고 읽을 수 있기 때문에 가장 성능이 뛰어난 표현을 고수해 봅시다. 물론 성능 문제가 발생하지 않는 한 이 코드가 더 좋습니다.

이제 Go를 사용하여 벤치마킹하는 방법에 대해 알아보겠습니다. 성능과 관련하여 답변하지 못한 후배 개발자의 질문을 모았습니다.

우리가 그를 위해 그들을 볼 거야.

# Super Simple 벤치마크 작성

문제를 풀기 전에 먼저 간단한 벤치마크를 작성하고 바둑을 이용한 벤치마크를 어떻게 하는지 보여드리겠습니다. 그 방법을 알고 나면, 필요한 답을 풀 수 있도록 진화해 봅시다.

이러한 벤치마크를 위해 새로운 프로젝트를 만들었는데, 직접 사용해 볼 수 있도록 동일한 작업을 수행하는 것이 좋습니다. 디렉토리를 생성하고 다음을 실행해야 합니다.

```undefined
벤치 신세를 지다
```

`_test.go`로 끝나는 파일도 만들어야 합니다. 저 같은 경우는 benching_test.go입니다.

바둑의 벤치마크는 일반 단위 테스트와 마찬가지로 테스트 패키지로 수행됩니다. 단위 테스트와 마찬가지로 벤치마크는 동일한 Go 테스트 툴링으로 트리거됩니다.

바둑 도구는 이름에 따라 벤치마크가 어떤 방법인지 알게 됩니다. `테스트`에 대한 포인터를 받아들이는 `벤치마크`로 시작하는 모든 방법.B는 벤치마크로 출마할 것이다.

go test 명령을 `-bench=로 실행하여 사용해 보십시오.기를 게양하다

![By running ‘go test -bench=.’ and seeing an output, we know the benchmark works](https://miro.medium.com/max/1056/1*lKacEi52Y9PgL_O26TuZmQ.png)

여기서 잠시 멈추고 결과를 반영합시다. 실행되는 각 벤치마크는 이름, 벤치마크 실행 횟수, ns/op의 세 가지 값을 출력합니다.

그 이름은 꽤 자명하다. 테스트 파일에 설정한 이름입니다.

벤치마크가 실행되는 횟수는 흥미롭다. 각 벤치마크는 여러 번 실행되며 각 실행에는 시간이 걸립니다. 그런 다음 실행된 횟수를 기준으로 실행 시간이 평균화됩니다. 벤치마크를 한 번 실행하면 통계 정확도가 저하되기 때문에 좋습니다.

ns/op은 나노초/작동을 의미한다. 메서드 호출이 걸린 시간입니다.

벤치마크가 여러 개 있고 하나 또는 몇 개만 실행하려면 `-bench=TrapSimpleest`와 같은 이름과 일치하는 문자열로 점을 바꿀 수 있습니다. -bench=Bench라고 말하는 것은 이 방법의 시작과 일치하기 때문에 여전히 우리의 벤치마크를 촉발시킬 것이라는 것을 기억하라.

![Replacing the ‘-bench=’ value can be used to specify what benchmarks to run](https://miro.medium.com/max/968/1*iuLjz6ZTkfcuRQLL4KhNxA.png)

따라서 지금 당장은 속도를 벤치마킹할 수 있지만 이것이 항상 우리가 측정하고자 하는 전부는 아닐 수도 있습니다. 감사하게도 테스트 패키지를 살펴보면 `-벤치멤` 플래그를 추가하면 작업당 할당된 바이트(B/op)와 작업당 할당(allocs/op)에 대한 정보가 추가된다는 것을 알 수 있다.

할당과 메모리에 대해 잘 모르신다면 빈센트 블랜컨의 기사를 제안해 드리겠습니다.

![Adding the ‘-benchmem’ flag adds B/op and allocs/op](https://miro.medium.com/max/1668/1*Zru0muQTY4WXN9ICcFlX9Q.png)

이제 곧 실제 상황을 벤치마킹할 준비가 되었습니다. 잠시 더 기다려 주십시오. 벤치마크에서 입력 매개 변수인 `*테스트`는 어떻게 된 것인가?B? 그럼 표준 라이브러리에 있는 정의부터 살펴보고 어떤 내용을 다루는지 알아보도록 하죠.

`테스트 중`B는 실행 벤치마크와 관련된 데이터를 모두 보유하고 있는 구조입니다. 또한 출력을 포맷하는 데 사용되는 `벤치마크 결과`라는 구조도 보유하고 있다. 출력물에 이해가 잘 안 되는 부분이 있다면 benchmark.go를 열고 코드를 읽어보기를 강력히 제안한다.

눈여겨볼 점은 N 변수다. 벤치마크가 여러 번 실행되는 방법을 기억하십니까? 벤치마크 수행 횟수는 테스트 내의 N 변수로 지정된다.B.

이 문서에 따르면, 이는 벤치마크에서 설명되어야 하므로, `벤치마크 심플레스트`를 `N`으로 업데이트하자.

N번 반복하는 `for` 루프를 만들어 업데이트했다. 벤치마킹할 때 N을 특정 값으로 설정하기를 좋아하기 때문에 벤치마크가 공정하도록 합니다. 그렇지 않으면 한 벤치마크가 100,000번 실행되고 다른 벤치마크가 두 번 실행될 수 있습니다.

이 작업은 `-benchtime=` 플래그를 추가하여 수행할 수 있습니다. 입력은 초 또는 X회이므로 벤치마크를 100회 강제로 실행하려면 -benchtime=100x로 설정할 수 있습니다.

![Benchmark the new method 100 times](https://miro.medium.com/max/1678/1*dNo3S9pin9cxO1-3EJ9fPQ.png)

# 준비, 설정, 벤치마크!

이제 성능과 관련된 이전의 질문에 대한 테스트와 답변을 시작해야 합니다.

지도와 슬라이스에 데이터를 삽입하기 위한 벤치마크를 구현하고 데이터를 다시 읽기 위한 다른 벤치마크를 구현하겠습니다. 제가 데이브 체니에게서 훔친 트릭은 우리가 벤치마킹하고자 하는 입력 매개 변수를 사용하는 방법을 만드는 것입니다. 이렇게 하면 다양한 값을 쉽게 벤치마킹할 수 있습니다.

이 방법은 맵에 삽입할 정수 값을 사용합니다. 이것은 맵의 크기가 삽입 성능에 영향을 미치는지 테스트하기 위한 것입니다. 이 방법은 우리의 벤치마크로 실행할 것이다. 벤치마킹할 다른 정수를 삽입하는 벤치마크 기능도 여러 개 만들겠습니다.

각 벤치마크에서 동일한 방법을 재사용하면서 삽입 횟수만 수정하는 방법을 볼 수 있습니까? 양이 많고 적으면 쉽게 테스트할 수 있으니 정말 멋진 수법입니다.

![The benchmark result of inserting into a map](https://miro.medium.com/max/1908/1*MBVvNexbvNyGYz7YBDDLHw.png)

따라서 시간이 늘어납니다. 삽입 횟수를 늘리기 때문에 예상한 대로입니다. 우리가 결과를 비교할 무언가가 필요하기 때문에 이것은 아직 우리에게 많은 것을 말해주지 않는다. 시간을 내어 한 가지 질문에 답변할 수 있습니다. 지도에서 사용하는 키 타입이 중요한가요?

나는 모든 방법을 복사하고 대신 사용된 키 타입을 인터페이스로 교체할 거야. 좀 더 쉽게 하기 위해 지금 `benching_map_interface_test.go`와 `benching_map_int_test.go`라는 두 개의 파일을 가지고 있습니다. 벤치마크 방법은 이름과 관련이 있습니다. 이는 벤치마크를 추가할 때 쉽게 탐색할 수 있는 구조를 유지하기 위한 것입니다.

![Benchmark results showing that key type does, in fact, matter](https://miro.medium.com/max/1900/1*WprWK4YiVICZglz2tigKpA.png)

지금까지 적어도 한 가지 질문에 대한 답을 찾은 것 같아요. 결과를 보면 알 수 있듯이 키 유형이 중요한 것 같습니다. 1000000 벤치마크를 고려할 때 인터페이스 대신 Int를 핵심으로 삼는 것이 이 벤치마크에서 2.23배 빠르다. 하지만 인터페이스가 키로 사용되는 경우는 한 번도 본 적이 없는 것 같습니다.

키를 기준으로 성능이 두 배로 향상되는 것은 제이슨 모이론이 읽은 결론과 일치하는 것 같습니다. 그는 "Go Performance Tales"를 썼는데, 꽤 읽힌다.

다음 단계로 넘어가기 전에, 재미있어서 잠시 시간을 내서 새로운 벤치마크를 추가하고 싶습니다. 방금 실행한 벤치마크에서 지도는 크기가 미리 할당되지 않았습니다. 그래서 우리는 그것을 바꾸고 차이를 벤치마킹할 수 있습니다.

우리가 변경해야 할 것은 `XINTMap 삽입` 방식이며, X 길이를 대신 사용하도록 지도 초기화도 변경해야 합니다. 저는 `benching_map_prealloc_int_test.go`라는 새 파일을 하나 만들었고, 그 안에서 크기를 미리 초기화하기 위해 `InsertXINtMap` 방법을 변경했습니다.

`-bench=` 플래그를 사용하여 실행할 벤치마크를 제어할 수 있다고 말했던 것 기억하십니까? 이제는 벤치마크가 많기 때문에 이 방법을 사용해야 할 때입니다. 하지만 이 특별한 벤치마크의 경우, 저는 정해진 크기가 없는 지도와 미리 할당된 크기를 가진 지도를 비교하는 데에만 관심이 있습니다.

새로운 벤치마크를 `벤치마크`라고 이름 지었습니다.IntMapPrealloc을 삽입하여 Benchmark와 동일한 이름을 공유합니다."IntMap"을 삽입합니다. 그걸 우리의 방아쇠로 사용할 수 있어 이 새로운 벤치마크 파일은 다른 `IntMap` 벤치마크의 정확한 복사본입니다. 저는 실행할 이름과 방법만 바꿨습니다.

벤치마크를 실행하고 벤치마크 플래그를 변경합니다.

```undefined
test -bench=로 이동합니다.InsertIntMap -benchmem -benchtime=100x
```

![Difference between a preallocated size in a map](https://miro.medium.com/max/2006/1*bAFn1RLe7hSwkDtBcAjerw.png)

이 벤치마크는 지도 크기를 설정하는 것이 상당히 큰 영향을 미친다는 것을 보여줍니다. 실제로 1000000의 검정 차이가 1.92배 나는 것을 볼 때 그렇다. 할당된 바이트(B/op)를 보십시오. 훨씬 더 좋습니다.

슬라이스에 대한 삽입 벤치마크를 구현하는 단계로 넘어갑시다. 지도 구현의 사본이 되겠지만 이번에는 `첨부`와 함께 슬라이스를 사용합니다.

또한 사전 할당된 슬라이스에 대한 벤치마크를 만들고 할당되지 않은 크기에 대한 벤치마크를 만들 것입니다. 매우 재미있었기 때문입니다. X 삽입 방법을 다시 만들어 모든 것을 복사 붙여넣기 한 다음 맵을 검색하여 슬라이스로 바꾸면 된다.

사전 할당된 슬라이스의 경우, 슬라이스에 인덱스가 추가되므로 부록을 사용하지 않습니다. 따라서 미리 할당된 인덱스는 올바른 인덱스를 사용하도록 변경해야 합니다.

이제 슬라이스 벤치마크를 마쳤으니, 이를 실행하여 결과를 살펴보겠습니다.

![This picture shows the benchmark of using slices instead](https://miro.medium.com/max/1986/1*aMDcXU_pBovzHAzcLrjl_Q.png)

사전 할당된 슬라이스와 동적 슬라이스의 차이가 큽니다. 1000000 벤치마크는 75388ns/op 대 7246ns/op이다. 이것은 10.4배 속도의 성능 차이입니다. 그러나 경우에 따라 고정 크기 슬라이스를 사용하는 것이 어려울 수 있습니다. 응용 프로그램의 크기는 동적인 경향이 있기 때문에 일반적으로 잘 모릅니다.

작은 숫자와 큰 숫자의 데이터를 삽입하는 경우 맵을 조각내서 수행하는 것처럼 보입니다. 또한 데이터 선택이 어떻게 수행되는지 벤치마킹해야 합니다.

이를 벤치마킹하기 위해 우리가 했던 것처럼 슬라이스와 맵을 초기화하고, `X`의 양을 추가한 다음 타이머를 재설정할 것이다. 그런 다음 "X" 항목을 얼마나 빨리 찾을 수 있는지 벤치마킹할 것입니다. 인덱스 값 `i`를 사용하여 슬라이스와 맵을 반복하기로 했습니다. 아래 두 벤치마크에 대한 코드를 게시하겠습니다. 거의 동일합니다.

![Benchmark results comparing maps and slices](https://miro.medium.com/max/1866/1*AQfQL04ZJ6ViaGB4BKxRBQ.png)

`SelectXINTSlice`와 `selectX`의 코드가 확인되었다.IntMap은 동일합니다. 유일한 차이점은 make 명령입니다. 하지만 이 두 사람의 성적 차이는 아주 명백하다.

# 벤치마크 결과 비교

이제 벤치마킹 번호가 있습니다. 검토하기 쉽도록 표로 정리해 보겠습니다.

그렇다면 슬라이스와 맵의 차이는 얼마나 될까요?

쓰기 성능을 동적 크기로 비교할 때 슬라이스가 21.65배(1321196/75388)만큼 빠릅니다.

쓰기 성능을 사전 할당된 크기로 비교할 때 슬라이스가 118.35배(857588/7246)만큼 빠릅니다.

슬라이스는 판독 성능을 비교할 때 177.19배(507843/2866)만큼 빠릅니다.

## 조각이나 지도가 더 빠릅니까?

이러한 벤치마크를 사용하면 슬라이스가 맵을 훨씬 능가하는 것으로 보입니다. 차이가 너무 커서 어떻게든 이 벤치마크를 망친 것 같아요.

그러나 맵은 사용하기 더 쉽습니다. 이러한 벤치마크에서 우리는 사용할 슬라이스의 인덱스를 알고 있다고 가정합니다. 나는 우리가 지수를 알지 못하고 아마도 지도[사용자]처럼 전체 조각을 반복해야 할 많은 경우를 생각할 수 있다.[[]사용자]에 대한 "for" 루프가 아닌 "User"입니다.

## 슬라이스와 맵의 속도가 크기에 따라 영향을 받습니까?

이런 경우에는 크기가 문제가 되지 않는 것 같습니다.

## 지도에서 사용하는 키 타입이 중요한가요?

네, 포함되었습니다. 정수를 사용하는 것이 인터페이스보다 2.23배 더 빨랐다.

# 보다 현실적인 활용 사례 추가

그래서 슬라이스가 훨씬 더 성능이 좋은 것 같지만 솔직히 말씀드리자면, 제 슬라이스에 대한 정확한 인덱스는 거의 모르겠습니다. 대부분의 경우, 저는 제가 찾고 있는 것을 찾기 위해 전체 조각을 반복해야 합니다. 이것이 제가 종종 지도를 대신 사용하는 주된 이유입니다.

저는 이 활용 사례를 가지고 벤치마크를 만들 것입니다. 지도[사용자]로 하겠습니다.ID]사용자 및 `[]사용자`입니다. 벤치마크는 특정 사용자를 찾는 경쟁이 될 것입니다.

임의 사용자를 생성하는 코드가 포함된 새 파일을 만들었습니다. 저는 한 조각과 지도에서 1만, 10만, 100만 명의 사용자를 생성할 것입니다. 만약 우리가 API를 가지고 있다면, 사용자 ID가 우리에게 전송되었고, 우리는 그 사용자를 찾고 싶어한다고 상상해 보세요. 이것이 우리가 테스트할 시나리오입니다. 데이터가 동적으로 추가되는 실제 사용 사례를 시뮬레이션하기 때문에 슬라이스도 섞겠습니다.

저는 이 벤치마크를 "라이언 일병 구하기"라고 부릅니다. 우리는 그를 찾아야 하며, 그는 7777이라는 사용자 ID를 가지고 있다.

![Benchmarking result of finding a user in a map versus a slice](https://miro.medium.com/max/1868/1*zbLoA6qqQc95OoBxBwz4Kw.png)

보시다시피 슬라이스가 더 이상 맵을 능가하지 않습니다. 결과를 보면 맵은 항목 수에 관계없이 동일한 속도를 유지하는 경향이 있는 반면 추가된 각 항목에 대해 슬라이스가 더 오래 걸립니다.

이 사용 사례에서 맵은 6678.53x(140917/21.1)의 인자로 성능이 더 우수합니다.

# 결론

## 조각이나 지도가 더 빠릅니까?

"라이언 일병 구하기" 벤치마크를 통해 소개한 바와 같이, 물리적 전력에 대해서는 훨씬 더 성능이 뛰어나지만 정교하고 사용하기는 더 어렵습니다. 때로는 힘이 전부가 아닐 때도 있다.

저장된 값에 쉽게 액세스할 수 있기 때문에 지도를 사용하는 경향이 있습니다. 프로그래밍의 경우 여러 번 그렇듯이 사용 사례에 따라 다릅니다.

## 슬라이스와 맵의 속도가 크기에 따라 영향을 받습니까?

슬프게도, 제 아내는 크기가 중요하다고 말해요. 제 벤치마크도 마찬가지입니다. 올바른 인덱스 번호를 사용할 때는 물론 상관 없습니다. 그러나 값이 저장된 인덱스를 모르면 크기가 매우 중요합니다.

## 지도에서 사용하는 키 타입이 중요한가요?

네, 포함되었습니다. 정수를 사용하는 것이 인터페이스보다 2.23배 더 빨랐다.

오늘은 여기까지입니다. 벤치마킹에 대해 배웠기를 바랍니다. 확실히 알고 있어요. 전체 코드는 여기에서 찾을 수 있습니다.

밖에 나가서 세계를 벤치마킹하는 것을 잊지 마세요.