---
layout: post
title: "React Hook Form에 대한 완전한 가이드
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/react-white.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/react-white.png?fit=750%2C487&ssl=1)

양식은 사용자가 웹 사이트 및 웹 응용 프로그램과 상호 작용하는 방식의 필수적인 부분입니다.
 양식을 통해 전달 된 사용자 데이터의 유효성을 검사하는 것은 개발자에게 중요한 책임입니다.
 

React-hook-form은 React에서 양식의 유효성을 검사하는 데 도움이되는 라이브러리입니다.
 React-hook-form은 다른 종속성이없는 최소 라이브러리입니다.
 성능이 뛰어나고 사용이 간편하므로 개발자가 다른 양식 라이브러리보다 적은 수의 코드를 작성해야합니다.
 

이 가이드에서는 React Hook Form 라이브러리를 사용하여 복잡한 render prop 또는 상위 구성 요소를 사용하지 않고 React에서 우수한 양식을 만드는 방법을 배웁니다.
 

## React Hook 양식 : 소개
 

React Hook Form은 React 생태계의 다른 양식 라이브러리와 약간 다른 접근 방식을 취합니다.
 React Hook Form은 입력을 제어하는 상태에 의존하는 대신 `ref`를 사용하여 제어되지 않은 입력을 사용합니다.
 이 접근 방식은 양식의 성능을 높이고 다시 렌더링하는 횟수를 줄입니다.
 

패키지 크기는 매우 작고 최소입니다. 압축 된 9.1KB에 불과하고 gzip으로 압축되며 종속성이 없습니다.
 API는 양식 작업시 개발자에게 원활한 경험을 제공하는 매우 직관적입니다.
 React Hook Form은 제약 기반 검증 API를 사용하여 양식을 검증하기 위해 HTML 표준을 따릅니다.
 

React Hook Form이 제공하는 또 다른 훌륭한 기능은 대부분의 라이브러리가`ref`를 지원하기 때문에 UI 라이브러리와의 간편한 통합입니다.
 

React Hook Form을 설치하려면 다음 명령을 실행하십시오.
 

```undefined
npm install react-hook-form
```

### 간단한 등록 양식
 

이 섹션에서는 매우 기본적인 등록 양식을 만들어`useForm` Hook의 기본 사항에 대해 알아 봅니다.
 

먼저,`react-hook-form` 패키지에서`useForm` 후크를 가져옵니다.
 

```coffeescript
import { useForm } from "react-hook-form";
```

그런 다음 구성 요소 내에서 다음과 같이 후크를 사용합니다.
 

```cpp
const { register, handleSubmit } = useForm();
```

`useForm` 후크는 속성이 거의없는 객체를 반환합니다.
 지금은`register`와`handleSubmit` 만 필요합니다.
 

`register` 메서드는 입력 필드를 React Hook Form에 등록하여 유효성 검사에 사용할 수 있고 해당 값이 변경 사항을 추적 할 수 있도록 도와줍니다.
 입력을 등록하려면 입력의`ref> / code> prop에`register` 메소드를 전달합니다.
 

```xml
<input type="text" ref={register} name="firstName" />
```

여기서 주목해야 할 중요한 점은 입력 구성 요소에`name` prop이 있어야하며 해당 값은 고유해야한다는 것입니다.
 

이름에서 알 수 있듯이`handleSubmit` 메소드는 양식 제출을 관리합니다.
 `form` 컴포넌트의`onSubmit` prop에 prop으로 전달해야합니다.
 

`handleSubmit` 메소드는 두 개의 함수를 인수로 처리 할 수 있습니다.
 인수로 전달 된 첫 번째 함수는 양식 유효성 검사가 성공하면 등록 된 필드 값과 함께 호출됩니다.
 두 번째 함수는 유효성 검사에 실패하면 오류와 함께 호출됩니다.
 

```js
const onFormSubmit  = data => console.log(data);

const onErrors = errors => console.error(errors);

