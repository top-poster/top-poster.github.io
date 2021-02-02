---
layout: post
title: "프런트 엔드 개발에서 비밀을 관리하고 저장하는 모범 사례
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/secureapikeys.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/secureapikeys.png?fit=730%2C487&ssl=1)

## 프론트 엔드 개발의 비밀
 

프론트 엔드 개발에서는 비밀과 자격 증명이 무모한 관행으로 혼란을 일으킬 가능성이 있으므로 적절하게 저장 및 관리되도록 적절한 조치를 취해야합니다.
 이 기사에서는 일반적으로 API에서 비밀과 키를 관리하는 가장 좋은 방법을 살펴볼 것입니다.
 

API는 개인 또는 공용 API 일 수 있습니다.
 비공개 API는 조직 내 사내 개발자가 개발하고 호스팅하는 API입니다.
 비공개 API는 사내 개발자 만 사용할 수 있으며 해당 조직 외부의 개발자가 공유하거나 사용하지 않습니다.
 개발자가 응용 프로그램을 개발하는 방법을 완전히 제어 할 수 있으므로 비공개 API를 쉽게 관리 할 수 있습니다.
 비공개 API는 기본적으로 특정 조직의 개발자가 구축 한 애플리케이션 기능과 인터페이스 할 수있는 API입니다.
 비공개 API의 좋은 예는 귀하 (프런트 엔드 개발자)가 조직의 데이터에 액세스 할 수 있도록 백엔드 개발자가 개발 한 API입니다.
 비공개 API는 제한되어 있으므로 API를 사용하기 전에 키 또는 비밀을 포함 할 필요가 없습니다.
 

반면에 공개 API는 독점 소프트웨어 응용 프로그램 또는 웹 서비스에 대한 액세스를 제공하는 공개적으로 사용할 수있는 타사에서 제공하는 API 서비스입니다.
 이름에서 알 수 있듯이 공개 API는 개발 된 조직 내부 및 외부의 모든 개발자가 사용할 수 있습니다.
 이를 통해 개발자는 처음부터 이러한 기능을 구축하는 대신 이미 사용 가능한 기능을 활용하여 애플리케이션을 향상시킬 수 있습니다.
 공개 API의 좋은 예는 개발자가 애플리케이션 내에서 Google지도를 사용할 수 있도록하는 Google Maps API입니다.
 일부 서비스 공급자는 평생 무료로 공용 API를 제공하는 반면 다른 서비스 공급자는 특정 요청 수에 대해 유료 또는 무료를 제공합니다.
 효과적인 권한 부여 및 인증을 위해 API 공급자는 API의 각 사용자에게 고유 한 키와 자격 증명 비밀을 사용합니다.
 이러한 키와 비밀은 잘못된 손에 들어간 경우 심각한 문제를 일으킬 수 있으므로 안전하게 관리하고 저장해야합니다.
 

## 노출 된 비밀의 결과로 발생할 수있는 잠재적 문제
 

적절하게 저장되지 않은 API 키 및 자격 증명 비밀은 재정적, 규제 적 또는 평판에 손상을 줄 수 있습니다.
 

- Google Cloud Platform (GCP)과 같은 타사 서비스 제공 업체가 제한된 요금으로 서비스에 대한 액세스를 제공하는 경우, 귀하의 비밀이 노출되면 권한없는 사용자가 많은 요청을 수행했기 때문에 서비스에 대한 액세스가 거부 될 수 있습니다.
 귀하를 대신하여 귀하의 한도를 초과합니다.
 한도를 초과하는 것 외에도 청구서가 급증 할 수 있습니다.
 
- 자격 증명 비밀이 유출되고 애플리케이션이 API 제공 업체 사용 약관을 위반하는 경우 API 제공 업체가 서비스에 대한 액세스를 철회 할 수 있으며 이는 조직의 평판을 손상시킬 수 있습니다.
 
- 마지막으로 해커가 공급자에게 직접 지시하고 비즈니스 로직을 우회 할 수 있으므로 리소스를 제어 할 수 없습니다.
 
- 해커는 조직의 데이터를 침해 할 수있는 민감한 데이터에 액세스 할 수 있습니다.
 

## 나쁜 습관
 

### 자격 증명 비밀을 코드에 직접 포함
 

다음 코드 스 니펫에서 React를 사용할 것이지만 바닐라 JavaScript 및 기타 프레임 워크에도 적용 할 수 있습니다.
 

