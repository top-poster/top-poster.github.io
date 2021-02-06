---
layout: post
title: "Vue.js 및 Node.js를 사용하여 처음부터 JWT 인증"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/jwt-from-scratch-vue-js-node-js.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/jwt-from-scratch-vue-js-node-js.png?fit=730%2C487&ssl=1)

## 도입

JSON Web Token의 약자인 JWT는 개발자가 서명을 통해 클레임이라는 정보의 진위를 확인할 수 있는 개방형 표준이다. 이 서명은 암호 또는 공용/개인 키 쌍일 수 있습니다. 헤더와 페이로드와 함께, 그것들은 나중에 우리가 볼 수 있듯이 JWT를 생성하거나 구성하는 데, 사용될 수 있다.

JWT는 일반적으로 인증 또는 서로 다른 당사자에게 정보를 안전하게 전송하는 데 사용됩니다. 다음은 JWT 기반 인증 시스템에 대한 일반적인 흐름입니다. 사용자가 앱에 로그인하면 JWT가 서버에 생성되어 호출 클라이언트로 돌아갑니다.

이후 각 요청에는 JWT가 인증 헤더로 포함되므로 보호된 경로 및 리소스에 액세스할 수 있습니다. 또한 백엔드 서버는 서명이 유효한지 확인한 후 필요에 따라 토큰에서 사용자 데이터를 추출합니다. JWT가 유효한지 확인하기 위해 키나 암호를 보유한 당사자만이 정보에 서명할 책임이 있습니다.

이 게시물에서는 JWT를 사용하여 Node.js 백엔드가 있는 Vue.js 클라이언트 앱에서 인증 요청을 수행하는 데 초점을 맞출 것이다. 먼저 JWT의 작동 방식을 간략하게 살펴보겠습니다.

## JWT 인증 작동 방식

JWT 인증 기반 시스템에서는 사용자가 자격 증명을 사용하여 로그인하면 JSON 웹 토큰이 호출 클라이언트에 다시 반환됩니다. 사용자가 보호된 경로나 리소스에 액세스하려고 할 때마다 사용자 에이전트는 동일한 JWT를 전송합니다(일반적으로 `Authorization` 헤더에서 `Bearer` 스키마를 사용합니다).

헤더의 내용은 다음과 같아야 합니다.

```undefined
Authorization: Bearer <token>
```

사용자가 보호된 리소스에 대한 액세스 권한을 부여받으려면 서버 경로가 `인증` 헤더에 유효한 JWT가 있는지 확인해야 합니다. 보너스로 승인 헤더에 JWT를 보내면 CORS와 관련된 문제도 해결된다. 이는 앱이 완전히 다른 도메인에서 제공되는 경우에도 적용됩니다.

> 참고: JWT에 서명하더라도 데이터가 암호화되지 않기 때문에 사용자나 다른 당사자에게 정보가 노출됩니다. 따라서 사용자는 JWT 페이로드 내에 자격 증명과 같은 중요한 정보를 포함하지 않는 것이 좋습니다. 또한 토큰은 항상 만료되어야 합니다.

## Node.js 응용 프로그램을 부트스트래핑하는 중

앞에서 말했듯이, 우리는 백엔드 Node.js 서버에 대한 인증 수단으로 JWT와 함께 Vue.js 애플리케이션을 설정하여 JWT의 작동 방식을 탐색할 것이다. 먼저 애플리케이션의 백엔드 부분을 구축하여 JWT 생성과 후속 확인을 모두 처리할 것입니다. 이 튜토리얼의 전체 코드는 이 GitHubrepo에서 찾을 수 있다.

우선 터미널을 열고 앱에 필요한 모든 종속성을 설치하자. 우리는 npm에 json webtoken 패키지를 사용할 것이고, 서버에는 express를 사용할 것이다. 또한 ES2015+ 전체 환경에 필요한 폴리필을 제공하는 `babel-polyfill`을 설치할 예정입니다.

응용 프로그램의 종속성 전체 목록을 보려면 `패키지`를 확인하십시오.json의 파일 해당 파일의 전체 내용은 다음과 같습니다.

```undefined
{
  "name": "jwt-vue-node-backend",
  "version": "1.0.0",
  "description": "Backend server for the JWT-Vue-Node-App",
  "main": "server.js",
  "scripts": {
    "create-dev-tables": "babel-node ./app/db/dbConnection createUserTable",
    "drop-dev-tables": "babel-node ./app/db/dbConnection dropUserTable",
    "dev": "nodemon --watch . --exec babel-node -- server/server",
    "start-dev": "babel-node server/server.js",
    "build-server": "babel server -d dist",
    "build": "rm -rf dist && mkdir dist && npm run build-server",
    "start-prod": "node dist/server.js",
    "start": "npm-run-all -p start-prod",
    "revamp": "npm-run-all -p dev drop-dev-tables"
  },
  "esModuleInterop": true,
  "keywords": [
    "Node",
    "Postgres"
  ],
  "author": "Alexander Nnakwue",
  "license": "MIT",
  "devDependencies": {
    "@babel/cli": "^7.12.1",
    "@babel/core": "^7.12.3",
    "@babel/node": "^7.12.10",
    "@babel/preset-env": "^7.12.1",
    "@babel/register": "^7.12.10",
    "babel-preset-es2015": "^6.24.1",
    "babel-watch": "^7.0.0",
    "eslint": "^7.11.0",
    "eslint-config-airbnb-base": "^14.2.0",
    "eslint-plugin-import": "^2.22.1",
    "nodemon": "^2.0.5"
  },
  "dependencies": {
    "@hapi/joi": "^17.1.1",
    "babel-plugin-add-module-exports": "^1.0.4",
    "babel-polyfill": "^6.26.0",
    "bcryptjs": "^2.4.3",
    "body-parser": "^1.19.0",
    "cors": "^2.8.5",
    "dotenv": "^8.2.0",
    "express": "^4.17.1",
    "jsonwebtoken": "^8.5.1",
    "make-runnable": "^1.3.8",
    "moment": "^2.29.1",
    "npm-run-all": "^4.1.5",
    "pg": "^8.4.1"
  },
  "engines": {
    "node": "13.7.0",
    "npm": "6.13.6"
  }
}
```

