---
layout: post
title: "Form Group 및 Form Control in Angular"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2019/09/manage-reactive-form-controls-angular.jpeg"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2019/09/manage-reactive-form-controls-angular.jpeg?fit=730%2C486&ssl=1)

JavaScript 프레임워크에서 작업할 때는 양식 논리가 구성요소 클래스에 있으므로 클러스터된 템플리트를 사용하지 않는 것이 가장 좋습니다. Angular(각도)의 반응형 양식을 사용하면 지시어를 너무 많이 사용하지 않고도 깨끗한 양식을 만들 수 있습니다. 또한 양식을 쉽게 검증할 수 있기 때문에 전체적인 테스트의 필요성을 줄여줍니다.

간단히 말해서, Angular의 양식 제어는 개발자에게 모든 제어권을 주고, 더 이상 암시적인 것은 없다. 입력과 제어에 대한 모든 선택은 의도적으로 그리고 물론 명시적으로 이루어져야 한다.

이 튜토리얼에서는 양식 그룹별로 양식 컨트롤을 분할하여 템플릿 요소를 그룹으로 쉽게 액세스할 수 있는 플랫폼을 제공하는 클러스터를 생성하는 방법을 설명합니다. 다음 사항에 대해 자세히 설명합니다.

- Angular(각도)에서 폼 컨트롤이란?
- Angular(각도)의 양식 그룹은 무엇입니까?
- Angular의 Form Control과 Form Group
- Angular(각도)에 양식 그룹 등록
- 각도에서 양식 그룹 중첩
- 양식 그룹에 양식 컨트롤을 추가하는 방법
- `폼 그룹` 값을 설정하는 방법
- Angular의 "Form Builder"는 무엇입니까?

Angular(각각)에서 양식 그룹의 개념을 설명하기 위해, 양식 그룹을 사용하여 이를 설정하는 방법을 완전히 이해할 수 있도록 반응형 양식을 작성하는 프로세스를 살펴보겠습니다.

계속하려면 GitHub에서 Starter 프로젝트를 다운로드하여 VS Code에서 여십시오. 아직 업데이트하지 않았다면 최신 버전인 Angular 11로 업데이트해야 합니다.

## Angular(각도)에서 폼 컨트롤이란?

Angular(각도)에서 양식 컨트롤은 모든 양식 요소의 데이터 값과 유효성 검사 정보를 모두 저장할 수 있는 클래스입니다. 반응형 형식의 모든 양식 입력은 양식 컨트롤에 의해 바인딩되어야 합니다. 이것들은 반응형태를 구성하는 기본 단위입니다.

## Angular(각도)의 양식 그룹은 무엇입니까?

양식 그룹은 양식 컨트롤 컬렉션을 줄바꿈합니다. 컨트롤이 요소의 상태에 대한 액세스를 제공하는 것과 마찬가지로 그룹도 동일한 액세스 권한을 부여하지만 압축된 컨트롤의 상태에 대한 액세스 권한을 부여합니다. 양식 그룹의 모든 양식 컨트롤은 초기화할 때 이름으로 식별됩니다.

## Angular의 Form Control과 Form Group

FormControl은 Angular(각각)의 클래스로 개별 Form Control의 값과 유효성 상태를 추적합니다. Angular 형태의 3가지 필수 구성 요소인 FormGroup, FormArray와 함께 FormControl은 가치, 검증 상태, 사용자 상호 작용 및 이벤트에 액세스할 수 있는 추상 제어 클래스를 확장합니다.

FormGroup은 FormControl과 함께 값을 추적하고 양식 컨트롤의 상태를 검증하는 데 사용됩니다. 실제로 `폼 그룹`은 각 컨트롤 이름을 키로 사용하여 각 자식 `폼 컨트롤`의 값을 단일 객체로 집계한다. 하위 컨트롤의 상태 값을 줄여서 상태를 계산하여 한 그룹의 컨트롤이 잘못되면 전체 그룹이 유효하지 않게 렌더링됩니다.

## Angular(각도)에 양식 그룹 등록

