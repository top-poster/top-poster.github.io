---
layout: post
title: "Next.js를 사용한 프리페치 및 동적 라우팅"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/prefetching-dynamic-routing-next-js.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/prefetching-dynamic-routing-next-js.png?fit=730%2C487&ssl=1)

SSR(Server-side rendering)은 웹 애플리케이션의 성능과 SEO를 향상시키기 위해 널리 채택된 기법이 되었다. 또한 정적 사이트 생성(SSG)이 더 간단하고 빠른 것으로 간주되지만 서버 측 렌더링이 유일한 옵션인 경우도 있습니다.

그러나 특정 페이지에 서버 측 렌더링을 구현하는 것은 어려운 작업이 될 수 있습니다. next.js는 응용 프로그램의 각 페이지에 대해 SSG와 SSR 중 하나를 선택할 수 있도록 하여 이 문제를 해결하려고 시도합니다.

이 게시물은 속도를 위해 구축된 CMS인 Agility CMS로 블로그를 구축하여 Next.js를 강력한 React 프레임워크로 만드는 이러한 개념과 기타 개념을 탐색합니다.

### 결과

이 게시물의 목표는 다음과 같은 사항을 확인하는 것입니다.

- Next.js가 제공하는 두 가지 유형의 프리로드 기법 이해
- Next.js와 함께 제공되는 여러 내장 기능을 효과적으로 활용할 수 있습니다.
- Agency CMS 인스턴스를 생성하고 설정할 수 있습니다.

## Next.js: SSR가 내장된 대응 프레임워크

Next.js는 많은 개발자들이 웹 애플리케이션을 구축할 때 직면하는 일반적인 문제를 해결하는 리액트 프레임워크이다. 웹 팩과 같은 번들을 사용하여 모든 코드를 번들링하고 최소화하거나 페이지 성능을 향상시키기 위해 코드 분할과 같은 최적화를 구현하는 경우 Next.js에 필요한 모든 도구가 있습니다.

TypeScript 사용자인 경우 시작하기 위해 `tsconfig.json` 파일만 생성하면 됩니다! 개발 환경은 Next.js와 같이 애플리케이션을 빌드할 대상을 선택할 수 있기 때문에 매우 좋습니다. 예를 들어, Next.js를 사용하면 CSS-in-JS 라이브러리를 사용할 수 있지만 Sass 및 CSS 모듈도 함께 제공됩니다.

그러나 Next.js가 진정으로 훌륭한 것은 기본적으로 페이지를 미리 렌더링한다는 사실입니다. 즉, 모든 페이지는 기본적으로 Next.js의 하이브리드 아키텍처를 구성하는 두 가지 렌더링 옵션 중 하나인 정적 사이트 생성을 사용합니다.

그러나 빌드 시 표시가 생성되므로 SSG가 항상 이상적인 옵션은 아닙니다. 즉, 페이지에 외부 소스에서 가져온 콘텐츠가 자주 변경되면 페이지를 다시 빌드할 때까지 변경 내용이 페이지에 반영되지 않습니다. SSR가 바로 여기에 해당합니다!

next.js를 사용하면 동적 콘텐츠를 가져온 다음 모든 요청에 따라 적절한 마크업을 생성할 수 있습니다. next.js는 페이지 유형 구성 요소인 `getServerSideProps() 또는 `getStaticProps()에서 두 개의 비동기 함수 중 하나를 선언하는 옵션을 제공하여 이 작업을 수행합니다.

## 민첩성 CMS 설정

Agency CMS는 대부분의 클라우드 아키텍처와 동일한 수준의 확장성을 제공하는 속도를 위해 구축된 컨텐츠 관리 시스템입니다. 대부분의 인기 있는 React 프레임워크(Next.js 포함)에 대한 시작점이 있기 때문에 설정하기가 매우 쉽다. 계정을 만드는 것으로 시작할 수 있습니다. 작업을 마치면 몇 가지 프로젝트 설정을 해야 합니다.

프로젝트를 만들 때쯤이면 CMS를 사용하여 컨텐츠를 바로 사용할 수 있습니다. 대시보드 및 페이지 섹션을 탐색하여 원하는 대로 내용을 편집할 수 있지만, 실제로 주의해야 할 사항은 다음과 같습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/api-keys-button-agility-cms.png?resize=730%2C295&ssl=1)

API Keys 버튼을 클릭하면 모달(모달)이 팝업됩니다. 여기서 Show API Key(s)를 클릭합니다. 나중에 사용할 수 있도록 이 키를 저장하십시오. 이 키가 필요할 것입니다!

## 개발 환경 설정

