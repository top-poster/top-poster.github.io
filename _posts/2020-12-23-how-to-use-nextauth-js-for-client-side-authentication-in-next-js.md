---
layout: post
title: "Next.js에서 클라이언트측 인증확인에 NextAuth.js를 사용하는 방법"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/next.js.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/next.js.png?fit=730%2C487&ssl=1)

## 도입

인증은 사용자 이름, 전자 메일 및 암호와 같은 사용자의 자격 증명이 확인되어 사용자가 말하는 사용자인지 확인하는 응용 프로그램에서 중요하고 중요한 기능입니다.

이용자에게 누구인지 물어본 뒤 증거를 받아 신원을 확인하는 방식이다. 시스템은 사용자가 제공한 자격 증명을 가져와서 유효한지 확인합니다.

이 문서에서는 NextAuth.js라는 강력한 보안 라이브러리를 사용하여 Next.js에서 암호가 필요하지 않은 클라이언트측 인증을 설정하는 방법에 대해 설명합니다.

이 게시물이 끝나면 사용자가 Ghub, Google, Facebook 계정을 사용하여 로그인할 수 있는 인증 앱이 생성됩니다. 등록에 성공하면 사용자의 프로필 사진, 전자 메일을 표시하여 소셜 미디어 계정에서 검색합니다.

우리는 리액트 훅과 기능성 부품을 이용하여 앱을 구축할 것입니다. 하지만 더 깊이 들어가기 전에, 우리가 사용할 두 가지 주요 도구를 살펴보도록 하자.

## Next.js란 무엇입니까?

Next.js는 React를 기반으로 구축된 프레임워크로, 프로덕션 준비 및 완벽하게 최적화된 React 앱을 매우 빠르고 쉽게 개발할 수 있도록 합니다. 리액트 생태계에서 나온 가장 좋은 것들 중 하나인데, 제로 구성으로 모든 힘을 발휘하기 때문입니다.

넥스트.js는 넷플릭스, 틱톡, 나이키와 같은 일류 회사들의 생산에 사용된다. 특히 리액트에 익숙할 때 배우는 것은 매우 쉽습니다.

## NextAuth.js란 무엇입니까?

NextAuth.js는 Next.js 응용 프로그램에서 인증을 구현하기 위한 완전히 안전한 인증 솔루션입니다. 모든 OAuth 서비스와 동기화되도록 설계된 유연한 인증 라이브러리로, 비밀번호 없는 로그인을 완벽하게 지원합니다.

데이터베이스와 함께 또는 데이터베이스 없이 사용할 수 있으며 MySQL, MongoDB, Postgre와 같은 널리 사용되는 데이터베이스를 기본적으로 지원합니다.SQL과 MariaDB.

OAuth 및 JSON Web Token과 같은 서비스와 동기화하여 데이터베이스 없이 사용할 수 있습니다.

### NextAuth.js의 작동 방식

NextAuth.js와 같은 라이브러리를 사용하면 OAuth를 사용하여 안전한 Next.js 애플리케이션을 구축해야 하는 경우처럼 ID 프로토콜 전문가가 될 필요가 없습니다. NextAuth.js는 사용자의 암호와 같은 중요한 데이터를 저장할 필요가 없도록 설계되었습니다. Next.js와 완벽하게 호환됩니다. 20줄의 코드만 있으면 앱에 인증 기능을 구현할 수 있습니다.

NextAuth.js에는 앱의 세션과 상호 작용하는 데 사용할 수 있는 클라이언트측 API가 있습니다. 공급자에서 반환되는 세션 데이터에는 사용자 페이로드가 포함되어 있으며, 로그인에 성공하면 사용자에게 이를 표시할 수 있습니다.

클라이언트에 반환된 세션 데이터는 다음과 같습니다.

```css
{
  expires: '2055-12-07T09:56:01.450Z';
  user: {
        email: 'sample@example.com';
        image: 'https://avatars2.githubusercontent.com/u/45228014?v=4';
        name: 'Ejiro Asiuwhu';
    }
}
```

페이로드에 중요한 데이터가 포함되어 있지 않은지 확인합니다. 세션 페이로드 또는 데이터는 프레젠테이션 용도로 사용됩니다. 즉, 사용자에게 표시되는 데이터입니다.