첫 번째 단계는 Angular(각도)에게 양식 그룹을 적절한 구성 요소 내에서 가져와 사용할 것임을 알리는 것입니다.

작동 방식을 보려면 `employee.component.ts` 파일로 이동하여 아래 코드 블록에 붙여넣으십시오.

```js
import { Component, OnInit } from '@angular/core';
import { FormControl, FormGroup } from '@angular/forms'
@Component({
  selector: 'app-employee',
  templateUrl: './employee.component.html',
  styleUrls: ['./employee.component.css']
})
export class EmployeeComponent implements OnInit {
  bioSection = new FormGroup({
    firstName: new FormControl(''),
    lastName: new FormControl(''),
    age: new FormControl('')
  });
constructor() { }
ngOnInit() {
  }
}
```

여기서 폼 그룹을 가져오고 초기화하여 폼의 바이오 섹션을 구성하는 몇 가지 폼 컨트롤을 그룹화합니다. 이 그룹을 반영하려면 다음과 같이 모델을 양식 그룹 이름과 연결해야 합니다.

```xml
// copy inside the employee.component.html file
<form [formGroup]="bioSection" (ngSubmit)="callingFunction()">

  <label>
    First Name:
    <input type="text" formControlName="firstName">
  </label>
<label>
    Last Name:
    <input type="text" formControlName="lastName">
  </label>
<label>
    Age:
    <input type="text" formControlName="age">
  </label>
<button type="submit">Submit Application</button>
</form>
```

양식 컨트롤과 마찬가지로 양식 그룹 이름은 보기에서 양식 그룹을 식별하는 데 사용되며, 제출 시 호출 기능이 트리거됩니다.

`app.component.html` 파일은 다음과 같아야 합니다.

```js
<div style="text-align:center">
  <h2>Angular Job Board </h2>
  <app-employee></app-employee>
</div>
```

이제 다음 명령을 사용하여 개발 중인 응용 프로그램을 실행하십시오.

```undefined
ng serve
```

다음과 같이 보입니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2019/08/angular-job-board-form.png?resize=730%2C208&ssl=1)

```undefined

```

## 각도에서 양식 그룹 중첩

Angular reactive forms API를 통해 양식 그룹을 다른 양식 그룹 내에 중첩할 수 있습니다.

아래의 코드 블록을 `employee.component.ts` 파일에 복사합니다.

```js
import { Component, OnInit } from '@angular/core';
import { FormControl, FormGroup } from '@angular/forms'
@Component({
  selector: 'app-employee',
  templateUrl: './employee.component.html',
  styleUrls: ['./employee.component.css']
})
export class EmployeeComponent implements OnInit {
  bioSection = new FormGroup({
    firstName: new FormControl(''),
    lastName: new FormControl(''),
    age: new FormControl(''),
    stackDetails: new FormGroup({
      stack: new FormControl(''),
      experience: new FormControl('')
    }),
    address: new FormGroup({
        country: new FormControl(''),
        city: new FormControl('')
    })
  });
constructor() { }
ngOnInit() {
  }
  callingFunction() {
    console.log(this.bioSection.value);
   }
}
```

기본 양식 그룹 래퍼에는 스택 세부 정보 그룹과 주소 그룹이 모두 중첩된 바이오 섹션이 있습니다. 보시는 것처럼 중첩 양식 그룹은 할당 문에 의해 정의되지 않고 양식 컨트롤처럼 콜론으로 정의됩니다.

이를 보기에 반영하면 다음과 같이 나타납니다.

```xml
// copy inside the employee.component.html file
<form [formGroup]="bioSection" (ngSubmit)="callingFunction()">
    <h3>Bio Details
</h3>

  <label>
    First Name:
    <input type="text" formControlName="firstName">
  </label> <br>
<label>
    Last Name:
    <input type="text" formControlName="lastName">
  </label> <br>
<label>
    Age:
    <input type="text" formControlName="age">
  </label>
<div formGroupName="stackDetails">
    <h3>Stack Details</h3>

    <label>
      Stack:
      <input type="text" formControlName="stack">
    </label> <br>

    <label>
      Experience:
      <input type="text" formControlName="experience">
    </label>
  </div>
<div formGroupName="address">
    <h3>Address</h3>

    <label>
      Country:
      <input type="text" formControlName="country">
    </label> <br>

    <label>
      City:
      <input type="text" formControlName="city">
    </label>
  </div>
<button type="submit">Submit Application</button>
</form>
```

