---
layout: post
title: "사용자 지정 JavaScript와 함께 부트스트랩 구성 요소 사용"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/image1-3.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/image1-3.png?fit=730%2C487&ssl=1)

부트스트랩은 버튼, 패널, 드롭다운과 같은 다양한 구성 요소를 제공하는 CSS 프레임워크이다. 웹 응용 프로그램의 웹 사이트 또는 그래픽 인터페이스를 신속하게 설계하는 데 사용할 수 있습니다.

부트스트랩 프런트 엔드를 생성하려면 HTML과 CSS에 대한 지식만 있으면 됩니다. 그러나 일부 기능은 JavaScript의 도움을 통해서만 구현할 수 있다. 이를 위해 부트스트랩 프레임워크는 간단한 JavaScript 인터페이스를 제공합니다.

## JavaScript 인터페이스에서 부트스트랩 작업

이 문서에서는 JavaScript 인터페이스를 통해 부트스트랩 구성 요소를 수정하고 제어하는 방법을 살펴봅니다. 사용할 예제는 사용자가 대화 상자(모달)를 열 수 있는 간단한 버튼입니다.

부트스트랩 설명서를 살펴보면 Carousel, Collap 또는 Drop과 같은 다른 대화형 부트스트랩 구성 요소의 인터페이스가 매우 유사하다는 것을 알 수 있습니다. 따라서 이 문서에서 배운 내용을 다른 구성 요소에 쉽게 적용할 수 있습니다.

### 샘플 페이지: 기본 구조

아래 샘플 페이지의 기본 구조에 대한 HTML 코드를 볼 수 있습니다. 페이지 내용을 배치할 유체 컨테이너와 결합된 부트스트랩 4.5의 시작 템플릿을 기반으로 합니다.

```xml
<!DOCTYPE html>

<html>

<head>

<meta charset="UTF-8"/>

<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

<!-- Bootstrap CSS -->

<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/css/bootstrap.min.css" integrity="sha384-TX8t27EcRE3e/ihU7zmQxVncDAy5uIKz4rEkgIXeMed4M0jlfIDPvg6uqKI2xXr2" crossorigin="anonymous">

<title>Bootstrap Example</title>

</head>

<body>

<div class="container-fluid">

<h1>My Bootstrap Playground</h1>

</div>

<script src="https://code.jquery.com/jquery-3.5.1.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-ho+j7jyWK8fNQe+A12Hb8AhRq26LrZ/JpcUGGOn+Y7RsweNrtN/tE3MoK7ZeZDyx" crossorigin="anonymous"></script>

</body>

</html>
```

이 예에서는 두 가지 사항을 고려해야 합니다.

- 향후 버전인 부트스트랩 5에서는 jQuery 라이브러리가 더 이상 필요하지 않습니다.
- 필요한 외부 CSS 및 JS 파일은 CDN(Content Delivery Network)을 통해 여기에 통합됩니다.

CDN을 사용하면 웹 사이트가 외부 서버에 연결할 수 있으며, 개인 정보 보호 정책에 이를 언급해야 할 수도 있습니다. 또는 프레임워크 파일을 수동으로 다운로드하고 자체 서버에서 호스팅할 수도 있습니다.

다음 섹션에서는 추가 JavaScript 없이 버튼/모달 예제를 검토합니다. 모달은 사용자가 버튼을 클릭하여 열 수 있습니다. 닫기를 클릭하거나 "x"를 클릭하여 다시 닫을 수 있습니다.

사용자가 "저장"을 클릭할 때와 같은 추가 동작을 구현하려면 사용자 지정 JavaScript가 필요합니다. 이에 대한 가능성은 다음 절에서 검토된다.

## 사용자 지정 JavaScript가 없는 대화형 부트스트랩 구성 요소

먼저 예제 페이지에 몇 가지 구성 요소를 추가하는 것으로 시작하겠습니다. 먼저 버튼(`버튼`)을 추가합니다.