NextAuth.js는 사용자 로그인 상태를 확인하는 데 사용할 수 있는 useSession() React Hook을 제공합니다.

한편 NextAuth.js는 Retact 앱에서 사용하는 REST API를 제공한다. REST API NextAuth 노출에 대한 자세한 내용은 공식 문서를 참조하십시오.

## 요구 사항들

- 로컬 시스템에 설치된 노드 js 10.13 이상
- React.js의 기본 사항

## 시작 응용 프로그램 가져오기

npx create-next-app을 사용하여 Next.js 프로젝트를 만들었습니다.

이 리포지토리를 복제하고 계속 진행하려면 `시작자` 분기로 체크아웃합니다. 완료된 코드를 보려면 전체 분기로 체크아웃합니다.

```php
git clone https://github.com/ejirocodes/Nextjs_Authentication.git
```

리포지토리가 복제되면 작업 디렉토리로 전환합니다.

```bash
cd Nextjs_Authentication
```

Next.js 종속성 설치:

```coffeescript
npm i
```

`스타터` 분기로 전환:

```undefined
git checkout starter
```

개발 서버 시작:

```coffeescript
npm run dev
```

기본적으로 프로젝트는 포트 3000에서 실행됩니다. 브라우저를 시작하고 `http://localhost:3000`으로 이동합니다. 마지막으로 다음과 같은 작업을 수행해야 합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/starter-project.png?resize=730%2C339&ssl=1)

## Next.js 앱을 NextAuth.js와 연결

다음 auth를 설치하는 것이 첫 번째 단계다. next-auth는 npm 패키지이므로 설치는 간단하다.

```coffeescript
npm i next-auth
```

설치가 완료되면 패키지의 종속성에 `next-auth`가 추가되어야 합니다.json 파일 우리는 사용자들에게 GitHub, Google, Facebook 계정을 사용하여 우리의 앱에 로그인할 수 있는 선택권을 줄 것입니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/next-auth-installed.png?resize=653%2C294&ssl=1)

### GitHub OAuth 앱 생성

다음으로, 우리는 GitHub 인증 제공자를 추가할 것입니다. 이 제공자는 본질적으로 사용자들이 GitHub 계정을 사용하여 우리 앱에 로그인할 수 있게 해줍니다. 하지만 먼저 GitHub OAuth 앱을 만들어야 합니다. New OAuth 앱을 클릭하고 그에 따라 양식을 채웁니다. 자세한 내용은 공식 문서를 참조하십시오.

- 응용 프로그램 이름: 이 이름은 응용 프로그램 이름입니다. 그것은 아무거나 다 말할 수 있다. 그것은 별로 중요하지 않다.
- 홈페이지 URL: 앱 홈페이지 전체 URL입니다. 우리는 아직 개발 모드이기 때문에, 우리의 개발 서버가 실행 중인 전체 URL을 채울 것입니다. 제 경우 http://localhost:3000입니다.
- 권한 부여 콜백 URL: GitHub가 앱에 로그인한 후 사용자를 리디렉션할 URL입니다. 홈페이지 URL에 `/api/auth/callback`이 추가되어 `http://localhost:3000/api/auth/callback`이 생성되어야 합니다.

GitHub은 OAuth App을 등록한 후 새로 만든 앱에 대한 클라이언트 ID와 클라이언트 암호를 만듭니다. 클라이언트 ID와 암호 키를 클립보드에 복사합니다. 새 클라이언트 암호 생성을 클릭하고 클라이언트 암호를 가져옵니다.

### 환경 변수 추가

프로젝트의 루트 디렉터리에 `.env.local` 파일을 생성하십시오. 임의의 이름이 될 수 없습니다. 그런 다음 다음 내용으로 채웁니다.

```undefined
GITHUB_ID=<cilent id of your github auth app should go in here>
GITHUB_SECRET=<clent secret of your github app should go in here>
NEXTAUTH_URL=http://localhost:3000
```

> 'NEXTAUTH_URL'은 저희 앱의 URL입니다. 개발 서버가 실행 중인 포트를 사용해야 합니다.

이제 우리 앱으로 돌아가자. 우리는 `[...next auth]라는 파일을 만들 것이다.page/api/auth`의 js는 다음 코드 행을 추가합니다.

