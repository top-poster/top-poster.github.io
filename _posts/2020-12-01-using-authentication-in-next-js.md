---
layout: post
title: "Next.js의 인증: NextAuth.js로 인증 API 구축"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/authentication-nextjs.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/authentication-nextjs.png?fit=730%2C487&ssl=1)

인증은 예를 들어 응용프로그램에서 사용자의 ID를 확인하는 다른 사용자의 신원을 확인하는 작업입니다. 이 튜토리얼에서는 Next.js 앱에서 인증 솔루션을 처리하도록 특별히 설계된 라이브러리인 NextAuth.js를 사용하여 Next.js 앱에서 인증을 구현하는 방법에 대해 알아봅니다.

설명서에 따르면 NextAuth.js는 Next.js 애플리케이션을 위한 완전한 오픈 소스 인증 솔루션입니다. Next.js 및 Serverless를 지원하도록 처음부터 설계되었습니다."

## 전제조건

이 튜토리얼을 수행하려면 다음이 필요합니다.

- 반응에 대한 기본적인 이해
- MongoDB 또는 기타 데이터베이스에 대한 기본 이해
- 인증 작동 방식에 대한 기본 이해

선물하기:

- Next.js 프레임워크에 대한 이전 경험

## 인증 API 구축

이 튜토리얼에서는 기본 Next.js API 경로를 사용하여 기본 인증 API를 구축하려고 합니다. 인증은 암호가 없는 전자 메일 로그인 및 Google과의 개방형 인증으로 구성됩니다. 그런 다음 API 끝점과 보호된 페이지 보안에 대해 알아보겠습니다.

이 튜토리얼에 작성된 모든 코드는 이 GitHub 저장소에서 사용할 수 있습니다.

## 응용 프로그램 비계 지정

next.js에는 시작 프로젝트를 생성하는 데 사용할 수 있는 편리한 CLI가 있습니다. 시작하려면 CLI를 전체적으로 설치하십시오.

```undefined
npm install -g create-next-app
```

이제 새 Next.js 앱을 생성합니다.

```undefined
create-next-app next-authentication
```

템플릿을 선택하라는 메시지가 나타나면 기본 시작 앱 옵션을 선택하고 Enter 키를 눌러 계속합니다.

이제 디렉토리를 새로 만든 프로젝트 폴더로 변경합니다.

```bash
cd next-authentication
```

그런 다음 개발 서버를 시작합니다.

```undefined
yarn dev
```

개발 서버는 http://localhost:3000에서 시작해야 합니다.

## 환경 변수 생성

우리는 몇 가지 자격 증명을 가지고 일을 할 것이기 때문에, 우리는 그것들을 숨길 필요가 있다. 프로젝트 폴더의 루트인 `.env.local`에 새 파일을 생성하고 다음 조각에 붙여넣으십시오.

```undefined
NEXTAUTH_URL=http://localhost:3000
```

이것은 당사의 개발 서버의 URL과 동일합니다. 다른 포트에서 실행 중인 경우 교체합니다. 진행하면서 이 파일을 채우겠습니다.

## NextAuth.js 설정 중

이 단계에서는 다음 인증 종속성을 설치하고 API 경로를 사용할 예정입니다. Next.js의 API 경로를 사용하면 사용자 지정 서버를 만들지 않고도 API 끝점을 만들 수 있습니다.

API 경로는 개발 중에 한 서버에서 실행되며, 배포 시 서로 독립적으로 실행되는 serverless 기능으로 배포됩니다. 설명서의 API 경로에 대해 자세히 알아봅니다.

### 종속성 설치

아래 조각을 실행하여 `next-auth`를 설치합니다.

```undefined
yarn add next-auth
```

### 동적 API 경로 생성

`[…next auth]라는 새 파일을 만듭니다.js`를 `page/api/auth`로 지정하고 이 조각을 새로 만든 파일에 붙여 넣습니다.

```js
// pages/api/auth/[...nextauth].js
import NextAuth from 'next-auth'

const options = {
  site: process.env.NEXTAUTH_URL
}

export default (req, res) => NextAuth(req, res, options)
```

대괄호로 묶인 동적 API 경로의 이름으로 스프레드 연산자를 어떻게 사용했는지 주목하십시오. 그 이유는 뒤에서 `/api/auth/*`(로그인, 콜백, 로그아웃 등)에 대한 모든 요청이 NextAuth.js에 의해 자동으로 전달되기 때문입니다.

