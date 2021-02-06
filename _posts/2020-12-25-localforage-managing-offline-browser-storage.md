---
layout: post
title: "로컬 폴더: 오프라인 브라우저 저장소 관리"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/localstorage-browser-offline-storage.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/localstorage-browser-offline-storage.png?fit=730%2C487&ssl=1)

정적 파일 기반 리소스를 캐시 API로 캐싱하여 인터넷에 연결하지 않고도 정적 페이지를 탐색할 수 있도록 하는 것이 대부분의 사람들이 생각하는 PWA(Progressive Web Apps)의 핵심입니다. 그러나 PWA는 브라우저 스토리지로 훨씬 더 많은 기능을 수행할 수 있습니다. 브라우저 스토리지를 통해 다음과 같은 기능을 갖춘 대화형 오프라인 앱을 구축할 수 있습니다.

- 인터넷 연결이 불량하거나 없는 경우 브라우저에 새 작업을 추가하는 등의 사용자 입력을 저장한 다음 연결이 감지되는 즉시 원격 데이터베이스와 동기화합니다. 이렇게 하면 사용자가 다른 장치에서 로그인하더라도 최신 데이터가 제공됩니다.
- 더 빠른 초기 로드를 위해 API 응답 데이터를 저장한 후 연결이 복원될 때 변경 사항이 있으면 오래된 확인 정책에 따라 나중에 업데이트
- 앱이 항상 이러한 데이터에 대해 서버 호출을 할 필요가 없도록 서버 측에서 거의 변경되지 않는 API 데이터 저장

이러한 요구를 위해 브라우저는 오프라인에서 영구적이고 액세스할 수 있는 두 가지 주요 스토리지 메커니즘인 localStorage와 Indexed를 제공합니다.DB.

하지만, 이 두 가지에는 몇 가지 단점이 있다.

## localStorage를 오프라인 저장 메커니즘으로 사용

한편, localStorage는 간단한 get-and-set API로 사용하기 매우 쉽습니다. 그러나 문자열 유형 데이터만 저장할 수 있습니다. 이는 저장 시 다른 데이터 유형을 JSON.stringify()가 있는 문자열로 변환한 다음 저장소에서 읽을 때 JSON.parse()로 다시 변환해야 한다는 것을 의미하며, 이는 매우 안전하지 않다. 또한 localStorage는 약 5MB의 매우 제한된 스토리지 크기를 제공하며 동기식이며 웹 작업자가 액세스할 수 없으므로 백그라운드 동기화를 지원하지 않습니다.

## 색인 사용오프라인 저장 메커니즘으로서의 DB

반면, 인덱스된 DB는 거의 정확히 우리가 필요로 하는 것입니다. 비동기적이므로 주 스레드를 차단하지 않습니다. 블로그, 파일, 이미지 등 다양한 데이터 유형을 수용하며, 사용자의 디스크 공간과 운영 체제에 따라 최대 1GB까지 저장 제한이 높아 웹 작업자가 접근할 수 있다.

그러나 기본 인덱스 작업DB API는 완전히 악몽과도 같습니다. 데이터를 저장하기 위해 10줄 이상의 코드를 작성해야 하는 경우도 있습니다.

## 현지 용어가 무엇입니까?

여기서 localForage가 나옵니다. (오프라인 스토리지의 개념을 처음 접하셨다면, 다음 단계로 넘어가기 전에 이 내용을 읽어보시기 바랍니다.)

크리에이터들의 말로, 로컬 포지는 오프라인 저장공간이 개선되었습니다. 추상화 레이어를 편리하게 제공하여 오프라인 스토리지를 쉽게 사용할 수 있도록 합니다. 인덱스의 유연성을 겸비하고 있습니다.단순 비동기 localStorage와 유사한 API를 가진 DB. 이것은 우리가 모두 좋아하게 된 비동기식 대기 구문을 사용할 수 있다는 것을 의미합니다.