위의 파일에서 볼 수 있듯이, 우리는 여러 개의 패키지를 설치했습니다. 기본적으로, 우리는 Babel이 개발 의존성으로 작동하도록 하기 위해 필요한 패키지를 설치했습니다. 먼저 Babel CLI가 전세계에 설치되어 있는지 확인하는 것으로 시작했습니다. 우리는 또한 위와 같이 애플리케이션에 필요한 다른 의존성인 에스린트, 노데몬, pg 등을 설치할 필요가 있다.

패키지의 스크립트 부분에 각별히 주의하십시오.사용자 테이블을 만들고, 사용자 테이블을 삭제하고, 앱의 서버 파일을 만들고, 개발 환경에서 앱을 실행하는 방법을 보여주는 json` 파일입니다. 이 섹션에서는 프로덕션에서 앱을 실행하는 방법도 보여줍니다. 기타 자세한 내용은 앞에서 언급한 파일에서 확인할 수 있습니다.

마지막으로 폴더 구조는 다음과 같습니다.

보시다시피 앱은 모노레포이므로 쉽게 클라이언트 폴더를 추가하고 빌드를 빌드하고 `dist` 디렉토리에 출력할 수 있습니다. 마지막으로 잠시 후에 볼 수 있듯이 빌드 경로를 `server.js` 파일에 추가합니다.

위의 폴더 구조에서 `app` 폴더는 백엔드를 성공적으로 실행하는 데 필요한 모든 추가 구성 요소를 포함합니다. 여기에는 컨트롤러, 데이터베이스 핸들러, 도우미, 미들웨어 폴더 등이 포함됩니다. 다시 한 번 폴더 구조의 전체 보기와 앱의 보고서를 체크아웃하거나 복제하려면 GitHub의 소스 코드를 확인하십시오.

앞서 언급했듯이, `클라이언트` 폴더에는 앱의 사용자 대면 부분과 관련된 모든 것이 포함되어 있습니다. `server` 폴더에는 응용 프로그램을 실행하는 기본 Express 서버인 `server.js` 파일이 포함되어 있습니다. 어떻게 생겼는지 봅시다.

```coffeescript
import express from 'express'
import 'babel-polyfill'
import cors from 'cors'
import usersRoute from '../app/route/user.js'
import path from 'path'

const __dirname = path.resolve();
const app = express()

// Add  cors middleware
app.use(cors())

// Add middleware for parsing JSON and urlencoded data
app.use(express.urlencoded({ extended: false }))
app.use(express.json())

// Serve static files from the Vue app
app.use(express.static(path.join(`${__dirname}/client/dist`)))

// The "catchall" handler: for any request that doesn't
// match one above, send back Vue's index.html file.
app.get('/*', (req, res) => {
    res.sendFile(path.join(`${__dirname}/client/dist/index.html`))
})

app.use('/api/v1', usersRoute)

app.get('/', (req, res) => {
    res.status(200).send('Welcome to the JWT authentication with Node and Vue.js tutorial')
})

// catch 404 and forward to error handler
app.use((req, res, next) => {
    const err = new Error('Not Found')
    console.log(err)
    err.status = 404
    res.send('Route not found')
    next(err)
})

const server = app.listen(process.env.PORT || 3000).on('listening', () => {
    console.log(`App live and listening on port: ${process.env.PORT}` || 3000)
})

export default app
```

위의 서버 파일에서 무슨 일이 일어나고 있는지 설명하기 전에, 우리는 우리의 애플리케이션을 위한 Babel 구성을 설정해야 합니다. .babelrc 파일은 다음과 같습니다.

```undefined
{
    "presets": ["@babel/preset-env"],
    "plugins": ["add-module-exports"]
}
```

위에서 사용하는 사전 설정을 사용하면 최신 JavaScript 구문을 작성할 수 있으며 OS 대상 환경에 필요한 구문 변환(선택 사항인 경우 브라우저 폴리필)을 확인할 필요가 없습니다. 이 플러그인을 통해 Babel은 우리의 `module.exports` 문과 변환을 이해하고 해석할 수 있다.

이제 위의 `server.js` 파일로 빠르게 돌아가 보겠습니다. 보시는 바와 같이, 클라이언트 빌드 경로에서 정적 파일과 자산을 제공하고 있습니다. 우리는 또한 `노선` 경로에서 우리의 노선을 수입하고 있다. 그리고 마지막으로 위에 설명한 바벨 폴리필 패키지를 수입하고 있습니다. `eslint` 및 기타 설정에 대한 보고서를 확인하십시오.

모든 것이 어떻게 잘 어울리는지 알아보기 전에, `.env` 파일에서 변수를 선택하는 `config` 파일을 빨리 설정해 보자. 파일의 내용은 다음과 같습니다.

```java
require('dotenv').config()
const { env } = process

