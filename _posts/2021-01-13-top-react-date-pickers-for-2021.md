---
layout: post
title: "2021년 상위 반응 날짜 선택자"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/reactdatepickers2021.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/reactdatepickers2021.png?fit=730%2C487&ssl=1)

개발자로서 우리는 항상 프로젝트 시간을 절약할 수 있는 방법을 모색합니다. 그렇기 때문에 도서관이 만들어져서 같은 것을 계속해서 실행하는 데 걸림돌이 되지 않습니다. React와 같은 프런트 엔드 프레임워크를 사용하면 다양한 프로젝트에 대한 공통 기능을 어느 때보다 쉽게 공유할 수 있습니다.

이 게시물에서 제가 찾은 달력 보기 라이브러리를 살펴보도록 하겠습니다. 최근에 업데이트된 라이브러리만 살펴보겠습니다. 이는 많은 후프를 거치지 않고 프로젝트에 효과가 있을 수 있도록 하기 위한 것입니다.

## 재료 UI 날짜/시간 선택기

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/materialuidate.png?resize=461%2C381&ssl=1)

프로젝트의 UI 구성 요소의 기준으로 재료 UI를 사용하는 경우 재료 UI의 날짜 및 시간 선택 도구도 사용해야 합니다.

이 라이브러리의 장점은 재료 설계에 제약이 있더라도 여전히 매우 사용자 정의가 가능하다는 것입니다. 재료 UI에서 제공하는 `createMuiTheme()` 기능을 통해 스타일을 사용자 정의할 수 있습니다.

이 목록에서 시계 보기를 가진 유일한 구성요소입니다. 따라서 데스크톱과 모바일 보기 모두에서 시간을 쉽게 선택할 수 있습니다.

공식 설명서의 예에서는 날짜 구문 분석 및 형식 지정에 날짜-fn을 날짜-시간 라이브러리로 사용하지만 원하는 날짜-시간 라이브러리를 사용할 수 있습니다. 이 라이브러리의 유일한 단점은 (최소한 안정적인 릴리스에서는) 자체 날짜 범위 선택기가 없다는 것입니다. 날짜 범위 선택기가 여전히 알파에 있습니다.

날짜 선택 도구에는 코어 재료 UI 라이브러리가 함께 제공되지 않으므로 다음 명령으로 설치해야 합니다.

```undefined
npm install @material-ui/core date-fns @date-io/date-fns@^1.3.13 @material-ui/pickers --save
```

그러면 그렇게 사용할 수 있습니다. 이 코드는 현재 안정적인 버전의 재료 UI에 대한 코드입니다. 더 이상 지원되지 않습니다(이 기사를 작성한 시점으로부터 약 한 달 전). 따라서 나중에 이 문서를 읽을 때 다음 버전의 재료 UI가 이미 안정되어 있다면 이 대신 다음을 사용해야 합니다.

```js
import React, { useState } from 'react';
import DateFnsUtils from '@date-io/date-fns';
import {
  MuiPickersUtilsProvider,
  KeyboardDatePicker,
} from '@material-ui/pickers';

export default function MaterialDatePicker() {

  const [selectedDate, setSelectedDate] = useState(new Date());

  const handleDateChange = (date) => {
    setSelectedDate(date);
  };

  return (
    <MuiPickersUtilsProvider utils={DateFnsUtils}>
      <KeyboardDatePicker
        disableToolbar
        variant="inline"
        format="MM/dd/yyyy"
        margin="normal"
        id="date-picker-inline"
        label="Date picker inline"
        value={selectedDate}
        onChange={handleDateChange}
        KeyboardButtonProps={
          'aria-label': 'change date',
        }
      />
    </MuiPickersUtilsProvider>
  );

}
```

## 특징들

- 재료 UI 설계를 따릅니다.
- 날짜/시간 선택기
- 날짜 라이브러리 불가지론자
- 현지화 가능

### 자원.

- 재료 UI 날짜/시간 선택기

## 반동일의

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/reactdaypicker.png?resize=330%2C334&ssl=1)

