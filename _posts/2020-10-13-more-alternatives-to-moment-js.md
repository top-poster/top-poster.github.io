---
layout: post
title: "Momentus.js의 추가 대안"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/alternativestomomentjs-nocdn.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/alternativestomomentjs-nocdn.png?fit=730%2C412&ssl=1)

Momentus.js가 JavaScript 에코시스템에서 가장 인기 있는 라이브러리 중 하나임은 의심의 여지가 없지만, 이제 이 라이브러리가 유지 보수 모드의 레거시 프로젝트로 간주되고 사용이 중단되었으므로 다른 대안을 찾고 있을 수 있습니다.

처음에는 Mind.js로 할 수 있는 일이 많기 때문에 대체 라이브러리를 찾는 것은 쉬운 일이 아닌 것 같습니다. 예를 들어:

- 밀리초에서 연도까지 임의의 시간 추가
- 날짜가 다른 날짜 이후인지 확인
- 다른 시간과 관련된 시간 표시
- 특정 로케일을 사용하여 날짜 표시

그러나 대부분의 프로젝트에는 이러한 기능이 모두 필요하지 않습니다. 어떤 프로젝트들은 특정한 방식으로 날짜 및 시간을 포맷하기 위해 모멘트.js를 사용할 수 있지만(상대적인 날짜 및 시간이 널리 사용됨) 다른 프로젝트에서는 날짜가 사용자의 로케일에 따라 이전, 이후 또는 다른 날짜 사이에 있는지 확인하는 것이 더 중요할 수 있다.

따라서, 많은 프로젝트들은 네이티브 자바스크립트 객체(예: Date 및 Intl)와 특정 목적을 위한 하나의 경량 라이브러리를 조합하여 그들의 요구사항을 충족시킬 수 있을 것이다.

작년에 나는 국제화의 맥락에서 Ment.js의 대안을 검토하는 기사를 썼다. 해당 기사에서 검토한 라이브러리(luxon, date-fns, day.js)는 여전히 Momentus.js의 좋은 대안이지만, 이 기사에서는 다음 세 개의 라이브러리의 기능을 검토하겠습니다.

- js-joda
- 설탕
- 시공간

또한, 저는 인텔을 다시 방문하겠습니다.Relative TimeFormat 개체는 2020년 초에 4단계에 도달했고 현재 더 많은 브라우저에서 지원됩니다.

슬슬 출발 해야지.

## js-joda

Java로 작업한 경우, Java 8에 도입된 Date/Time API의 포트이기 때문에 이 라이브러리 사용법을 익히는 데 어려움을 겪지 않습니다.

js-joda는 불변의 핵심 클래스 집합으로 구성된다. 주요 사항은 다음과 같습니다.

- 로컬 날짜 - 시간 정보가 없는 날짜를 나타냅니다. 예를 들어, `2020-12-01`
- 로컬 시간, 이것은 시간을 나타냅니다. 예를 들어 `10:00:01.999999999`와 같습니다.
- LocalDateTime, 시간 정보가 있는 날짜를 나타냅니다. 예를 들어 `2020-12-01T10:00:01.999999999`

이러한 클래스는 시간대 정보를 제공하지 않습니다. 이를 위해 날짜, 시간 및 시간대 정보를 저장하는 ZonedDateTime과 특정 시간대와 함께 작업하려면 ZoneId 또는 ZoneOffset을 사용해야 합니다(패키지 `@js-joda/timezone`도 가져와야 합니다).

또한 라이브러리는 다른 클래스(예: ChronoField)와 날짜 또는 시간의 일부(예: 월)를 나타내는 유형을 제공합니다. 특히, 금액과 시점을 나타내는 클래스 집합이 있습니다.

- 기간 - 나노초에서 일까지의 시간을 나타냅니다. 예를 들어, 2시간
- 기간: 일, 월 및 연도를 사용하는 날짜 기반 시간을 나타냅니다. 예를 들어 2년 5일
- Instant는 1970-01-01T00:00Z(시간대 정보 포함)에서 측정한 단일 시점을 나타냅니다. 예를 들어 `2020-12-01T10:00:47.202Z`