module.exports = {
    name: env.APP_NAME,
    baseUrl: env.APP_BASE_URL,
    port: env.APP_PORT,
    databases: {
        postgres: {
            user: env.PG_USERNAME,
            password: env.PG_PASSWORD,
            host: env.PG_HOST,
            port: env.PG_PORT,
            db_name: env.PG_DATABASE,
            url: env.PG_URL,
        },
    },
}
```

보다시피, Postgres는 오늘날 우리가 선택한 데이터베이스입니다. 즉, 라이브 Postgres DB에 연결하는 데 필요한 세부 정보를 제공해야 합니다. 이제 `앱` 폴더 아래에 있는 코드베이스의 가장 중요한 부분을 살펴보자. 기본적으로 여기에 있는 모든 파일은 서버가 HTTP 요청에 응답하도록 하기 위해 필요합니다.

먼저 `routs` 폴더의 `user.js` 파일부터 살펴보겠습니다. 파일 내용은 다음과 같습니다.

```js
const express = require('express')
const { registerUser, loginUser, me } = require('../controller/users.js')
const  {
 createUser,
} = require( '../helpers/validation.js')
const {
  decodeHeader,
 } = require( '../middleware/verifyAuth.js')

const router = express.Router()

// user Routes

router.post('/auth/signup', createUser, registerUser)
router.post('/auth/login', loginUser)
router.get('/me', decodeHeader, me)

module.exports = router
```

위의 경로 정의 파일에서 볼 수 있듯이, JWT가 Vue 및 Node와 어떻게 작동하는지 시연하기 위해 세 개의 끝점만 필요합니다. 여기에서 컨트롤러 파일과 미들웨어 파일을 가져오는 것을 알 수 있습니다(`.../ 미들웨어/확인Auth.js)는 클라이언트에서 보낸 `Authorization` 헤더를 디코딩하는 데 사용됩니다. 잠시 후에 살펴보도록 하겠습니다.

지금은 `~/controller/users.js` 경로를 통해 `user.js` 파일에 있는 컨트롤러를 살펴보겠습니다. 예를 들어 `login.js` 방법을 살펴보겠습니다.

```js
/**
   * login User
   * @param {object} req
   * @param {object} res
   * @returns {object} user object
   */

const loginUser = async (req, res) => {
    const { email, password } = req.body

    if (isEmpty(email) || isEmpty(password)) {
        return Response.sendErrorResponse({
            res,
            message: 'Email or Password detail is missing',
            statusCode: 400,
        })
    }

    if (!isValidEmail(email) || !validatePassword(password)) {
        return Response.sendErrorResponse({
            res,
            message: 'Please enter a valid Email or Password',
            statusCode: 400,
        })
    }

    const loginUserQuery = 'SELECT * FROM users WHERE email = $1'

    try {

        const { rows } = await dbQuery.query(loginUserQuery, [email])
        const dbResponse = rows[0]

        if (!dbResponse) {
            return Response.sendErrorResponse({
                res,
                message: 'User with this email does not exist',
                statusCode: 400,
            })
        }

        if (!comparePassword(dbResponse.password, password)) {
            return Response.sendErrorResponse({
                res,
                message: 'The password you provided is incorrect',
                statusCode: 400,
            })
        }

        const token = Utils.generateJWT(dbResponse)
        const refreshExpiry = moment().utc().add(3, 'days').endOf('day')
            .format('X')
        const refreshtoken = Utils.generateJWT({ exp: parseInt(refreshExpiry), data: dbResponse._id })
        delete dbResponse.password
        return Response.sendResponse({
            res,
            responseBody: { user: dbResponse, token, refresh: refreshtoken },
            message: 'login successful',
        })
    } catch (error) {
        console.log(error)
        return Response.sendErrorResponse({
            res,
            message: error,
            statusCode: 500,
        })
    }
}
```

