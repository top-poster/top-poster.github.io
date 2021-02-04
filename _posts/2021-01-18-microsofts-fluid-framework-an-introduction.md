---
layout: post
title: "Microsoft Fluid Framework : 소개
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/microsoft-fluid-framework-introduction.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/microsoft-fluid-framework-introduction.png?fit=730%2C487&ssl=1)

Microsoft의 Fluid Framework는 최근에 오픈 소스 화 된 새롭고 흥미로운 기술입니다.
 Microsoft는 Office 365 및 Teams를 비롯한 많은 인기 앱에서 Fluid Framework를 사용합니다.
 

이 기술의 주요 사용 사례는 사용자 간의 공동 작업 및 실시간 업데이트를 가능하게하는 것입니다.
 이것은 WebSocket을 통해 실시간 업데이트를 브로드 캐스트 할뿐만 아니라 분산 데이터 구조 (DDS)를 통해 해당 데이터에서 이러한 업데이트를 유지한다는 점에서 기존 SignalR 기술과 다릅니다.
 

Fluid Framework가 오픈 소스 화되었으므로이 기술은 Microsoft 에코 시스템 내부와 그 밖의 클라이언트 응용 프로그램 내에서 활용할 수 있습니다.
 Fluid Framework의 사용 사례는 다음과 같습니다.
 

- 공유 프로젝트 (문서, 프레젠테이션 등)
 
- 노름
 
- 현재 상태를 표시해야하는 앱 (사람이 온라인 상태임을 표시)
 
- Microsoft Visio 또는 순서도 도구와 같은 브레인 스토밍 및 아이디어 앱
 
- 팀 협업
 

Fluid의 주요 목표는 개발자가 메시징 및 데이터 동기화를 처리하는 대신 경험에 집중할 수 있도록 실시간 업데이트의 파이프 및 메커니즘을 처리하는 것입니다.
 Fluid Framework는 응용 프로그램이 실시간 업데이트를 가질 수 있도록하는 도우미 메서드 및 래퍼를 제공합니다.
 

이 게시물에서는 Fluid Framework를 소개 한 다음 샘플 응용 프로그램을 살펴보고이를 프로젝트에 통합하는 방법을 보여줍니다.
 이 기술이 실제로 어떻게 보이는지에 대한 간략한 소개는 Build 2019에 표시된 데모를 확인하십시오.
 

## Fluid Framework의 작동 방식
 

소개에서 언급했듯이 Fluid Framework는 한동안 사용되어 왔으며 오늘날 볼 수있는 많은 Microsoft 앱에 존재합니다.
 일반적인 의미에서 역학에 대해 논의 할 수 있으며 Microsoft Teams와 같은 앱을 사용하는 경우 실제로 작동하는 것을 볼 수도 있습니다.
 

프레임 워크는 다음 용어로 설명 할 수 있습니다.
 

- 유체 로더
 
- 유체 용기
 
- 유체 서비스
 

Fluid Framework 문서에서 다음 그래프를 빌려 왔으며 뛰어난 시각적 효과를 제공합니다.
 

응용 프로그램에서 Fluid Framework를 사용하면 Fluid Loader에서 시작됩니다.
 Fluid Loader는 클라이언트가 Fluid Framework와 통신 할 수 있도록하는 모든 메커니즘을 수용하는 Fluid Container를 포장합니다.
 

유체 컨테이너에는 유체 로더와 통신 한 다음 유체 서비스와 다시 통신하는 모든 로직이 포함되어 있습니다.
 Fluid Container에는 응용 프로그램에 연결된 클라이언트에 데이터를 유지하는 분산 데이터 구조 (DDS)가 포함 된 Fluid Runtime도 포함되어 있습니다.
 

Fluid Service는 클라이언트 내 DDS의 모든 변경 사항을 작업 (변경)으로 가져옵니다.
 작업이 Fluid Service로 전달 될 때마다 발생한 DDS 내에서 변경 사항을 유지 한 다음 연결된 모든 클라이언트에 변경 사항을 전파합니다.
 

