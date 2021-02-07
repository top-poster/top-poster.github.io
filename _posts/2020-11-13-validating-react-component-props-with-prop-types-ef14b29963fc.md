---
layout: post
title: "반응하는 PropType: 완벽한 가이드"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2018/08/react-proptypes.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2018/08/react-proptypes.png?fit=730%2C487&ssl=1)

소품 및 PropType은 반응 구성 요소 간에 읽기 전용 속성을 전달하기 위한 중요한 메커니즘입니다.

"속성"을 나타내는 반응 소품은 한 구성 요소에서 다른 구성 요소로 데이터를 보내는 데 사용됩니다. 구성 요소가 잘못된 유형의 소품을 수신하면 앱에서 버그와 예기치 않은 오류가 발생할 수 있습니다.

JavaScript에는 기본 제공 유형 검사 솔루션이 없기 때문에 많은 개발자가 TypeScript 및 Flow와 같은 확장 기능을 사용합니다. 그러나 React는 PropTypes라는 소품 검증을 위한 내부 메커니즘을 가지고 있다.

이 포괄적인 가이드에서는 ReactPropTypes를 사용하여 소품을 검증하는 방법에 대해 설명합니다. 다음 내용을 다루겠습니다.

- 반응 소품의 작동 방식
- React에서 소품을 검증하는 이유는 무엇입니까?
- 반응 시 PropType 사용
- `prop-type` 라이브러리 사용
- 반응 프로프 유형 검증기
- 유형 검사를 위한 사용자 정의 검증자 반응 도구
- 반응에서 `백분율 통계` 확인

시각 학습자라면 ReactPropType에 대한 비디오 튜토리얼을 참조하십시오.

## 반응 소품의 작동 방식

응답 소품을 사용하면 해당 구성 요소를 호출할 때 숫자, 문자열, 함수, 개체, 배열 등을 포함한 데이터를 구성 요소로 전송할 수 있습니다. 여러 개의 구성 요소가 있는 경우 한 구성 요소에서 다른 구성 요소로 데이터를 전달할 수 있습니다.

구성 요소 간에 소품을 전달하려면 일반 JavaScript 함수를 호출할 때 인수를 전달하듯이 구성 요소가 호출될 때 해당 소품을 호출할 때 인수를 전달합니다.

자세한 내용은 리액션 소품 마스터에 대한 초보 가이드를 참조하십시오.

## React에서 소품을 검증하는 이유는 무엇입니까?

반응 응용 프로그램을 개발할 때 버그와 오류를 방지하기 위해 프로펠러를 구조화하고 정의해야 할 필요가 있을 수 있습니다. 함수에 필수 인수가 있을 수 있는 것과 마찬가지로, Retact 구성 요소는 소품을 정의해야 할 수 있으며, 그렇지 않으면 제대로 렌더링되지 않습니다. 필요한 소품을 필요한 구성 요소에 전달하는 것을 잊어버린 경우 앱이 예기치 않게 작동할 수 있습니다.

다음 코드를 고려하십시오.

```xml
import React from 'react';
import ReactDOM from 'react-dom';

function PercentageStat({ label, score = 0, total = Math.max(1, score) }) {
  return (
    <div>
      <h6>{ label }</h6>
      <span>{ Math.round(score / total * 100) }%</span>
    </div>
  )
}

function App() {
  return (
    <div>
      <h1>Male Population</h1>
      <div>
        <PercentageStat label="Class 1" total={360} score={203} />
        <PercentageStat label="Class 2" total={206} />
        <PercentageStat label="Class 3" score={107} />
        <PercentageStat label="Class 4" />
      </div>
    </div>
  )
}

const rootElement = document.getElementById('root');
ReactDOM.render(<App />, rootElement);
```

위의 조각에서 PercentageStat라는 구성 요소는 적절한 렌더링을 위해 레이블, 점수, 토탈의 세 가지 소품을 필요로 한다. 스코어 및 토탈 소품이 제공되지 않는 경우 기본값은 "score" 및 "total" 소품에 대해 설정됩니다. PercentageStat는 앱 구성 요소에서 각각 다른 소품을 사용하여 4번 렌더링됩니다.

다음은 앱의 모양(일부 부트스트랩 스타일링 포함):

