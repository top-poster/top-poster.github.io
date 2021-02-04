---
layout: post
title: "React를 사용한 MobX 소개
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/mobxreact-1.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/mobxreact-1.png?fit=730%2C487&ssl=1)

## MobX 소개
 

MobX는 오픈 소스 상태 관리 도구입니다.
 웹 애플리케이션을 만들 때 개발자는 종종 애플리케이션 내에서 상태를 관리하는 효과적인 방법을 모색합니다.
 한 가지 해결책은 React 팀에서 소개하고 나중에 React-Redux라는 패키지에 구현 된 Flux라는 단방향 데이터 흐름 패턴을 사용하는 것입니다.이 패턴은 Flux 패턴을 훨씬 더 쉽게 사용할 수 있도록합니다.
 

단순하고 확장 가능한 독립형 상태 관리 라이브러리 인 MobX는 FRP (Functional Reactive Programming) 구현을 따르고 모든 파생이 자동으로 수행되도록하여 불일치 상태를 방지합니다.
 MobX 시작하기 페이지에 따르면 "MobX는 근본 문제를 해결하여 상태 관리를 다시 간단하게 만듭니다. 일관성없는 상태를 생성 할 수 없게합니다."
 

MobX는 독립형이며 프론트 엔드 라이브러리 또는 프레임 워크에 의존하지 않습니다.
 React, Vue 및 Angular와 같은 인기있는 프런트 엔드 프레임 워크에는 MobX가 구현되어 있습니다.
 

이 튜토리얼에서는 React와 함께 MobX를 사용하는 방법을 논의하지만 먼저 MobX를 조금 더 잘 이해하는 것으로 시작합니다.
 

## MobX : 핵심 개념
 

라이브러리 인 것 외에도 MobX는 상태, 동작 및 파생 (반응 및 계산 된 값 포함)과 같은 몇 가지 개념을 도입합니다.
 

응용 프로그램 상태는 응용 프로그램의 전체 모델을 나타내며 배열, 숫자 및 개체를 비롯한 다양한 데이터 유형을 포함 할 수 있습니다.
 MobX에서 액션은 상태를 조작하고 업데이트하는 메서드입니다.
 이러한 메서드는 JavaScript 이벤트 처리기에 바인딩되어 UI 이벤트가 트리거하도록 할 수 있습니다.
 

추가 상호 작용없이 애플리케이션 상태에서 파생 된 모든 것 (단지 값이 아님)을 파생이라고합니다.
 파생은 특정 상태를 수신 한 다음 일부 계산을 수행하여 해당 상태와 구별되는 값을 생성합니다.
 파생은 개체를 포함한 모든 데이터 유형을 반환 할 수 있습니다.
 MobX에서 두 가지 유형의 파생은 반응과 계산 된 값입니다.
 

때로는 상태가 변경 될 때 상태를 업데이트하는 데 필요한 자동 부작용이있을 수 있습니다.
 MobX는 이것을 반응이라고하며 DOM의 이벤트 핸들러와 반응을 구별합니다.
 반응은 원격 네트워크 요청을 만들거나 로컬 저장소를 호출하거나 새 DOM 요소를 즉석에서 추가 할 수도 있습니다.
 

반드시 값을 반환하지 않는 반응과 달리 계산 된 값 파생은 항상 현재 상태에서 파생 된 값을 반환합니다.
 

## 데모 : MobX를 사용하여 상점 만들기
 

MobX의 작동 방식을 보여주기 위해 애완 동물 소유자 상점을 구현하는 예제를 만들 것입니다.
 시작하려면 애완 동물과 소유자를 인스턴스 속성으로 포함하고 빈 배열로 초기화 된 `클래스`를 사용하여 상점의 기본 표현을 만듭니다.
 

```undefined
class PetOwnerStore {
  pets = [];
  owners = [];
}
```

### 새 항목 만들기
 

이상적으로는 매장이 새로운 애완 동물과 새로운 주인을 만들 수 있기를 바랍니다.
 이를 위해 스토어에 두 가지 메소드를 소개합니다. pet 객체를 받아 현재 인스턴스의 pet 배열로 푸시하는`createPet`과 소유자 객체를 받아 끝까지 푸시하는`createOwner`
 현재 인스턴스의 소유자 배열 :
 

