---
layout: post
title: "Nodemailer를 사용하여 Node.js 서버에서 이메일을 보내는 방법
 "
author: 'Code Tower'
thumbnail: https://images.unsplash.com/photo-1534567406103-997f6c5e7f93?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MXwxMTc3M3wwfDF8c2VhcmNofDk3fHxtYWlsfGVufDB8fHw&ixlib=rb-1.2.1&q=80&w=2000
tags: undefined
---


Nodemailer는 서버에서 쉽게 이메일을 보낼 수있는 Node.js 모듈입니다.
 사용자와 통신하든 문제가 발생했을 때 자신에게 알리기를 원하든이를위한 옵션 중 하나는 메일을 이용하는 것입니다.
 

베어 본 형태로 Nodemailer를 사용하는 방법을 설명하는 많은 기사가 있지만이 기사는 그중 하나가 아닙니다.
 여기에서는 Nodemailer와 Gmail을 사용하여 Node.js 백엔드에서 이메일을 보내는 가장 일반적인 방법을 보여 드리겠습니다.
 

## Nodemailer를 시작하는 방법
 

먼저 Express를 사용하여 Node.js 상용구를 설정해야합니다.
 Node 및 npm이 설치되어 있는지 확인하려면 다음 명령을 실행할 수 있습니다.
 

```undefined
node -v 
npm -v
```

이 두 명령이 모두 버전을 표시하면 이동하는 것이 좋습니다.
 그렇지 않으면 누락 된 것을 설치하십시오.
 

프로젝트에 대한 디렉토리를 만듭니다.
 nodemailerProject를 사용할 것입니다.
 

```undefined
mkdir nodemailerProject
```

새로 생성 된 디렉터리로 이동하여 실행
 

```undefined
npm init
```

그러면 package.json 파일로 프로젝트가 초기화됩니다.
 

다음으로 다음을 사용하여 Express를 설치해야합니다.
 

```undefined
npm install express
```

진입 점으로 지정한 파일 (기본값은 index.js)에 따라 파일을 열고 다음 코드를 붙여 넣습니다.
 

위는 Express를 사용하여 간단한 서버를 시작하는 데 필요한 것입니다.
 다음을 실행하여 제대로 작동하는지 확인할 수 있습니다.
 

```undefined
node index.js
```

### Nodemailer를 설치하는 방법
 

다음 명령을 사용하여 nodemailer를 설치하십시오.
 

```undefined
npm install nodemailer
```

Nodemailer의 API는 매우 간단하며 다음을 수행해야합니다.
 

- Transporter 개체 만들기
 
- MailOptions 개체 만들기
 
- Transporter.sendMail 메서드 사용
 

전송기 객체를 생성하기 위해 다음을 수행합니다.
 

```undefined
let transporter = nodemailer.createTransport({
      service: 'gmail',
      auth: {
        type: 'OAuth2',
        user: process.env.MAIL_USERNAME,
        pass: process.env.MAIL_PASSWORD,
        clientId: process.env.OAUTH_CLIENTID,
        clientSecret: process.env.OAUTH_CLIENT_SECRET,
        refreshToken: process.env.OAUTH_REFRESH_TOKEN
      }
    });
```

> ✋ Gmail 계정에 대한 자신의 자격 증명 인 사용자 및 패스 키를 제외하고 다른 세 키는 OAuth를 설정 한 후 검색해야합니다.
 

이 기사의 시작 부분에서 언급했듯이 메일 전송 요구에 Gmail을 사용할 것입니다.
 짐작 하셨겠지만 Gmail은 사용자 계정으로 /로 전송되는 메일에 대해 높은 수준의 보안을 제공합니다.
 

이 장애물을 극복 할 수있는 방법에는 여러 가지가 있으며 (일부는 다른 것보다 낫다) Google Cloud Platform에서 프로젝트를 설정해야하는 방법을 선택할 것입니다.
 Gmail에서 OAuth 보안을위한 자격 증명을 사용하려면이를 수행해야합니다.
 

> nodemailer와 함께 Gmail을 사용하는 복잡성에 대해 자세히 알아 보려면 여기로 이동하세요.
 

