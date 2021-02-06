---
layout: post
title: "JavaScript의 사용자 정의 이벤트: 완전한 안내서"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/javascript-custom-events.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/javascript-custom-events.png?fit=730%2C487&ssl=1)

웹 응용 프로그램의 모든 곳에 이벤트가 있습니다. 브라우저가 HTML을 로드하고 구문 분석하면 바로 트리거되는 DOM ContentLoaded 이벤트부터 사용자가 사이트를 떠나기 직전에 트리거되는 언로드 이벤트까지 웹앱 사용 경험은 기본적으로 일련의 이벤트에 불과하다. 개발자의 경우, 이러한 이벤트는 응용프로그램에서 방금 일어난 일, 특정 시간에 사용자의 상태가 어떤 상태인지 등을 결정하는 데 도움이 됩니다.

사용 가능한 JavaScript 이벤트가 응용프로그램의 상태를 적절하거나 올바르게 설명하지 못하는 경우가 있습니다. 예를 들어, 사용자 로그인이 실패하고 상위 구성 요소나 요소가 실패에 대해 알도록 하려면 로그인 실패 이벤트나 유사한 이벤트를 발송할 수 없습니다.

다행히 응용프로그램에 대한 JavaScript 사용자 정의 이벤트를 만드는 방법이 있습니다. 이 튜토리얼에서 다룰 내용입니다.

다음 사항에 대해 자세히 살펴보겠습니다.

- JavaScript에서 사용자 정의 이벤트를 생성하는 방법
- 이벤트 생성기 사용
- 사용자 지정 이벤트 생성기 사용
- JavaScript에서 사용자 정의 이벤트 발송
- JavaScript 사용자 지정 이벤트는 어떻게 작동합니까?
- JavaScript 끌어서 놓기
- JavaScript에서 개체 삭제 사용 방법

이 튜토리얼을 따르려면 다음 사항에 대한 기본 지식을 갖춰야 합니다.

- HTML 및 CSS
- JavaScript 및 ES6
- DOM 및 DOM 조작

시작해 봅시다!

## JavaScript에서 사용자 정의 이벤트를 생성하는 방법

사용자 정의 이벤트는 두 가지 방법으로 생성할 수 있습니다.

- 이벤트 생성기 사용
- 사용자 지정 이벤트 생성기 사용

사용자 정의 이벤트는 `document.createEvent`를 사용하여 생성할 수도 있지만 함수에서 반환된 객체가 노출하는 대부분의 방법은 사용되지 않습니다.

## 이벤트 생성기 사용

이벤트 생성자를 사용하여 다음과 같이 사용자 지정 이벤트를 생성할 수 있습니다.

```undefined
const myEvent = new Event('myevent', {
  bubbles: true,
  cancelable: true,
  composed: false
})
```

위 조각에서 이벤트 이름을 이벤트 생성자에게 전달하여 my event라는 이벤트를 생성하였습니다. 이벤트 이름은 대소문자를 구분하지 않으므로 마이이벤트나 마이이벤트 등과 같다.

이벤트 생성자는 이벤트와 관련된 일부 중요한 속성을 지정하는 개체도 허용합니다.

### ➡➡➡➡➡➡➡➡➡➡ ➡

버블스 속성은 이벤트가 상위 요소로 상향 전파되는지 여부를 지정합니다. 이를 `참`으로 설정하면 하위 요소에서 이벤트가 발송될 경우 상위 요소가 이벤트를 청취하고 이를 기반으로 작업을 수행할 수 있습니다. 대부분의 기본 DOM 이벤트의 동작이지만 사용자 지정 이벤트의 경우 기본적으로 `false`로 설정됩니다.