localForage는 인덱스 DB에 데이터를 저장하고 인덱스 DB에 대한 지원이 없는 경우 localStorage로 다시 이동하도록 설계되어 있습니다(그러나 저장 시 모든 데이터가 직렬화되고 `JSON.stringify()`를 통해 문자열로 변환될 수 있는 데이터만 저장되므로 상당한 성능 및 스토리지 부작용이 있을 수 있습니다).

인덱싱되었기 때문에DB는 현재 모든 주요 브라우저에서 지원되고 있으며 PWA를 작성하는 경우 이전 브라우저에서는 작업해야 하는 대부분의 기능을 지원하지 않으므로 최신 브라우저만 지원하기로 즉시 결정할 수 있습니다.

현재 저희 회사에서는 중소기업용 일체형 SaaS 플랫폼인 PWA 제품에 대해서만 Chrome을 지원하고 있습니다. 로컬포지는 인터넷 연결과 상관없이 이용할 수 있어야 하는 데이터를 저장하는데, 이 앱은 판매 대리점이 주로 사용하기 때문에, 때때로 인터넷 연결이 좋지 않은 파격적인 장소에 있을 수 있기 때문이다.

이 문서의 나머지 부분에서는 다른 데이터베이스와 마찬가지로 로컬 포지를 설정하고 기본 CRUD 작업을 수행하는 방법에 대해 설명합니다. 영업 담당자가 잠재적 고객 연락처 세부 정보를 수집하기 위해 사용하는 가상 CRM 세일즈 앱의 일부를 구성하기 위한 기능을 작성하려고 합니다(대개 연결 문제가 발생할 수 있는 곳). 슬슬 출발 해야지.

## 1. HTML 페이지 설정

먼저, 고객 데이터를 수집하는 양식과 모든 고객 세부 정보를 표시하는 테이블이 포함된 간단한 HTML 페이지를 설정해 보겠습니다.

```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LocalForage Demo</title>

</head>
<body>
    <div>
       <label>Client Name:
           <input id="clientName" type="text" name='clientName' value="">
       </label>
       <label>Phone Number:
           <input id="clientPhone" type="number" name='clientPhone' value="">
       </label>
       <label>Needs:
           <input id="clientNeed" type="text" name='clientNeed' value="">
       </label>
       <button type="submit" id="submit">Save</button>
    </div>

    <div>
        <table>
            <thead>
                <tr>
                    <th>Client Name</th>
                    <th>Phone Number</th>
                    <th>Needs</th>
                    <th>Actions</th>
                    <th>Actions</th>
                    <th>Actions</th>
                </tr>
            </thead>
            <tbody>

            </tbody>
        </table>
    </div>

   <script src="https://cdnjs.cloudflare.com/ajax/libs/localforage/1.9.0/localforage.min.js"></script>
   <script src="index.js"></script>
</body>
</html>
```

이 튜토리얼 프레임워크와 툴링에 구애받지 않도록 CDN 링크를 통해 로컬 Forge를 사용하지만 npm 또는 yarn을 통해 설치할 수 있습니다.

다음으로, 오프라인 데이터베이스를 조금 구성하겠습니다.

```cpp
//in index.js
const ContactTable = localforage.createInstance({
    name: "CRMApp",
    storeName: "ContactTable"
});
```

여기서는 로컬 Forge create를 사용합니다.앱의 새 데이터베이스(`CRMapp`)와 스토어(`ContactTable`)를 생성하는 인스턴스 방법입니다. localForage의 스토어는 데이터베이스의 개별 테이블로 생각할 수 있는 인덱스 DB의 개체 저장소와 같습니다.

저장하려는 데이터의 범주가 서로 다른 저장소가 있는 것이 이상적입니다. 이렇게 하지 않으면 localForage는 기본적으로 "localforage"라는 이름의 데이터베이스와 "key value pairs"라는 이름의 저장소를 설정합니다. 여기서 모든 데이터를 저장할 수 있습니다.

## 2. 새 고객 추가

이제 기본 설정을 마쳤으니, CRUD 기능을 작성하도록 하겠습니다. 예를 들어, 영업 팀이 새로운 고객 세부 정보를 입력에 입력하고 저장 버튼을 클릭하면 모든 입력에서 값을 가져와 객체로 구성해야 합니다.

