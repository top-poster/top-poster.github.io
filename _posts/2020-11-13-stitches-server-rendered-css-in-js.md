---
layout: post
title: "실밥: 현대의 서버 렌더링 CSS-in-JS 라이브러리"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/react-stitches-feature-image.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/react-stitches-feature-image.png?fit=730%2C487&ssl=1)

CSS-in-JS를 활용한 스타일링 컴포넌트는 2014년 처음 선보여 인기를 이어가고 있다. 개발자 커뮤니티의 광범위한 패턴 채택은 라이브러리 제조업체들이 CSS-in-JS 라이브러리에 중요한 개념을 결정하는 데 도움을 주었다.

예를 들어, Silves는 컴포넌트 재사용과 서버측 렌더링을 위한 변형을 만드는 것과 같은 가장 최근의 컴포넌트 스타일링 트렌드를 핵심 기능으로 삼는 CSS-in-JS 라이브러리이다. 컴포넌트 기반 아키텍처와 개발자 경험에 초점을 맞춘 완전한 형식의 CSS-in-JS 라이브러리이다.

다른 CSS-in-JS 라이브러리와 마찬가지로 실밥은 중요한 CSS 주입과 자동 벤더 접두사의 일반적인 장점을 가지고 있다. 그러나 다른 CSS-in-JS 라이브러리와 비교했을 때, Silves는 다음과 같은 특장점 때문에 눈에 띈다.

### 성과

실밥은 런타임에 불필요한 프로퍼 보간을 방지하여 다른 스타일링 라이브러리보다 훨씬 뛰어난 성능을 제공합니다.

### 서버측 렌더링

실밥은 응답성이 뛰어난 스타일과 변형에 대해서도 크로스 브라우저 서버 측 렌더링을 지원합니다.

### 변종

차종에서 최고의 지원을 제공하므로 컴포지트 가능한 구성 요소 API를 설계할 수 있습니다.

### 메잉

CSS 변수로 여러 테마를 정의한 다음 클래스 이름을 정의하여 구성 요소에 사용

### 특수성

그것의 원자 출력 때문에, 구체성 문제는 과거의 것이다.

### 개발자 경험

토큰 인식 속성, 중단점 도우미 및 사용자 지정 유틸리티와 함께 매우 유용한 구성 파일이 있습니다. 실밥은 재미있고 직관적인 DX를 제공합니다.

Silbs는 프레임워크에 구애받지 않도록 설계되었지만, 작성 당시에는 React만 지원하며 Vue에 대한 지원이 진행 중입니다.

## 실밥 시작

Shilts with React를 사용하려면 패키지 관리자에게 라이브러리를 설치해야 합니다.

```coffeescript
# With npm
npm install @stitches/react

# With yarn
yarn add @stitches/react
```

그런 다음 구성 파일을 생성하고 설계 시스템의 구성을 정의해야 합니다. typeScript를 사용하지 않는 경우 `stches.config.ts` 파일(또는 `.js`)을 생성하고 라이브러리에서 `createStyled` 함수를 가져옵니다.

`createStyled` 함수는 반응 후크 함수처럼 작동합니다. 다음과 같은 선택적 속성을 가진 구성 개체를 수신합니다.

- 충돌을 피하기 위해 모든 클래스 이름에 접두사를 사용합니다.
- `토큰`: CSS 값으로 정의하고 적용할 수 있는 특수 변수
- `breakpoint`: 응답성이 뛰어난 중단점을 만들어 대응 스타일 작성
- `utils`: CSS 속성을 쓰는 데 필요한 단축키 역할을 하는 사용자 지정 함수를 만듭니다.

스타일링 요구 사항을 충족하려면 다음 두 가지 기능을 사용하십시오.

- `tyled`: 스타일을 사용하여 React 구성 요소를 생성하는 함수
- `css`: 테마 및 SSR 스타일을 생성하는 함수

```js
// stitches.config.ts
import { createStyled } from '@stitches/react';export const { styled, css } = createStyled({
prefix: '',
tokens: {},
breakpoints: {},
utils: {},
});
```

구성 속성은 나중에 검토하겠습니다. 지금은 Silbs를 구현하고 스타일링된 구성 요소를 렌더링하는 데 초점을 맞추도록 하겠습니다.

`stches.config` 파일을 구성 요소로 가져와야 하므로 Create-Ract-App을 사용하는 경우 `src/` 디렉토리에 저장하는 것을 잊지 마십시오.

스타일링 버튼 구성 요소를 생성하여 실밥을 테스트해 보겠습니다. 새 구성 요소 파일을 생성하고 맨 위에 있는 구성에서 `스타일링`을 가져옵니다.