일반적인 암호 비교를 마친 후에는 `pg` 데이터베이스를 쿼리하여 반환된 사용자의 세부 정보가 포함된 JWT를 생성합니다. 다음으로, 우리는 (물론 `새로 고침`E` 만료와 함께) 새로 고침 토큰을 생성합니다. 마지막으로 응답의 일부로 토큰과 새로 고침 토큰을 반환합니다. 저희가 관심 있는 부분은 다음과 같습니다.

```undefined
const token = Utils.generateJWT(dbResponse)
const refreshExpiry = moment().utc().add(3, 'days').endOf('day')
            .format('X')
const refreshtoken = Utils.generateJWT({ exp: parseInt(refreshExpiry), data: dbResponse._id })
```

> 참고: 이미 Heroku Postgres 추가 기능으로 Postgres 데이터베이스를 설정했습니다. 자세한 내용은 Heroku 환경에 앱을 배포할 예정이므로 나중에 다시 논의하겠습니다.

이제 `generate J`가 있는 `Utils` 폴더를 살펴보겠습니다.WT 방법이 정의되어 있습니다. 우리는 라인 10의 컨트롤러 파일 상단에 있는 Utils 모듈을 가져옵니다. helpers/utils.js 경로로 이동하여 토큰을 확인하고 서명하는 방법을 직접 학습한다. 해당 파일의 내용을 살펴보겠습니다.

```js
const bcrypt = require('bcryptjs')
const jwt = require('jsonwebtoken')
const fs = require('fs')

const privateKEY = fs.readFileSync('./private.key', 'utf8')
const publicKEY = fs.readFileSync('./public.key', 'utf8')

const i = 'jwt-node'
const s = 'jwt-node'
const a = 'jwt-node'

const verifyOptions = {
    issuer: i,
    subject: s,
    audience: a,
    expiresIn: '8784h',
    algorithm: ['RS256'],
}

const saltRounds = 10

const salt = bcrypt.genSaltSync(saltRounds)

    const generateJWT = (payload) => {
        const signOptions = {
            issuer: i,
            subject: s,
            audience: a,
            expiresIn: '8784h',
            algorithm: 'RS256',
        }

        const options = signOptions
        if (payload && payload.exp) {
            delete options.expiresIn
        }
        return jwt.sign(payload, privateKEY, options)
    }

    const verifyJWT = (payload) => {
        return jwt.verify(payload, publicKEY, verifyOptions)
    }

    const hashPassword = (password) => {
        const hash = bcrypt.hashSync(password, salt)
        return hash
    }

module.exports = {
    hashPassword, verifyJWT, generateJWT
}
```

> 참고: 'generate J'WT 메서드는 privateK를 사용하여 지정된 페이로드를 JSON Web Token 문자열에 동기적으로 서명합니다.EY와 옵션 논증.

위의 코드 조각에서 알 수 있듯이, 우리는 관심 있는 두 가지 주요 방법을 가지고 있다: verifyJ.WT 및 `generate J`WT. 헤더, 페이로드, 점으로 구분된 서명/암호화 데이터 등 세 가지 요소로 구성된 JWT 생성 방식을 따르는 후자의 방법을 알아보자.

## JWT 구조

방금 언급한 것처럼 JWT는 헤더, 페이로드 및 서명을 포함한 세 가지 다른 요소로 구성된다.

헤더는 토큰 유형을 포함한 자신에 대한 클레임을 포함하는 JSON 개체입니다. 또한 HMAC, RSA와 함께 SHA-256 또는 ES256과 같은 서명 알고리즘으로 구성된다. 여기서는 RS256 알고리즘을 사용하고 있습니다. 또한 JWT의 서명 또는 암호화 여부도 정의합니다.

```undefined
const signOptions = {
            issuer: i,
            subject: s,
            audience: a,
            expiresIn: '8784h',
            algorithm: 'RS256',
            "typ": "JWT"
        }
```

토큰의 두 번째 부분은 페이로드이며, 일반적으로 사용자에 대한 세부 정보를 포함합니다. 또한 해당 사용자에 대한 진술인 일부 클레임을 포함할 수 있습니다.

> 참고: 세 가지 유형의 클레임이 있습니다. 여기에는 등록, 공개 및 비공개 클레임이 포함됩니다. 이 중 일부는 'iss'(발행자), 'exp'(만료시간), 'sub'(주제자), 'aud'(청중자) 등이다.

샘플 페이로드의 구조는 다음과 같습니다.

```css
{
  id: 2,
  email: 'alex.nnakwue@gmail.com',
  firstname: 'Kelechi',
  lastname: 'Oti',
  password: '$2a$10$yK1bInjD2KieruxwFE9UV.ZuYiwZHu8ciX2vMZmLSvbrOdZ4/Maju',
  createdon: 2020-10-31T23:00:00.000Z
}
```

> 참고: 서명된 토큰의 경우 변조로부터 보호되지만 이 정보는 누구나 읽을 수 있습니다. 따라서 암호화되지 않은 경우 JWT의 페이로드 또는 헤더 요소에 비밀 정보를 넣지 마십시오.

세 번째 부분은 시그니처인데, 도중에 메시지가 변경되지 않았는지 확인하는 데 사용되며, 개인 키로 서명한 토큰의 경우 JWT의 발신인이 누구인지도 확인할 수 있다.

서명은 서명 또는 암호화에 사용되는 알고리즘에 따라 크게 달라지며, 암호화되지 않은 JWT의 경우에는 없습니다. 기본적으로 암호화되지 않은 JWT의 경우 헤더가 아래에 표시됩니다.

```undefined
{
    "alg": "none"
}
```

여기서는 JWT가 암호화되고 보고서의 루트 경로에서 사용할 수 있는 개인/공용 키 쌍에 의해 서명된다.

아래 JWT 샘플을 봅시다. JWT의 세 요소(각각 헤더, 페이로드 및 시그니처)를 구분하는 점(기본 64url 인코딩)에 주목하십시오.

```css
eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MiwiZW1haWwiOiJhbGV4Lm5uYWt3dWVAZ21haWwuY29tIiwiZmlyc3RuYW1lIjoiS2VsZWNoaSIsImxhc3RuYW1lIjoiT3RpIiwicGFzc3dvcmQiOiIkMmEkMTAkeUsxYkluakQyS2llcnV4d0ZFOVVWLlp1WWl3Wkh1OGNpWDJ2TVptTFN2YnJPZFo0L01hanUiLCJjcmVhdGVkb24iOiIyMDIwLTExLTAxVDAwOjAwOjAwLjAwMFoiLCJpYXQiOjE2MDgyNjY5MDYsImV4cCI6MTYzOTg4OTMwNiwiYXVkIjoicXVlc3Rpb25zLWdhbWUiLCJpc3MiOiJxdWVzdGlvbnMtZ2FtZSIsInN1YiI6InF1ZXN0aW9ucy1nYW1lIn0.FLXTabVVpZ_zuPEspoQe8U5kr8CwKNFKgXBwgQN6yKkW2RXmQGXyuML3dLy4XOEVEgaNoAjSI8De65CKlAq_dA
```

> 참고: JWT.io은 JWT에 대해 자세히 알아보기 위한 대화형 놀이터입니다. 예를 들어, 위에서 토큰을 복사하여 편집할 때 어떻게 되는지 확인할 수 있습니다.

다음은 `검증 J`를 살펴보겠습니다.기본적으로 공개키(public KEY)를 활용해 비공개 K를 검증할 수 있는 WT 방식.EY는 헤더와 페이로드의 서명을 담당했다.

verifyJ의 일부WT 방법은 다음과 같습니다.

```coffeescript
const verifyJWT = (payload) => {
        return jwt.verify(payload, publicKEY, verifyOptions)
}
```

> 참고: JWT는 암호 또는 공개 키를 사용하여 지정된 토큰을 동기적으로 확인하는 '확인' 방법과 확인을 위한 '옵션'을 가지고 있습니다. 토큰이 확인되면 해당 토큰의 디코딩된 값이 반환됩니다.

이제 앱에 로그인하면 아래와 같이 `user` 개체, 토큰 및 새로 고침 토큰을 볼 수 있습니다.

```undefined
{
    "data": {
        "user": {
            "id": 2,
            "email": "alex.nnakwue@gmail.com",
            "firstname": "Kelechi",
            "lastname": "Oti",
            "createdon": "2020-10-31T23:00:00.000Z"
        },
        "token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MiwiZW1haWwiOiJhbGV4Lm5uYWt3dWVAZ21haWwuY29tIiwiZmlyc3RuYW1lIjoiS2VsZWNoaSIsImxhc3RuYW1lIjoiT3RpIiwicGFzc3dvcmQiOiIkMmEkMTAkeUsxYkluakQyS2llcnV4d0ZFOVVWLlp1WWl3Wkh1OGNpWDJ2TVptTFN2YnJPZFo0L01hanUiLCJjcmVhdGVkb24iOiIyMDIwLTEwLTMxVDIzOjAwOjAwLjAwMFoiLCJpYXQiOjE2MDgyNzMxNTgsImV4cCI6MTYzOTg5NTU1OCwiYXVkIjoiand0LW5vZGUiLCJpc3MiOiJqd3Qtbm9kZSIsInN1YiI6Imp3dC1ub2RlIn0.ky4EYUP3t10qaH1aCc0g_jxX5zt8U03l7lrwfpVjViYGqufxRcLoYNV8rw8SygxouXOqSPiF_LxLGv3fi4MQyQ",
        "refresh": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE2MDg1OTUxOTksImlhdCI6MTYwODI3MzE1OCwiYXVkIjoiand0LW5vZGUiLCJpc3MiOiJqd3Qtbm9kZSIsInN1YiI6Imp3dC1ub2RlIn0.VvWQ17Wzyw-tJdRiIxsVzXNhl-nQSPf6Wo1iFOtC3BeueywB1lLaqOK4rmO0UAxKovx6sjZufu2mXEubKsiLOg"
    },
    "status": true,
    "message": "login successful"
}
```

### 토큰 및 새로 고침 토큰

토큰은 사용자에게 보호된 리소스에 대한 액세스 권한을 부여합니다. 일반적으로 수명이 짧으며 만료 날짜가 헤더에 첨부되어 있을 수 있습니다. 또한 사용자에 대한 추가 정보도 포함할 수 있습니다.

반면에 토큰 새로 고침은 사용자가 새 토큰을 요청할 수 있도록 허용합니다. 예를 들어 토큰이 만료된 후 클라이언트는 백엔드 서버에서 생성할 새 토큰에 대한 요청을 수행할 수 있습니다. 이렇게 하려면 새로 고침 토큰이 필요합니다. 액세스 토큰과 달리 새로 고침 토큰은 일반적으로 수명이 오래 걸립니다.

이제 `검증`을 살펴보겠습니다.auth` 파일은 미들웨어 폴더에 있습니다. 이 파일에는 클라이언트에서 보낸 `Authorization`을 디코딩하고 토큰이 유효한지 확인하는 미들웨어 메서드가 포함되어 있습니다. 그 파일에서 우리는 `decode`에 관심이 있다.헤더` 방법(아래 참조):

```js
const decodeHeader = (req, res, next) => {
    let token = req.headers['x-access-token'] || req.headers.authorization || req.body.token
    if (!token) {
        return Response.sendErrorResponse({ res, message: 'No token provided', statusCode: 401 })
    }
    if (token.startsWith('Bearer ')) {
        // Remove Bearer from string
        token = token.slice(7, token.length)
        if (!token || token === '') Response.sendErrorResponse({ res, message: 'No token provided', statusCode: 401 })
    }
    // call the verifyJWT method to verify the token is valid
    const decoded = Utils.verifyJWT(token)
    if (!decoded) Response.sendErrorResponse({ res, message: 'invalid signature', statusCode: 403 })
    // attach the decoded token to the res.user object
    if (decoded) res.user = decoded
    res.token = token
    return next()
}
```

디코드로부터 알 수 있듯이위의 헤더 방법에서는 인증 헤더의 형태나 `req.body`의 형태로 클라이언트로부터 토큰을 받고 있습니다. 토큰이 제공되지 않으면 오류를 반환하는 것입니다. 또한 토큰이 베어러 스키마와 함께 제공되는지 확인하고 있으며, 함께 제공되는 토큰이 있다면 verifyJ라고 부른다.Utils 모듈의 WT 메서드입니다.

이 방법은 앞에서 설명한 대로 공용 키를 사용하여 클라이언트에서 보낸 토큰을 확인하고 토큰의 디코딩된 값을 반환합니다. 디코딩된 값이 반환되지 않으면 처음에 서명된 토큰이 유효하지 않음을 의미합니다. 마지막으로, 우리는 디코딩된 값을 응답 개체에 추가합니다.

> 참고: JWT 인증에 주로 집중하기 때문에 앱의 일부 부분은 다루지 않습니다. 예를 들어 데이터베이스 연결에 대한 자세한 내용은 'app' 디렉토리에 있는 'db' 폴더를 확인할 수 있습니다. 전체 응용 프로그램에 따라 서로 잘 맞는 방법에 대한 자세한 내용은 GitHub 보고서를 참조하십시오.

## 클라이언트측 Vue.js 앱 구축

이제 애플리케이션의 클라이언트 부분에 초점을 맞춰 보겠습니다. Vue.js 앱을 부트스트랩하려면 터미널/명령 프롬프트로 이동하여 루트 폴더에서 `vue create <folder-name` 명령을 실행하면 됩니다.

먼저 Vue CLI를 이전에 설치하지 않았다면 Vue CLI를 시스템에 전체적으로 설치해야 합니다. 이를 위해 npm install-g @vue/cli 또는 yarn global add @vue/cli를 실행할 수 있습니다. 우리는 또한 `패키지`를 살펴볼 수 있다.json의 파일을 통해 클라이언트 앱에 필요한 종속성을 확인할 수 있습니다.

```undefined
{
  "name": "vue-jwt-app",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint",
    "heroku-postinstall": "npm install && npm run build"
  },
  "proxy": "http://localhost:3000",
  "dependencies": {
    "@babel/preset-env": "7.3.4",
    "axios": "^0.20.0",
    "bootstrap": "^4.5.3",
    "bootstrap-vue": "^2.18.0",
    "core-js": "^3.6.5",
    "vue": "^2.6.11",
    "vue-jwt-decode": "^0.1.0",
    "vue-router": "^3.4.7"
  },
  "devDependencies": {
    "@vue/babel-preset-app": "^4.5.8",
    "@vue/cli-plugin-babel": "^4.5.8",
    "@vue/cli-plugin-eslint": "^4.5.0",
    "@vue/cli-service": "^4.5.0",
    "babel-eslint": "^10.1.0",
    "eslint": "^6.7.2",
    "eslint-config-prettier": "^6.14.0",
    "eslint-plugin-prettier": "^3.1.4",
    "eslint-plugin-vue": "^6.2.2",
    "less": "^3.12.2",
    "less-loader": "^7.0.2",
    "vue-template-compiler": "^2.6.11"
  },
  "eslintConfig": {
    "root": true,
    "env": {
      "node": true
    },
    "extends": [
      "plugin:vue/essential",
      "plugin:prettier/recommended",
      "eslint:recommended"
    ],
    "parserOptions": {
      "parser": "babel-eslint"
    },
    "rules": {}
  },
  "browserslist": [
    "> 1%",
    "last 2 versions",
    "not dead"
  ],
  "Author": "Alexander Nnakwue"
}
```

우리가 `패키지`에서 볼 수 있다.위의 json 파일은 클라이언트 앱이 실행되는 데 필요한 종속성을 몇 개 설치했습니다. 가장 중요한 것은 Vue.js 애플리케이션을 위한 JWT 디코더인 `vue-jwt-decode`를 설치했다는 점이다. 또한 Axios를 사용하여 백엔드 서버에 API 호출을 수행하고 있습니다. 클라이언트 앱을 실행하는 데 필요한 기타 중요한 종속성은 `패키지`에서 찾을 수 있다.위의 json 파일

다음으로 `src/components/auth` 폴더로 이동하겠습니다. 거기서, 우리는 우리의 클라이언트 앱에 필요한 모든 구성 요소를 찾을 것입니다. 먼저 로그인 구성 요소를 살펴보겠습니다. 이유는 무엇입니까? 사용자가 앱에 로그인할 때 생성된 JWT를 로컬 스토리지에 저장합니다. 로그인 구성 요소의 모양을 살펴보겠습니다.

```xml
<template>
  <div class="container">
    <b-card
      bg-variant="dark"
      header="Vue_JWT_APP"
      text-variant="white"
      class="text-center"
    >
      <b-card-text>Already have an account? Login Here!</b-card-text>
      <div class="row">
        <div class="col-lg-6 offset-lg-3 col-sm-10 offset-sm-1">
          <form
            class="text-center border border-primary p-5"
            style="margin-top:70px;height:auto;padding-top:100px !important;"
            @submit.prevent="loginUser"
          >
            <input
              type="text"
              id="email"
              class="form-control mb-5"
              placeholder="Email"
              v-model="login.email"
            />
            <!-- Password -->
            <input
              type="password"
              id="password"
              class="form-control mb-5"
              placeholder="Password"
              v-model="login.password"
            />
            <p>
              Dont have an account? Click
              <router-link to="/register"> here </router-link> to sign up
            </p>
            <!-- Sign in button -->
            <center>
              <button class="btn btn-primary btn-block w-75 my-4" type="submit">
                Sign in
              </button>
            </center>
          </form>
        </div>
      </div>
    </b-card>
  </div>