`site` 옵션은 NextAuth.js에서 작업할 기본 URL로 사용되므로 모든 리디렉션 및 콜백 URL은 기본 URL로 http://localhost:3000을 사용합니다. 예를 들어 프로덕션에서 이 URL은 웹 사이트의 기본 URL로 대체해야 합니다.

### NextAuth.js 공급자

설명서에 따르면 "제공된 리액트(React)를 사용하면 useSession()의 인스턴스가 후드 아래 리액트 컨텍스트를 사용하여 세션 개체를 여러 구성 요소 간에 공유할 수 있습니다. 이렇게 하면 성능이 향상되고 네트워크 호출이 줄어들며 렌더링 시 페이지 깜박임이 방지됩니다. 적극 권장되며 page/_app.js를 사용하여 Next.js 앱의 모든 페이지에 쉽게 추가할 수 있습니다.

이제 `pages/_app.js`를 열고 다음 조각으로 대체합니다.

```js
// pages/_app.js
import { Provider } from 'next-auth/client'

export default function App({ Component, pageProps }) {
  return (
    <Provider session={pageProps.session}>
      <Component {...pageProps} />
    </Provider>
  )
}
```

이 코드에서, 우리는 NextAuth.js의 `Provider` 컴포넌트로 우리의 애플리케이션을 포장하고 세션 페이지 프로포트를 통과시켰다. 이것은 서버와 클라이언트 측 렌더링을 모두 지원하는 페이지에서 세션을 두 번 확인하지 않기 위한 것입니다.

## 이메일로 로그인

전자 메일 로그인이 작동하려면 사용자에 대한 정보를 저장할 데이터베이스가 필요합니다. 앞서 언급했듯이, 우리는 몽고DB를 선택할 수 있는 데이터베이스로 사용할 것이지만, 다른 어떤 데이터베이스라도 자유롭게 사용해 주세요.

> 참고: NextAuth.js에서는 이메일 로그인으로 작업할 경우 데이터베이스가 필요합니다. 그러나 OAuth의 경우 데이터베이스가 필요하지 않습니다.

### MongoDB 데이터베이스 생성

MongoDB를 사용하기 때문에 다음 조각을 실행하여 데이터베이스를 생성할 수 있습니다.

MongoDB 셸 입력:

```undefined
mongo
```

`next auth`라는 새 데이터베이스를 생성합니다.

```php
use nextauth;
```

### 데이터베이스 자격 증명을 추가하는 중

데이터베이스 연결 문자열은 다음과 같습니다.

```cpp
mongodb://localhost:27017/nextauth.
```

이제 `.env.local` 파일을 열고 다음 조각을 추가하십시오.

```undefined
DATABASE_URL=mongodb://localhost:27017/nextauth
```

데이터베이스가 작동하는 데 필요한 마지막 부분은 데이터베이스 드라이버입니다. 다행히 이제 의존적으로 mongodb만 설치하면 된다.

```undefined
yarn add mongodb
```

다른 데이터베이스를 사용하는 경우 이 링크를 따라 적절한 데이터베이스 드라이버를 설치하십시오.

### 공급자 추가

NextAuth.js는 이메일이나 OAuth와 같은 로그인에 사용할 수 있는 서비스를 정의하는 제공자 개념을 가지고 있다.

시작하려면 `Providers` 모듈을 가져오고 옵션 개체를 `pages/api/auth/[…next auth]의 다음 조각으로 바꾸십시오.js:

```undefined
// pages/api/auth/[…nextauth].js
// ...other imports
import Providers from 'next-auth/providers'