```coffeescript
class PetOwnerStore {
  pets = [];
  owners = [];

  createPet(pet = { id: 0, name: "", type: "", breed: "", owner: null }) {
    this.pets.push(pet);
  }

  createOwner(owner = { id: 0, firstName: "", lastName: "" }) {
    this.owners.push(owner);
  }
}
```

### 항목 자동 업데이트
 

또한 상점 항목을 자동으로 업데이트 할 수 있기를 원합니다.
 이를 위해 두 가지 메소드를 더 소개합니다. `updateOwner`는 ID를 사용하여 소유자를 업데이트하고 `updatePet`는 ID를 사용하여 애완 동물을 업데이트합니다.
 

```js
class PetOwnerStore {
  pets = [];
  owners = [];

    // ...create pet

    // ...create owner

    // update owner
  updateOwner(ownerId, update) {
    const ownerIndexAtId = this.owners.findIndex((owner) => owner.id === ownerId);
    if (ownerIndexAtId > -1 && update) {
      this.owners[ownerIndexAtId] = update;
    }
  }

    // update pet
  updatePet(petId, update) {
    const petIndexAtId = this.pets.findIndex((pet) => pet.id === petId);
    if (petIndexAtId > -1 && update) {
      this.pets[petIndexAtId] = update;
    }
  }
}
```

### 항목 제거
 

마찬가지로, 우리는 상점에서 주인이나 애완 동물을 제거 할 수 있기를 원합니다.
 

```js
class PetOwnerStore {
  pets = [];
  owners = [];

  // ...create pet

    // ...create owner

  // ...update pet

    // ...update owner

  // delete pet by user id
  deletePet(petId) {
    const petIndexAtId = this.pets.findIndex((pet) => pet.id === petId);
    if (petIndexAtId > -1) {
      this.pets.splice(petIndexAtId, 1)
    }
  }

    // delete owner by owner id
  deleteOwner(ownerId) {
    const ownerIndexAtId = this.owners.findIndex((owner) => owner.id === ownerId);
    if (ownerIndexAtId > -1) {
      this.owners.splice(ownerIndexAtId, 1)
    }
  }
}
```

### `get`에 대한 액세스 권한 부여
 

또한`totalOwners`,`totalPets` 및`getPetsByOwner`를 얻기 위해 액세스 권한을 부여해야합니다.
 

```undefined
class PetOwnerStore {

  pets = [];
  owners = [];

    // total number owners
  get totalOwners() {
    return this.owners.length;
  }

    // total number of pets
  get totalPets() {
    return this.pets.length;
  }

    // Get pets using ownerId
  getPetsByOwner(ownerId) {
    return this.pets.filter((pet) => {
      return pet.owner && pet.owner.id === ownerId;
    });
  }

    // ...create pet

    // ...create owner

  // ...update pet

    // ...update owner

  // ...delete pet by user id

    // ...delete owner by owner id
}
```

### `id` 할당
 

마지막으로`ownerId` 및`petId`를 사용하여 애완 동물에 소유자를 할당하고`$ {this.totalPets ()} 총 애완 동물 수 및 $ {this.totalOwners ()}`를 사용하여 스토어에 대한 일부 세부 정보를 업데이트하려고합니다.
 문자열로 :
 

```js
class PetOwnerStore {

  pets = [];
  owners = [];

  // ... total number owners

  // ... total number of pets

  // ... Get pets using ownerId

    // ...create pet

    // ...create owner

  // ...update pet

    // ...update owner

  // ...delete pet by user id

    // ...delete owner by owner id

  // assign an owner using ownerId to a pet using petId
  assignOwnerToPet(ownerId, petId) {
    const petIndexAtId = this.pets.findIndex((pet) => pet.id === petId);
    const ownerIndexAtId = this.owners.findIndex((pet) => pet.id === ownerId);
    if (petIndexAtId > -1 && ownerIndexAtId > -1) {
      this.pets[petIndexAtId].owner = this.owners[petIndexAtId];
    }
  }

    // get store details
    get storeDetails () {
    return `We have ${this.totalPets()} total pets and ${this.totalOwners()} total owners, so far!!!`
  }

    // Log the store details to the console
  logStoreDetails() {
    console.log(this.storeDetails);
  }
}
```