</template>
<script>
export default {
  data() {
    return {
      login: {
        email: "",
        password: ""
      }
    };
  },
  methods: {
    async loginUser() {
      try {
        let response = await this.$http.post("/auth/login", this.login);
        let token = response.data.data.token;
        localStorage.setItem("user", token);
        // navigate to a protected resource 
        this.$router.push("/me");
      } catch (err) {
        console.log(err.response);
      }
    }
  }
};
</script>
```

우리는 "bootstrap-vue"를 사용하여 구성 요소를 스타일링하고 있습니다. 위와 같이 백엔드 API를 통해 앱에 로그인하면 반환된 토큰을 localStorage로 설정하고 있습니다. 그렇게 되면, 우리는 라우터 푸시 API를 사용하여 `홈` 컴포넌트로 이동한다.

여기서는 전체 흐름을 입증하기 위해 로컬 스토리지에서 토큰을 검색한 다음 vue-jwt-decode 패키지를 사용하여 토큰을 디코딩한 다음 마지막으로 이를 사용자 개체에 추가하여 홈 구성 요소에 표시할 수 있습니다.

```xml
<template>
  <div>
    <b-navbar id="navbar" toggleable="md" type="dark" variant="info">
      <b-navbar-brand href="#">
        You are currently viewing the Vue_JWT_App
      </b-navbar-brand>
      <b-navbar-nav class="ml-auto">
        <b-nav-text>{ user.firstname } | </b-nav-text>
        <b-nav-item @click="logUserOut" active>Logout</b-nav-item>
      </b-navbar-nav>
    </b-navbar>
    <section>
      <div class="container mt-5">
        <div class="row">
          <div class="col-md-12">
            <ul class="list-group">
              <li class="list-group-item">
                Name : { user.firstname } { user.lastname }
              </li>
              <li class="list-group-item">Email : { user.email }</li>
            </ul>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>