다음 단계에서는 코딩 대신 몇 가지 구성이 필요하므로 스스로 준비하십시오.
 

![image](https://images.unsplash.com/photo-1503387762-592deb58ef4e?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MXwxMTc3M3wwfDF8c2VhcmNofDI2fHxkcmF3aW5nJTIwYm9hcmR8ZW58MHx8fA&ixlib=rb-1.2.1&q=80&w=2000)

## Google Cloud Platform 구성
 

Google Cloud Platform 계정이없는 경우 사전 요구 사항으로 설정해야합니다.
 설정이 완료되면 왼쪽 상단의 드롭 다운 메뉴를 클릭하여 새 프로젝트를 만듭니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/1_a4fnFLNMoTtLJuqsKilVnA.png)

새 프로젝트 옵션을 선택하십시오.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/1_HNwUG3wPdbrwc3JB5D7_tg.png)

다음 창에서 프로젝트 이름을 지정해야합니다.
 원하는 것을 선택하되 NodemailerProject 이름을 계속 사용하겠습니다.
 위치 속성의 경우 조직 없음으로 둘 수 있습니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/1_TRlA6RBLCCCSMQ5R4di27A.png)

프로젝트를 설정하는 데 몇 초 정도 걸릴 수 있지만 그 후에 다음 화면을 볼 수 있습니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/1_FT9MhBZyU4cZd4Qg6zeFag.png)

왼쪽 상단 모서리에있는 세 개의 파선을 클릭하여 탐색 메뉴를 열고 API 및 서비스를 선택하십시오.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/1_qPaPpPadHQLdKCQbhjND7Q.png)

Nodemailer와 Gmail을 사용하려면 OAuth2를 사용해야합니다.
 OAuth에 익숙하지 않은 경우 인증을위한 프로토콜입니다.
 필요하지 않기 때문에 여기서 구체적인 내용은 다루지 않겠습니다. 자세한 내용은 여기로 이동하세요.
 

먼저 OAuth 동의 화면을 구성해야합니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/1_W2oeT1KmJXpwSQlIMIVo5w.png)

G-Suite 회원이 아닌 경우 사용할 수있는 유일한 옵션은 외부 사용자 유형입니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/1_l_GrPVtXODPS0GXKLMdWYA.png)

만들기를 클릭 한 후 다음 화면에서 애플리케이션 정보 (서버)를 입력해야합니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/1_reZ04hUX4jh1IzLGh7vCFA.png)

사용자 지원 이메일 필드와 개발자 연락처 정보 필드에 이메일을 입력합니다.
 저장하고 계속을 클릭하면이 구성의 범위 단계로 이동합니다.
 이 단계는 우리와 관련이 없으므로 건너 뛰고 사용자 테스트 단계로 이동합니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/1_Jms50wZ5mVmUyOaiVF7b4w.png)

여기에서 자신을 사용자로 추가하고 저장하고 계속하기를 클릭합니다.
 

## OAuth 설정을 구성하는 방법
 

이 단계에서는 Nodemailer에서 사용할 OAuth 자격 증명을 만듭니다.
 OAuth 동의 화면 위의 자격 증명 탭으로 이동하십시오.
 Create Credentials 텍스트가있는 더하기 (➕) 기호를 클릭하고 OAuth 클라이언트 ID를 선택합니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/1_h0nME2ccR7HPjKmz_DMZRw.png)

애플리케이션 유형 드롭 다운 메뉴에서 웹 애플리케이션을 선택합니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/1_72Em-VS-fdM2WCwOA6zcfg.png)