```js
import NextAuth from 'next-auth'
import Providers from 'next-auth/providers'

const options = {
    providers: [
        Providers.GitHub({
            clientId: process.env.GITHUB_ID,
            clientSecret: process.env.GITHUB_SECRET
        }),
    ],
}

export default (req, res) => NextAuth(req, res, options)
```

1번 라인에서는 NextAuth를 수입하고 있습니다. 라인 2에서는 OAuth와 사용자 로그인이 가능하도록 앱에 통합할 수 있는 서비스인 `next-auth` 라이브러리에서 프로바이더를 가져오고 있습니다.

6번 온라인에서는 제공자에 GitHub를 구성하고 환경변수를 통해 GitHub 비밀과 클라이언트 id를 전달한다. 마지막으로, 라인 13에서는 NextAuth를 반환하고 옵션 변수를 세 번째 매개 변수로 사용하는 함수를 내보냅니다.

신사 숙녀 여러분, 마법은 이미 일어났습니다. next-auth가 제공하는 REST API를 활용하면 GitHub 계정으로 로그인 할 수 있습니다. `https://localhost:3000/api/auth/signin`으로 이동하면 다음과 같은 👇🏽이(가)

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/sign-in-github-page.png?resize=730%2C361&ssl=1)

그 과정을 따라라, 그리고 샤잠! 로그인했습니다. 다음으로, 사용자의 로그인 상태를 저장하고 표시해야 합니다.

### 'useSession() 후크를 사용하여 사용자 로그인 상태 확인

사용자의 로그인 상태를 가져와 앱의 프런트 엔드에 사용자 세부 정보를 렌더링해야 합니다. 사용자 지정 앱이라고 하는 Next.js 기능을 사용하면 이 작업을 쉽게 수행할 수 있습니다. 그런 다음 구성 요소를 공급자로 포장합니다.

`페이지` 디렉토리에 `_app.js` 파일을 만들고(아직 존재하지 않는 경우) 다음 코드 행을 추가합니다.

```js
import { Provider } from 'next-auth/client'
function MyApp({ Component, pageProps }) {
  return (
      <Provider session={pageProps.session}>
        <Component {...pageProps} />
      </Provider>
  )
}
export default MyApp
```

구성 요소를 공급자에 줄임으로써 페이지 간에 세션 상태를 공유할 수 있게 되며, 이는 페이지 탐색 중에 상태를 유지하고 성능을 향상시키며 네트워크 트래픽을 감소시킵니다.

그런 다음 `components/Header.js` 파일을 열고 `next-auth/client`에서 `useSession, SignIn 및 SignOut`을 가져옵니다.

```coffeescript
import { useSession, signIn, signOut } from 'next-auth/client'
```

사용자 로그인 및 로그아웃 상태를 관리하는 데 사용 세션이 사용되며, 앱에서 로그인 및 로그아웃 기능을 수행하는 데 로그인 및 로그아웃 기능이 사용됩니다.

세션 사용 후크를 사용합니다.

```cpp
 const [session] = useSession();
```

세션에서 사용자의 세부 정보를 반환합니다. 반환된 세부 정보를 사용하여 로그인 및 로그아웃 버튼을 조건부로 렌더링합니다.

`components/Header.js`의 반환문에 있는 모든 항목을 다음 코드 행으로 바꿉니다.

```js
 <div className='header'>
      <Link href='/'>
        <a className='logo'>NextAuth.js</a>
      </Link>
           {session && <a href="#" onClick={handleSignout} className="btn-signin">Sign out</a>  } 
           {!session && <a href="#" onClick={handleSignin}  className="btn-signin">Sign in</a>  } 
    </div>
```

사용자가 로그인하고 로그아웃할 수 있도록 로그인 및 로그아웃 방법을 만들어야 합니다.

```coffeescript
const handleSignin = (e) => {
      e.preventDefault()
      signIn()
  }    
const handleSignout = (e) => {
      e.preventDefault()
      signOut()
    }
```

이제 `Header.js`는 다음과 같이 표시됩니다.

