---
layout: post
title: "반응을 위한 사용자 정의 드롭다운 메뉴 구성 요소 빌드"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2018/05/React-custom-dropdown-menu.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2018/05/React-custom-dropdown-menu.png?fit=730%2C487&ssl=1)

기존 구성 요소를 프로젝트에 적응시키는 것이 특정 요구 사항 및 스타일링에 대해 원하는 만큼 항상 원활하게 진행되지는 않을 수 있습니다. 이러한 경우 적응 프로세스에 사용할 수 있는 시간을 고려하여 구성 요소를 직접 구축하는 것이 가장 유리할 수 있습니다.

이 자료에서는 Response에서 사용자 정의 드롭다운 메뉴 구성 요소를 만들기 위해 개인 프로젝트에서 수행한 접근 방식에 대해 설명합니다.

전체 소스 코드 및 스타일링 파일은 GitHubrepo를 참조할 수 있습니다.

## 드롭다운 메뉴 구성요소의 시각적 구조

기술 자료로 들어가기 전에 드롭다운 메뉴 구성요소의 시각적 구조를 빠르게 살펴보고 요구 사항을 결정하겠습니다.

드롭다운 메뉴는 네 가지 기본 구성 요소로 이루어집니다.

- 헤더 래핑
- 헤더 타이틀
- 목록 포장
- 항목을 나열하다

해당 HTML은 다음과 같을 수 있습니다.

```xml
  
<div className="dd-wrapper">
  <div className="dd-header">
    <div className="dd-header-title"></div>
  </div>
  <div className="dd-list">
    <button className="dd-list-item"></button>
    <button className="dd-list-item"></button>
    <button className="dd-list-item"></button>
  </div>
</div>
```

- dd-header 클릭 시 dd-list를 전환하고 dd-wrapper 외부 클릭 시 dd-list를 닫을 수 있어야 합니다.
- 데이터를 기반으로 `< 버튼> 태그를 자동으로 채워야 합니다.
- 헤딩 제목을 동적으로 제어할 수 있어야 합니다.

## 구성 요소의 상위-하위 관계

상위 구성요소는 단일 또는 다중 드롭다운 메뉴를 보유하며, 각 드롭다운 메뉴는 고유한 내용을 가지므로 정보를 소품으로 전달하여 매개 변수를 지정해야 한다.

여러 위치를 선택하는 드롭다운 메뉴가 있다고 가정해 보겠습니다.

상위 구성 요소 내부의 다음 상태 변수를 고려하십시오.

```js
constructor(){
  super()
  this.state = {
    location: [
      {
          id: 0,
          title: 'New York',
          selected: false,
          key: 'location'
      },
      {
          id: 1,
          title: 'Dublin',
          selected: false,
          key: 'location'
      },
      {
          id: 2,
          title: 'California',
          selected: false,
          key: 'location'
      },
      {
          id: 3,
          title: 'Istanbul',
          selected: false,
          key: 'location'
      },
      {
          id: 4,
          title: 'Izmir',
          selected: false,
          key: 'location'
      },
      {
          id: 5,
          title: 'Oslo',
          selected: false,
          key: 'location'
      }
    ]
   }
  }
