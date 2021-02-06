---
layout: post
title: "예제를 포함한 각도 단위 테스트 튜토리얼"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2018/10/angular-unit-testing-tutorial-examples.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2018/10/angular-unit-testing-tutorial-examples.png?fit=730%2C487&ssl=1)

편집자 참고: 이 튜토리얼은 2020년 12월 31일에 마지막으로 업데이트되었습니다.

이 Angular unit test tutorial에서는 간단한 Angular app을 만든 다음 예제와 함께 유닛 테스트 프로세스를 단계별로 살펴보는 방법에 대해 설명합니다.

다음 사항에 대해 자세히 설명합니다.

- Angular testing이란?
- Angular unit test란?
- Angular(각각) 앱을 단위 테스트해야 하는 이유
- Angular test는 어떻게 쓰나요?
- 앵글의 카르마란 무엇인가?
- Angular(각도)에서 단위 검정을 작성하는 방법
- 각도 서비스를 테스트하는 방법
- 각도 구성 요소를 테스트하는 방법
- Angular(각도)에서 비동기 작업을 테스트하는 방법

이 튜토리얼을 따라가려면 Angular(각각) 사용 방법을 기본적으로 이해해야 합니다.

## Angular testing이란?

각도 테스트는 Angular CLI로 설정된 모든 프로젝트에서 사용할 수 있는 핵심 기능입니다.

JavaScript 에코시스템과 동기화를 유지하기 위해 Angular 팀은 매년 두 가지 주요 Angular 버전을 출시할 것을 주장한다. 가장 최근 릴리스인 Angular 11, Angular는 테스트 가능성을 염두에 두고 설계되었습니다.

각도 테스트에는 두 가지 유형이 있습니다.

- 단위 테스트는 분리된 작은 코드 조각을 테스트하는 프로세스입니다. 분리 테스트라고도 하는 장치 테스트에서는 네트워크나 데이터베이스와 같은 외부 리소스를 사용하지 않습니다.
- 기능 테스트란 사용자 경험 관점에서 Angular(각각) 앱의 기능 및 기능을 테스트하는 것을 말합니다. 즉, 앱이 브라우저에서 실행되는 것처럼 앱과 상호 작용합니다.

## Angular unit test란?

Angular(각도의 단위)의 단위 시험은 개별 단위의 코드를 시험하는 과정을 말한다.

Angular unit test는 코드 조각을 분리하여 잘못된 논리, 잘못된 동작 기능 등과 같은 문제를 발견하는 것을 목표로 한다. 특히 우려의 분리가 잘 되지 않는 복잡한 프로젝트의 경우 이는 말처럼 어려운 경우가 있습니다. Angular는 앱의 기능을 개별적으로 테스트할 수 있는 방식으로 코드를 작성할 수 있도록 설계되었습니다.

## Angular(각각) 앱을 단위 테스트해야 하는 이유

각도 단위 테스트를 통해 사용자 동작을 기준으로 앱을 테스트할 수 있습니다. 가능한 각 동작을 테스트하는 것은 지루하고 비효율적이며 비효율적일 수 있지만 응용 프로그램의 각 커플링 블록에 대한 쓰기 테스트는 이러한 블록이 어떻게 동작하는지 입증하는 데 도움이 될 수 있습니다.

이러한 블럭의 장점을 테스트하는 가장 쉬운 방법 중 하나는 각 블럭에 대한 검정을 작성하는 것입니다. 단추를 누를 때 입력 필드가 어떻게 동작하는지 사용자가 불평할 때까지 기다릴 필요는 없습니다. 블록(구성 요소, 서비스 등)에 대한 단위 테스트를 작성하면 중단 시간을 쉽게 감지할 수 있습니다.

예제 Angular 앱에는 서버에서 가져오는 데이터를 시뮬레이션하는 서비스, 구성 요소 및 비동기 작업이 있습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2018/10/angular-unit-testing-example-app.gif?resize=720%2C375&ssl=1)

## Angular test는 어떻게 쓰나요?

Angular CLI(ng new appName)로 새 프로젝트를 생성하면 기본 구성 요소와 테스트 파일이 추가됩니다. 또한 저처럼 항상 바로 가기를 찾는 경우 Angular CLI를 사용하여 생성하는 모든 구성 요소 모듈(서비스, 구성 요소)과 함께 테스트 스크립트가 생성됩니다.