### 최종 구현
 

완료되면 상점의 최종 구현은 다음과 같아야합니다.
 

```js
class PetOwnerStore {

  pets = [];
  owners = [];

    // total number owners
  get totalOwners() {
    return this.owners.length;
  }

    // total number of pets
  get totalPets() {
    return this.pets.length;
  }

    // Get pets using ownerId
  getPetsByOwner(ownerId) {
    return this.pets.filter((pet) => {
      return pet.owner && pet.owner.id === ownerId;
    });
  }

  createPet(pet = { id: 0, name: "", type: "", breed: "", owner: null }) {
    this.pets.push(pet);
  }

  createOwner(owner = { id: 0, firstName: "", lastName: "" }) {
    this.owners.push(owner);
  }

  updateOwner(ownerId, update) {
    const ownerIndexAtId = this.owners.findIndex((pet) => owner.id === ownerId);
    if (ownerIndexAtId > -1 && update) {
      this.owners[ownerIndexAtId] = update;
    }
  }

  updatePet(petId, update) {
    const petIndexAtId = this.pets.findIndex((pet) => pet.id === petId);
    if (petIndexAtId > -1 && update) {
      this.pets[petIndexAtId] = update;
    }
  }

  deletePet(petId) {
    const petIndexAtId = this.pets.findIndex((pet) => pet.id === petId);
    if (petIndexAtId > -1) {
      this.pets.splice(petIndexAtId, 1)
    }
  }

  deleteOwner(ownerId) {
    const ownerIndexAtId = this.owners.findIndex((owner) => owner.id === ownerId);
    if (ownerIndexAtId > -1) {
      this.owners.splice(ownerIndexAtId, 1)
    }
  }

  // assign an owner using ownerId to a pet using petId
  assignOwnerToPet(ownerId, petId) {
    const petIndexAtId = this.pets.findIndex((pet) => pet.id === petId);
    const ownerIndexAtId = this.owners.findIndex((pet) => pet.id === ownerId);
    if (petIndexAtId > -1 && ownerIndexAtId > -1) {
      this.pets[petIndexAtId].owner = this.owners[petIndexAtId];
    }
  }

    get storeDetails () {
    return `We have ${this.totalPets()} total pets and ${this.totalOwners()} total owners, so far!!!`
  }

  logStoreDetails() {
    console.log(this.storeDetails);
  }
}
```

### 상점 첫 화면 초기화
 

작동중인 저장소를보기 위해 일반 JavaScript 클래스를 초기화하는 것처럼 초기화합니다.
 초기화 후 표시된 메소드를 사용하여 상점과 인터페이스 할 수 있습니다.
 

예를 들어, 새로운 애완 동물과 주인을 상점에 추가하고 지금까지의 세부 정보를 기록합니다.
 

```undefined
const petOwnerStore = new PetOwnerStore();

  petOwnerStore.createPet({
    id: 1,
    name: "Bingo",
    type: "Dog",
    breed: "alsertian",
  });
  petOwnerStore.createPet({
    id: 2,
    name: "Lloyd",
    type: "Cat",
    breed: "winky",
  });
  petOwnerStore.createOwner({ id: 1, firstName: "Aleem", lastName: "Isiaka" });

  petOwnerStore.logStoreDetails(); // -> We have 2 pets and 1 owners, so far!!!
```

## MobX 스토어를 반응 형으로 만들기
 

앞서 논의한 바와 같이 MobX 스토어는 반응 형이어야하며 따라서 변경 사항에 대응해야합니다.
 MobX 라이브러리에서 제공하는`makeObservable` 함수를 구현하여이를 테스트 할 수 있습니다.
 

```coffeescript
import { makeObservable } from "mobx";
```

`makeObservable` 함수는 클래스를 관찰 가능한 상태로 바꾸어 필드의 일부가 변경 될 때마다 새로 고침되고 업데이트됩니다.
 MobX 라이브러리의`makeObservable` 내보내기는 클래스 인스턴스에 대한 참조와 클래스 인스턴스 메소드 및 필드의 객체 구성이라는 두 가지 매개 변수를 허용합니다.
 

상점을 관찰 가능하게 만드는 데 도움이되는 몇 가지 MobX 구성 옵션은 다음과 같습니다.
 

