---
layout: post
title: "읽을 수 있는 코드를 쓰기 위한 TypeScript 열거형과 Type"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2019/03/writing-readable-code-typescript-enums.jpeg
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2019/03/writing-readable-code-typescript-enums.jpeg?fit=1440%2C795&ssl=1)

TL;DR: 이 자료에서는 TypeScript 열거형 모범 사례를 살펴보고, TypeScript의 내용과 사용 방법을 검토합니다. 또한 샘플 코드를 사용하여 작성된 포인트를 설명하겠습니다.

TypeScript(이미 알고 계시겠지만)는 Microsoft의 팀에서 개발 및 유지관리하는 오픈 소스, 강력한 형식의 객체 지향 컴파일된 언어입니다. 정적 입력 옵션이 있는 JavaScript의 상위 집합입니다. 자바스크립트로 컴파일되는 확장 가능한 대규모 애플리케이션의 개발을 위해 설계되었다.

### 열거형이란?

C, C# 및 Java와 같은 대부분의 객체 지향 프로그래밍 언어에는 열거형이라고 알려진 데이터 유형이 있다. 자바 열거형(Java enum)은 상수의 집합을 정의하는 데 사용되는 특별한 종류의 자바 클래스이다. 그러나 Javascript에는 열거형 데이터 유형이 없지만 다행히도 버전 2.4 이후 TypeScript에서 사용할 수 있습니다. 열거형을 사용하면 숫자 또는 문자열이 될 수 있는 관련 값의 집합을 명명된 상수 집합으로 정의하거나 선언할 수 있습니다.

TypeScript에서 사용할 수 있는 일부 유형과 달리 열거형은 사전 처리되며 컴파일 시간 또는 런타임에 테스트되지 않습니다.

### 왜 열거하는가?

Enum은 TypeScript에서 코드를 구성하는 유용한 방법 중 하나입니다. 열거형이 유용한 몇 가지 이유는 다음과 같습니다.

- 열거형을 사용하면 쉽게 연관시킬 수 있는 상수를 만들어 상수를 보다 읽기 쉽게 만들 수 있습니다.
- TypeScript 열거형을 사용하면 개발자는 JavaScript에서 메모리 효율적인 사용자 지정 상수를 만들 수 있습니다. 아시다시피 JavaScript는 열거형을 지원하지 않지만, TypeScript는 이러한 열거형에 액세스하는 데 도움이 됩니다.
- 앞서 언급했듯이, TypeScript 열거형은 JavaScript에서 인라인 코드로 런타임과 컴파일 시간을 절약합니다(이 문서의 뒷부분에서 확인할 수 있습니다).
- 또한 TypeScript 열거형은 이전에 Java와 같은 언어에서만 사용되었던 특정 유연성을 제공합니다. 이러한 유연성 덕분에 우리의 의도와 사용 사례를 쉽게 표현하고 문서화할 수 있습니다.

### 열거 구문

Enum은 다음과 같이 Enum 키워드로 정의됩니다.

```undefined
enum Continents {
    North_America,
    South_America,
    Africa,
    Asia,
    Europe,
    Antartica,
    Australia
}

// usage
var region = Continents.Africa;
```

### TypeScript 열거형 유형

TypeScript 열거형에는 다음과 같은 세 가지 유형이 있습니다.

- 숫자 열거형
- 문자열 열거형
- 이종 열거형

### 숫자 열거형

기본적으로 TypeScript 열거형은 숫자 기반입니다. 즉, 문자열 값을 숫자로 저장할 수 있습니다. 숫자 및 번호와 호환되는 기타 유형을 열거형 인스턴스에 할당할 수 있습니다. 주말에 날짜를 저장하려고 합니다. TypeScript에서 나타내는 열거형은 다음과 같을 수 있습니다.

```cpp
enum Weekend {
  Friday,
  Saturday,
  Sunday
}
```

위의 코드 블록에는 우리가 `주말`이라고 부르는 열거형이 있습니다. 열거형에는 금요일 토요일 일요일 등 3가지 값이 있다. 다른 언어에서와 마찬가지로 TypeScript에서도 열거형 값은 0에서 시작하여 각 멤버에 대해 하나씩 증가합니다. 다음과 같이 저장됩니다.

```undefined
Friday = 0
Saturday = 1
Sunday = 2
```

열거형에는 항상 스토리지에 대한 번호가 할당되어 있지만, 이 값은 항상 0의 숫자 값을 사용하지만 고유한 논리로 스토리지 값을 사용자 지정할 수 있습니다.

#### 사용자 지정 숫자 열거형

TypeScript에서는 열거형의 첫 번째 숫자 값을 지시할 수 있습니다. 위의 주말 일 예제를 사용하여 다음과 같은 첫 번째 숫자 값을 초기화할 수 있습니다.

```undefined
enum Weekend {
  Friday = 1,
  Saturday,
  Sunday
}
```