```js
import Link from 'next/link'
import { useSession, signIn, signOut } from 'next-auth/client'
export default function Header() {

  const [session] = useSession();

    const handleSignin = (e) => {
      e.preventDefault()
      signIn()
  }
    const handleSignout = (e) => {
      e.preventDefault()
      signOut()
    }
  return (
    <div className='header'>
      <Link href='/'>
        <a className='logo'>NextAuth.js</a>
      </Link>
           {session && <a href="#" onClick={handleSignout} className="btn-signin">Sign out</a>  } 
           {!session && <a href="#" onClick={handleSignin}  className="btn-signin">Sign in</a>  } 
    </div>
  )
}
```

헤더 버튼을 사용하여 얼마든지 로그인하고 앱에서 로그아웃할 수 있습니다.

### 사용자 정보 검색 및 표시

이제 `pages/index.js`로 이동합니다. 로그인 세부 정보를 기반으로 사용자의 세부 정보를 표시하고 조건부로 렌더링해야 합니다. 먼저, 우리는 node_modules 폴더의 next-auth에서 useSession Hook을 가져와야 한다.

따라서 `index.js`를 다음 내용으로 바꾸십시오.

```xml
import Head from 'next/head'
import Header from '../components/Header'
import styles from '../styles/Home.module.css'
import { useSession } from 'next-auth/client'

export default function Home() {

  const [session, loading] = useSession();

  return (
    <div className={styles.container}>
      <Head>
        <title>Nextjs | Next-Auth</title>
        <link rel="icon" href="/favicon.ico" />
      </Head>
      <Header />
      <main className={styles.main}>
        <h1 className={styles.title}>Authentication in Next.js app using Next-Auth</h1>
        <div className={styles.user}>
          {loading && <div className={styles.title}>Loading...</div>}
          {session && <> <p style={ marginBottom: '10px' }> Welcome, {session.user.name ?? session.user.email}</p> <br />
            <img src={session.user.image} alt="" className={styles.avatar} />
          </>}
          {!session &&
            <>
              <p className={styles.title}>Please Sign in</p>
              <img src="https://cdn.dribbble.com/users/759083/screenshots/6915953/2.gif" alt="" className={styles.avatar} />
        <p className={styles.credit}>GIF by <a href="https://dribbble.com/shots/6915953-Another-man-down/attachments/6915953-Another-man-down?mode=media">Another man</a> </p>            
</>
          }
        </div>
      </main>
    </div>
  )
}
```

위에서는 사용자의 이미지, 이름 및 사진이 우리의 앱에 로그인되어 있는 경우 세션 상태에서 얻은 데이터를 사용하여 조건부로 렌더링하고 있습니다. 그렇지 않으면 로그인하라는 텍스트가 있는 더미 GIF를 표시합니다.

### Google OAuth 앱 만들기

사용자가 Google 계정을 사용하여 앱에 로그인할 수 있도록 하려면 Google API Console에서 OAuth 2.0 클라이언트 자격 증명을 받아야 합니다. 자격 증명으로 이동하고 자격 증명 만들기를 클릭한 다음 OAuth 클라이언트 ID:

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/create-google-oauth-client-id.png?resize=730%2C261&ssl=1)

다음 항목을 입력하라는 메시지가 표시됩니다.

- 이름: 응용 프로그램 이름입니다. 그것은 아무거나 부를 수 있다.
- URIs: 이것은 저희 앱의 홈페이지 전체 URL입니다. 우리는 아직 개발 모드이기 때문에, 우리의 개발 서버가 실행 중인 전체 URL을 채울 것입니다. 제 경우는 http://localhost:3000입니다.
- 인증 콜백 URL: `http://localhost:3000/api/auth/callback/google`로 인증한 후 사용자가 이 경로로 리디렉션됩니다.

코딩할 때 클라이언트 ID와 클라이언트 암호를 표시하는 팝업이 나타납니다. 파일을 복사하여 `env.local` 파일에 추가합니다.

```coffeescript
GOOGLE_ID=<cilent id of your google auth app should go in here>
GOOGLE_SECRET=<cilent secret of your google auth app should go in here>
```

다음으로 `page/api/auth/[...next auth]`로 이동합니다.js`를 선택하고 다음을 공급자 배열에 추가합니다.

```css
   Providers.Google({
      clientId: process.env.GOOGLE_ID,
         clientSecret: process.env.GOOGLE_SECRET,
    }),