모델과 뷰의 모든 이름이 일치해야 하므로, 양식 컨트롤 이름의 철자를 틀리지 않도록 하십시오. 응용 프로그램을 저장하고 실행할 때 오류가 발생하면 오류 메시지를 읽고 사용해야 하는 오타를 수정하십시오.

아래 스타일 지침에 따라 구성 요소를 스타일링할 수 있습니다.

```css
input[type=text] {
    width: 30%;
    padding: 8px 14px;
    margin: 2px;
    box-sizing: border-box;
  }
  button {
      font-size: 12px;
      margin: 2px;
      padding: 8px 14px;
  }
```

응용 프로그램을 실행하면 다음과 같은 내용이 브라우저에 표시됩니다.

양식을 사용하고 제출하면 브라우저 콘솔에서 입력 결과가 반환되는 것을 볼 수 있습니다. 이 튜토리얼에 사용된 전체 코드는 GitHub에서 사용할 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2019/08/angular-job-board-form-bio-e1567003921989.png?resize=730%2C456&ssl=1)

## 양식 그룹에 양식 컨트롤을 추가하는 방법

`FormGroup`에서 컨트롤을 추가, 업데이트 또는 제거하려면 다음 명령을 사용하십시오.

- `AddControl()`은 컨트롤을 추가하고 해당 값과 유효성을 업데이트합니다.
- 컨트롤 제거() 컨트롤 제거
- `설정 컨트롤()`이 기존 컨트롤을 대체합니다.
- 지정된 이름과 연관된 사용 가능한 컨트롤에 대해 `확인`
- registerControl()은 컨트롤을 등록하지만 다른 방법과 달리 값 및 유효성을 업데이트하지 않습니다.

## '폼 그룹' 값을 설정하는 방법

Angular(각도)에서 값을 개별 폼 그룹으로 설정하거나 모든 `폼 그룹` 값을 동시에 설정할 수 있습니다.

patchValue를 사용하여 일부 값만 설정합니다.

```cpp
this.myFormGroup.patchValue({
  formControlName1: myValue1, 
  // formControlName2: myValue2
});
```

여기에 모든 값을 제공할 필요는 없습니다. 값이 설정되지 않은 필드는 영향을 받지 않습니다.

모든 `양식 그룹` 값을 동시에 설정하려면 `setValue`를 사용하십시오.

```coffeescript
this.myFormGroup.setValue({
  formControlName1: myValue1, 
  formControlName2: myValue2
});
```

## Angular의 "Form Builder"는 무엇입니까?

특히 매우 긴 양식을 사용하는 경우 양식 컨트롤 설정이 지루할 수 있습니다. Angular의 `Form Builder`는 반복을 피하면서 고급 양식을 작성하는 과정을 간소화하도록 도와줍니다.

간단히 말해 폼빌더(FormBuilder)는 폼컨트롤(Form Control), 폼그룹(FormGroup) 또는 폼어레이(FormArray)의 인스턴스 생성 부담을 덜어주고 복잡한 폼을 만드는 데 필요한 보일러판의 양을 줄여주는 통사당을 제공한다.

복잡한 양식을 작성하는 방법에 대한 자세한 내용과 예는 당사의 종합적인 Angular 양식 작성기 자습서를 참조하십시오.

## 결론

이 튜토리얼에서는 `폼 컨트롤`을 사용하는 방법, 폼 컨트롤을 `폼 그룹`으로 그룹화하는 방법, 컨트롤의 집단 인스턴스를 한 번에 캡처하는 것이 왜 중요한지 등 Angular의 폼 컨트롤에 대해 알아야 할 모든 사항을 다뤘다.