Fluid Service는 다음을 위해 작동합니다.
 

- 주문 유지
 
- 방송 변경
 
- 데이터 저장
 

모든 클라이언트에서 데이터 상태를 유지하는 방법은 세션 저장소와 영구 저장소를 사용하는 것입니다.
 세션 스토리지는 클라이언트 자체에서 실행되는 Fluid Service에 의해 관리됩니다.
 영구 저장소는 Fluid Service 외부 (일반적으로 데이터베이스 또는 파일)에 저장된 작업 기록입니다.
 

Fluid Framework를 사용하면 클라이언트 코드가 모든 무거운 작업을 처리하는 npm에서 사용 가능한 라이브러리를 가져올 수 있습니다.
 Fluid Framework의 가장 좋은 부분 중 하나는 React, Vue.js 및 Angular를 포함한 가장 인기있는 UI 라이브러리와 독립적으로 작동한다는 것입니다.
 

이는 팀이이 기술을 구현하기 위해 선택한 프레임 워크를 사용할 수있는 많은 유연성을 제공합니다.
 개발자는 클라이언트 경험에 집중하고 Fluid Framework가 나머지 작업을 수행하도록 할 수 있습니다.
 

Fluid Service에 대한 서버 구성 요소도 있습니다.
 클라이언트의 작업이 지속 되려면 데이터를 저장할 서버가 필요합니다.
 Microsoft 응용 프로그램은 SharePoint 및 OneDrive의 형태로 Office 365에서이를 지원합니다.
 

직접 구축하려는 경우 Fluid Service를 Routerlicious를 통해 구현할 수도 있습니다.이 서비스는 서로 다른 클라이언트 간의 작업 교환을 처리합니다.
 이 구현은 로컬 서버로 사용하거나 애플리케이션을 위해 생산할 수 있습니다.
 자세한 내용은 Routerlicious README를 확인하세요.
 

전반적인 구현에 대한 자세한 정보를 원하신다면 Nick Simmons와 Dan Wahlin의이 비디오를 시청하시기를 적극 권장합니다.
 

### Fluid Framework 대 SignalR
 

Fluid Framework는 둘 다 실시간 통신이 가능하다는 점에서 SignalR 프로토콜과 다소 유사합니다.
 그러나 Fluid Framework와 SignalR의 주요 차이점은 Fluid Framework가 앞서 언급 한 DDS 개체와의 통신을 조정한다는 것입니다.
 

SignalR은 클라이언트 간의 직접 통신을 가능하게합니다.
 Fluid Framework는 전송할 데이터를 가져 와서 전송할뿐만 아니라 DDS 개체가 설정된 방식에 따라 조정합니다.
 SignalR에 대한 자세한 내용은 Microsoft SignalR과 Angular 연결에 대한 내 블로그 게시물을 확인하십시오.
 

## Fluid Framework로 응용 프로그램 작성
 

지금까지 기술과 작동 방식에 대해 논의했습니다.
 애플리케이션 코드에서이를 사용하는 방법의 시작 단계까지 진행했습니다.
 

모든 것이 어떻게 결합되는지 더 잘 이해하려면 예제 애플리케이션에서 확인하는 것이 좋습니다.
 Fluid Framework는 클라이언트를위한 하나의 라이브러리에 의존하지 않기 때문에 React, Vue.js 및 Angular를 포함하기 위해 널리 사용되는 프런트 엔드 라이브러리 또는 프레임 워크로 가져올 수 있습니다.
 

대부분의 경우 Fluid Framework를 사용하려면 Fluid Service를 실행하는 서버와 Fluid Container를 수용하는 클라이언트 응용 프로그램이 필요합니다.
 이 두 가지를 모두 수행하는 방법에는 여러 가지가 있으며 이는 기술의 가장 강력한 부분 중 하나입니다.
 