const options = {
  site: process.env.NEXTAUTH_URL,
  providers: [
    Providers.Email({
      server: {
        port: 465,
        host: 'smtp.gmail.com',
        secure: true,
        auth: {
          user: process.env.EMAIL_USERNAME,
          pass: process.env.EMAIL_PASSWORD,
        },
        tls: {
          rejectUnauthorized: false,
        },
      },
      from: process.env.EMAIL_FROM,
    })
  ],
  database: process.env.DATABASE_URL
}
```

이러한 새로운 변화가 무엇인지 살펴보겠습니다.

먼저, 우리는 NextAuth.js가 지원하는 다른 제공자에 대한 액세스를 제공하는 `Providers` 모듈을 수입했다. 다음으로, 우리는 모든 제공자의 목록을 포함하는 배열인 새로운 `공급자` 옵션을 도입했다. 현재로서는 이메일 서버에 대한 몇 가지 옵션과 함께 이메일 제공자를 전달했습니다.

이메일 공급자는 Nodeemailer를 사용하여 위에서 전달된 자격 증명을 기반으로 확인 이메일을 보내는 것과 유사한 연결 문자열 또는 구성 개체를 수락합니다.

전자 메일 공급자 구성에 대한 자세한 내용은 여기를 참조하십시오.

> 참고: 위의 구성에서는 Gmail 계정을 사용했습니다. 을(를) 사용하도록 선택한 경우 보안 수준이 낮은 앱을 사용하도록 설정해야 할 수 있습니다. 그렇지 않은 경우 전자 메일 서버의 자격 증명을 제공합니다.

앞으로 `데이터베이스` 옵션은 이전에 생성된 데이터베이스에 대한 연결 문자열을 가져옵니다.

위의 코드 조각에서 몇 가지 새 변수를 참조했으므로 이 코드 조각을 `.env.local` 파일에 붙여 추가합니다.

```undefined
EMAIL_FROM=YOUR NAME <youremail@example.com>
EMAIL_USERNAME=youremail@example.com
EMAIL_PASSWORD=yourpassword
```

이 때 서버를 재시작하여 이러한 새로운 환경 변수를 적용할 수 있도록 해야 합니다. 일단 그 일이 마무리되면 우리는 `http://localhost:3000/api/auth/signin`에 로그인 화면을 보고로 이동할 필요가 있다.

다음과 같이 보입니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/localhost-authentication-signin-email-e1605713111173.png?resize=730%2C389&ssl=1)

기본적으로 NextAuth.js는 구성 중에 제공된 인증 공급자를 기준으로 로그인 옵션을 표시하는 최소 UI로 구성됩니다.

양식을 제출한 후 성공 페이지로 리디렉션되어 해당 전자 메일로 처음 로그인하는 경우 확인 토큰이 포함된 전자 메일을 수신해야 합니다. 그렇지 않은 경우 로그인 전자 메일 링크가 표시됩니다.

성공 페이지는 다음과 같이 표시됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/authentication-signin-redirected-success-page-e1605713218263.png?resize=730%2C388&ssl=1)

검증 이메일은 다음과 같습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/authentication-verification-signin-email-e1605714616394.png?resize=730%2C380&ssl=1)

로그인 링크를 클릭하면 기본적으로 홈페이지로 이동됩니다.

다음 섹션에서는 현재 사용자 정보를 표시하는 방법을 살펴보겠습니다. 지금 데이터베이스를 살펴보면 로그인한 사용자에 대한 정보가 포함된 새 항목이 표시됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/authentication-database-signedin-user-e1605714736176.png?resize=730%2C279&ssl=1)

## 사용자 세션 처리

이 섹션에서는 세션 데이터를 사용하여 현재 사용자에 대한 정보를 표시합니다. 기본적으로 NextAuth.js는 이메일 로그인에 데이터베이스 세션을 사용하고, OAuth에는 JWT를 사용합니다.

이메일 로그인을 사용할 때 JWT를 사용하도록 설정하려면 해당 옵션을 API 경로에 추가해야 합니다. 이렇게 하려면 이 조각을 `옵션` 개체에 추가하십시오.

```css
 session: {
  jwt: true,
  maxAge: 30 * 24 * 60 * 60 // 30 days
},
```

이 옵션은 NextAuth.js에 사용자 세션을 저장하는 데 JWT를 사용하도록 지시하고 세션이 30일 동안 지속되도록 합니다. 가능한 옵션에 대한 자세한 내용은 설명서를 참조하십시오.

이제 앱에서 세션 데이터를 얻기 위해 클라이언트 측에서 세션 사용 후크를 사용하거나 서버 측에서 세션 가져오기 기능을 사용할 수 있습니다.

그런 다음 `pages/index.js`를 열고 현재 내용을 다음 조각으로 바꿉니다.

```js
// pages/index.js
import { signIn, signOut, useSession } from 'next-auth/client'

export default function Page() {
  const [session, loading] = useSession()

  if (loading) {
    return <p>Loading...</p>
  }

  return (
    <>
      {!session && (
        <>
          Not signed in <br />
          <button onClick={signIn}>Sign in</button>
        </>
      )}
      {session && (
        <>
          Signed in as {session.user.email} <br />
          <button onClick={signOut}>Sign out</button>
        </>
      )}
    </>
  )
}
```

