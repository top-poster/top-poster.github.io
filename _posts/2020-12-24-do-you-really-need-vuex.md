---
layout: post
title: "Vuex가 정말 필요하세요?"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/vue-vuex.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/vue-vuex.png?fit=730%2C487&ssl=1)

Vue는 이미 주요 JavaScript 프레임워크로 설정되어 있습니다. 원활한 개발자 경험부터 광범위한 에코시스템에 이르기까지 다양한 이유 때문이다. Vue의 핵심 에코시스템과 툴링은 특히 훌륭하며, 이를 완전한 프레임워크라고 부를 수 있는 자격이 있습니다.

Vue는 다재다능하기 위해 구축되었지만, 이러한 다재다능성은 신규 및 특정 노련한 개발자에 의해 종종 잘못 해석되며, 이는 "나중에 필요할 수 있다"고 생각하기 때문에 많은 도구를 포함하는 경향이 있습니다. 실제로 이러한 툴이 항상 필요한 것은 아닙니다. 그러므로 도구를 추가하기 전에 우리에게 어떤 이익을 줄 수 있는지 이해하는 것이 매우 중요하다. 그렇지 않으면 도구가 도움이 되는 어떤 것보다 단점으로 바뀔 수 있다.

이제 Vue.js:Vuex의 코어 플럭스 라이브러리를 살펴보겠습니다.

Vuex는 Vue.js의 상태 관리 시스템입니다. Vue 응용 프로그램에 있는 모든 구성 요소의 상태를 관리하기 위해 엄격한 규칙을 따르는 중앙 집중식 저장소입니다.

굉장히 중요하게 들리는데요. 그렇죠? 실제로는 사용 사례에 따라 도움이 될 수도 있고 그렇지 않을 수도 있는 강력한 도구입니다.

## 왜 Vuex가 필요하죠?

Vuex는 상태 관리 시스템입니다. Vuex가 무엇을 하는지 이해하기 위해서는 먼저 SPA의 상태 개념과 SPA가 다른 요소와 어떻게 상호 작용하는지를 잘 파악해야 합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/actions-state-view-circular-relationship.png?resize=730%2C494&ssl=1)

기본적으로 상태는 구성요소가 의존하는 데이터입니다. 예를 들어, 작업관리 목록의 작업관리 또는 블로그의 게시물 등이 있습니다. 이 데이터를 상태라고 하며 각 Vue 구성 요소는 하나씩 가질 수 있습니다.

앱이 확장되면 구성 요소가 크게 증가할 수 있습니다. Vuex와 같은 상태 관리 라이브러리를 사용하지 않는 경우 이러한 각 구성 요소는 해당 목적을 위해 설계된 Vue.js API를 활용하여 자체 상태를 관리합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/vuejs-component-tree.png?resize=654%2C434&ssl=1)

Vue는 위와 같은 나무와 같은 구성요소 아키텍처를 유지하고, 또한 `props`를 통한 부모-자녀간 의사소통과 `이벤트`를 통한 자녀간 의사소통을 가능하게 한다는 것을 명심해야 한다. 두 개의 먼 형제 구성 요소가 동일한 데이터 변경에 대응해야 하는 경우 얼마나 어려울지 짐작할 수 있습니다. 가장 가까운 상위 구성 요소와 상위 구성 요소 사이에 데이터를 전달해야 합니다(위의 그림에 강조 표시된 구성 요소 참조).

이것은 매우 힘들고 실수하기 쉽다.

그것이 바로 Vuex가 나오는 곳이다. Vuex는 전체 모듈의 상태를 추상화하여 중앙 저장소에 보관하는 작업을 수행하며 Vuex의 상태 관리 패턴을 따르고 그에 따라 구성 요소와 통신합니다.

그것이 종종 "진실의 단일 출처"라고 불리는 이유이다. Vuex는 이와 같이 별칭 관계이지만 구성 요소가 여전히 자체 상태를 유지할 수 있다는 사실은 변하지 않습니다. 이러한 상태를 다른 구성 요소와 공유하도록 결정할 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/sharing-states-components-vuex.png?resize=635%2C468&ssl=1)

이제 Vuex와 같은 상태 관리 시스템이 필요한 이유를 이해하셨으니 이제 이러한 과감한 가정을 할 수 있습니다. Vue를 사용하여 상태 관리 여부를 결정하는 주요 요인은 두 가지뿐입니다.

## 앱 크기

사람들은 종종 "앱이 커지면 Vuex가 필요하다"고 말한다. 사실, 그것은 자라는 방법에 따라 다릅니다. 앱은 증가할 수 있지만 데이터 흐름은 핵(예: 부모-자녀 및 가까운 형제)을 유지합니다.