이것은 그 무리들 중에서 가장 단순한 도서관이다. 프로젝트에 사용할 경량 라이브러리를 찾는 경우 이 달력 보기를 선택하십시오. 그 크기에 속지 마세요. 데이트 상대를 고르는 사람들이 일반적으로 필요로 하는 모든 공통적인 기능들을 제공할 수 있고 그리고 더 많은 기능들을 제공할 수 있습니다. 기본 스타일은 매우 단순해 커스터마이징이 용이하다. 날짜 및 현지화를 위한 자체 날짜 유틸리티가 함께 제공됩니다. 원하는 경우 원하는 날짜 라이브러리를 사용할 수 있습니다.

이 라이브러리에서 가장 좋은 점은 라이브러리로 수행할 수 있는 거의 모든 작업에 대한 광범위한 예제 목록이 있다는 것입니다. 예를 들어 특정 날짜를 사용하지 않도록 표시하거나 클릭 시 날짜 범위를 선택할 수 있습니다.

다음과 같이 설치할 수 있습니다.

```undefined
npm install react-day-picker --save
```

설치 후 사용할 수 있는 방법은 다음과 같습니다.

```js
import React, { useState } from "react";
import DayPickerInput from "react-day-picker/DayPickerInput";
import "react-day-picker/lib/style.css";

export default function ReactDayPicker() {
  const [date, setDate] = useState(new Date());

  function onChange(date) {
    setDate(date);
  }

  return <DayPickerInput onDayChange={onChange} />;
}
```

## 특징들

- 날짜 범위 선택기
- 일정관리 및 텍스트 필드 입력
- 현지화 가능
- 사용자 지정 가능
- 자체 날짜 유틸리티와 함께 제공됩니다.

### 자원.

- 반동일의

## 탄소 설계 시스템 달력 보기

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/carbondesignsystemsdatepicker.png?resize=374%2C438&ssl=1)

카본은 IBM의 오픈 소스 디자인 시스템입니다. IBM의 Design Language를 기반으로 합니다. IBM은 구글의 재료 설계 지침에 상당합니다. 그러나 리액트 전용인 재료 UI와 달리 카본은 Vue, Angular, Svelte, 심지어 바닐라 자바스크립트도 지원한다. 따라서 다음 프로젝트에 탄소 설계를 채택하려면 이 달력 보기를 사용할 수 있습니다. 그렇지 않으면 건너뜁니다.

그 핵심에는 디자인 시스템이 있기 때문에 디자인의 일관성에 큰 초점을 두고 있습니다. 다양한 요소들의 정렬, 그리고 다양한 상태들이 어떻게 보여야 하는지와 같은 것들이 가장 우선순위가 주어집니다.

이 라이브러리의 단점은 날짜 선택 도구 구성 요소가 없다는 것입니다. 또한 사용자가 수동으로 날짜를 입력할 수도 있습니다. 이는 종종 실수하기 쉬우므로 직접 검증을 구현해야 합니다. 후드 아래에서는 플랫 피커를 사용하므로 플랫 피커 옵션을 사용하여 완전히 사용자 정의할 수 있습니다.

다음과 같이 설치할 수 있습니다.

```undefined
npm install carbon-components carbon-components-react carbon-icons --save
```

그런 다음 다음과 같이 사용합니다.

```js
import React from 'react';
import { DatePickerInput } from 'carbon-components-react';

export default function CarbonDatePicker() {

  return (
    <DatePickerInput
        placeholder="mm/dd/yyyy"
        labelText="Date Picker label"
        id="date-picker-single"
        onChange={date => {
          console.log(date);
        }
      />
    </DatePicker>  
  );
}
```

카본은 별로 좋지 않기 때문에 납작한 픽커 문서를 참고하세요.

## 특징들

- 일관성 있는 설계
- 날짜, 시간, 날짜 범위 선택기
- 현지화 가능

### 자원.

- 탄소 설계 시스템
- 탄소 설계 시스템 달력 보기
- 날짜 선택기 API
- 납작한 피커