- 프리미티브, 배열 또는 객체를 보유하는 저장소의 모든 필드 값을 `관찰 가능`으로 만드십시오. 값 유형에 따라 관찰 가능을 생성하는 다양한 방법을 사용하십시오.
 
- `mobx`에서 `import {action}`을 사용하여 MobX 라이브러리에서 이름이 지정된 내보내기로 가져올 `action`으로 메서드를 장식합니다.
 MobX는 다른 액션 유형과 함께 제공됩니다.
 
- 상점의 현재 상태 (일명 파생)를 기반으로 값을 `계산 됨`으로 반환하는 함수를 구성합니다.
 
- 반응 (현재 상태에서 실행되지만 값을 반환하지 않는 함수)을 `자동 실행`으로 구성합니다.
 여기에서 반응에 대한 다른 옵션을 봅니다.
 

## MobX 스토어를 관찰 가능하게 만들기
 

`PetOwnerStore` 클래스를 관찰 가능하게 만들기 위해 저장소를 반응 형으로 만드는 구성을 보유 할 생성자를 저장소에 도입하는 것으로 시작합니다.
 

```js
class PetOwnerStore {
  pets = [];
  owners = [];

  constructor () {
    makeObservable(this, {
      pets: observable,
      owners: observable,
      totalOwners: computed,
      totalPets: computed,
      storeDetails: computed,
      getPetsByOwner: action,
      createPet: action,
      createOwner: action,
      updatePet: action,
      updateOwner: action,
      deletePet: action,
      deleteOwner: action,
      assignOwnerToPet: action
    });
    autorun(logStoreDetails);
  }

    // ... remaining store implementation
}
```

알다시피, 값이 변경 될 때 스토어 인터페이스를 계속 업데이트하기 위해`pets` 및`owners`를`observable`로 표시했습니다.
 

또한`totalOwners`,`totalPets` 및`storeDetails`를`computed`로 표시하여 이러한 값이 업데이트되고 반환 될 때 캐싱을 허용합니다.
 또한 상태 수정을 설명하기 위해`createPet`,`createOwner`,`updatePet`,`updateOwner`,`deletePet`,`deleteOwner` 및`assignownerToPet`을`action`으로 표시했습니다.
 

`logStoreDetails`는 상점 세부 정보를 기록하기 때문에 반응으로 실행되지만 값을 반환하지 않습니다.
 

## MobX 스토어 등록 및 상호 작용
 

비 반응성 저장소에서했던 것처럼`new `연산자를 사용하여 저장소의 새 인스턴스를 생성하여 반응 저장소를 적용 할 수 있습니다.
 

```cpp
const petOwnerStore = new PetOwnerStore();
// -> We have 0 pets and 0 owners, so far!!!
```

MobX는 초기화 중에 그리고 저장소에 업데이트가있을 때마다 반응을 호출합니다. 즉, `logStoreDetails`반응 함수는 모든 초기화 후에 문질러집니다.
 

이제 새로운 애완 동물과 주인을 만들어 상점과 상호 작용할 수 있습니다.
 반응이 기록됩니다.
 

```undefined
const petOwnerStore = new PetOwnerStore();
// -> We have 0 pets and 0 owners, so far!!!

petOwnerStore.createPet({
  id: 1,
  name: "Bingo",
  type: "Dog",
  breed: "alsertian",
});
// -> We have 1 pets and 0 owners, so far!!!

petOwnerStore.createPet({
  id: 2,
  name: "Lloyd",
  type: "Cat",
  breed: "winky",
});
// -> We have 2 pets and 0 owners, so far!!!
petOwnerStore.createOwner({ id: 1, firstName: "Aleem", lastName: "Isiaka" });
// -> We have 2 pets and 1 owners, so far!!!
```

## MobX 및 React로 프런트 엔드 관리
 

이제 React로 스토어에 프런트 엔드를 추가 할 때입니다!
 

### 새 React 앱 만들기
 

먼저 create-react-app 라이브러리를 사용하여 새 React 애플리케이션을 만듭니다.
 터미널에서 다음을 실행합니다.
 

```undefined
npx create-react-app mobx-react
```

위의 명령은 응용 프로그램을 부트 스트랩하고 종속성을 설치합니다.
 `cd mobx-react`를 사용하여 폴더로 이동할 수 있습니다.
 