결국, Vuex와 함께 제공되는 보일러 플레이트를 추가하지 않고도 소품 및 이벤트를 사용하여 이 데이터를 공유할 수 있습니다.

한편, 구성요소에 추상적인 데이터 소스가 있어야 한다는 것을 알게 되는 즉시 소품 및 이벤트 사용을 중단하고 Vuex에 의존하는 것이 현명합니다. 이렇게 하면 엄청난 시간을 절약할 수 있습니다. 왜냐하면 나중에 Vuex로 전환할수록 리팩터링이 더 많이 필요하기 때문입니다.

## 논리학

때때로 우리는 특정한 복잡한 논리의 구현을 우리의 요소들로부터 분리해야 할 필요가 있다. 특히 이 논리가 우리 시스템의 다른 요소들과 관련이 있는 경우 말이다. 이 사용 사례의 좋은 예는 인증입니다.

Vuex는 Vue에서 복잡한 앱의 인증을 처리하는 인기 있는 방법입니다. Vuex를 사용하면 토큰의 가용성과 액세스 제어 및 앱 전체 경로 블록을 처리할 수 있습니다. 돌연변이, 게터, 세터 등이 이 작업에 도움이 됩니다.

이 패턴을 사용하면 인증 흐름을 추적할 수 있고 데이터에 액세스할 수 있도록 유지하면서 인증 논리와 앱의 논리를 완전히 분리할 수 있습니다.

Vuex는 또한 전체 앱에서 단위 테스트를 간소화합니다. 앱이 복잡해져서 상태 관리가 어려울 경우 도움이 될 수 있습니다. 하지만 Vuex를 사용하지 않고도 완벽하게 테스트를 완료할 수 있습니다.

## 정확히 언제 Vuex가 필요할까요?

다음은 Dan Abramov의 재미있는 인용구입니다.

> 플럭스 도서관은 안경과 같다: 여러분이 필요할 때 알게 될 것이다.

이것은 매우 진실된 진술이다. 하지만 어떤 사람들은 플럭스 라이브러리가 필요하다는 것을 너무 늦게 알아차릴 수 있다고 말합니다. 불행히도, 이렇게 하면 이미 복잡한 애플리케이션에 복잡한 흐름을 도입할 수밖에 없습니다.

위에서 언급한 두 가지 주요 요인(앱 크기 및 데이터 복잡성)을 바탕으로 개발자가 Vuex에서 앱을 오버차징해야 하는지 프로젝트 계획 단계에서 바로 결정할 수 있도록 돕기 위해 이 작은 흐름도를 만들었습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/vuex-decision-map.png?resize=730%2C370&ssl=1)

Vuex 문서에는 Vuex를 사용해야 할 시기를 간략하게 설명하는 섹션도 있습니다.

## 왜 Vuex를 빼먹어?

Vue-CLI를 사용하여 새 Vue.js 앱을 만들 때 상태 관리 시스템이 필요한지 묻는 방식으로 버그를 일으키지 않습니다. 그것은 단지 그것을 포함하지 않는다. Vue.js는 매우 다재다능하지만 매우 접근성이 높은 것을 목표로 하고 있으며, 새로운 프로젝트를 최대한 간단하게 만들려고 노력하고 있습니다.

사람들이 Vuex를 피하는 주된 이유는 Vuex가 장황하기 때문이다. 이 방법은 - 상태를 의도적으로 변경하기 위해 패턴을 따라야 합니다. 다음은 Vuex를 사용하는 기본 작업 구성 요소와 그렇지 않은 작업 구성 요소입니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/comparing-todo-component-vuex.png?resize=730%2C497&ssl=1)

위의 코드는 Vuex를 사용하여 매우 간단한 앱에서 상태를 관리할 때 얼마나 번거로워지는지 보여줍니다. 이와 같은 경우에는 Vuex를 건너뛰고 소품 및 이벤트 또는 다른 간단한 상태 관리 패턴에 의존하는 것이 가장 좋습니다.

## Vuex에서 생략하는 방법

중간에서 작은 앱으로 작업하는 동안 Vuex의 다공성에 말려들지 않으려면 탐색할 수 있는 몇 가지 옵션이 있습니다. 이러한 방법 중 일부는 간단한 상태 관리를 처음부터 작성해야 하며, 다른 방법은 타사 라이브러리를 사용해야 합니다. 코드 구조에 따라 가장 적합한 기능을 사용할 수 있습니다.

### 기타 플럭스 라이브러리