## 잡역부/리액트 데이트 신청자

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/wojtekmajdatepicker.png?resize=389%2C343&ssl=1)

이 목록의 경량 라이브러리 중 하나입니다. 이 달력 보기는 작업하기 위해 날짜 라이브러리에 종속되지 않습니다. 사용자 정의 가능한 일정관리 보기를 가지고 있으므로 월, 연도, 십년, 세기별 보기를 가질 수 있습니다.

이 도서관의 단점은 사용 샘플의 부족이다. 가장 일반적인 사용 사례에 대한 사용 샘플만 있습니다. 사용 사례가 흔하지 않은 경우, 해당 설명서를 자세히 살펴보아야 합니다.

다음 명령을 사용하여 설치할 수 있습니다.

```undefined
npm install react-date-picker --save
```

그런 다음 다음과 같이 사용할 수 있습니다.

```js
import React, { useState } from 'react';
import DatePicker from 'react-date-picker';

export default function MyDatePicker() {
  const [value, updateValue] = useState(new Date());

  const onChange = (date) => {
    updateValue(date);
  }

  return (
    <div>
      <DatePicker
        onChange={onChange}
        value={value}
      />
    </div>
  );
}
```

## 특징들

- 날짜, 시간, 선택 도구 및 날짜 범위 선택 도구
- 현지화 가능
- 날짜 라이브러리 종속성 없음

### 자원.

이 라이브러리를 만든 개발자는 시간 선택기, 날짜 시간 선택기 및 날짜 범위 선택기와 같은 관련 기능을 자신의 패키지로 구분했습니다. 달력 보기만 필요한 경우 다음 항목을 확인하십시오.

- 잡역부/리액트 데이트 신청자
- 잡역부/반응 시간 기록
- wojekmaj/react-date range-contrace
- 노동 전공자/반응 날짜의 시간표

## 에어비앤비/반응 방지 장치

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/airbnbreactdates.png?resize=650%2C452&ssl=1)

에어비앤비의 리액트 날짜는 이 목록에 있는 오래된 라이브러리 중 하나입니다. 하지만 그런 상황임에도 불구하고, 그것은 여전히 활발하게 유지되고 있습니다. 로컬리제이션이 가능하고, 모바일 친화적이며, 접근성을 염두에 두고 제작되었습니다. 날짜 작업을 위해 Memote.js에 의존하기 때문에 이 목록의 경량 라이브러리에 비해 다소 무겁습니다.

이 도서관의 가장 큰 단점은 그들이 적절한 문서 및 사용 샘플을 가지고 있지 않다는 것입니다. 그들이 가지고 있는 것은 스토리북과 깃허브포의 몇 가지 예시뿐이다. 리액션을 처음 접하거나 코드를 잘 파헤치는 것을 별로 좋아하지 않는 사람이라면 이 코드를 건너뛸 수 있습니다.

다음 명령을 사용하여 설치할 수 있습니다.

```undefined
npm install react-dates --save
```

그런 다음 다음과 같이 사용합니다.

```js
import React, { useState } from "react";
import "react-dates/initialize";
import "react-dates/lib/css/_datepicker.css";
import { SingleDatePicker } from "react-dates";

export default function ReactdatesDatepicker() {
  const [date, setDate] = useState(null);
  const [isFocused, setIsFocused] = useState(false);

  function onDateChange(date) {
    setDate(date);
  }

  function onFocusChange({ focused }) {
    setIsFocused(focused);
  }

  return (
    <SingleDatePicker
      id="date_input"
      date={date}
      focused={isFocused}
      onDateChange={onDateChange}
      onFocusChange={onFocusChange}
    />
  );
}
```

## 특징들

- 날짜, 날짜 범위 선택기
- 현지화 가능
- 모바일 친화적
- 접근 가능

### 자원.

- 에어비앤비/반응 방지 장치
- 리액션 스토리북

## 해커1에 의한 날짜 선택기 대응

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Hackeronedatepicker.png?resize=276%2C310&ssl=1)

단순하고 재사용 가능한 달력 보기 구성 요소입니다.