`PetOwner` 스토어에 더 쉽게 액세스 할 수 있도록`src` 폴더에 새 파일 i,`PetOwnerStore.js`를 생성하고`PetOwner` MobX 스토어의 콘텐츠와 함께로드합니다.
 `PetOwner` MobX를 별도로 개발 했으므로 이제 앞에서 만든`PetOwnerStore`를 프론트 엔드 프로젝트의`src` 폴더에 복사하여 액세스 할 수 있도록 만들 것입니다.
 

```undefined
# inside of /path/to/mobx-react
cp path/to/PetOwnerStore.js ./src/PetOwnerStore.js
```

이제`src` 폴더 안에 구성 요소 폴더를 만들고 폴더 안에`PetList` 구성 요소를 만들고 원하는 편집기에서 파일을 열 수 있습니다.
 

```bash
cd src
mkdir components
touch components/PetList.jsx
```

### 구성 요소 관리
 

먼저 PetList 구성 요소에 상점의 세부 정보를 표시해 보겠습니다.
 

```js
import React from "react";

function PetList({ store }) {
  return <div>{store.storeDetails}</div>;
}

export default PetList;
```

`App.jsx` 내에서`PetList` 구성 요소를 가져 와서 store 객체를 소품으로 전달합니다. 그 후에`App.jsx` 구성 요소는 다음과 같습니다.
 

```js
import PetOwnerStore from "./PetOwnerStore";
import PetList from "./components/PetList";

function App() {
  const store = new PetOwnerStore();
  return (
    <div className="App">
      <PetList store={store} />
    </div>
  );
}

export default App;
```

결과는 다음과 같습니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/mobx-components-example-1024x274.png?resize=691%2C185&ssl=1)

### 구성 요소 수정 및 세부 정보 추가
 

다음으로`PetList.jsx` 컴포넌트를 수정하여 목록에 새 애완 동물을 추가합니다.
 이를 위해 버튼을 추가하고 `onClick`이벤트에 할당합니다.
 

```js
function PetList({ store }) {
  const handleAddPet = () => {};

  return (
    <div>
      {store.storeDetails}
      <button onClick={handleAddPet}>+ New pet</button>
    </div>
  );
}
```

이제`handleAddPet` 함수를 업데이트하여 사용자로부터 세부 정보를 수집하고 상점의`createPet` 메소드를 호출하여 상점 내부의 pets 배열에 애완 동물을 추가 할 수 있습니다.
 

```js
const handleAddPet = () => {
  const name = prompt("Name of the pet");
  const type = prompt("Type of the pet");
  const breed = prompt("Breed of the pet");
  const ownerId = prompt("Owner's Id of the pet");

  const pet = store.createPet({ id: Date.now(), name, breed, type });
  store.assignOwnerToPet(ownerId, pet.id);
};
```

이 시점에서 콘솔이 보이는 상태에서이 작업을 실행하려고하면 저장소가 업데이트되지만 구성 요소가 새 데이터를받지 못했음을 알 수 있습니다.
 다음 섹션에서 이에 대해 설명합니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/mobx-output-example.png?resize=693%2C264&ssl=1)

## React 컴포넌트를 관찰 가능하게 만들기
 

React 구성 요소가 저장소의 업데이트를 인식하여 구성 요소의 다시 렌더링을 트리거함으로써 위의 문제를 해결할 수 있습니다.
 `PetList` 구성 요소는 도움말`mobx-react-lite` 패키지를 사용하여 관찰 할 수 있습니다.
 

### `mobx-react-lite` 설치
 

시작하려면`npm` / yarn을 사용하여`mobx-react-lite`를 설치합니다.
 

```undefined
npm install mobx-react-lite --save
# or
yarn add mobx-react-lite
```

mobx-state-tree 패키지는 React 애플리케이션에서 MobX를 설정하는 데에도 사용할 수 있습니다.
 

### '관찰자'가져 오기
 

`PetList` 컴포넌트 내에서`mobx-react-lite`에서`observer`를 가져옵니다.
 그런 다음 `PetList`구성 요소를 다음과 같이 래핑합니다.
 

