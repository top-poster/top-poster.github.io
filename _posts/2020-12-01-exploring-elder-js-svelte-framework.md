---
layout: post
title: "SEO 중심의 Svelte 프레임워크인 Elder.js 탐색"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/12/exploring-elder-js-svelte-framework.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/exploring-elder-js-svelte-framework.png?fit=730%2C487&ssl=1)

## 배경

Jamstack과 함께 빌딩의 세계에 오신 것을 환영합니다. 원래 JavaScript, API 및 마크업을 나타내는 Jamstack 아키텍처는 보다 빠르고 안전하며 확장 가능한 웹을 지원합니다. Jamstack 애플리케이션은 사전 렌더링되기 때문에 매우 빠릅니다. 즉, 모든 프런트 엔드 구성 요소와 자산이 고도로 최적화된 정적 페이지에 사전 구축됩니다.

이 설정으로 이제 글로벌 CDN을 통해 사이트를 서비스할 수 있습니다. 또한 Jamstack 앱은 깔끔하고 최소화된 아키텍처를 지원하므로 전체 프런트 엔드가 빌드 프로세스에서 분리되므로 불필요한 리소스 할당이 필요하지 않습니다. 물론 인프라 및 유지보수에 대한 비용 절감은 물론 많은 이점이 있습니다.

Jamstack 사이트를 위한 많은 정적 사이트 생성기 중 하나인 Elder.js는 SEO가 특히 중요한 복잡하고 데이터 집약적인 웹 사이트에 더 나은 솔루션을 제공한다. Elder.js는 자사 웹 사이트에서 10~100K 페이지에 이르는 매우 복잡한 SEO 지향 사이트를 구축하는 문제를 해결합니다.

## Elder.js 소개

Jamstack 기술을 기반으로 구축된 Elder.js는 복잡하고 데이터 지향적이며 검색에 최적화된 애플리케이션을 구축하기 위한 보다 실행 가능한 솔루션의 필요성을 안고 태어났다. 프레임워크 작성자는 관련된 페이지 수나 데이터 양에 신경 쓸 필요 없이 모든 크기의 정적 사이트를 구축하는 표준 방법이 필요하다고 느꼈다.

Elder.js는 Svelte 템플릿을 기반으로 합니다. 스벨트는 완전히 새로운 접근 방식의 UI를 구축하는 자바스크립트 웹 프레임워크이다. 템플릿은 HTML 위에 데이터 계층과 함께 구축된다. 차례로, Svelte는 빌드/컴파일 시 응용 프로그램 코드를 JavaScript로 변환한다.

저자에 따르면, Elder.js의 범위는 SEO를 주요 우선순위로 두고 Svelte를 위한 플러그형 정적 사이트 생성기/서버측 렌더링 프레임워크를 구축하는 것으로 제한된다. 연장자는 이 프레임워크의 다양한 이점을 누리며 다음과 같은 몇 가지 인상적인 기능을 제공합니다.

- 최적화되고 사용자 정의가 가능한 빌드 프로세스로, 여러 CPU 코어를 포괄할 수 있습니다.
- 혼합물에 부분 수분을 함유한 슬리브 템플릿. 수화 기능을 사용하면 대화식이어야 하는 클라이언트의 일부에 수분을 공급하여 구성 요소의 느린 로딩, 작은 번들 등의 이점을 통해 페이로드를 줄일 수 있습니다.
- 여러 데이터 소스를 사용할 수 있는 직관적인 데이터 계층
- SSR 및 SSG에 대한 우선 순위 지원
- 전체 페이지 생성 프로세스에 사전 빌드된 후크가 있어 개발 프로세스의 모든 단계에서 페이지를 쉽게 사용자 정의할 수 있습니다.

### 전제조건

우리가 Elder.js에 대한 탐구를 시작하기 전에, 독자들은 Svelte와 이 웹 프레임워크로 최소한의 애플리케이션을 구축하는 방법에 대해 잘 알고 있어야 한다. 또한 정적 사이트를 구축하거나 일반적으로 Jamstack과 함께 작업하는 것을 조금 더 알고 있다면 큰 플러스가 되겠지만, 이 제목은 이러한 개념과 아이디어를 포괄하므로 독자들이 반드시 알아야 할 필요는 없습니다.