이 라이브러리의 좋은 점은 설명서에는 여러분이 생각할 수 있는 모든 사용 사례의 예가 포함되어 있다는 것입니다. 사용자 정의 클래스 이름 사용, 특정 날짜 강조 표시, 날짜 및 시간 필터 추가와 같은 모든 항목에는 해당 예가 있습니다. 그들의 예들은 또한 개발자가 날짜 조작을 위해 어떤 날짜 라이브러리도 사용할 수 있다는 것을 의미하는 바닐라 자바스크립트를 사용한다. 그러나 로컬리제이션에는 date-fns를 사용합니다.

이 라이브러리의 단점은 기본 UI가 그다지 좋아 보이지 않는다는 것입니다. 단순하게 제작되었으므로 개발자가 스스로 스타일을 사용자 정의해야 한다고 가정합니다.

다음 명령을 사용하여 설치할 수 있습니다.

```undefined
npm install react-datepicker --save
```

그런 다음 다음과 같이 사용할 수 있습니다.

```js
import React, { useState } from "react";
import DatePicker from "react-datepicker";
import "react-datepicker/dist/react-datepicker.css";

export default function HackeroneDatepicker() {
  const [date, setDate] = useState(new Date());

  function onChange(date) {
    setDate(date);
  }

  return <DatePicker selected={date} onChange={onChange} />;
}
```

## 특징들

- 날짜, 시간, 날짜 범위 선택기
- 사용자 지정 가능
- 접근 가능
- 현지화 가능

### 자원.

- 해커0x01 / 대응 날짜 선택기
- 대응 날짜 선택기

## 반응 레인보우 구성요소 달력 보기

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/reainbowcomponentsdatepicker.png?resize=521%2C486&ssl=1)

React Rainbow는 재료 UI와 마찬가지로 UI 구성 요소 라이브러리입니다. 이미 리액트 레인보우를 사용하고 있다면, 날짜와 시간 구성 요소를 사용하는 데 어려움을 겪을 가능성이 높습니다. 그렇지 않으면 라이브러리 전체를 채택해야 사용할 수 있습니다. 달력 보기, 날짜 선택기 및 날짜 선택기 모달 등이 있습니다.

이 라이브러리의 주요 단점은 구성 요소를 개별적으로 설치할 수 없으므로 전체 UI 라이브러리를 사용하여 해당 날짜 및 시간 선택기 구성 요소를 사용해야 한다는 것입니다.

또 다른 단점은 구성 요소의 모양과 기능에 대해 매우 의견이 많다는 것입니다. 예를 들어 모든 항목이 모달에 있어야 하므로 인라인 달력 보기를 사용할 수 없습니다.

다음 명령을 사용하여 설치할 수 있습니다.

```undefined
npm install react-rainbow-components --save
```

그러면 날짜 선택기를 다음과 같이 사용할 수 있습니다.

```js
import React, { useState } from "react";
import { DatePicker } from "react-rainbow-components";

export default function RainbowDatepicker() {
  const [date, setDate] = useState(null);

  function onChange(date) {
    setDate(date);
  }

  return (
    <DatePicker
      id="datePicker-1"
      value={date}
      onChange={onChange}
      label="DatePicker Label"
      formatStyle="large"
    />
  );
}
```

## 특징들

- 날짜, 날짜 범위, 시간 선택기
- 현지화 가능
- 접근 가능
- TypeScript 지원

### 자원.

- 날짜 선택기
- 날짜 시간 선택기
- 날짜 선택기 모델

## 개미 설계 날짜 선택기

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/antdesigndateandtimepicker.png?resize=321%2C394&ssl=1)

React Rainbow와 마찬가지로 날짜 및 시간 선택기는 전체 UI 구성 요소 라이브러리 자체와 함께 패키징됩니다. Ant Design Specifications를 따랐기 때문에 더 나은 사용자 환경을 제공하기 위해 설계의 일관성에 초점을 맞추고 있습니다. 따라서 전체 UI 구성 요소 라이브러리도 사용하는 경우에만 이 구성 요소를 사용할 수 있습니다.