사용량에 따라 라벨 소자는 문자열이 될 것으로 예상된다. 점수(core)와 합계(total)는 백분율을 계산하는 데 사용되기 때문에 숫자값이어야 하는 것도 같은 맥락이다. 또한, "total"은 "0"이 될 수 없을 것으로 예상된다.

아래는 `PercentageStat` 구성 요소를 잘못된 소품으로 렌더링하는 수정된 앱을 보여주는 또 다른 코드 조각입니다.

```xml
function App() {
  return (
    <div>
      <h1>Male Population</h1>
      <div>
        <PercentageStat label="Class 1" total="0" score={203} />
        <PercentageStat label="Class 2" total={0} />
        <PercentageStat label="Class 3" score={f => f} />
        <PercentageStat label="Class 4" total={} score="0" />
      </div>
    </div>
  )
}
```

이제 앱 보기는 다음과 같습니다.

## 반응 시 PropType 사용

PropType은 부품에 유형 검사를 추가하기 위한 React의 내부 메커니즘입니다.

반응 구성 요소는 `propTypes`라는 특수 속성을 사용하여 유형 검사를 설정합니다.

```js
/**
 * FUNCTIONAL COMPONENTS
 */
function ReactComponent(props) {
  // ...implement render logic here
}

ReactComponent.propTypes = {
  // ...prop type definitions here
}


/**
 * CLASS COMPONENTS: METHOD 1
 */
class ReactComponent extends React.Component {
  // ...component class body here
}

ReactComponent.propTypes = {
  // ...prop type definitions here
}


/**
 * CLASS COMPONENTS: METHOD 2
 * Using the `static` class properties syntax
 */
class ReactComponent extends React.Component {
  // ...component class body here
  
  static propTypes = {
    // ...prop type definitions here
  }
}
```

소품이 Retact 구성 요소로 전달되면 `propTypes` 속성에 구성된 유형 정의와 대조하여 확인됩니다. Prop에 잘못된 값이 전달되면 JavaScript 콘솔에 경고가 표시됩니다.

React 구성 요소에 대해 기본 소품이 설정되어 있으면 `propTypes`에 대해 type을 확인하기 전에 먼저 값이 확인됩니다. 따라서 기본값도 특성 유형 정의의 적용을 받습니다.

`propType` 유형 검사는 개발 모드에서만 수행되므로 개발 중에 Retact 응용 프로그램에서 버그를 잡을 수 있습니다. 성능상의 이유로 프로덕션 환경에서는 트리거되지 않습니다.

## 반응에서 'prop-type' 라이브러리 사용

React 15.5.0 이전에는 React 패키지의 일부로 PropTypes라는 유틸리티를 사용할 수 있었는데, 이 유틸리티는 구성요소 소품에 대한 유형 정의를 구성하는 많은 검증자를 제공했다. 리액트(Ract)로 액세스할 수 있습니다.PropTypes"입니다.

그러나 이후 버전의 React에서는 이 유틸리티가 `prop-type`이라는 별도의 패키지로 이동되었으므로 `PropTypes` 유틸리티에 액세스하려면 프로젝트의 종속성으로 추가해야 합니다.

```undefined
npm install prop-types --save
```

다음과 같이 프로젝트 파일로 가져올 수 있습니다.

```coffeescript
import PropTypes from 'prop-types';
```

`prop-type`을 사용하는 방법과 `Ract`를 사용하는 방법을 자세히 알아보면 됩니다.PropType과 사용 가능한 모든 검증자는 공식 `prop-type` 문서를 참조하십시오.

## 반응 프로프 유형 검증기

PropTypes 유틸리티는 유형 정의를 구성하기 위한 광범위한 검증자를 내보냅니다. 아래에는 기본, 렌더링 가능, 인스턴스, 다중, 컬렉션 및 필수 특성 유형에 대해 사용 가능한 검증자가 나열되어 있습니다.

### 기본형

다음은 기본 데이터 유형에 대한 검증자입니다.

