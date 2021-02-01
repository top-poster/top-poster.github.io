---
layout: post
title: "몇 가지 간단한 단계로 웹 개발 용 VS 코드를 설정하는 방법
 "
author: 'Code Tower'
thumbnail: https://www.freecodecamp.org/news/content/images/size/w600/2021/01/ep11-vscode-1.jpg
tags: undefined
---


Visual Studio Code는 가장 인기있는 소스 코드 편집기가되었습니다.
 가볍지 만 강력하며 제가 가장 좋아하는 제품입니다.
 

이 기사에서는 웹 개발자를위한 VS Code를 시작하고 설정하는 방법을 안내합니다.
이 기사를 보완하려면 다음 비디오를 시청할 수 있습니다.
 

## VS 코드 소개
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/Screenshot-2021-01-20-at-17.22.57.png)

컴퓨터에 VS Code가 아직 설치되어 있지 않은 경우 code.visualstudio.com으로 이동하여 다운로드하세요.
 드롭 다운을 열어 다운로드 할 버전을 선택할 수 있지만 일반적으로 큰 버튼이 작업을 수행합니다.
 

## VS Code 시작 탭
 

일단 설치하고 열면 가장 먼저 보게 될 것은 환영 탭입니다.
 여기에 5 개의 섹션이 있습니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/Screenshot-2021-01-20-at-17.26.12.png)

시작 : 새 파일을 만들거나 폴더를 열도록 선택할 수 있습니다.
 

최근 : 최근에 연 폴더를 찾을 수 있습니다.
 

도움말 : 유용한 정보를 찾을 수 있습니다.
 예를 들어 인쇄 가능한 키보드 치트 시트 또는 일련의 소개 동영상이 있습니다.
 

사용자 지정 : Vim 또는 Atom과 같은 다른 코드 편집기에서 설정 및 키보드 단축키를 설치할 수 있음을 확인할 수 있습니다.
 따라서 현재 이러한 편집기를 사용하는 데 익숙하다면 계속해서 확인해 볼 수 있습니다.
 

그러나 우리가 살펴보고 싶은 것은 색상 테마입니다.
 선택하면 선택할 테마 목록이 있음을 알 수 있습니다.
 위쪽 및 아래쪽 화살표 키를 사용하여 테마를 미리 볼 수도 있습니다.
 하지만 제가 가장 좋아하는 테마는 기본 테마이므로 그대로 유지하겠습니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/Screenshot-2021-01-20-at-17.59.13.png)

학습 : 여기에서 3 가지 선택 항목을 찾을 수 있습니다.
 목록의 첫 번째 선택은 모든 명령 찾기 및 실행입니다.
 이를 통해 사용 가능한 모든 명령을 찾아 실행할 수 있습니다.
 많이 사용할 것이기 때문에 단축키 `Command / Control + Shift + P`를 외우는 것이 좋습니다.
 

두 번째 선택은 인터페이스 개요입니다.
 선택하면 사용자 인터페이스에서 가장 일반적인 요소를 볼 수 있으며 요소를 토글하는 바로 가기도 볼 수 있습니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/Screenshot-2021-01-20-at-17.30.16.png)

마지막 선택은 Interactive Editor Playground입니다.
 여기에서 지침 및 예제와 함께 VS Code의 주요 기능을 찾을 수 있습니다.
 

예를 들어 Emmet을 선택하겠습니다.
 Emmet을 사용하면 바로 가기를 작성한 다음 코드 조각으로 확장 할 수 있습니다.
 

예를 들어, 내부에 3 개의 항목이있는 정렬되지 않은 목록 요소를 만들고 각 항목에 "fruit"라는 클래스 이름이 있다고 가정 해 보겠습니다.
 `ul> li.fruit * 3`을 입력하고`tab`을 누릅니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/emmet.gif)

또한 기존 예제 (`ul> li.item $ * 5`)에서 정렬되지 않은 목록 요소와 클래스 이름이`item` 인 항목 5 개를 내부에 생성하려고 시도한 것을 볼 수 있습니다.
 그러나 클래스 이름에는 번호 매기기도 함께 제공됩니다.
 

```html
<ul>
    <li class="item1"></li>
    <li class="item2"></li>
    <li class="item3"></li>
    <li class="item4"></li>
    <li class="item5"></li>
</ul>
```

이 섹션에서는 매우 유용한 Emmet 치트 시트에 대한 링크를 찾을 수도 있습니다.
 

좋습니다.이 모든 기능을 확인하시기 바랍니다.
 많은 것들이 있는데 지금 다 이해하지 못해도 괜찮습니다.
 언제든 다시 올 수 있습니다.
 

## VS 코드 파일 탐색기
 

다음으로 측면 탐색의 첫 번째 탭 또는 `Command / Control + Shift + E`를 선택하여 파일 탐색기로 이동합니다.
 

