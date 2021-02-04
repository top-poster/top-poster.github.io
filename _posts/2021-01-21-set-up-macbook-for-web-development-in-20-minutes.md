---
layout: post
title: "웹 개발을위한 M1 MacBook 설정
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/set_up_macbook_air_web_development.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/set_up_macbook_air_web_development.png?fit=730%2C487&ssl=1)

내 M1 MacBook Air가 며칠 전에 도착했는데 더 이상 행복 할 수 없었습니다.
 처음 얻은 이후로 한계를 뛰어 넘어 왔고, 이건 빠르다.
 여러 개의 설치가 병렬로 실행되고 있으며 온도는 화씨 104도까지 거의 최고치에 도달하지 못했습니다.
 전반적으로이 기계는 Intel 모델에 비해 완전히 새로운 수준입니다.
 

그러나 모든 정보가없는 엄청난 양의 리소스가 있기 때문에 이상적인 웹 개발 환경에 맞게 MacBook을 설정하는 데 너무 오래 걸렸습니다.
 그래서 저는 웹 개발 도구를 20 분 안에 시작하고 실행하는 데 도움이되는 자습서를 작성했습니다.
 

이 웹 개발 환경에는 다음이 포함됩니다.
 

- 로제타 2
 
- 홈브류
 
- 힘내
 
- 마디
 
- MongoDB
 
- Chrome [+ Canary]
 
- Firefox [+ Nightly]
 
- VS 코드
 
- npm
 
- nvm
 
- Zsh
 
- 오 마이 Zsh
 
- 전력선 글꼴
 

모두 20 분 안에!
 

## MacBook Pro를 업그레이드 한 이유
 

1200 회가 넘는 충전 주기로 인해, 6 살이 된 13 인치 MacBook Pro는 날이 갈수록 불안정 해졌고, Apple이 M1 라인을 출시 할 즈음에 새 컴퓨터를 구입해야한다는 어려운 선택에 직면했습니다.
 

새로운 칩에 대한 모든 리뷰는 마법과 비슷한 그림을 그렸고 모두 공통 분모를 공유했습니다. 칩은 엄청나게 빠릅니다.
 그러나 M1이 ARM 아키텍처를 기반으로한다는 점을 고려할 때 결정은 간단하지 않았습니다.
 

웹 개발자의 경우 이는 Docker Desktop이 2021 년 1 월 현재 지원되지 않음을 의미합니다. 그러나 좋은 소식은 이미 베타 단계에 있으며 Docker 팀은 향후 몇 달 내에 기본 지원을 출시 할 계획이라는 것입니다.
 제 경우에는 현재 프로젝트에서 Docker를 사용하고 있지 않으므로 이것은 실제로 저에게 좌절이 아닙니다.
 

그럼에도 불구하고 아직 많은 앱이 ARM 칩에 최적화되어 있지 않다는 사실을 철저히 고려했습니다.
 소음이 사라질 때까지 기다렸다가 개발자가 성공적으로 실행하는 것을 확인한 후 미래로 도약하고 M1에 올인 할 때라고 결정했습니다.
 

## 지도 시간
 

튜토리얼을 진행하는 데 필요한 모든 것이 Mac에 미리 설치되어 있으므로 목록에있는 도구를 설치하는 데 대부분의 시간을 터미널에서 보냅니다.
 

### 로제타 2
 

먼저 인텔에서 실행되도록 설계된 소프트웨어가 새로 개발 된 프로세서와 동일한 언어를 사용하도록해야합니다.
 Apple은 원활한 전환을 가능하게하는 솔루션을 출시했습니다. 즉, ARM에서 x86 명령어 (Intel 칩에서 사용하는 명령어 세트)를 사용하는 앱을 실행할 수있는 Rosetta 2라는 에뮬레이터입니다.
 

Rosetta는 기본적으로 설치되지 않습니다.
 이를 사용하려면 Utilities 폴더에 미리 설치된 터미널로 이동하여 다음 명령을 실행해야합니다.
 

```undefined
/usr/sbin/softwareupdate --install-rosetta --agree-to-license
```

`--agree-to-license` 플래그는 대화 형 설치를 건너 뛰고 Apple의 라이선스에 동의합니다.하지만 가입 대상을 정말로 알고 싶다면 플래그를 삭제하고 라이선스를 읽어보세요.
 

### 홈브류
 

우리는 Homebrew를 사용하여 대부분의 앱과 유틸리티를 설치할 것입니다.
 2020 년 12 월 현재 많은 수식이 여전히 ARM에 최적화되어 있지 않으므로 몇 가지 문제가 발생할 수 있습니다.
 내 경험상 아직 오류가 발생하지 않았으며 내가 설치 한 모든 도구가 즉시 작동합니다.
 

