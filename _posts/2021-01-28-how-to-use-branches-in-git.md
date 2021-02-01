---
layout: post
title: "Git에서 브랜치를 사용하는 방법 – 궁극의 치트 시트
 "
author: 'Code Tower'
thumbnail: https://www.freecodecamp.org/news/content/images/size/w600/2021/01/header-image@2080x1090-1.png
tags: GIT
---


브랜치는 Git의 핵심 개념 중 하나입니다.
 그리고 그들과 함께 할 수있는 일은 끝이 없습니다.
 생성 및 삭제, 이름 변경 및 게시, 전환 및 비교 등을 할 수 있습니다.
 

이 게시물의 의도는 Git에서 브랜치로 할 수있는 일에 대한 포괄적 인 개요를 만드는 것입니다.
 책 길이의 기사를 만들고 싶지 않았기 때문에 모든 작업에 대해 자세히 설명하지는 않겠습니다.
 하지만 더 자세히 알고 싶다면 링크를 제공하겠습니다.
 

다음은 우리가 다룰 내용에 대한 개요입니다.
 

- 분기를 만드는 방법
 
- 분기 이름을 바꾸는 방법
 
- 분기를 전환하는 방법
 
- 브랜치를 게시하는 방법
 
- 지점을 추적하는 방법
 
- 분기를 삭제하는 방법
 
- 분기를 병합하는 방법
 
- 분기를 리베이스하는 방법
 
- 지점을 비교하는 방법
 

## Git에서 분기를 만드는 방법
 

브랜치로 작업하기 전에 저장소에 일부가 있어야합니다.
 이제 분기를 만드는 방법에 대해 이야기 해 보겠습니다.
 

```git
$ git branch <new-branch-name>
```

`git branch` 명령에 이름 만 제공 할 때 Git은 현재 체크 아웃 된 개정판을 기반으로 새 분기를 시작한다고 가정합니다.
 새 분기를 특정 개정에서 시작하려면 개정의 SHA-1 해시를 추가하기 만하면됩니다.
 

```git
$ git branch <new-branch-name> 89a2faad
```

로컬 저장소에서만 새 브랜치를 만들 수 있다는 것은 말할 필요도 없습니다.
 원격 저장소에서 분기를 "만들기"는 기존 로컬 분기를 게시하여 발생합니다. 이에 대해서는 나중에 설명하겠습니다.
 

## Git에서 분기 이름을 바꾸는 방법
 

지점의 이름을 잘못 입력하거나 사실 후에 단순히 마음을 바꾸는 것은 너무 쉽습니다.
 그렇기 때문에 Git을 사용하면 로컬 브랜치의 이름을 쉽게 바꿀 수 있습니다.
 현재 HEAD 브랜치의 이름을 바꾸려면 다음 명령을 사용할 수 있습니다.
 

```git
$ git branch -m <new-name>
```

현재 체크 아웃되지 않은 다른 로컬 브랜치의 이름을 바꾸려면 이전 이름과 새 이름을 제공해야합니다.
 

```git
$ git branch -m <old-name> <new-name>
```

이 명령은 다시 로컬 브랜치에서 작업하는 데 사용됩니다.
 원격 브랜치의 이름을 바꾸고 싶다면 좀 더 복잡합니다. Git에서는 원격 브랜치의 이름을 바꿀 수 없기 때문입니다.
 

실제로 원격 브랜치의 이름을 바꾸려면 이전 브랜치를 삭제 한 다음 로컬 리포지토리에서 새 브랜치를 푸시하면됩니다.
 

```git
# First, delete the current / old branch:
$ git push origin --delete <old-name>

# Then, simply push the new local branch with the correct name:
$ git push -u origin <new-name>
```

Tower와 같은 Git 데스크톱 GUI를 사용하는 경우 이러한 세부 정보에 신경 쓰지 않아도됩니다. 컨텍스트 메뉴에서 로컬 및 원격 브랜치의 이름을 간단하게 바꿀 수 있습니다 (아무것도 삭제하고 다시 푸시 할 필요가 없음).
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/tower_rename-branch.gif)

## Git에서 분기를 전환하는 방법
 

현재 분기 (HEAD 분기라고도 함)는 현재 작업중인 컨텍스트를 정의합니다.
 즉, 현재 HEAD 분기는 새 커밋이 생성되는 곳입니다.
 

하지만 현재 활성 브랜치를 전환하는 것은 개발자가 브랜치 작업을 할 때 가장 많이 사용하는 작업 중 하나라는 것이 합리적입니다.
 

분기 전환을 "체크 아웃"분기라고도하므로이 작업을 수행하는 데 사용되는 명령을 배우는 것에 놀라지 않을 것입니다.
 

```git
$ git checkout <other-branch>
```

