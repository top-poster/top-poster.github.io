---
layout: post
title: "Next.js 10의 새로운 기능"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/nextjs10.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/nextjs10.png?fit=730%2C487&ssl=1)

## 도입

next.js는 최근 오랫동안 기다려온 주요 버전 중 하나(버전 10)를 출시했으며, 이 릴리스의 일부로 롤아웃된 많은 흥미로운 기능들이 있다. 이 기사에서는 이러한 각 항목을 개별적으로 세분화하여 개발 경험과 사용자 경험에 어떤 영향을 미칠지 이해할 것입니다. 이 GitHub 저장소에 언급된 코드를 따라 아래에 언급된 대부분의 기능을 테스트할 수 있습니다(모든 종속성을 설치하려면 npm install을 실행하고 localhost:3001에 웹 사이트를 배포하는 dev 서버를 실행하려면 npm run dev를 실행해야 함).

## 새로운 '이미지' 구성 요소

수년간 Next.js 팀은 플랫폼을 사용하여 구축된 사이트의 속도를 개선하려고 노력해 왔습니다. 정적 생성, 서버 측 렌더링과 같은 최적화 전략을 사용하고 브라우저에서 로드되는 JavaScript 번들 크기를 보다 압축적으로 만듭니다. 이는 사이트에 제공되는 마크업과 JavaScript를 최적화하는 데 도움이 되었지만, 이러한 사이트의 일부로 제공되는 이미지(웹 사이트 크기의 거의 50%를 구성함)는 여전히 최적화되지 않은 상태로 남아 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/image_component.png?resize=730%2C324&ssl=1)

이미지를 적절하게 관리하면 사이트 성능에 심각한 영향을 미칩니다. 그 이유는 주로 다음과 같습니다.

- 이미지가 최적으로 압축되지 않고 크기가 각각 1메가바이트 이상임
- 이 사이트는 원래 이미지 크기(2000x2000픽셀)를 로드하지만 모바일 장치에서는 매우 작은 버전의 이미지가 표시됩니다.
- 이러한 이미지는 대부분 처음에 뷰포트 외부에 있으므로 사용자가 스크롤할 때까지 브라우저가 로드하지 않아도 됩니다.

여기서 Next.js 이미지 구성요소가 도움이 될 수 있습니다. Next.js 팀이 사용할 수 있는 새로운 구성 요소로, 이미지와 관련된 대부분의 무거운 리프팅이 개발자에게서 멀어지게 한다. 개발자가 해야 할 유일한 작업은 img HTML 태그를 next/image의 image 태그로 교체하는 것이다. 예는 다음과 같습니다.

HTML 코드:

```xml
<img src="/profile-picture.jpg" width="400" height="400" alt="Profile Picture">
```

next.js 등가:

```js
import Image from 'next/image';

<Image src="/profile-picture.jpg" width="400" height="400" alt="Profile Picture">
```

릴리스 문서에 따라 Next.js 팀은 Google Chrome 팀과 협력하여 다음을 지원하도록 `Image` Retact 구성 요소를 최적화했습니다.

- 포장 개봉 후 로딩 및 프리로드가 느림
- 이미지 치수를 미리 렌더링하면 이미지 로드 후 레이아웃 이동이 방지됩니다.

그 외에도, 더 작은 크기를 생성하고 WebP와 같은 최신 이미지 형식으로 변환함으로써 이미지가 더욱 최적화된다. 또한 이미지가 뷰포트에 진입하려고 할 때마다 필요할 때마다 발생하는 모든 작업을 빌드 시간이 아니라 필요에 따라 수행하므로 사이트 구축 시간이 영향을 받지 않습니다. 전반적으로, 이미지를 훨씬 더 쉽고 효율적으로 처리할 수 있습니다.

## 실제 사용 사례

이것을 우리의 코드에 사용함으로써 실제 사례를 통해 시험해 보자. `Image` 구성 요소를 테스트하기 위한 코드는 `pages/test-image.js`에서 찾을 수 있습니다. 이 이미지는 언스플래시의 공용 폴더로 다운로드되어 초기 보기 포트에서 벗어나도록 낮은 위치의 페이지에 삽입되었습니다. 코드베이스에서 이 이미지는 원래 .jpeg 이미지 3648x3951 픽셀이며 크기는 약 1MB이다. 페이지 경로가 로드되면 네트워크 탭에서 이미지가 처음에 로드되지 않고 맨 아래로 스크롤하여 이미지에 도달하려는 경우에만 로드되기 시작하는지 확인할 수 있습니다. 이것은 아래의 gif에서 분명히 볼 수 있다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/scroll2.gif?resize=730%2C386&ssl=1)