대부분의 클래스는 공통 인터페이스를 가지고 있기 때문에 동일한 방법을 다른 방법으로 구현할 수 있다. 예를 들어 Temporary는 Instant, LocalTime, ZonedDateTime과 같은 클래스에서 구현되는 날짜 시간 필드(일 또는 초)를 조작하는 방법을 계약으로 제공한다.

이렇게 하면 날짜-시간 객체를 구문 분석하거나 생성할 때 모든 클래스는 from, from, 구문 분석, now 등의 방식으로 변한다. 다음은 몇 가지 예입니다.

```undefined
// Creates a LocalDate object from a year, month, and dayOfMonth value
const ld1 = LocalDate.of(2020, Month.DECEMBER, 1);
// Creates a LocalTime object from an ISO 8601 string
const lt1 = LocalTime.parse("10:01:00.123456789");
// Creates a LocalDateTime object from the current datetime in UTC time
const ldt1 = LocalDateTime.now(ZoneOffset.UTC);
// Creates a ZonedDateTime ojbect from from a local date, time, and a time zone
const zdt1 = ZonedDateTime.of(ld1, lt1, ZoneId.of("Europe/Paris"));
// Creates an instant from the ZonedDateTime object
const i1 = Instant.from(zdt1);
// Creates a Period object from a text string such as PnYnMnD
const p1 = Period.parse("P1Y10M");
// Creates a Duration object from a number of standard hours (positive or negative)
const d1 = Duration.ofHours(-48);
```

`with*` 메서드는 세터로 사용할 수 있습니다(모든 것이 불변하므로 새 인스턴스 생성).

```cpp
// Returns a copy of the LocalDate with the value 2020/12/3
const ld2 = ld1.withDayOfMonth(3);
// Returns a copy of the Instant object with the specified seconds but without changing the nanoseconds part
const i2 = i1.withFieldValue(ChronoField.INSTANT_SECONDS, 923232434);
```

또는 `at*` 메서드를 사용하여 다음과 같은 서로 다른 유형의 두 인스턴스를 결합할 수 있습니다.

```cpp
// Combines a LocalDate and a LocalTime object to create a new LocalDateTime
const ldt2 = ld1.atTime(lt1);
// Combines a LocalDateTime object and a time zone to create a ZonedDateTime object
const zdt2 = ldt1.atZone(ZoneId.of("+04:00"));
```

비슷한 방법으로 조건을 쿼리하고 확인하는 방법은 `is`로 시작합니다.

```css
console.log(ld1.isLeapYear());
console.log(ldt1.isBefore(ldt2));
```

또한 개체의 날짜/시간 정보를 조작하는 `플러스` 및 `마이너스` 방법이 있습니다.

```cpp
// Add 10 minutes: 10:11:00.123456789
const lt2 = lt1.plusMinutes(10);
// Add a duration
const ldt3 = ldt1.plus(d1);
// Subtract one hour: 09:01:00.123456789
const lt3 = lt1.minus(1, ChronoUnit.HOURS);
// Substract a period
const zdt3 = zdt1.minusAmount(p1);
```

날짜 형식과 관련하여, js-joda는 minute.js만큼 많은 옵션을 가지고 있지 않지만, 사용자 정의 형식을 위한 패턴 문자열 집합(Java에서 사용되는 패턴과 동일한 패턴)을 가지고 있다. 다음은 몇 가지 예입니다.

```cpp
// 10:00
console.log(lt2.format(DateTimeFormatter.ofPattern("h:m")));
// 2019, 1
console.log(zdt3.format(DateTimeFormatter.ofPattern("y, Q")));
```

그러나 패턴에 텍스트가 있는 경우 로케일을 사용해야 합니다.