Vuex 외에 다른 플럭스 라이브러리를 사용하는 것도 나쁘지 않다. 그것들은 같은 목적을 위해 사용될 수 있지만, 종종 용도가 다르다. 일부는 Vuex에 비해 사용 사례에 맞는 보일러 플레이트를 적게 제공할 수 있습니다.

### 처음부터 플럭스

저는 이 방법을 따르는 것보다 Vuex를 선택하는 것을 추천합니다. 현실적으로는 Vuex를 다시 만들 수 있기 때문입니다. 그러나 "사용자 정의" 소스가 필요한 경우 "Vue.reactive" 방법을 사용하여 직접 구현할 수도 있습니다.

그것이 믿을 수 있는 진리의 원천이 되기 위해서는, 변화가 돌연변이에 의해서만 일어나야 한다는 것을 명심하세요. 그렇지 않으면 여러분은 추적할 수 없는 벌레들을 발견할 수 있을 것입니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/shared-state-built-from-scratch.png?resize=730%2C730&ssl=1)

Vue는 문서에서 개발자가 자체 중앙 집중식 상태 관리 시스템을 만들 수 있도록 지원하는 간단한 가이드를 제공합니다.

### 이벤트 버스

또한 Vue를 사용하면 글로벌 이벤트 버스를 통해 앱 전체에 데이터를 전달할 수 있습니다. 이 문서를 읽고 이 방법에 대한 자세한 자습서 https://blog.logrocket.com/using-event-bus-in-vue-js-to-pass-data-between-components/을 볼 수 있습니다.

Vue 3을 사용하는 경우, 문서에 제시된 대로 이벤트 허브 역할을 하기 위해 작은 송신기나 미트 같은 타사 라이브러리를 사용하는 것이 좋습니다.

### 구성 API

Vue의 구성 API의 동기는 Vue 앱에서 더 나은 논리 재사용과 코드 구성을 가능하게 하는 것이다. 구성 API는 구성 요소의 로직을 유연하게 구성할 수 있는 모듈형 패턴을 Vue에 제공합니다.

안드레즈 갈루프가 설명한 이 우아한 패턴으로 컴포지션 API를 스토어로 활용할 수 있다. 나중에 공유 저장소로 사용할 함수를 구성할 수 있습니다. 이렇게 하면 Vuex가 가져올 수 있는 것보다 더 작은 앱의 보일러 플레이트가 크게 줄어듭니다.

```js
// @/services/counter.js
import { ref, computed } from '@vue/composition-api'
const count = ref(0)
export const getCount = computed(() => count.value)
export const increment = () => count.value += 1
```

```xml
// @/components/ComponentA.vue
<template>
    <div id="component-a">
        <button @click="increment()">Increment Counter</button>
    </div>
</template>

<script>
import { createComponent } from '@vue/composition-api'
import { increment } from '@/services/click-counter'

export default createComponent({
    setup() {
        return {
            increment
        }
    }
})
</script>
```

```xml
// @/components/ComponentB.vue
<template>
    <div id="component-b">
        Clicks: { getCount }
    </div>
</template>

<script lang="ts">
import { createComponent } from '@vue/composition-api'
import { getCount } from '@/services/click-counter'

export default createComponent({
    setup() {
        return {
            getCount
        }
    }
})
</script>
```

### 플러그인

Vuex에는 또한 보일러 플레이트를 단순화하는 데 도움이 될 수 있는 플러그인도 있습니다. 예를 들어, Getters 속성을 사용하지 않고 상태의 심층 속성에 액세스할 수 있도록 도와주는 Vuex-pathify 플러그인이 있습니다. 경로를 사용하여 액세스할 수 있습니다. 예를 들어 `products@items.0.name`을 사용할 수 있습니다. Vuex pathify를 사용하면 단일 경로로 돌연변이를 설정할 수도 있습니다.

## 결론

Vuex가 훌륭한 도구라는 것에는 의심의 여지가 없다. 하지만, 큰 힘과 함께 큰 책임이 따른다. 이러한 책임 중 하나는 그 힘을 이해하고 더 복잡한 코드베이스를 갖는 대가를 치르고 활용 여부를 결정하는 것입니다.

Vue의 이벤트 및 소품 데이터 통신 패턴으로 제공되는 대부분의 간단한 애플리케이션도 괜찮습니다. Vuex를 추가하면 이러한 앱은 훨씬 더 복잡해지는 경향이 있습니다. 또한 Vue.js는 구성 API 및 최상위 계층 반응성과 같은 훌륭한 도구를 제공하므로 Vuex보다 훨씬 더 우수하고 간단한 방법으로 작업을 수행할 수 있습니다.

다음에 멋진 Vue 프로젝트를 진행할 때는 추가하기 전에 Vuex가 실제로 필요한지 확인하십시오.