또한 이미지가 완전히 로드되지 않았는데도 렌더 레이아웃이 변경되지 않음을 알 수 있습니다. 그런 다음 렌더링할 이미지 치수를 미리 계산하고 이미지 아래에 있는 요소(gif에도 분명히 나타나 있음)에 레이아웃 이동이 없도록 DOM에 있는 이미지의 자리 표시자를 유지합니다.

또 다른 주목할 점은 이미지 자체가 추가로 최적화되어 있다는 점인데, 네트워크 탭에서 이를 확인할 수 있다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/img.png?resize=730%2C649&ssl=1)

영상 형식이 최적화된 webp 형식으로 변경되었으며, 가로 세로 비율을 유지하면서 원래 영상에서 치수(1080x1170픽셀)가 수정되었습니다. 이러한 최적화로 인해 크기가 원래 크기에서 37kB로 감소했습니다. 따라서 모든 장치에서 이미지를 작업하는 동안 전반적인 성능이 향상됩니다.

## 국제화 지원

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/internationalization.png?resize=730%2C401&ssl=1)

Next.js 공식 페이지에 따르면:

> 72%의 소비자들은 번역이 되면 당신의 사이트에 머무를 가능성이 더 높으며, 55%의 소비자들은 그들의 모국어로만 전자상거래 사이트에서 구입한다고 말했다.

따라서 사이트의 소비자 기반을 구성하는 대부분의 공통 지역을 지원하는 것이 필수적입니다. 이 릴리스에서는 Next.js에 대해 다루었습니다.

next.js는 이제 가장 일반적인 라우팅 전략인 하위 경로 라우팅(로컬이 URL의 일부로 존재하며 모든 언어가 단일 도메인에서 지원됨)과 도메인 라우팅(로컬이 최상위 도메인에 매핑됨)을 모두 지원합니다.

국제화를 시작하려면 다음 .config.js에 다음 줄을 추가해야 합니다.

```java
module.exports = {
  i18n: {
    locales: ['en', 'nl'],
    defaultLocale: 'en'
  }
}
```

루트 디렉터리에 `next.config.js` 파일을 생성하여 이 국제화 구성을 예제 프로젝트에 추가할 수 있습니다. 로케일은 en-US 또는 nl-NL로 정의하여 로케일 지원에서 보다 구체적일 수 있다. 이 작업이 완료되고 dev 서버가 다시 시작되면 사용자가 생성한 이전 페이지도 localhost:3001/fr/test-image 경로(프랑스어 버전의 페이지)에서 액세스할 수 있음을 알 수 있습니다. 이 페이지는 브라우저에서 기본 로케일이 `fr`로 설정된 사용자에게 제공되는 페이지입니다. 설정 페이지에서 기본 로케일을 프랑스어로 변경하여 Google Chrome에서 동일한 로케일을 테스트할 수 있습니다.

변경 내용이 저장되면 새로 고치면 홈 경로를 방문하면 사이트의 `/fr` 버전으로 리디렉션됩니다. 즉, Next.js는 `/` 경로의 기본 로케일을 자동으로 검색합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/localeChange.png?resize=1442%2C788&ssl=1)

그리고 그것이 그렇게 원활하게 작동되는 이유는 크롬이 처음 페이지(프랑스가 목록의 맨 앞에 있는)를 가져오라는 요청에서 보내는 `수락 언어` 헤더 때문이다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/french-1.gif?resize=730%2C510&ssl=1)

도메인 라우팅은 하위 경로 라우팅과 달리 다음과 같은 추가 구성이 필요합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/acceptLanguage2.png?resize=1152%2C916&ssl=1)

```java
module.exports = {
  i18n: {
    locales: ['en', 'fr'],
    domains: [
      {
        domain: 'example.com',
        defaultLocale: 'en'
      },
      {
        domain: 'example.fr',
        defaultLocale: 'fr'
      }
    ]
  }
}
```

특정 도메인을 각 로케일에 매핑합니다. 따라서 요청이 홈 경로 `/l`에 도착할 때마다 해당 로케일에 해당하는 지정된 도메인으로 리디렉션됩니다.

하지만 현재 사용 중인 로케일을 감지하고 그에 따라 특정 결정을 내려야 한다면 어떨까요? 그것은 useRouter 후크를 이용하여 next/router를 통해 가능하다. 아래 코드에서 먼저 `useRouter`에서 로케일을 읽은 다음 `Link` 구성 요소(로케일 프로펠러 허용)를 사용하여 로케일로 리디렉션합니다.

