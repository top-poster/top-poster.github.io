---
layout: post
title: "DOM 조작 방법-궁극의 초보자 가이드
 "
author: 'Code Tower'
thumbnail: https://images.unsplash.com/photo-1581291518857-4e27b48ff24e?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MXwxMTc3M3wxfDF8c2VhcmNofDZ8fGhlbHB8ZW58MHx8fA&ixlib=rb-1.2.1&q=80&w=2000
tags: undefined
---


좋아, 그래서 나는 당신이 전능 한 DOM에 대해 들어 보았다고 가정한다. 그래서 당신이 여기있는 이유 다.
 어렵게 느껴진다면이 기사를 읽은 후 DOM 조작에 대해 충분히 편안하게 느끼실 수 있습니다.
 

하지만 시작하기 전에 제가 DOM에 대해 알게 된 방법에 대한 저의 작은 이야기를 들려주세요 (재미있는 이야기).
 

## DOM에 대해 배운 방법
 

웹 개발 경력을 쌓은 지 몇 달이 지났지 만 여전히 좋은 HTML과 CSS를 배우고있었습니다.
 w3schools에서 DOM 과정을 실수로 우연히 발견했습니다.
 첫 번째 예는 전구 하나와 버튼 두 개였습니다.
 

버튼 중 하나를 클릭하면 전구가 "켜지고"두 번째 버튼을 클릭하면 전구가 "꺼집니다".
 나는 말 그대로 날아 갔다.
 

웹 사이트의 버튼이 어떻게 전구를 켤 수 있습니까?
 어떻게!?
 

나는 그것에 대해 트위트했다.
 그런 다음 이미지의 소스 속성 (src)을 변경하고 있다는 것을 알았습니다.
 나는 마음이 아팠지만 그 작은 경험에도 불구하고 나는 DOM과 사랑에 빠졌습니다.
 더 알고 싶게 만들었습니다.
 

그리고이 기사에서 나는 당신에게 그것을 안내 할 것입니다.
 끝까지 나와 함께하고 내가 작성한 모든 내용을 연습한다면 전체 DOM 문제가 다시는 문제가되지 않을 것이라고 약속합니다.
 준비 되셨나요?
 Ok Allons-y (가자!).
 

> 이해하기 쉽도록 모든 것을 아래 섹션으로 그룹화했습니다.
 

- DOM 및 기본 개념의 정의
 
- DOM에서 요소를 선택하는 방법
 
- DOM을 순회하고 이동하는 방법
 
- DOM에서 요소를 조작하는 방법
 
- 일반 스타일링
 
- DOM에서 이벤트 처리
 

그러니 커피 나 좋아하는 것을 마시고 각 섹션을 안내하면서 휴식을 취하십시오.
 

## DOM 및 기본 개념의 정의
 

### DOM이란 무엇입니까?
 

DOM은 Document Object Model을 나타냅니다.
 브라우저에 의해 생성 된 노드 트리로 간단히 이해할 수 있습니다.
 이러한 각 노드에는 JavaScript를 사용하여 조작 할 수있는 고유 한 속성과 메서드가 있습니다.
 

DOM을 조작하는 능력은 JavaScript의 가장 독특하고 유용한 능력 중 하나입니다.
 

아래 이미지는 DOM 트리의 모양을 시각적으로 보여줍니다.
 

여기에 문서 객체가 있습니다.
 이것이 DOM의 핵심 / 기초입니다.
 모든 형태의 DOM 조작을 수행하려면 먼저 문서 객체에 액세스해야합니다.
 

다음으로 문서 객체의 자식 인`html` 루트 요소가 있습니다.
 

다음 줄에는 서로 형제 인`body` 및`head` 요소와`html` 요소의 하위 요소가 있습니다.
 

head 요소 아래에 동의 할 수있는 title 요소가 head 요소의 자식이고 텍스트 노드의 부모 인 "my text"라는 점에 동의 할 수 있습니다.
 