```js
const getClientDetails = () => {
    const clientName = document.getElementById("clientName").value;
    const clientPhone = document.getElementById("clientPhone").value;
    const clientNeed = document.getElementById("clientNeed").value;

    return {
        clientName: clientName,
        clientPhone: clientPhone,
        clientNeed: clientNeed,
    }
}
```

그런 다음 새 고객 세부 정보 개체를 브라우저 스토리지에 저장하고 테이블에 표시해야 합니다.

```coffeescript
const addInput = async () => {
    const inputValues = getClientDetails();
    const dbLength = await ContactTable.length();
    let id = dbLength === 0 ? 1 : dbLength + 1;
    try{
        let row = `<tr id="${id}">
                        <td>${inputValues.clientName}</td>
                        <td>${inputValues.clientPhone}</td>
                        <td>${inputValues.clientNeed}</td>
                        <td><button class="edit">Edit</button></td>
                        <td><button class="delete"> Delete</button></td>
                    </tr>`
        document.querySelector('tbody').insertAdjacentHTML('afterbegin', row);
        await ContactTable.setItem(id, inputValues);
        alert('contact added successfully');
    }catch(e){
        console.log(err.message);
    }
document.getElementById('submit').addEventListener('click', async(e) => {
    e.preventDefault();
    await addInput();
})
```

addInput 기능을 사용하면 저장 버튼을 클릭하면 getClientDetails 함수를 호출하여 고객 상세 객체를 가져온 다음 ES6 템플릿 리터럴을 사용하여 해당 세부 정보가 포함된 새 행을 HTML 테이블 상단에 추가합니다.

또한 localForage의 길이 방법을 사용하여 개별 항목을 검색하는 데 사용되는 고유 ID를 생성하고 있습니다. 마지막으로 setItem 방법을 사용하여 고객 상세 객체를 `contactTable` 스토어에 추가합니다.

크롬 개발 도구의 응용 프로그램 창을 열어 이 도구를 실제로 볼 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/chrome-dev-tools-page.png?resize=730%2C292&ssl=1)

## 3. 모든 고객 데이터 로드

다음으로, 앱을 새로 고치거나 다시 로드하거나 다른 탭에서 열 때마다 모든 고객 세부 정보 데이터를 읽고 테이블에 표시할 수 있어야 합니다.

```coffeescript
const loadContactsFromStorage = async () => {
    try {
        await ContactTable.iterate((value, key, iterationNumber) => {
            let newContact = `<tr id="${key}">
                                <td>${value.clientName}</td>
                                <td>${value.clientPhone}</td>
                                <td>${value.clientNeed}</td>
                                <td><button class="edit">Edit</button></td>
                                <td><button class="delete"> Delete</button></td>
                                <td><button class="view">View</button></td>
                           </tr>`
            document.querySelector('tbody').insertAdjacentHTML('afterbegin', newContact);
        })
    } catch (err) {
        console.log(err)
    }

}

window.addEventListener('load', async() => {
    await loadContactsFromStorage();
})
```

여기서는 로컬 Forage 반복 방법을 사용하여 오프라인 데이터베이스의 모든 항목을 순환합니다. 반복측정법은 ES6 지도 함수처럼 매 반복마다 호출하는 콜백 함수를 취한다.

이 콜백 함수는 반복 및 반복 번호에서 현재 데이터의 값과 키를 인수로 수신합니다. 템플릿 리터럴을 사용하여 각 개체 키의 값을 테이블에 추가합니다. 그런 다음 로드 이벤트를 듣고 `저장소에서 연락처 로드`를 호출합니다.

사용자가 새 장치를 통해 앱에 액세스하는 경우 길이 속성을 사용하여 저장소에 데이터가 있는지 확인한 다음 원격 데이터베이스에서 데이터를 로드하여 브라우저 저장소에 저장하여 오프라인 액세스를 시도하는 것이 좋습니다.

## 4. 고객 삭제