.spec.ts로 끝나는 이 테스트 스크립트는 항상 추가됩니다. 초기 테스트 스크립트 파일인 `app.component.specs.ts`를 살펴보겠습니다.

```js
import { TestBed, async } from '@angular/core/testing';
import { AppComponent } from './app.component';
describe('AppComponent', () => {
  beforeEach(async(() => {
    TestBed.configureTestingModule({
      declarations: [
        AppComponent
      ],
    }).compileComponents();
  }));
  it('should create the app', async(() => {
    const fixture = TestBed.createComponent(AppComponent);
    const app = fixture.debugElement.componentInstance;
    expect(app).toBeTruthy();
  }));
  it(`should have as title 'angular-unit-test'`, async(() => {
    const fixture = TestBed.createComponent(AppComponent);
    const app = fixture.debugElement.componentInstance;
    expect(app.title).toEqual('angular-unit-test');
  }));
  it('should render title in a h1 tag', async(() => {
    const fixture = TestBed.createComponent(AppComponent);
    fixture.detectChanges();
    const compiled = fixture.debugElement.nativeElement;
    expect(compiled.querySelector('h1').textContent).toContain('Welcome to angular-unit-test!');
  }));
});
```

첫 번째 테스트를 실행하여 고장난 것이 없는지 확인합니다.

```bash
ng test
```

프로젝트가 브라우저에서 렌더링되는 경우에도 테스트만 작성하면 어떻게 사용자 동작을 시뮬레이션할 수 있는지 궁금하실 수 있습니다. 진행하면서 브라우저에서 실행되는 Angular 앱을 시뮬레이션하는 방법을 보여드리겠습니다.

## 앵글의 카르마란 무엇인가?

카르마(Karma)는 Angular에서 단위 테스트 스니펫을 실행하는 자바스크립트 테스트 러너이다. 카르마는 또한 테스트 결과가 콘솔이나 파일 로그에서 출력되도록 보장합니다.

기본적으로 Angular는 카르마를 기반으로 실행됩니다. 다른 시험 주자들로는 모카와 재스민이 있다. 카르마는 Angular에서 코드를 쓰면서 재스민 테스트를 쉽게 호출할 수 있는 도구를 제공합니다.

## Angular(각도)에서 단위 검정을 작성하는 방법

Angular 테스트 패키지에는 TestBed와 sync라는 두 가지 유틸리티가 포함되어 있습니다. 테스트베드는 Angular 유틸리티의 주요 패키지입니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2018/10/angular-unit-testing-example-flow.jpeg?resize=461%2C383&ssl=1)

description(설명) 컨테이너에는 서로 다른 블록(그 블록, before each, exit 등)이 포함되어 있습니다. `각각`은 다른 블록보다 먼저 실행됩니다. 다른 블록은 서로에 따라 실행되지 않습니다.

첫 번째 블록은 app.component.spects 파일에서 컨테이너 내부의 before each(설명)입니다. 이 블록이 다른 블록보다 먼저 실행되는 유일한 블록입니다(`it`). 앱.module.ts 파일의 앱 모듈 선언은 beforeEach 블록에서 시뮬레이션(선언)됩니다. BeforeEach 블록에 선언된 구성 요소(AppComponent)는 이 테스트 환경에서 우리가 원하는 주요 구성 요소입니다. 동일한 논리가 다른 시험 선언에도 적용된다.

컴파일 구성 요소 개체를 호출하여 템플릿, 스타일 등의 구성 요소 리소스를 컴파일합니다. 웹 팩을 사용하는 경우 구성 요소를 컴파일하지 않아도 됩니다.

```coffeescript
beforeEach(async(() => {
   TestBed.configureTestingModule({
      declarations: [
         AppComponent
      ],
   }).compileComponents();
}));
```

이제 구성 요소가 `before each` 블록에 선언되었으므로 구성 요소가 생성되었는지 확인해 보겠습니다.

fixture.debugElement.componentInstance는 클래스의 인스턴스를 만듭니다(`AppComponent`). 클래스의 인스턴스가 `to Be`를 사용하는지 여부를 테스트합니다.참말:

```js
it('should create the app', async(() => {
    const fixture = TestBed.createComponent(AppComponent);
    const app = fixture.debugElement.componentInstance;
    expect(app).toBeTruthy();
}));
```