```undefined
// Change the import to where you put your stitches.config file
import { styled } from '../stitches.config';
```

이제 단추의 스타일을 쓰십시오. Seachs는 스타일링 구성 요소와 같은 템플릿 문자열 구문을 사용하는 대신 일반 개체 구문을 사용하여 스타일링 패턴을 구현하여 번들 크기를 줄이는 방법을 선택합니다.

```js
import { styled } from '../stitches.config';

const Button = styled('button', {
  color: '#fff',
  backgroundColor: '#007bff',
  borderRadius: '10px',
  fontSize: '16px',
  fontWeight: 400,
  paddingTop: '10px',
  paddingBottom: '10px',
  paddingLeft: '16px',
  paddingRight: '16px',
  '&:hover': {
    backgroundColor: '#0069d9',
  },
});

export default Button;
```

이제 구성 요소를 가져오면 다음을 렌더링할 수 있습니다.

```js
import React from 'react';
import ReactDOM from 'react-dom';
import Button from './components/Button'

function App() {
  return (
    <Button>This button is styled using Stitches</Button>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

그리고 그게 다야. 이제 화면에 실밥 단추 구성 요소가 렌더링됩니다.

다음에는 다양한 버전의 구성 요소를 생성하는 방법에 대해 알아보겠습니다.

## 내장형 변형 서포트 바느질

Silves의 주요 기능 중 하나는 동일한 구성 요소의 여러 변형을 일류 API로 쓰는 지원이다. 스타일링 객체 구문 내에 변형을 직접 쓸 수 있으며, 이 구문은 해당 구성요소의 소품으로 컴파일됩니다. 다음은 동일한 버튼 구성 요소이지만 `color` 변형은 다음과 같습니다.

```undefined
const Button = styled('button', {
  color: '#fff',
  backgroundColor: '#007bff',
  borderRadius: '10px',
  fontSize: '16px',
  fontWeight: 400,
  paddingTop: '10px',
  paddingBottom: '10px',
  paddingLeft: '16px',
  paddingRight: '16px',
  '&:hover': {
    backgroundColor: '#0069d9',
  },
  variants: {
    color: {
      violet: {
        backgroundColor: 'blueviolet',
        ':hover': {
          backgroundColor: 'darkviolet',
        },
      },
      gray: {
        color: '#000',
        backgroundColor: 'gainsboro',
        ':hover': {
          backgroundColor: 'lightgray',
        },
      },
    },
  }
});
```

버튼을 렌더링할 때는 색상을 소품으로 지정하기만 하면 됩니다.

```xml
<div style={ display: 'flex', gap: '20px' }>
  <Button color="violet">Violet button</Button>
  <Button color="gray">Gray button</Button>
</div>
```

그에 따라 다음과 같이 제공됩니다.

자세한 내용은 실밥의 변형 설명서를 참조하십시오. 이제 변형 지원에 대해 알았으니 구성 속성으로 넘어갑시다.

## 실밥의 구성 속성

앞에서 살펴본 것처럼 `createStyled` 함수를 호출할 때 설정할 수 있는 구성 속성은 다음과 같습니다.

- 접두사를 붙이다
- 토큰
- 중단점
- 공리품

이러한 구성을 통해 개발자 환경을 개선하는 방법에 대해 알아봅니다.

### 1. 접두사 구성

접두사 구성은 CSS 충돌을 방지하기 위해 Silts에 의해 생성된 각 클래스 이름에 접두사만 추가합니다.

```js
export const { styled, css } = createStyled({
  prefix: 'zxc',
  tokens: {},
  breakpoints: {},
  utils: {},
});
```

브라우저에서 요소를 검사하여 접두사를 볼 수 있습니다. 결과는 다음과 같습니다.

```undefined
<button class="zxc__initial_bc_hiMOlA zxc__initial_bc_cfnJEG zxc__initial_c_kFTTvV zxc__initial_bblr_eEqHhd zxc__initial_btlr_fAvRqR zxc__initial_btrr_hGRUya zxc__initial_bbrr_iAiVRy zxc__initial_fs_kBiqwx zxc__initial_fw_cftqkj zxc__initial_pt_keBEHr zxc__initial_pb_ddiFNf zxc__initial_pl_frIgGZ zxc__initial_pr_eOnNpm scid-bZicNS">
  Violet button