Fluid Framework 사이트의 시작하기 섹션을 확인하면 시작하는 데 도움이되는 몇 가지 훌륭한 문서와 여러 예제 프로젝트를 찾을 수 있습니다.
 게시물의 다음 섹션에서는 여기에 설명 된 자습서를 살펴 보겠습니다.
 

### 주사위 롤러 예
 

Dice Roller 예제 애플리케이션 소스 코드는 여기 GitHub 저장소에서 찾을 수 있습니다.
 

응용 프로그램 자체는 매우 간단하며 롤을 클릭하면 업데이트되는 주사위 이미지 만 표시됩니다.
 이 응용 프로그램에 연결하는 클라이언트는 Fluid Framework를 통해 주사위를 굴릴 때마다 실시간 업데이트를받습니다.
 

이 응용 프로그램은 Fluid Service를 실행하는 로컬 서버에 연결된 하나의 Fluid Container 만 가지고 있기 때문에 좋은 예입니다.
 

#### 주사위 롤러보기
 

Fluid Framework를 앱에 연결하기 전에 첫 번째 단계는 주사위보기를 정의하는 것입니다.
 주요 프런트 엔드 프레임 워크 및 라이브러리는 다양한 부트 스트랩 메커니즘을 통해이를 수행합니다.
 이 예제는 매우 간단하며 Webpack과 함께 TypeScript를 활용하기 때문에 다음과 같이 초기보기를 정의 할 수 있습니다.
 

```js
export function renderDiceRoller(div: HTMLDivElement) {
    const wrapperDiv = document.createElement("div");
    wrapperDiv.style.textAlign = "center";
    div.append(wrapperDiv);
    const diceCharDiv = document.createElement("div");
    diceCharDiv.style.fontSize = "200px";
    const rollButton = document.createElement("button");
    rollButton.style.fontSize = "50px";
    rollButton.textContent = "Roll";

    rollButton.addEventListener("click", () => { console.log("Roll!"); });
    wrapperDiv.append(diceCharDiv, rollButton);

    const updateDiceChar = () => {
        const diceValue = 1;
        // Unicode 0x2680-0x2685 are the sides of a die (⚀⚁⚂⚃⚄⚅).
        diceCharDiv.textContent = String.fromCodePoint(0x267F + diceValue);
        diceCharDiv.style.color = `hsl(${diceValue * 60}, 70%, 50%)`;
    };
    updateDiceChar();
}
```

눈치 채면 기본 div 스타일을 지정하고 Roll 버튼을 클릭하고 주사위가 업데이트 될 때 반응 할 이벤트 리스너를 추가합니다.
 

#### 주사위 롤러 모델 및 구현
 

이 예제는 TypeScript로 구현되었으므로 인터페이스를 사용하여 앱의 동작을 정의하고 해당 인터페이스의 모델 구현을 구현할 수 있습니다.
 

이 섹션에서 정의 할 구현은 Tinylicious라는 Fluid Framework의 도우미 함수 중 하나를 통해 실행중인 Fluid Service 인스턴스에 연결됩니다.
 어떻게 부트 스트랩되는지 확인하려면 여기에서 프로젝트의`src / app.ts` 파일을 참조하십시오.
 

샘플 앱에서 사용하는 Dice Roller 모델은 "roll"이 발생할 때마다 "EventEmitter"이벤트를 전송하며 다음과 같이 정의됩니다.
 

```coffeescript
export interface IDiceRoller extends EventEmitter {
    readonly value: number;
    roll: () => void;
    on(event: "diceRolled", listener: () => void): this;
}
```

이제 npm 모듈에서 Fluid Framework의`DataObject` 클래스를 가져 오면 (자세한 내용은 여기 참조) 다음 구현에서 Fluid Container에 주사위 롤을 등록합니다.
 

