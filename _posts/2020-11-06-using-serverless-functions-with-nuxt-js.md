---
layout: post
title: "Nux.js에서 서버리스 기능 사용"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/using-serverless-functions-with-nuxt-js.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/using-serverless-functions-with-nuxt-js.png?fit=730%2C487&ssl=1)

프런트엔드 개발자들이 웹 애플리케이션을 구축하는 방식에 지속적인 변화가 있었습니다. 오늘날에는 애플리케이션을 구축하고 전송하기 위해 서버를 관리하는 방법에 대해 알 필요가 없습니다. 그것이 서버리스의 전부입니다.

포트폴리오 사이트에 접속 양식을 작성하고 있다고 가정해 보겠습니다. 일반적으로, 방문자가 서비스에 관심이 있을 때 전자 메일을 보내도록 합니다. 원하는 메일 서비스 공급자에 연결하는 API를 작성한 다음 서버를 스핀업하여 API를 실행할 수 있습니다.

이 접근법에 무슨 문제가 있나요? 아니요. 하지만 서버는 24시간 작동하며 항상 요청에 대해 준비됩니다. 포트폴리오 사이트 접속 양식의 경우, 이 접근 방식은 비용 비효율적이고 서버 자원을 낭비합니다. 이와 같은 활용 사례는 서버리스가 실제로 유용하게 활용되는 경우입니다.

## 서버리스란 무엇을 의미합니까?

서버리스(Serverless)는 서버가 없는 것이 아니라 AWS와 같은 타사 서비스를 사용하여 서버를 관리하고 설정하므로 자율 기능만 제공하면 됩니다. 이러한 기능을 서버리스 함수라고 합니다. 이렇게 하면 요청이 있을 때마다 서버리스 "서버"가 회전하고 해당 기간 동안 사용된 리소스에 대해서만 요금이 부과됩니다.

## Nux.js의 서버리스 함수

서버리스 함수의 작동 방식을 보여주기 위해 Nux.js 및 Netlifify Functions로 연락처 양식을 작성하겠습니다. 전체 응용 프로그램의 데모를 여기에서 사용할 수 있습니다.

이 튜토리얼을 따라가려면 다음이 필요합니다.

- Vue.js 및 Nux.js 관련 작업 경험
- Netliify 계정
- 메일건 계정

### Nux.js 프로젝트 설정

먼저 새 Nux.js 응용 프로그램을 만드는 것부터 시작하겠습니다. 앱을 스타일링하기 위해, 우리는 Vue.js 애플리케이션을 빠른 속도로 구축할 수 있는 빌딩 블록을 제공하는 단순한 모듈식 접근 가능한 컴포넌트 라이브러리인 Chakra UI를 사용할 것이다.

```undefined
yarn create nuxt-app serverless-contact-form
```

UI 프레임워크 메시지가 나타나면 Chakra UI를 선택합니다. Nux.js 모듈의 경우 Axios를 선택합니다.

작업이 완료되면 응용 프로그램을 시작합니다.

```bash
cd serverless-contact-form
yarn dev
```

튜토리얼의 나머지 부분에서는 앱이 실행 중인 것으로 가정합니다.

### 연락처 양식 작성

간단명료하게 하기 위해 연락처 양식을 홈 페이지에 직접 배치합니다. 페이지 디렉토리 내에 있는 index.vue 파일을 업데이트해 보겠습니다.

아래와 같이 `템플릿` 섹션을 업데이트하십시오.

