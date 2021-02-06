---
layout: post
title: "FormBuilder를 통한 각도 반응형 양식 검증"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2019/09/form-builders-angular-8-validate-reactive-forms.jpeg
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2019/09/form-builders-angular-8-validate-reactive-forms.jpeg?fit=730%2C483&ssl=1)

편집자 참고: 이 게시물은 2020년 12월 23일에 업데이트되었습니다.

Angular(각도)의 반응형 양식을 사용하면 너무 많은 지시사항을 사용하지 않고도 깨끗한 양식을 작성할 수 있습니다. 이는 다음과 같은 이유로 중요합니다.

- JavaScript 프레임워크는 일반적으로 클러스터된 템플릿을 사용하지 않도록 주의합니다.
- 이제 양식 논리가 구성 요소 클래스에 있습니다.

기본적으로, 각 반응형 양식은 입력 및 제어와 관련된 모든 결정이 의도적이고 명시적이어야 하기 때문에 개발자에게 더 많은 통제력을 제공한다.

데이터의 품질을 보장하기 위해서는 항상 정확성과 완전성을 위해 반응형 폼 입력을 검증하는 것이 좋습니다. 이 튜토리얼에서는 `FormBuilder`를 사용하여 Angular(각각)에서 무효화된 양식의 유효성을 검사하는 방법을 설명합니다.

다음 사항에 대해 자세히 설명합니다.

- Angular(각도)에서 양식 컨트롤 및 양식 그룹
- Angular(각도)에서 양식 유효성 검사란?
- FormBuilder란?
- FormBuilder 사용 방법
- 양식 작성기를 등록하는 중
- Angular(각도)에서 양식 유효성 검사 방법
- 입력 값 및 상태 표시

계속하려면 Angular CLI(11.0.5)와 함께 최신 버전의 Node.js(15.5.0) 및 Angular(11.0.5)가 설치되어 있어야 합니다. 시작 프로젝트는 GitHub에서 다운로드할 수 있습니다.

Angular 양식 유효성 검사에 대한 간단한 시각적 개요를 보려면 아래의 비디오 튜토리얼을 참조하십시오.

## Angular(각도)에서 양식 컨트롤 및 양식 그룹

양식 컨트롤은 모든 양식 요소의 데이터 값과 유효성 검사 정보를 모두 포함할 수 있는 클래스입니다. 즉, 반응형 폼의 모든 양식 입력이 양식 컨트롤에 의해 바인딩되어야 합니다. 이것들은 반응형태를 구성하는 기본 단위입니다.

FormControl은 Angular(각각)의 클래스로 개별 Form Control의 값과 유효성 상태를 추적합니다. Angular 형태의 3가지 필수 구성 요소인 FormGroup, FormArray와 함께 FormControl은 가치, 검증 상태, 사용자 상호 작용 및 이벤트에 액세스할 수 있는 추상 제어 클래스를 확장합니다.

양식 그룹은 기본적으로 양식 컨트롤 컬렉션을 싸는 구성 요소입니다. 컨트롤이 요소의 상태에 대한 액세스를 제공하는 것처럼 그룹도 동일한 액세스 권한을 부여하지만, 닫힌 컨트롤의 상태에 대한 액세스 권한을 부여합니다. 양식 그룹의 모든 양식 컨트롤은 초기화할 때 이름으로 식별됩니다.

FormGroup은 FormControl과 함께 값을 추적하고 양식 컨트롤의 상태를 검증하는 데 사용됩니다. 실제로 `폼 그룹`은 각 컨트롤 이름을 키로 사용하여 각 자식 `폼 컨트롤`의 값을 단일 객체로 집계한다. 하위 컨트롤의 상태 값을 줄여서 상태를 계산하여 한 그룹의 컨트롤이 잘못되면 전체 그룹이 유효하지 않게 렌더링됩니다.

자세한 내용은 Angular(각각)에서 양식 그룹 및 양식 컨트롤에 대한 포괄적인 자습서를 참조하십시오.

## Angular(각도)에서 양식 유효성 검사란?

Angular(각도)의 양식 검증을 통해 입력이 정확하고 완전한지 확인할 수 있습니다. UI에서 사용자 입력을 검증하고 유용한 유효성 검사 메시지를 템플릿 기반 및 사후 대응 형식으로 모두 표시할 수 있습니다.

Angular(각도)에서 반응형식을 검증할 때, 검증자 함수는 구성요소 클래스의 양식 제어 모델에 직접 추가됩니다. 각도는 컨트롤 값이 변경될 때마다 이러한 함수를 호출합니다.

검증자 기능은 동기식 또는 비동기식일 수 있습니다.

- 동기식 검증자는 제어 인스턴스를 가져와서 오류 집합 또는 null을 반환합니다. FormControl을 생성할 때 동기화 기능을 두 번째 인수로 전달할 수 있습니다.
- 비동기 검증자는 제어 인스턴스를 가져와서 나중에 오류 집합 또는 null을 발급하는 약속 또는 관찰 가능을 반환합니다. `폼 컨트롤`을 인스턴스화할 때 비동기 함수를 세 번째 인수로 전달할 수 있습니다.

프로젝트의 고유한 요구 사항 및 목표에 따라 사용자 정의 검증자 함수를 작성하거나 Angular의 기본 제공 검증자를 사용할 수 있습니다.

더 이상 진행하지 않고 반응형 양식을 작성하도록 하겠습니다.

## FormBuilder란?