```js
import React from "react";
import { observer } from "mobx-react-lite";

function PetList({ store }) {
  const handleAddPet = () => {
    const name = prompt("Name of the pet");
    const type = prompt("Type of the pet");
    const breed = prompt("Breed of the pet");
    const ownerId = prompt("Owner's Id of the pet");

    const pet = store.createPet({ id: Date.now(), name, breed, type });
    store.assignOwnerToPet(ownerId, pet.id);
  };

  return (
    <div>
      {store.storeDetails}
      <p>
        <button onClick={handleAddPet}>+ New pet</button>
      </p>
    </div>
  );
}

export default observer(PetList);
```

옵저버로 컴포넌트를 래핑하면 이제 자동으로 상점의 변경 사항을 인식하게됩니다.
 이제 새 애완 동물을 만들고 구성 요소를 다시 렌더링 할 수 있습니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/important-observer.png?resize=681%2C263&ssl=1)

이제 MobX가 애플리케이션과 상호 작용할 수 있으므로 다음 섹션에서는 애완 동물 항목을 나열하고, 항목을 업데이트하고, 항목을 삭제하는 방법에 대해 설명합니다.
 

### 상태 항목 나열
 

MobX 및 React를 사용하여 펫 상태의 항목을 나열하는 테이블과 상점에서 펫 항목을 업데이트 및 삭제하는 버튼을 만들 수 있습니다.
 

```xml
<p>{store.storeDetails}</p>
<table>
  <thead>
    <tr>
      <th>##</th>
      <th>Pet Name</th>
      <th>Pet Type</th>
      <th>Pet Breed</th>
      <th>Owner</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    {store.pets.map((pet) => {
      return (
        <tr key={pet.id}>
          <td>{pet.id}</td>
          <td>{pet.name}</td>
          <td>{pet.type}</td>
          <td>{pet.breed}</td>
          <td>
            {pet.owner
              ? `${pet.owner?.firstName} ${pet.owner?.lastName}`
              : "---"}
          </td>
          <td>
            <button
              onClick={() => handleDeletePet(pet)}
              style={ marginRight: "1rem" }
            >
              Delete {pet.name}
            </button>
            <button onClick={() => handleUpdatePet(pet)}>
              Update {pet.name}
            </button>
          </td>
        </tr>
      );
    })}
  </tbody>
</table>
<button onClick={handleAddPet}>+ New pet</button>
```

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/observer-output.png?resize=696%2C124&ssl=1)

### 항목 편집
 

반려 동물을 나열 할 때 제공 한`handleUpdate` 기능을 구현하여 반려 동물과 같은 항목을 편집 할 수 있습니다.
 

`PetList` 구성 요소의 맨 위에 아래`handleUpdatePet` 함수를 추가합니다.
 

```js
const handleUpdatePet = (pet) => {
  pet.name = prompt("Name of the pet", pet.name);
  pet.type = prompt("Type of the pet", pet.type);
  pet.breed = prompt("Breed of the pet", pet.breed);
  const ownerId = prompt("Owner's Id of the pet", pet.owner?.id);
  store.updatePet(pet.id, pet);
  if (ownerId !== pet.owner?.id) {
    store.assignOwnerToPet(ownerId, pet.id);
  }
};
```

이 함수는 애완 동물을 받아들이고 프롬프트를 사용하여 이름, 유형, 품종 및 `ownerId`를 가져옵니다. 스토어에서`updatePet` 함수를 호출하여 수정 된 애완 동물 개체를 전달합니다.
 새로운`ownerId`가있는 경우 스토어 인스턴스에서`assignOwnerToPet` 메소드를 호출하여 애완 동물을 소유자에게 할당합니다.
 

### 항목 삭제
 

아이템을 삭제하려면 목록에있는 펫 아이템에 `handleDelete`함수를 구현하면됩니다.
 이 함수는`pet` 객체를 받아들이고`store.deletePet`을 호출하여 프로세스를 완료합니다.
 

```coffeescript
const handleDeletePet = (pet) => {
  store.deletePet(pet.id);
};
```

### 전체 코드 : 생성, 업데이트 및 삭제
 

다음은 상점 항목을 작성, 업데이트 및 삭제할 수있게하는 PetList 구성 요소의 전체 코드입니다.
 