세 번째 블록은 생성된 구성 요소(`AppComponent`)의 속성에 액세스할 수 있는 방법을 보여줍니다. 기본적으로 추가된 유일한 속성은 제목입니다. 설정한 제목이 구성 요소(`AppComponent`)의 생성 인스턴스에서 변경되었는지 쉽게 확인할 수 있습니다.

```coffeescript
it(`should have as title 'angular-unit-test'`, async(() => {
     const fixture = TestBed.createComponent(AppComponent);
     const app = fixture.debugElement.componentInstance;
     expect(app.title).toEqual('angular-unit-test');
}));
```

네 번째 블록은 브라우저 환경에서 테스트가 작동하는 방식을 보여줍니다. 구성 요소를 생성한 후 브라우저 환경에서 실행을 시뮬레이션하기 위해 생성된 구성 요소의 인스턴스(`detectChanges`)가 호출됩니다. 구성 요소가 렌더링되었으므로 렌더링된 구성 요소의 `nativeElement` 개체(`fixture.debugElement.nativeElement`)에 액세스하여 하위 요소에 액세스할 수 있습니다.

```js
it('should render title in a h1 tag', async(() => {
   const fixture = TestBed.createComponent(AppComponent);
   fixture.detectChanges();
   const compiled = fixture.debugElement.nativeElement;
 expect(compiled.querySelector('h1').textContent).toContain('Welcome to angular-unit-test!');
}));
```

이제 구성요소 테스트의 기본 사항을 숙지하셨으므로 Angular(각도) 예제 응용 프로그램을 테스트해 보겠습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2018/10/angular-unit-testing-example-app-overview.jpeg?resize=299%2C472&ssl=1)

## 각도 서비스를 테스트하는 방법

서비스는 종종 Angular가 생성자에게 주입하는 다른 서비스에 의존합니다. 많은 경우 `제공됨`을 추가하여 이러한 종속성을 생성하고 주입하기 쉽다.In: 모든 구성 요소 또는 서비스에서 액세스할 수 있도록 하는 주입 가능한 개체에 대한 루트:

```js
import { Injectable } from "@angular/core";
import { QuoteModel } from "../model/QuoteModel";

@Injectable({
  providedIn: "root"
})
export class QuoteService {
  public quoteList: QuoteModel[] = [];

  private daysOfTheWeeks = ["Sun", "Mon", "Tue", "Wed", "Thurs", "Fri", "Sat"];

  constructor() {}

  addNewQuote(quote: String) {
    const date = new Date();
    const dayOfTheWeek = this.daysOfTheWeeks[date.getDate()];
    const day = date.getDay();
    const year = date.getFullYear();
    this.quoteList.push(
      new QuoteModel(quote, `${dayOfTheWeek} ${day}, ${year}`)
    );
  }

  getQuote() {
    return this.quoteList;
  }

  removeQuote(index) {
    this.quoteList.splice(index, 1);
  }
}
```

다음은 견적 서비스 클래스를 테스트하는 몇 가지 방법입니다.

```js
/* tslint:disable:no-unused-variable */
import { QuoteService } from "./Quote.service";

describe("QuoteService", () => {
  let service: QuoteService;

  beforeEach(() => {
    service = new QuoteService();
  });

  it("should create a post in an array", () => {
    const qouteText = "This is my first post";
    service.addNewQuote(qouteText);
    expect(service.quoteList.length).toBeGreaterThanOrEqual(1);
  });

  it("should remove a created post from the array of posts", () => {
    service.addNewQuote("This is my first post");
    service.removeQuote(0);
    expect(service.quoteList.length).toBeLessThan(1);
  });
});
```

첫 번째 블록인 `beforeEach`에서는 `QuoteService` 인스턴스가 생성되어 한 번만 생성되도록 하고 일부 예외적인 경우를 제외하고 다른 블록에서 반복되지 않도록 합니다.

```undefined
it("should create a post in an array", () => {
    const qouteText = "This is my first post";
    service.addNewQuote(qouteText);
    expect(service.quoteList.length).toBeGreaterThanOrEqual(1);
  });
```

첫 번째 블록은 포스트 모델인 `따옴표 모델(텍스트, 날짜)`이 배열 길이를 확인하여 배열로 생성되는지 테스트합니다. 견적 리스트의 길이는 1이 될 것으로 예상된다.