```

당신의 `[... 다음 저자.js는 이제 다음과 같이 보입니다.

```js
import NextAuth from 'next-auth'
import Providers from 'next-auth/providers'
const options = {
    providers: [
        Providers.GitHub({
            clientId: process.env.GITHUB_ID,
            clientSecret: process.env.GITHUB_SECRET
        }),
        Providers.Google({
            clientId: process.env.GOOGLE_ID,
            clientSecret: process.env.GOOGLE_SECRET,
        }),
    ],
}
export default (req, res) => NextAuth(req, res, options)
```

Google 계정을 사용하여 사용자 로그인을 테스트하려면 개발 서버를 종료하고 `npm run dev`를 실행하십시오.

이제, 손가락을 깍지 끼고 머리를 높이 쳐들면, 우리는 구글 계정으로 우리의 앱에 로그인할 수 있을 것입니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/sign-in-google.png?resize=730%2C304&ssl=1)

### Facebook OAuth 앱 만들기

앱에 페이스북 로그인을 이용하려면 페이스북 개발자 계정이 필요합니다. 계정을 만든 다음 앱을 등록하고 양식을 작성합니다. 앱의 플랫폼을 선택하라는 메시지가 나타나면 웹을 선택합니다.

작업이 완료되면 화면에 다음과 같이 표시됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/login-options.png?resize=730%2C295&ssl=1)

Facebook Login(페이스북 로그인)을 클릭하고 Web(웹)을 선택합니다. 다음을 추가합니다.

- 사이트 URL: 개발 서버 `http://localhost:3000/`에 대한 URL을 입력하십시오.

앱 ID와 앱 암호를 가져오려면 설정에서 Basic으로 이동하여 복사한 후 다음과 같이 `env.local` 파일에 추가하십시오.

```coffeescript
FACEBOOK_ID=<app id of your facebook app should go in here>
FACEBOOK_SECRET=<app secret of your facebook app should go in here>
```

> 앱 도메인 필드에 'http://localhost:3000/'을 추가하는 것을 잊지 마십시오.

다음으로 `page/api/auth/[...next auth]`로 이동합니다.js`를 선택하고 다음을 공급자 배열에 추가합니다.

```css
    Providers.Facebook({
            clientId: process.env.FACEBOOK_ID,
            clientSecret: process.env.FACEBOOK_SECRET
      })
```

당신의 `[... 다음 저자.js는 이제 다음과 같이 보입니다.

```js
import NextAuth from 'next-auth'
import Providers from 'next-auth/providers'
const options = {
    providers: [
        Providers.GitHub({
            clientId: process.env.GITHUB_ID,
            clientSecret: process.env.GITHUB_SECRET
        }),
        Providers.Google({
            clientId: process.env.GOOGLE_ID,
            clientSecret: process.env.GOOGLE_SECRET,
        }),
        Providers.Facebook({
            clientId: process.env.FACEBOOK_ID,
            clientSecret: process.env.FACEBOOK_SECRET
        })
    ],
}
export default (req, res) => NextAuth(req, res, options)
```

개발 서버를 재시작하면 사용자는 GitHub, Google 및 Facebook 계정을 사용하여 Next.js 앱에 로그인할 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/sign-in-facebook.png?resize=730%2C339&ssl=1)

## 결론

이 게시물에서는 사용자를 식별하고 사용자 프로필 정보를 가져오고 프런트 엔드에서 렌더링하는 보안 라이브러리인 NextAuth.js를 사용하여 Next.js에서 사용자 인증을 구현했다.

대부분의 사용 사례를 다루었지만 NextAuth.js로 할 수 있는 일이 훨씬 많습니다. JSON 웹 토큰, 보안 페이지 등을 사용하여 데이터베이스를 추가할 수 있습니다.

더 많은 콘텐츠를 원하는 경우 NextAuth.js 공식 문서의 튜토리얼 페이지를 확인하십시오.

이 튜토리얼에 대해 어떻게 생각하셨는지 아래 코멘트 부분에 알려주세요. 나는 트위터와 깃허브에서 친하다. 읽어주셔서 감사하고 계속 조율해 주세요.