<script>
import VueJwtDecode from "vue-jwt-decode";
export default {
  data() {
    return {
      user: {}
    };
  },
  methods: {
    getUserDetails() {
      // get token from localstorage
      let token = localStorage.getItem("user");
      try {
      //decode token here and attach to the user object
      let decoded = VueJwtDecode.decode(token);
      this.user = decoded;    
      } catch (error) {
        // return error in production env
        console.log(error, 'error from decoding token')
      }
    },
    logUserOut() {
      localStorage.removeItem("user");
      this.$router.push("/login");
    }
  },
  created() {
    this.getUserDetails();
  }
};
</script>
<style>
#navbar {
  margin-bottom: 15px;
}
</style>
```

주목할 점은 vue-jwt-decode 패키지는 무게가 가볍기 때문에 "try/catch" 블록으로 포장하여 오류 사례를 처리해야 한다는 점이다. 또한 전체 클라이언트 앱 설정에 필요한 경로 파일과 기타 파일에 대한 자세한 내용은 GitHub의 클라이언트 폴더에서 확인할 수 있습니다.

## Heroku에 앱 배포

Heroku에 앱을 배포하려면 먼저 Heroku 계정과 Heroku CLI가 개발 기기에 설치되어 있어야 합니다. 자세한 내용은 설명서의 이 부분을 참조하십시오. 노드 엔진 버전 및 시작 스크립트와 같은 설정을 지정한 후 앱을 로컬에서 테스트할 수 있습니다.

그러기 위해서는 우리 프로젝트의 뿌리에 프럭파일이 필요하다. Herku는 앱에 실행할 서버의 경로를 알려주는 Profile이 필요합니다. 파일 내용은 다음과 같습니다.

```undefined
web: node ./dist/server.js
```

로컬에서 앱을 시작하려면 herku local web을 실행하면 된다. 출력은 다음과 같습니다.

```coffeescript
[OKAY] Loaded ENV .env File as KEY=VALUE Format
01:43:05 web.1   |  postgres://kdwqywwonzxzoa:82a2d163fe213bf3d37c1af67b9b8adbbaf2206354c6f261551eb4b8c9f15caf@ec2-54-156-149-189.compute-1.amazonaws.com:5432/daclachtll2s1g DATABASE_URL
01:43:05 web.1   |  3000 APP-PORT
01:43:05 web.1   |  App live and listening on port: 3000
```

보시는 바와 같이, 앱에 Postgres 데이터베이스를 사용하기 때문에 Heroku 대시보드에 Postgres 추가 기능을 설정했습니다. 또한 패키지의 스크립트 섹션을 살펴봅니다.json 파일, 우리는 우리 앱에서 생산에 필요한 부품들을 볼 수 있습니다.

빌드 명령은 `rm -rf dist`를 실행합니다.

이제 우리 앱을 헤로쿠에 배포하기만 하면 됩니다. 그러려면 Git(마스터 분기에 있는)에 변경 사항을 커밋한 다음 Heroku CLI에서 `heroku login` 명령을 사용하여 Heroku 계정에 로그인해야 합니다. 올바른 자격 증명을 입력하면 `heroku create` 명령으로 새 Heroku 앱을 만들 수 있습니다.

작업이 끝나면 git push herku master를 실행해 앱을 헤로쿠 마스터 지점에 푸시할 수 있다. 그런 다음 배포에 대한 URL을 확인할 수 있습니다. 아래 표시된 출력의 잘린 부분에서 확인할 수 있습니다.

```undefined
remote:        found 0 vulnerabilities
remote:        
remote:        
remote: -----> Build succeeded!
remote: -----> Discovering process types
remote:        Default types for buildpack -> web
remote: 
remote: -----> Compressing...
remote:        Done: 36.8M
remote: -----> Launching...
remote:        Released v5
remote:        https://secure-cliffs-18654.herokuapp.com/ deployed to Heroku
remote: 
remote: Verifying deploy... done.
To https://git.heroku.com/secure-cliffs-18654.git
   91ced12..28baa4c  master -> master