```xml
<div class="container-fluid">

<h1>My Bootstrap Playground</h1>

<button type="button" class="btn btn-success">Task 1</button>

</div>
```

단추를 클릭하면 "저장 단추를 눌러 작업을 완료하십시오."라는 텍스트가 표시되는 모달이 열립니다. Live demo(실시간 데모) 섹션에서는 이 동작을 구현하는 방법을 설명합니다.

버튼은 `data-target=`data` 및 `data-target=`#task1_Modal` 속성으로 보완해야 합니다.

```undefined
<button type="button" class="btn btn-success" data-toggle="modal" data-target="#task1_Modal">Task 1</button>
```

모달 구성 요소의 HTML 코드를 삽입해야 합니다. 할당된 ID(여기서 `task1_Modal`)는 버튼의 `data-target` 속성에서 올바르게 참조되어야 합니다(앞의 # 기호는 `#task1_Modal`로 표시됨).

```js
< div class="modal fade" id="task1_Modal" tabindex="-1" aria-labelledby="task1_ModalLabel" aria-hidden="true">

<div class="modal-dialog">

<div class="modal-content">

<div class="modal-header">

<h5 class="modal-title" id="task1_ModalLabel">Task 1</h5>

<button type="button" class="close" data-dismiss="modal" aria-label="Close">

<span aria-hidden="true">×</span>

</button>

</div>

<div class="modal-body">
</div>

<div class="modal-footer">

<button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>

</div>

</div>

</div>

</div>
```

브라우저에서 페이지를 보면 단추를 클릭하면 대화 상자가 표시되고 "닫기" 단추를 클릭하면 다시 닫힐 수 있습니다.

여기서는 첫 번째 버전의 예제를 사용해 볼 수 있습니다.

부트스트랩 프레임워크의 사용이 없다면, 이러한 상호작용은 추가 자바스크립트 코드에서만 가능할 것이다. 우리는 클릭 이벤트에 반응하고, 모달을 보여주고, 숨기고, 버튼의 스타일을 조정해야 할 것이다. 부트스트랩 프레임워크가 이 모든 것을 해 줍니다.

## 사용자 지정 JavaScript를 사용한 대화형 부트스트랩 구성 요소

다음 코드 예에서는 JavaScript 라이브러리 jQuery를 웹 사이트에 포함해야 합니다. 코드를 단순 자바스크립트로 변경하고 싶다면 토바이어스 알린의 훌륭한 치트 시트를 추천합니다.

### 버튼 방식

개별 구성 요소의 부트스트랩 설명서에는 종종 메서드 섹션이 있습니다. 부트스트랩은 JavaScript를 통해 버튼을 제어할 수 있는 유용한 `토글` 방법을 제공합니다. 이 방법을 사용하려면 먼저 버튼에 `task1_button`과 같은 ID를 지정해야 합니다.

```undefined
<button id="task_button"… </button>
```

Toggle(토글) 방식을 호출하면 버튼의 모양을 클릭되지 않음에서 클릭됨으로 전환하고, 자바스크립트를 통해 버튼의 모양을 전환할 수 있다.

테스트하려면 닫기 본문 태그 바로 앞에 다음 코드 섹션을 삽입하십시오.

```xml
…

<script>

$("#task1_button").button("toggle");

</script>

</body>
```

브라우저에 의해 페이지가 로드된 후 즉시 코드가 실행됩니다.

선택기 `#task1_button`을 사용하여 `toggle` 방법을 ID `task1_button`이 있는 버튼에만 적용해야 함을 나타낸다(페이지에 더 많은 버튼이 있어야 함).

페이지를 열 때, 단추는 이미 클릭된 단추처럼(암녹색) 표시되어야 합니다.

이제 `토글`의 두 번째 호출을 추가합니다.

```shell
$("#task1_button").button("toggle");

$("#task1_button").button("toggle");
```

페이지를 열 때 이제 버튼은 다시 초기 상태(연한 녹색)가 되어야 합니다.

### 모달 방식

모달 구성요소의 방법 섹션도 유사한 구조를 가지고 있다. 여기에는 또한 `토글` 방법이 있는데, 모달은 닫힌 상태에서 열린 상태로 프로그래밍 방식으로 이동할 수 있다(그리고 다시).

```xml
…

<script>

$("#task1_Modal").modal("toggle");

</script>

</body>
```

이 코드 조각으로, 모달은 페이지가 로드될 때 사용자가 먼저 버튼을 클릭할 필요 없이 자동으로 열립니다. 두 번째 호출을 수행하면 대화 상자가 닫힙니다.

대화 상자를 열고 닫으려면 별도의 표시 및 숨기기 방법을 사용할 수도 있습니다. 저장 버튼을 클릭하면 Hide 방법을 사용하여 Modal을 닫을 수 있습니다. 이 작업에는 두 가지 단계가 필요합니다.

먼저 `onclick` 특성을 사용하여 버튼을 클릭할 때 실행할 JavaScript 기능을 정의합니다(예: `task1_save()`:

```undefined
<button type="button" class="btn btn-primary" onclick="task1_save()">Save changes</button>
```

그런 다음 본문 태그를 닫기 전에 다음 스크립트 코드를 삽입하십시오.

```xml
<script>

function task1_save(){

$("#task1_Modal").modal("hide");

}

</script>
</body>
```

Carousel, Collapse, Dropdown과 같은 다른 대화형 구성 요소도 비슷한 방식으로 구성 요소 상태를 제어한다.

### 이벤트

일부 부트스트랩 구성 요소의 경우 설명서에 추가 "이벤트" 섹션이 있습니다. 이는 사용자가 각 구성 요소와의 상호 작용 중에 트리거하는 미리 정의된 이벤트에 프로그래밍 방식으로 반응하는 것입니다.

예를 들어 모달의 경우 이벤트 `쇼` 또는 `숨기기`가 트리거될 때(예: 구성 요소의 해당 `쇼` 또는 `숨기기` 방법을 호출하여) 어떤 일이 발생해야 하는지 정의할 수 있다.

이 예에서는 다음을 정의해야 한다.

- 쇼 이벤트가 트리거되는 즉시 버튼의 라벨은 "Task 1"에서 "Task 1 진행 중..."으로 변경됩니다.
- 숨김 이벤트가 트리거되는 즉시 레이블이 다시 "Task 1"로 변경됩니다.

페이지 하단의 스크립트 영역에 다음 코드를 삽입합니다.

```js
$('#task1_Modal').on('show.bs.modal', function (e) {

$("#task1_button").text("Task 1 in process...");

});

$('#task1_Modal').on('hide.bs.modal', function (e) {

$("#task1_button").text("Task 1");

});
```

이제 대화상자를 닫을 때 사용자가 "닫기" 또는 "저장"을 클릭하더라도 라벨이 재설정된다는 것을 알 수 있습니다. 숨김 이벤트는 두 경우 모두 트리거됩니다.

여기서 두 번째 버전의 예제를 사용해 볼 수 있습니다.

## 부트스트랩 구성 요소를 사용하여 진행률 표시

앞의 예에서, 우리는 HTML 요소의 텍스트 콘텐츠를 사용자 정의하기 위해 jQuery 함수 `text`를 어떻게 사용할 수 있는지 보았다. JavaScript를 사용하면 CSS 클래스 또는 개별 CSS 속성을 포함하여 HTML 요소의 모든 속성을 수정할 수 있습니다. 다음 예를 살펴보십시오.

#### 'task1_save' 기능을 확장합니다.

대화상자를 저장한 후 버튼에 대한 CSS 클래스 `btn-success`를 `btn-secondary`로 변경한다. 이는 작업이 이미 완료되었음을 나타냅니다.

```js
function task1_save(){

$("#task1_Modal").modal("hide");

$("#task1_button").removeClass("btn-success");

$("#task1_button").addClass("btn-secondary");

}
```

#### 진행 표시줄 추가

작은 진행 표시줄로 버튼을 상황에 맞게 조정하십시오.

```xml
<h1>My Bootstrap Playground</h1>
<button id="task1_button" type="button" class="btn btn-success" data-toggle="modal" data-target="#task1_Modal">Task 1</button>

<div id="task1_progress" class="progress">
<div id="task1_progressbar" class="progress-bar bg-success" style="width:0%" role="progressbar" aria-valuenow="0" aria-valuemin="0" aria-valuemax="100"></div>
</div>
```

진행률 표시줄은 0%에서 시작합니다. 이제 다음 동작을 구현합니다.

첫째, 대화 상자가 표시되는 동안 진행률을 이미 100%로 설정해야 합니다. 이를 위해 CSS 속성 `width`를 수정해야 한다. 작업이 아직 진행 중이므로 진행 표시줄에 애니메이션 스트라이프 패턴을 표시하려고 합니다. 우리는 CSS 클래스 `progress-bar-stripeed`와 `progress-bar-animated`를 추가하여 이를 달성한다.

```shell
$('#task1_Modal').on('show.bs.modal', function (e) {

$("#task1_button").text("Task 1 in progress...");

$("#task1_progressbar").css("width", "100%");

$("#task1_progressbar").addClass("progress-bar-striped");
$("#task1_progressbar").addClass("progress-bar-animated");

});
```

그런 다음 (사용자의 저장 여부와 상관없이) 대화 상자를 숨긴 후 애니메이션과 스트라이프 패턴이 제거되고 진행률이 0%로 설정됩니다.

```shell
$('#task1_Modal').on('hide.bs.modal', function (e) {

$("#task1_button").text("Task 1");

$("#task1_progressbar").css("width", "0%");

$("#task1_progressbar").removeClass("progress-bar-striped");
$("#task1_progressbar").removeClass("progress-bar-animated");
});
```

사용자가 대화 상자를 저장한 경우 진행률을 100%로 영구적으로 설정해야 합니다.

```shell
function task1_save(){

$("#task1_Modal").modal("hide");

$("#task1_button").removeClass("btn-success");
$("#task1_button").addClass("btn-secondary");

$("#task1_progressbar").css("width", "100%");

}
```

사용자가 "변경사항 저장"을 클릭할 때 대화상자를 열기 전, 도중 및 후에 버튼과 진행 표시줄이 표시됩니다.

#### 신호 작업 완료

작업이 이미 저장된 후 단추를 두 번 클릭하면 "이미 작업을 완료했습니다"라는 내용의 다른 대화 상자가 표시됩니다. 우리는 이것을 위한 두 번째 모델을 준비하고 있습니다.

이 모달의 코드는 우리의 첫 번째 모달과 동일하지만, 우리는 다른 ID를 선택해야 한다. 예를 들어 `task1_message`와 같은 것이다. 그런 다음 모달 본문에 있는 텍스트를 "이미 작업을 완료했습니다"로 설정합니다.

첫 번째 대화 상자를 저장할 때 호출하는 함수 `task1_save()it`에서 버튼의 속성 `data-target`을 `task1_Modal`에서 `task1_Message`로 변경하는 명령을 추가합니다.

```shell
function task1_save(){

$("#task1_Modal").modal("hide");

$("#task1_button").removeClass("btn-success");
$("#task1_button").addClass("btn-secondary");

$("#task1_progressbar").removeClass("progress-bar-striped");
$("#task1_progressbar").removeClass("progress-bar-animated");

$("#task1_button").attr("data-target", "#task1_Message");

}
```

여기서 전체 예제의 코드를 다운로드할 수 있습니다. 부트스트랩_example.html

또는 CodePen에서 최종 버전의 예제를 사용해 볼 수 있습니다.

## 결론

추가 자바스크립트가 없어도 버튼모달카루젤카루젤폴드드드롭다운 등 인터랙티브 부트스트랩 구성 요소를 그대로 사용할 수 있다. 그러나 부트스트랩의 작은 JavaScript 인터페이스를 활용하면 방법 및 이벤트와의 상호 작용을 높일 수 있다.