body 요소 바로 아래에는 서로 형제이며 body 요소의 자식 인 두 요소 (`a` 태그 및`h1` 태그)가 있습니다.
 

마지막으로 `href`속성과 텍스트 노드 인 `my link`는 `a`태그의 하위 항목입니다.
 텍스트 노드 `My header`가 `h1`요소의 자식 인 것과 똑같은 방식입니다.
 

당신이 완전 초보자라면 이것은 약간 혼란스러워 보일 수도 있지만, 저를 믿으십시오. (물론 연습을 통해) 항상 좋아집니다.
 

## DOM에서 요소를 선택하는 방법
 

DOM에서 요소를 조작 할 수 있으려면 특정 요소를 선택해야합니다.
 운 좋게도 요소를 선택하는 4 가지 주요 방법이 있습니다.
 

### getElementById 메서드로 DOM 요소를 선택하는 방법
 

HTML 요소에 액세스하는 가장 일반적인 방법은 요소의 ID를 사용하는 것입니다.
 

아래 예에서`getElementById ()`메소드는 id = "master"를 사용하여 요소를 찾습니다.
 

```js
<p id="master">i love javascript</p>

 <script>
const masterEl = document.getElementById('master')
console.log(masterEl) //<p id="master">i love javascript</p> 
 </script>
```

ID는 대소 문자를 구분합니다.
 예를 들어 `마스터`와 `마스터`는 완전히 다른 ID입니다.
 

요소를 선택한 후에는 요소에 스타일을 추가하고, 해당 속성을 조작하고, 상위 및 하위 요소로 트래버스 할 수 있습니다.
 

### getElementsByClassName () 메서드를 사용하여 DOM 요소를 선택하는 방법
 

이 메서드는 지정된 클래스 이름을 가진 문서의 모든 요소 컬렉션을 반환합니다.
 

예를 들어 아래의 HTML 페이지에는 class = "master2"가 포함 된 3 개의 요소가 포함되어 있으며 ID가 `btn`인 버튼을 선택했습니다.
 

버튼을 클릭하면 클래스 이름이 "master2"인 모든 요소가 선택되고 세 번째 요소의 innerHTML이 변경됩니다.
 

```js
        <p class="master2">i love javascript</p>
        <p class="master2">i love react</p>
        <h1 class="master2">i want a job</h1>

        <button id="btn">click me</button>
 
 <script>
 
 const btn = document.getElementById('btn')
        
        btn.addEventListener('click', function master(){
            var master = document.getElementsByClassName("master2");
            master[2].innerHTML = 'i need a job';
        })

</script>
```

버튼을 클릭하기 전에 다음과 같이 표시됩니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/22.png)

버튼을 클릭하면 다음이 표시됩니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/444.png)

> 아직 설명하지 않은`addEventListener ()`를 사용했다는 것을 알고 있지만 저를 고수하십시오.
 아래에서 설명 할 내용의 일부입니다. :)
 

### getElementsByTagName () 메서드로 DOM 요소를 선택하는 방법
 

이 메서드는 태그 이름을 받아들이고 지정된 태그 이름의 모든 요소를 문서에 나타나는 순서대로 반환합니다.
 

다음 코드는 페이지의 모든`p` 요소를 가져오고 두 번째 요소의 내용을 변경하여`getElementsByTagName ()`의 구문을 보여줍니다.
 

```js
 <p>VsCode</p>
 <p>Atom</p>
 <p>Sublime text</p>
        <button id="btn">click me</button>
       

 <script>

const btn = document.getElementById('btn')
        
        btn.addEventListener('click', function master(){
            let master = document.getElementsByTagName('p');
            let masterEl = master[1].innerHTML = 'Code editors';
            console.log(masterEl) //Code editors
        })

//<p>Atom</p> changes to <p>Code editors</p>
</script>
```

### CSS 선택기로 DOM 요소를 선택하는 방법
 