```

마지막 단계는 URL `https://secure-cliffs-18654.herokuapp.com/`을 복사하고, 클라이언트 앱의 `src/main.js` 폴더로 이동하여 기본 URL을 업데이트하는 것입니다. 해당 파일의 최종 내용은 다음과 같습니다.

```js
import Vue from "vue";
import App from "./App.vue";
import router from "./router";
import axios from "axios";
import BootstrapVue from "bootstrap-vue";
import "bootstrap/dist/css/bootstrap.css";
import "bootstrap-vue/dist/bootstrap-vue.css";
const base = axios.create({
  baseURL: "https://secure-cliffs-18654.herokuapp.com/api/v1" // replace on production env
});
import store from "./store/index";
Vue.config.productionTip = false;
Vue.use(BootstrapVue);
Vue.prototype.$http = base;
new Vue({
  store,
  router,
  render: h => h(App)
}).$mount("#app");
```

`베이스`를 추가한 부분을 주목하십시오.axios.create 메서드의 URL. 그 후, 우리는 Git에게 새로운 변화를 다시 할 수 있습니다. 그리고 우리는 살아있습니다. 마지막으로 앱을 테스트하기 위해 로그인 경로로 이동하여 배포를 테스트할 수 있습니다. 경로는 다음과 같습니다.

