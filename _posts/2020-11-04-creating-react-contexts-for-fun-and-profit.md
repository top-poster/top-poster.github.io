---
layout: post
title: "재미와 이익을 위한 리액트 컨텍스트 생성"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/creatingcontextsforfunandprofit.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/creatingcontextsforfunandprofit.png?fit=730%2C487&ssl=1)

리액션은 항상 장점과 단점을 가지고 있다. 믹신(mixin)이 유해하다고 여겨지지 않았던 좋은 시절, 가장 큰 불만 중 하나는 중요한 것을 저장하는 앱의 맨 위에서 데이터를 제공하는 나뭇잎 구성 요소로 공유 상태를 전달해야 한다는 것이었습니다.

이를 해결하기 위해, 비공식적인, 사용하지 않는, 매우 비공식적인 컨텍스트 API라고 불리는 것이 있었고, Redux와 같은 일부 라이브러리들은 이 기능을 큰 성공을 위해 사용했다. 기록되었음에도 불구하고, 미리 크고 뚱뚱한 경고와 함께 왔습니다. 그러면 안 될 것입니다. 원래의 API는 투박하고 사용하기 어려웠으며, 결코 사용자 대면 기능을 의미하지 않았다. 필요에 따라 추가되었지만 항상 사용되지 않도록 예약되었습니다.

오늘날에는 더 나은 도구를 마음대로 사용할 수 있습니다. 새로운 컨텍스트 API는 16.3.0에 출시되었으며 컨텍스트 제공자 및 소비자라는 개념과 함께 제공되었다. 16.8에 Hooks가 추가됨에 따라 Hook을 통해 컨텍스트를 소비할 수 있게 되어 개발자 인체공학이 한층 더 향상되었습니다.

이 기사는 어떤 맥락이 있는지, 언제 그들에게 다가가야 하는지, 그리고 그것들을 만드는 가장 좋은 방법에 대해 자세히 알아보려고 합니다.

## 정말로 문맥이란 무엇인가?

난 항상 맥락을 리액트의 웜홀이라고 표현해왔어 일부 값을 이 값 안에 전달하여 일부 구성 요소 트리를 중첩한 다음 이러한 값을 소품으로 전달하지 않고도 검색할 수 있습니다. 정말 마술이에요.

컨텍스트를 생각하는 또 다른 방법은 해당 컨텍스트가 코드의 "바로 가기"로 작동한다는 것입니다. 보다 원활한 API를 생성하고 구성 요소 구조를 데이터 흐름에서 분리할 수 있습니다.

그러나 컨텍스트를 생각해 보면 React에서 매우 유용한 기능입니다. 제 생각에는 자주 사용하지 않는 기능인 것 같아.

## 당신은 언제 문맥을 찾아야 합니까?

컨텍스트 API는 소품으로 직접 값을 전달하지 않으려는 경우에 사용할 수 있는 훌륭한 도구입니다. 그것을 위한 몇 가지 활용 사례가 있다.

### 공유 상태가 있는 경우

컨텍스트는 애플리케이션 전체에서 공유해야 하는 상태가 있는 경우에 적합한 도구입니다. 예를 들어, 현재의 언어 또는 색 구성표는 이러한 종류의 상태를 보여주는 좋은 예이다.

또한 애플리케이션의 특정 부분에 대해 더 낮은 수준의 컨텍스트를 사용할 수 있으므로 `index.tsx` 파일에 모든 Provider가 있을 필요는 없습니다.

### 복합 구성요소를 작성하는 경우

때로는 함께 사용할 구성 요소를 만들기도 합니다. 두 사람 간에 정보를 공유해야 하지만, 모든 사람에게 동일한 정보를 전달하려고 하지는 않습니다.

제가 가장 좋아하는 예는 양식 컨트롤과 ID입니다. 레이블은 설명하는 입력 필드의 ID를 알아야 하며, 오류 메시지나 추가 설명 요소도 정확한 영역 태그를 설정하는 데 필요합니다. 수동으로 전달하는 것은 오류가 발생하기 쉬운 프로세스이며, 최상위 구성요소에 전달하는 것이 훨씬 더 바람직할 것이다.

## 단일 파일 "text-n-Hook"

좋아요, 그래서 우리는 문맥이 무엇이고 언제 그것이 좋은 도구인지 다루었습니다. 하지만 그것들을 구성하는 가장 좋은 방법은 무엇일까요?

나는 켄트 C의 이 멋진 기사를 우연히 발견했다. 얼마 전까지만 해도 이런 접근 방식은 그 기사의 영향을 많이 받았다. 확실히 읽을 것을 권합니다.

