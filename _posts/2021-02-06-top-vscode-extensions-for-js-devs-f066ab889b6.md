---
layout: post
title: "JS Dev를 위한 상위 8개 VS 코드 확장"
author: "Logger"
thumbnail: "null"
tags: 
---


![Woman standing in a pink neon-walled room with black floors and ceiling](https://miro.medium.com/max/11966/1*_Im77S9xrmSFSBy2lNbvPA.jpeg)

다음 목록은 VSCode 확장 스토어에서 만든 행운의 발견과 구문, 전체 스택 라디오, 뷰 온 뷰와 같은 제가 좋아하는 개발 팟캐스트의 큐레이션된 제안의 조합입니다.

# 버전 렌즈

한 번의 클릭으로 패키지 버전을 업데이트합니다.

![animation of screenshot from Version Lens](https://miro.medium.com/max/1900/1*sC6IZjuVTQ-KeI6NSaF0kQ.gif)

버전 렌즈는 당신을 `와!`라고 말하게 만드는 플러그 인 중 하나이다.

설치 후 오른쪽 상단 모서리에 있는 큰 V을 클릭한 다음 버전 태그 아이콘을 클릭하여 현재 사용 중인 모든 패키지의 현재 버전을 가져옵니다. 클릭 한 번으로 추가 검색 없이 패키지를 업데이트할 수 있습니다. 대단해!

# 가져오기 비용

해당 가져오기를 위해 얼마나 많은 크기를 희생하는지 확인하십시오.

![screenshot from Import Cost](https://miro.medium.com/max/2126/1*8rD082BGhnjfRvG75zWJZQ.png)

외부 라이브러리를 사용할 때, 외부 라이브러리를 사용할 때 우리가 부담하는 크기 비용을 완전히 고려하는 경우는 많지 않습니다. Import Cost(가져오기 비용)는 특정 패키지를 사용할 때 프로젝트의 무게가 얼마나 더 무거워지는지 정확하게 보여주기 위해 여기에 있으며 지퍼 크기가 표시됩니다!

패키지 가져오기가 어려운 것으로 간주될 경우 Import Cost의 강조 표시된 텍스트가 녹색에서 빨간색으로 변경됩니다. 뚱뚱한 패키지를 사용하는 것도 괜찮을지 모르지만, 이러한 빨간 선을 보면, 여러분은 여러분의 프로젝트 요구를 해결하기 위해 더 가벼운 것을 찾아보고자 할 수 있습니다.

# 벌른

가져온 패키지의 취약성을 검색합니다.

Vuln을 설치한 후 `패키지`로 이동합니다.json` 파일을 보고 목록의 각 항목을 확인하여 알려진 취약성이 있는지 확인합니다. 이 경우 다음과 같이 표시됩니다.

![screenshot from Vuln](https://miro.medium.com/max/2126/1*WbZv364_wJhl8PbfT48XKA.png)

문제 위를 맴돌면 정확히 어떤 취약점이 있는지 확인할 수 있는 팝업이 표시되고 문제 해결 옵션이 제공됩니다(사용 가능한 경우).

![screenshot from Vuln](https://miro.medium.com/max/2126/1*Q5d0iy0IvjvUVAKQzwYQNQ.png)

이제 설치된 각 패키지를 수동으로 검색하고 두 번 검사하는 데 시간을 들이지 않고도 프로젝트를 안전하고 취약점이 없도록 할 수 있습니다.

# 작업 트리

모든 항목을 쉽게 검토프로젝트의 TODO.

![screenshot from Todo Tree](https://miro.medium.com/max/2126/1*S_D6l3ISkMF8ve7OVVCl7Q.png)

설치 후 VS Code 사이드바에서 트리 아이콘을 클릭하여 Todo Tree를 활성화합니다. `TODO`가 있는 파일만 파일 패널에 표시됩니다.

이렇게 하면 `TODO`를 쉽게 나열할 수 있을 뿐만 아니라 템플릿에서 각 항목을 강조 표시할 수 있습니다. 이 작업을 수행하기 위해 이전에 TODO Highlight를 사용하고 있었습니다. 그러나 Todo Tree에도 이 기능이 포함되어 있으므로 이제 모든 `TODO` 목록에 필요한 플러그인이 하나만 필요합니다.

이것은 프로젝트의 리팩터링 단계에 특히 편리한 플러그인입니다.

# 스냅스냅

사용하는 모든 언어의 조각에 액세스하는 방법을 표준화합니다.

![animated screenshot from SnipSnap](https://miro.medium.com/max/3332/0*-MVKuFL-xyDrToxr.gif)

SnipSnap은 다음과 같은 몇 가지 문제를 해결하기 위해 구축되었습니다.

SnipSnap은 현재 베타 버전이지만 이 기능은 이미 매우 유용하며 여기서만 개선됩니다.

사용하는 모든 언어에 대한 스니펫을 설치한 다음 각 언어의 작동 방식과 개발자가 선택한 개별 언어를 파악하는 데 시간을 낭비하는 일은 잊으십시오. SnipSnap을 사용하고 스니펫을 쉽게 사용하면 됩니다.

사용자 지정 스니펫을 사용하시겠습니까? 아래의 `JSON` 형식을 사용하여 자신만의 조각을 추가하고 관련 언어 또는 프레임워크에 따라 액세스할 수 있도록 하십시오.

![screenshot from SnipSnap showing custom snippet format](https://miro.medium.com/max/2126/1*NlvSVZDWCUFq_YviGpVJVQ.png)

# 코드 타임

최고의 코딩 플로우 상태를 발견하는 글로벌 커뮤니티.

![Code Time dashboard](https://miro.medium.com/max/2264/1*Zusyh0gpRADI9d-E326tdA.png)

코드타임은 여러분의 손가락이 키보드를 가로질러 날아가는 놀라운 흐름의 상태로 여러분을 가두는 것입니다. 그리고 여러분은 코딩 신과 같은 프로젝트 특징을 만들어냅니다.

매일 사용하지 않을 수 있는 플러그 인 중 하나이지만, 이 플러그 인을 사용하면 진행 상황을 추적하기 위해 이 플러그 인을 설치했다는 사실에 감사하게 될 것입니다.

하루 중 특정 날이나 시간에 얼마나 효과적인지 등 코딩 생활에 대한 상세한 개요를 제공하며, 코딩을 가장 잘 할 때 몸의 생체리듬을 통찰할 수 있을 것 같다. 이제 그것은 유용한 정보입니다!

# 뮤직 타임

플로우 상태에 대한 개발자 통계의 글로벌 커뮤니티입니다.

![screenshot from Music Time](https://miro.medium.com/max/2264/1*-JG-crVd0fa9UgR_bhYEvQ.png)

코드 타임의 크리에이터로부터 또 다른 놀라운 플러그인이 나옵니다!

간단히 말해, 뮤직 타임은 당신이 가장 잘 코딩하는 음악을 볼 수 있는 뷰의 잠금을 해제한다.
Spotify 계정을 연결하고 코딩을 시작하십시오. 뮤직타임이 나머지를 할 것이다. 며칠 동안 사용한 후에는 청취한 음악을 바탕으로 언제 가장 생산적이었는지 다시 확인하십시오.

어디서부터 시작해야 할지 모르겠나요? 전 세계의 다른 개발자들이 어떤 음악을 가장 효과적으로 코드화하는지 그들의 글로벌 통계를 보면, 아마도 당신은 다음에 좋아하는 아티스트를 찾을 수 있을 것이다.

# 보너스 — i18n 앨리

템플릿에 직접 지역화된 모든 언어를 표시, 변환 및 편집합니다.

![screenshot from i18n Ally](https://miro.medium.com/max/2126/1*15pB4tTKRSeG0Ut0_coplA.png)

이것은 제가 이전 기사 중 하나에서 언급했기 때문에 보너스입니다. 하지만 인터페이스를 대폭 개선한 만큼 다시 한번 짚어볼 만하다고 생각합니다.

i18nAlli가 제공하는 놀라운 몇 가지 기능은 다음과 같습니다.

또한 위의 이미지에서 흰색 음성 버블 아이콘을 클릭하여 얻을 수 있는 편집 UI의 확장 버전도 함께 제공됩니다. 이 경우 다음 탭이 새 탭에 표시되고 각 변환 문자열을 편집, 변환 및 검토할 수 있습니다.

![screenshot from i18n Ally showing advanced string editing UI](https://miro.medium.com/max/1316/1*xJFFLXHXPgshf4BcBKe9iQ.png)

# 더 드릴까요?

보다 뛰어난 VSCode 확장을 위해 BetterProgramming에 대한 이전 기사를 확인해 보십시오.