이렇게 하려면 모든 로케일을 가져올 `@js-joda/locale` 패키지와 함께 `@js-joda/timezone` 패키지를 가져오거나 개별 로케일 패키지(예: `@js-joda/locale_de)를 가져오십시오.

```js
import "@js-joda/timezone";
import { Locale } from "@js-joda/locale_de";

// ...

// Formatting text with the DE locate: Okt. 1 2:52 PM
console.log(
  ldt3.format(
    DateTimeFormatter.ofPattern("MMM d h:m a").withLocale(Locale.GERMAN)
  )
);
```

이 샌드박스의 모든 예를 시도해 볼 수 있습니다.

## 설탕

슈가(Sugar)는 배열, 숫자, 객체, 날짜 등 다양한 유틸리티 기능을 제공하는 라이브러리이다.

라이브러리에는 약 38kgzip 크기의 많은 모듈 및 폴리필이 포함되어 있습니다. 그러나 전체 기능 집합을 사용하지 않을 계획이면 세 가지 옵션이 있습니다.

- 다운로드 페이지를 통해 사용자 지정 빌드 만들기
- 전체 패키지를 npm 또는 bower와 함께 설치합니다. 이 패키지는 방법과 전체 모듈을 개별적으로 요구할 수 있습니다.
- 사용할 모듈의 패키지만 설치합니다. 이 경우, 설탕 날짜 패키지는

설탕은 세 가지 사용 방법이 있다.

- `슈가` 글로벌 개체를 사용하는 Default는 네이티브 클래스를 확장하는 모듈에 해당하는 네임스페이스로 구성됩니다. 메소드를 개체에서 직접 호출합니다. 예를 들면, `슈가`입니다.Date.create("2020-01-01")
- Chainable - 인스턴스(instance)를 빌드하기 위해 Sugar 네임스페이스를 생성자로 사용합니다. 예를 들어 새로운 슈가.날짜("2020-01-01")는 도약년(.raw`)입니다.
- 확장 - 메소드를 네이티브 유형에 직접 매핑합니다.

```css
Sugar.Date.extend();
console.log(new Date().isLeapYear());
```

여기서 기본 모드를 사용합니다.

인스턴스를 만들려면 다양한 형식을 전달하는 `생성() 방법`을 사용합니다. 이러한 방법 중 대부분은 일반적이지 않습니다.

```cpp
const d1 = Sugar.Date.create("last month");
const d2 = Sugar.Date.create("in 2 hours");
const d3 = Sugar.Date.create("20th of May");
const d4 = Sugar.Date.create("13 Jan 2014 11:00:00 CST");
const d5 = Sugar.Date.create("the 2nd Friday of October 2009");
const d6 = Sugar.Date.create("6-2017"); // months are zero-based, this is actually May, 2017
const d7 = Sugar.Date.create("5 minutes ago");
```

라이브러리의 단위 테스트에서 날짜를 만드는 방법에 대한 많은 예를 볼 수 있습니다.

Sugar 인스턴스를 생성한 후에는 `set` 방법을 사용하여 날짜를 조작할 수 있습니다. 또한 부울 매개 변수를 사용하여 전달된 단위보다 더 구체적인 단위를 재설정할 수 있습니다(달은 0입니다).

```cpp
// Sets month to January
Sugar.Date.set(d1, { month: 0 });
// Sets day to 10 and reset hours to midnight
Sugar.Date.set(d2, { day: 10 }, true);
```

슈가에서는 모든 방법이 불변의 것은 아니다. 여기서 날짜 개체를 변이시키는 방법 목록을 찾을 수 있습니다.