## Elder.js를 시작하는 중

Elder.js를 시작하기 위해, 우리는 Git 저장소의 복사본을 만들어 Git 기반 프로젝트를 빠르게 비계할 수 있는 degit을 사용할 수 있다. 기본 `깃 클론` 기능을 사용하지 않는 이유가 궁금할 수 있습니다. 음, Degit은 전체 Git 기록 대신 최신 커밋만 다운로드함으로써 Gitrepo에서 관련 부분만 가져옵니다.

### 설치 절차

Elder.js를 시작하는 가장 쉬운 방법은 위에서 설명한 것처럼 degit을 사용하여 GitHubrepo(Elder.js가 특정 파일 구조를 예상하는 것처럼)에서 템플릿을 복제하는 것이다. 실행으로 다음을 수행할 수 있습니다.

```cpp
npx degit Elderjs/template elderjs-app
```

위 명령의 실행 결과는 다음과 같습니다.

```shell
npx: installed 1 in 9.946s
> cloned Elderjs/template#master to elderjs-app
```

위의 출력에서 알 수 있듯이 명령은 먼저 npx를 설치한 다음 GitHub에서 Elder.js 템플릿 마스터 분기를 복제합니다. 이제 단순히 elderjs-app 디렉토리로 이동하여 npm이나 yarn을 사용하여 종속성을 설치한 다음 앱을 시작할 수 있습니다. 폴더 구조는 다음과 같아야 합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/elder-js-folder-structure.png?resize=203%2C577&ssl=1)

dev 서버를 시작하기 위해 npm start를 실행하면 npm run build:rollup이 실행됩니다.

> 참고: 'npm run build'를 실행할 수 있습니다. 이 명령은 'node./src/cleanPublic'입니다.js

### Elder.js 템플릿 탐색 중

이제 Elder.js 템플릿을 살펴보겠습니다. 실행 중인 템플릿을 보려면 npm start 명령을 실행한 후 브라우저에서 localhost:3000으로 이동할 수 있습니다. 샘플 블로그 사이트를 보려면 `http://localhost:3000/how-to-build-a-blog with-lderjs/`로 이동하십시오.

이제 템플릿의 블로그 섹션을 살펴보고 어떻게 작동하는지 알아보겠습니다. 먼저 `routs/blog` 폴더로 이동합니다. 여기서 다음과 같은 내용의 `routes.js` 파일을 볼 수 있습니다.

```coffeescript
module.exports = {
  data: {},
  all: () => [],
  permalink: ({ request }) => `/${request.slug}/`,
};
```

위와 같이 데이터, 올, 퍼멀링크 기능을 수출하고 있습니다. 데이터 함수는 데이터를 Svelte 템플릿에 소프로 전달합니다. `all` 함수는 이 특정 경로에 대해 가능한 모든 `request` 객체의 배열을 반환합니다.

마지막으로 permalink 함수는 `all` 함수에서 반환된 `request` 객체를 각각의 상대 URL로 변환한다. 여기가 바로 마법이 일어나는 곳입니다. 자세한 내용은 Elder.js 라우팅 섹션에서 나중에 설명합니다.

블로그 경로에 대한 `data`와 `all` 함수는 템플릿의 GitHub 저장소에 있는 `/plugins/packages/markdown/index.js` 파일에 정의된 Markdown 플러그인으로 채워진다. 그래서 우리 코드에서는 `npm install --save @elderjs/plugin-markdown`을 실행하여 플러그인을 사용할 수 있다. 그러나 이 경우에는 플러그인이 템플릿과 함께 번들로 제공되므로 그럴 필요가 없습니다.

플러그인이 어떻게 구성되어 있는지 이해하기 위해, `root dir`의 `lder.config.js` 파일을 살펴봅시다. 다음은 저희가 관심 있는 부분입니다.

```bash
plugins: {
    '@elderjs/plugin-markdown': {
    routes: ['blog'],
},
```