다음 기능은 영업 사원에게 테이블에서 고객의 연락처 정보를 삭제할 수 있는 기능을 제공하는 것입니다. 이것은 삭제 버튼을 클릭했을 때 오프라인 데이터베이스뿐만 아니라 앱 인터페이스에서도 연락처를 제거해야 함을 의미합니다.

```js
const deleteContact = async(e) => {
    const row = e.target.parentElement.parentElement;
    const key = row.id;
    row.remove();
    try{
        await ContactTable.removeItem(key);
        alert('Contact deleted successfully');
    }catch(err){
        console.log(err.message);
    }

}

window.addEventListener('load', async() => {
     await loadContactsFromStorage();
     document.querySelectorAll('.delete').forEach(button =>{
        button.addEventListener('click', async(e) => {
          await deleteContact(e);
        })
    });
  })
```

여기서는 HTML `parentElement` 특성을 사용하여 클릭된 삭제 버튼이 포함된 행을 가져옵니다. 그런 다음 HTML `remove` 방법을 사용하여 UI에서 해당 행을 제거합니다.

오프라인 저장소에서 제거하려면 대상 행 ID를 키로 전달하는 localForage removeItem 방법을 사용합니다. 또한 이벤트 수신기를 연결하려면 먼저 행을 DOM에 로드해야 하므로 `LoadContactFromStorage` 기능 아래의 로드 이벤트 내의 모든 삭제 버튼에 클릭 이벤트 수신기를 첨부합니다.

## 5. 고객 업데이트

고객의 전화 번호나 니즈가 변경될 수 있습니다. 영업 사원에게 오프라인 상태에서도 고객의 세부 정보를 편집하고 업데이트할 수 있는 기능을 제공하고자 합니다. 이를 위해 편집 버튼을 클릭하면 대상 개별 고객 상세 내역이 포함된 입력 양식과 업데이트 버튼이 있는 모달을 보여줘야 합니다. 모달의 업데이트 버튼을 클릭하면 오프라인 데이터베이스와 테이블의 대상 고객 상세 내역 항목을 업데이트해야 합니다.

먼저 HTML에 모달(모달)을 추가해 보겠습니다.

```xml
<div id="modal">
        <div class="backdrop">
            <div class="form">
                <label><span>Client Name:</span>
                    <input id="editName" type="text" name='editName' value="">
                </label>
                <label><span>Phone Number:</span>
                    <input id="editPhone" type="number" name='editPhone' value="">
                </label>
                <label><span>Needs:</span>
                    <input id="editNeed" type="text" name='editNeed' value="">
                </label>
                <button type="submit" id="update">Update</button>
            </div>
        </div>

    </div>
```

또한 일부 CSS를 추가하여 모달 스타일을 지정하고 초기 로드 시 숨기도록 하겠습니다.

```css
css

#modal{
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  display: none;
}

#modal .backdrop{
    background-color: rgba(43, 40, 40, 0.5);
    width: 100vw;
    height: 100vh;
}

#modal .form{
    background-color: #fff;
    padding: 40px;
    border: 1px solid blue;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 1;
}
```

이제 업데이트 작업의 첫 번째 부분인 편집 버튼을 클릭했을 때 정보를 검색하는 기능을 작성하겠습니다.

```js
let updateKey;

const viewContact = async(e) => {
    updateKey =  e.target.parentElement.parentElement.id;

    const editName = document.getElementById("editName");
    const editPhone = document.getElementById("editPhone");
    const editNeed = document.getElementById("editNeed");

    try{
        const contact = await ContactTable.getItem(updateKey);

        editName.value = contact.clientName;
        editPhone.value = contact.clientPhone;
        editNeed.value = contact.clientNeed;

        document.getElementById("modal").style.display= "block";

    }catch(err){
        console.log(err.message);
    }

}

window.addEventListener('load', async() => {
    await loadContactsFromStorage();
    ....
    document.querySelectorAll('.edit').forEach(button =>{
        button.addEventListener('click', async(e) => {
          await viewContact(e);
        })
    })
})
```