```xml
// pages/index.vue

<template>
  <div class="container">
    <CBox
      v-bind="mainStyles[colorMode]"
      d="flex"
      w="100vw"
      h="100vh"
      flex-dir="column"
      justify-content="center"
    >
      <CHeading text-align="center" mb="4">
        Serverless contact form
      </CHeading>

      <CFlex justify="center" direction="column" align="center">
        <CBox mb="3">
          <CIconButton
            mr="3"
            :icon="colorMode === 'light' ? 'moon' : 'sun'"
            :aria-label="`Switch to ${
              colorMode === 'light' ? 'dark' : 'light'
            } mode`"
            @click="toggleColorMode"
          />
        </CBox>

        <CBox text-align="left" width="50%">
          <form @submit.prevent="sendContactToLambdaFunction">
            <CFormControl>
              <CFormLabel for="name">
                Name
              </CFormLabel>
              <CInput id="name" v-model="form.name" type="text" aria-describedby="name" />
            </CFormControl>

            <CFormControl>
              <CFormLabel for="email">
                Email
              </CFormLabel>
              <CInput id="email" v-model="form.email" type="email" aria-describedby="email-helper-text" />
              <CFormHelperText id="email-helper-text">
                We'll never share your email.
              </CFormHelperText>
            </CFormControl>

            <CFormControl>
              <CFormLabel for="message">
                Message
              </CFormLabel>
              <CTextarea v-model="form.message" placeholder="Type your message" />
            </CFormControl>

            <CBox mt="12" d="flex" flex-dir="column" align="center">
              <CButton type="submit" right-icon="arrow-forward" width="20%" variant-color="vue" variant="outline">
                Submit
              </CButton>
            </CBox>
          </form>
        </CBox>
      </CFlex>
    </CBox>
  </div>
</template>
```

연락처 양식에는 이름, 이메일, 메시지 등 세 개의 필드가 있습니다. 양식을 제출하면 메시지 전송을 처리하는 `sendContactToLambdaFunct()가 호출됩니다.

스크립트 섹션도 업데이트하겠습니다.

```xml
// pages/index.vue

<script lang="js">
  import {
    CBox,
    CButton,
    CIconButton,
    CFlex,
    CHeading,
    CTextarea,
    CFormControl,
    CFormLabel,
    CInput,
    CFormHelperText
  } from '@chakra-ui/vue'

export default {
  name: 'App',
  inject: ['$chakraColorMode', '$toggleColorMode'],
  components: {
    CBox,
    CButton,
    CIconButton,
    CFlex,
    CHeading,
    CTextarea,
    CFormControl,
    CFormLabel,
    CInput,
    CFormHelperText
  },
  data () {
    return {
      mainStyles: {
        dark: {
          bg: 'gray.700',
          color: 'whiteAlpha.900'
        },
        light: {
          bg: 'white',
          color: 'gray.900'
        }
      },
      form: {
        name: '',
        email: '',
        message: ''
      }
    }
  },
  computed: {
    colorMode () {
      return this.$chakraColorMode()
    },
    theme () {
      return this.$chakraTheme()
    },
    toggleColorMode () {
      return this.$toggleColorMode
    }
  },
  methods: {
    async sendContactToLambdaFunction () {
      try {
        const response = await this.$axios.$post('/.netlify/functions/contact-mail', {
          name: this.form.name,
          email: this.form.email,
          message: this.form.message
        })

        this.$toast({
          title: 'Mail sent',
          description: response,
          status: 'success',
          duration: 10000,
          isClosable: true
        })

        this.form.name = ''
        this.form.email = ''
        this.form.message = ''
      } catch (error) {
        this.$toast({
          title: 'An error occured',
          description: error,
          status: 'error',
          duration: 10000,
          isClosable: true
        })
      }
    }
  }
}
</script>
```

syncsendContactToLambdaFunction()이 트리거되면 Netliify 함수에 API 요청을 합니다(다음 섹션에서 확인할 수 있습니다).

브라우저에 접속하면 다음과 같은 것을 볼 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/nuxt-serverless-contact-form-1.png?resize=720%2C382&ssl=1)

이 단계에서는 UI와 논리가 준비되었지만 API 끝점이 없습니다. 마지막 섹션에서는 아직 존재하지 않는 `/.netliify/functions/contact-mail`로 전화를 걸게 됩니다.

그러기 전에 Netlifify 기능이 무엇인지 잠시 살펴보겠습니다.

## Netlifify 기능이란?

가장 인기 있는 서버리스 아키텍처(FaaS) 제공자는 AWS Lambda입니다. 하지만 AWS는 소규모 앱에는 적합하지 않습니다. Netliify를 사용하면 AWS 계정 없이 서버리스 람다 기능을 배포할 수 있으며 Netliify 내에서 직접 관리가 처리됩니다.

Netlifify는 또한 Netlifify Dev를 사용하여 로컬에서 서버 없는 기능을 테스트할 수 있는 방법을 제공합니다. 우리가 그것을 설치했는지 확인해 보자.

```undefined
// NPM
npm install netlify-cli -g