위의 구성을 통해 마크다운 플러그인을 사용하여 어떤 경로를 제어할지 결정할 수 있습니다. 이 경우 블로그 경로를 활용하고자 한다. 이 플러그인은 지정된 `route/blog` 폴더에서 Markdown 컨텐츠/파일을 읽고 수집하며, 발견된 및 구문 분석된 Markdown 파일을 자동으로 `all Requests` 후크에 요청으로 추가합니다.

플러그인은 `allRequests` 후크를 사용하여 처리된 Markdown 파일과 해당 슬러그를 Elder.js에 등록합니다. 여기서 우리가 의미하는 것은 `all Requests` 후크는 수집된 파일을 전면 물질이나 파일 이름을 슬러그로 사용하여 `all Requests` 배열에 추가한다는 것이다.

그러나 그 전에 `bootstrap` 후크는 구문 분석된 Markdown content/data를 `data` 함수에 추가한다. 그러면 `Blog.svelte` 템플릿 파일에서 `data` prop로 사용할 수 있습니다. 아래 내용을 살펴보겠습니다.

```xml
<script>
  export let data;
// destructure data prop
  const { html, frontmatter } = data;
</script>

<style>
</style>

<svelte:head>
  <title>{frontmatter.title}</title>
</svelte:head>

<a href="/">&LeftArrow; Home</a>
<div class="title">
  <h1>{frontmatter.title}</h1>
  {#if frontmatter.author}<small>By {frontmatter.author}</small>{/if}
</div>

{#if html}
  {@html html}
{:else}
  <h1>Oops!! Markdown not found!</h1>
{/if}
```

위의 Blog.svelte 파일에서는 앞에서 논의한 바와 같이 olderjs-plugin-markdown 파일에서 data를 채우고 있다. 다음 줄에서는 데이터 개체를 UI를 채우는 데 사용하는 html 및 frontmatter 개체로 파괴한다. if 문은 `data` 소품에서 html 개체가 있는지 확인하고 렌더링하며, 없으면 오류가 표시됩니다.

또한, 경로/홈 폴더에 있는 홈 페이지인 경로 디르(route dir)를 방문할 수 있습니다. 그 안에서 우리는 `route.js`와 `Home.svlte`라는 두 개의 파일을 찾을 수 있을 것이다. 여느 때처럼 route.js 파일에는 우리가 이미 다루었던 all, permalink, data 기능이 포함되어 있다.

> 참고: 'all()' 쿼리는 특정 경로(이 경우 'home' 경로)에서 특정 페이지를 생성하는 데 필요한 매개 변수를 나타내는 개체를 반환합니다.

홈스벨트 파일의 스크립트 섹션을 보면 홈페이지의 모든 구성품을 어디서 가져오는지 알 수 있다.

```xml
<script>
  import BlogTeaser from '../../components/BlogTeaser.svelte';
  export let data, helpers;
  // add permalinks to the hook list so we can link to the posts.
  const hooks = data.hookInterface.map((hook) => ({ ...hook, link: helpers.permalinks.hooks({ slug: hook.hook }) }));
</script>
```

