---
layout: post
title: "정방향 Refin 반응을 사용하여 DOM 정리"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2019/11/forwardRef-react.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2019/11/forwardRef-react.png?fit=730%2C487&ssl=1)

ReactforwardRef는 상위 구성요소가 하위 구성요소를 전달하도록 하는 방법입니다. 정방향 Refin React를 사용하면 하위 구성 요소가 상위 구성 요소에 의해 생성된 DOM 요소를 참조할 수 있습니다. 그러면 어린이가 해당 요소를 읽고 수정할 수 있습니다.

본 튜토리얼에서는 React에서 참조 전달 개념을 살펴보고 DOM과의 상호 작용을 관리하는 데 어떤 도움이 되는지 이해하겠습니다. 보다 몰입적인 경험을 위해, 참조 작성 방법, 작성된 참조를 DOM 요소 및 클래스에 첨부, 전달 참조 방법 등을 다룹니다.

또한 우리는 종종 이미 존재하는 정보를 기반으로 문서 페이지를 참조하고 코드 샌드박스에서 호스팅될 관련 가능한 실제 사례와 조각으로 우리의 개념을 증명한다는 점에 주목할 필요가 있다.

## 반응에서 ForwardRef는 어떻게 작동합니까?

리퍼포워딩을 이해하기 위해서는 먼저 reforward가 무엇인지, reforward가 어떻게 작동하는지 파악하고 몇 가지 사용 사례를 검토해야 합니다. 일반적으로 반응에서 상위 구성요소는 소품을 통해 자녀에게 데이터를 전달합니다.

하위 구성 요소의 동작을 변경하려면 새 소품 집합을 사용하여 렌더링합니다. 어린이 구성 요소가 약간 다른 동작을 보이도록 수정하기 위해, 우리는 상태에 도달하거나 구성 요소를 다시 렌더링하지 않고 이러한 변경을 수행할 수 있는 방법이 필요하다.

우리는 ref를 사용해서 이것을 이룰 수 있다. ref를 사용하면 요소로 대표되는 DOM 노드에 액세스할 수 있습니다. 그 결과, 상태를 건드리지 않고 리렌더를 하지 않고도 수정할 수 있습니다.

refs는 DOM 요소 자체에 대한 참조를 보유하므로, Retact 라이브러리에서 사용할 수 없는 기본 JavaScript 함수를 사용하여 이를 조작할 수 있다. 예를 들어, 버튼을 클릭하면 입력 필드에 포커스를 설정할 수 있습니다.

```js
import ReactDOM from "react-dom";
import React, { Component } from "react";
export default class App extends Component {
  constructor(props) {
    super(props);
    this.myInput = React.createRef();
  }
  render() {
    return (
      <div>
        <input ref={this.myInput} />
        <button
          onClick={() => {
            this.myInput.current.focus();
          }
        >
          focus!
        </button>
      </div>
    );
  }
}
const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

코드 샌드박스에서도 코드를 찾을 수 있습니다.

순수한 JavaScript로 구현하기 위해 다음과 같은 작업을 수행할 수 있습니다.

```coffeescript
document.getElementById('input').focus
```

ref를 통해 우리는 버튼을 클릭할 때마다 입력 요소에 자동으로 초점을 맞추도록 커서를 조작했다. 참조가 없으면 상태를 사용하여 입력 필드가 초점을 맞춰야 하는지 여부를 확인해야 합니다. 즉, 결정을 내리기 전이며, 이러한 경우에는 종종 불필요합니다.

이 정보는 forwardRef를 통해 ref가 가리키는 요소를 내부적으로 정의할 수 있기 때문에 중요합니다.

### 반응에서 ref를 사용해야 하는 경우

리액트에는 `forward ref`를 사용하는 것을 지적할 수 있는 많은 ref들이 있다. 공식 반응 문서에서 볼 수 있듯이 다음과 같은 다양한 작업에 ref를 사용할 수 있습니다.

#### 포커스, 텍스트 선택 또는 미디어 재생 관리

입력 구성 요소를 가지고 있다고 가정해 보겠습니다. 프로그램의 일부에서는 사용자가 단추를 클릭할 때 커서 초점을 맞출 수 있습니다. 구성 요소가 매번 다시 렌더링되도록 하는 상태(소품을 통해)를 변경하는 것보다 상태를 변경하지 않고 (참조를 통해) 입력 구성 요소의 특정 인스턴스만 수정하는 것이 더 타당하다. 마찬가지로, 버튼을 클릭할 때마다(상태 변경) 다시 렌더링하지 않고 참고 자료를 사용하여 음악 또는 비디오 플레이어(일시 중지, 재생, 중지)의 상태를 제어할 수 있다.

#### 값 증가

중간 박수 단추를 생각해 보십시오. 유사한 기능을 구현하는 빠른 방법은 사용자가 박수를 클릭할 때마다 상태에 저장된 카운트 값을 증가시키는 것이다. 그러나 이것은 그리 효율적이지 않을 수 있다. 사용자가 짝짝이 단추를 클릭할 때마다 다시 렌더링되며, 값을 서버에 저장하기 위한 네트워크 요청을 보내는 경우 단추를 클릭하는 횟수만큼 전송됩니다. ref를 사용하면 사용자가 다시 렌더링하지 않고 버튼을 클릭할 때마다 해당 노드를 대상으로 지정할 수 있으며 마지막으로 최종 값으로 하나의 요청을 서버에 전송할 수 있습니다.

#### 필수 애니메이션 트리거 중

우리는 참조(reforwarding)를 사용하여 다음 상태를 위해 자신에게 의존하지만 서로 다른 구성 요소에 존재하는 요소 간에 애니메이션을 트리거할 수 있다(이 개념을 reforwarding이라고 한다). 또한 참조는 타사 DOM 라이브러리와의 통합을 단순화하고 다중 단계 양식 값 상태 관리 등에 사용할 수 있습니다.

### 참조 작성

ref를 생성하기 위해 React는 `Ract.createRef()라는 함수를 제공합니다. 생성된 후에는 ref 속성을 통해 React 요소에 연결할 수 있습니다. 심판은 어느 정도 국가와 유사하다는 점도 주목할 만하다. 구성 요소가 구성되면 해당 구성 요소의 인스턴스(instance) 속성에 ref를 할당하여 구성 요소 내 어디에서나 참조할 수 있도록 합니다.