또한 날짜를 앞뒤로 이동(각각 `진격()`과 `반올림()`하고, 단일 시간 단위 `추가()`를 추가하고, 날짜를 시간 단위의 시작(`시작(`*)(`)과 끝(`끝)으로 이동하는 방법도 있다.

```cpp
// Shifts the date forward one month
Sugar.Date.advance(d3, { months: 1 }); // Keys can be singular too: month
// Shifts the date backwards 2 hours
Sugar.Date.rewind(d4, { hours: 2 });
// Adds 1 month
Sugar.Date.addMonths(d5, 1);
// Sets the date to Jan 1st at midgnight of the same year
Sugar.Date.beginningOfYear(d6);
// Sets the time to 11:59:59
Sugar.Date.endOfDay(d7);
```

is로 시작하는 방법을 통해 날짜를 비교하거나 테스트할 수 있습니다.

```undefined
// Is the year of d1 2020?
Sugar.Date.is(d1, '2020');
// Is d1 before January 2nd, 2020?
Sugar.Date.isBefore(d1, 'January 2nd, 2020');
// Is d1 after December 2018?
Sugar.Date.isAfter(d1, 'December 2018');
// Is d1 Friday?
Sugar.Date.isFriday(d1);
// Is d1 a date in the future?
Sugar.Date.isFuture(d1);
// Is d1 a weekday?
Sugar.Date.isWeekday(d1);
```

슈가는 또한 *이후, *아고, *이후, *이후로, *이후로, *이후로"와 *이후로"라는 방법으로 많은 단위에서 시차를 얻을 수 있는 방법을 제공한다.

```coffeescript
// How many months have passed since d4?
Sugar.Date.monthsSince(d4);
// How many years have passed between d4 and d5?
Sugar.Date.yearsSince(d4, d5);
// How many days ago was d4)
Sugar.Date.daysAgo(d4);
// How many hours aga from d5 was d4?
Sugar.Date.hoursAgo(d5, d4);
// How many weeks from d4 until now?
Sugar.Date.weeksUntil(d4);
// How many seconds until d5 from d4?
Sugar.Date.secondsUntil(d5, d4);
// How many ms from now to d4)
Sugar.Date.millisecondsFromNow(d4);
```

하지만 제가 슈가에 대해 가장 좋아하는 것은 포맷을 위한 모든 옵션입니다. 형식 방법은 LDML과 strfttime이라는 두 가지 유형의 토큰을 지원합니다.

LDML: 짧고 기억하기 쉬운 형식입니다(여기에서 토큰 목록을 검색합니다).

```cpp
Sugar.Date.format(d3, "{Weekday}, {hours}:{mm}:{ss}{TT}"); // e.g. Saturday, 12:00:00AM
```

그리고 python과 같은 다른 프로그래밍 언어에서 사용되는 strfttime(여기서 토큰 목록 검색):

```cpp
Sugar.Date.format(d3, "{Weekday}, {hours}:{mm}:{ss}{TT}"); // e.g. Saturday, 12:00:00AM
```

또한 다음과 같은 네 가지 사전 정의된 형식 패턴(짧은 형식, 중간 형식, 긴 형식, 꽉 찬 형식)이 있습니다.

```cpp
Sugar.Date.short(d6); // 01/01/2017
Sugar.Date.medium(d6); // January 1, 2017
Sugar.Date.long(d6); // January 1, 2017 12:00 AM
Sugar.Date.full(d6); // Sunday, January 1, 2017 12:00 AM
```

그리고 가장 적절한 단위를 자동으로 선택하는 상대 시간(상대 시간 및 상대 시간)에 대한 두 가지 방법:

```cpp
Sugar.Date.relative(d2); // 2 weeks ago
Sugar.Date.relativeTo(d2, d5); // 10 years
```

영어는 자동으로 포함된 기본 로케일입니다. 이 글 작성 당시 슈가는 공식 빌드에 포함되거나 다운로드 페이지를 통해 별도로 추가된 17개 로케일을 지원한다.

로캘을 `setLocale` 메서드로 전체적으로 설정하거나 `isLast`와 같은 로캘 종속 메서드에 인수로 전달할 수 있습니다.주간(주초 알 수 있음), 생성() 또는 상대():

```cpp
import "sugar-date/locales";

// Sets the locale to italian globaly
// Sugar.Date.setLocale("it");

// Parse the string with the spanish locale
const d8 = Sugar.Date.create("hace 5 dias", "es");

// Uses the default locale, english (or the one set with setLocale)
// It doesn't use the locale used to create the instance
Sugar.Date.full(d8);

// Uses the french locale
Sugar.Date.full(d8, "fr");
```

이 샌드박스의 모든 예를 시도해 볼 수 있습니다.

## 시공간

Spacetime은 다른 DST 규칙을 사용하여 시간대 및 서머타임(DST)의 시간을 조작할 때 오류를 피하기 위해 표준 시간대에 특별히 초점을 맞추어 날짜를 구문 분석, 조작, 비교 및 포맷하는 라이브러리이다.

그러나 일부 고려사항에는 Spaceetime의 API가 Ment.js와 매우 유사하며(몇 가지 추가 사항이 있음) 모든 방법이 불변하다는 점을 알아야 합니다.

예를 들어 다음과 같은 여러 입력 형식과 몇 가지 도우미 방법을 사용하여 Spacetime 인스턴스를 생성할 수 있습니다.

```cpp
/ ISO Format
const s1 = spacetime("2020-12-01");
// As long date
const s2 = spacetime("Dec 02 2020 17:50");
// As epoch in ms
const s3 = spacetime(1606975200000);
// As an array (months are zero-based)
const s4 = spacetime([2020, 0, 1, 20, 0]);
// As an object (months are zero-based)
const s5 = spacetime({year:2020, month:0, date:1});
// Current time
const s6 = spacetime.now();
// Today at midgnight
const s7 = spacetime.today();
// Tomorrow at midnight
const s8 = spacetime.tomorrow();
```

Getter 및 Setter는 Memote.js와 동일한 방식으로 처리되며, `season()`hourFlat()the`s 및 `progress()`와 같은 몇 가지 유용한 추가 사항이 있습니다.

```js
// Sets a new date based on s1 with 400 milliseconds
const s9 = s1.millisecond(400);
// Get milliseconds: 400
console.log("s9.milliseconds(): " + s9.millisecond());
// Get month (zero-based): 11
console.log("s9.month(): " + s9.month());
// Get day of year: 336
console.log("s9.dayOfYear(): " + s9.dayOfYear());
// Get day of year: winter
console.log("s9.season(): " + s9.season());
// Set the hour + minute in decimal form
const s10 = s2.hourFloat(16.5);
// Get the time: 4:30pm
console.log("s10.time(): " + s10.time());
// How far the moment lands between the start and end of the day/week/month/year (percentage-based)
console.log("s3.progress('year'): " + s3.progress('year'));
```

쿼리 또는 비교 방법에서도 마찬가지입니다.

```cpp
// s3: 2020-12-03
// s4: 2020-01-01
console.log("s3.isAfter(s4): " + s3.isAfter(s4));
console.log("s3.isBefore(s4): " + s3.isBefore(s4));
console.log("s3.isEqual(s4): " + s3.isEqual(s4));
console.log("s3.leapYear(): " + s3.leapYear());
// Detect if two date/times are the same day, week, or year, etc
console.log("s3.isSame(s4, 'year'): " + s3.isSame(s4, "year"));
// Given a date amd a unit, count how many of them you'd need to make the dates equal
console.log("s3.diff(s4, 'day'): " + s3.diff(s4, "day"));
// Is daylight-savings-time activated right now, for this timezone?
console.log("s3.inDST(): " + s3.inDST());
// Does this timezone ever use daylight-savings?
console.log("s3.hasDST(): " + s3.hasDST());
// The current, DST-aware time-difference from UTC, in hours
console.log("s3.offset(): " + s3.offset());
// Checks if the current time is between 10pm and 8am
console.log("s3.isAsleep(): " + s3.isAsleep());
```

조작 방법뿐 아니라:

```undefined
// s5: 2020-01-10 1:31pm
// Move to the first millisecond of the day, week, month, year, etc.
const s11 = s5.startOf('month'); // 2020-01-01 12:00am
// Move to the last millisecond of the day, week, month, year, etc.
const s12 = s5.endOf('week'); // 2020-01-12 11:59pm
// Increment the date/time by a number and unit
const s13 = s5.add(1, 'season') // 2020-05-10 1:31pm
// Decrease the date/time by a number and unit
const s14 = s5. subtract(2, 'years') // 2018-01-10 1:31pm
// Move forward/backward to the closest unit
const s15 = s5.nearest('hour'); // 2020-01-10 2:00pm
// Go to the beginning of the next unit
const s16 = s5.next('quarter'); // 2020-01-04 12:00am
// Go to the beginning of the previous unit
const s17 = s5.last('month'); // 2019-12-01 12:00am
```

날짜와 시간을 포맷하기 위해 Spacetime에는 다음과 같은 몇 가지 미리 정의된 형식이 있습니다.

```php
s11.format('numeric-uk') // 01/01/2020
s12.format('iso-utc') // 2020-01-13T05:59:59.999Z
s13.format('mm/dd') // 05/10
s14.format('nice') // Jan 10th, 1:31pm  
s15.format('quarter') // Q1     
// They can be combined using this syntax
s16.format('{day} {date-ordinal}, {month-short} {year}')); // Sunday 1st, Dec 2019
```

그러나 더 많은 표준 날짜 형식 패턴을 사용할 수도 있습니다.

```undefined
s17.unixFmt('yyyy/MM/dd h a')); // 2019/12/01 12 AM
```

또는 상대 시간에 대한 since() 함수입니다. 예를 들어, 실행할 때:

```bash
spacetime('September 1 2020').since('September 30 2020')
```

다음 개체를 반환합니다.

```undefined
{
   "diff": {
      "years": 0,
      "months": 0,
      "days": -29,
      "hours": 0,
      "minutes": 0,
      "seconds": 0
   },
   "rounded": "in 29 days",
   "qualified": "in 29 days",
   "precise": "in 29 days"
}
```

표준 시간대에서는 인스턴스를 생성할 때 추가 매개 변수를 전달하여 표준 시간대를 지정할 수 있습니다(IANA 이름 사용 권장).

```cpp
const s18 = spacetime(1601521200000, "Europe/Paris");
const s19 = spacetime([2020, 0, 1, 20, 0], "Lima"); // America/Lima
const s20 = spacetime.now("-4h");
```

그러나 인스턴스가 있으면 goto()를 사용하여 다른 표준시로 쉽게 변경할 수도 있습니다(다시 한 번 말하지만 IANA 이름은 사용하는 것이 좋습니다).

```cpp
const s21 = s18.goto("Australia/Sydney"); // Oct 1 2020, 1:00pm
const s22 = s18.goto("GMT-5"); //-5 is actually +5
```

또한 로컬 시간을 기준으로 시간 범위 내에 모든 시간대가 포함된 배열을 얻을 수 있습니다.

```coffeescript
spacetime.whereIts('12:00pm', '2:00pm') // ["asia/seoul", "asia/tokyo", "pacific/palau", "australia/adelaide", ...]
spacetime.whereIts('10am') // Within an hour, from 10am to 11am
```

인스턴스의 시간대에 대한 메타데이터를 가져옵니다.

```bash
s21.timezone();
/* Returns:
{
   "name": "Australia/Sydney",
   "hasDst": true,
   "default_offset": 10,
   "hemisphere": "South",
   "current": {
      "offset": 10,
      "isDST": false
   },
   "change": {
      "start": "04/05:03",
      "back": "10/04:02"
   }
}
*/
```

이 샌드박스의 모든 예를 시도해 볼 수 있습니다.

## Intl.상대 시간 형식

Momentus.js의 가장 유용한 기능 중 하나는 날짜를 상대 시간으로 표시하는 기능입니다.

```undefined
const start = moment('2020-12-01');
const end   = moment('2020-12-04');
start.to(end); // "in 3 days"
start.to(end, true); // "3 days"
```

모든 라이브러리가 이 기능을 제공하는 것은 아니지만, 이제 `Intl`을 사용할 수 있습니다.Relative Time Format`은 대부분의 브라우저에서 완벽하게 지원되므로 더 이상 문제가 되지 않습니다.

