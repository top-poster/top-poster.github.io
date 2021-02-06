---
layout: post
title: "next.js를 사용한 증분 정적 재생성"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/nextjs-incremental-static-regeneration.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/nextjs-incremental-static-regeneration.png?fit=730%2C487&ssl=1)

Next.js는 프로덕션 사용 사례에 필요한 모든 기능을 번들로 제공하는 다중 페이지 Retact 프레임워크입니다. React 애플리케이션 구축을 위한 선도적인 플랫폼 중 하나입니다.

최근까지는 두 가지 서빙 전략만 사용할 수 있었습니다. 정적 사이트 생성(SSG) 및 서버측 렌더링(SSR)입니다. 그러나 Next.js v9.5는 둘의 하이브리드 버전인 ISR(Incremental Static Regeneration)이라는 새로운 전략을 도입한다.

이 게시물은 이 새로운 전략의 관련 부분을 다루며 웹 개발을 위한 게임 체인저인 이유를 설명할 것입니다.

## 정적 사이트 생성 대 서버 측 렌더링

SSG와 SSR의 차이점은 페이지의 HTML이 생성되는 경우입니다. SSG를 사용하면 빌드 시간 내에 HTML이 생성됩니다. SSG 프리렌더 기능을 통해 캐슁이 쉽고 빠르게 제공됩니다.

"static"이라는 용어는 HTML이 static이라는 사실에서 유래했다. 그러나 반드시 페이지가 정적임을 의미하지는 않습니다. 클라이언트 측 JavaScript를 사용하여 데이터를 가져오고 동적 상호 작용을 만들 수 있습니다.

Next.js 팀에 따르면 SSG를 진행하는 것이 좋습니다. 적합한 경우 기본 선택이 되어야 합니다. SSG의 일부 사용 사례에는 블로그, 포트폴리오 웹사이트, 전자상거래 애플리케이션 또는 문서 웹사이트가 포함될 수 있습니다.

반면 SSR는 각 요청에 대해 페이지의 HTML을 생성합니다. 매번 애플리케이션을 구축하지 않고도 HTML을 변경할 수 있기 때문에 SSG보다 훨씬 유연합니다. 따라서 지속적으로 업데이트되는 데이터가 있는 경우 SSG는 적합하지 않을 수 있습니다.

당신의 트위터 홈페이지를 정적 페이지로서 서비스한다고 상상해보세요. 상상조차 할 수 없죠, 그렇죠? 이러한 유연성은 성능에 상당한 영향을 미치고 캐슁이 훨씬 더 어려워집니다. SSR는 SSG가 적합하지 않을 때마다 사용할 수 있는 방법입니다.

next.js를 사용하면 두 모드를 쉽게 전환할 수 있습니다. 모든 페이지는 페이지를 렌더링하는 데 필요한 속성을 가져오는 기능을 내보내야 합니다.

페이지의 반응 구성 요소는 속성이 어디서 왔는지도 알 수 없습니다. Next.js 설명서에서 가져온 SSG 및 SSR의 예를 살펴보겠습니다.

다음은 SSG의 예입니다.

```js
export async function getStaticProps() {
  const res = await fetch('https://...');
  const data = await res.json();

  return { props: { data } };
}
```

모든 정적 페이지는 "getStaticProps" 함수를 내보내야 합니다. 함수는 빌드 시 호출되며 반환된 `prop`는 react 구성 요소로 `prop`로 전달됩니다. 이 예에서는 원격 API 끝점을 호출하고 응답을 반환합니다.

다음은 SSR에 해당하는 예입니다.

```js
export async function getServerSideProps() {
  const res = await fetch('https://...');
  const data = await res.json();

  return { props: { data } };
}
```

유일한 차이점은 함수 이름입니다. 또 다른 근본적인 차이점이 있습니다. 이번에는 빌드 시간이 아니라 모든 요청에 따라 기능이 호출됩니다.

## Next.js의 증분 정적 재생성

ISR(Incremental Static Regeneration)은 런타임 동안 정적 페이지를 재생성할 수 있는 새로 릴리스된 기능입니다. SSG와 SSR의 하이브리드 솔루션입니다.