</button>
```

### 2. 토큰 구성

토큰 구성을 사용하면 CSS 값을 보유하는 변수 역할을 하는 재사용 가능한 설계 토큰을 쓸 수 있습니다. 다음은 `색상` 및 `fontSize` 토큰 유형을 정의하는 방법의 예입니다.

```bash
export const { styled, css } = createStyled({
  tokens: {
    colors: {
      $gray500: 'hsl(206,10%,76%)',
      $blue500: 'hsl(206,100%,50%)',
      $purple500: 'hsl(252,78%,60%)',
      $green500: 'hsl(148,60%,60%)',
      $red500: 'hsl(352,100%,62%)',
    },
    fontSizes: {
      $1: '12px',
      $2: '13px',
      $3: '15px',
    },
  },
});
```

구성 요소에서 직접 토큰을 사용할 수 있습니다.

```undefined
const Button = styled('button', {
  color: '#fff',
  backgroundColor: '$red500',
  borderRadius: '10px',
  fontSize: '$3',
  fontWeight: 400,
  paddingTop: '10px',
  paddingBottom: '10px',
  paddingLeft: '16px',
  paddingRight: '16px',
  '&:hover': {
    backgroundColor: '$blue500',
  },
});
```

꿰미는 구성 파일에 정의할 수 있는 14개의 토큰 유형을 제공합니다.

### 3. 중단점 구성

중단점 구성을 사용하면 특정 중단점 중에 구성 요소에 스타일을 적용할 수 있습니다. 사용자 자신의 중단점 속성 이름을 자유롭게 정의할 수 있습니다. 예를 들어:

```coffeescript
# using bp1, bp2, bp3 and bp4
export const { styled, css } = createStyled({
  breakpoints: {
    bp1: (rule) => `@media (min-width: 640px) { ${rule} }`,
    bp2: (rule) => `@media (min-width: 768px) { ${rule} }`,
    bp3: (rule) => `@media (min-width: 1024px) { ${rule} }`,
    bp4: (rule) => `@media (min-width: 1280px) { ${rule} }`,
  },
});

#or using sm, md, lg, xl
export const { styled, css } = createStyled({
  breakpoints: {
    sm: (rule) => `@media (min-width: 640px) { ${rule} }`,
    md: (rule) => `@media (min-width: 768px) { ${rule} }`,
    lg: (rule) => `@media (min-width: 1024px) { ${rule} }`,
    xl: (rule) => `@media (min-width: 1280px) { ${rule} }`,
  },
});
```

그런 다음 구성 요소 스타일의 일부로 중단점 속성을 적용할 수 있습니다.

```undefined
const Button = styled('button', {
  height: '35px',
  // apply styles to the `bp1` breakpoint
  bp1: {
    height: '45px'
  }
});
```

또는 스타일 패턴을 재정의하지 않으려면 변형 API를 중단점 속성과 함께 사용할 수 있습니다. 먼저 변형 모델을 스타일에 쓰십시오.

```undefined
const Button = styled('button', {
  height: '35px',
  // variants for height
  variants: {
    height: {
      small: {
        height: '25px'
      },
      normal: {
        height: '45px'
      },
      large: {
        height: '75px'
      },
    }
  }
});
```

그런 다음 각 중단점에 적용할 변형을 정의합니다. 중단점을 적용하기 전에 `initial` 중단점을 사용하여 초기 변형을 선언해야 합니다.

```bash
<Button height={ initial: 'small', bp2: 'normal', bp3: 'large' }>
  Responsive button
</Button>
```

### 4. utils 구성

utils 구성을 사용하면 스타일을 정의할 때 바로 가기의 역할을 하는 사용자 지정 함수를 작성할 수 있습니다. 예를 들어, m`utility 함수를 minage 속성을 쓰는 바로 가기로 쓰도록 하겠습니다.

```undefined
export const { styled, css } = createStyled({
  utils: {
    m: (config) => (value) => ({
      marginTop: value,
      marginBottom: value,
      marginLeft: value,
      marginRight: value,
    }),
  }
});
```

다음으로, 구성 요소 내부의 유틸리티를 사용합니다.

```undefined
const Button = styled('button', {
  height: '35px',
  m: '20px'
});
```

렌더링된 버튼은 모든 면에서 `20px` 여백이 됩니다. 필요한 만큼 사용률 함수를 정의할 수 있습니다.

## 결론

뛰어난 성능과 컴포넌트 아키텍처에 초점을 맞춘 것 외에도, Silves는 마침내 변형을 위한 내장된 퍼스트 클래스 지원을 제공하는 최신 CSS-in-JS이다. 변형 API의 설계를 사용하면 소품을 기준으로 스타일을 재정의하거나 여러 클래스를 작성하여 기존 방식으로 진행하지 않고도 구성 요소의 시각적 프레젠테이션을 변경할 수 있습니다.

또한 이 라이브러리에는 사용자 지정 토큰, 중단점 및 활용 기능을 정의하여 스타일링 구성요소를 재미있고 쉽게 만들 수 있는 강력한 구성 파일이 있습니다. 자세한 내용은 바느질 설명서를 참조하십시오.