```coffeescript
it("should remove a created post from the array of posts", () => {
    service.addNewQuote("This is my first post");
    service.removeQuote(0);
    expect(service.quoteList.length).toBeLessThan(1);
  });
```

두 번째 블록은 배열에 게시물을 작성하고 서비스 객체에 있는 `제거 견적`을 호출하여 즉시 제거합니다. 인용목록의 길이는 0으로 예상된다.

## 각도 구성 요소를 테스트하는 방법

Angular unit testing 예제 앱에서 `service`를 `Quote Component`에 주입하여 해당 속성에 액세스하고, 이는 다음과 같은 관점에서 필요할 것입니다.

```js
import { Component, OnInit } from '@angular/core';
import { QuoteService } from '../service/Quote.service';
import { QuoteModel } from '../model/QuoteModel';

@Component({
  selector: 'app-Quotes',
  templateUrl: './Quotes.component.html',
  styleUrls: ['./Quotes.component.css']
})
export class QuotesComponent implements OnInit {

  public quoteList: QuoteModel[];
  public quoteText: String = null;

  constructor(private service: QuoteService) { }

  ngOnInit() {
    this.quoteList = this.service.getQuote();
  }

  createNewQuote() {
    this.service.addNewQuote(this.quoteText);
    this.quoteText = null;
  }

  removeQuote(index) {
    this.service.removeQuote(index);
  }
}
```

```xml
<div class="container-fluid">
  <div class="row">
    <div class="col-8 col-sm-8 mb-3 offset-2">
      <div class="card">
        <div class="card-header">
          <h5>What Quote is on your mind ?</h5>
        </div>
        <div class="card-body">
          <div role="form">
            <div class="form-group col-8 offset-2">
              <textarea #quote class="form-control" rows="3" cols="8" [(ngModel)]="quoteText" name="quoteText"></textarea>
            </div>
            <div class="form-group text-center">
              <button class="btn btn-primary" (click)="createNewQuote()" [disabled]="quoteText == null">Create a new
                quote</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div class="row">
    <div class="card mb-3 col-5 list-card" id="quote-cards" style="max-width: 18rem;" *ngFor="let quote of quoteList; let i = index"
      (click)="removeQuote(i)">
      <div class="card-body">
        <h6>{ quote.text }</h6>
      </div>
      <div class="card-footer text-muted">
        <small>Created on { quote.timeCreated }</small>
      </div>
    </div>
  </div>
</div>
```

설명 컨테이너의 처음 두 블록은 연속적으로 실행됩니다. 첫 번째 블록에서 `양식 모듈`을 구성 테스트로 가져옵니다. 이를 통해 ng모델 등 양식 관련 지시문을 사용할 수 있다.

또한 견적 구성 요소는 appModule 파일에 있는 ngModule에서 선언하는 방법과 유사하게 configTestMod에서 선언된다. 두 번째 블록은 인용 구성 요소와 해당 인스턴스(instance)를 생성하며, 다른 블록은 이를 사용합니다.

```coffeescript
let component: QuotesComponent;
  let fixture: ComponentFixture<QuotesComponent>;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [FormsModule],
      declarations: [QuotesComponent]
    });
  });

  beforeEach(() => {
    fixture = TestBed.createComponent(QuotesComponent);
    component = fixture.debugElement.componentInstance;
  });
```

이 블록은 생성되는 구성 요소의 인스턴스가 정의되었는지 테스트합니다.

```coffeescript
it("should create Quote component", () => {
    expect(component).toBeTruthy();
  });
```

주입된 서비스는 모든 작업의 조작(추가, 제거, 가져오기)을 처리합니다. QuoteService 변수에는 주입된 서비스(QuoteService)가 포함됩니다. 이 시점에서 구성 요소는 `detectChanges` 메서드가 호출될 때까지 렌더링되지 않습니다.

```coffeescript
it("should use the quoteList from the service", () => {
    const quoteService = fixture.debugElement.injector.get(QuoteService);
    fixture.detectChanges();
    expect(quoteService.getQuote()).toEqual(component.quoteList);
  });
```

이제 게시물을 성공적으로 만들 수 있는지 테스트해 보겠습니다. 인스턴스화 시 구성 요소의 속성에 액세스할 수 있으므로 렌더링된 구성 요소는 값이 따옴표로 전달될 때 새로운 변경 사항을 감지합니다.텍스트 모델. nativeElement 개체는 렌더링된 HTML 요소에 대한 액세스를 제공하므로 추가된 인용문이 다음과 같이 렌더링된 텍스트의 일부인지 쉽게 확인할 수 있습니다.