이 절에서는 signIn, signOut, useSession 등 몇 가지 기능을 불러왔다. 첫 번째 두 가지는 추측한 바와 같이, 사용자를 로그인하고 로그인한 사용자를 로그아웃하는 데 사용됩니다.

세션 사용 후크는 세션과 로드 상태를 포함하는 튜플을 반환합니다. 로드 상태를 사용하여 로드 텍스트를 표시하고 사용자의 현재 로그인 여부에 따라 로그인 또는 로그아웃 버튼과 사용자 데이터를 조건부로 렌더링했습니다.

다음은 로그인한 사용자가 있거나 없는 홈 페이지의 미리보기입니다.

## 보호 경로

이제 전자 메일 로그인을 구현했으므로 사용자 세션을 사용하여 원하는 페이지에 대한 액세스를 허용하거나 거부할 수 있습니다. 서버나 클라이언트 쪽에서 이 작업을 수행할 수도 있습니다.

### 서버측 보호

SSR를 사용하여 경로 보호를 이용하려면 대신 getSession 기능을 사용할 수 있습니다. 시작하려면 `페이지` 폴더에 `dashboard.js`라는 새 파일을 만들고 다음 조각을 붙여 넣으십시오.

```js
// pages/dashboard.js
import { getSession } from 'next-auth/client'

export default function Dashboard({ user }) {
  return (
    <div>
      <h1>Dashboard</h1>
      <p>Welcome {user.email}</p>
    </div>
  )
}

export async function getServerSideProps(ctx) {
  const session = await getSession(ctx)
  if (!session) {
    ctx.res.writeHead(302, { Location: '/' })
    ctx.res.end()
    return {}
  }

  return {
    props: {
      user: session.user,
    },
  }
}
```

Next.js에서 페이지를 렌더링하려면 getServerSideProps라는 sync 함수를 내보내야 합니다. 이 함수는 컨텍스트 인수를 수신하고 컨텍스트를 `getSession` 함수에 전달함으로써 세션을 다시 가져옵니다. Next.js의 SSR에 대한 자세한 내용은 설명서를 참조하십시오.

세션이 없는 경우 사용자를 홈 페이지로 다시 리디렉션하기만 하면 `user` prop가 있는 개체를 반환합니다.

마지막으로, `Dashboard` 구성 요소에서 사용자 데이터가 포함된 `user` prop에 액세스했다.

### 클라이언트측 보호

시작하려면 `pages` 폴더에 `profile.js`라는 새 파일을 생성하고 이 조각을 붙여 넣으십시오.

```js
// pages/profile.js
import { useSession } from 'next-auth/client'
import dynamic from 'next/dynamic'

const UnauthenticatedComponent = dynamic(() =>
  import('../components/unauthenticated')
)
const AuthenticatedComponent = dynamic(() =>
  import('../components/authenticated')
)

export default function Profile() {
  const [session, loading] = useSession()

  if (typeof window !== 'undefined' && loading) return <p>Loading...</p>

  if (!session) return <UnauthenticatedComponent />

  return <AuthenticatedComponent user={session.user} />
}
```

이는 pages/index.js 파일과 비슷하지만 이번에는 Authenticated Component와 Unauthenticated Component를 모두 느리게 로드하여 코드 분할을 이용하고 있다.

다음은 이 조각의 기능에 대한 요약입니다.

먼저 useSession 후크를 가져온 다음 Next.js에서 dynamic이라는 특수 함수를 가져옵니다. 이 기능을 통해 모든 구성 요소를 동적으로 가져올 수 있습니다. 앱은 항상 두 가지 상태(인증됨 또는 인증되지 않음) 중 하나이므로 한 번에 하나만 사용할 경우 두 구성 요소를 모두 가져올 필요가 없습니다.

다음으로, 우리는 `session`과 `loading` 상태를 파괴하고 `session`을 사용하여 DOM에 동적으로 렌더링한다.

마지막으로, NextAuth.js가 여전히 로드 중인 동안 로드 텍스트를 렌더링했으며 세션이 있는지 여부에 따라 `Authentified Component` 또는 `Unauthentified Component`가 있다.

이제 `components`라는 새 폴더와 두 개의 파일인 `authenticated.js`와 `unauthenticated.js`를 만듭니다.

authenticated.js에서 다음 조각을 붙여 넣습니다.