Homebrew를 설치하는 가장 좋은 방법은 응용 프로그램의 유틸리티 폴더로 이동하여 터미널에서 Rosetta를 통해 모든 명령을 실행하도록하는 것입니다.
 

다음 GIF와 같이 터미널의 정보 가져 오기 탭을 열고 Rosetta를 사용하여 열기 확인란을 선택하면됩니다.
 

![image](https://paper-attachments.dropbox.com/s_23BAA28EB3F99331599A5AF02BD9271C20CB3C98FDA27022B8F050832EDE04AE_1608690754666_terminal.gif)

이제 터미널을 열고 아래 명령을 실행 한 다음 메시지가 표시되면 컴퓨터의 비밀번호를 입력합니다.
 

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## 도구
 

Homebrew가 설치를 마치면 이제이를 사용하여 많은 도구를 설치할 수 있습니다.
 나는 나를 위해 무거운 작업을 수행하는 스크립트가 있습니다.
 스크립트는 zellwk의 저장소에서 파생되었으며 내 환경에 맞게 조정되었습니다.
 다음 저장소에서 내 버전의 스크립트를 찾을 수 있습니다.
 

후자의 저장소로 이동하여 다운로드하십시오.
 압축을 풀고`cd`를 설정 폴더에 넣으십시오.
 저장소에는 일부 구성 파일이 포함되어 있지만이 가이드에서는 brew-installs.sh 스크립트 만 사용합니다.
 

더 진행하기 전에 텍스트 편집기에서 brew-installs.sh를 열고 그것이 수행하고 설치하는 모든 것을 보는 것이 좋습니다.
 저와 같은 도구를 사용할 것으로 예상하지 않기 때문에 환경에 맞게 조정할 수 있습니다.
 

만족 스러우면`ls -al`을 실행하여 brew-installs.sh 파일이 실행 가능한지 확인하십시오.
 파일이 실행 가능하지 않은 경우 출력은`-rw-r-xr-x ... brew-installs.sh`와 같이 표시됩니다 (세 개의 점은 목적과 관련이없는 일부 정보를 나타냄).
 

실행 가능하게하려면`chmod + x brew-installs.sh`를 실행하면됩니다.
 이제 출력됩니다 :`-rwxr-xr-x ... brew-installs.sh`.
 

이제 현재 작업 디렉토리가 설정되어 있다고 가정하고`. / brew-installs.sh`를 터미널에 입력하여`brew-installs.sh` 스크립트를 실행합니다.
 여기에서 스크립트가 당신을 위해 마법을 수행하게하십시오.
 인터넷 속도에 따라 모든 것을 다운로드하고 설치하는 데 약 5 분이 소요됩니다.
 

### iTerm
 

이전 설정 스크립트에 포함 된 iTerm이 지금까지 설치되어 있어야하므로 튜토리얼을 완료 할 수 있습니다.
 터미널에서 처음했던 것처럼 Rosetta 레이어를 추가하는 것이 중요합니다.
 한 가지 옵션은 앱을 복제하고 Rosetta iTerm 및 일반 iTerm을 사용하는 것입니다.
 아래 GIF를 따라하면됩니다.
 

![image](https://www.dropbox.com/s/51k24kjb1ppi74q/optimized-iterm.gif?raw=1)

### Zsh
 

brew-installs.sh 스크립트에서 제외하지 않은 경우 Zsh가 이제 기본 셸이어야합니다.
 제외했다면`brew install zsh`를 실행하십시오.
 계속 진행하기 전에`/ bin / zsh`를 출력하는`echo $ SHELL`을 실행하여 Zsh가 기본 셸인지 확인하겠습니다.
 그렇지 않은 경우`chsh -s $ (which zsh)`를 실행하여 Zsh로 변경합니다.
 

### 오 마이 Zsh
 

Oh My Zsh와 함께 Zsh에게 스테로이드를 줘요.
 다음 명령을 실행하여 설치하십시오.
 

```bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Oh My Zsh에서 사용할 수있는 많은 플러그인과 테마가 있습니다.
 GitHub 저장소에서 전체 목록을 확인할 수 있습니다.
 구문 강조는 내가 없이는 살 수없는 플러그인 중 하나입니다.
 

올바른 명령을 입력하고 있는지 또는 경로에 있는지 확인하는 것이 훨씬 쉽습니다.
 명령이 인식되면 텍스트는 녹색으로 표시되고 그렇지 않으면 빨간색으로 표시됩니다.
 

![image](https://paper-attachments.dropbox.com/s_23BAA28EB3F99331599A5AF02BD9271C20CB3C98FDA27022B8F050832EDE04AE_1608690779110_terminal-prompt.gif)

혼란을 줄이려면 먼저 다음 경로로 이동하여 플러그인을 설치하는 것이 가장 좋습니다.`cd $ HOME / .oh-my-zsh / plugins` 다음 명령을 실행하면 소스가 폴더에 자동으로 추가됩니다.
 .zshrc 파일 :
 

```undefined
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
```

### nvm
 

Homebrew를 통해 nvm을 설치하려고했지만 비참하게 실패했습니다… 많은 사람들이 동일한 문제에 직면 했으므로 현재 가장 좋은 대안은 다음 명령을 실행하여`curl `을 통해 수행하는 것입니다.
 

```undefined
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
```

설치가 완료되면 홈 디렉토리의 .zshrc 파일에 다음 줄을 추가합니다 (bash를 사용하는 경우 .bash-profile 또는 .bashrc에 추가해야 함).
 

```bash
export NVM_DIR="$HOME/.nvm"
#This loads nvm
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
#This loads nvm bash_completion
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
```

콘솔 세션을 재설정하고`nvm --version`을 실행하여 nvm이 제대로 설치되었는지 확인합니다. 그러면`현재 버전 0.37.2`가 출력됩니다.
 

### Git 및 GitHub
 

Git은 brew-installs.s에 포함 된 설치 중 하나이며 지금까지 설치해야합니다.
 구성하려면 먼저 사용자 이름과 이메일을 설정합니다.
 

<USERNAME> 및 <EMAIL>을 자신의 것으로 대체하고 다음 명령 시퀀스를 실행하십시오.
 

```php
git config --global user.name < USERNAME > &&
git config --global user.email < EMAIL > &&
git config --global --list
```

GitHub를 인증하기 위해 권장되는 방법은 개인 액세스 토큰을 사용하는 것입니다.
 이것은이 튜토리얼의 범위를 벗어나므로 공식 GitHub 튜토리얼을 방문하십시오.
 

### VS 코드
 

한 컴퓨터에서 다른 컴퓨터로 원활하게 전환하기 위해 VS Code에는 설정 동기화라는 이름의 달콤한 확장 기능이있어 구성을 GitHub Gist에 업로드 할 수 있습니다.
 GitHub에 설치되면 확장 프로그램은 설정 파일, 키 바인딩, 스 니펫, 작업 공간 폴더, 확장 및 해당 구성과 같은 파일을 동기화 상태로 유지합니다.
 

확장 프로그램 페이지에는 설정 방법에 대한 자세한 설명이 있으며 원하는 설정으로 VS 코드를 사용하는 데 몇 분 밖에 걸리지 않습니다.
 

### 전력선 글꼴
 

Oh My Zsh의 대부분의 테마에는 Powerline Fonts가 필요합니다.
 공식 Powerline 저장소에서 다음 정보 (모든 크레딧은 작성자에게 제공됨)를 가져와 편의를 위해 명령을 시퀀스로 변환했습니다.
 아래 블록을 복사하여 터미널에 붙여 넣으면 Powerline이 다운로드되어 설치됩니다.
 

```bash
git clone https://github.com/powerline/fonts.git --depth=1 &&
cd fonts &&
./install.sh &&
cd ..
```

이제`rm -rf fonts`를 실행하여 생성 된 fonts 폴더를 삭제할 수 있습니다.
 보안을 위해이 명령을 순서에서 제외했습니다.
 

Agnoster 또는 Powerline을 사용하는 다른 테마를 사용 중이고 어떤 이유로 아이콘 대신 물음표가 표시되는 경우 iTerm 설정에서 비 ASCII 글꼴을 변경해야합니다.
 프로필의 텍스트 탭에서 찾을 수 있습니다.
 저는 개인적으로 Powerline에 Space Mono를 사용하지만 확인할 수있는 다른 많은 옵션이 있습니다.
 

## 결론
 

그리고 wahlah!
 웹 개발을 위해 설정되었습니다.
 이것은 밀도가 높을 수 있지만 좋은 소식은 프로젝트가 지금까지 컴파일되어야한다는 것입니다 (물론 npm이 필요한 node_modules를 설치 한 후).
 

질문이나 의견이 있으시면 댓글 섹션을 통해 연락 주시면 기꺼이 도와 드리겠습니다.
 