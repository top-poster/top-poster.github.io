---
layout: post
title: "백엔드 개발에 모든 프로그래밍 언어를 사용할 수 있습니다 — Brainfuck도 마찬가지입니다."
author: "Logger"
thumbnail: "undefined"
tags: 
---


![person choosing one pastry from a display case](https://miro.medium.com/max/8544/1*pSRWG2xBE_HjW-CQyi1Dlw.jpeg)

웹 개발을 처음 접하고 웹 응용 프로그램의 백엔드에서 작업하려는 경우 응용 프로그램에 사용할 수 있는 최상의 언어가 무엇인지 궁금할 수 있습니다. 이 질문을 누구에게 하느냐에 따라 다른 답변이 나올 수 있습니다. PHP, JavaScript, C#, Python 및 기타 많은 언어를 들을 수 있습니다.

여기서의 진실은 어떤 프로그래밍 언어를 사용하는지는 그다지 중요하지 않다는 것입니다. 물론, 모든 개발자는 각자 선호하는 것이 있지만, 첫 번째 애플리케이션을 개발하는 경우에는 문제가 되지 않습니다. 두 가지 언어를 사용해 보고 그 언어에서 동일한 응용 프로그램을 개발하십시오. 어떤 언어가 가장 적합한지 알아보십시오.

매우 기본적인 수준에서 모든 백엔드 프로그래밍 언어는 클라이언트(예: 브라우저)가 이해할 수 있는 데이터를 생성하는 도구일 뿐입니다. HTML, JavaScript, CSS, JSON, Raw file data, 또는 다른 것이 될 수 있습니다.

PHP와 같은 언어를 사용하는 경우 PHP 코드를 실행하고 생성된 콘텐츠를 Apache 또는 nginx와 같은 클라이언트에 반환하는 프로그램을 실행합니다. C#에서 응용프로그램을 작성하는 경우 응용프로그램 자체에서 클라이언트와의 연결을 처리합니다.

브라우저가 이해할 수 있는 콘텐츠를 생성할 수 있는 응용 프로그램을 개발하는 동안 사용하는 언어는 중요하지 않습니다.

물론, 모든 언어에는 장단점이 있습니다. 인터넷에는 모든 인기 있는 프로그래밍 언어를 비교하는 수많은 목록이 있다. 그러나 이 기사는 그것에 관한 것이 아니다. 이 기사는 대중적인 언어들에 깊이 빠져들어 어떤 것이 가장 좋은지 설명하지 않을 것이다. 이 기사는 요점을 밝히는 것에 관한 것이다. 그리고 그 점은 언어를 선택할 때 가장 중요한 부분은 개인적인 선호라는 것입니다. 왜냐하면 언어는 그다지 중요하지 않기 때문입니다.

이 점을 증명하기 위해, 저는 브레인퍽으로 아주 간단한 웹 어플리케이션을 만드는 방법을 보여드리겠습니다. 우리가 할 일은 입력 매개 변수 하나를 가지고 그것을 인사말로 처리하는 것입니다. 왜 그럴까요? 할 수 있으니까.

# 브레인-뭐라고?!

브레인퍽(Brainfuck)은 1993년에 만들어진 매우 단순한 프로그래밍 언어이다. 실제 사용을 위한 것은 아니지만 튜링의 완성도는 높습니다. 이 테이프는 값을 쓰고 이 테이프의 다른 셀을 가리키도록 포인터를 이동할 수 있는 "테이프"를 사용하여 작동합니다. 이렇게 하려면 지시사항을 아주 간단하고 작은 단계로 나누어야 합니다.

Brainfuck에는 8개의 한자 명령만이 있다. >와 <<를 사용하여 테이프의 오른쪽과 왼쪽으로 셀 포인터를 이동합니다. ipson을 사용하여 포인터가 현재 가리키고 있는 테이프의 셀에 있는 값에 속하는 ASCII 문자를 출력합니다.

[]와 []를 사용하여 포인터가 가리키는 테이프의 값이 루프 시작 시 0이 아닌 경우 실행할 루프를 만들 수 있습니다.

ascular를 사용하여 입력에서 한 문자를 읽고 현재 선택한 셀에 ASCII 값을 넣습니다.

마지막으로, `+`와 `-`를 각각 사용하여 현재 선택한 셀의 값을 증가 및 감소시킵니다.

이제 Brainfuck의 전체 구문을 살펴보도록 하겠습니다. 이제 이 구문을 사용하여 웹 백엔드를 만드는 방법을 살펴보겠습니다.

# 브레인 퍽 스크립트 해석

Brainfuck에서 프로그램을 작성하기 전에, 우리는 통역사가 필요합니다. 우리의 Brainfuck 프로그램을 읽고 실제로 실행할 수 있는 프로그램이죠. 리눅스 시스템에서 실행할 애플리케이션을 목표로 하기 때문에, 이미 Ubuntu 패키지로 사용할 수 있기 때문에 Beef를 사용할 것입니다.

Beef를 사용하여 Brainfuck 스크립트를 실행하는 것은 쉽습니다. 명령줄을 통해 간단히 호출할 수 있습니다. 위의 연결된 Ubuntu 수동 페이지에서 Hello World 예를 다운로드하여 실행할 수 있습니다.

```undefined
육류 세계
```

안녕, 세상!이라는 반응을 단말기에 담아야 한다.

스크립트의 경우 스크립트에 제공할 입력을 stdin에 전달해야 합니다. 우리는 다음과 같이 보이는 Beef의 내용물과 배관을 `echo`하여 이것을 할 것이다.

```undefined
"MyInput"을 에코하다. | 비프 스크립트.b
```

# 단순 스크립트 작성

Brainfuck을 사용하여 백엔드를 구축할 수 있다는 것을 보여주기 위해 간단한 사용 사례를 만들고 있습니다. 저희가 만들 것은 URL의 경로에서 이름을 따온 인사말 스크립트입니다. 그래서 우리가 `http://our-web-service/David`를 방문했을 때, "Hello, David!"라고 쓰여진 제목을 보고자 합니다. 이를 위해 경로의 첫 번째 문자(`/David`)를 벗겨내어 이름을 얻은 다음 문자열에 추가합니다. PHP에서는 다음과 같이 보입니다.

이제 이걸 브레인퍽에 써보죠. 이것은 Brainfuck이기 때문에, 이것은 단순한 하나의 라이너입니다.

어, 뭐라고요?

이 프로그램은 이미 Brainfuck 프로그래밍 언어가 그 이름에 걸맞는다는 것을 보여준다.

이 프로그램은 셀 1부터 9까지 30부터 110까지의 값으로 채우고 이 값을 사용하여 `<h1>Hello>를 인쇄한다. 그런 다음 모든 입력 데이터를 읽고 셀 11 이상에서 배치합니다. 그런 다음 10번 셀로 루프하고 첫 번째 문자를 제외한 모든 읽기 입력 데이터를 인쇄합니다(따라서 URL 경로에 있는 첫 번째 `/`는 포함하지 않습니다).

Beef에 입력을 전달하기 위해 사용하는 입력 방법(Beef에 입력되는 것과 배관을 `echo`로 입력되는 것)은 입력 끝에 새 줄을 포함하므로 입력의 마지막 문자도 생략합니다.

그 후, 미리 채워진 셀로 돌아가 최종 `!/h1`을 인쇄한다. Brainfuck 프로그램을 분해해봅시다. 백색공간, 댓글, 테이프 상태 등을 더해서 실제로 무슨 일이 일어나고 있는지 볼 수 있도록 하죠.

이제 이 대본을 쓰고 `hello.b`로 저장했으니 비프와 함께 실행해 보자. 우리는 조롱당한 경로를 `에코`하고 그 경로를 스크립트에 연결함으로써 스크립트에 입력을 제공합니다. 우리가 목표로 하는 조롱받는 경로는 `/YourName`입니다. 그것이 우리가 웹 서버를 위해 작업하기를 원하는 경로이기 때문입니다.

```undefined
~$ 에코 "/데이비드" | 쇠고기 안녕.b
안녕, 데이비드!</h1>
```

좋아, 됐다! 이제 요청 시 이를 브라우저에 제공할 수 있는 방법을 알아보겠습니다.

# 서비스합시다!

![photo of a server](https://miro.medium.com/max/1600/1*o1ZM1DiE7Jia-bAPYTUcuQ.jpeg)

이제 우리는 HTML 콘텐츠를 생성하기 위한 Brainfuck 스크립트를 작성했으므로, 이를 위한 방법이 필요합니다. 우리는 브레인퍽을 이 일에 사용하지 않을 것입니다. 가능하다면, 이것은 완전히 미친 짓이 될 것입니다. 오늘날, 우리는 단지 규칙적으로, 건강한 양의 광기를 목표로 하고 있습니다.

우리는 파이썬에서 간단한 HTTP 서버를 설정하려고 합니다. 여러분은 이것을 부정행위로 보고 우리가 지금 Brainfuck에서 프로그래밍하고 있지 않다고 말할 수 있지만, PHP로 웹 애플리케이션을 프로그래밍할 때, Apache orginx와 같은 외부 프로그램을 사용하여 컨텐츠를 서비스할 수 있습니다. 그리고 아무도 Apache에서 애플리케이션을 작성했다고 주장하지 않습니다.

Python에서 수행하는 작업은 포트 80(기본 HTTP 포트)에서 도착하는 모든 GET 요청에 응답하고 적절한 헤더를 전송하며 제공된 경로를 사용하여 Brainfuck 스크립트를 호출하는 것입니다. 이것은 좋거나 효율적인 설정은 아니지만 개념 증명으로 작동합니다.

우리는 지금 `python server.py`고, 우리의 스크립트 `/app/hello.b`에 저장되어서 저희 브레인퍽 스크립트를 호출한 실무 웹 서버가 있어야 한다 이 대본을 달릴 수 있다. 하지만 결과를 확인하기 전에 한 가지 단계를 더 밟겠습니다.

# 도커 컨테이너에 포장

모든 종속성을 로컬에서 설정하지 않고 웹 응용 프로그램을 실행하기 위해 도커 컨테이너를 만듭니다. 우리는 이것을 파이썬 이미지를 기반으로 하고 비프를 설치하고 파이썬과 브레인퍽 스크립트를 거기에 넣습니다.

이 도커 파일을 사용하면 애플리케이션을 실행할 컨테이너를 만들 수 있습니다.

```undefined
도커 빌드-트 브레인 퍽-서버.
```

컨테이너가 성공적으로 생성되면, 서버를 실행해봅시다. 그러면 우리는 Brainfuck으로 구동되는 웹사이트를 방문할 수 있습니다. 호스트 컴퓨터에서 실행 중인 다른 서비스와의 충돌을 방지하기 위해 컨테이너의 포트 80을 호스트 시스템의 포트 8080에 노출합니다.

```undefined
도커 실행 -p 8080:80-it brainfuck-server
```

그리고 지금은 최고입니다. 브라우저를 열고 무엇이 있는지 봅시다! `http://localhost:8080/YourName`으로 이동하여 결과를 확인하십시오.

![screenshot showing the words Hello David](https://miro.medium.com/max/838/1*mOMHJbjTY_s-YVb5IZJ9vw.png)

우리가 해냈어! 매우 간단한 결과를 얻기 위해 많은 노력을 기울였습니다.

# 우리는 이것으로부터 무엇을 배울 수 있을까?

Brainfuck에서 웹 애플리케이션의 백엔드를 작성하는 것은 너무 복잡하고 매우 어렵습니다. 그러나 Brainfuck으로 출력을 생성하여 최종 사용자에게 제공할 수 있습니다. 이는 웹 백엔드에 사용하는 프로그래밍 언어가 그다지 중요하지 않다는 것을 보여주므로 다른 방법을 사용해 보십시오!

다양한 언어, 프레임워크 및 설정을 통해 사용자에게 가장 적합한 기능을 확인하십시오. 몇 가지 실험을 한 후, 여러분은 다른 언어의 장단점을 알게 될 것이고, 여러분에게 어떤 것이 효과가 있는지 알게 될 것입니다.

하지만 가장 중요한 것은, 그렇게 하는 동안 즐겁게 지내세요!

해피 코딩!