```js
// components/authenticated.js
import { signOut } from 'next-auth/client'

export default function Authenticated({ user }) {
  return (
    <div>
      <p>You are authenticated {user.email}</p>
      <button onClick={signOut}>Sign Out</button>
    </div>
  )
}
```

인증되지 않은 .js 파일에서 다음 조각을 붙여 넣습니다.

```js
// components/unauthenticated.js
import { signIn } from 'next-auth/client'

export default function Unauthenticated() {
  return (
    <div>
      <p>You are not authenticated</p>
      <button onClick={signIn}>Sign In</button>
    </div>
  )
}
```

`components/authentication.js`에서는 `signOut` 함수를 가져오고 `pages/profile.js`에서 전달된 `user` proput을 해체했다. 사용자 데이터 및 로그아웃 버튼도 표시하였습니다.

`components/unauthentication.js`에서는 사용자에게 메시지를 표시했습니다. 사용자가 로그인 버튼을 클릭하면 위에서 가져온 signIn 기능이 호출됩니다.

### API 경로 보호

또한 인증 서비스를 Next.js의 API 경로로 확장할 수 있습니다. 서버측 렌더링과 유사하며 인증된 사용자만 API 액세스를 허용하는 데 유용할 수 있습니다.

시작하려면 `page/api`에 `data.js`라는 새 파일을 만들고 이 조각을 붙여 넣으십시오.

```js
// pages/api/data.js
import { getSession } from 'next-auth/client'

export default async (req, res) => {
  const session = await getSession({ req })

  if (session) {
    res.status(200).json({
      message: 'You can access this content because you are signed in.',
    })
  } else {
    res.status(403).json({
      message:
        'You must be sign in to view the protected content on this page.',
    })
  }
}
```

기본 `async` 기능을 내보내고 `page/api` 폴더에 생성되는 파일은 자동으로 API 경로가 됩니다. 이 경우 이 파일은 `http://localhost:3000/api/data`에 매핑됩니다.

이 코드 조각에서 우리는 getSession 함수를 가져오고 API 경로에서 획득한 요청 객체를 전달하였다. 여기에서 NextAuth.js는 쿠키를 읽습니다. 마지막으로 세션의 가용성에 따라 사용자에게 메시지를 반환했습니다.

이를 테스트하기 위해 GET 요청을 `http://localhost:3000/api/data`에 생성하여 응답을 받을 수 있습니다.

## OAuth로 로그인

nextAuth.js에는 기본 제공자가 많습니다. 이메일 공급자와 달리, 우리는 그것들을 사용하기 위해 데이터베이스가 필요하지 않습니다. 이 섹션에서는 Google의 개발자 콘솔에 앱을 만들어 클라이언트 ID와 암호를 얻으려고 합니다.

이러한 플랫폼에서 앱을 만드는 데 익숙한 경우 "페이지 리디렉션 처리" 섹션으로 건너뛸 수 있습니다.

### Google로 로그인

구글로 로그인을 하려면 개발자 콘솔에서 새 프로젝트를 만들고, 구글 계정으로 로그인하고, 모달의 NEW PROJECT 버튼을 클릭하여 새 앱을 만들어야 합니다.

다음과 같은 모양이어야 합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/google-developer-console-new-project-e1605715362311.png?resize=730%2C199&ssl=1)

그런 다음 프로젝트 이름을 지정하고 CREATE 버튼을 클릭해야 합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/google-developer-console-project-name-e1605715426419.png?resize=730%2C317&ssl=1)

그런 다음 Configure Agreement SCREEN 버튼을 클릭합니다. 구성) 버튼을 클릭합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/google-developer-console-consent-screen-e1605715473796.png?resize=730%2C336&ssl=1)

그런 다음 External(외부) 확인란을 선택하고 CREATE(작성) 버튼을 클릭합니다. 화면은 다음과 같습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/google-developer-console-external-checkbox-screen-e1605715514173.png?resize=730%2C372&ssl=1)

다음 단계를 위해서는 앱 이름을 지정해야 합니다. 작업이 완료되면 저장 버튼을 클릭합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/google-developer-console-application-name-e1605715573585.png?resize=730%2C373&ssl=1)

이제 인증 정보 페이지로 돌아가서 CREATE CERTINS 드롭다운 버튼을 클릭하고 OAuth 클라이언트 ID를 선택해야 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/google-developer-console-oauth-client-id-e1605715635540.png?resize=730%2C369&ssl=1)