기본적으로 달력 보기는 날짜 작업에 Ment.js를 사용합니다. 하지만 이 제품들은 특히 번들 크기에 관심이 있는 경우 또 다른 멋진 제품을 사용할 수 있는 방법도 제공합니다.

또 다른 좋은 점은 Ant Design의 모든 구성 요소도 CodeSandbox, CodePen 및 StackBlitz를 통해 편집 가능한 데모를 제공한다는 것입니다. 이것은 단순히 그들의 데모를 조작하는 것만으로 그것들을 시험해 보는 것을 매우 쉽게 만든다.

다음 명령을 사용하여 설치할 수 있습니다.

```undefined
npm install antd --save
```

설치한 후에는 다음과 같이 사용할 수 있습니다.

```js
import React, { useState } from "react";
import { DatePicker } from "antd";
import "antd/dist/antd.css";

export default function AntDatepicker() {
  const [date, setDate] = useState(new Date());

  function onChange(date, dateString) {
    setDate(date);
  }

  return <DatePicker onChange={onChange} />;
}
```

## 특징들

- 날짜, 날짜 범위 및 시간 선택기
- UI 구성 요소 라이브러리와 함께 제공됩니다.
- TypeScript 지원
- 현지화 가능
- 날짜 라이브러리 불가지론자

### 자원.

- 날짜 선택기
- 타임피커

## 하이페서버/반응 날짜 범위

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/hypeserverreactdaterange.png?resize=367%2C320&ssl=1)

CSS 사용자 지정이 거의 또는 전혀 없는 멋진 날짜 범위 선택기를 찾고 있는 경우 이 패키지를 사용할 수 있습니다. 그렇지만 이 라이브러리는 여전히 매우 사용자 정의가 가능합니다. 유일한 단점은 문서와 예시가 약간의 개선을 사용할 수 있다는 것이다. 따라서 일반적인 날짜 범위 선택 기능을 구현하려는 경우 문제가 발생할 수 있습니다.

다음 명령을 사용하여 설치할 수 있습니다. GitHubrepo에서 날짜 라이브러리 관련 없는 달력 보기라고 언급했지만 날짜-fns 라이브러리를 피어 종속으로 표시했기 때문에 프로젝트에 이 라이브러리를 설치해야 합니다.

```undefined
npm install react-date-range date-fns --save
```

그런 다음 다음과 같이 사용할 수 있습니다.

```js
import React, { useState } from "react";
import { Calendar } from "react-date-range";
import "react-date-range/dist/styles.css";
import "react-date-range/dist/theme/default.css";

export default function HypeserverDatepicker() {
  const [date, setDate] = useState(new Date());

  function onChange(date) {
    setDate(date);
  }

  return <Calendar date={date} onChange={onChange} />;
}
```

## 특징들

- 날짜 범위 선택기
- 캘린더 입력
- 접근 가능
- 선택 항목을 클릭한 상태로 유지

### 자원.

- 하이페서버/반응 날짜 범위

## 비교

여기 모든 도서관의 비교표가 있습니다. 이 목록의 모든 라이브러리는 사용자 지정, 액세스 및 로컬리제이션이 가능하므로 아래 표에서 해당 기능을 제외했습니다.

## 결론

이렇게 달력 보기 라이브러리를 반올림합니다. 보셨듯이, 선택할 수 있는 달력 보기 라이브러리는 여러 개 있습니다. 일부는 자체 시간 선택기 구성 요소를 포함하고 일부는 달력 보기 구성 요소만 가지고 있습니다. 기본 달력 보기 기능만으로 간단한 것도 있고 사용자 환경을 사용자 지정하는 데 사용할 수 있는 옵션이 많은 것도 있습니다. 마지막으로 Material UI, Ant Design과 같은 대형 UI 구성 요소 라이브러리와 잘 통합되는 것들이 있습니다.

위의 목록에 없는 멋진 달력 보기 라이브러리를 사용한 경우 아래 설명으로 알려주십시오.