- `PropType.any`: 소품은 모든 데이터 유형일 수 있습니다.
- `PropType.boole`: 받침대는 부울이어야 합니다.
- `PropType.number`: 소품은 숫자여야 합니다.
- `PropTypes.string`: 받침대는 끈이어야 한다.
- `PropTypes.펑크: 소품은 함수여야 합니다.
- `PropType.array`: 받침대는 배열이어야 합니다.
- `PropType.object`: 소품은 물체여야 합니다.
- `PropType.symbol`: 받침대는 기호여야 합니다.

```cpp
Component.propTypes = {
  anyProp: PropTypes.any,
  booleanProp: PropTypes.bool,
  numberProp: PropTypes.number,
  stringProp: PropTypes.string,
  functionProp: PropTypes.func
}
```

### 렌더링 가능 유형

또한 PropType은 Prop에 전달된 값이 React에 의해 렌더링될 수 있도록 다음 검증자를 내보냅니다.

- `PropType.node`: 프로포트는 이러한 유형을 포함하는 숫자, 문자열, 요소 또는 배열(또는 조각)을 사용하여 렌더링할 수 있는 모든 항목이어야 합니다.
- `PropType.Element`: 소자는 반응 요소여야 합니다.

```undefined
Component.propTypes = {
  nodeProp: PropTypes.node,
  elementProp: PropTypes.element
}
```

`PropTypes.Element` 검증기의 일반적인 용도는 구성 요소에 단일 하위 항목이 있는지 확인하는 것입니다. 구성 요소에 하위 항목이 없거나 하위 항목이 여러 개 있는 경우 JavaScript 콘솔에 경고가 표시됩니다.

```undefined
Component.propTypes = {
  children: PropTypes.element.isRequired
}
```

### 인스턴스 유형

특정 JavaScript 클래스의 인스턴스로 prop가 필요한 경우 `PropType.instanceOf` 검증자를 사용할 수 있습니다. 이것은 기본 자바스크립트 `인스턴스 오브 연산자`를 활용한다.

```php
Component.propTypes = {
  personProp: PropTypes.instanceOf(Person)
}
```

### 다중 유형

또한 PropType은 제한된 값 집합이나 여러 데이터 형식 집합을 허용할 수 있는 검증자를 내보낸다. 다음은 다음과 같습니다.

- `PropType.One Of: 프로펠러가 지정된 값 집합으로 제한되어 열거형처럼 처리됩니다.
- `PropType.oneOfType`: 소품은 지정된 형식 집합 중 하나여야 하며 형식 조합처럼 동작해야 합니다.

```undefined
Component.propTypes = {

  enumProp: PropTypes.oneOf([true, false, 0, 'Unknown']),
  
  unionProp: PropTypes.oneOfType([
    PropType.bool,
    PropType.number,
    PropType.string,
    PropType.instanceOf(Person)
  ])
  
}
```

### 컬렉션 유형

PropType.array 및 PropType.object 검증자 외에도 PropType은 어레이 및 개체의 보다 세밀한 검증을 위한 검증자를 제공합니다.

#### 'PropType.arrayOf'

`PropType.arrayOf`는 모든 항목이 지정된 유형과 일치하는 배열임을 보장합니다.

```cpp
Component.propTypes = {

  peopleArrayProp: PropTypes.arrayOf(
    PropTypes.instanceOf(Person)
  ),
  
  multipleArrayProp: PropTypes.arrayOf(
    PropTypes.oneOfType([
      PropType.number,
      PropType.string
    ])
  )
  
}
```

#### 'PropType.objectOf'