그러나`git checkout` 명령에는 다양한 임무가 있기 때문에 Git 커뮤니티 (최근에)는 현재 HEAD 브랜치를 변경하는 데 사용할 수있는 새 명령을 도입했습니다.
 

```git
$ git switch <other-branch>
```

나는 `checkout`명령에서 벗어나-다른 많은 작업을 수행하는데 사용되기 때문에-대신에 그것이하는 일에 대해 절대적으로 모호하지 않은 새로운`switch `명령으로 이동하는 것이 합리적이라고 생각합니다.
 

## Git에 브랜치를 게시하는 방법
 

위의 "브랜치 만들기"섹션에서 이미 언급했듯이 원격 저장소에 새 브랜치를 만드는 것은 불가능합니다.
 

그러나 우리가 할 수있는 것은 원격 저장소에 기존 로컬 브랜치를 게시하는 것입니다.
 로컬에있는 것을 원격에 "업로드"하여 팀과 공유 할 수 있습니다.
 

```git
$ git push -u origin <local-branch>
```

이 명령은 전반적으로 당신에게 큰 놀라움은 아닐 것입니다.
 그러나 하나의 매개 변수 인`-u` 플래그는 설명 할 가치가 있습니다. 다음 섹션에서 이에 대해 설명하겠습니다.
 

그러나 여기서 짧은 버전을 제공하기 위해 Git에게 "추적 연결"을 설정하도록 지시하여 앞으로 훨씬 쉽게 밀고 당기는 작업을 수행합니다.
 

## Git에서 분기를 추적하는 방법
 

기본적으로 로컬 및 원격 분기는 서로 관련이 없습니다.
 Git에서 독립적 인 개체로 저장 및 관리됩니다.
 

그러나 실제 생활에서 물론 로컬 및 원격 지점은 종종 서로 관계를 맺습니다.
 예를 들어, 원격 분기는 종종 로컬 분기의 "상대"와 같습니다.
 

이러한 관계는 Git에서 모델링 할 수 있습니다. 한 브랜치 (일반적으로 로컬 브랜치)는 다른 브랜치 (일반적으로 원격)를 "추적"할 수 있습니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/tracking-ahead-behind.gif)

이러한 추적 관계가 설정되면 몇 가지 작업이 훨씬 쉬워집니다. 특히 밀거나 당길 때 추가 매개 변수없이 바닐라 명령을 사용할 수 있습니다 (예 : 간단한`git push`).
 

추적 연결은 Git이 빈칸을 채우는 데 도움이됩니다.
 

이러한 추적 연결을 설정하는 한 가지 방법에 대해 이미 읽었습니다. 로컬 브랜치를 처음 게시 할 때`-u` 옵션과 함께`git push`를 사용하면 정확히 수행됩니다.
 그런 다음 원격 또는 대상 브랜치를 언급하지 않고 간단히`git push`를 사용할 수 있습니다.
 

이것은 또한 다른 방법으로 작동합니다 : 원격 분기를 기반으로해야하는 로컬 분기를 만들 때.
 즉, 원격 분기를 추적하려는 경우 :
 

```git
$ git branch --track <new-branch> origin/<base-branch>
```

또는`git checkout` 명령을 사용하여이를 수행 할 수도 있습니다.
 원격 분기 뒤에 로컬 분기의 이름을 지정하려면 원격 분기의 이름 만 지정하면됩니다.
 

```git
$ git checkout --track origin/<base-branch>
```

이 주제에 대해 자세히 알아 보려면 "Git에서 관계 추적"에 대한이 게시물을 확인하십시오.
 

## Git에서 분기를 삭제하는 방법
 

모든 지점이 영원히 살 수있는 것은 아닙니다.
 실제로 모든 저장소의 대부분의 분기는 수명이 짧습니다.
 따라서 약간의 청소를하고 싶다면 로컬 브랜치를 삭제하는 방법은 다음과 같습니다.
 

```git
$ git branch -d <branch-name>
```

병합되지 않은 변경 사항이 포함 된 분기를 삭제하려는 경우`-f` 옵션이 필요할 수도 있습니다.
 이 옵션은 데이터 손실을 매우 쉽게하므로주의해서 사용하십시오!
 

원격 브랜치를 삭제하기 위해`git branch` 명령어를 사용할 수 없습니다.
 대신`git push`가`--delete` 플래그를 사용하여 트릭을 수행합니다.
 

```git
$ git push origin --delete <branch-name>
```

브랜치를 삭제할 때는 해당 브랜치도 삭제해야하는지 확인해야합니다.
 

예를 들어 원격 기능 분기를 방금 삭제 한 경우 로컬 추적 분기도 삭제하는 것이 좋습니다.
 이렇게하면 쓸모없는 브랜치가 많고 지저분한 Git 저장소가 남지 않도록 할 수 있습니다.
 

## Git에서 분기를 병합하는 방법
 

