---
layout: post
title: "Nux.js의 구성 요소 테스트"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/componenttestinginnuxtjs.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/componenttestinginnuxtjs.png?fit=730%2C487&ssl=1)

최근 Vue는 Vue와 함께 강력하고 보편적인 애플리케이션을 구축하는 데 사용되는 프레임워크인 Nux.js에 대한 관심을 불러일으켰다. 강력한 애플리케이션을 구축하는 동안 디버깅과 코드 리팩토링에 소요되는 시간을 줄일 수 있으므로 테스트를 향한 명확한 경로를 제공하는 것이 중요합니다. 이 기사에서는 Nux.js로 게임 스토어 애플리케이션을 설정하고 그 구성 요소를 테스트하는 방법을 살펴보겠습니다. 이 게시물을 이해하는 데 있어서 가장 중요한 전제조건은 Vue와 Nux.js의 기본 지식이다.

## Nux.js 응용 프로그램 설정

Nux.js를 사용하여 애플리케이션을 만드는 첫 번째 단계는 프로젝트 폴더에 애플리케이션을 설치하는 것입니다. 터미널에서 프로젝트 폴더로 이동하여 다음 명령을 입력하겠습니다.

```coffeescript
npm install nuxt
```

여전히 터미널을 사용하고 있습니다. 프로젝트 폴더에서 npx를 통해 애플리케이션을 생성합니다(npm 5.2.0 이후 기본적으로 제공됨).

```undefined
npx create-nuxt-app game-store
```

옵션 목록을 살펴봅니다. 여기에서는 옵션이 다를 수 있습니다. 아래에서는 우리가 작업할 애플리케이션을 만들 때 선택한 사항에 대해 자세히 설명하는 안내서를 제공합니다.

```css
? Project name: game-store
? Programming language: JavaScript
? Package manager: NPM
? UI Framework: None
? Nuxt.js modules: None
? Linting tools: None
? Testing framework: Jest
? Rendering mode: Single Page App
? Deployment target: Static
? Development tools: jsconfig.json
? Version Control System: Git
```

애플리케이션 생성이 완료되면 터미널을 통해 애플리케이션으로 이동하여 브라우저에서 실행할 수 있습니다.

```bash
cd game-stores
npm run dev
```

이 작업이 완료되면 다음 창이 나타납니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/gamestorevuewindow.png?resize=730%2C457&ssl=1)

또한 다음과 유사한 프로젝트 폴더 구조를 가져야 합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/filelayout.png?resize=465%2C574&ssl=1)

## 스토어 구성 중

상태를 효율적으로 관리하기 위해 Nux는 Vuex의 기능을 활용할 수 있습니다. 이렇게 하면 `/store` 디렉토리에 생성된 모든 파일을 Vuex 모듈로 처리할 수 있습니다(즉, 자체 상태, 돌연변이, 작업 및 게터 포함). 응용 프로그램의 시작점으로 스토어 디렉토리를 사용할 것입니다. 먼저 필요한 데이터를 포함하는 것으로 시작하겠습니다. 샘플은 다음과 같습니다.

```undefined
// store/games.js

const games = [
    {
      title: "Star Wars Battlefront 2",
      console: "PlayStation 4",
      rating: 7,
      price: 15.30,
      photo: 'https://res.cloudinary.com/fullstackmafia/image/upload/v1604990005/SWBF2_box_or6x8s.jpg'
    },

    {
      title: "BioShock: The Collection",
      console: "PlayStation 4",
      rating: 9,
      price: 16.00,
      photo: 'https://res.cloudinary.com/fullstackmafia/image/upload/v1604990078/220px-BioShock-_The_Collection_tix1ol.jpg'
    },

    {
      title: "Call of Duty: Black Ops 4",
      console: "PlayStation 4",
      rating: 9,
      price: 11.70,
      photo: 'https://res.cloudinary.com/fullstackmafia/image/upload/v1604990123/220px-Call_of_Duty_Black_Ops_4_official_box_art_vvhd7w.jpg'
    },

    {
      title: "Tom Clancy's Rainbow Six: Siege",
      console: "PlayStation 5",
      rating: 9,
      price: 13.90,
      photo: 'https://res.cloudinary.com/fullstackmafia/image/upload/v1604990231/56c494ad88a7e300458b4d5a_qeyro6.jpg'
    }
  ];
```