// Yarn
yarn add global netlify-cli
```

이제 프로젝트의 루트 디렉터리에 `netliify.toml` 파일을 생성하겠습니다. 이 구성 파일은 Netlifify에게 프로젝트를 빌드하고 배포하는 방법을 알려줍니다.

또한 지역 개발 환경에도 필요합니다.

```bash
// netlify.toml

[dev]
   command = "yarn dev"
[build]
  command = "yarn generate"
  publish = "dist"
  functions = 'functions'  # directs netlify to where your functions directory is located

[[headers]]
  # Define which paths this specific [[headers]] block will cover.
  for = "/*"
    [headers.values]
    Access-Control-Allow-Origin = "*"
```

아래 명령을 실행하여 Netlifify 기능을 만듭니다.

```bash
netlify functions:create contact-mail
```

그런 다음 비동기/대기 및 응답 포맷 옵션을 보여주는 [hello-world] 기본 기능을 선택합니다. 이 명령은 프로젝트의 루트 디렉터리에 `functions` 디렉토리와 `contact-mail.js` 파일을 생성합니다.

이 기능을 로컬로 테스트하려면 다음을 실행하십시오.

```undefined
netlify dev
```

그러면 개발 서버가 시작됩니다.

그런 다음:

```undefined
netlify functions:invoke contact-mail --no-identity
```

이와 같은 것을 얻어야 합니다.

```undefined
{"message":"Hello World"}
```

## 메일건 설정

전자 메일 전송을 처리하기 위해 테스트용 샌드박스 도메인을 제공하는 Mailgun을 사용합니다.

계정 대시보드에서 샌드박스 도메인과 API 키를 가져옵니다. Sending > Domains(도메인 전송) 아래에 샌드박스 도메인이 표시됩니다. API를 선택하고 API 키, API BASE URL, 샌드박스 도메인을 복사합니다.

그런 다음 필요한 종속성을 설치합니다.

```undefined
yarn add dotenv mailgun-js
```

우리는 `dotenv`를 사용하여 우리의 지역 환경 변수에 접근할 것이다. 앱이 배포되면 Netliify가 프로덕션에서 적절한 세부 정보를 사용하도록 구성합니다. mailgun-js는 Mailgun API와 상호 작용하기 위한 Node.js 모듈이다.

루트 디렉터리에 `.env` 파일을 생성하고 올바른 값을 연결하십시오.

```undefined
// .env

MG_API_KEY=YOUR_API_KEY
MG_DOMAIN=YOUR_SANDBOX_DOMAIN
MG_HOST=YOUR_API_BASE_URL
TO_EMAIL_ADDRESS=ADDRESS_EMAIL_WILL_BE_SENT_TO
```

이제 실제 함수 논리로 넘어갑시다. 다음 조각을 `contact-mail.js` 안에 붙여 넣습니다.

```js
// functions/contact-mail.js

require('dotenv').config()

const { MG_API_KEY, MG_DOMAIN, MG_HOST, TO_EMAIL_ADDRESS } = process.env
const mailgun = require('mailgun-js')({ apiKey: MG_API_KEY, domain: MG_DOMAIN, url: MG_HOST})