이 기술은 컨텍스트 제공자와 소비자 후크라는 두 가지 항목만 표시하는 단일 파일을 만듭니다. 기본 컨텍스트 개체는 노출되지 않으며, 작업을 잊었을 경우를 대비한 예비 값도 없습니다.

다음은 언어 전환기를 만드는 예입니다.

```php
const LanguageContext = React.createContext();

export const LanguageProvider = ({ 
  defaultLanguage = 'en', 
  children 
}) => {
  const [language, setLanguage] = React.useState(props.defaultLanguage);
  const contextValue = React.useMemo(
    () => ({ language, setLanguage }), 
    [language]
  );
  return (
    <LanguageContext.Provider value={contextValue}>
      {props.children}
    </LanguageContext.Provider>
  );
}

export const useLanguage = () => {
  const context = React.useContext(LanguageContext);
  if (!context) {
    throw new Error("You have to wrap this component in a LanguageProvider");
  }
  return context;
}
```

많은 코드였습니다. 이제 차근차근 살펴보겠습니다.

```cpp
const LanguageContext = React.createContext();
```

여기서 우리는 문맥 자체를 창조합니다. 우리는 문맥에 기본값을 전달하지 않습니다. 이것은 의도적인 선택입니다. 디폴트 값이 타당할 수 있는 경우가 있기는 하지만, 저는 일반적으로 피합니다. 이렇게 하면 개발자에게 앱의 일부를 공급자에서 포장하는 것을 잊어버렸다고 경고할 수 있습니다.

```js
export const LanguageProvider = ({ 
  defaultLanguage = 'en', 
  children 
}) => {
  const [language, setLanguage] = React.useState(defaultLanguage);
  const contextValue = React.useMemo(
    () => ({ language, setLanguage }), 
    [language]
  );
  return (
    <LanguageContext.Provider value={contextValue}>
      {props.children}
    </LanguageContext.Provider>
  );
}
```

이것은 공급자 구성 요소입니다. 그 책임은 우리가 물려주고 있는 상태를 억제하고 마지막 단계에서 우리가 만든 문맥에 그 가치를 전달하는 것입니다.

상황별 값을 `Ract.use Memo Hook`에 대한 호출로 묶고 있는 것을 알 수 있습니다. 불필요한 재접속을 방지하기 위해 이 작업을 수행합니다. 건너뛰면 이 컨텍스트의 모든 소비자가 이 제공자를 다시 렌더링할 때마다 다시 렌더링됩니다. 그리고 너무 사전 최적화해서는 안 된다 하더라도 위와 같은 use Memo로 문맥을 포장하는 것은 대개 안전한 선택이다.

리렌더 수를 최적화하는 것도 좀 지나칠 수 있습니다. 값을 표시하는 대신 값 변경만을 처리하는 구성 요소가 다시 렌더링되지 않도록, 이론적으로 두 개의 다른 컨텍스트를 각각 하나씩 및 업데이트 기능에 대해 생성할 수 있습니다. 실제 사용 사례를 찾아본 적은 없지만, 알고 보면 멋진 속임수입니다. 인터넷에 그렇게 적혀있으니까 하지 마세요.

```js
export const useLanguage = () => {
  const context = React.useContext(LanguageContext);
  if (!context) {
    throw new Error("You have to wrap this component in a LanguageProvider");
  }
  return context;
}
```

퍼즐의 마지막 부분은 소비자 후크이다. 여기서는 `React.useContext` 후크를 사용하여 컨텍스트 값을 검색합니다. 우리는 이 후크를 사용하는 개발자가 그것을 공급자 구성 요소로 포장하는 것을 기억했는지 확인하기 위해 수표를 추가한 다음, 단지 컨텍스트를 반환한다.

저는 "원시" 값과 업데이트 프로그램 기능을 컨텍스트를 통해 전달하고 후크에 더 나은 추상화를 만드는 것을 좋아합니다. 이렇게 하면 동일한 API를 기반으로 새로운 Hook을 만들 수 있고 최종 API는 제공자가 아닌 Hook에게 맡깁니다. 하지만 이 예에서는 그럴 필요가 별로 없었습니다!

이러한 부분을 사용하면 매우 간결한 API를 통해 정말 멋진 컨텍스트를 만드는 데 필요한 모든 것을 얻을 수 있습니다. 노출되는 항목은 컨텍스트 제공자 및 해당 값을 반환하기 위한 후크라는 두 가지 항목입니다. 정말 멋지네요!

나는 이 기사가 당신에게 문맥을 다루기 위한 몇 가지 새로운 기술을 주었기를 바랍니다. 읽어주셔서 감사합니다!