```undefined
export class DiceRoller extends DataObject implements IDiceRoller {
    protected async initializingFirstTime() {
        this.root.set(diceValueKey, 1);
    }

    protected async hasInitialized() {
        this.root.on("valueChanged", (changed: IValueChanged) => {
            if (changed.key === diceValueKey) {
                this.emit("diceRolled");
            }
        });
    }

    public get value() {
        return this.root.get(diceValueKey);
    }

    public readonly roll = () => {
        const rollValue = Math.floor(Math.random() * 6) + 1;
        this.root.set(diceValueKey, rollValue);
    };
}
```

`root` 개체는 Dice Roller 모델 (이전보기)을 실행하는 유체 컨테이너를 유체 서비스에 연결합니다.
 `initializedFirstTime` 및`hasInitialized` 메소드를 발견하면 Fluid Framework의`SharedDirectory`에서`DataObject`를 사용하여 Fluid Service 인스턴스에 저장되는 DDS에 Fluid Container를 등록합니다.
 

우리는이 모든 것을 다음과 같이 모든 클라이언트가 호출 할 수있는 Factory Method로 래핑합니다.
 

```js
import { ContainerRuntimeFactoryWithDefaultDataStore } from "@fluidframework/aqueduct";

export const DiceRollerContainerRuntimeFactory = new ContainerRuntimeFactoryWithDefaultDataStore(
    DiceRollerInstantiationFactory,
    new Map([
        DiceRollerInstantiationFactory.registryEntry,
    ]),
);
```

이 메서드는 컨테이너 인스턴스를 정의하는 Fluid Framework의`ContainerRuntimeFactoryWithDefaultDataStore` 도우미 메서드를 사용합니다.
 전체 구현과 샘플 프로젝트의 위치를 보려면 GitHub 저장소에서`src / dataObject.ts` 파일을 확인하세요.
 

#### 유체 서비스에 유체 컨테이너 연결
 

이제 뷰와 주사위 컨테이너를 정의 했으므로 앞서 언급 한 Tinylicious 서버와이 모든 것을 연결할 수 있습니다.
 `src / app.ts` 파일을 보면 앱이 시작될 때 발생하는 모든 부트 스트랩을 볼 수 있습니다.
 

여기에서 방법에 특별한주의를 기울이십시오.
 

```js
import { getTinyliciousContainer } from "@fluidframework/get-tinylicious-container";

const container = await getTinyliciousContainer(documentId, DiceRollerContainerRuntimeFactory, createNew);
```

가져온 함수`getTinyliciousContainer`는 Fluid 서비스를 실행하는 로컬 서버를 시작할 수있는 Fluid Framework의 npm 패키지의 도우미 메서드입니다.
 프로덕션 환경에서는이를 더 많은 오케스트레이션과 연결하지만 여기의 도우미 메서드를 사용하면 초기 소개로 시작할 수 있습니다.
 

다음은 함수에 전달 된 인수입니다.
 

- `documentId` – Fluid Service가 업데이트를 저장하고 게시하기 위해 키-값 쌍을 올바르게 등록 할 수 있도록하는 세션의 식별자
 
- `DiceRollerContainerRuntimeFactory` – 이는 이전에 Factory 메서드를 사용하여 Fluid Container 생성을 래핑 할 때 생성되었습니다.
 
- `createNew` – Tinylicious가 새 세션을 시작할지 기존 세션을 사용할지 알 수있는 부울 값
 

#### 주사위보기 정리
 

모든 조각이 연결된 상태에서 이제 Fluid Framework를 고려하기 위해 원래 만든보기를 수정하기 만하면됩니다.
 이전에 만든 원래`renderDiceRoller` 함수를 수정하면 다음과 같이 표시됩니다.
 

