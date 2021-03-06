---
layout: post
title: "Python에서 tqdm을 사용하여 진행 표시줄 표시"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![abstract pattern of swirling lights](https://miro.medium.com/max/10368/0*l8BWVt1iMRGwGHj7)

현재 실행 중인 파이썬 코드에 남은 시간이 얼마나 되는지 궁금해 본 적 있나요? 알고 보니 쉽게 알아낼 수 있는 방법이 있더군요. 어떻게요? tqdm Python 라이브러리를 사용하여 Python 코드에 진행 표시줄을 추가합니다.

이 튜토리얼에서는 tqdm 라이브러리를 사용하여 코드가 실행될 때(팬더에서도) 스마트 진행 표시줄을 표시하는 방법에 대해 알아봅니다. 또한 주피터 노트북에서도 시각적으로 더 매력적인 옵션을 어떻게 사용할 수 있는지 알아보겠습니다.

# tqdm

아랍어에 익숙하다면 tqdm의 발음이 진전을 의미하는 아랍어 단어(taqadum, ت➡)와 같다는 것을 알아챘을 것이다. 따라서 이 진행률 표시줄 라이브러리에 적합한 이름입니다.

Tqdm을 사용하여 Python 루프에 대한 진행률 막대를 표시하는 방법을 살펴보겠습니다.

# tqdm 설치

먼저 tqdm을 설치해야 합니다. 명령 프롬프트 또는 터미널에서 다음과 같이 pip install을 사용할 수 있습니다.

```undefined
pip 설치 tqdm
```

아나콘다 파이썬 배포판이 있는 경우 tqdm이 이미 설치되어 있을 수 있습니다. 그렇지 않은 경우 콘다 설치도 사용할 수 있습니다. pip list를 사용하여 설치된 Python 패키지를 확인할 수 있습니다.

# tqdm 가져오기

다음으로, 우리는 우리의 코드로 tqdm 라이브러리를 가져와야 합니다. 구체적으로 다음과 같이 tqdm 라이브러리에서 tqdm 함수를 가져올 필요가 있다.

```undefined
tqdm 가져오기 tqdm에서
```

우리는 전체 tqdm 라이브러리를 가져오면 `tqdm.tqdm(itable)`과 같은 `tqdm` 기능에 접근할 수 있다.

# tqdm 사용

`tqdm` 함수를 사용하여 파이썬 루프에 대한 진행 표시줄을 표시하기 위해 모든 해당 항목을 `tqdm(iterable)`로 포장하기만 하면 된다. 주피터 노트북의 예를 살펴보겠습니다.

![screenshot of a progress bar](https://miro.medium.com/max/2410/1*XqZ9BuECRJGe5Qub2Npjlw.png)

우리는 이 예에서 범위 객체인 대상자를 `tqdm` 함수로 포장했다. 진행 표시줄은 주피터 노트북 셀 아래에 있습니다. `for` 루프가 실행 중이면 {eltaxed time}<{naming}시간 및 초당 반복이 표시됩니다.

![animation of progress bar](https://miro.medium.com/max/2400/1*LxsIE2Cm8P-ov7Y68nkErQ.gif)

# 다른 루프 또는 기능과 함께 tqdm 사용

tqdm을 for 루프와 함께 사용할 수 있을 뿐만 아니라, 반복할 수 있는 다른 기능과 함께 사용할 수도 있다. 예를 들어 map, filter, reduce 함수와 함께 사용할 수 있다.

예를 들어, 숫자를 포함하는 함수를 작성하고 `축소` 함수를 사용하여 0과 전달된 숫자 사이의 모든 정수의 합을 반환하면 다음과 같은 진행률 막대를 포함할 수 있다.

```undefined
추가(숫자):
(num) 유형 == 다른 경우 return reduct (x+y, tqdm(범위(num+1))) (반환율 x,y: x+y,tqdm)(범위(num+1))) 0
```

우리는 `tqdm` 기능으로 우리의 반복 가능한 객체인 범위 객체를 포장하여 진행 표시줄을 표시할 수 있다. if 문은 입력이 정수인지 여부를 확인하는 것이며, 그렇지 않으면 함수는 0을 반환합니다.

![screenshot of progress bar](https://miro.medium.com/max/2418/1*D_68uaqw9Y88X9pQ23e6oA.png)

# 주피터 노트북의 더 나은 옵션

주피터 노트북에서 작업하는 경우 tqdm.노트북 하위 모듈에서 대신 `tqdm` 기능을 가져올 수 있습니다. 이 모듈은 일부 색상 힌트(일반, 완료, 녹색, 오류/중단, ETA 없음)를 포함하는 보다 시각적으로 매력적인 진행 표시줄을 제공합니다.

```undefined
tqdm.dm 가져오기 tqdm에서
추가(숫자):
(num) 유형 == 다른 경우 return reduct (x+y, tqdm(범위(num+1))) (반환율 x,y: x+y,tqdm)(범위(num+1))) 0
```

![animation of progress bar in color](https://miro.medium.com/max/2400/1*M2WQIbkaeNYsxpVUQE1eHg.gif)

또한 다음과 같은 목록 내에서 tqdm을 사용할 수 있습니다.

![Image for post](https://miro.medium.com/max/2374/1*LIh7NB_ayXI0NKOuqts9BQ.png)

# 판다와 함께 tqdm 사용

우리는 심지어 팬더와 tqdm을 사용할 수도 있어요! 예를 들어, 우리는 `dataframe.apply()` 방법으로 진행 표시줄을 표시할 수 있다.

먼저 `.pandas()` 방법을 시작해야 합니다.

```undefined
tqdm.dmpcs
```

그러면 `.apply()를 `.progress_apply()`로 바꾸면 끝!

```undefined
df = pd.DataFrame(np.random.randint(0, 100, (100,000, 600))
tqdm.dmpcs
# 이제 우리는 '응용'이 아닌 '진행_응용'을 사용할 수 있다.
# 및 "map" 대신 "progress_map"
df.progress_apply(프로그레스 x: x**2)
```

![screenshot of progress bar used with pandas table](https://miro.medium.com/max/2230/1*p4D3WtNq7p-ORcg_9j4eFg.png)

판다와 함께 tqdm 사용에 대한 자세한 내용은 설명서를 참조하십시오.

# tqdm의 이점

tqdm은 Linux, Windows, Mac 등을 포함한 모든 플랫폼의 모든 콘솔 또는 GUI에서 작동합니다. 위에서 살펴본 바와 같이 주피터 노트북과 호환됩니다.

또한, 진행률 표시줄을 표시하는 다른 방법 대신 tqdm을 사용하면 tqdm의 오버헤드가 거의 없으며, 이는 반복당 약 60나노초라는데, 이는 반복당 800나노초의 오버헤드를 갖는 ProgressBar와 비교할 때 성능에 큰 영향을 주지 않는다는 것을 의미한다. 정교한 알고리즘을 사용하여 우리 코드에 남아 있는 시간을 예측하고, 성능을 희생시키는 불필요한 반복 디스플레이를 건너뛰는 방식으로 그렇게 한다.

마지막으로, tqdm은 작동하는 데 종속성이 필요하지 않습니다.

# 요약

이 튜토리얼에서는 파이썬 코드에 남아 있는 시간을 추적할 수 있는 방법을 살펴 보았다. 우리는 `tqdm` 함수로 해당 항목을 감싸서 코드에서 루프를 위한 스마트 진행 표시줄을 표시하기 위해 tqdm 라이브러리를 사용하는 방법을 배웠다. 또한 주피터 노트북에서 작업하는 경우 tqdm.노트북 하위 모듈에서 보다 시각적으로 매력적인 진행 표시줄을 사용할 수 있음을 확인했습니다. 마지막으로, 우리는 판다와 함께 일할 때 어떻게 tqdm을 사용하여 진행 표시줄을 표시할 수 있는지 보았습니다.

tqdm에 대한 자세한 내용은 설명서를 참조하십시오.