웹 응용 프로그램을 응용 프로그램 유형으로 선택합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/google-developer-console-oauth-web-application-type-e1605715716572.png?resize=730%2C372&ssl=1)

아래 스크린샷과 같은 양식을 작성하십시오.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/google-developer-console-form-fill-e1605715779525.png?resize=730%2C374&ssl=1)

여기에 앱의 기본 URL과 Google OAuth에 대한 콜백 URL NextAuth.js를 추가했습니다. GitHub와 같은 다른 서비스를 사용한다는 것은 "/콜백/구글"을 "/콜백/github"로 바꿔야 한다는 것을 의미합니다.

CREATE 버튼을 클릭하면 다음과 같이 모달에 자격 증명이 표시됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/google-developer-console-oauth-credentials.png?resize=730%2C371&ssl=1)

지금 우리가 해야 할 일은 이것들을 환경변수로 추가하고 새로운 제공자를 등록하는 것이다.

### Google 자격 증명 추가

`.env.local` lo 파일을 열고 다음 조각을 붙여 넣습니다.

```undefined
GOOGLE_ID=YOUR_GOOGLE_ID
GOOGLE_SECRET=YOUR_GOOGLE_SECRET
```

### 새 공급자 추가

page/api/auth/[next auth]를 엽니다.js 파일 및 이 조각을 `sweet` 배열에 추가합니다.

```css
 Providers.Google({
  clientId: process.env.GOOGLE_ID,
  clientSecret: process.env.GOOGLE_SECRET,
}),
```

새로운 변경 사항을 보려면 앱을 다시 시작해야 합니다. 다시 시작한 후에는 로그인 페이지가 다음과 같이 표시됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/google-developer-console-signin-page.png?resize=730%2C374&ssl=1)

이것으로, 우리는 구글 계정으로 로그인할 수 있을 것입니다. 앞으로 새 공급자를 추가하는 것은 플랫폼에 앱을 만들고 자격 증명을 얻은 다음 공급자 목록에 추가하는 것만큼 쉬워야 합니다.

## 페이지 리디렉션 처리

next.js는 앱에서 콜백을 사용자 지정하는 방법을 제공합니다. 이러한 콜백 중 하나가 리디렉션 콜백입니다. 예를 들어 로그인 및 로그아웃과 같은 사용자가 콜백으로 리디렉션될 때마다 이를 호출합니다.

하지만 왜 우리는 이것에 관심을 가질까요? 그럼, 우리가 로그인하면 홈 페이지로 다시 이동되는 거 알아? 이전에 작성한 `/profile` 페이지로 이동하도록 변경해 보겠습니다.

이 작업을 수행하려면 NextAuth.js의 `callbacks` 옵션에 연결하여 `redirect` 함수만 수정해야 합니다. 이 조각을 `page/api/auth/[…nextauth]의 `options` 개체에 추가하십시오.js:

```js
// ...other options
callbacks: {
    redirect: async (url, _) => {
      if (url === '/api/auth/signin') {
        return Promise.resolve('/profile')
      }
      return Promise.resolve('/api/auth/signin')
    },
},
```

redirect 기능은 url과 baseUrl의 두 가지 매개 변수를 사용합니다. 그러나 이 예에서는 두 번째 파라미터는 상관하지 않습니다. 이 기능은 또한 우리가 약속을 반환할 것을 기대합니다.

이제 현재 로그인 페이지에 있는지 확인합니다. 만일 그렇다면, 우리는 `/profile` 페이지로 이동하려면 그렇지 않으면 우리는 다시 로그인 페이지로 보내 있다.

## 결론

이 튜토리얼에서는 응용 프로그램에서 Next.js 및 NextAuth.js를 사용하여 전자 메일 및 OAuth 인증을 구현하는 방법에 대해 배웠습니다. 이 과정에서 세션 데이터를 사용하여 클라이언트 측과 서버 측의 페이지를 모두 보호합니다. 이 내용을 읽고 나면 새로운 Next.js 앱이나 기존 Next.js 앱에서 인증을 추가할 수 있게 됩니다.

NextAuth.js는 이 튜토리얼에서 다루지 않은 많은 기능을 기본으로 제공합니다. 다음 단계는 더 많은 OAuth 제공자를 추가하고 이 안내서에 따라 로그인 페이지의 설계를 사용자 정의하는 것입니다.