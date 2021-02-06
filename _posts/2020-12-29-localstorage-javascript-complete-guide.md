---
layout: post
title: "JavaScript의 localStorage: 완전한 안내서"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2018/07/javascript-localstorage.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2018/07/javascript-localstorage.png?fit=730%2C487&ssl=1)

편집자 참고: 이 게시물은 2020년 12월 29일에 업데이트되었습니다.

이 튜토리얼에서는 `localStorage` 메커니즘 및 `Window.localStorage` 속성을 사용하는 방법과 JavaScript에서 웹 스토리지의 기본 사항에 대해 살펴봅니다.

다음 사항에 대해 자세히 설명합니다.

- 웹 스토리지 API란 무엇입니까?
- 세션 스토리지와 로컬 스토리지의 차이점은 무엇입니까?
- JavaScript에서 localStorage란?
- localStorage는 어디에 저장됩니까?
- Windows.localStorage란?
- localStorage는 어떻게 작동합니까?
setItem(): `localStorage`에 값을 저장하는 방법
getItem(): localStorage에서 항목을 가져오는 방법
`제거 항목()`: `localStorage` 세션을 삭제하는 방법
clear(): localStorage의 모든 항목을 삭제하는 방법
key(): localStorage에서 키 이름을 얻는 방법
- setItem(): `localStorage`에 값을 저장하는 방법
- getItem(): localStorage에서 항목을 가져오는 방법
- `제거 항목()`: `localStorage` 세션을 삭제하는 방법
- clear(): localStorage의 모든 항목을 삭제하는 방법
- key(): localStorage에서 키 이름을 얻는 방법
- `localStorage` 브라우저 지원
- 로컬 스토리지 제한

## 웹 스토리지 API란 무엇입니까?

웹 스토리지 API는 브라우저가 키/값 쌍을 저장할 수 있는 메커니즘의 집합입니다. 그것은 쿠키를 사용하는 것보다 훨씬 더 직관적이도록 설계되었다.

키/값 쌍은 페이지 로드 중에 손상되지 않고 항상 문자열로 유지되는 경우를 제외하고 개체와 유사한 스토리지 개체를 나타냅니다. 개체나 getItem() 메서드를 사용하여 이러한 값에 액세스할 수 있습니다(나중에 추가).

### 세션 스토리지와 로컬 스토리지의 차이점은 무엇입니까?

Web Storage API는 sessionStorage와 localStorage라는 두 가지 메커니즘으로 구성되어 있습니다. 세션 저장소와 로컬 저장소 모두 페이지 세션 기간 동안 사용 가능한 각 오리진에 대해 별도의 저장소 영역을 유지합니다.

세션스토리지와 로컬스토리지는 브라우저가 열려 있는 동안(페이지 다시 로드 또는 복원 시 포함) 저장 영역만 유지하고 로컬스토리지는 브라우저를 닫은 후에도 데이터를 계속 저장한다는 점이 가장 큰 차이점이다. 즉, 페이지를 닫으면 세션 저장소에 저장된 데이터가 지워지지만 로컬 저장소에 저장된 데이터는 만료되지 않습니다.

이 튜토리얼에서는 JavaScript에서 `localStorage`를 사용하는 방법에 대해 살펴보겠습니다.

## JavaScript에서 localStorage란?

localStorage는 자바스크립트 사이트와 앱이 만료일 없이 웹 브라우저에 키/값 쌍을 저장할 수 있는 속성이다. 이는 브라우저 창이 닫힌 후에도 브라우저에 저장된 데이터가 지속됨을 의미합니다.

JavaScript에서 `localStorage`를 사용하는 방법에 대한 시각적 새로 고침을 보려면 아래의 비디오 튜토리얼을 확인하십시오.

### localStorage는 어디에 저장됩니까?