PropType.objectOf`는 prop가 모든 속성 값이 지정된 유형과 일치하는 개체인지 확인합니다.

```cpp
Component.propTypes = {

  booleanObjectProp: PropTypes.objectOf(
    PropTypes.bool
  ),
  
  multipleObjectProp: PropTypes.objectOf(
    PropTypes.oneOfType([
      PropType.func,
      PropType.number,
      PropType.string,
      PropType.instanceOf(Person)
    ])
  )
  
}
```

#### "PropType.shape"

객체 소품에 대한 보다 자세한 확인이 필요할 때 `PropType.shape`를 사용할 수 있습니다. 이 명령은 지정된 유형의 값을 가진 지정된 키 집합을 포함하는 개체인지 확인합니다.

```cpp
Component.propTypes = {
  profileProp: PropTypes.shape({
    id: PropTypes.number,
    fullname: PropTypes.string,
    gender: PropTypes.oneOf(['M', 'F']),
    birthdate: PropTypes.instanceOf(Date),
    isAuthor: PropTypes.bool
  })
}
```

#### 'PropType.정확한'

엄격한(또는 정확한) 개체 일치의 경우 다음과 같이 `PropTypes.act`를 사용할 수 있습니다.

```bash
Component.propTypes = {
  subjectScoreProp: PropTypes.exact({
    subject: PropTypes.oneOf(['Maths', 'Arts', 'Science']),
    score: PropTypes.number
  })
}
```

### 필수 유형

지금까지 살펴본 `propType` 검증자는 모두 prop를 선택 사항으로 허용합니다. 그러나 `isRequired`를 모든 승인자에게 체인으로 연결하여 프로포트가 제공되지 않을 때마다 경고가 표시되도록 할 수 있습니다.

```undefined
Component.propTypes = {

  requiredAnyProp: PropTypes.any.isRequired,
  requiredFunctionProp: PropTypes.func.isRequired,
  requiredSingleElementProp: PropTypes.element.isRequired,
  requiredPersonProp: PropTypes.instanceOf(Person).isRequired,
  requiredEnumProp: PropTypes.oneOf(['Read', 'Write']).isRequired,
  
  requiredShapeObjectProp: PropTypes.shape({
    title: PropTypes.string.isRequired,
    date: PropTypes.instanceOf(Date).isRequired,
    isRecent: PropTypes.bool
  }).isRequired
  
}
```

## 유형 검사를 위한 사용자 정의 검증자 반응 도구

일반적으로 구성 요소 소품에 대한 사용자 지정 유효성 검사 논리를 정의해야 합니다. 예를 들어, 소품이 유효한 전자 메일 주소를 전달하는지 확인해야 합니다. `prop-type`을 사용하면 유형 검사 소품에 사용할 수 있는 사용자 지정 유효성 검사 기능을 정의할 수 있습니다.

### 기본 사용자 지정 검증자

사용자 지정 유효성 검사 함수는 다음 세 가지 인수를 사용합니다.

- 모든 소품이 들어 있는 물건인 `➡`은 구성부품에 전달된다.
- `propName`, 검증할 prop 이름
- `componentName`, 구성 요소의 이름

유효성 검사에 실패하면 `오류` 개체를 반환해야 합니다. 오류가 발생하면 안 됩니다. 콘솔도요.사용자 정의 유효성 검사 기능 내에서 warn`을 사용하면 안 됩니다.

```js
const isEmail = function(props, propName, componentName) {
  const regex = /^((([^<>()[]\.,;:s@"]+(.[^<>()[]\.,;:s@"]+)*)|(".+"))@(([[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}])|(([a-zA-Z-0-9]+.)+[a-zA-Z]{2,})))?$/;
  
  if (!regex.test(props[propName])) {
    return new Error(`Invalid prop `${propName}` passed to `${componentName}`. Expected a valid email address.`);
  }
}

Component.propTypes = {
  email: isEmail,
  fullname: PropTypes.string,
  date: PropTypes.instanceOf(Date)
}
```

`PropTypes.oneOfType`에서도 사용자 지정 유효성 검사 기능을 사용할 수 있습니다. 다음은 이전 코드 조각의 `isEmail` 사용자 지정 유효성 검사 기능을 사용하는 간단한 예입니다.

```undefined
Component.propTypes = {
  email: PropTypes.oneOfType([
    isEmail,
    PropTypes.shape({
      address: isEmail
    })
  ])
}
```

`구성요소`는 다음 두 시나리오 모두에서 유효하다.

```xml
<Component email="glad@me.com" />
<Component email={ address: 'glad@me.com' } />
```

### 사용자 지정 검증자 및 컬렉션

사용자 지정 유효성 검사 기능은 `PropType.arrayOf` 및 `PropType.objectOf`에서도 사용할 수 있습니다. 이러한 방식으로 사용할 경우 배열 또는 개체의 각 키에 대해 사용자 지정 유효성 검사 기능이 호출됩니다.

그러나 사용자 정의 유효성 검사 함수는 세 개의 인수 대신 다섯 개의 인수를 사용합니다.

- `propValue`, 배열 또는 개체 자체
- 반복 항목의 현재 키인 `key`
- `componentName`, 구성 요소의 이름
- `위치`, 검증된 데이터의 위치(보통 `prop`)
- `propFullName`은 유효성 검사 중인 현재 항목의 전체 이름입니다. 배열의 경우 배열[인덱스]이고 개체의 경우 개체입니다.핵심