```js
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.newRef = React.createRef(); //newRef is now available for use throughout our component
  }
 ...
}
```

이때 newRef라는 ref를 만들었습니다. 이 Ref를 구성 요소에 사용하기 위해 다음과 같은 `ref` 속성에 이 값을 전달하기만 하면 됩니다.

```js
class MyComponent extends React.Component {
 ...
  render() {
    return <div ref={this.myRef} />;
  }
}
```

우리는 ref를 여기에 첨부했고 new ref를 가치로 전달했습니다. 그 결과, 우리는 이제 상태를 변경하지 않고 이 정보를 업데이트할 수 있습니다. 자세한 내용을 보려면 createRefin React를 사용하는 방법에 대해 자세히 알아보십시오.

### 참고문헌장

참조는 구성 요소가 렌더링될 때 생성되며 `componentDidMount() 또는 `component()에서 정의할 수 있습니다. 따라서 DOM 요소 또는 클래스 구성 요소에 연결할 수 있지만 인스턴스가 없기 때문에 함수 구성 요소에 연결할 수 없습니다.

정의하는 모든 참조는 DOM의 노드를 나타냅니다. 따라서 `렌더() 함수`에서 해당 노드를 참조하려는 경우 React는 해당 노드를 참조하는 `현재` 특성을 제공합니다.

```cpp
const DOMNode = this.newRef.current; // refers to the node it represents
```

참조 값은 참조하는 노드의 유형(클래스 구성요소 또는 DOM 요소)에 따라 달라집니다.

참조가 참조하는 참조와 노드 유형 및 각 노드 관련 기본값을 더 잘 이해하기 위해 다음 문서를 참조하십시오.

- HTML 요소에 ref 속성을 사용할 때 `React.createRef()`로 생성자에서 생성된 ref는 기본 DOM 요소를 `current` 속성으로 수신합니다.
- 사용자 정의 클래스 구성 요소에 ref 속성을 사용할 때 ref 개체는 구성 요소의 마운트된 인스턴스를 `current` 즉 구성 요소 소품, 상태 및 메서드로 수신합니다.

이 컨셉을 작은 비디오 플레이어로 보여드리겠습니다. 비디오 플레이어에는 일시 중지 및 재생 기능이 있습니다. 계속 빌드하려면 새 CodeSandbox 프로젝트를 생성하고 다음 코드를 추가하십시오.