`Int.RelativeTimeFormat`은 숫자를 지역화된 방식으로 상대 시간으로 포맷할 수 있는 표준 내장 객체입니다.

생성자는 선택적으로 두 가지 인수, 즉 BCP 47 언어 태그(또는 이러한 문자열 배열)와 로케일 일치 알고리즘, 형식 및 출력 메시지의 길이를 구성하는 속성을 가진 객체를 사용합니다.

```js
const rtf = new Intl.RelativeTimeFormat("es", {
    localeMatcher: "best fit",
    numeric: "auto",
    style: "short"
});
```

이 개체의 인스턴스가 있는 경우 메시지에 사용할 숫자 값을 전달하는 `format() 방법`을 첫 번째 인수로 사용하고 단위(예: 단수 또는 복수 형식)를 두 번째 인수로 사용할 수 있습니다.

```undefined
rtf.format(-4, 'second'); // hace 4 s
rtf.format(-1, 'week'); // la semana pasada
rtf.format(3, 'quarter'); // dentro de 3 trim.
rtf.format(2, 'year'); // dentro de 2 a
```

또는 메시지 부분이 분리된 배열을 가져오려면 "formatToParts()"를 선택합니다.

```bash
rtf.formatToParts(-4, 'second');
/* Returns:
[
   {
      "type": "literal",
      "value": "hace "
   },
   {
      "type": "integer",
      "value": "4",
      "unit": "second"
   },
   {
      "type": "literal",
      "value": " s"
   }
]
**/
```