```js
import React from "react";
import { observer } from "mobx-react-lite";

function PetList({ store }) {
  const handleAddPet = () => {
    const name = prompt("Name of the pet");
    const type = prompt("Type of the pet");
    const breed = prompt("Breed of the pet");
    const ownerId = prompt("Owner's Id of the pet");

    const pet = store.createPet({ id: Date.now(), name, breed, type });
    store.assignOwnerToPet(ownerId, pet.id);
  };

  const handleUpdatePet = (pet) => {
    pet.name = prompt("Name of the pet", pet.name);
    pet.type = prompt("Type of the pet", pet.type);
    pet.breed = prompt("Breed of the pet", pet.breed);
    const ownerId = prompt("Owner's Id of the pet", pet.owner?.id);
    store.updatePet(pet.id, pet);
    if (ownerId !== pet.owner?.id) {
      store.assignOwnerToPet(ownerId, pet.id);
    }
  };

  const handleDeletePet = (pet) => {
    store.deletePet(pet.id);
  };

  return (
    <div>
      <p>{store.storeDetails}</p>
      <table>
        <thead>
          <tr>
            <th>##</th>
            <th>Pet Name</th>
            <th>Pet Type</th>
            <th>Pet Breed</th>
            <th>Owner</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          {store.pets.map((pet) => {
            return (
              <tr key={pet.id}>
                <td>{pet.id}</td>
                <td>{pet.name}</td>
                <td>{pet.type}</td>
                <td>{pet.breed}</td>
                <td>
                  {pet.owner
                    ? `${pet.owner?.firstName} ${pet.owner?.lastName}`
                    : "---"}
                </td>
                <td>
                  <button
                    onClick={() => handleDeletePet(pet)}
                    style={ marginRight: "1rem" }
                  >
                    Delete {pet.name}
                  </button>
                  <button onClick={() => handleUpdatePet(pet)}>
                    Update {pet.name}
                  </button>
                </td>
              </tr>
            );
          })}
        </tbody>
      </table>
      <button onClick={handleAddPet}>+ New pet</button>
    </div>
  );
}

export default observer(PetList);
```

### 신청 마무리
 

신청서가 완성 되려면 소유자를 생성, 업데이트 및 삭제해야합니다.
 시작하려면 구성 요소 폴더 안에 새 구성 요소 `OwnerList`를 만듭니다.
 

```undefined
touch ./src/components/OwnerList.jsx
```

그런 다음 애플리케이션에서`App.jsx` 내부의 구성 요소를 가져 와서`PetList` 구성 요소에 대해 수행 한 것처럼 저장소에 전달합니다.
 

```js
import PetOwnerStore from "./PetOwnerStore";
import PetList from "./components/PetList";
import OwnerList from "./components/OwnerList";
import "./App.css";

function App() {
  const store = new PetOwnerStore();
  return (
    <div className="App">
      <h3>Pets List</h3>
      <PetList store={store} />
      <hr />
      <h3>Owners List</h3>
      <OwnerList store={store} />
    </div>
  );
}

export default App;
```

다음으로 `OwnerList`구성 요소를 다음 코드로 업데이트합니다.
 

```js
import { observer } from "mobx-react-lite";
import React from "react";

function OwnerList({ store }) {
  const handleAddOwner = () => {
    const firstName = prompt("Firstname?");
    const lastName = prompt("Lastname?");
    store.createOwner({ id: Date.now(), firstName, lastName });
  };

  const handleUpdateOwner = (owner) => {
    owner.firstName = prompt("Firstname?", owner.firstName);
    owner.lastName = prompt("Lastname?", owner.lastName);
    store.updateOwner(owner.id, owner);
  };

  const handleDeleteOwner = (owner) => {
    store.deleteOwner(owner.id);
  };

  return (
    <div className="pet-owner-app">
      <table>
        <thead>
          <tr>
            <th>##</th>
            <th>First Name</th>
            <th>last Name</th>
            <th>Owner</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          {store.owners.map((owner) => {
            return (
              <tr key={owner.id}>
                <td>{owner.id}</td>
                <td>{owner.firstName}</td>
                <td>{owner.lastName}</td>
                <td>
                  <button
                    onClick={() => handleDeleteOwner(owner)}
                    style={ marginRight: "1rem" }
                  >
                    Delete {owner.firstName}
                  </button>
                  <button onClick={() => handleUpdateOwner(owner)}>
                    Update {owner.firstName}
                  </button>
                </td>
              </tr>
            );
          })}
        </tbody>
      </table>
      <button onClick={handleAddOwner}>+ New owner</button>
    </div>
  );
}

export default observer(OwnerList);
```