Authorized Redirect URIs 섹션에서 OAuth2 Playground (https://developers.google.com/oauthplayground)를 추가해야합니다.이 문서의 시작 부분에서 언급 한 키 중 하나를 가져 오는 데 사용할 것입니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/1_ywIcOlqA5DHdsPaSNnjJ9Q.png)

만들기를 클릭하면 클라이언트 ID와 클라이언트 암호가 표시됩니다.
 이것들을 자신에게만 보관하고 어떠한 방식, 모양, 형태로도 노출하지 마십시오.
 

![image](https://images.unsplash.com/photo-1575783970733-1aaedde1db74?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MXwxMTc3M3wwfDF8c2VhcmNofDF8fHBsYXlncm91bmR8ZW58MHx8fA&ixlib=rb-1.2.1&q=80&w=2000)

### OAuth 새로 고침 토큰 받기
 

Nodemailer의 전송기 개체 내에서 사용할 새로 고침 토큰을 얻으려면 OAuth2 Playground로 이동해야합니다.
 우리는 이전 단계에서이 특정 목적을 위해이 URI를 승인했습니다.
 

1. 오른쪽에있는 톱니 바퀴 아이콘 (OAuth2 구성)을 클릭하고 자체 OAuth2 자격 증명을 사용하려면 확인란을 선택합니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/1_Kbg3RnTBNkDd_RQ0zn59mQ.png)

2. 웹 사이트의 왼쪽을 보면 서비스 목록이 표시됩니다.
 Gmail API v1이 표시 될 때까지 아래로 스크롤합니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/1_BppvkU1r4JzZ6j6FvC2qNw.png)

3. API 승인을 클릭합니다.
 

Gmail 계정에 로그인 할 수있는 화면이 표시됩니다.
 테스트 사용자로 나열한 사용자를 선택하십시오.
 

4. 다음 화면에서 Google에서 아직이 신청서를 확인하지 않았 음을 알 수 있지만 확인을 위해 제출하지 않았으므로 괜찮습니다.
 계속을 클릭하십시오.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/1_rL0tNdaZqOyIg6aCp4IR3g.png)

5. 다음 화면에서 프로젝트에 Gmail 계정과 상호 작용할 수있는 권한을 부여하라는 메시지가 표시됩니다.
 그렇게하세요.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/1_y0TUXbtC_oUaB6KoGlURbQ.png)

6. 완료되면 OAuth Playground로 다시 리디렉션되고 왼쪽 메뉴에 인증 코드가 있음을 확인할 수 있습니다.
 토큰에 대한 Exchange 인증 코드라고 표시된 파란색 버튼을 클릭합니다.
 

이제 새로 고침 토큰 및 액세스 토큰에 대한 필드가 채워집니다.
 

## 서버로 돌아 가기
 

이러한 구성을 모두 수행 한 후 애플리케이션으로 돌아가서 모든 데이터를 전송기 생성에 입력 할 수 있습니다.
 모든 자격 증명을 비공개로 유지하려면 dotenv 패키지를 사용할 수 있습니다.
 생성 할 .env 파일도 .gitignore에 추가하는 것을 잊지 마십시오.
 

이제 우리는 이것을 가지고 있습니다 :
 

```undefined
let transporter = nodemailer.createTransport({
      service: 'gmail',
      auth: {
        type: 'OAuth2',
        user: process.env.MAIL_USERNAME,
        pass: process.env.MAIL_PASSWORD,
        clientId: process.env.OAUTH_CLIENTID,
        clientSecret: process.env.OAUTH_CLIENT_SECRET,
        refreshToken: process.env.OAUTH_REFRESH_TOKEN
      }
    });
```

다음으로 이메일을 보낼 위치와 데이터에 대한 세부 정보를 보유하는 mailOptions 객체를 만듭니다.
 

```undefined
let mailOptions = {
      from: tomerpacific@gmail.com,
      to: tomerpacific@gmail.com,
      subject: 'Nodemailer Project',
      text: 'Hi from your nodemailer project'
    };
```

이 개체는 더 많은 필드와 여러 수신자를 가질 수 있지만 여기서는 다루지 않겠습니다.
 

마지막으로 sendMail 메소드를 사용합니다.
 

```undefined
transporter.sendMail(mailOptions, function(err, data) {
      if (err) {
        console.log("Error " + err);
      } else {
        console.log("Email sent successfully");
      }
    });
```

응용 프로그램을 실행하면받은 편지함이 새 이메일로 채워지는 것을 볼 수 있습니다.
 

이 기사는 Nodemailer를 사용하여 만든 프로젝트에서 영감을 받았습니다.
 확인하려면 여기로 이동하세요.
 