여기에서 기존 폴더를 열 수 있지만 새 폴더와 새 파일을 만들어 보겠습니다.
 새 창을 여는 대신 VS Code에서 터미널을 열어 보겠습니다.
 상태 표시 줄에서 오류 및 경고 버튼을 선택한 다음 `터미널`탭을 선택하거나 단축키 `Control +``를 사용할 수 있습니다.
 

지금은 내 홈 디렉토리에 있습니다.
 `mkdir vscode-tutorials`를 입력하여 새 폴더를 만들고`cd vscode-tutorials`를 사용하여 그 안에 들어가 보겠습니다.
 

이제이 폴더를 열고 자하므로`폴더 열기`버튼을 선택하고 폴더 디렉토리로 이동할 수 있습니다.하지만 많은 작업이 필요합니다.
 따라서 대신 터미널에서`code .`라고 말할 수 있습니다.
 이제 `bash : code : command not found`오류가 발생할 수 있습니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/Screenshot-2021-01-20-at-17.52.42.png)

이 문제를 해결하려면 Command Palette를 열고`Shell Command : Install code command in Path`를 검색하여 선택합니다.
 이제 터미널로 돌아가서`code .`를 입력하면 생성 된 폴더와 함께 새 VS Code 창이 열립니다.
 

자, 다음으로 새 파일을 만들고 싶습니다.
 폴더 섹션에서 새 파일 아이콘을 클릭하거나 마우스 오른쪽 버튼을 클릭하고 `새 파일`을 선택할 수 있습니다.
 `index.html` 파일의 이름을 지정하고 그 안에 느낌표 (!)를 입력하고 Tab 또는 Enter 키를 누릅니다.
 Emmet을 사용하면 HTML 템플릿이 생성됩니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/Screenshot-2021-01-20-at-17.55.20.png)

이제 Command Palette를 다시 열고 Format Document를 검색하여 선택하겠습니다.
 다른 섹션 사이에 공백을 추가하고 코드를 정리하는 것을 볼 수 있습니다.
 

이것은 VS Code의 매우 유용한 기능입니다.
 그러나 우리는 항상 문서 서식을 검색하지 않고 파일을 저장할 때마다 서식을 지정하기를 원합니다.
 

여기에서 들여 쓰기가 이제 4 개의 공백과 동일하다는 것을 알 수 있습니다.
 이제 2로 변경하겠습니다. 설정으로 이동하거나 단축키`Command / Control +,`를 사용할 수 있습니다.
 

일반적으로 사용되는 탭에서 탭 크기를 2로 변경할 수 있으며 텍스트 편집기 / 서식에서 저장시 형식을 선택할 수 있습니다.
 이제 저장할 때마다 파일 형식이 올바르게 지정됩니다.
 

## VS 코드 확장
 

마지막으로 사용 방법을 보여 드리고 싶은 것은 확장 기능입니다.
 확장 프로그램 탭은 측면 탐색 메뉴에서 선택하거나 `Command / Control + Shift + X`단축키를 사용하여 선택할 수 있습니다.
 

여기에서 가장 인기있는 항목 또는 권장 항목을 기준으로 확장 프로그램을 필터링 할 수 있습니다.
 

선택할 수있는 많은 확장이 있습니다.
 그러나 우리가 설치해야 할 첫 번째 확장은 Live Server입니다.
 이를 통해 로컬 서버가 정적 웹 페이지를 다시로드하도록 할 수 있습니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/Screenshot-2021-01-20-at-17.56.38.png)

예를 들어`index.html`로 이동하여 Command Palette를 열고 Live Server : Open with Live Server를 검색하면 브라우저의 새 탭이 열린 것을 볼 수 있습니다.
 

HTML에 새 요소 (예 :`<h1> VScode Introduction <h1 />`)를 생성하면 저장 후 페이지가 자동으로 다시로드되고 변경 사항을 볼 수 있습니다.
 `index.html`에서는 상태 표시 줄의 라이브 실행 버튼을 사용하여 라이브 서버를 열 수도 있습니다.
 

## 결론
 

다른 많은 유용한 확장 기능이 있지만 다른 기사와 비디오에서 다룰 것입니다.
 

지금은이 소개 및 설정 가이드를 통해 웹 개발 여정을 시작할 준비가되었다고 확신합니다.
 

이것으로 기사를 마칩니다.
 향후 업데이트를 위해 소셜 미디어에서 저를 팔로우 할 수 있습니다.
 그렇지 않으면 즐거운 코딩을 유지하고 향후 게시물에서 만나십시오.
__________ 🐣 내 정보 __________
 

- 저는 DevChallenges의 창립자입니다.
 
- 내 채널 구독
 
- 내 트위터 팔로우
 
- Discord 가입
 