<form onSubmit={handleSubmit(onFormSubmit, onErrors)}>
{/* ... */}
</form>
```

이제`useForm` 후크의 기본 사용법에 대한 공정한 아이디어를 얻었으므로보다 실제적인 예를 살펴보십시오.
 

```xml
import React from "react";
import { useForm } from "react-hook-form";
import { Form, FormGroup, Label, Input, Button } from "reactstrap";
const RegisterForm = () => {
  const { register, handleSubmit } = useForm();
  const handleRegistration = (data) => console.log(data);
  return (
    <Form onSubmit={handleSubmit(handleRegistration)}>
      <FormGroup>
        <Label>Name</Label>
        <Input name="name" innerRef={register} />
      </FormGroup>
      <FormGroup>
        <Label>Email</Label>
        <Input type="email" name="email" innerRef={register} />
      </FormGroup>
      <FormGroup>
        <Label>Password</Label>
        <Input type="password" name="password" innerRef={register} />
      </FormGroup>
      <Button color="primary">Submit</Button>
    </Form>
  );
};
export default RegisterForm;
```

보시다시피 입력 값을 추적하기 위해 다른 구성 요소를 가져 오지 않았습니다.
 `useForm` 후크는 구성 요소 코드를 더 깔끔하고 유지 관리하기 쉽게 만듭니다.
 그리고 양식이 제어되지 않기 때문에 `onChange`및 `value`와 같은 소품을 각 입력에 전달할 필요가 없습니다.
 

위의 예에서`reactstrap` 라이브러리는 양식 UI를 빌드하는 데 사용됩니다.
 여기서`register` 메소드가`ref` 대신`innerRef` prop에 전달되었음을 알 수 있습니다.
 이는`reactstrap` 양식 구성 요소가`innerRef` prop을 사용하여 기본 DOM 입력에 대한 액세스를 제공하기 때문입니다.
 

양식을 작성하기 위해 선택한 다른 UI 라이브러리를 사용할 수 있습니다.
 기본 입력 구성 요소에 대한 참조에 액세스하려면 prop을 확인하십시오.
 

다음 섹션에서는 위의 양식에서 양식 유효성 검사를 처리하는 방법을 배웁니다.
 

### 유효성 검사 및 오류 처리
 

필드에 유효성 검사를 적용하려면 register 메서드에 유효성 검사 옵션을 전달할 수 있습니다.
 유효성 검사 옵션은 기존 HTML 양식 유효성 검사 표준과 유사합니다.
 

유효성 검사 옵션에는 다음 속성이 포함됩니다.
 

- `required` – 필드가 필수인지 여부를 나타냅니다.
 이 속성이 true로 설정되면 필드를 비워 둘 수 없습니다.
 
- `minlength` 및`maxlength`는 문자열 입력 값의 최소 및 최대 길이를 설정합니다.
 
- `min` 및`max` – 숫자 값의 최소값과 최대 값을 설정합니다.
 
- `type` – 입력 필드의 유형을 나타냅니다.
 이메일, 숫자, 텍스트 또는 기타 표준 HTML 입력 유형일 수 있습니다.
 
- `pattern` – 정규 표현식을 사용하여 입력 값의 패턴을 정의합니다.
 

필드를 필수로 표시하려면 다음과 같이 작성할 수 있습니다.
 

```cpp
<Input name="name" innerRef={register({ required: true })} />
```

제출시 다음 오류 개체가 발생합니다.
 

```css
{
name: {
  type: "required"
  message: ""
  ref: <INPUT name="name" type="text" class="form-control"></INPUT>
  }
}
```

여기서`type` 속성은 실패한 유효성 검사의 유형을 나타내며`ref` 속성에는 기본 DOM 입력 요소가 포함됩니다.
 

유효성 검사 속성에 부울 대신 문자열을 전달하여 필드에 대한 오류 메시지를 전달할 수도 있습니다.
 

```js
// ...
<Form onSubmit={handleSubmit(handleRegistration, handleError)}>
  <FormGroup>
      <Label>Name</Label>
      <Input name="name" innerRef={register({ required: "Name is required" })} />
  </FormGroup>