```

위치 배열을 채울 때 맵 방법의 키 받침대와 함께 사용할 고유한 `id`, 목록의 각 항목에 대한 `제목`, 목록에서 선택한 항목을 전환하기 위해 `선택됨`이라는 부울 변수(드롭 메뉴에서 여러 항목을 선택한 경우), `키` 변수가 있다.

키 변수는 setState 함수와 함께 사용할 때 유용합니다. 그건 나중에 얘기할게요.

이제 우리가 지금까지 소품으로 `드롭다운` 컴포넌트에 대해 설명합니다. 아래에는 상위 구성 요소에 사용되는 `Dropdown` 구성 요소가 나와 있습니다. 여기서는 표시할 제목과 드롭다운 목록을 채울 데이터 배열을 전달했습니다.

렌더링() 방법을 편집하기 전에 드롭다운 구성 요소에 다음과 같은 상태 변수가 필요합니다.

```js
<constructor(props){
  super(props)
  this.state = {
    isListOpen: false,
    headerTitle: this.props.title
  }
}
```

여기서는 메뉴 목록을 전환하기 위한 `isListOpen` 부울 변수와 기본적으로 `제목` 받침대와 같은 `headerTitle` 변수가 있습니다.

이제 우리 부품의 렌더링() 방식을 살펴본다. 렌더 JSX 마크업에 사용되는 `<FontAwesome>`은 외부 NPM 패키지이며 드롭다운 구성 요소 내부에 설치 및 가져와야 합니다.

`react-fontawesome`에서 폰트어썸

또한 프로젝트의 index.html에 다음과 같은 < 태그를 포함해야 합니다. 이것은 FontAwesome이 제대로 작동하기 위해 필요하다.

```xml
<link href="https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css" rel="stylesheet" />
```

여기 렌더 방법에 있습니다. 우리는 앞서 언급한 구조물에 머리글과 목록 항목이 포함되어 있습니다. 렌더링 방법에 사용되는 toggleList() 및 SelectItem() 함수가 보이므로 생성해 보겠습니다.

```xml
render() {
  const { isListOpen, headerTitle } = this.state;
  const { list } = this.props;

  return (
    <div className="dd-wrapper">
      <button
        type="button"
        className="dd-header"
        onClick={this.toggleList}
      >
        <div className="dd-header-title">{headerTitle}</div>
        {isListOpen
          ? <FontAwesome name="angle-up" size="2x" />
          : <FontAwesome name="angle-down" size="2x" />}
      </button>
      {isListOpen && (
        <div
          role="list"
          className="dd-list"
        >
          {list.map((item) => (
            <button
              type="button"
              className="dd-list-item"
              key={item.id}
              onClick={() => this.selectItem(item)}
            >
              {item.title}
              {' '}
              {item.selected && <FontAwesome name="check" />}
            </button>
          ))}
        </div>
      )}
    </div>
  )
}
```

toggleList() 함수는 단순히 isListOpen 상태 변수를 전환하여 항목 목록을 표시하거나 숨기는 것입니다.

```js
toggleList = () => {
   this.setState(prevState => ({
     isListOpen: !prevState.isListOpen
  }))
}
```

반면 선택 항목() 함수는 헤더타이틀 상태를 선택한 항목의 제목으로 설정하고 isListOpen 상태를 false로 설정하여 선택 시 목록을 닫는다.

이러한 상태를 설정한 후 `reset`을 호출합니다.그러면 "Drop down/in"에 전달해야 하는 소품인 콜백 함수 설정()

이것은 다음 장에서 설명될 것이다.

이 콜백 함수를 호출하면 상위 구성 요소의 `위치` 상태가 업데이트되고 클릭한 목록 항목이 선택된 것으로 표시됩니다.

```coffeescript
selectItem = (item) => {
  const { resetThenSet } = this.props;
  const { title, id, key } = item;

  this.setState({
    headerTitle: title,
    isListOpen: false,
  }, () => resetThenSet(id, key));
}
```

## 하위 구성 요소에서 상위 상태 제어

하위 구성 요소에 소품으로 전달할 때 추가 소품을 배포하지 않으면 해당 데이터만 사용할 수 있으며 변경할 수 없습니다. 상태를 제어하는 상위 구성 요소에서 함수를 정의한 다음 이 기능을 하위 구성 요소로 전달하면 하위 구성 요소에서 이 기능을 호출하여 상위 구성 요소의 상태를 설정할 수 있습니다.

드롭다운의 경우 목록 요소를 클릭하면 상위 구성 요소의 `위치` 상태에서 해당 개체에 대해 `선택한` 키를 전환할 수 있어야 한다.

`리셋`을 사용하여 이 작업을 수행합니다.그런 다음 Set() 함수가 드롭다운 구성 요소로 전달되었습니다.

이 함수는 `위치` 상태를 복제한 다음 어레이에서 각 개체의 `선택한` 키를 `거짓`으로 설정한 다음 클릭한 항목의 `선택한` 키만 `참`으로 설정하므로 이름이 재설정됩니다.그다음에 세트.

이 기능은 상위 구성 요소에 정의되어 있습니다.

```coffeescript
resetThenSet = (id, key) => {
  const temp = [...this.state[key]];

  temp.forEach((item) => item.selected = false);
  temp[id].selected = true;

  this.setState({
    [key]: temp,
  });
}
```

그리고 소품으로 <Drop down /> 컴포넌트에 전달되었다.

```cpp
<Dropdown
  title="Select location"
  list={this.state.location}
  resetThenSet={this.resetThenSet}
