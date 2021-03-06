---
layout: post
title: "좋아하는 Python IDE를 DevOps 도구로 변환"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![Code on a laptop](https://miro.medium.com/max/6000/0*zy4l0umCyqKKsXEi)

# DevOps란 무엇인가?

위의 정의는 나에게 맞지 않는다.

통치를 위한 중요한 역할이 있다. 그러나, 행동은 시작이며, 그러한 행동의 통치는 기껏해야 행동 과정의 성숙도 중간에 도착하지만, 대개는 나중에 도착한다.

나는 DevOps에 대한 나의 관점이 논쟁의 여지가 있다는 것을 인정한다. 나는 행동에 집중하고 통치에 거의 신경을 쓰지 않는다. 결국, 왜 프로세스가 실행 가능한 것으로 나타날 때까지 프로세스의 거버넌스에 값비싼 에너지를 낭비하는가?

아마도 내가 부사장이나 xEO급이라면 지배구조에 무게를 둘 것이다.

거버넌스는 워크플로우보다 "유행"이 빠르게 변화합니다. 나는 다음과 같은 거버넌스 방법론을 고려한다. Fallow, Agile, Scrum, 6시그마, 오프쇼어, 멀티 타임 존 등 여러분이 가장 좋아하는 거버넌스 방법론들이 10년입니다.

내가 제일 좋아하는? SDLC — 소프트웨어 개발 수명 주기. 시간이 없다. SDLC는 머신 러닝 응용 프로그램이 SDLC 방정식의 활성 부분이 되기 전까지 제가 가장 좋아하는 용어였습니다.

저는 9개 이상의 언어를 사용하여 30년 이상 코딩 작업을 해왔습니다. 이 기간 동안, 저는 SDLC 프레임워크와 툴의 여러 형태와 반복에 참여했습니다.

여기 "DevOps란 무엇인가?"에 대한 제 견해입니다.

# Python 지정 작업을 사용하여 DevOps 정의

이 기사에서는 파이썬 코드에서 DevOps(개발 운영 파이프라인) 사양을 만드는 방법을 다룰 것이다. 이 코드를 사용하여 즐겨 사용하는 IDE(Interactive Development Environment)를 DevOps 도구로 변환할 수 있습니다.

동료들이 로컬 샌드박스에서 사용하는 다양한 SDLC 파이썬 스크립트를 기반으로 한다는 것을 보여드리겠습니다.

이 기사에서 사용되는 파이썬 개발자 도구는 우리가 좋아하는 IDE에 사용된다.

# CI/CD 단계란?

나의 우주에서 SDLC 단계는 SDLC 조치와 동의어이다.

"연속 개발/연속 통합의 주요 단계는 무엇입니까?"에 대한 표준은 없습니다. 나는 거칠고 폭넓게 받아들여지는 정의를 제시할 것이다.

실제로, 제가 아는 대부분의 개발자는 보풀, 프로파일링, 복잡성 측정, 테스트 범위 결정 및 기타 진단 도구를 실행합니다. "진단"이 "테스트" 단계의 일부일 수 있습니까?

# CI/CD 단계 실행 방법

다음 Python 함수는 CI/CD 단계를 Python 하위 프로세스로 실행하는 방법을 보여줍니다. sp.open(...) 호출은 serial_script_cmd 셸 명령을 비동기 하위 프로세스로 실행합니다. 하위 프로세스 개체 인스턴스 `child`의 실행이 완료되면 `child.returncode` 호출이 반환됩니다.

멀티 코어 컴퓨터에서 자식 프로세스를 동시에 실행하려면 `devops_step(...)`을 변경할 수 있습니다.

유명한 DevOps 툴과 비슷한 보고서 형식을 가지도록 컬러라마를 추가했습니다.

# SDLC 도구

아래의 모든 SDLC 공구는 이 문서에 자세히 설명되어 있습니다.

## 1. 코드 포맷 표준화를 위한 검정색

검정색은 일반적인 PEP-8 스타일 가이드를 기반으로 하는 코드 형식 도구입니다.

```js
nstep = 1
return_code = 0
# 검은색 pep-8 포맷
serial_script_tests = 'black -v tests'
nstep,rc = devops_step(serial_script_class, nstep, return_code)
```

출력:

![Output](https://miro.medium.com/max/2588/1*SuSLHCdZHIAP0TB4cgHNnQ.png)

## 2. Pypy for Python 코드 타입 암시 확인

유형 힌트는 Mypy와 같은 타사 도구가 잠재적인 버그를 지적하는 데 사용할 수 있는 힌트입니다.

```js
#mypy 타입 확인
serial_script_src = 'mypy src'
nstep,rc = devops_step(serial_script_class, nstep, return_code)
```

출력:

![Output](https://miro.medium.com/max/1984/1*Zw_psD__C43-aLo1VhjoOw.png)

## 3. 코드 검토를 위한 Pylint

우리는 샌드박스에서 오류를 찾고 코드를 의심하기 위해 Pylint를 사용하고 있습니다.

```js
#필린트
serial_script_disc = 'pylint src -E -v'
nstep,rc = devops_step(serial_script_class, nstep, return_code)
```

출력:

![Output](https://miro.medium.com/max/2124/1*tTUdiTWfAqp4hKP5HN4RVA.png)

## 4. 코드 단위 테스트를 위한 py테스트

우리는 주로 최소의 보일러 플레이트가 필요하기 때문에 유닛 테스트를 위해 파이 테스트를 결정했습니다.

```js
#테스트용 py테스트
serial_script_tests = 'pytest -v tests'
nstep,rc = devops_step(serial_script_class, nstep, return_code)
```

출력:

![Output](https://miro.medium.com/max/3084/1*PGeiGJ5uuQ8fu6zUEZJ59A.png)

## 5-6. 테스트의 코드 적용 범위

커버리지.py는 우리의 pytest 프레임워크에서 다루는 코드의 양을 측정하기 위해 사용하는 도구이다.

```js
#the #theats
serial_script_test = 'serial run -mpytest tests'
nstep,rc = devops_step(serial_script_class, nstep, return_code)

serial_script_discret = 'serial report'
nstep,rc = devops_step(serial_script_class, nstep, return_code)
```

출력:

![Output](https://miro.medium.com/max/1956/1*cL6opsvWPWJqM6-P04YQUQ.png)

## 7-10. 아티팩트 버전 제어를 위한 Git 명령

우리는 로컬 파일 버전 제어를 위해 Git를 사용한다.

출력:

![Output](https://miro.medium.com/max/3596/1*efD_3xWGAGfx5cVUPCSBNQ.png)

![Output](https://miro.medium.com/max/3692/1*0AD6mgH6ccW6u0QmhxNVtA.png)

## 11. GitHub 버전 제어 아티팩트 공유를 위한 Git 명령

장치 테스트가 로컬 컴퓨터 중 하나를 통과하면 GitHub 클라우드에 대한 보고서로 코드를 출력합니다.

```js
serial_script_incips = 'git push -u 오리진' + pid
nstep,rc = devops_step(serial_script_class, nstep, return_code)
```

## 추가하거나 변경할 수 있는 기타 단계

참고: 위의 내용은 개발자 샌드박스 단계가 아닌 SDLC 프로젝트 릴리스 후보 작업(단계)입니다.

# 요약

이 코드는 깃허브(devops.py)의 오픈 소스다. 전체 스크립트는 다음과 같습니다.

SDLC(또는 내 DevOps 정의)를 통해 샌드박스에 얼마나 쉽게 적용할 수 있는지 확인해 보십시오. 좋아하는 IDE에서 OS(운영 체제)로 호출할 수 있는 언어를 사용합니다.

프로젝트 수준에서 DevOps용 CircleCI 또는 GitHub Actions 도구를 살펴보고 다음 Continuous Deployment 단계를 수행하는 것이 좋습니다.

해피 코딩!