```js
import React from "react";
 
const index = () => {
 const Api_key = "1234567"
 
 return(
   <>
   <p>Hello, Secrets </p>
   </>
 )
}
export default index;
```

개발자 도구를 사용하여 브라우저에서 자격 증명 비밀을 쉽게 추출 할 수 있기 때문에 이것은 나쁜 습관입니다.
 이것은 간단하게 수행 할 수 있습니다.
 

- 웹 페이지 검사 또는 CTRL + SHIFT + I
 
- "소스"탭으로 이동
 
- static / js를 클릭하십시오.
 
- main.chunk.js를 클릭하십시오.
 

누구나 쉽게 추출 할 수있는 자격 증명 비밀을 찾을 수 있습니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/extractedsecret.png?resize=512%2C288&ssl=1)

### 코드에서 직접 암호를 사용하여 코드베이스를 git / GitHub에 업로드
 

```coffeescript
import emailjs from ‘emailjs-com’
function App(){
    const handleSubmit = (e) => {
 e.preventDefault(); 
 
 emailjs
   .sendForm(`gmail`, "876TY43sa23r56y789", e.target, process.env.REACT_APP_USER_ID)
   .then(
     (result) => {
alert("Message Sent, We will get back to you shortly",     result.text);
     },
     (error) => {
       alert("An error occured, Please try again", error.text);
     }
   );
};
 
    return(
    <>
    <form onSubmit={handleSubmit}>
<input name="name"/>
<input name="email"/>
<button type="submit">Submit</button>
</form>
    </>
)
}
export default App;
```

누구나 온라인으로 저장소에 액세스 할 수 있기 때문에 이것은 또한 나쁜 습관입니다.
 저장소가 비공개 저장소이더라도 일부 해커는 GitHub 크롤러를 사용하여 자격 증명 비밀을위한 저장소를 크롤링합니다.
 이에 대한 좋은 해결책은 자격 증명 비밀을 다음 섹션에서 볼 .env 파일에 저장하는 것입니다.
 

어느 시점에서든 API 자격 증명을 커밋하고 git repo에 푸시 한 경우 가능한 한 빨리 키를 재설정해야합니다.이 작업은 API 서비스 제공 업체에서 대시 보드에 액세스하여 수행하거나 git rebase를 사용하여 모든 추적을 제거하여 수행 할 수 있습니다.
 키를 추가 한 특정 커밋을 제거합니다.
 

### API 키 또는 비밀에 대한 제한을 설정하지 않음
 

대부분의 API 서비스 공급자는 일일 요청 수와 API에 액세스 할 수있는 특정 URL에 대한 제한을 설정하여 사용을 제한 할 수 있습니다.
 아래 이미지에서 도메인이 저장되지 않았으므로 API 자격 증명이있는 모든 URL에서 요청을 보낼 수 있습니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/nodomainwassaved.png?resize=512%2C288&ssl=1)

## 좋은 습관
 verified_user

### API 키에 대한 제한 설정
 

일부 서비스 제공 업체에서는 API 키 사용에 대한 제한을 설정할 수 있으므로 API 키는 지정한 URL에서만 액세스 할 수 있습니다.
 즉, 해커가 키에 액세스하더라도 지정된 URL에서만 사용할 수 있으므로 쓸모가 없습니다.
 또한 API 자격 증명 사용에 대한 일일 제한을 설정할 수 있습니다.
 아래 이미지에서 API에 대한 요청은 내가 지정한 URL로만 이루어질 수 있습니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/domainrestrictions.png?resize=512%2C288&ssl=1)

### 환경 변수 (.env) 파일에서 키 숨기기
 

.env 파일을 사용하면 암호가 코드에 직접 포함되지 않습니다.
 이것은 git에서 특히 좋습니다.
 코드를 git에 업로드하고 .env 파일을 .gitignore 파일에 추가 할 수 있습니다.
 이렇게하면 .env 파일이 GitHub에 커밋되지 않습니다.
 다음 단계로 수행 할 수 있습니다.
 

- 프로젝트의 루트에 .env 파일을 만듭니다.
 

```css
- your_react_project_folder
 - public
 - src
 - node_modules
 - .env         <-- your .env file
 - .gitignore
 - package-lock.json
 - package.json
```

- .env 파일에서 REACT_APP_를 API 키 이름에 접두사로 추가하고 값 (React 애플리케이션의 경우) 및 VUE_APP_를 API 키 이름의 접두사로 설정하고 값 (Vue 애플리케이션의 경우)을 설정합니다.
 이를 통해 프레임 워크는 변수를 식별 할 수 있습니다.
 