Google Chrome에서 웹 스토리지 데이터는 사용자 프로필의 하위 폴더에 있는 SQLite 파일에 저장됩니다. 하위 폴더는 `\AppData\Local\Google\`에 있습니다.Windows 컴퓨터에서 Chrome\User Data\Default\Local Storage를 사용하고 MacOS에서 `~/Library/Application Support/Google/Chrome/Default/Local Storage`를 사용합니다.

Firefox는 사용자의 프로필 폴더에도 있는 `webappstore.sqlite`라는 SQLite 파일에 스토리지 개체를 저장합니다.

## Window.localStorage란?

localStorage 메커니즘은 `Windows.localStorage` 속성을 통해 사용할 수 있습니다. Window.localStorage는 자바스크립트에서 DOM 문서를 포함하는 윈도우를 나타내는 윈도우 인터페이스의 일부이다.

윈도 인터페이스는 다양한 기능, 생성자, 객체, 네임스페이스를 갖추고 있다. Window.localStorage는 데이터를 생성한 오리진에만 액세스할 수 있는 데이터를 저장하는 데 사용되는 로컬 스토리지 개체에 대한 참조를 반환하는 읽기 전용 속성입니다.

## 로컬 스토리지는 어떻게 작동합니까?

웹 응용 프로그램에서 `localStorage`를 사용하려면 다음과 같은 다섯 가지 방법 중 하나를 선택해야 합니다.

- setItem(): `localStorage`에 키 및 값 추가
- get Item(): localStorage에서 아이템을 얻는 방법입니다.
- `제거 항목()`: `localStorage`에서 키로 항목을 제거합니다.
- clear(): 로컬 스토리지 모두 지우기
- 키(): `localStorage`의 키를 검색할 수 있는 번호 전달

### setItem(): 'localStorage'에 값을 저장하는 방법

이름에서 알 수 있듯이 이 방법을 사용하면 `localStorage` 개체에 값을 저장할 수 있습니다.

키와 값의 두 가지 매개 변수가 필요합니다. 키는 나중에 참조하여 연결된 값을 가져올 수 있습니다.

```coffeescript
window.localStorage.setItem('name', 'Obaseki Nosa');
```

여기서 이름이 관건이고 오바세키 노사가 값이다. localStorage는 문자열만 저장할 수 있습니다.

배열 또는 개체를 저장하려면 해당 배열을 문자열로 변환해야 합니다.

이를 위해 `setItem()에 전달하기 전에 `JSON.stringify() 방법을 사용한다.

```js
const person = {
    name: "Obaseki Nosa",
    location: "Lagos",
}

window.localStorage.setItem('user', JSON.stringify(person));
```

### getItem(): localStorage에서 항목을 가져오는 방법

localStorage에서 항목을 가져오려면 `getItem()` 방법을 사용합니다. getItem()을 사용하면 브라우저의 `localStorage` 개체에 저장된 데이터에 액세스할 수 있습니다.

getItem()은 키인 매개 변수를 하나만 허용하고 값을 문자열로 반환합니다.

사용자 키를 검색하려면:

```coffeescript
window.localStorage.getItem('user');
```

값이 다음과 같은 문자열이 반환됩니다.

```undefined
“{“name”:”Obaseki Nosa”,”location”:”Lagos”}”
```

이 값을 사용하려면 개체로 다시 변환해야 합니다.

이를 위해 JSON 문자열을 JavaScript 개체로 변환하는 JSON.parse() 방법을 사용한다.

```js
JSON.parse(window.localStorage.getItem('user'));
```

### '제거 항목()': 'localStorage' 세션을 삭제하는 방법

로컬 저장소 세션을 삭제하려면 `removeItem()` 방법을 사용하십시오.

키 이름을 전달하면 `removeItem() 메서드는 해당 키가 있는 경우 해당 키를 저장소에서 제거합니다. 지정된 키와 연결된 항목이 없으면 이 메서드는 아무 작업도 수행하지 않습니다.

```coffeescript
window.localStorage.removeItem('name');
```

### clear(): localStorage의 모든 항목을 삭제하는 방법

clear() 방법을 사용하여 `localStorage`의 모든 항목을 삭제합니다.

이 메서드는 호출되면 해당 도메인에 대한 모든 레코드의 전체 저장소를 지웁니다. 매개 변수를 수신하지 않습니다.

```css
window.localStorage.clear();
```

### key(): localStorage에서 키 이름을 얻는 방법

키() 방법은 키를 순환해야 하는 상황에서 유용하며 숫자 또는 인덱스를 로컬 저장소에 전달하여 키 이름을 검색할 수 있습니다.

```js
var KeyName = window.localStorage.key(index);
```

## 'localStorage' 브라우저 지원

웹 스토리지의 일종인 로컬 스토리지는 HTML5 규격이다. IE8을 포함한 주요 브라우저에서 지원한다. 브라우저가 `localStorage`를 지원하는지 확인하기 위해 다음 조각을 사용하여 확인할 수 있습니다.

```undefined
if (typeof(Storage) !== "undefined") {
    // Code for localStorage
    } else {
    // No web storage Support.
}
```

## 로컬 스토리지 제한

로컬 스토리지(local storage)를 쉽게 사용할 수 있는 만큼 오용도 쉽다. 다음은 제한 사항이며 `localStorage`를 사용하지 않는 방법입니다.

- 중요한 사용자 정보를 `localStorage`에 저장하지 않음
- 정보는 브라우저에만 저장되므로 서버 기반 데이터베이스를 대체할 수 없습니다.
- 로컬 스토리지는 모든 주요 브라우저에서 5MB로 제한됩니다.
- `local storage`는 데이터 보호 형태가 없고 웹 페이지의 어떤 코드로도 액세스할 수 있기 때문에 매우 불안정합니다.
- localStorage는 동기식이며 호출된 각 작업이 차례로 실행되어야 한다는 의미입니다.

이를 통해 우리는 웹 애플리케이션에서 로컬 스토리지의 강력한 기능을 갖추게 되었습니다.