exports.handler = async (event) => {
  if (event.httpMethod !== 'POST') {
    return {
      statusCode: 405,
      body: 'Method Not Allowed',
      headers: { 'Allow': 'POST' }
    }
  }

  const data = JSON.parse(event.body)

  if (!data.message || !data.name || !data.email) {
    return { statusCode: 422, body: 'Name, email, and message are required.' }
  }

  const mailgunData = {
    from: data.email,
    to: TO_EMAIL_ADDRESS,
    'h:Reply-To': data.email,
    subject: `New mail from ${data.name}`,
    html: `
    <h4> Email from ${data.name} ${data.email} </h4>
    <p> ${data.message}</p>
    `
  }

  try {
    await mailgun.messages().send(mailgunData)

    return {
      statusCode: 200,
      body: 'Your message was sent successfully!'
    }
  } catch (error) {
      return {
        statusCode: 422,
        body: `Error: ${error}`
      }
  }
}
```

이 기능의 움직이는 부분을 살펴봅시다. 첫째, require(`dotenv`).config()는 `process.env`를 통해 환경 변수에 액세스할 수 있는 기능을 제공합니다. 다음으로, 필요한 매개 변수로 Mailgun 모듈을 인스턴스화합니다.

그런 다음 수신 요청에 대한 유효성 검사를 수행합니다. 먼저, 수신 요청은 `POST` 요청이어야 합니다. 필드가 비어 있는 경우 서버는 상태 코드 `422`(처리 불가능한 엔티티)를 반환해야 합니다.

다음으로, 우리는 이메일 본문을 구성합니다. 메일군은 수신인, 발신인, 제목, HTML 및 텍스트 부분, 첨부 파일 등을 수신합니다.

마지막으로, 객체를 `mailgun.messages.send()`에 전달하고 메일건에서 오류 또는 성공 메시지를 반환합니다.

이제 다음을 실행할 수 있습니다.

```undefined
netlify dev
```

이렇게 하면 함수 및 Nux.js 응용 프로그램의 개발 서버가 회전합니다. 프런트 엔드와 API 끝점이 함께 작동하는 방식에 대해 걱정할 필요가 없습니다. Netliify는 이러한 세부 정보를 대신 처리합니다.

이 함수는 포트 `8888`에서 실행되는 반면 Nux.js 앱은 포트 `3000`에서 실행됩니다. Netlifify Dev는 이를 병합하여 브라우저의 포트 `8888`에서 실행합니다.

## 테스트

이제 우리가 무엇을 만들어 왔는지 시험해 볼 시간이다. 당신은 포스트맨이나 다른 HTTP 클라이언트를 사용할 수 있지만, 우리는 Netliify Dev를 사용할 것입니다. 왜냐하면 그것은 우리가 `POST` 요청을 수행할 수 있게 해주기 때문입니다.

다음 명령을 실행합니다.

```undefined
netlify functions:invoke contact-mail --no-identity --payload '{"email": "chioma@example.com", "name": "Chioma", "message": "hello this function works fine!"}'
```

터미널과 우편함에서 모두 아래와 같은 응답을 받아야 합니다.

```undefined
Your message was sent successfully! We'll be in touch.
```

물론, 브라우저에 있는 앱으로 직접 테스트할 수 있습니다.

## Netlifify로 배포

마지막으로 정적 앱을 배포하겠습니다. 아래 명령을 실행하여 `dist` 디렉토리를 생성합니다.

```undefined
yarn generate
```

다음 단계는 Netliify에 새로운 사이트를 만드는 것입니다.

```undefined
netlify init
```

Netlifify의 사이트 대시보드로 이동하여 기능 탭을 클릭하면 `contact-mail.js` 기능이 표시됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/nuxt-contact-mail-function-1.png?resize=635%2C148&ssl=1)

마무리하기 전에 Netliify에 환경 변수를 추가해 보겠습니다. 설정 > 빌드로 이동

마지막으로 앱을 다시 배포하여 변경 내용을 적용합니다.

## 결론

이 튜토리얼에서는 Nux.js로 정적 앱을 구축하고 Netlifify Functions로 API 끝점을 설정하여 서버리스(서버리스)를 탐색했다. 기존 서버와는 달리 이 접근 방식의 이점에 대해 논의했습니다. 마지막으로 Netliify Dev를 사용하여 서버 없는 애플리케이션을 쉽고 빠르게 구축하고 배포하는 방법에 대해 살펴봤습니다.

자세한 내용은 Netlifify Functions 문서를 참조하십시오.

이 프로젝트의 소스 코드는 GitHub에서 사용할 수 있습니다.