특정 요소에서만 이벤트를 전송하려는 경우 `event.stopPropagation()을 통해 이벤트 전파를 중지할 수 있습니다. 이벤트를 수신하는 콜백에 있어야 합니다. 나중에 이것에 대해 더 많이.

### 취소할 수 있는

이름에서 알 수 있듯이 `취소 가능`은 이벤트를 취소할 수 있는지 여부를 지정합니다.

기본 DOM 이벤트는 기본적으로 취소할 수 있으므로 해당 이벤트에서 `event.preventDefault()`를 호출하면 이벤트의 기본 작업이 차단됩니다. 사용자 정의 이벤트가 `취소 가능`을 `false`로 설정한 경우 `event.preventDefault()를 호출하면 아무런 작업도 수행되지 않습니다.

### ➡➡➡➡➡➡➡➡➡➡ ➡

`composed` 속성은 이벤트가 섀도 DOM(웹 구성 요소를 사용할 때 생성됨)에서 실제 DOM으로 버블링할지 여부를 지정합니다. 버블을 거짓으로 설정하면 이벤트에서 버블을 위로 올리지 않도록 명시적으로 지시하는 것이기 때문에 이 부동산의 가치는 중요하지 않습니다. 그러나 웹 구성 요소에서 사용자 지정 이벤트를 전송하고 실제 DOM의 상위 요소에서 사용자 지정 이벤트를 청취하려면 구성된 속성을 `true`로 설정해야 합니다.

이 방법을 사용하면 수신자에게 데이터를 전송할 수 없다는 단점이 있습니다. 그러나, 대부분의 응용프로그램에서, 우리는 이벤트가 수신자에게 발송되는 위치로부터 데이터를 전송할 수 있기를 원할 것이다. 이를 위해 `Custom Event` 컨트롤러를 사용할 수 있습니다.

또한 기본 DOM 이벤트를 사용하여 데이터를 보낼 수 없습니다. 데이터는 이벤트 대상에서만 얻을 수 있습니다.

## 사용자 지정 이벤트 생성기 사용

사용자 지정 이벤트 생성자를 사용하여 사용자 지정 이벤트를 생성할 수 있습니다.

```cpp
const myEvent = new CustomEvent("myevent", {
  detail: {},
  bubbles: true,
  cancelable: true,
  composed: false,
});
```

위와 같이 CustomEvent 생성자를 통해 사용자 정의 이벤트를 생성하는 것은 이벤트 생성자를 사용하여 이벤트를 생성하는 것과 유사합니다. 유일한 차이는 생성자에 두 번째 매개 변수로 전달된 개체입니다.

이벤트 생성자로 이벤트를 만들 때 이벤트를 통해 데이터를 수신자에게 전달할 수 없다는 점 때문에 한계가 있었습니다. 여기서는 수신자에게 전달해야 하는 모든 데이터를 이벤트를 초기화할 때 생성되는 `세부 정보` 속성으로 전달할 수 있습니다.

## JavaScript에서 사용자 정의 이벤트 발송

이벤트를 만든 후에는 이벤트를 발송할 수 있어야 합니다. 이벤트는 `EventTarget`을 확장하는 객체에 발송할 수 있으며, 여기에는 모든 HTML 요소, 문서, 창 등이 포함됩니다.

다음과 같은 사용자 지정 이벤트를 보낼 수 있습니다.

```js
const myEvent = new CustomEvent("myevent", {
  detail: {},
  bubbles: true,
  cancelable: true,
  composed: false,
});
document.querySelector("#someElement").dispatchEvent(myEvent);
```

사용자 정의 이벤트를 청취하려면 기본 DOM 이벤트와 마찬가지로 청취할 요소에 이벤트 수신기를 추가하십시오.

```coffeescript
document.querySelector("#someElement").addEventListener("myevent", (event) => {
  console.log("I'm listening on a custom event");
});
```

## JavaScript 사용자 지정 이벤트는 어떻게 작동합니까?

JavaScript 응용 프로그램에서 사용자 지정 이벤트를 사용하는 방법을 보여주기 위해 사용자가 프로필을 추가하고 자동으로 프로필 카드를 얻을 수 있는 간단한 앱을 구축합니다.