```undefined
# .env
 
REACT_APP_YOUR_API_KEY_NAME=your_api_key  <-- for react apps
VUE_APP_YOUR_API_KEY_NAME=your_api_key <-- for vue apps
 
# Example:
REACT_APP_TEMPLATE_ID=98765432123456789
REACT_APP_USER_ID=98765432123567
VUE_APP_USER_ID=98765432123456789
```

.env 파일을 .gitignore 파일에 추가합니다. 이렇게하면 .env 파일이 git에 커밋되지 않고 리포지토리를 GitHub에 푸시 할 때 API 키가 숨겨집니다.
 

```shell
#.gitignore file
 
# dependencies
/node_modules
 
# env
.env
```

이제 process.env를 추가하여 코드에서 API 키를 사용할 수 있습니다.
 

```coffeescript
//app.js
//here I used api keys from emailjs already declared in the .env file.
import emailjs from ‘emailjs-com’
function App(){
    const handleSubmit = (e) => {
 e.preventDefault(); 
 
 emailjs
   .sendForm(`gmail`, process.env.REACT_APP_TEMPLATE_ID, e.target, process.env.REACT_APP_USER_ID)
   .then(
     (result) => {
alert("Message Sent, We will get back to you shortly",     result.text);
     },
     (error) => {
       alert("An error occured, Plese try again", error.text);
     }
   );
};
 
    return(
    <>
    <form onSubmit={handleSubmit}>
        <input name="name"/>
        <input name="email"/>
        <button type="submit">Submit</button>
        </form>
    </>
)
}
export default App;
```

이는 좋은 방법이지만 API 키가 브라우저 devtool에 계속 표시되므로 매우 안전한 방법은 아닙니다.
 API 자격 증명은 여전히 빌드의 일부가되며 파일을 검사하는 모든 사람이 볼 수 있습니다.
 이전에했던 것처럼 :
 

- 웹 페이지 검사 또는 CTRL + SHIFT + I
 
- "소스"탭으로 이동
 
- static / js를 클릭하십시오.
 
- main.chunk.js를 클릭하십시오.
 

devtools의 .env 파일에 정의 된 API 키를 찾을 수 있습니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/envfilesindevtools.png?resize=512%2C288&ssl=1)

### GitGuardian과 같은 비밀 스캔 솔루션으로 git 리포지토리 스캔
 

비밀 스캔 도구는 GitHub, GitLab 또는 Bitbucket에서 원격 저장소의 git 커밋을 스캔하여 실수로 커밋 된 비밀을 확인하는 도구입니다.
 이렇게하면 중요한 정보가 원격 저장소에 노출되는 것을 방지 할 수 있습니다.
 이러한 솔루션을 사용하면 리포지토리에 커밋 된 비밀이 자동으로 감지되고 캡처됩니다.
 

프로젝트에 GitGuardian을 설정하려면 :
 

- 적절한 계획을 선택하고 GitGuardian에서 계정을 만듭니다.
 
- 이메일을 확인하고 대시 보드에 로그인하세요.
 
- 대시 보드에서 통합 ⇒ 소스 모니터링 ⇒ 설치 (웹 기반 리포지토리로 이동 – GitHub, GitLab, Github Enterprise)로 이동합니다.
 
- 설치했으면 저장소에서 프로젝트를 선택하십시오.
 
- GitGuardian은 저장소를 스캔하고 비밀 유출 가능성을 알려주는 메일을 보냅니다.
 

### GitHub 자격 증명을 공유하지 마십시오.
 

GitHub 자격 증명을 개발 팀 외부 사람과 공유하지 말고 더 이상 팀에서 일하지 않는 개발자의 액세스 권한을 취소해야합니다.
 

## 결론
 

API 자격 증명에 대해 원하는 보안 수준에 따라 API 키 및 비밀 보안은 프런트 엔드 애플리케이션에서 매우 중요합니다.
 .env 파일에 비밀을 저장하는 것은 좋지만 그것만으로는 안전하지 않습니다.
 항상 키에 대한 제한을 설정해야합니다.
 이것으로 당신의 비밀이 유출 되더라도 그것에 접근 할 수있는 사람의 손에는 쓸모가 없을 것입니다.
 추가 보안 계층은 비밀 스캔 서비스를 사용하여 저장소를 스캔합니다.
 이 문서에서 강조 표시된 관행을 사용하여 프로젝트 작업 중에 민감한 데이터를 보호하십시오.
읽어 주셔서 감사합니다.
 