주어진 선택자와 일치하는 첫 번째 값을 반환합니다.
 이 메서드는 모든 CSS 스타일 선택자를 허용하여 태그, 클래스 또는 ID별로 선택할 수 있습니다.
 

```js
<div id=master>i am a frontend developer</div>

<script>
const master = document.querySelector("#master") 
</script>
```

위의 메서드는 CSS 선택자인 하나의 인수를 취하고 선택자와 일치하는 첫 번째 요소를 반환합니다.
 

이것은 모든 일치하는 요소의 노드 목록 컬렉션을 반환하는 위와 유사하게 작동합니다.
 

```undefined
     <p class="master">React</p>
     <p class="master">Vue</p>
     <p class="master">Angular</p>

<script>
const master = document.querySelectorAll(".master") 
console.log(master[1])  //<p class="master">Vue</p>
</script>
```

### DOM 요소 선택 방법 요약
 

DOM 요소를 선택해야 할 때 선택할 수있는 4 가지 옵션, 특정 작업을 수행하는 4 가지 방법 (요소 선택)이 있습니다.
 

따라서 첫 번째를 기억하지 못하면 두 번째를 사용합니다.
 그리고 우연히 둘 다 기억하지 못한다면 여전히 옵션 3과 4가 있습니다. 그것은 나뿐입니까 아니면 JavaScript가 우리 삶을 더 쉽게 만들어 줄까요?
 :)
 

내 개인적인 권장 사항은 옵션 1 또는 옵션 4a (ID가있는 쿼리 선택기)를 고수하는 것입니다.
 HTML을 배우는 초창기부터 요소가 동일한 ID를 가져서는 안된다는 것을 이해했을 것입니다. 즉, ID는 문서 내 요소의 고유 식별자입니다.
 

이를 염두에두고 ID가있는 요소를 선택하는 것은 "안전한 내기"입니다. 동일한 "조작"을 다른 요소에 적용 할 수 없기 때문입니다.
 다른 옵션 사용).
 

## 문서를 탐색하는 방법
 

이 단계에서 당신은 HTML 문서의 모든 것이 노드라는 것에 동의 할 것입니다.
 또한 HTML 요소 내부의 텍스트는 텍스트 노드입니다.
 

HTML DOM을 사용하면 앞에서 설명한 노드 관계 (부모, 자식, 형제 등)를 사용하여 노드 트리를 탐색하고 트리의 노드에 액세스 할 수 있습니다.
 

> 새 노드를 만들 수 있으며 모든 노드를 수정하거나 삭제할 수 있습니다.
 

### 약간의 검토
 

- 모든 노드에는 상위 노드 (부모가 없음)를 제외하고 정확히 하나의 상위 노드가 있습니다.
 
- 노드는 둘 이상의 자식을 가질 수 있습니다.
 
- 형제 (형제 또는 자매)는 부모가 동일한 노드입니다.
 

이 섹션에서는 부모 요소, 요소의 형제 및 요소의 자식을 얻는 방법을 살펴 보겠습니다.
 이러한 작업을 수행하기 위해 다음 노드 속성을 사용합니다.
 

- parentNode
 
- childrenNodes
 
- firstElementChild
 
- lastElementChild
 
- nextElementSibling
 
- previousElementSibling
 

또한 아래의이 HTML 페이지 만 사용하여 이러한 각 노드 속성을 사용하는 방법을 보여 드리겠습니다.
 그리고 위의 섹션 4에서 DOM을 조작하는 방법을 보여 드리겠습니다.
 

이것이이 기사의 목적-DOM 조작 방법을 아는 것입니다.
 요소를 선택하고 DOM을 조작하는 방법을 모르는 경우 DOM을 탐색하는 방법을 알고 있는지 여부는 중요하지 않습니다.
 CSS 스타일을 추가하고, 요소를 만들고 추가하고, innerHTML을 설정하고, 이벤트를 처리하는 방법을 아는 것이 중요합니다.
 

그게이 기사의 핵심 이니 제발 저와 함께있으세요.
 계속합시다.
 