작업을 마치면 다음과 같은 페이지가 나타납니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/profile-card.png?resize=730%2C343&ssl=1)

### UI 구성

폴더를 만들고 원하는 이름을 지정한 후 폴더에 `index.html` 파일을 만듭니다.

다음을 `index.html`에 추가하십시오.

```xml
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Custom Events Application</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <h1>Profile Card</h1>
    <main>
      <form class="form">
        <h2>Enter Profile Details</h2>
        <section>
          Drag an Image into this section or
          <label>
            select an image
            <input type="file" id="file-input" accept="image/*" />
          </label>
        </section>
        <div class="form-group">
          <label for="name"> Enter Name </label>
          <input type="text" name="name" id="name" autofocus />
        </div>
        <div class="form-group">
          <label for="occupation"> Enter Occupation </label>
          <input type="text" name="occupation" id="occupation" />
        </div>
      </form>

      <section class="profile-card">
        <div class="img-container">
          <img src="http://via.placeholder.com/200" alt="" />
        </div>
        <span class="name">No Name Entered</span>
        <span class="occupation">No Occupation Entered</span>
      </section>
    </main>
    <script src="index.js"></script>
  </body>
</html>
```

여기, 페이지에 대한 마크업을 추가하고 있습니다.

페이지에는 두 개의 섹션이 있습니다. 첫 번째 섹션은 사용자가 다음을 수행할 수 있는 양식입니다.

- 끌어서 놓기 또는 수동으로 이미지 파일을 선택하여 이미지 업로드
- 이름 입력
- 직종 입력

양식에서 가져온 데이터는 프로파일 카드인 두 번째 섹션에 표시됩니다. 두 번째 섹션에는 자리 표시자 텍스트와 이미지가 포함되어 있습니다. 양식에서 받은 데이터가 내용 자리 표시자 데이터를 덮어씁니다.

`style.css` 파일을 생성하고 다음 파일로 채웁니다.

```css
* {
  box-sizing: border-box;
}
h1 {
  text-align: center;
}
main {
  display: flex;
  margin-top: 50px;
  justify-content: space-evenly;
}
.form {
  flex-basis: 500px;
  border: solid 1px #cccccc;
  padding: 10px 50px;
  box-shadow: 0 0 3px #cccccc;
  border-radius: 5px;
}
.form section {
  border: dashed 2px #aaaaaa;
  border-radius: 5px;
  box-shadow: 0 0 3px #aaaaaa;
  transition: all 0.2s;
  margin-bottom: 30px;
  padding: 50px;
  font-size: 1.1rem;
}
.form section:hover {
  box-shadow: 0 0 8px #aaaaaa;
  border-color: #888888;
}
.form section label {
  text-decoration: underline #000000;
  cursor: pointer;
}
.form-group {
  margin-bottom: 25px;
}
.form-group label {
  display: block;
  margin-bottom: 10px;
}
.form-group input {
  width: 100%;
  padding: 10px;
  border-radius: 5px;
  border: solid 1px #cccccc;
  box-shadow: 0 0 2px #cccccc;
}
#file-input {
  display: none;
}
.profile-card {
  flex-basis: 300px;
  border: solid 2px #cccccc;
  border-radius: 5px;
  box-shadow: 0 0 5px #cccccc;
  padding: 40px 35px;
  align-self: center;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.img-container {
  margin-bottom: 50px;
}
.img-container img {
  border-radius: 50%;
  width: 200px;
  height: 200px;
}
.profile-card .name {
  margin-bottom: 10px;
  font-size: 1.5rem;
}
.profile-card .occupation {
  font-size: 1.2rem;
}
```

마지막으로 응용프로그램에 기능을 추가할 수 있도록 `index.js` 파일을 작성합니다.

## JavaScript 끌어서 놓기

애플리케이션에 추가할 첫 번째 기능은 이미지를 업로드하는 기능입니다. 이를 위해 드래그 앤 드롭과 수동 업로드를 지원합니다.

