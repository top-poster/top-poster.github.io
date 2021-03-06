---
layout: post
title: "디버깅은 어렵습니까? 더 쉽게 만드는 방법 알아보기"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![Ladybug on white surface](https://miro.medium.com/max/7776/1*zyQBZfbmwwYuELzfZ0jYBA.jpeg)

심판의 날이었다. 나는 지난 한 달 동안 특집기사를 하고 있었다. 그날 오후, 우리 팀은 CEO에게 그 일을 발표하기로 되어 있었다.

나는 꽤 자신감에 차 있었다. 전날, 한 테스터가 아침 커피 마시는 동안 고쳐야 할 몇 가지 사소한 벌레들을 보고했습니다.

한 가지 과제는 그 회사들의 로고를 표로 보여주는 것이었다. 당연하죠. 그런데 이상했어요. 아까 고친 것 같았어요. 나는 심지어 내가 어떻게 간단한 논리를 망칠 수 있는지 궁금했다.

실수를 하지 않는 유일한 사람은 아무것도 하지 않는 사람이다. 나는 코드에서 느낌표 하나를 지우고 그 일을 해야 한다고 믿었다.

그렇지 않았다.

한 시간 반 후에, 저는 14개의 파일을 만졌고 250개의 코드 라인을 바꿨습니다. 나는 그 고정이 효과가 있기를 기도했다.

# 디버깅이 어렵습니다.

우리가 컴퓨터 프로그램을 만들기 시작했을 때, 우리는 우리가 항상 그것을 망치고 있다는 것을 즉시 알아차렸다. 설상가상으로, 우리의 실수를 고치는 것이 쉽지 않았어요. Brian Kernighan의 표현대로라면:

그런데 왜 디버깅이 그렇게 어려운 걸까요? 이 모든 것은 복잡함으로 귀결된다.

가장 단순한 앱도 거대한 코드 기반 위에 구축된다. 현대의 개발자들은 많은 도구들을 무료로 얻을 수 있습니다. 즉, 잘 이해하지 못하는 도구입니다. 우리는 그것들을 사용할 수 있지만, 어떻게 작동하는지 모릅니다.

특히 엉성한 오류 메시지나 복잡한 스택 추적을 풀면 복잡성이 우리를 엄습한다. 종종 오류와 증상 사이의 코드 공간이 큽니다.

우리의 시스템은 복잡하지만, 우리의 입력 또한 문제를 일으킬 수 있습니다. 최종 사용자에게 버그가 있지만 컴퓨터에서 볼 수 없는 상황이 자주 발생합니다. 버그는 구성과 관련이 있거나 브라우저 버전 또는 이벤트 순서에 따라 다를 수 있습니다. 어떤 벌레들은 팀을 이루어 일을 하고 그림을 더 흐리게 한다.

또한 하나의 오류로 인해 다양한 버그가 발생할 수 있습니다. 설상가상으로, 실수를 고치는 것은 종종 새로운 실수를 만들어 낼 수 있다. 이는 일반적으로 코드를 이해하지 못한 상태에서 구현된 테스트 또는 엉성한 수정 사항을 나타냅니다.

![Man in hazmat suit watering plant](https://miro.medium.com/max/12000/1*HzrnTwdfZ9NcOO0SNiBH1A.jpeg)

# 더 효율적으로 디버깅하는 방법

우리 모두는 디버깅이 어렵다는 것을 인정하지만, 어떻게 하면 더 쉽게 만들 수 있을까?

## 버그 찾기

디버깅을 배우려면 다음 중 하나를 이해해야 합니다. 코드를 추가 또는 삭제하거나 모든 것을 로깅하는 것이 아닙니다. 디버깅의 본질적인 부분은 타이핑과 무관하다. 그것은 사고와 모든 관련이 있다.

디버깅을 시작하기 전에 코드를 생각하고 분석해야 합니다. 뭐가 잘못되었다고 생각하세요? 벌레가 무엇이든 간에, 단지 몇 분 동안 깊이 생각한 후에, 여러분은 가설을 세워야 합니다. 아마 몇 개라도.

다음 단계는 무엇입니까?

가장 가능성이 높은 가설에서 시작하여 가설을 검정합니다. 테스트는 입력(예: 업로드에 대한 다른 파일 크기 또는 형식) 변경, 다른 브라우저를 사용하거나, 사용자가 OK라고 간주하는 코드 부분을 주석 처리하여 구성할 수 있습니다. 목표는 버그를 최대한 줄이는 것이다.

이제 버그의 위치에 대해 상당히 자신감을 가져야 합니다. 그렇지 않다면, 여러분을 도울 수 있는 추가 기술이 있습니다: 분열과 정복.

그 기술에 대한 기사를 썼는데, 그 본질은 간단하다. 코드베이스를 반으로 자르고 로그를 사용하여 버그가 전반부인지 후반부인지 확인합니다. 이제 버기 부분을 다시 반으로 자르세요. 버그를 몇 개의 코드 줄로 좁힐 때까지 이 과정을 반복합니다.

## 버그 수정 중

버그를 찾았군요. 이제 그것을 고칠 시간이다. 코딩 작업을 즉시 받고 싶은 충동을 억누르십시오. 언제나 디버깅과 마찬가지로, 생각하는 것이 하는 것보다 더 중요하다.

입력과 출력, 앱의 흐름을 이해하십니까? 오류가 발생한 이유를 정확히 알기 위해서는 코드의 정신 모델을 현실에 맞게 조정해야 합니다.

때로는 빠른 수정이 가능하지만, 무슨 일이 일어나고 있는지 완전히 이해하지 못할 수도 있습니다. 코드가 실패한 모든 사례를 확인합니다. 랜덤 입력으로 테스트해 보십시오. 예상대로 되나요?

수정 후 모든 사례(성공 및 실패)를 다시 시도합니다. 이제 아시겠습니까? 문제를 해결했는데도 여전히 실패가 무엇인지 잘 모르겠으면 작업이 완료되지 않습니다. 무슨 일이 일어나고 언제 일어나는지 완전히 이해할 때까지 빈둥빈둥 놀아라. 그렇지 않으면, 여러분은 다시 그 벌레에 걸려 넘어질 것입니다. 아마도 다른 곳에서요. 그리고 여러분은 여전히 무슨 일이 일어나고 있는지 모를 것입니다.

# 결론

"수리가 잘 되었나요?"라고 궁금할 수도 있습니다. 네! 간신히 성공했고 발표도 잘 됐어요.

입력에 대한 오해가 나의 버그를 야기했다. 저는 데이터베이스에서 반환된 거대한 물체에 대해 연구했고 일부 속성에 대한 잘못된 인식을 가지고 있었습니다. 버그는 부울논리의 단순한 오류처럼 보였지만 시스템에 대한 큰 오해였다.

그 사건은 나에게 어떤 것도 당연하게 여기지 말라고 일깨워 주었다. 프로그래밍은 어렵다. 디버깅이 어렵습니다. 때때로 겉보기에는 단순한 벌레들이 여러분의 정신 모델의 주요한 결함을 발견하도록 도와줄 수 있습니다. 그것으로부터 배우세요.

디버깅에 실패했습니까? 얼마든지 댓글로 공유해주세요.