```js
 <div id="parent">
        <div id="firstchild">i am a first child</div>
        <p id="secondchild">i am the second child</p>
        <h4>i am alive</h4>
        <h1>hello world</h1>
        <p>i am the last child</p>
    </div>  
    
    const parent = document.getElementById('parent').lastElementChild
    console.log(parent) //<p>i am the last child</p>
    
    const parent2 = document.getElementById('parent').children[3]
    console.log(parent2) //<h1>hello world</h1>
    
    const secondchild = document.getElementById('secondchild')
    console.log(secondchild) //<p id="secondchild">i am the second child</p>
    
    console.log(secondchild.parentNode) //<div id="parent">...</div>

    console.log(secondchild.nextElementSibling) //<h4>i am alive</h4>

    console.log(secondchild.previousElementSibling) //<div id="firstchild">i am a first child</div>
```

## DOM에서 요소를 조작하는 방법
 

이 섹션에서는 다음을 살펴볼 것입니다.
 

- 요소를 만드는 방법
 
- 요소의 innerHTML / 텍스트 내용을 설정하는 방법
 
- 요소를 추가하는 방법
 
- 한 요소를 다른 요소 앞에 삽입하는 방법
 
- 자식 요소를 바꾸는 방법
 
- 자식 요소를 제거하는 방법
 

```html

    <div id="parent">
        <div id="firstchild">i am a first child</div>
        <p id="secondchild">i am the second child</p>
        <h4>i am alive</h4>
        <h1>hello world</h1>
        <p>i am the last child</p>
    </div>  

```

### 요소를 만드는 방법
 

위의 코드는 5 개의 하위 요소가있는 상위 요소를 보여줍니다.
 자바 스크립트로 다른`div` 태그를 추가하고 싶다고 가정 해 보겠습니다.
 다음과 같이`createElement ()`메서드를 사용하여 새 요소를 만들어야합니다.
 

```js
 const createEl = document.createElement('div')
 console.log(createEl) //<div></div>
```

### innerHTML 설정 방법
 

`div` 태그를 성공적으로 만들었지 만 현재는 텍스트 노드가 없습니다.
 `.innerHTML ()`속성을 사용하여 텍스트 노드를 추가 할 것입니다.
 

```js
 const innerhtml = createEl.innerHTML = 'i am a frontend developer'
 console.log(createEl) //<div>i am a frontend developer</div>

```

### 요소를 추가하는 방법
 

지금까지 달성 한 것은 요소를 만들고 텍스트 노드를 삽입하는 것입니다.
 그러나이 생성 된 요소는 아직 DOM 트리의 일부가 아닙니다.
 

이제이 섹션의 HTML 페이지에 추가하는 방법을 보여 드리겠습니다.
 위의 코드를 기반으로 작성 :
 

```js
 const createEl = document.createElement('div')

 const innerhtml = createEl.innerHTML = 'i am a frontend developer'

 const parentEl = document.getElementById('parent')

 parentEl.appendChild(createEl)

 console.log(parentEl) 

```