다음으로, 이 파일의 상태, 돌연변이, 동작 및 게터를 구성합니다. 저희 상점은 PlayStation 4 제목만 표시하기를 원합니다.

```js
// store/games/games.js

 const state = () => {
    return games;
  };
  
  const mutations = {
  };
  
  const actions = {};
  
  const getters = {
    bestGames (state) {
        return state.filter(({ rating }) => {
          return rating === 9
        });
    },
    playstationfour(state) {
      return state.filter(({ console }) => {
        return console === 'PlayStation 4'
      });
    },

     consoleType (state) {
      return (consoleName) => {
        return state.filter(({ console }) => {
          return console === consoleName
        });
      }
    },

    cheapGames(state) {
      return state.filter(({ price }) => {
        return price === 15.30
      });
    }
  };
  
  export default { state, mutations, actions, getters };
```

다음으로, 저희 가게의 상태를 브라우저에 매핑하겠습니다. 이 작업은 `pages/index.vue`에 있는 기본 보기를 교체하여 수행됩니다.

```xml
<!-- pages/index.vue -->
<template>
      <v-flex xs4 v-for="game in psfourGames" :key="game.title">
        <v-card>
          <v-img :src="game.photo" aspect-ratio="1"></v-img>
          <v-card-title primary-title>
            <div>
              <h3>{game.title}</h3>
              <h4>Rating: {game.rating}</h4>
              <h4>Price: ${game.price}</h4>
            </div>
          </v-card-title>
        </v-card>
      </v-flex>
</template>
```

그런 다음 Vuex의 `MapGetter` 도우미를 사용하여 `games.js`에서 이전에 정의한 Getter를 `index.vue` 파일의 계산 속성에 매핑합니다.

```xml
<!-- pages/index.vue -->
<script>
import { mapGetters } from 'vuex'
export default {
  computed: {
    ...mapGetters({
      consoleType: 'games/games/consoleType'
    }),
    psfourGames () {
      return this.consoleType('PlayStation 4')
    }
  }
}
</script>
```

브라우저에 표시되는 방법을 살펴보겠습니다. 터미널로 이동하여 `npm run dev`를 실행하십시오. 브라우저 보기는 다음과 같아야 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/gamesui.png?resize=730%2C456&ssl=1)

## 테스트 프레임워크 구성

응용 프로그램의 테스트 프레임워크는 Jest이다(이 프레임워크는 설정 중에 이전에 선택되었다). 앞서 살펴본 바와 같이 Nux는 스토어의 모든 컨텐츠를 Vuex 모듈로 구축합니다. 여기서 목표는 다음과 같은 기능을 갖추는 것입니다.

- 다양한 기능을 담당하는 다양한 스토어를 보유합니다.
- 구성 요소에 사용되는 것과 동일한 방식으로 이러한 스토어를 테스트할 수 있습니다(테스트할 특정 스토어를 선택하십시오).

이를 위해 모든 테스트가 실행되기 전에 한 번 트리거되는 비동기 기능을 내보내는 `global Setup` 모듈을 사용하도록 Jest를 구성합니다. 이렇게 하면 테스트할 특정 스토어를 선택할 수 있습니다. 아래의 Jest 구성 파일에서 테스트를 실행하기 전에 먼저 Jest 설정 파일을 실행하도록 `global Setup` 모듈을 설정합니다.

```java
// jest.config.js

module.exports = {
  globalSetup: "<rootDir>/jest.setup.js",  *****
  moduleNameMapper: {
    '^@/(.*)$': '<rootDir>/$1',
    '^~/(.*)$': '<rootDir>/$1',
    '^vue$': 'vue/dist/vue.common.js'
  },
  moduleFileExtensions: [
    'js',
    'vue',
    'json'
  ],
  transform: {
    '^.+\\.js$': 'babel-jest',
    '.*\\.(vue)$': 'vue-jest'
  },
  collectCoverage: true,
  collectCoverageFrom: [
    '<rootDir>/components/**/*.vue',
    '<rootDir>/pages/**/*.vue'
  ]
}
```

다음은 `제스트`를 만들겠습니다.setup.js` 파일에서 프로세스 변수를 통해 스토어의 디렉터리를 노출합니다.

```js
import { Nuxt, Builder } from "nuxt"
import nuxtConfig from "./nuxt.config"