```coffeescript
it("should create a new post", () => {
    component.quoteText = "I love this test";
    fixture.detectChanges();
    const compiled = fixture.debugElement.nativeElement;
    expect(compiled.innerHTML).toContain("I love this test");
  });
```

HTML 컨텐츠에 접근할 수 있는 것 외에, 당신은 또한 그것의 CSS 속성을 통해 요소를 얻을 수 있다. 인용할 때텍스트 모델이 비어 있거나 null이므로 버튼이 비활성화되어야 합니다.

```coffeescript
it("should disable the button when textArea is empty", () => {
    fixture.detectChanges();
    const button = fixture.debugElement.query(By.css("button"));
    expect(button.nativeElement.disabled).toBeTruthy();
  });
```

```coffeescript
it("should enable button when textArea is not empty", () => {
    component.quoteText = "I love this test";
    fixture.detectChanges();
    const button = fixture.debugElement.query(By.css("button"));
    expect(button.nativeElement.disabled).toBeFalsy();
  });
```

우리가 CSS 속성을 가지고 요소에 접근하는 방법과 마찬가지로, 우리는 클래스 이름으로 요소에 접근할 수도 있다. `예: By.css(.className.className`)`를 사용하여 여러 클래스에 동시에 액세스할 수 있습니다.

버튼 클릭은 `트리거 이벤트 핸들러`를 호출하여 시뮬레이션됩니다. 이 경우 클릭되는 `이벤트` 유형을 지정해야 합니다. 다음을 클릭하면 표시된 따옴표가 `따옴표 목록`에서 삭제됩니다.

```coffeescript
it("should remove post upon card click", () => {
    component.quoteText = "This is a fresh post";
    fixture.detectChanges();

    fixture.debugElement
      .query(By.css(".row"))
      .query(By.css(".card"))
      .triggerEventHandler("click", null);
    const compiled = fixture.debugElement.nativeElement;
    expect(compiled.innerHTML).toContain("This is a fresh post");
  });
```

## Angular(각도)에서 비동기 작업을 테스트하는 방법

결국 데이터를 원격으로 가져와야 합니다. 이 작업은 비동기 작업으로 가장 적합합니다.

`fetchQoutesFromServer`는 2초 후에 따옴표 배열을 반환하는 비동기 작업을 나타냅니다.

```coffeescript
fetchQuotesFromServer() {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve([new QuoteModel("I love unit testing", "Mon 4, 2018")]);
      }, 2000);
    });
  }
```

`spyOn` 개체는 `FetchQuotesFromServer` 메서드의 작동 방식을 시뮬레이션합니다. 구성 요소에 주입되는 QuoteService 인수와 FetchQuotesFromServer 메소드 두 가지를 수용한다. 서버로부터 인용을 가져오면 약속이 반환됩니다. 스파이온(spyOn)은 앤(and)과 가짜 약속 콜(reetValue)을 연동시켜 반환되는 방식이다. 서버로부터 인용문 가져오기 기능을 본받고자 하는 만큼 인용문 목록을 가지고 해결할 약속을 넘길 필요가 있다.

이전과 마찬가지로 `변경사항 탐지` 방법을 호출하여 업데이트된 변경 사항을 가져올 것입니다. whenStable을 사용하면 모든 sync 작업이 완료되면 다음과 같은 결과에 액세스할 수 있습니다.

```js
it("should fetch data asynchronously", async () => {
    const fakedFetchedList = [
      new QuoteModel("I love unit testing", "Mon 4, 2018")
    ];
    const quoteService = fixture.debugElement.injector.get(QuoteService);
    let spy = spyOn(quoteService, "fetchQuotesFromServer").and.returnValue(
      Promise.resolve(fakedFetchedList)
    );
    fixture.detectChanges();
    fixture.whenStable().then(() => {
      expect(component.fetchedList).toBe(fakedFetchedList);
    });
  });
```

## 결론

Angular(각도)를 사용하면 테스트 결과를 브라우저에서 볼 수 있습니다. 이렇게 하면 테스트 결과를 더 잘 시각화할 수 있습니다.

프로젝트의 소스 코드는 GitHub에서 사용할 수 있습니다.