시작하려면 Next.js 응용 프로그램을 함께 사용해야 합니다. React 애플리케이션을 생성한 적이 있는 경우 create-react-app을 알고 있어야 합니다. Next.js에는 Create-next-app이라는 고유한 아날로그가 있습니다. 터미널을 통해 다음 명령을 실행합니다.

```undefined
npx create-next-app my-blog
```

모든 종속성을 설치했으면 `cd my-blog`를 실행하여 `my-blog` 디렉토리로 변경합니다.

이 게시물의 주요 주제를 고수하기 위해, 우리는 Material-UI라는 구성 요소 프레임워크를 사용할 것입니다. 그러나 자신만의 스타일을 쓰기를 원하는 경우 Next의 내장 CSS Module 및 Sass 지원을 사용할 수 있습니다. 또한 API에서 콘텐츠를 가져오려면 Agency CMS SDK를 설치해야 합니다. 다음 명령을 실행하여 SDK 및 Material-UI를 설치합니다.

```coffeescript
npm install @material-ui/core @agility/content-fetch
```

작업이 완료되면 프로젝트의 루트에 `.env.local` 파일을 생성하고 다음 환경 변수를 추가하면 Agency CMS API에 연결하는 데 필요한 자격 증명이 노출됩니다.

```undefined
AGILITY_CMS_GUID=xxx
AGILITY_CMS_API_FETCH_KEY=xxx
```

API Keys 버튼을 클릭하면 Getting Started(시작하기 시작)에서 찾을 수 있습니다. 그것으로, 우리는 모두 코드를 쓸 준비가 되었습니다!

## 첫 페이지 작성

첫 번째 Next.js 페이지를 생성하기 전에 Next.js에는 파일 시스템 기반 라우터가 있으므로 `pages`라는 디렉토리 아래에 있는 모든 파일이 애플리케이션의 경로가 된다는 점을 이해해야 합니다.

그렇다면, 우리는 모든 경로를 만들어야 합니다. 페이지 디렉토리 아래에 `index.js` 파일이 있습니다. `index.js` 파일에는 `/sp`에서 서비스될 페이지 구성 요소 또는 애플리케이션의 기본/메인 경로가 포함됩니다.

또한 모든 블로그 페이지를 서비스할 수 있는 경로가 필요합니다. 이제 `blog`라는 디렉터리와 `[...]라는 파일을 생성해 보겠습니다.slug.js` 우리는 다음 주제에서 이 개념으로 돌아갈 것입니다. 프로젝트 구조는 다음과 같아야 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/next-js-agility-cms-project-structure.png?resize=398%2C408&ssl=1)

다음으로 Agency CMS 클라이언트를 초기화해야 합니다. 재사용을 촉진하기 위해 프로젝트 루트인 lib의 새 디렉터리 아래에 agility.js라는 별도의 파일을 만들 것입니다.

```js
// lib/agility.js
import agility from "@agility/content-fetch";

const api = agility.getApi({
  guid: process.env.AGILITY_CMS_GUID,
  apiKey: process.env.AGILITY_CMS_API_FETCH_KEY
});

export default api;
```

Agency CMS 프로젝트의 자격 증명을 `.env.local` 파일에 입력했는지 확인합니다. 이제 애플리케이션의 `홈/인덱스` 페이지로 이동하여 `getServerSideProps()`를 사용하여 Agency CMS API를 호출하여 홈페이지의 컨텐츠를 받아 보겠습니다.

```js
// pages/index.js
import api from "../lib/agility";

/* 
* IndexPage content collapsed for readability
*/

export async function getServerSideProps() {
  const page = await api.getPage({
    pageID: 2,
    languageCode: "en-us"
  });

  return {
    props: {
      meta: { title: page.title, desc: page.seo.metaDescription },
      data: page.zones.MainContentZone.reduce((acc, curr) => {
        acc[curr.module] = curr.item;
        return acc;
      }, {})
    }
  };
}
```

getPage()는 Agility CMS SDK에서 제공하는 비동기식 방법으로, `page()`를 제공하면 전체 페이지를 한 번에 가져올 수 있습니다.ID. 컨텐츠가 여러 개의 로케일로 되어 있는 경우, 올바른 "언어 코드"도 함께 전달해야 합니다.

페이지 값ID의 속성은 특정 페이지 설정에서 찾을 수 있습니다. 예를 들어 홈이라는 이름의 페이지인 경우 Agility CMS 대시보드의 페이지 -> 홈 -> 설정으로 이동합니다.