const resetConfig = {
  loading: false,
  loadingIndicator: false,
  fetch: {
    client: false,
    server: false
  },
  features: {
    store: true,
    layouts: false,
    meta: false,
    middleware: false,
    transitions: false,
    deprecations: false,
    validate: false,
    asyncData: false,
    fetch: false,
    clientOnline: false,
    clientPrefetch: false,
    clientUseUrl: false,
    componentAliases: false,
    componentClientOnly: false
  },
  build: {
    indicator: false,
    terser: false
  }
}

const config = Object.assign({}, nuxtConfig, resetConfig, {
  mode: "spa",
  srcDir: nuxtConfig.srcDir,
  ignore: ["**/components/**/*", "**/layouts/**/*", "**/pages/**/*"]
})

const buildNuxt = async () => {
  const nuxt = new Nuxt(config)
  await new Builder(nuxt).build()
  return nuxt
}

module.exports = async () => {
  const nuxt = await buildNuxt()

  process.env.buildDir = nuxt.options.buildDir
}
```

위의 설정 파일에서 `resetConfig`는 빌드 프로세스를 실행할 때 스토어만 빌드되도록 합니다. 그런 다음 `process.env.buildDir`를 사용하여 스토어의 경로를 노출합니다. 이 작업이 완료되면 저희 매장에 대한 테스트 작성을 진행합니다.

```js
// store/games.test.js

import _ from "lodash"
import Vuex from "vuex"
import { createLocalVue } from "@vue/test-utils"
describe("store/games/games", () => {
  const localVue = createLocalVue()
  localVue.use(Vuex)
  let NuxtStore
  let store
  beforeAll(async () => {
    const storePath = `${process.env.buildDir}/store.js`
    NuxtStore = await import(storePath)
  })
  beforeEach(async () => {
    store = await NuxtStore.createStore()
  })
}
```

위의 필기시험에서 우리는 제스트의 before All 블록을 사용하여 우리가 만든 상점을 수입했다. beforeEach 블록은 별도의 테스트를 실행할 때마다 새 저장소를 생성하도록 보장합니다. 다음으로, 우리는 우리의 지원서에 대해 우리가 원하는 구체적인 테스트를 작성할 것입니다. 다음과 같은 특정 기준 세트를 원한다고 가정합시다.

- 비디오 게임 DOM은 플레이스테이션 4 타이틀로만 나온다.
- 비디오 게임 스타워즈 배틀프론트 2의 가격은 정확히 15달러 30센트이다.
- 등급이 9인 비디오 게임만 표시할 저장소

```coffeescript
describe("consoleType", () => {
    let playstationfour
    beforeEach(() => {
      playstationfour = store.getters['games/games/playstationfour']
    })
    test("DOOM should be on only playStation 4", () => {
      expect(playstationfour).toEqual(
        expect.arrayContaining([
          expect.objectContaining({
            console: 'PlayStation 4',
            title: 'DOOM'
          })
        ])
      )
    })
  })

  describe('cheapGames', () => {
    let cheapGames
    beforeEach(() => {
      cheapGames = store.getters['games/games/cheapGames']
    })
    test(`StarWars BattleFront must cost exactly ${15.30}`, () => {
      expect(cheapGames).toEqual(
        expect.arrayContaining([
          expect.objectContaining({
            price: 15.30
          })
        ])
      )
    })
  })

  describe('bestGames', () => {
    let bestGames
    beforeEach(() => {
      bestGames = store.getters['games/games/bestGames']
    })
    test('Display only the best titles we have', () => {
      expect(bestGames).toEqual(
        expect.arrayContaining([
          expect.objectContaining({
            rating: 9
          })
        ])
      )
    })
  })
```

테스트를 한 번 해보겠습니다. 터미널로 이동하여 `npm 테스트`를 실행하십시오. 이 테스트는 지정된 모든 테스트를 실행하고 예상 결과를 제공합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/passedtest.png?resize=730%2C210&ssl=1)

## 요약

보편적 응용 프로그램을 위한 쓰기 시험은 번거롭게 보일 수 있다. 일반적으로 테스트를 단순하고 간결하게 유지하는 것이 원칙입니다. 이 가이드가 이를 지원할 수 있습니다. 데모 코드를 보려면 GitHub 링크를 클릭하십시오. 자세한 내용은 이 링크를 참조하십시오.