/>
```

## 단일 또는 다중 선택 드롭다운

이 설정은 단일 선택 드롭다운에 필요합니다. 그러나 드롭다운에서 여러 항목을 선택하려면 `reset` 대신 다른 기능이 필요하다.그 후 설정()

"위치" 배열에 있는 항목의 "선택한" 키만 토글하기 때문에 "togle Item"() 함수 이름을 지정합니다.

```coffeescript
toggleItem = (id, key) => {
  const temp = [...this.state[key]];

  temp[id].selected = !temp[id].selected;

  this.setState({
    [key]: temp,
  });
}
```

그런 다음 이전과 같이 이 기능을 소품으로 전달해야 합니다.

```cpp
<Dropdown
  title="Select location"
  list={this.state.location}
  toggleItem={this.toggleItem}
/>
```

그리고 <Drop down/> 컴포넌트에서 사용할 때는 헤더타이틀을 설정하거나 목록을 닫을 필요가 없다는 점에서 이전과 달리 중간 기능 없이 직접 호출할 수 있다.

하지만, 우리는 얼마나 많은 장소들이 선정되었는지 보여줄 수 있도록 `헤더타이틀`을 여전히 다룰 필요가 있다.

```cpp
render() {
  const { list, toggleItem } = this.props;

  return (
    //
    //
      <button
        type="button"
        className="dd-list-item"
        key={item.id}
        onClick={() => toggleItem(item.id, item.key)}
      >
    //
    //
  )
}
```

### 동적 헤더 제목

앞서 언급했듯이 멀티 셀렉트 드롭다운의 경우 헤더타이틀을 설정하지 않았습니다. 그러나 단일 또는 다중 선택 드롭다운이든 상관없이 전달된 목록 배열에 기본적으로 선택된 키를 true로 설정한 항목이 포함될 수 있기 때문에 헤더타이틀을 별도로 처리해야 한다. 구성 요소는 이를 감지하고 그에 따라 헤더타이틀을 설정할 수 있어야 합니다.

이를 처리하기 위해 우리는 `Static getDerivedStateFromProps` 라이프사이클 후크를 사용할 것이다.

getDerivedStateFromProps의 목적은 소품이 변경됨에 따라 구성 요소가 내부 상태를 업데이트할 수 있도록 하기 위함이다. 상태를 업데이트할 개체를 반환하거나 업데이트할 필요가 없는 경우 null을 반환해야 합니다.

### 싱글 선택 드롭다운 메뉴

먼저 `선택한` 키를 `true`로 설정한 객체가 있는지 확인하기 위해 `list` 받침대를 필터링한다. 있는 경우 반환되며 `선택됨`에서 사용할 수 있습니다.항목. 그런 다음 이 개체의 제목 키를 사용하여 헤더 제목을 설정합니다. `선택한 경우항목이 비어 있으면 반환하고 제목 받침대를 헤더타이틀로 설정하는 데 반대합니다.

```cpp
static getDerivedStateFromProps(nextProps) {
  const { list, title } = nextProps;
  const selectedItem = list.filter((item) => item.selected);

  if (selectedItem.length) {
    return {
      headerTitle: selectedItem[0].title,
    };
  }
  return { headerTitle: title };
}
```

### 다중 선택 드롭다운 메뉴

다중 선택을 처리할 때, 우리는 `선택한` 키가 `true`로 설정된 항목의 길이를 점검한다. 이 카운트가 "0"이면 "헤더타이틀"을 기본 "타이틀"로 설정하기만 하면 됩니다.

카운트가 `1`이면 `제목`이라는 소품을 사용한다.도우미입니다. 이 경우, 이것은 제목에 `1 위치`를 표시하기 위해 `위치`와 같은 문자열 값입니다.