```cpp
const router = useRouter();
// store the current locale
const currentLocale = router.locale;

// somewhere in the JSX, pass currentLocale to Link:
<Link
  href="/test-image" 
  locale={currentLocale}
>
...
</Link>
```

이를 통해 메인 페이지에서 링크를 클릭하면 이전과 같이 기본 페이지가 아닌 `localhost:3001/fr/test-image`로 리디렉션됩니다. 테스트 이미지 경로에서 로캘에 비슷한 방식으로 액세스할 수 있으며 화면에 프랑스어의 특정 아티팩트를 렌더링할 수 있습니다.

getStaticProps 또는 getServerSideProps와 함께 작업하는 동안 Next.js 설명서에 명시된 컨텍스트 객체를 사용하여 유사한 작업을 수행할 수 있습니다.

> getStaticProps 또는 getServerSideProps로 페이지를 미리 렌더링할 때 로케일 정보가 함수에 제공된 컨텍스트로 제공됩니다.

이것은 Next.js 10에 소개된 엔드 투 엔드 로케일 지원의 요약이다.

## next.js커머스

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/ecom.png?resize=730%2C464&ssl=1)

next.js 10은 BigCommerce와 협업하여 next.js Commerce를 출시했는데, Next.js Commerce는 몇 번의 클릭만으로 전자상거래 사이트 구축을 시작할 수 있는 올인원 키트입니다. 가장 좋은 점은 Next.js Commerce를 사용하여 전자상거래 사이트를 구축할 때 Next.js의 모든 좋은 부분을 포장에서 꺼낸다는 것입니다. 그 중 일부는 다음과 같습니다.

- 파일 시스템 기반 라우팅
- 코드 분할 및 번들링
- SSR 및 SSG를 사용한 하이브리드 렌더링
- 빠른 새로 고침

Next.js Commerce 사이트를 배포하기 위한 유일한 전제 조건은 BigCommerce 스토어를 구성해야 한다는 것입니다. 그러나 아직 해당 기능이 없는 경우 계속 시도하려면 Next.js commerce 홈 페이지로 이동하여 Clone(복제)을 클릭하면 됩니다.

- BigCommerce에 가입합니다. 아직 더미 저장소를 설정하지 않은 경우(BigCommerce에서 등록할 때 제공되는 옵션)
- Next.js가 사이트와 관련된 코드를 배치할 GitHubrepo를 생성합니다. 이 보고서로 밀어넣으면 사이트가 다시 배포됩니다.
- Next.js를 사용하여 사이트 배포

바로 그거야! 이 설치 완료 화면을 보여주는 데모를 위해 5-7분 이내에 완전한 기능을 갖춘 Next.js 호환 eCommerce 사이트를 배포할 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/deployed.png?resize=730%2C367&ssl=1)

여기에서 방문을 클릭하면 방금 배포된 새로운 전자상거래 사이트로 이동하고 대시보드 열기를 클릭하면 Vercel의 배포 관리 페이지가 열립니다.

## 기타 주목할 만한 특징

### 반응 17 지지대

next.js는 이제 React 17을 지원하는데, 이는 JSX 변환과 같은 최신 React 기능을 React와 Next를 최신 버전으로 업그레이드하기만 하면 사용할 수 있다는 것을 의미한다. React의 이전 버전에서는, 컴파일 단계에서 JSX 코드가 동등한 React.createElement() 코드로 컴파일되어, 동작하기 위해서는 React가 적용되어야 한다. 그러나 React 17 이후부터는 JSX 변압기를 컴파일러에 의해 자동으로 가져오게 된다. 최신 버전으로 이동한 후 리액트를 .jsx 파일로 명시적으로 가져올 필요가 없는 것 외에도 개발자는 다른 작업을 수행할 필요가 없습니다. 다음은 이전 프로젝트를 최신 버전의 Next.js로 업그레이드하기 위해 실행하는 명령입니다.

```coffeescript
npm install next@latest react@latest react-dom@latest
```

### 빠른 새로 고침

GetStaticProps 및 GetServerSideProps 기능이 변경되면 페이지를 새로 고칠 필요 없이 자동으로 다시 실행됩니다. next/mdx로 작성된 모든 마크다운 페이지도 빠른 새로 고침을 지원합니다.

### 타사 CSS