숫자 부분을 얻는 방법, 즉 두 날짜 사이의 경과 시간을 알고 싶은 경우 다음 기능을 공유하는 스택 오버플로 답변을 확인하십시오.

```js
// in miliseconds
var units = {
  year  : 24 * 60 * 60 * 1000 * 365,
  month : 24 * 60 * 60 * 1000 * 365/12,
  day   : 24 * 60 * 60 * 1000,
  hour  : 60 * 60 * 1000,
  minute: 60 * 1000,
  second: 1000
};

var rtf = new Intl.RelativeTimeFormat('en', { numeric: 'auto' });

var getRelativeTime = (d1, d2 = new Date()) => {
  var elapsed = d1 - d2;
  // "Math.abs" accounts for both "past" & "future" scenarios
  for (var u in units)
    if (Math.abs(elapsed) > units[u] || u == 'second')
      return rtf.format(Math.round(elapsed/units[u]), u);
}
```

이 샌드박스의 모든 예를 시도해 볼 수 있습니다.

## 결론

Ment.js에 대한 대안이 많다는 것은 의심의 여지가 없다. 이 기사에서는 유사한 기능을 제공하는 세 가지 라이브러리와 경우에 따라 몇 가지 유용한 추가 사항을 검토했다.

각 라이브러리는 다양한 사용 사례에 더 적합하다고 생각합니다.

- js-joda는 특히 Java 배경이 있는 경우 범용 라이브러리로 적합합니다.
- 설탕은 날짜 포맷에 특히 좋고, 날짜 이외의 다른 타입에 많은 방법을 제공하기 때문에 응용 프로그램의 다른 부분에 유용할 수 있다.
- 공간 시간은 원격 시간을 계산하는 방법 및 시간대 간 변경 방법으로 인해 여러 시간대를 지원해야 하는 경우에 특히 유용합니다.

또한, 이제 자바스크립트 국제화 API가 브라우저에 의해 더욱 광범위하게 지원되고 있으며, 경량 라이브러리와 네이티브 기능의 조합을 고려하거나 외부 라이브러리가 필요한 경우에도 마찬가지입니다.

해피 코딩!