`OwnerList` 구성 요소는 PetList 구성 요소와 동일한 방식으로 작동합니다.
 여기서 유일한 차이점은`PetList` 구성 요소에서 수행 한 것처럼 애완 동물에게 소유자를 할당하지 않는다는 것입니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/OwnerList-mobx.png?resize=716%2C351&ssl=1)

## MobX를 사용하여 원격 서버에서 데이터 관리
 

여러 번 데이터를 원격 서버에서로드해야합니다.
 `PetOwnerStore`를 수정하고 클래스 끝에`prefetchData` 메소드를 추가하여이를 시뮬레이션 할 수 있습니다.
 `setTimeout`을 사용하여 네트워크 요청을 시뮬레이션 한 다음 클래스에서 create 메서드를 호출하여 새로 사용 가능한 데이터를 저장소에 추가합니다.
 

```coffeescript
class PetOwnerStore {
  // Other implemetations

  prefetchData = () => {
    const owners = [{ firstName: "Aleem", lastName: "Isiaka", id: 1 }];
    const pets = [
      {
        id: 1,
        name: "Lincy",
        breed: "Siamese",
        type: "Cat",
        ownerId: 1,
      },
    ];

    setTimeout(() => {
      console.log("Fetch complete update store");
      owners.map((pet) => this.createOwner(pet));
      pets.map((pet) => {
        this.createPet(pet);
        this.assignOwnerToPet(pet.ownerId, pet.id);
        return pet;
      });
    }, 3000);
  };
}
```

생성자에서이 메소드를 MobX가 관리 할 액션으로 등록합니다.
 앱로드를 시작하자마자 데이터를 가져와야하므로 스토어 초기화 중에 메서드를 호출합니다.
 이를 위해 다음과 같이`runInAction`을 사용합니다.
 

```js
import {
  action,
  computed,
  makeObservable,
  observable,
  autorun,
  runInAction,
} from "mobx";

class PetOwnerStore {
  pets = [];
  owners = [];

  constructor() {
    makeObservable(this, {
      pets: observable,
      owners: observable,
      totalOwners: computed,
      totalPets: computed,
      storeDetails: computed,
      getPetsByOwner: action,
      createPet: action,
      createOwner: action,
      updatePet: action,
      updateOwner: action,
      deletePet: action,
      deleteOwner: action,
      assignOwnerToPet: action,
    });
    autorun(this.logStoreDetails);
    // A reaction that runs just once!
    runInAction(this.prefetchData);
  }

  logStoreDetails = () => {
    console.log(this.storeDetails);
  };

  prefetchData = () => {
    const owners = [{ firstName: "Aleem", lastName: "Isiaka", id: 1 }];
    const pets = [
      {
        id: 1,
        name: "Lincy",
        breed: "Siamese",
        type: "Cat",
        ownerId: 1,
      },
    ];

    setTimeout(() => {
      console.log("Fetch complete update store");
      owners.map((pet) => this.createOwner(pet));
      pets.map((pet) => {
        this.createPet(pet);
        this.assignOwnerToPet(pet.ownerId, pet.id);
        return pet;
      });
    }, 3000);
  };
}

export default PetOwnerStore;
```

그리고 그게 다야!
 이제 CRUD 기능이있는 완전한`PetOwner` React / MobX 앱을 만들었습니다.
 향후 설명을 위해 최종 애플리케이션과 코드를 검토하는 것이 좋습니다.
 

## 결론
 

이 기사에서는 MobX의 반응성을 사용하여 애플리케이션의 상태를 관리하는 방법과 특히 MobX를 사용하여 React 애플리케이션의 상태를 관리하는 방법을 살펴 보았습니다.
 

예를 들어, 우리는 기사 작성자가 개인적으로 선호하는 지침 목적으로 UI에서 상점의 로직을 분리하려고 노력했습니다.
 MobX는 프로젝트에 대한 특정 구조를 주장하지 않으며 실제로 응용 프로그램의 구조와 일치하는 설정을 권장합니다.
 