아래는 수집 유형에 사용하기 위해 `isEmail` 사용자 지정 유효성 검사 기능의 수정 버전입니다.

```js
const isEmail = function(propValue, key, componentName, location, propFullName) {
  const regex = /^((([^<>()[]\.,;:s@"]+(.[^<>()[]\.,;:s@"]+)*)|(".+"))@(([[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}])|(([a-zA-Z-0-9]+.)+[a-zA-Z]{2,})))?$/;
  
  if (!regex.test(propValue[key])) {
    return new Error(`Invalid prop `${propFullName}` passed to `${componentName}`. Expected a valid email address.`);
  }
}

Component.propTypes = {
  emails: PropTypes.arrayOf(isEmail)
}
```

### 다목적 사용자 지정 검증자

사용자 지정 검증 기능에 대해 배운 모든 것을 고려하여, 이제 독립 실행형 검증자로 사용할 수 있는 다목적 사용자 지정 검증자를 만들고 컬렉션 유형에서도 사용할 수 있습니다.

isEmail 사용자 지정 검증 기능을 약간 수정하면 아래와 같이 다목적 검증자가 됩니다.

```js
const isEmail = function(propValue, key, componentName, location, propFullName) {
  // Get the resolved prop name based on the validator usage
  const prop = (location && propFullName) ? propFullName : key;
  
  const regex = /^((([^<>()[]\.,;:s@"]+(.[^<>()[]\.,;:s@"]+)*)|(".+"))@(([[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}])|(([a-zA-Z-0-9]+.)+[a-zA-Z]{2,})))?$/;
  
  if (!regex.test(propValue[key])) {
    return new Error(`Invalid prop `${prop}` passed to `${componentName}`. Expected a valid email address.`);
  }
}

Component.propTypes = {
  email: PropTypes.oneOfType([
    isEmail,
    PropTypes.shape({
      address: isEmail
    })
  ]),
  emails: PropTypes.arrayOf(isEmail)
}
```

## 반응에서 '백분율 통계' 확인

다음 코드 조각은 이 튜토리얼의 시작 부분에서 검토한 `백분율통계` 구성 요소에 prop 유형을 추가합니다.

```js
import React from 'react';
import PropTypes from 'prop-types';

// The PercentageStat component
function PercentageStat({ label, score = 0, total = Math.max(1, score) }) {
  return (
    <div>
      <h6>{ label }</h6>
      <span>{ Math.round(score / total * 100) }%</span>
    </div>
  )
}

// Checks if a value is numeric
// Either a finite number or a numeric string
function isNumeric(value) {
  const regex = /^(\+|-)?((\d*\.?\d+)|(\d+\.?\d*))$/;
  return Number.isFinite(value) || ((typeof value === "string") && regex.test(value));
}


// Checks if value is non-zero
// Value is first converted to a number
function isNonZero(value) {
  return +value !== 0;
}


// Takes test functions as arguments and returns a custom validation function.
// Each function passed in as argument is expected to take a value argument
// expected to accept a value and return a Boolean if it passes the validation.
// All tests must pass for the custom validator to be marked as passed.
function validatedType(...validators) {
  return function(props, propName, componentName) {
  
    const value = props[propName];
    
    const valid = validators.every(validator => {
      if (typeof validator === "function") {
        const result = validator(value);
        return (typeof result === "boolean") && result;
      }
      
      return false;
    });
    
    if (!valid) {
      return new Error(`Invalid prop \`${propName}\` passed to \`${componentName}\`. Validation failed.`);
    }
    
  }
}

// Set the propTypes for the component
PercentageStat.propTypes = {
  label: PropTypes.string.isRequired,
  score: validatedType(isNumeric),
  total: validatedType(isNumeric, isNonZero)
}
```

## 결론

본 가이드에서는 반응 구성 요소를 개선하고 예상대로 사용되는지 확인하기 위해 적절한 유형을 사용하는 방법을 살펴 보았습니다.

React에서 구성요소 소품 확인에 대해 자세히 알아보려면 이 안내서를 참조하십시오.

코딩하는 것을 즐기세요…