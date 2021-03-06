---
layout: post
title: "도커로 주피터 노트북을 공유하는 방법"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![Image of a whale](https://miro.medium.com/max/2000/0*QGWAaIuMrVGx6vzL)

데이터 과학자로서 우리는 우리의 연구가 재현될 수 있기를 원한다. 즉, 우리가 분석을 공유할 때 모든 사람들이 그것을 다시 실행하고 동일한 결과를 도출할 수 있어야 한다는 것을 의미한다. 서로 다른 운영 체제(iOS, Windows, Linux)와 다른 프로그래밍 언어 버전 및 패키지를 다루고 있기 때문에 이것이 항상 쉬운 것은 아닙니다. 그렇기 때문에 Conda 환경과 같은 가상 환경에서 작업할 것을 권장합니다. 콘다 환경의 또 다른 강력한 솔루션은 도커스와 협력하는 것입니다.

시나리오: 우리는 자체 데이터에 파이썬 주피터 노트북을 사용하여 분석을 실행했으며, 우리는 이 분석을 다양한 커뮤니티와 공유하여 모든 사람이 결과를 재현할 수 있도록 하고 있다.

# 로컬에서 분석 실행

단순성을 위해 다음 분석을 실행했다고 가정해 보겠습니다.

![Image for post](https://miro.medium.com/max/1896/0*vrUZGjrAE8LDbmu0.png)

본질적으로, 나는 판다스, 숫자피, 그리고 vader Sentiment 라이브러리를 사용하여 my_data.csv에 대한 감정 분석을 실행하려고 한다. 그래서 저는 이 주피터 노트북을 공유하고 싶고, 플러그 앤 플레이가 되었으면 합니다. 데이터 및 필수 라이브러리를 비롯하여 주피터 노트북이 포함된 도커 이미지를 만드는 방법을 살펴보겠습니다.

# 주피터 도커 스택

주피터 도커 스택은 주피터 응용 프로그램과 대화형 컴퓨팅 도구를 포함하는 실행 준비된 도커 이미지 세트입니다. 스택 이미지를 사용하여 다음 중 하나를 수행할 수 있습니다.

Jupyter/Scipy를 기반으로 맞춤형 이미지를 구축하겠습니다.

# 요구 사항을 작성합니다.txt 파일

주피터 도커 코어 이미지에는 가장 일반적인 라이브러리가 포함되어 있지만 추가 설치가 필요할 수도 있습니다. 우리의 경우와 마찬가지로, 우리는 `vader Sentiment==3.3.2` 라이브러리를 설치하기를 원했습니다. 이것은 우리가 `요구사항`을 만들어야 한다는 것을 의미한다.txt 파일

`요구사항`입니다.txt` 파일은 다음과 같습니다.

```undefined
vader Sentiment== 3.3.2
```

# 도커 파일 만들기

이제 다음과 같이 `Docker 파일`을 생성해야 합니다.

```undefined
FROM 주피터/스파이 노트북
요구사항 복사.txt./요구사항.txt의
my_data.csv ./my_data.csv 복사
my_jupyter.ipynb./my_jupyter.ipynb 복사
RUN pip install -r 요구 사항.txt의
```

그래서 우리는 `주피터/스파이 노트북`으로 우리의 이미지를 시작한 다음, 필요한 파일을 우리의 로컬 컴퓨터에서 이미지로 복사합니다. 경로와 디렉토리를 사용할 수 있었습니다. 마지막으로, 요구 사항에 필요한 라이브러리를 설치합니다.txt 파일

# 도커 파일 빌드

도커 파일을 만들었기 때문에, 우리는 그것을 만들 준비가 되었습니다. 이름을 지정할 수 있습니다. 나는 그것을 내 공유 노트북이라고 부르기로 했다. 팁: 그 기간을 잊지 마세요!

명령은 다음과 같습니다.

```undefined
$docker build - 내 공유 노트북.
```

이미지가 생성되었는지 확인하려면 다음을 입력합니다.

```undefined
$도커 이미지
```

도커 이미지를 가져올 수 있습니다.

# 이미지 실행

이미지가 예상대로 실행되고 있는지 확인하려면 다음을 실행합니다.

```undefined
$도커 실행 -p 8888:8888 내 공유 노트북
```

그리고 우리는 주피터 노트북에 대한 링크를 얻을 것입니다!

![Image of Jupyter notebook](https://miro.medium.com/max/2048/0*Wx8hvDwPQYwiUshJ.png)

실행 중인 컨테이너를 확인하려면 다음을 입력합니다.

```undefined
$dockerps-a
```

# 이미지를 도커 허브로 밀어넣기

이미지가 예상대로 작동하는지 확인한 후 모든 사용자가 이미지를 끌어당길 수 있도록 도커 허브로 밀어넣을 수 있습니다. 당신이 가장 먼저 해야 할 일은 당신의 이미지에 태그를 붙여야 합니다. 다음 명령을 사용할 수 있습니다.

```undefined
$ 도커 태그 9811503b3d3a gpipis/내 공유 노트북: first
```

9811503b3d3a는 이미지 ID이며, docker images 명령에서 얻은 것이다. gpipis는 내 사용자 이름이고, 공유 노트북은 위에서 만든 이미지 이름입니다. 마지막으로 `:first`는 옵션 태그입니다.

이제 다음을 입력하여 이미지를 푸시할 준비가 되었습니다.

```undefined
$docker push gpipis/내 공유 노트북: first
```

![Image for post](https://miro.medium.com/max/1344/0*wrxJEW8Ndqie_rs7.png)

# 도커 허브에서 이미지 꺼내기

위의 일은 자신의 일을 공유하고 싶은 사람이 합니다. 이제 이 이미지를 어떻게 얻을 수 있는지 보고 재현 가능한 주피터 노트북에 대해 알아보겠습니다.

우리가 해야 할 일은 다음을 입력하여 이미지를 `끌어당기는` 것입니다.

```undefined
$docker pull gpipis/내 공유 노트북: first
```

이제 다음을 입력하여 실행할 준비가 되었습니다.

```undefined
$docker run -it -p 8888:8888 gpipis/내 공유 노트북: first
```

![Image of Jupyter deprecation notice](https://miro.medium.com/max/2048/0*jW8-qkwqmHqPMnSU.png)

URL을 복사하여 브라우저에 붙여넣으면 다음과 같은 이점이 있습니다.

![Image for post](https://miro.medium.com/max/2048/0*oefpUUHFIQC7iRrJ.png)

포트를 변경할 수 있습니다. 예를 들어 `8889`에서 실행하려면 다음을 입력합니다.

```undefined
$docker run -it -p 8888:8888 gpipis/내 공유 노트북: first
```

또한 URL의 포트로 변경해야 합니다.

```undefined
http://127.0.1:8889/?http=7e767d9a8dbb92e9d93ce7a5f52ba3c524a3cfcc65401714
```

# 더 테이크어웨이

여러 사람과 작업을 공유하고 분석을 재현할 수 있기를 원할 때 도커와 함께 작업하는 것이 가장 좋습니다.