```js
import ReactDOM from "react-dom";
import React, { Component } from "react";

export default class App extends Component {
  constructor(props) {
    super(props);
    this.myVideo = React.createRef();
  }
  render() {
    return (
      <div>
        <video ref={this.myVideo} width="320" height="176" controls>
          <source
            src="https://res.cloudinary.com/daintu6ky/video/upload/v1573070866/Screen_Recording_2019-11-06_at_4.14.52_PM.mp4"
            type="video/mp4"
          />
        </video>
        <div>
          <button
            onClick={() => {
              this.myVideo.current.play();
            }
          >
            Play
          </button>
          <button
            onClick={() => {
              this.myVideo.current.pause();
            }
          >
            Pause
          </button>
        </div>
      </div>
    );
  }
}
const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

여기에서도 코드를 찾을 수 있습니다.

여기서는 비디오에서 일시 중지 및 재생 방법을 호출하여 비디오 플레이어를 일시 중지 및 재생하는 데 참고를 사용했습니다. 일시 중지 또는 재생 버튼을 클릭하면 다시 렌더링하지 않고 비디오 플레이어에서 기능이 호출됩니다.

#### 기능 구성 요소와 함께 참조 사용

참조는 기능 구성 요소에 부착할 수 없습니다. 단, 참조를 정의하여 DOM 요소 또는 클래스 구성 요소에 첨부할 수 있습니다. 결론은 함수 구성 요소에 인스턴스가 없으므로 참조할 수 없다는 것입니다.

그러나 함수 구성 요소에 참조를 첨부해야 하는 경우 수명 주기 방법 또는 상태가 필요할 때처럼 구성 요소를 클래스로 변환하는 것이 좋습니다.

### 조건부 참조

기본 `ref` 속성을 전달하는 것 외에도, 우리는 ref를 설정하는 기능을 전달할 수 있다. 이 접근 방식의 주요 이점은 참조가 설정되고 설정되지 않은 시기를 더 잘 제어할 수 있다는 것입니다. 그것은 우리에게 특정한 행동이 발사되기 전에 심판의 상태를 결정할 수 있는 능력을 주기 때문에 가능하다. 아래 설명서 페이지의 이 조각을 고려하십시오.

```js
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = null;
    this.setTextInputRef = element => {
      this.textInput = element;
    };
    this.focusTextInput = () => {
      // Focus the text input using the raw DOM API
      if (this.textInput) this.textInput.focus();
    };
  }
  componentDidMount() {
    this.focusTextInput();
  }
  render() {
    return (
      <div>
        <input
          type="text"
          ref={this.setTextInputRef}
        />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focusTextInput}
        />
      </div>
    );
  }
}
```

생성자에서 ref를 정의하는 대신 초기 값을 null로 설정합니다. 이 접근 방식의 장점은 요소가 생성될 때 구성 요소가 로드될 때까지 `textInput`이 노드를 참조하지 않는다는 것입니다.

## 전달 참조 'Forward Ref'를 사용하여 반응하는 전달 참조

하위 구성 요소가 상위 구성 요소 현재 노드를 참조해야 하는 경우 상위 구성 요소는 하위 구성 요소로 참조를 보낼 수 있는 방법이 필요합니다. 이 기술을 리포워딩이라고 합니다.

Refforwarding은 구성 요소를 통해 해당 하위 구성 요소 중 하나에 자동으로 참조 정보를 전달하는 기술입니다. 재사용 가능한 구성요소 라이브러리를 작성할 때 매우 유용합니다. `forward ref`는 참조를 하위 구성 요소로 전달하는 데 사용되는 함수입니다.

```js
function SampleButton(props) {
  return (
    <button className="button">
      {props.children}
    </button>
  );
}
```

샘플 버튼() 구성 요소는 애플리케이션 전체에서 일반 DOM 버튼과 유사한 방식으로 사용되는 경향이 있으므로 해당 DOM 노드에 액세스하는 것은 이와 관련된 포커스, 선택 또는 애니메이션을 관리하는 데 불가피할 수 있습니다.

아래 예에서 `SampleComponent()`는 `Ract.forwardRef`를 사용하여 전달된 참조를 가져온 다음 렌더링하는 DOM 버튼으로 전달합니다.

```js
const SampleButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="button">
    {props.children}
  </button>
));

const ref = React.createRef();
<SampleButton ref={ref}>Click me!</SampleButton>;
```

이제 샘플 버튼 구성 요소를 `ForwardRef` 방법으로 포장했으므로, 이 구성 요소를 사용하는 구성 요소는 DOM 버튼을 직접 사용한 경우와 마찬가지로 기본 버튼 DOM 노드를 참조하고 필요한 경우 액세스할 수 있습니다.

위 코드에 대한 설명은 다음과 같습니다.

- 참조가 필요한 구성 요소에 ref를 정의하여 버튼 구성 요소에 전달합니다.
- 반응이 참조를 전달하여 JSX 속성으로 지정하여 ` as 버튼 ref={ref}`로 전달합니다.
- ref가 연결되면 ref.current는 `<button>` DOM 노드를 가리킵니다.

> SampleButton 구성 요소의 두 번째 기준 인수는 'Ract.forwardRef' 호출로 구성 요소를 정의하는 경우에만 존재합니다.

> 일반 함수 또는 클래스 구성 요소는 참조를 수신하지 않으며 소품에서도 참조를 사용할 수 없습니다.

> RefForwarding은 DOM 구성 요소에 국한되지 않습니다. 클래스 구성 요소 인스턴스로 참조를 전달할 수도 있습니다.

## 결론

ref를 사용하면 상태, 소품 및 리렌더 관리 방법에 있어 더욱 결정적이기 때문에 리액트 코드가 확실히 개선될 것입니다. 본 튜토리얼에서는 reforwarding과 reforwarding의 기본 사항에 대해 다룹니다. 또한 몇 가지 사용 사례와 참조인을 호출할 수 있는 몇 가지 방법을 살펴보았습니다. 참조에 대한 자세한 내용은 여기서 문서를 참조하십시오.