JavaScript 파일에 다음을 추가하십시오.

```undefined
const section = document.querySelector(".form section");

section.addEventListener("dragover", handleDragOver);
section.addEventListener("dragenter", handleDragEnter);
section.addEventListener("drop", handleDrop);

/**
 * @param {DragEvent} event
 */
function handleDragOver(event) {
  // Only allow files to be dropped here.
  if (!event.dataTransfer.types.includes("Files")) {
    return;
  }
  event.preventDefault();
  // Specify Drop Effect.
  event.dataTransfer.dropEffect = "copy";
}

/**
 * @param {DragEvent} event
 */
function handleDragEnter(event) {
  // Only allow files to be dropped here.
  if (!event.dataTransfer.types.includes("Files")) {
    return;
  }
  event.preventDefault();
}

/**
 * @param {DragEvent} event
 */
function handleDrop(event) {
  event.preventDefault();
  // Get the first item here since we only want one image
  const file = event.dataTransfer.files[0];
  // Check that file is an image.
  if (!file.type.startsWith("image/")) {
    alert("Only image files are allowed.");
    return;
  }
  handleFileUpload(file);
}
```

여기서는 DOM에서 섹션을 선택합니다. 이를 통해 드래그 앤 드롭 작업을 허용하는데 필요한 적절한 이벤트(예: `드래그오버`, `드래거터`, `드랍`)를 들을 수 있다.

자세한 내용은 HTML 끌어서 놓기 API에 대한 포괄적인 튜토리얼을 참조하십시오.

handleDragOver 기능에서, 우리는 끌려는 항목이 파일인지 확인하고 드롭 효과를 "복사"로 설정합니다. handleDragEnter도 이와 유사한 기능을 수행해 파일 드래그만 처리할 수 있습니다.

실제 기능은 파일이 삭제될 때 발생하며, 우리는 `handleDrop`을 사용하여 이를 처리합니다. 먼저, 파일을 전송하기 전에 파일을 여는 브라우저의 기본 작업을 방지합니다.

파일이 이미지인지 확인합니다. 그렇지 않은 경우 사용자에게 이미지 파일만 허용한다는 오류 메시지를 보냅니다. 유효성 검사가 통과되면 `handle FileUpload` 기능에서 파일을 처리합니다. 이 기능은 다음에 생성합니다.

다음과 같이 `index.js`를 업데이트합니다.

```php
/**
 * @param {File} file
 */
function handleFileUpload(file) {
  const fileReader = new FileReader();
  fileReader.addEventListener("load", (event) => {
    // Dispatch an event to the profile card containing the updated profile.
    dispatchCardEvent({
      image: event.target.result,
    });
  });
  fileReader.readAsDataURL(file);
}

const profileCard = document.querySelector(".profile-card");
const CARD_UPDATE_EVENT_NAME = "cardupdate";

function dispatchCardEvent(data) {
  profileCard.dispatchEvent(
    new CustomEvent(CARD_UPDATE_EVENT_NAME, {
      detail: data,
    })
  );
}
```

handleFileUpload 기능은 파일을 매개 변수로 사용하고 파일 리더를 사용하여 데이터 URL로 읽기를 시도합니다.

파일 리더 생성자는 이벤트 청취가 가능한 이벤트 타겟에서 확장되었습니다. 로드 이벤트는 이미지가 로드된 후 발생합니다(이 경우 데이터 URL).

다른 형식으로 영상을 로드할 수도 있습니다. 파일 리더에 대해 자세히 알고 싶다면 MDN에는 FileReader API에 대한 훌륭한 문서가 있습니다.

이미지가 로드되면 프로필 카드에 표시해야 합니다. 이를 위해 사용자 정의 이벤트인 `카드 업데이트`를 프로필 카드로 발송하겠습니다. `DispatchCardEvent`는 이벤트 생성 및 전송을 프로파일 카드로 처리합니다.