첫 번째 요청에 따라 페이지가 생성됩니다. 방문자가 데이터 가져오기를 기다려야 하는 SSR와 달리 예비 페이지가 즉시 제공됩니다.

폴백 스테이지에서는 모든 것이 해결될 때까지 자리 표시자와 스켈레톤 페이지를 제시할 수 있습니다. 스켈레톤 페이지는 거의 모든 곳에서 볼 수 있는 일반적인 패턴입니다.

데이터가 확인되면 최종 페이지가 캐시되고 SSG와 마찬가지로 방문자도 캐시된 버전을 즉시 받게 됩니다. 또한 Next.js가 페이지를 다시 검증하고 업데이트해야 하는 시기를 설정할 수 있습니다.

재검증 시에도 방문자는 먼저 캐시된 버전을 받은 후 업데이트된 버전만 받습니다. 이 캐싱 전략은 일반적으로 "확정 기간 동안 유효화"라고 알려져 있습니다. 물론 이전에도 가능했지만 ISR은 이러한 능력을 민주화하고 접근성을 높였습니다.

SSR 페이지를 ISR로 전환하면 애플리케이션 성능이 크게 향상되고 Lighthouse 점수가 수십 포인트 향상될 뿐만 아니라 빠른 속도로 방문자를 즐겁게 할 수 있습니다.

ISR을 사용하는 것은 추가적인 검증 속성을 가진 SSG와 매우 유사하다. 이 새 속성은 페이지를 다시 확인하는 빈도(초)를 나타냅니다.

```js
export async function getStaticProps() {
  const res = await fetch('https://...');
  const data = await res.json();

  return { props: { data }, revalidate: 60 };
}
```

이렇게 간단합니다. 멋지지 않아요.

React Hooks를 사용하는 것만큼 쉽게 폴백 모드를 감지할 수 있습니다. Next.js 라우터는 우리를 위해 이 정보를 보관한다. 우리가 해야 할 일은 라우터를 가져와서 다음과 같이 이 정보를 추출하는 것이다:

```undefined
const { isFallback } = useRouter();
```

isFallback은 폴백 모드가 변경되면 자동으로 업데이트되는 부울입니다. 이 조각을 가지고 원하는 모든 구성 요소에 넣을 수 있습니다.

## ISR, SSG 및 SSR의 성능 측면

SSG를 다른 것보다 추천하는 주된 이유는 첫 번째 페인트에 대한 시간과 차단 시간을 단축하여 방문자에게 더 나은 사용자 환경을 제공하기 위함입니다.

SSR 경로를 따라 내려가면 차단 시간이 길어지고 결과적으로 첫 번째 페인트가 만들어집니다. 이는 서버가 HTML을 클라이언트에 다시 보내기 전에 일부 데이터를 가져온 다음 HTML을 즉시 생성해야 하기 때문입니다.

최상의 환경을 제공하기 위해 서버 측에서 가져올 데이터를 신중하게 선택해야 합니다.

내 규칙은 메타 태그에만 필요한 데이터를 가져오는 것입니다. 예를 들어, 올바른 메타 태그는 다른 사람이 소셜 미디어에서 메타 태그를 공유할 때 링크의 모양에 영향을 줄 수 있습니다. 또한 SEO도 개선할 수 있습니다. 검색 봇이 사용자를 올바르게 인덱싱하기 위해 JavaScript를 사용하지 않도록 합니다. 그들이 당신의 웹사이트를 쉽게 긁어모으고 이해하도록 하세요.

ISR은 조금 다르지만, 제 일반적인 규칙은 여전히 유효합니다. ISR을 사용할 때 첫 번째 페인트와 차단 시간은 폴백 단계로 인해 영향을 받지 않을 수 있습니다. next.js는 서버측 데이터를 가져오기 전에 HTML을 즉시 다시 전송합니다. 페이지 설계에 따라 서버측 데이터 가져오기가 첫 번째 의미 있는 페인트를 지연시킬 수 있습니다.

## ISR, SSG 및 SSR의 작동 방식에 대한 라이브 데모