병합은 아마도 변경 사항을 통합하는 가장 일반적인 방법 일 것입니다.
 다른 브랜치의 모든 새 커밋을 현재 HEAD 브랜치로 가져올 수 있습니다.
 

Git의 가장 큰 장점 중 하나는 브랜치 병합이 매우 간단하고 스트레스가 없다는 것입니다.
 두 단계 만 필요합니다.
 

```git
# (1) Check out the branch that should receive the changes
$ git switch main
 
# (2) Execute the "merge" command with the name of the branch that contains the desired changes
$ git merge feature/contact-form
```

종종 병합의 결과는 소위 "병합 커밋"이라는 별도의 새 커밋이됩니다.
 이것은 Git이 들어오는 변경 사항을 결합하는 곳입니다.
 두 가지를 연결하는 매듭처럼 생각할 수 있습니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/merge-commit.png)

물론`git merge`에 대해 할 말이 더 많습니다.
 자세히 알아 보는 데 도움이되는 몇 가지 무료 리소스는 다음과 같습니다.
 

- Git에서 병합을 취소하는 방법
 
- 병합 충돌을 수정하고 해결하는 방법
 
- "git merge"개요
 

## Git에서 분기를 리베이스하는 방법
 

다른 브랜치의 커밋을 통합하는 또 다른 방법은 `rebase`를 사용하는 것입니다.
 그리고 저는 그것을 "대안"방식이라고 부르는 데 매우주의를 기울이고 있습니다. 더 좋거나 나쁘지는 않지만 단순히 다릅니다.
 

리베이스를 사용하는 경우 주로 개인 취향과 팀의 규칙에 따라 결정됩니다.
 일부 팀은 리베이스를 좋아하고 일부 팀은 병합을 선호합니다.
 

병합과 리베이스의 차이점을 설명하려면 다음 그림을 살펴보십시오.
 `git merge`를 사용하면 branch-B를 branch-A에 통합 한 결과는 다음과 같습니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/end-situation-merge-commit.gif)

반면`git rebase`를 사용하면 최종 결과가 상당히 다르게 보입니다. 특히 별도의 병합 커밋이 생성되지 않기 때문입니다.
 rebase를 사용하면 개발 기록이 직선으로 발생한 것처럼 보입니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/end-situation-rebase.gif)

실제 프로세스를 시작하는 것은 매우 간단합니다.
 

```git
# (1) Check out the branch that should receive the changes
$ git switch feature/contact-form
 
# (2) Execute the "rebase" command with the name of the branch that contains the desired changes
$ git rebase main

```

rebase에 대한 더 깊은 이해를 위해 "Using git merge instead of git merge"게시물을 추천합니다.
 

## Git에서 브랜치를 비교하는 방법
 

특정 상황에서는 두 가지를 비교하는 것이 매우 유용 할 수 있습니다.
 예를 들어 브랜치를 통합하거나 삭제하기 전에 다른 브랜치와 어떻게 다른지 확인하는 것이 흥미 롭습니다.
 새로운 커밋이 포함되어 있습니까?
 그렇다면 : 가치가 있습니까?
 

분기 B에는 있지만 분기 A에는없는 커밋을 확인하려면 `git log`명령을 이중 점 구문과 함께 사용할 수 있습니다.
 

```git
$ git log branch-A..branch-B
```

물론 이것을 사용하여`git log main..origin / main`과 같은 내용을 작성하여 로컬 및 원격 상태를 비교할 수도 있습니다.
 

커밋 대신 이러한 차이점을 구성하는 실제 변경 사항을보고 싶다면`git diff` 명령을 사용할 수 있습니다.
 

```git
$ git diff branch-A..branch-B
```

## Git으로 생산성을 높이는 방법
 

Git에서 브랜치 작업에는 많은 측면이 있습니다!
 그러나 이것은 일반적으로 Git의 경우에 해당됩니다. 많은 개발자가 알지 못하거나 생산적으로 사용할 수없는 강력한 기능이 많이 있습니다.
 

Interactive Rebase에서 Submodules, Reflog에서 File History로 : 생산성을 높이고 실수를 줄임으로써 이러한 고급 기능을 배우는 것이 좋습니다.
 

특히 유용한 주제 중 하나는 Git으로 실수를 실행 취소하는 방법을 배우는 것입니다.
 피할 수없는 실수로부터 목을 구할 수있는 방법에 대해 자세히 알아 보려면 Git에서 실수를 취소하는 방법에 대한이 비디오를 확인하십시오.
 

더 나은 프로그래머가 되세요!
 

## 저자 정보
 

Tobias Günther는 전 세계 10 만 명 이상의 개발자가 Git을 사용하여 생산성을 높일 수 있도록 지원하는 인기있는 Git 데스크톱 클라이언트 인 Tower의 공동 창립자입니다.
 