</Form>
```

`useForm` 후크에서 오류 객체에 액세스 할 수도 있습니다.
 

```cpp
const { register, handleSubmit, errors } = useForm();
```

아래에서 완전한 예를 찾을 수 있습니다.
 

```xml
import React from "react";
import { useForm } from "react-hook-form";
import { Form, FormGroup, Label, Input, Button } from "reactstrap";
const RegisterForm = () => {
  const { register, handleSubmit, errors } = useForm();
  const handleRegistration = (data) => console.log(data);
  const handleError = (errors) => {};
  const registerOptions = {
    name: { required: "Name is required" },
    email: { required: "Email is required" },
    password: {
      required: "Password is required",
      minLength: {
        value: 8,
        message: "Password must have at least 8 characters"
      }
    }
  };
  return (
    <Form onSubmit={handleSubmit(handleRegistration, handleError)}>
      <FormGroup>
        <Label>Name</Label>
        <Input name="name" innerRef={register(registerOptions.name)} />
        <small className="text-danger">
          {errors.name && errors.name.message}
        </small>
      </FormGroup>
      <FormGroup>
        <Label>Email</Label>
        <Input
          type="email"
          name="email"
          innerRef={register(registerOptions.email)}
        />
        <small className="text-danger">
          {errors.email && errors.email.message}
        </small>
      </FormGroup>
      <FormGroup>
        <Label>Password</Label>
        <Input
          type="password"
          name="password"
          innerRef={register(registerOptions.password)}
        />
        <small className="text-danger">
          {errors.password && errors.password.message}
        </small>
      </FormGroup>
      <Button color="primary">Submit</Button>
    </Form>
  );
};
export default RegisterForm;
```

`onChange` 또는`onBlur` 이벤트가있을 때 필드의 유효성을 검사하려면`useForm` 후크에`mode` 속성을 전달할 수 있습니다.
 

```cpp
const { register, handleSubmit, errors } = useForm({
  mode: "onBlur"
});
```

API 참조에서`useForm` 후크에 대한 자세한 내용을 찾을 수 있습니다.
 

## 타사 구성 요소와 함께 사용
 

경우에 따라 양식에서 사용하려는 외부 UI 구성 요소가 `ref`를 지원하지 않고 상태에 의해서만 제어 될 수 있습니다.
 

React Hook Form에는 이러한 사용 사례에 대한 조항이 있으며 타사 제어 구성 요소와 쉽게 통합 할 수 있습니다.
 

React Hook Form은`register` 메서드가 작동하는 방식과 유사하게 제어되는 외부 구성 요소를 등록 할 수있는`Controller`라는 래퍼 구성 요소를 제공합니다.
 이 경우`register` 메소드 대신`useForm` Hook의`control` 객체를 사용합니다.
 

```cpp
const { register, handleSubmit, errors, control } = useForm();
```

선택 입력에서 값을 허용하는 역할 필드를 양식에 작성해야한다고 고려하십시오.
 `react-select`라이브러리를 사용하여 선택 입력을 생성 할 수 있습니다.
 

`control` 객체는 필드의`name`과 함께`Controller` 구성 요소의`control` prop에 전달되어야합니다.
 `rules` 소품을 사용하여 유효성 검사 규칙을 지정할 수 있습니다.
 

제어되는 구성 요소는`as` 소품을 사용하여`Controller` 구성 요소에 전달되어야합니다.
 `as` prop은`onChange`,`onBlur`,`value` prop을 컴포넌트에 삽입합니다.
 `Select` 구성 요소에는 드롭 다운 옵션을 렌더링하기위한`options` prop도 필요합니다.
 따라서`options` 소품을`Controller` 구성 요소에 직접 추가 할 수 있으며 다른 추가 소품과 함께`Select` 구성 요소에 전달됩니다.
 

```xml
<Controller
  name="role"
  control={control}
  as={Select}
  options={selectOptions}
  defaultValue=""
  rules={registerOptions.role}
/>
```

아래의 역할 필드에 대한 전체 예제를 확인할 수 있습니다.
 

```js
import { useForm, Controller } from "react-hook-form";
import Select from "react-select";
// ...
const { register, handleSubmit, errors, control } = useForm({
  mode: "onBlur"
});

const selectOptions = [
  { value: "student", label: "Student" },
  { value: "developer", label: "Developer" },
  { value: "manager", label: "Manager" }
];

const registerOptions = {
  // ...
  role: { required: "Role is required" }
};

// ...

<FormGroup>
  <Label>Your Role</Label>
  <Controller
    name="role"
    control={control}
    as={Select}
    options={selectOptions}
    defaultValue=""
    rules={registerOptions.role}
  />
  <small className="text-danger">
    {errors.role && errors.role.message}
  </small>
</FormGroup>
```

여기에서 `Controller`구성 요소에 대한 API 참조를 통해 자세한 설명을 확인할 수 있습니다.
 

## 결론
 

React Hook Form은 React 오픈 소스 생태계에 추가 된 훌륭한 기능입니다.
 개발자가 양식을 훨씬 쉽게 만들고 유지 관리 할 수 있습니다.
 이 라이브러리의 가장 좋은 점은 개발자 경험에 더 초점을 맞추고 작업하기에 매우 유연하다는 것입니다.
 React Hook Form은 상태 관리 라이브러리와 잘 통합되며 React Native에서 훌륭하게 작동합니다.
 

이것이이 가이드의 내용입니다.
 여기서 참조 할 수있는 전체 코드와 데모를 확인할 수 있습니다.
 다음 번까지 안전을 유지하고 더 많은 양식을 작성하십시오.
 건배 ✌
 

### 참고 문헌
 

React Hook Form 문서
 