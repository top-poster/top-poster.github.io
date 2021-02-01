---
layout: post
title: "JavaScript 양식 유효성 검사 – JS 예제 코드를 사용하여 HTML 양식에서 사용자 입력을 확인하는 방법
 "
author: 'Code Tower'
thumbnail: https://images.unsplash.com/photo-1554252116-30abdf759321?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MXwxMTc3M3wwfDF8c2VhcmNofDJ8fGZvcm1zfGVufDB8fHw&ixlib=rb-1.2.1&q=80&w=2000
tags: undefined
---


양식은 웹 애플리케이션에서 어디에나 있습니다.
 일부 앱은 양식을 사용하여 데이터를 수집하여 사용자를 등록하고 이메일 주소를 제공합니다.
 다른 사람들은 쇼핑 경험을 촉진하기 위해 온라인 거래를 수행하는 데 사용합니다.
 

일부 웹 양식을 사용하여 새 자동차 대출을 신청할 수 있지만 다른 양식을 사용하여 저녁 식사로 피자를 주문할 수 있습니다.
 따라서 이러한 양식에서 수집 된 데이터를 정리하고 올바른 형식을 지정하고 악성 코드가없는 것이 중요합니다.
 이 프로세스를 양식 유효성 검사라고합니다.
 

사용자 입력을받을 때마다 양식 유효성 검사가 필요합니다.
 입력 한 데이터가 올바른 형식인지, 유효한 데이터 범위 (예 : 날짜 필드) 내에 있는지, SQL 삽입으로 이어질 수있는 악성 코드가 포함되어 있지 않은지 확인해야합니다.
 잘못된 데이터 또는 누락 된 데이터로 인해 API에서 오류가 발생할 수도 있습니다.
 

## 양식 유효성 검사에는 어떤 유형이 있습니까?
 

양식 유효성 검사는 클라이언트 측과 서버 측에서 발생할 수 있습니다.
 

클라이언트 측 유효성 검사는 HTML5 속성 및 클라이언트 측 JavaScript를 사용하여 발생합니다.
 

일부 양식에서는 유효하지 않은 이메일 주소를 입력하자마자 "유효한 이메일을 입력하십시오"라는 오류 메시지가 나타납니다.
 이 즉각적인 유형의 유효성 검사는 일반적으로 클라이언트 측 JavaScript를 통해 수행됩니다.
 

다른 경우에는 양식을 작성하고 신용 카드와 같은 세부 정보를 입력 할 때 로딩 화면이 표시되고 "이 신용 카드가 유효하지 않습니다"라는 오류가 표시 될 수 있습니다.
 

여기에서 양식은 서버 측 코드를 호출하고 추가 신용 카드 확인을 수행 한 후 유효성 검사 오류를 반환했습니다.
 서버 측 호출이 이루어지는이 유효성 검사 사례를 서버 측 유효성 검사라고합니다.
 

## 어떤 데이터를 검증해야합니까?
 

사용자로부터 데이터를 수락 할 때마다 양식 유효성 검사가 필요합니다.
 여기에는 다음이 포함될 수 있습니다.
 

- 이메일 주소, 전화 번호, 우편 번호, 이름, 비밀번호와 같은 필드 형식을 확인합니다.
 
- 필수 필드 확인
 
- 사회 보장 번호와 같은 필드에 대한 문자열 대 숫자와 같은 데이터 유형 확인.
 
- 입력 한 값이 국가, 날짜 등과 같은 유효한 값인지 확인합니다.
 

## 클라이언트 측 유효성 검사를 설정하는 방법
 

클라이언트 측에서 유효성 검사는 두 가지 방법으로 수행 할 수 있습니다.
 

- HTML5 기능 사용
 
- JavaScript 사용
 

### HTML5 기능으로 유효성 검사를 설정하는 방법
 

HTML5는 데이터의 유효성을 검사하는 데 도움이되는 다양한 속성을 제공합니다.
 다음은 몇 가지 일반적인 유효성 검사 사례입니다.
 

- `필수`를 사용하여 필수 필드 만들기
 
- 데이터 길이 제한 :
`minlength`,`maxlength` : 텍스트 데이터 용
num 유형의 최대 값에 대한`min` 및`max`
 
- `minlength`,`maxlength` : 텍스트 데이터 용
 
- num 유형의 최대 값에 대한`min` 및`max`
 
- `type`을 사용하여 데이터 유형 제한 :
`<input type = "email"name = "multiple>`
 
- `<input type = "email"name = "multiple>`
 
- `pattern`을 사용하여 데이터 패턴 지정 :
입력 한 양식 데이터가 일치해야하는 정규식 패턴을 지정합니다.
 
- 입력 한 양식 데이터가 일치해야하는 정규식 패턴을 지정합니다.
 

입력 값이 위의 HTML5 유효성 검사와 일치하면 의사 클래스`: valid`가 할당되고 그렇지 않으면`: invalid`가 할당됩니다.
 

예를 들어 보겠습니다.
 

```html
<form>
<label for="firstname"> First Name: </label>
<input type="text" name="firstname" id="firstname" required maxlength="45">
<label for="lastname"> Last Name: </label>
<input type="text" name="lastname" id="lastname" required maxlength="45">
<button>Submit</button>
</form>

```

JSFiddle에 링크
 

여기에는 이름과 성이라는 두 가지 필수 필드가 있습니다.
 JSFidle에서이 예제를 시도하십시오.
 이 필드 중 하나를 건너 뛰고 제출을 누르면 "이 필드를 작성하십시오"라는 메시지가 표시됩니다.
 이것은 내장 HTML5를 사용한 유효성 검사입니다.
 