특히 매우 긴 형태의 양식 조정기를 설정하는 것은 단조롭기도 하고 스트레스도 받을 수 있다. Angular의 FormBuilder는 반복을 피하면서 복잡한 양식을 작성하는 프로세스를 능률적으로 수행할 수 있도록 도와줍니다.

간단히 말해 폼빌더(FormBuilder)는 폼컨트롤(Form Control), 폼그룹(FormGroup) 또는 폼어레이(FormArray)의 인스턴스 생성 부담을 덜어주고 복잡한 폼을 만드는 데 필요한 보일러판의 양을 줄여주는 통사당을 제공한다.

## FormBuilder 사용 방법

아래의 예는 FormBuilder를 사용하여 Angular에서 반응형 양식을 작성하는 방법을 보여줍니다.

VS Code에서 시작 프로젝트를 다운로드하여 열었어야 합니다. `employee.component.ts`를 열면 다음과 같은 파일이 생성됩니다.

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

모든 양식 컨트롤은 물론, 모든 양식 컨트롤이 분할되는 양식 그룹까지 철자가 지정되므로 시간이 지남에 따라 반복됩니다. Form Builder는 이러한 효율성 문제를 해결하는 데 도움이 됩니다.

### '양식 작성기' 등록 중

FormBuilder를 사용하려면 먼저 등록해야 합니다. 컴포넌트에 FormBuilder를 등록하려면 Angular 양식에서 가져옵니다.

```coffeescript
import { FormBuilder } from ‘@angular/forms’;
```

다음 단계는 사후 대응적 양식 모듈과 함께 제공되는 주입식 공급자인 양식 작성기 서비스를 주입하는 것입니다. 그런 다음 양식 작성기를 삽입한 후 사용할 수 있습니다. `employee.component.ts` 파일로 이동하여 아래 코드 블록에 복사하십시오.

```js
import { Component, OnInit } from '@angular/core';
import { FormBuilder } from '@angular/forms'
@Component({
  selector: 'app-employee',
  templateUrl: './employee.component.html',
  styleUrls: ['./employee.component.css']
})
export class EmployeeComponent implements OnInit {
  bioSection = this.fb.group({
    firstName: [''],
    lastName: [''],
    age: [''],
    stackDetails: this.fb.group({
      stack: [''],
      experience: ['']
    }),
    address: this.fb.group({
        country: [''],
        city: ['']
    })
  });
constructor(private fb: FormBuilder) { }
ngOnInit() {
  }
  callingFunction() {
    console.log(this.bioSection.value);
   }
}
```

이 작업은 시작할 때 보았던 이전 코드 블록과 정확히 동일하지만, 훨씬 적은 코드와 더 많은 구조를 가지고 있다는 것을 알 수 있습니다. 따라서 리소스를 최적으로, 양식 작성자는 사후 대응 양식의 코드를 효율적으로 만드는 데 도움이 될 뿐만 아니라 양식 유효성 검사에도 중요합니다.

## Angular(각도)에서 양식 유효성 검사 방법

Angular(각도)의 반응형 양식을 사용하여 양식 작성기 내부에서 양식을 검증할 수 있습니다.

다음 명령을 사용하여 개발 중인 응용 프로그램을 실행하십시오.

```undefined
ng serve
```

텍스트 상자에 값을 입력하지 않아도 양식이 제출된다는 것을 알게 됩니다. 이것은 반응형 형태의 양식 검증기로 쉽게 확인할 수 있습니다. 반응형식의 모든 요소와 마찬가지로 가장 먼저 해야 할 일은 각도형에서 가져오는 것입니다.

```undefined
import { Validators } from '@angular/forms';
```

이제 제출 단추를 활성화하려면 채워야 하는 양식 컨트롤을 지정하여 검증자를 사용할 수 있습니다. 아래의 코드 블록을 `employee.component.ts` 파일에 복사합니다.

마지막으로 제출 단추의 활성 설정이 적절하게 설정되어 있는지 확인하는 것입니다. `employee.component.html` 파일로 이동하여 제출 문이 다음과 같이 표시되는지 확인하십시오.

```xml
<button type=”submit” [disabled]=”!bioSection.valid”>Submit Application</button>
```

지금 응용 프로그램을 실행하면 이름에 대한 입력을 설정하지 않으면 양식을 제출할 수 없다는 것을 알 수 있습니다. 멋지지 않습니까?

### 입력 값 및 상태 표시

값 및 상태 속성을 사용하여 반응형 양식의 입력 값과 제출 가능 여부를 실시간으로 표시하려고 합니다.

사후 대응 양식 API를 사용하여 양식 그룹의 값 및 상태 속성을 사용하거나 템플릿 섹션의 양식 컨트롤을 사용할 수 있습니다. `employee.component.html` 파일을 열고 아래 코드 블록에 복사하십시오.

```xml
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
<button type="submit" [disabled]="!bioSection.valid">Submit Application</button>
  <p>
    Real-time data: { bioSection.value | json }
  </p>
  <p>
    Your form status is : { bioSection.status }
  </p>
</form>
```

양식을 사용할 때 인터페이스에 대한 제출의 값과 상태가 모두 표시됩니다. 이 튜토리얼의 전체 코드는 GitHub에서 찾을 수 있습니다.

## 결론

이 문서에서는 양식 작성기에 대한 개요와 양식 제어 및 양식 그룹을 위한 뛰어난 효율성 실현 방법에 대해 설명합니다. 또한 양식 유효성 검사를 사후 대응 양식을 사용하여 쉽게 처리하는 데 있어 얼마나 중요한지 보여줍니다. 해피 해킹!