마무리하기 전에, 우리가 배운 모든 것을 실천에 옮기는 작업 사례가 있습니다. 데모에서는 ISR, SSG 및 SSR의 세 가지 방법으로 이 멋진 API를 사용하여 공용 API를 임의로 선택합니다.

코드를 직접 사용해 보고 싶다면 GitHub에서 사용할 수 있습니다.

먼저 페이지 구성요소를 작성하는 것으로 시작하겠습니다. Next.js 덕분에 동일한 소품만 제공하면 한 번 쓰고 모든 방법에 사용할 수 있습니다.

```js
import { useRouter } from 'next/router';

export default function Page(props) {
    const { isFallback } = useRouter();

    if (isFallback) {
        return <></>;
    }

    return <div>
        <h1>{props.name}</h1>
        <p>{props.description}</p>
    </div>
}
```

우리의 소품 객체에는 공용 API의 이름과 그 설명이 포함되어 있다. 우리는 헤더와 그에 따른 단락 요소를 사용하여 그것들을 표시한다. 한 가지 주의할 점은 위에서 언급한 바와 같이 ISR 폴백 모드를 확인해야 한다는 것입니다.

이 모드에서는 소품이 정의되지 않을 수 있으므로 우리가 처리해야 합니다. 단순성을 위해 빈 요소를 반환하지만, 더 나은 방법은 로드러나 스켈레톤 페이지를 보여주는 것입니다.

페이지가 준비되었고, 이제 API에서 데이터를 가져올 수 있습니다. 모든 호출이 브라우저 외부에서 이루어지기 때문에 브라우저의 기본 제공 가져오기 기능을 사용할 수 없기 때문에 노드 페치를 이러한 목적으로 사용할 것이다.

```js
import fetch from 'node-fetch';

export async function getRandomAPI() {
    const res = await fetch('https://api.publicapis.org/random');
    const json = await res.json();
    return {
        name: json.entries[0].API,
        description: json.entries[0].Description,
    };
}
```

우리는 임의 공개 API를 반환하는 API 끝점을 호출하고 소품 개체와 마찬가지로 이름과 설명을 포함하는 더 나은 구조로 구성한다.

기본 요소가 준비되었으며 이제 모든 렌더링 방법에 대해 하나씩 실제 Next.js 페이지를 만드는 일만 남았습니다.

SSR:

```js
import Page from '../Page';
import { getRandomAPI } from '../publicApis';

export default Page;

export async function getServerSideProps() {
    const props = await getRandomAPI();
    return { props };
}
```

SSG:

```js
import Page from '../Page';
import { getRandomAPI } from '../publicApis';

export default Page;

export async function getStaticProps() {
    const props = await getRandomAPI();
    return { props };
}
```

ISR:

```js
import Page from '../Page';
import { getRandomAPI } from '../publicApis';

export default Page;

export async function getStaticProps() {
    const props = await getRandomAPI();
    return { props, revalidate: 30 };
}
```

이러한 구현이 모두 비슷해 보입니다. SSR 버전은 getStaticProps 대신 getServerSideProps를 사용합니다. 그리고 ISR 버전은 재평가 기간을 30초로 설정한다. 꽤 쉽죠?

여기서 실제로 볼 수 있습니다. SSR 버전은 모든 요청에 따라 임의로 API를 선택합니다. SSG 버전은 빌드 시 임의로 하나를 선택합니다. 마지막으로 ISR 버전은 30초마다 선택한 API를 새로 고칩니다.

## 결론

이제 SSG, SSR 및 ISR의 뉘앙스를 이해해야 합니다. 성능을 최적화하는 경우 먼저 SSG를 사용하고 그 다음으로는 ISR을 사용하고 마지막 수단으로만 SSR 사용을 고려해야 합니다.

SSG와 ISR 모두 몇 가지 제한이 있기 때문에 SSR와 같은 모든 사용 사례에 적합하지 않을 수 있습니다. 또한 이 세 가지 중 몇 가지 성능 측면도 다루었습니다. 내 규칙은 페이지의 메타데이터 태그에 필요한 데이터만 서버측으로 가져오는 것입니다. 해피 코딩!