여기서는 글로벌 변수인 updateKey를 생성하는데, 이를 여러 함수에 활용할 것이기 때문입니다. 연락처 보기 기능은 클릭된 버튼이 포함된 테이블 행의 ID를 가져옵니다.

그런 다음 localForagegetItem 방법을 사용하여 오프라인 데이터베이스에서 제공된 키와 일치하는 단일 키 값 쌍 항목에서 값을 검색합니다. 그런 다음 반환된 값을 모달 입력 값으로 표시하고 모달 디스플레이를 블록으로 설정합니다.

또한 이전 단계와 동일한 이유로 클릭 이벤트 수신기를 로드 이벤트 내의 모든 편집 버튼에 첨부했습니다.

다음으로, 모달의 업데이트 버튼을 클릭했을 때 발생하는 업데이트 작업의 두 번째 부분을 처리하기 위한 기능을 작성해야 합니다.

```coffeescript
const updateContact = async() => {

    try{
        const updatedClient ={
            clientName: document.getElementById("editName").value,
            clientPhone: document.getElementById("editPhone").value,
            clientNeed: document.getElementById("editNeed").value,
        }

        let updatedRow = `<tr id="${updateKey}">
                        <td>${updatedClient.clientName}</td>
                        <td>${updatedClient.clientPhone}</td>
                        <td>${updatedClient.clientNeed}</td>
                        <td><button class="edit">Edit</button></td>
                        <td><button class="delete"> Delete</button></td>
                    </tr>`

        document.getElementById(`${updateKey}`).remove();
        document.querySelector('tbody').insertAdjacentHTML('afterbegin', updatedRow);

        await ContactTable.setItem(updateKey, updatedClient);

        document.getElementById("modal").style.display= "none";

        alert('Contact updated successfully');
    }catch(err){
        console.log(err.message);
    }
}


document.getElementById('update').addEventListener('click', async(e) => {
    e.preventDefault();
    await updateContact();
})
```

`업데이트 연락처` 기능은 모달 입력에서 업데이트된 고객의 세부 정보를 가져온 다음 템플릿 리터럴을 사용하여 해당 세부 정보를 HTML 테이블 행으로 구성합니다. 그런 다음 테이블에서 이전 데이터가 있는 행을 삭제한 후 생성된 행을 테이블 시작 부분에 추가합니다.

휴! 등을 한 번 두드려 보세요. 로컬 포지를 사용하여 완전히 작동하는 오프라인 CRUD 앱을 성공적으로 만들었습니다.

이해를 돕기 위해 우리 코드에 몇 가지 반복이 있었다. 드라이아웃을 시도할 수 있습니다. 또한, 이 github repo에 있는 전체 코드베이스를 보고 여기서 라이브 데모를 확인할 수 있습니다.

다음 단계는 스토리지 흐름에 백그라운드 동기화를 구현하는 것입니다. 사용하기로 결정한 원격 데이터베이스에 따라 접근 방식이 달라집니다. 걱정마세요, 시작할 수 있도록 아래에 몇 가지 기사를 추가했습니다.

여기서 논의된 모든 개념이 적용된 데모 프로젝트를 보시려면 제가 얼마 전에 인덱스 DB로 구축한 이 비용과 수입 추적기 PWA를 확인해 보십시오. 오프라인으로 작동합니다.

원격 방화벽 데이터베이스에 백그라운드 동기화를 구현한 방법을 보려면 controller.js 파일, line 56 및 sw.js를 살펴보십시오.

## 결론

이 기사에서는 브라우저 오프라인 스토리지의 강력한 기능과 놀라운 유용성, 그리고 로컬 Forge 라이브러리가 이러한 스토리지를 사용하여 작업을 훨씬 쉽게 수행하고 관리하는 방법을 소개했습니다. 또한 로컬 포지를 사용하여 브라우저 스토리지에서 기본 CRUD 기능을 수행하는 방법을 보여드렸습니다. 이제, 새로 얻은 이 지식으로 멋진 것을 만들어 보세요!