위의 코드 블록은 금요일을 1로, 토요일을 2로, 일요일을 3으로 저장한다. 첫 번째 멤버에 숫자를 추가해도 나머지 멤버에 대해 1씩 순차적으로 증가됩니다. 그러나, 우리는 그들에게 어떤 수치적 가치를 줌으로써 순차적 추적을 원하지 않는다고 지시할 수 있는 힘을 가지고 있다. 아래의 코드 블록은 의미론적이며 TypeScript에서 작동합니다.

```undefined
enum Weekend {
  Friday = 1,
  Saturday = 13,
  Sunday = 5
}
```

TypeScript의 다른 데이터 유형과 마찬가지로 다음과 같이 열거형을 함수 매개 변수 또는 반환 유형으로 사용할 수 있습니다.

```undefined
enum Weekend {
  Friday = 1,
  Saturday,
  Sunday
}
function getDate(Day: string): Weekend {
    if ( Day === 'TGIF') {
        return Weekend.Friday;
    }
 }
let DayType: Weekend = getDate('TGIF');
```

위의 코드 블록에서 우리는 `주말` 열거형을 선언했다. 그런 다음 "주말" 열거형을 반환하는 입력 "데이"를 사용하는 "getDate" 함수를 선언했습니다. 함수에서 이제 열거형 멤버를 반환하는 일부 조건을 확인합니다.

### 문자열 열거형

지금까지 멤버 값이 숫자인 열거형만 살펴보았다. TypeScript에서는 열거형 멤버도 문자열 값이 될 수 있습니다. 문자열 열거형은 의미 있는 문자열 값 때문에 오류 기록 및 디버깅 시 가독성을 위해 필수적이고 다루기 쉽다.

```undefined
enum Weekend {
  Friday = 'FRIDAY',
  Saturday = 'SATURDAY',
  Sunday = 'SUNDAY'
}
```

그런 다음 다음과 같은 조건문의 문자열을 비교하는 데 사용할 수 있습니다.

```undefined
enum Weekend {
  Friday = 'FRIDAY',
  Saturday = 'SATURDAY',
  Sunday ='SUNDAY'
}
const value = someString as Weekend;
if (value === Weekend.Friday || value === Weekend.Sunday){
    console.log('You choose a weekend');
    console.log(value); 
}
```

위의 예에서 우리는 위의 숫자 열거형처럼 `Weekend`라는 문자열 열거형을 정의했지만 이번에는 열거형 값을 문자열로 사용한다. 숫자 열거형과 문자열 열거형의 분명한 차이점은 숫자 열거형은 대부분 자동으로 순차적으로 증가하는 반면 문자열 열거형은 증가하지 않고 각 값이 독립적으로 초기화된다는 것이다.

### 이종 열거형

또한 TypeScript는 이질적인 열거형 값이라고 하는 문자열과 숫자의 혼합을 허용합니다.

```undefined
enum Weekend {
  Friday = 'FRIDAY',
  Saturday = 1,
  Sunday = 2
}
```

비록 이것이 가능할지라도, 이 사용 사례를 필요로 할 가능성이 있는 시나리오의 범위는 매우 작다. 따라서 JavaScript의 런타임 동작을 현명하게 활용하려는 경우가 아니라면 이기종 열거형을 사용하지 않는 것이 좋습니다.

### 계산된 열거형

숫자 열거형의 값은 TypeScript의 다른 숫자 데이터 유형과 마찬가지로 일정하거나 평가할 수 있습니다. 계산된 값으로 숫자 열거형을 정의하거나 초기화할 수 있습니다.

```undefined
enum Weekend {
  Friday = 1,
  Saturday = getDate('TGIF'),
  Sunday = Saturday * 40
}

function getDate(day : string): number {
    if (day === 'TGIF') {
        return 3;
    }
}
Weekend.Saturday; // returns 3
Weekend.Sunday; // returns 120
```

규칙 #1 - 열거형이 계산 멤버와 상수 멤버의 혼합을 포함할 경우 초기화되지 않은 열거형 멤버는 우선이거나 숫자 상수가 있는 다른 초기화된 멤버 다음에 와야 합니다.

위에서 이 규칙을 무시하면 이니셜라이저 오류가 발생합니다. 이 오류가 표시되면 그에 따라 열거 멤버를 다시 정렬해야 합니다.

### 구성 요소

숫자 열거형의 성능을 높이려면 해당 열거형을 상수로 선언할 수 있습니다. 주말 예제를 사용하여 다음을 설명하겠습니다.

```undefined
enum Weekend {
  Friday = 1,
  Saturday,
  Sunday
}
var day = Weekend.Saturday;
```

자바스크립트로 컴파일하면 실행 시 런타임이 위켄드(Weekend)를 조회하고 위켄드(Weekend)를 조회한다.토요일. 런타임 성능을 최적화하려면 다음과 같이 열거형을 상수로 만들 수 있습니다.