`getPage()에서 응답을 받은 후에는 구성 요소가 편리하게 데이터를 사용할 수 있도록 데이터를 변환할 수 있습니다. 예를 들어 코드 블록에서 데이터 속성은 항상 `MainContentZone` 어레이의 모든 값을 포함합니다.

MainContentZone은 모듈이라고 불리는 객체의 배열이다. 이러한 모듈에는 이름이 있으며, 각 인덱스(즉, 배열의 위치)를 사용하지 않고 이러한 모듈 이름을 통해 컨텐츠에 액세스하는 것이 편리합니다.

현재 보유 중인 데이터를 통해 당사의 설계에 맞는 방식으로 페이지를 구성할 수 있습니다.

```xml
import Head from "next/head";
import { useRouter } from "next/router";
import {
  AppBar,
  Toolbar,
  Typography,
  Button,
  Grid,
  Container
} from "@material-ui/core";
import api from "../lib/agility";

const HomePage = ({ meta, data }) => {
  const router = useRouter();
  const hero = data.Hero.fields;

  return (
    <>
      <Head>
        <title>{meta.title}</title>
        <meta name="viewport" content="initial-scale=1.0, width=device-width" />
        <meta name="description" content={meta.desc} />
      </Head>
      <Container maxWidth="lg" style={ marginTop: "30px" }>
        <Grid container spacing={3} alignItems="center">
          <Grid item xs={6}>
            <Typography variant="h2">
              {hero.title} <strong>{hero.subTitle}</strong>
            </Typography>
            <Typography variant="body1">{hero.announcement}</Typography>
            <Button
              variant="contained"
              color="primary"
              onClick={() => router.push(hero.primaryCTA.href)}
              role="link"
            >
              {hero.primaryCTA.text}
            </Button>
          </Grid>
          <Grid item xs={6}>
            <img
              src={hero.backgroundImage.url}
              alt={hero.backgroundImage.label}
              height="360px"
              width="100%"
            />
          </Grid>
        </Grid>
      </Container>
    </>
  );
};

export default HomePage;

// getServerSideProps() has been collapsed for readability.
```

우리는 받은 데이터를 메타와 데이터라는 두 가지 속성으로 나누었다. 메타 속성은 페이지에 대한 모든 SEO 관련 데이터로 구성되며, 이 데이터는 Agency CMS에서 페이지의 SEO 범주에 설정할 수 있습니다.

프로그램의 메타 태그에 추가한 경우 반응 헬멧에 익숙할 수 있습니다. next.js에는 `<head> 태그 next/head를 사용자 지정할 수 있는 자체 라이브러리가 있습니다. 페이지의 모든 메타 태그는 next/head가 제공하는 Head 구성 요소 안에 넣을 수 있습니다.

## 글로벌 탐색 모음 추가

일반 반응을 사용하면 전체 구성 요소 계층의 진입점 역할을 하는 `app.js` 파일에 익숙해집니다. 프로젝트 디렉터리를 보면 `app.js`가 없다는 것을 알 수 있습니다. 여기서 라우팅 로직을 설정했지만 Next.js에서 처리하므로 `app.js` 파일은 생성되지 않습니다.

전체 애플리케이션을 컴포넌트로 포장하거나 초기화 작업을 수행하거나 글로벌 스타일을 포함하려면 `_app.js`라는 파일을 생성해야 합니다. 이 구성 요소는 애플리케이션의 사용자 지정 `App` 구성 요소가 됩니다. 새로 생성된 `App` 구성 요소에 탐색 모음을 추가해 보겠습니다.

```coffeescript
import "../styles/global.css";
import { AppBar, Toolbar, Typography } from "@material-ui/core";

const App = ({ Component, pageProps }) => {
  return (
    <>
      <AppBar
        style={ marginBottom: "50px" }
        color="transparent"
        position="static"
      >
        <Toolbar>
          <Typography variant="h6">NextJS Blog</Typography>
        </Toolbar>
      </AppBar>
      <Component {...pageProps} />
    </>
  );
};

export default App;
```

<구성 요소/>는 어려서 앱 구성 요소에 로드될 수밖에 없는 페이지 구성 요소입니다. 이상적으로는 애플리케이션 구성 요소는 애플리케이션에 테마 공급자 또는 Redux <Provider>를 추가하는 것입니다.

한 가지 더 주의해야 할 점은 Next.js에서는 페이지 구성 요소에 글로벌 스타일을 추가할 수 없다는 것입니다. 이 경우 Next.js에서 해당 작업을 요청하는 오류가 발생합니다. 자, 바로 잡읍시다! `style`이라는 디렉토리와 해당 디렉토리에서 `global.css`라는 스타일시트를 작성한 후 다음 스타일로 붙여넣으십시오.

```css
body {
  margin: 0;
  box-sizing: border-box;
}
```