이 기능을 사용하면 `_app.js`에서 CSS를 가져올 필요 없이 단일 구성 요소를 위한 CSS에 대해 코드 분할이 가능하다. 이러한 종류의 CSS 가져오기 사용 사례는 단일 구성 요소에만 적용되며 코드베이스의 다른 곳에서는 사용할 필요가 없는 스타일이다(_app 파일에서 스타일을 가져오는 강력한 사용 사례).

### href에서 자동 확인

next/link를 사용하여 동적으로 라우팅하는 동안, 우리는 이전의 표준이었던 href와 as가 아닌 href 속성만 제공하면 된다.

```js
<Link href="/books/[bookId]" as={`/books/${bookId}`}>
  Harry Potter
</Link>
```

이전 시나리오에서 href는 런타임에 변경되지 않고 경로에 형식을 제공하는 정적 문자열이었다. 반면, `as` 소품은 실제 연결을 수행하는 동적으로 계산된 경로였다. Next.js 10 이후부터는 다음과 같은 코드로 나타납니다.

```xml
<Link href={`/book/${bookId}`}>
  Harry Potter
</Link>
```

여기서 `href`가 유일한 필수 매개 변수이고 `as`는 다음 설명과 함께 선택적 매개 변수로 남아 있습니다.

> 브라우저 URL 표시줄에 표시될 경로에 대한 선택적 장식기. Next.js 9.5.3 이전에는 동적 경로에 이 기능이 사용되었습니다.

### 코드모드 CLI

next.js는 새로운 기능으로 업그레이드하고 오래된 기능을 쉽게 더 이상 사용하지 않기 위해 단일 명령을 통해 전체 응용 프로그램을 업데이트하고 호환되도록 하는 `@next/codemod` CLI를 제공하고 있습니다.

```css
npx @next/codemod <transform> <path>
```

### GetStaticPaths에 대한 폴백 차단

GetStaticPaths를 반환하는 동안 fallback: `blocking` 플래그를 설정하면 브라우저로 정적 fallback이 전송되지 않고 초기 요청이 사전 렌더링 대기 상태일 때 차단 동작이 활성화됩니다. 초기 렌더링을 수행한 후 이후 요청에 대해 페이지를 다시 사용합니다.

### 'Redirect' 지원

getStaticProps 및 getServerSideProps와 함께 작업하는 동안 다음과 같은 리디렉션이라는 새 필드를 반환할 수 있습니다.

```js
export function getStaticProps() {
  return {
    // returns a redirect to an internal page `/another-page`
    redirect: {
      destination: '/another-page',
      permanent: false
    }
  }
}

export function getServerSideProps() {
  return {
    // returns a redirect to an external domain `example.com`
    redirect: {
      destination: 'https://example.com',
      permanent: false
    }
  }
}
```

리디렉션 구성에 지정된 대상이 요청을 리디렉션하는 데 사용됩니다. 로컬 호스트/테스트-리디렉트 경로를 방문하여 이 정보를 확인할 수 있으며, 리디렉션 구성이 사용되고 있기 때문에 /fast-refresh 페이지로 이동하는지 확인할 수 있습니다.

### 찾을 수 없음 지원

리디렉트 구성과 마찬가지로 응답의 not Found(찾지 않음) 플래그를 true로 반환하면 기본 404 페이지는 개발자 측의 수동 개입 없이 404 상태로 사용자에게 반환된다. 이는 일반적으로 Get StaticProps 함수에서 일부 비즈니스 논리를 실행할 때 유용하지만, 우리가 이 경로를 지원하지 않는 것은 분명하다. 그런 다음 사용자 정의 UI에 해당 정보를 표시하거나 페이지에 리디렉션 로직을 쓰는 대신 코드베이스의 `not-found.js` 구성 요소에 표시된 대로 리디렉션을 처리하는 `getStaticProps`에서 플래그를 반환하면 됩니다.

```js
export async function getStaticProps() {
  let notFound = false;

  // perform some business logic here.
  // Got to know that the page need not show up
  notFound = true;

  return {
    notFound: notFound
  }
}
```

## 결론

전반적으로, 이 릴리스는 개발자뿐만 아니라 사용자 환경에서도 많은 주요 개선 사항을 제공합니다. 이러한 모든 주요 기능 향상과 이 기능이 중단되지 않는 주요 릴리스라는 사실은 Next.js 10을 반드시 시도해야 하는 것으로 만든다. 따라서 이전 버전의 Next.js를 계속 사용하고 있다면 지금이 업그레이드하기에 적절한 시기일 수 있습니다.