### JavaScript를 사용하여 유효성 검사를 설정하는 방법
 

양식 유효성 검사를 구현할 때 고려해야 할 몇 가지 사항이 있습니다.
 

- "유효한"데이터 란 무엇입니까?
 이를 통해 형식, 길이, 필수 필드 및 데이터 유형에 대한 질문에 답할 수 있습니다.
 
- 유효하지 않은 데이터를 입력하면 어떻게됩니까?
 이렇게하면 유효성 검사의 사용자 경험을 정의하는 데 도움이됩니다. 오류 메시지를 인라인으로 표시할지 또는 양식 상단에 표시할지, 오류 메시지를 얼마나 자세하게 표시해야하는지, 양식을 제출해야하는지, 잘못된 형식을 추적하는 분석이있는 경우
 데이터?
 등등.
 

두 가지 방법으로 JavaScript 유효성 검사를 수행 할 수 있습니다.
 

- JavaScript를 사용한 인라인 유효성 검사
 
- HTML5 제약 조건 유효성 검사 API
 

```html
<form id="form">
  <label for="firstname"> First Name* </label>
  <input type="text" name="firstname" id="firstname" />
  <button id="submit">Submit</button>

  <span role="alert" id="nameError" aria-hidden="true">
    Please enter First Name
  </span>
</form>

```

```js
const submit = document.getElementById("submit");

submit.addEventListener("click", validate);

function validate(e) {
  e.preventDefault();

  const firstNameField = document.getElementById("firstname");
  let valid = true;

  if (!firstNameField.value) {
    const nameError = document.getElementById("nameError");
    nameError.classList.add("visible");
    firstNameField.classList.add("invalid");
    nameError.setAttribute("aria-hidden", false);
    nameError.setAttribute("aria-invalid", true);
  }
  return valid;
}

```

```css
#nameError {
  display: none;
  font-size: 0.8em;
}

#nameError.visible {
  display: block;
}

input.invalid {
  border-color: red;
}

```

JSFiddle에 링크
 

이 예에서는 JavaScript를 사용하여 필수 필드를 확인합니다.
 필수 필드가 없으면 CSS를 사용하여 오류 메시지를 표시합니다.
 

Aria 라벨은 오류를 알리기 위해 그에 따라 수정됩니다.
 CSS를 사용하여 오류를 표시 / 숨김으로써 필요한 DOM 조작 수를 줄이고 있습니다.
 오류 메시지는 상황에 맞게 제공되므로 사용자 경험을 직관적으로 만듭니다.
 

`required` 및`pattern` HTML 속성은 기본 유효성 검사를 수행하는 데 도움이 될 수 있습니다.
 그러나 더 복잡한 유효성 검사를 원하거나 자세한 오류 메시지를 제공하려는 경우 Constraint Validation API를 사용할 수 있습니다.
 

이 API에서 제공하는 몇 가지 메소드는 다음과 같습니다.
 

- `checkValidity`
 
- `setCustomValidity`
 
- `reportValidity`
 

다음 속성이 유용합니다.
 

- `유효성`
 
- `validationMessage`
 
- `willValidate`
 

이 예에서는 Constraint Validation API와 함께`required` 및`length`와 같은 HTML5 내장 메서드를 사용하여 자세한 오류 메시지를 제공하는 유효성을 검사합니다.
 

```html
<form>
<label for="firstname"> First Name: </label>
<input type="text" name="firstname" required id="firstname">
<button>Submit</button>
</form>

```

```js
const nameField = document.querySelector("input");

nameField.addEventListener("input", () => {
  nameField.setCustomValidity("");
  nameField.checkValidity();
  console.log(nameField.checkValidity());
});

nameField.addEventListener("invalid", () => {
  nameField.setCustomValidity("Please fill in your First Name.");
});

```

JSFiddle에 링크
 

## 서버 측 유효성 검사를 잊지 마세요
 

클라이언트 측 유효성 검사가 수행해야하는 유일한 유효성 검사는 아닙니다.
 또한 서버 측 코드에서 클라이언트로부터받은 데이터의 유효성을 검사하여 데이터가 예상 한 것과 일치하는지 확인해야합니다.
 

서버 측 유효성 검사를 사용하여 클라이언트 측에 있으면 안되는 비즈니스 로직 확인을 수행 할 수도 있습니다.
 

## 양식 유효성 검사 모범 사례
 

- 악의적 인 행위자가 클라이언트 측 유효성 검사를 우회 할 수 있으므로 항상 서버 측 유효성 검사를 수행합니다.
 
- 오류를 생성 한 필드와 상황에 맞는 자세한 오류 메시지를 제공합니다.
 
- "이메일이 형식과 일치하지 않음-test@example.com"과 같은 오류 메시지의 경우 데이터가 어떻게 표시되어야하는지에 대한 예를 제공합니다.
 
- 리디렉션과 관련된 단일 오류 페이지를 사용하지 마십시오.
 이것은 나쁜 사용자 경험이며 사용자가 양식을 수정하고 컨텍스트를 잃기 위해 이전 페이지로 돌아가도록 강제합니다.
 
- 항상 필수 필드를 표시하십시오.
 

### 이와 같은 더 많은 튜토리얼과 기사에 관심이 있으십니까?
 내 뉴스 레터에 가입하십시오.
 또는 Twitter에서 나를 따르십시오.
 