구체적으로 2번 라인에서는 `...///구성 요소/BlogTeaser.svelte 경로에서 `BlogTeaser` 구성 요소를 가져올 수 있습니다. 평소처럼 데이터 소품, 헬퍼 소품(데이터 기능과 모든 후크에서 사용 가능한 도우미의 개체)을 경로 파일에서 다음 줄 홈페이지 파일로 전달하고 있다.

또한 홈.svelte 구성 요소에서는 이미 데이터 소품에서 사용 가능한 구문 분석된 Markdown 파일의 블로그 게시물마다 루프를 수행하고 있습니다. 우리는 아래와 같이 블로그와 도우미 데이터를 수입된 블로그 티저 구성요소에 소품으로 전달한다.

```js
<div class="blog">
  <div class="entries">
    {#each data.markdown.blog as blog}
      <BlogTeaser {blog} {helpers} />
    {/each}
  </div>
</div>
```

마지막으로 템플릿의 src/구성 요소 경로에는 위의 홈.svelte 구성 요소에서 소품으로 전달된 블로그와 도움말에 액세스할 수 있는 BlogTeaser.svelte 구성 요소가 있습니다. 여기 있습니다:

```xml
<script>
  export let blog;
  export let helpers;
</script>

<div class="entry">
  <a href={helpers.permalinks.blog({ slug: blog.slug })}>{blog.frontmatter.title}</a>
  <p>{blog.frontmatter.excerpt}</p>
</div>
```

위의 스크립트 태그를 보면 알 수 있듯이, 우리는 앞서 홈페이지에서 소품으로 전달된 블로그와 도우미 오브젝트에 접근할 수 있다. 6번 온라인에서는 helpers.permalinks.blog.slug}}`은(는) Permalink 해결사인 Elder.js 기본 도우미 중 하나입니다. 이렇게 하면 단순히 요청 객체를 전달하여 영구 링크를 확인할 수 있습니다. 일반적인 구조는 다음과 같습니다.

```undefined
helpers.permalink\[routeName\]({requestObject})
```

## 다른 SSG에 비해 Elder.js의 이점

- 대부분의 SSG는 단순한 사이트/블로그 또는 전체 규모의 애플리케이션을 위해 구축되었으며 SEO 속성이 매우 인상적인 대규모 데이터 집약적인 사이트를 위해 구축되지 않았습니다.
- 또한 다른 SSG의 경우 다른 여러 소스에서 데이터를 가져와 유지 관리하기 어려운 코드베이스를 만들 수 있습니다.
- 큰 번들 크기를 초래할 수 있는 클라이언트 측 라우팅과 관련된 복잡성은 대부분의 다른 SSG에서 거의 피할 수 없다.

### Elder.js 라우팅

Elder.js의 라우팅에 대한 접근 방식은 다른 인기 있는 프레임워크에서 발견되는 매개 변수 기반 라우팅과는 다르지만 몇 가지 장점을 제공한다. 우리가 원하는 URL 구조를 완벽하게 제어할 수 있기 때문입니다. 또한 어떤 페이지를 생성해야 하는지 알기 위해 사이트의 모든 링크를 탐색할 필요가 없습니다.

Elder.js의 경로는 `src/routes` 디렉토리에 있는 두 개의 파일로 구성됩니다. 예를 들어 템플릿의 `후크` 폴더로 이동하면 `후크(Hooks.svelte)` (템플릿으로 사용되는 Svelt 구성 요소)와 `route.js` 파일 (예: `퍼멀링크`, `all`, `data` 함수 등의 경로 세부 정보를 정의하기 위한)을 찾을 수 있다.

> 참고: 슬리브 템플릿은 각 경로에 대해 정의되며 서버에서만 렌더링됩니다. HTML에 원하지 않는 데이터가 포함될 수 있는 데이터베이스 자격 증명, env 변수 등이 포함된 중요한 소품을 받기 때문입니다.

permalink, all, data 기능은 일반적인 route.js 파일과 같이 이전에 논의된 바 있다. 이러한 함수에 대한 규격에 대한 링크는 Elder.js 설명서에서 사용할 수 있다.

성능상의 이유로, 우리는 `요청` 객체에 있는 데이터베이스, API 또는 기타 데이터 소스를 쿼리하는 데 필요한 모든 세부 정보를 포함하고 대용량 데이터를 저장하지 않아야 한다. 또한 데이터를 가져오거나 준비 또는 처리하려는 경우에는 페이지에 필요한 모든 데이터를 반환하므로 `데이터` 함수에서 처리해야 합니다.

또한 일부 데이터를 여러 경로로 사용하려는 경우 부팅 스트랩 후크에 데이터 개체를 채워 해당 데이터를 공유할 수 있다. 페이지 생성 프로세스의 이 단계에서 정의된 데이터를 모든 경로에서 사용할 수 있기 때문입니다.

### Elder.js 훅

Elder.js 앱의 핵심 페이지 생성 프로세스를 사용자 정의하시겠습니까? 흠, 승리를 위한 갈고리! 후크에 대한 흥미로운 점은 대부분의 사용 사례에 대해 플러그인으로 번들링하고 공유할 수 있다는 것입니다.

저자에 따르면 "Elder.js의 후크 구현의 목표는 `route.js` 파일에 들어갈 수 없는 모든 변경 사항을 모든 사람이 숨겨진 세부 정보를 찾을 수 있는 단일 `hooks.js` 파일에 넣을 수 있어 사용자가 Elder.js 페이지 생성 프로세스를 완벽하게 제어할 수 있도록 하는 것이다."라고 말했다.

Elder.js 후크 시스템은 `후크`를 기반으로 합니다."인터페이스"입니다. 이 인터페이스는 각 후크가 수행할 수 있는 작업과 후크가 가진 속성을 정의합니다.

각 후크는 어떤 `prop`가 후크에 등록된 함수에 `변형`되거나 해당 함수에 의해 변경될 수 있는 `prop`와 함께 사용 가능한 `prop`를 정의한다. 이것은 본질적으로 후크 인터페이스가 구현하는 `계약`을 정의한다.

> 참고: 시스템 부트스트랩(즉, '부트스트랩' 후크)에서 '공용' 폴더에 HTML을 쓰는 것(즉, '요청 완료' 후크)에 이르기까지 페이지 생성 프로세스의 모든 주요 단계에 후크가 있습니다.

주목할 만한 후크는 bootstrap (Elder.js가 스스로 부팅이 지연되고 사용자가 임의 기능을 실행할 수 있게 된 후에 실행됨), requestComplete, data 등이 있다. 사용 가능한 후크에 대한 전체 개요는 `후크`를 참조하십시오.Interface.ts의 파일입니다. 후크 구성 위치 및 방법, 사용 가능한 후크의 전체 목록과 같은 자세한 내용은 설명서를 참조하십시오.

### Elder.js 플러그인

공식 가이드에 따르면, 플러그인은 Elder.js 사이트에 추가 기능(예: 파일을 s3 버킷에 업로드하는 플러그인)을 추가하는 데 사용할 수 있는 후크 및/또는 경로의 그룹이다. 후크 호출 사이에, 플러그인은 모든 후크 또는 `init` 함수에 추가된 데이터가 전체 라이프사이클 동안 지속되므로 데이터를 저장할 수 있다.

> 참고: 플러그인을 사용하려면 'lder.config.js' 파일에 등록되어 있어야 하며 npm 패키지의 진입점에서 로드할 수 있습니다./.node_modules/${pluginName}/.

공식 플러그인 목록은 설명서의 이 섹션을 참조하십시오. 또한 자체 플러그인을 작성하기 위해 Elder.js 플러그인 템플릿을 복제할 수 있습니다. 다음 작업을 실행하여 디지트를 사용하여 로컬로 복제할 수 있습니다.

```cpp
npx degit Elderjs/plugin-template elderjs-plugin
```

샘플 플러그인의 구조를 보려면 여기를 확인하십시오.

> 참고: 'init()' 함수는 플러그인 초기화를 처리하는 동기화 함수입니다. Elder.js 및 플러그인 구성을 포함한 플러그인 정의를 수신합니다.

## 결론

Jamstack은 웹 애플리케이션을 개발, 배포 및 구동하는 새로운 방법을 제공합니다. 기본적으로 이러한 사이트는 미리 구축되어 있으며 서비스되기 전에 최적화되어 있습니다. 기존의 프런트 엔드 인프라가 필요 없는 새로운 접근 방식을 제공합니다. 대신 HTML은 정적 파일로 미리 렌더링됩니다.

10K페이지에서 100K페이지에 이르는 SEO에 특별히 초점을 맞춰 우아하고 데이터 집약적인 정적 사이트를 신속하게 구축하려면 Elder.js를 가장 먼저 염두에 두어야 합니다. 이 웹 프레임워크에 대해 자세히 알아보려면 Github의 소스 코드를 살펴보십시오.

Elder.js 응용 프로그램의 전체 데이터 흐름을 제대로 파악하려면 설명서의 이 섹션을 확인하십시오. 또한 이 문서는 공통 사양 및 구성 요구 사항, 부분 수화, 단축 코드 등을 포함하여 여기서 다루지 않는 다른 주제도 학습하는 데 도움이 될 것입니다.