이렇게 하면 <body> 태그에 적용된 기본 여백이 제거되지만 이상적으로는 여기에 normal.cs와 같은 스타일 재설정을 추가하는 것이 좋습니다.

## Next.js에서 동적 라우팅 이해

next.js는 다양한 사용 사례를 염두에 두고 신중하게 구축한 매우 강력한 라우터를 가지고 있습니다. 아까 기억하시면 `[...]라는 파일을 만들었습니다.slug.js` 이 파일은 여러 사용 사례를 해결하는 Next.js의 동적 라우팅 기능을 활용합니다.

- `/페이지/[페이지 ID]js는 /page/1 또는 /page/2와 같은 경로와 일치하지만 /page/1/2는 일치하지 않습니다.
- `/페이지/[...페이지)`js는 /page/1/2와 같은 경로와 일치하지만 /page/는 일치하지 않습니다.
- / 페이지/[...]js는 /page/1/2 및 /page/와 같은 경로와 일치합니다.

주의할 점은 특정 경로 매개 변수와 이름이 같은 쿼리 매개 변수가 경로 매개 변수에 의해 덮어쓰기된다는 것입니다. 예를 들어 `/page/123?id=456`의 경우 `/page/[id] 경로가 있다면 ID는 456이 아니라 123이 됩니다.

이제 동적 경로에 대해 이해하셨으니 블로그 경로에 대해 알아보겠습니다.

```coffeescript
import api from "../../lib/agility";
import ErrorPage from "next/error";
import { Container, Typography } from "@material-ui/core";

const BlogPostPage = ({ page }) => {
  if (!page) {
    return <ErrorPage statusCode={404} />;
  }
  return (
    <Container maxWidth="lg">
      <Typography variant="h2">{page.fields.title}</Typography>
      <Typography variant="subtitle1">
        Written by: {page.fields.authorName} on{" "}
        {new Date(page.fields.date).toUTCString()}
      </Typography>
      <img
        style={ maxWidth: "inherit", margin: "20px 0" }
        src={page.fields.image.url}
        alt={page.fields.image.label}
      />
      <div dangerouslySetInnerHTML={ __html: page.fields.content } />
    </Container>
  );
};

export async function getServerSideProps(ctx) {
  const posts = await api.getContentList({
    referenceName: "posts",
    languageCode: "en-us"
  });
  const page = posts.items.find((post) => {
    return post.fields.slug === ctx.params.slug.join("/");
  });
  return {
    props: {
      page: page || null
    }
  };
}

export default BlogPostPage;
```

블로그포스트페이지는 여기서 우리가 활용할 수 있는 몇 가지 속성이 포함된 컨텍스트 객체(ctx)를 활용한다는 점을 제외하고는 모두 홈페이지와 같은 개념과 원칙을 따른다. `파람`은 우연히 그런 재산 중 하나이다. `params`에는 모든 경로 매개 변수와 해당 값이 포함됩니다.

또한 Agility CMS SDK의 `getContentList()라는 새로운 방법을 사용하고 있습니다. 이 방법을 사용하면 참조 이름으로 데이터 목록을 가져올 수 있습니다. 페이지 -> 블로그 -> 게시물 목록 -> 콘텐츠 편집으로 이동하면, 이미 작성된 게시물 목록이 표시됩니다.

수신한 데이터에서 어레이 고차 방법을 사용하여 우리가 `슬러그`로 선언한 라우터 매개 변수에 해당하는 슬러그를 가진 게시물을 찾는다. 이 페이지를 찾으면 페이지 세부 정보가 전달되고, 그렇지 않으면 null이 소포로 전달됩니다.

next.js에서는 `정의되지 않음`을 prop로 전달할 수 없습니다. 페이지 소품이 거짓일 경우 블로그 포스트 페이지는 대신 에러 페이지를 렌더링합니다!

### 동적 라우팅을 위한 공통 사용 사례 처리

경로 매개 변수가 전혀 정의되지 않은 상황에서 특징적인 게시물 목록이나 일종의 콘텐츠(즉, `/blog/inc)를 보여 주고자 하는 경우가 많다. 동적 라우팅의 선택적 캐치 올 구문을 사용할 수 있습니다([...slug]].js`) 그러나 slug route 매개 변수가 정의되지 않았는지 확인하고 대신 조건부로 렌더링해야 합니다.

더 나은 방법은 경로 매개 변수가 정의되지 않은 경우 대신 이 파일을 가리키도록 해당 경로 아래에 `index.js` 파일을 선언하는 것이다. 이 문제는 명명된/정적 경로가 동적 경로보다 우선순위가 높기 때문입니다.

예를 들어, `/blog/[id]와 같은 동적 경로를 선언한 경우.js는 `/blog/feature`와 같은 정적 경로이지만 Next.js는 동적 경로를 처리하는 파일보다 정적 경로를 처리하는 파일을 선호합니다.