```undefined
const enum Weekend {
  Friday = 1,
  Saturday,
  Sunday
}
var day = Weekend.Saturday;
```

따라서 상수로 컴파일할 때 생성된 JavaScript는 다음과 같습니다.

```undefined
var day = 2;
```

우리는 컴파일러가 열거형을 볼 때 어떻게 단지 열거형을 선형화하고 심지어 열거형 선언을 위해 자바스크립트를 생성하지 않는지 본다. 이 선택 사항 및 검색에서 문자열에 대한 숫자 또는 문자열이 필요한 사용 사례의 경우 결과를 숙지하는 것이 중요합니다. 컴파일러 플래그인 `preserveConstellationEnums`도 전달할 수 있으며, 여전히 `Weekend` 정의를 생성합니다.

### 역방향 매핑

TypeScript enum은 역방향 매핑을 지원하는데, 이는 단순히 우리가 enum 멤버의 값에 접근할 수 있는 것과 마찬가지로 enum name 자체에 대한 접근도 있다는 것을 의미한다. 첫 번째 데모의 샘플은 아래 설명을 위해 사용됩니다.

```cpp
enum Weekend {
  Friday = 1,
  Saturday,
  Sunday
}
Weekend.Saturday     
Weekend["Saturday"];  
Weekend[2];
```

위의 코드 블록에서 `주말`입니다.토요일은 2번, 주말은 2번으로 돌아가지만 역지도로 주말[2]은 토요일이라는 멤버 이름을 돌려준다. 이는 역방향 매핑 때문입니다. TypeScript가 로그 명령으로 매핑을 역방향 해석하는 간단한 방법을 볼 수 있습니다.

```cpp
enum Weekend {
  Friday = 1,
  Saturday,
  Sunday
}
console.log(Weekend);
```

콘솔에서 이를 실행하면 다음과 같은 출력이 표시됩니다.

```bash
{
  '1': 'Friday',
  '2': 'Saturday',
  '3': 'Sunday',
  Friday   : 1,
  Saturday : 2,
  Sunday  : 3
}
```

개체에는 TypeScript 의도대로 값과 이름으로 모두 표시되는 열거형이 포함됩니다. 이것은 TypeScript에서 역방향 매핑의 역효과를 보여준다.

### TypeScript는 모범 사례를 열거합니다.

열거형을 사용하기에 최적이고 매우 효율적인 장소와 적절한 사용 사례가 있습니다. 열거형을 삭제해야 하는 경우도 있습니다. 아래에서는 TypeScript 열거 모범 사례와 대체 방법을 언제 사용해야 하는지에 대해 설명합니다.

#### Typescript 열거형을 사용하는 경우

열거형은 주 7일과 같이 상수로 볼 수 있는 구별되는 값이 있는 경우에 이상적으로 사용되어야 한다.

```undefined
enum Days {
  Sunday = 1,
  Monday,
  Tuesday,
  Wednesday,
  Thursday,
  Friday,
  Saturday
}
```

다른 TypeScript 데이터 유형과 마찬가지로 어레이 초기화 내에서 열거형을 사용할 수 있습니다.

다음은 간단한 예입니다.

```undefined
enum NigerianLanguage {
  Igbo,
  Hause, 
  Yoruba
}

//can be used in array initialisation 
let citizen = {
  Name: 'Ugwunna',
  Age: 75,
  Language: NigerianLanguage.Igbo
}
```

문자열 또는 상수를 변수에 나타내야 하는 장소에서도 열거형을 사용할 수 있습니다.

#### 스크립트 열거형 대 대체 형식

TypeScript 열거형은 다음 위치에서 사용할 수 없습니다.

- 열거형 멤버 값을 다시 할당하거나 변경하려는 경우 열거형은 안전하므로 재할당 시 컴파일 오류가 반환됩니다.
- 동적 값을 기록하려는 경우 열거형이 유한 항목에 가장 적합하며 그 이면에 있는 일반적인 아이디어는 사용자 정의 상수 시스템을 만드는 데 도움이 됩니다.
- 열거형을 변수로 사용할 수 없습니다. 그렇게 하면 오류가 반환됩니다.

### 추가 리소스:

- TypeScript 공식 설명서
- 캣 버쉬의 일러스트
- TypeScript 2.4 릴리스 정보
- 튜토리얼 교사 기사

### 결론

우리는 유형과 속성을 포함하여 TypeScript의 열거형을 잘 살펴볼 수 있었다. 우리는 또한 그것들이 어떻게 사용되는지에 대한 구문과 실제적인 예를 보았다. 열거형, 계산형 열거형 및 심지어 역방향 매핑의 상수와 같은 다른 중요한 열거형 측면을 보았다. 문자열 열거형의 경우 역방향 매핑이 지원되지 않습니다. 또한 유형이 다른 멤버의 경우 숫자 유형 멤버에만 지원되고 문자열 유형 멤버에는 지원되지 않습니다. 해피 코딩!