count가 1보다 크면 제목을 통해 우리 부품에 제공하는 location의 복수형을 사용한다.도우미 Plural` 소품. 이 소품들은 우리의 경우 "위치"와 같다.

```js
static getDerivedStateFromProps(nextProps) {
  const {
    list,
    title,
    titleHelper,
    titleHelperPlural
  } = nextProps;

  const count = list.filter((item) => item.selected).length;

  if (count === 0) {
    return { headerTitle: title };
  }
  if (count === 1) {
    return { headerTitle: `${count} ${titleHelper}` };
  }
  if (count > 1) {
    return { headerTitle: `${count} ${titleHelperPlural}` };
  }
  return null;
}
```

따라서 멀티 셀렉트 드롭다운인 경우 구성 요소에는 다음과 같은 소품이 있습니다.

```cpp
<Dropdown
  titleHelper="Location"
  titleHelperPlural="Locations"
  title="Select location"
  list={this.state.location}
  toggleItem={this.toggleItem}
/>
```

## 외부 클릭 처리

마지막으로 처리해야 할 일은 드롭다운을 닫는 것입니다.

window 객체의 클릭 이벤트를 듣고 isListOpen 상태 변수를 전환하는 것은 매우 간단하다. 그러나, 이 접근 방식은 그것을 제대로 작동시키기 위한 몇 가지 작은 속임수를 필요로 한다.

isListOpen 상태 변수에 따라 이벤트 수신기를 `window` 개체에 추가하는 다음 조각을 고려하십시오. 그러나 이 시도를 통해 툴팁이 사실상 동시에 열리고 닫히게 된다.

```coffeescript
close = () => {
  this.setState({
    isListOpen: false,
  });
}

componentDidUpdate(){
  const { isListOpen } = this.state;

  if(isOpen){
    window.addEventListener('click', this.close)
  }
  else{
    window.removeEventListener('click', this.close)
  }
}
```

해결책은 `0` 밀리초 지연이 있거나 시간 지연이 정의되지 않은 `setTimeout` 메서드를 사용하여 다음 이벤트 루프에서 실행할 새 작업을 대기열에 넣는 것이다. `0` 밀리초를 사용하는 것은 일반적으로 즉시 실행되어야 하는 작업을 설명하지만, 자바스크립트의 단일 스레드 동기적 특성에는 해당되지 않는다.

setTimeout이 사용되면 비동기 콜백이 생성됩니다. 주제에 대한 자세한 설명은 특정 MDN 웹 문서를 참조할 수 있습니다.

```coffeescript
componentDidUpdate(){
  const { isListOpen } = this.state;

  setTimeout(() => {
    if(isListOpen){
      window.addEventListener('click', this.close)
    }
    else{
      window.removeEventListener('click', this.close)
    }
  }, 0)
}
```

우리가 고려해야 할 것이 하나 더 있다. 다중 선택 모드에서 드롭다운을 사용할 경우 단일 선택 모드와 달리 항목을 선택한 경우 목록을 닫지 않을 수 있습니다. 이 문제를 해결하려면 목록 항목의 onClick(온클릭) 이벤트에 대해 전파 중지() 방법을 호출해야 합니다.

이렇게 하면 동일한 이벤트가 상위 요소로 번지는 것을 방지하므로 항목을 클릭할 때 항목 목록이 열린 상태로 유지됩니다.

```xml
<button
  type="button"
  className="dd-list-item"
  key={item.id}
  onClick={(e) => {
    e.stopPropagation();
    this.selectItem(item);
  }
>
```

## 결론

본 튜토리얼에서는 단일 및 다중 선택 기능을 모두 지원하는 드롭다운 메뉴 구성 요소를 구성하였다. 우리는 소품으로서의 기능을 어린이 구성 요소로 전달하여 어린이 구성 요소 내부에서 호출함으로써 어린이 구성 요소의 상태를 제어하는 방법을 배웠다.

또한, 우리는 소품 변경 시 상태 변수를 업데이트하기 위해 정적 `GetDerivedStateFromProps` 방법을 사용했다.

이 튜토리얼에서는 사용자 정의 드롭다운 메뉴를 만드는 방법에 대한 소개 접근 방식을 제공합니다. 완전히 다듬어진 드롭다운 구성 요소를 생성하려면 접근성도 염두에 두어야 합니다.