이제 `blog` 디렉토리 아래에 `index.js`라는 파일을 만들어 보겠습니다.

```js
import {
  Card,
  Container,
  CardContent,
  CardActionArea,
  Typography,
  CardActions,
  Button,
  CardMedia,
  Grid
} from "@material-ui/core";
import Link from "next/link";
import api from "../../lib/agility";

const BlogPage = ({ posts }) => {
  return (
    <Container maxWidth="lg">
      <Typography variant="h2" gutterBottom="10px">
        Featured Blog Posts
      </Typography>
      <Grid container spacing={2}>
        {posts.map((post) => (
          <Grid item xs={4}>
            <Card>
              <CardActionArea>
                <CardMedia
                  style={ height: "240px" }
                  image={post.image.url}
                  title={post.image.label}
                />
                <CardContent>
                  <Typography gutterBottom variant="h5" component="h2">
                    {post.title}
                  </Typography>
                  <Typography
                    variant="body2"
                    color="textSecondary"
                    component="p"
                  >
                    Written by: {post.authorName} on{" "}
                    {new Date(post.date).toUTCString()}
                  </Typography>
                </CardContent>
              </CardActionArea>
              <CardActions>
                <Link href="/blog/[...slug]" as={`/blog/${post.slug}`} prefetch>
                  <Button role="link" size="small" color="primary">
                    Read
                  </Button>
                </Link>
              </CardActions>
            </Card>
          </Grid>
        ))}
      </Grid>
    </Container>
  );
};

export default BlogPage;

export async function getServerSideProps() {
  const posts = await api.getContentList({
    referenceName: "posts",
    languageCode: "en-us",
    take: 3
  });

  return {
    props: {
      posts: posts.items.map((post) => {
        const { title, slug, authorName, image, date } = post.fields;
        return { title, slug, authorName, image, date };
      })
    }
  };
}
```

이 페이지는 업무 개념상 블로그 포스트 페이지와 거의 동일하지만, 이러한 설정에서는 페이지를 사전 추출하는 것이 유리할 수 있다. Next.js를 사용하면 사용자가 해당 페이지를 방문할 것을 알고 있는 경우 페이지를 미리 가져올 수 있습니다.

그러기 위해서는 next/link를 통해 우리에게 제공되는 < 구성 요소를 사용해야 합니다. 동적 경로를 사용하므로 경로 매개변수의 값을 포함하는 `as` 소품을 사용해야 합니다.

Next.js 라우터를 누르고 프리페치 방법을 사용하여 페이지를 프로그래밍 방식으로 프리페치할 수도 있습니다. Next.js 라우터를 탭하려면 `useRouter() 후크를 사용해야 합니다.

```js
import { useRouter} from "next/router";
import { useEffect } from "react";
// If you want to prefetch the contact page 
// while in the home page, you can do so like this

const HomePage = () => {
 const router = useRouter();
 useEffect(() => {
  router.prefetch('/contact');
 }, []);
 // ...
}
```

## SSR 대 SSG: 최상의 선택

페이지 내용이 자주 변경될 때 서버측 렌더링을 선택하는 것이 가장 좋지만, 내용이 정적인 경우 정적 사이트 생성을 선택하는 것이 좋습니다.

즉, 대부분의 페이지가 정적일 경우 서버측 렌더링으로 한 번에 이동할 필요가 없습니다. 더 나은 접근 방식은 `use`와 같이 데이터를 가져오기 위해 Next.js에서 만든 사용자 지정 후크를 사용하여 클라이언트 측 데이터 가져오기를 사용하는 것입니다.SWR 또는 리액트 쿼리와 같은 라이브러리입니다.

SSR와 SSG 간에 결정하는 방법에 대한 자세한 내용은 Vercel 블로그를 참조하십시오.

## 결론

NAT에서 구축한 애플리케이션은 운영 환경에 전혀 대비하지 못하지만, 이제 Next.js의 작동 방식과 Agency CMS가 컨텐츠 관리 요구사항을 충족하는 데 어떤 도움을 줄 수 있는지 확실히 이해하셔야 합니다.

Next.js에 대해 자세히 알아보려면 생성된 단계별 튜토리얼 Next.js를 확인하십시오. 운영 환경에 더 적합한 Agency CMS 및 Next.js 콤보가 어떻게 표시되는지 보려면 이 예제 저장소를 확인하십시오.