위의 섹션에서 호출하면 사용자 정의 이벤트에는 데이터를 전달하는 데 사용할 수 있는 `상세` 속성이 있습니다. 이 경우 파일 리더에서 가져온 이미지 URL이 포함된 개체를 전달합니다.

다음으로, 우리는 카드 업데이트를 듣고 그에 따라 DOM을 업데이트하기 위해 프로필 카드가 필요합니다.

```php
profileCard.addEventListener(CARD_UPDATE_EVENT_NAME, handleCardUpdate);
/**
 * @param {CustomEvent} event
 */
function handleCardUpdate(event) {
  const { image } = event.detail;
  if (image) {
    profileCard.querySelector("img").src = image;
  }
}
```

위와 같이 이벤트 수신기를 평소처럼 추가하고 이벤트가 트리거되면 `핸들 카드 업데이트` 기능을 호출하기만 하면 됩니다.

## JavaScript에서 개체 삭제 사용 방법

`handleCardUpdate`는 이벤트를 매개 변수로 수신합니다. 개체 파괴를 사용하면 `event.detail`에서 `image` 속성을 얻을 수 있습니다. 다음으로 프로필 카드에 있는 이미지의 `src` 속성을 이벤트에서 얻은 이미지 URL로 설정합니다.

사용자가 입력 필드를 통해 이미지를 업로드하도록 허용하려면:

```coffeescript
const fileInput = document.querySelector("#file-input");

fileInput.addEventListener("change", (event) => {
  handleFileUpload(event.target.files[0]);
});
```

사용자가 이미지를 선택하면 파일 입력 시 변경 이벤트가 트리거됩니다. 우리는 프로필 카드에 이미지가 하나만 필요하기 때문에 첫 번째 이미지 업로드를 처리할 수 있습니다.

드래그 앤 드롭 지원을 추가할 때 모든 기능을 개발했기 때문에 이제 새로운 작업을 수행할 필요가 없습니다.

다음으로 추가할 기능은 이름 및 작업 업데이트입니다.

```coffeescript
const nameInput = document.querySelector("#name");
const occupationInput = document.querySelector("#occupation");

occupationInput.addEventListener("change", (event) => {
  dispatchCardEvent({
    occupation: event.target.value,
  });
});
nameInput.addEventListener("change", (event) => {
  dispatchCardEvent({
    name: event.target.value,
  });
});
```

이를 위해 변경 이벤트를 청취하고 카드 업데이트 이벤트를 발송하고 있는데 이번에는 다른 데이터로 발송합니다. 이미지 이상을 처리할 수 있도록 핸들러를 업데이트해야 합니다.

```php
/**
 * @param {CustomEvent} event
 */
function handleCardUpdate(event) {
  const { image, name, occupation } = event.detail;
  if (image) {
    profileCard.querySelector("img").src = image;
  }
  if (name) {
    profileCard.querySelector("span.name").textContent = name;
  }
  if (occupation) {
    profileCard.querySelector("span.occupation").textContent = occupation;
  }
} 
```

위의 코드 조각처럼 보이도록 `handleCardUpdate` 기능을 업데이트합니다. 여기서는 개체 파괴를 사용하여 `이벤트.detail`에서 이미지, 이름 및 작업을 가져옵니다. 데이터를 받은 후 프로파일 카드에 표시합니다.

## 결론

사용자 정의 및 기본 DOM 이벤트 모두 이벤트가 발송되는 경우 코드를 보다 쉽게 이해할 수 있습니다. JavaScript 사용자 지정 이벤트는 올바르게 사용되었을 때 앱의 사용자 환경을 향상시킬 수 있습니다. Vue.js(Vue에서는 `$emit`를 사용하여 사용자 지정 이벤트를 발송함)와 같은 상위 JavaScript 프레임워크에 포함되어 있는 것은 놀랄 일이 아닙니다.

이 튜토리얼에 사용된 데모 코드는 GitHub에서 사용할 수 있습니다.