```js
export function renderDiceRoller(diceRoller: IDiceRoller, div: HTMLDivElement) {
    const wrapperDiv = document.createElement("div");
    wrapperDiv.style.textAlign = "center";
    div.append(wrapperDiv);
    const diceCharDiv = document.createElement("div");
    diceCharDiv.style.fontSize = "200px";
    const rollButton = document.createElement("button");
    rollButton.style.fontSize = "50px";
    rollButton.textContent = "Roll";

    // Call the roll method to modify the shared data when the button is clicked.
    rollButton.addEventListener("click", diceRoller.roll);
    wrapperDiv.append(diceCharDiv, rollButton);

    // Get the current value of the shared data to update the view whenever it changes.
    const updateDiceChar = () => {
        // Unicode 0x2680-0x2685 are the sides of a die (⚀⚁⚂⚃⚄⚅).
        diceCharDiv.textContent = String.fromCodePoint(0x267F + diceRoller.value);
        diceCharDiv.style.color = `hsl(${diceRoller.value * 60}, 70%, 50%)`;
    };
    updateDiceChar();

    // Use the diceRolled event to trigger the re-render whenever the value changes.
    diceRoller.on("diceRolled", updateDiceChar);
}
```

여기에서 우리는 이제`diceRoller` 값을 함수에 전달하고 있음을 알 수 있습니다.
 이것은 Fluid Framework에 의해 업데이트되고 Dice가 롤링 될 때 이미지가 어떻게 보이도록 업데이트할지 뷰에 알려줍니다.
 

이 모든 작업을 확인하려면 여기에서 프로젝트 저장소의`git clone`을 수행 한 다음 터미널에서 열고 먼저`npm install`을 실행 한 다음`npm run start`를 실행하여 서버를 시작합니다.
 웹 브라우저를`localhost : 8080`으로 열고 렌더링 된 주사위가 보이면 URL을 복사하고 두 번째 탭을 열어 Fluid Framework가 두 탭을 동기화 상태로 유지하는지 확인합니다.
 

여기의 탭은 독립 클라이언트가 Fluid Containers 및 Fluid Service를 사용하여 보유한 앱에 연결했을 때 표시되는 내용을 모방합니다.
 여기에서 실제로 작동하는 것을 확인하십시오.
 

## 마지막 생각들
 verified_user

이 게시물에서는 Microsoft의 Fluid Framework를 소개하고 응용 프로그램에서이 기술을 사용하는 방법에 대해 설명했습니다.
 우리는 기술의 작동 방식과 유체 컨테이너 및 유체 서비스를 포함하여 관련된 부분에 대해 Dice Roller 샘플 프로젝트를 살펴 보았습니다.
 

이 게시물은 실제로이 기술의 가능성의 표면에 도달했습니다.
 많은 사람들이 원격으로 작업하고 온라인 공동 작업이 가장 중요한 시대에 Fluid Framework는 이러한 종류의 실시간 통신을 가능하게하는 확실한 경로를 제공합니다.
 

Teams 및 Office 365를 통한 Microsoft의 성공은이 기술이 얼마나 유용 할 수 있는지를 잘 보여줍니다.
 또한 Fluid Framework를 쉽게 가져 와서 자신 만의 응용 프로그램을 구축 할 수 있다는 점도 시작하는 데 큰 동기가됩니다.
 

Microsoft는 최근 Fluid Framework 오픈 소스를 만들었습니다 (여기에서 자세한 내용 참조).
 이제 모든 개발자가 소스 코드를 사용할 수 있으므로이 기술은 미래에 큰 잠재력을 가지고 있습니다.
 

이 게시물을 즐겁게 읽으 셨기를 바라며 Fluid Framework에 대해 더 많이 배우고 싶습니다.
 자세한 내용은 Fluid Framework 웹 사이트를 확인하는 것이 좋습니다.
 

내 게시물을 읽어 주셔서 감사합니다!
 andrewevans.dev에서 저를 팔로우하고 Twitter @ AndrewEvans0102에서 저와 연결해주세요.
 