```cpp
https://secure-cliffs-18654.herokuapp.com/login
```

새 계정에 등록하면 로그인할 수 있으며, 즉시 `홈` 경로로 리디렉션됩니다.

홈페이지로 리디렉션되면 시연 목적으로 현재 인증된 사용자의 JWT 페이로드를 콘솔에 로깅합니다. 자세한 내용은 아래 이미지에 나와 있습니다.

## 결론

이 튜토리얼에서는 노드 및 Vue.js 앱에서 JWT를 통합하는 방법에 대해 다루었다. 우리는 전체 백엔드 설정으로 시작하여 애플리케이션을 구축한 후 애플리케이션의 클라이언트 부분에서 작업했습니다.

연습에서 알 수 있듯이, JWT는 사용자 상태가 서버 메모리나 데이터베이스에 저장되지 않기 때문에 순수하게 상태 비저장 인증 메커니즘을 제공합니다. 서버의 보호된 경로는 Authorization 헤더에서 유효한 JWT를 검사하며, 권한이 있는 경우 사용자에게 액세스 권한이 부여됩니다.

우리는 JWT의 작동 방식, JWT의 구조 및 검증 방법을 배웠다. 우리는 또한 JWT를 생성하고 JWT 데이터를 확인하는 방법에 대해서도 배웠다. 클라이언트 계층에서 우리는 `vue-jwt-decode` 라이브러리를 사용하여 JWT를 디코딩하는 방법을 배웠다.

마지막으로, 토큰은 사이트 간 스크립팅(XSS) 공격에 취약하기 때문에 `localStorage`에 토큰을 저장하면 자체 위험 집합이 수반된다는 점에 주의해야 한다. JWT의 보안 문제 방지와 다른 기능을 사용해야 하는 시기에 대한 세부 정보를 검토하는 것이 중요합니다.

JWT에 대한 자세한 내용은 이 상세 핸드북 및 설명서를 참조하십시오. 질문 및 피드백은 의견란에 표시해주시고 읽어주셔서 감사합니다!