![image](https://www.freecodecamp.org/news/content/images/2021/01/Document---Google-Chrome-16_01_2021-11_50_14-PM--2-.png)

### 한 요소를 다른 요소 앞에 삽입하는 방법
 

위의 콘솔 로그 이미지에서 알 수 있다면 추가 된 하위`div` 태그가 하단에 자동으로 추가되었습니다.
 

어떤 이유로 든 원하는 곳에 추가하려면 어떻게해야합니까?
 첫 번째 요소 이전 또는 네 번째 요소 이전 일 수 있습니다.
 나는 그것이 매우 가능하다는 것을 당신에게 말하기 위해 여기에 있습니다.
 아래 코드에서 현재 첫 번째 요소 앞에 추가 할 것입니다.
 

두 개의 매개 변수 인`newNode`와`existingNode`를 다음 순서로 받아들이는`insertBefore ()`JavaScript 메소드를 사용할 것입니다 =>`document.insertBefore (newNode, existingNode)`.
 

```js
 const parentEl = document.getElementById('parent')
 const firstchildEl = document.getElementById('firstchild')
 
 const createEl = document.createElement('div')

 const innerhtml = createEl.innerHTML = 'i am a frontend developer'

 parentEl.insertBefore(createEl, firstchildEl)
   console.log(parentEl)

```

![image](https://www.freecodecamp.org/news/content/images/2021/01/mmm.png)

### 자식 요소를 바꾸는 방법
 

첫 번째 요소를 새로 만든 요소로 대체하기 위해 두 개의 매개 변수를받는`replaceChild ()`JavaScript 메서드를 사용할 것입니다.
 이 순서대로 작동합니다 =>`document.replaceChild (newNode, existingNode)`.
 

```js
 const firstchildEl = document.getElementById('firstchild')
 const parentEl = document.getElementById('parent')

 const createEl = document.createElement('div')
 const innerhtml = createEl.innerHTML = 'i am a frontend developer'

 parentEl.replaceChild(createEl, firstchildEl)

   console.log(parentEl)

```

![image](https://www.freecodecamp.org/news/content/images/2021/01/kkk.png)

### 자식 요소를 제거하는 방법
 

제거하려는 요소 인 하나의 매개 변수 () 만 받아들이는`removeChild ()`JavaScript 메서드를 사용할 것입니다.이 경우에는 원래 첫 번째 요소입니다.
 이 순서대로 작동합니다 =>`document.removeChild (element)`
 

```js
const firstchildEl = document.getElementById('firstchild')
 const parentEl = document.getElementById('parent')
 
 parentEl.removeChild(firstchildEl)

 console.log(parentEl)
```

![image](https://www.freecodecamp.org/news/content/images/2021/01/vvv.png)

## CSS로 스타일링을 추가하는 방법
 

이전 예제에서 요소를 생성하고 지정된 부모 요소에 추가하는 방법을 보았습니다.
 

따라서 요소가 스타일을 가지려면 CSS 클래스를 추가해야합니다.
 이 경우 JavaScript로 수행합니다.
 

수업을 추가하는 방법 만 보여 드리는 것이 아닙니다.
 또한 클래스를 제거하는 방법과 클래스간에 전환하는 방법도 보여 드리겠습니다.
 

걱정하지 마세요. 어렵지 않습니다.
 나는 당신에게 모든 것을 안내하기 위해 여기에 있습니다.
 

### CSS 클래스를 추가하는 방법
 

현재 우리는 ID가 "master"이지만 스타일이 적용되지 않은 일반 HTML 버튼을 가지고 있습니다.
 아래 이미지를 참조하십시오.
 

가장 먼저 할 일은 버튼의 CSS 스타일을 만드는 것입니다.
 

다음으로 JavaScript에서 단추에 이벤트 리스너를 추가하여 단추를 클릭하면 JavaScript가 "button"클래스로 CSS 스타일을 자동으로 추가합니다.
 

```js
 <style>
        body{
            background-color: hotpink;
            display: flex;
            align-items: center;
        }

        .button{
            background-color: blueviolet;
            width: 200px;
            border: none;
            font-size: 2rem;
            padding: 0.5rem;
            border-radius: 5px;
            cursor: pointer;
        }
    </style>
    

  <button id="master">Click me</button>
  
    
const buttonEl = document.getElementById('master')
buttonEl.addEventListener('click', addFunction)

 function addFunction(){
     buttonEl.classList.add('button')
  }
```

버튼을 클릭하면 아래와 같이 표시됩니다.
 아름답 죠?
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/jjj.png)

### 클래스를 제거하는 방법
 

위의 동일한 예제를 사용하여 이번에는 JavaScript에서`classList.remove ()`를 사용하여 CSS 스타일을 제거합니다.
 무슨 일이 일어날 지 이미 짐작 하셨 겠죠?
 

정확히 버튼이 기본 상태로 돌아갑니다.
 

```js

const buttonEl = document.getElementById('master')
buttonEl.addEventListener('click', addFunction)

function addFunction(){
    buttonEl.classList.remove('button')
 }
 
```

### 클래스를 전환하는 방법
 

CSS 스타일을 완전히 제거하고 싶지 않다고 가정 해 보겠습니다.
 스타일이 지정된 버튼과 스타일이 지정되지 않은 버튼 사이를 전환하는 방법을 원합니다.
 

`classList.toggle ()`JavaScript 메서드는 이러한 기능을 제공합니다.
 

`classList.toggle ()`메소드는 일반적으로 Twitter와 같은 대부분의 소셜 미디어 플랫폼에서 사용됩니다.
 버튼이있는 게시물을 좋아하고 원할 때마다 같은 버튼과 달리 게시물을 좋아할 수 있습니다.
 

따라서 JavaScript는 버튼에 CSS 클래스가 있는지 확인합니다.
 

클래스가 있고 버튼을 클릭하면 제거됩니다.
 클래스가없는 상태에서 버튼을 클릭하면 추가됩니다.
 

```js

const buttonEl = document.getElementById('master')
buttonEl.addEventListener('click', addFunction)


function addFunction(){
    buttonEl.classList.toggle('button')
 }
 
```

### HTML 이벤트 란 무엇입니까?
 

HTML 이벤트는 버튼 클릭, 텍스트 영역 입력 등과 같은 HTML 요소에 발생하는 "사물"입니다.
 위와 같은 이벤트가 발생하면 실행될 이벤트 핸들러를 호출하는 JavaScript 코드를 작성할 수 있습니다.
 

이러한 이벤트 핸들러는 JavaScript 함수입니다.
 따라서 요소에서 이벤트가 발생하면 핸들러 함수가 실행됩니다.
 

### 이벤트 리스너
 

지금까지 기본적으로 위의 모든 예제에서 이벤트 리스너를 사용했습니다.
 이것은 이벤트 리스너가 DOM을 조작하는 데 얼마나 중요한지를 보여줍니다.
 

요소 또는 DOM 객체에 이벤트 리스너를 추가하려면`addEventListener ()`메소드가 필요합니다.
 이 방법은 HTML 마크 업에서 처리 할 이벤트를 포함하는 이전 방법보다 선호됩니다.
 

이를 통해 JavaScript는 html 마크 업과 분리되어 더 깔끔하고 읽기 쉽습니다.
 

나는 별도의 JS, 별도의 CSS 등의 아이디어를 좋아하므로 나와 같은 경우이 이벤트 리스너를 원합니다.
 

이벤트 리스너는 3 개의 매개 변수를받습니다.
 

- 첫 번째는 "클릭"등과 같은 이벤트 유형입니다.
 
- 두 번째 매개 변수는 실행할 기능입니다.
 
- 세 번째 매개 변수는 이벤트 버블 링 또는 이벤트 캡처를 사용할지 여부를 지정하는 부울 값입니다.
 이 매개 변수는 선택 사항입니다.
 

> 하나의 요소에 여러 이벤트 핸들러를 추가 할 수 있습니다.
 

> 두 개의 "클릭"이벤트와 같이 하나의 요소에 동일한 유형의 여러 이벤트 핸들러를 추가 할 수도 있습니다.
 

## 결론
 

JavaScript로 DOM을 조작하는 방법을 아는 것은 매우 중요합니다.
 당신이 알지 못하도록 결정할 수있는 것이 아닙니다.
 

위에 제시 한 예제 / 그림을 이해한다면 작은 JS 프로젝트를 빌드 할 수있을 것입니다.
 좋은 개발자가되고 싶다면 프로젝트 구축의 중요성을 지나치게 강조 할 수 없습니다.
 

프로젝트를 구축하기가 어렵습니까?
 아래 링크에서 두 번째 및 세 번째 링크를 클릭하십시오.
 