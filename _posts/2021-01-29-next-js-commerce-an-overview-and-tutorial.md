---
layout: post
title: "Next.js Commerce : 개요 및 자습서
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/Screen-Shot-2021-01-28-at-10.58.42-AM.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Screen-Shot-2021-01-28-at-10.58.42-AM.png?fit=730%2C486&ssl=1)

Next.js는 React로 SSR 및 정적 웹 애플리케이션을 모두 빌드 할 수있는 인기있는 오픈 소스 웹 프레임 워크입니다.
 Vercel에서 만든이 생산 준비 프레임 워크는 확장 성과 사용 용이성으로 인해 전 세계 엔지니어링 팀에서 사용됩니다.
 

이제 프레임 워크는 고성능의 시각적으로 매력적이며 완전히 사용자 정의 가능한 전자 상거래 사이트를 구축하기위한 개발자 시작 키트 인 Next.js Commerce도 제공합니다.
 그런 다음 이러한 사이트를 장바구니 및 분석 도구와 같은 다양한 앱 통합으로 확장하여 고객과 상점 소유주 모두에게 완전한 온라인 쇼핑 경험을 제공 할 수 있습니다.
 

Next.js Commerce는 BigCommerce와 함께 제공되며 향후 더 인기있는 전자 상거래 백엔드를 지원할 계획입니다.
 

## 주목할만한 기능
 

### 국제화
 

v10.0.0부터 Next.js는 국제화 된 라우팅을 기본적으로 지원합니다.
 로케일 목록, 기본 로케일 및 도메인 별 로케일을 입력하기 만하면 Next.js가 자동 라우터 처리를 설정합니다.
 

### 해석학
 

모든 전자 상거래 비즈니스에서는 애플리케이션 성능과 사용자 및 잠재 고객이 애플리케이션 성능을 어떻게 사용하는지 모니터링하는 것이 중요합니다.
 

답변을 원하는 몇 가지 질문은 다음과 같습니다.
 

- 내 앱이 얼마나 빨리로드 되나요?
 
- 반응 형이며 여러 플랫폼에서 일관되게 만족스러운 경험을 제공합니까?
 
- 적절한 시각적 안정성이 있습니까?
 

이러한 주요 지표는 웹 성능 워킹 그룹과 함께 Google에서 설정했습니다.
 

Vercel Analytics는 이러한 측정 항목을 수집하고 실제 경험 점수를 계산하여 애플리케이션의 전반적인 상태와 성능을 강력하게 표시합니다.
 강력한 분석 기능으로 전자 상거래 사이트를 보강하면 애플리케이션 필수 요소의 모든 불규칙성에 대한 경고를받을 수 있으므로 방문자가 빠르고 원활하며 중단없는 경험을 즐길 수 있습니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Vercel-Analytics-Real-Experience-Score.png?resize=730%2C375&ssl=1)

### 이미지 최적화
 

Next.js는 v10.0.0부터 자동 이미지 최적화뿐 아니라 내장 이미지 구성 요소도 제공합니다.
 

next / image 구성 요소는 HTML`<img>`요소를 확장하는 반면 자동 이미지 최적화는 WebP와 같은 최신 파일 형식의 이미지 제공을 지원합니다.
 또한 자동 크기 조정 기능을 제공하여 작은 디스플레이에 적합하도록 매우 큰 이미지를 재조정합니다.
 

이 기능은 CMS와 같은 외부 데이터 원본에서 호스팅하는 이미지를 포함하여 모든 이미지에서 작동합니다.
 이미지는 기본적으로 지연로드되므로 렌더링 속도는 영향을받지 않습니다.
 

### 하이브리드 애플리케이션
 

단일 프로젝트 내에서 서버 측 렌더링과 정적 사이트 생성을 모두 활용할 수 있습니다.
 

### 빠른 새로 고침
 

이 기능을 사용하면 구성 요소 상태를 잃지 않고 React 구성 요소의 변경 사항에 대해 거의 즉각적인 피드백을받을 수 있습니다.
 빠른 새로 고침을 사용하면 (Next.js 9.4 이후 기본값) 대부분의 변경 사항이 1 초 이내에 반영됩니다.
 

### 구성이 필요하지 않습니다.
 

자동 컴파일 및 번들링 기능을 갖춘 Next.js는 처음부터 확장 할 수있는 프로덕션 준비 프레임 워크를 목표로합니다.
 

기타 기능으로는 TypeScript 지원, 내장 CSS 지원, 코드 분할 및 번들링, 간편한 파일 시스템 라우팅, API 엔드 포인트 생성이 있습니다.
 

## Next.js Commerce 시작하기
 

### 전제 조건
 

시작하기 전에 Next.js가 설치되어 있는지 확인하십시오.
 설치되어 있지 않은 경우 다음을 실행하여 설치할 수 있습니다.
 

```coffeescript
npm i next
```

Next.js를 설치하려면 Node.js 10.13 이상이 필요합니다.
 

### 데모 애플리케이션 복제 및 배포
 

Next.js에 발을 담그기 위해 데모 애플리케이션을 복제하고 배포합니다.
 또한 기본 상점 로고를 자체 로고로 맞춤 설정할 것입니다.
 

이 자습서를 마치면 Vercel에서 고유 한 상점 로고가있는 응용 프로그램을 설치하고 실행해야하며 원하는대로 사이트를 사용자 지정하기 위해 구성 요소를 직접 수정할 수 있습니다.
 

#### 먼저 Next.js Commerce 홈페이지로 이동하여 복제 및 배포를 클릭합니다.
 다음과 같은 페이지가 표시됩니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Next.js-Commerce-Clone-Deploy.png?resize=730%2C494&ssl=1)

GitHub, GitLab 또는 Bitbucket 계정으로 로그인합니다.
 또는 이메일 주소로 Vercel 계정을 만들 수 있습니다.
 로그인하면 다음 페이지로 이동합니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Next.JS-Commerce-Homepage-Clone-Deploy.png?resize=730%2C447&ssl=1)

프로젝트 이름을 입력하고 계속을 누르십시오.
 

Next.js Commerce는 현재 BigCommerce 백엔드를 지원하므로 BigCommerce의 타사 서비스를 설치하라는 메시지가 표시됩니다 (하지만 가까운 장래에 모든 주요 전자 상거래 백엔드로 지원을 확장 할 계획 임).
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Install-Next.JS-Integrations-.png?resize=730%2C508&ssl=1)

설치를 클릭하십시오.
 다음이 표시되어야합니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Install-BigCommerce-to-Vercel.png?resize=730%2C550&ssl=1)

기존 BigCommerce Store가있는 경우 로그인을 누르고 자격 증명을 입력하십시오.
 그렇지 않은 경우 제시된 단계에 따라 계정을 등록하십시오.
 

가입 및 / 또는 로그인하면 필수 BigCommerce 통합 옆에 파란색 체크 표시가 표시됩니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/BigCommerce-Install-Complete.png?resize=730%2C601&ssl=1)

계속을 클릭하십시오.
 이제 다음과 같이 Git 저장소를 만들 수 있습니다.
 저장소 이름을 입력하고 계속을 클릭하십시오.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Create-Git-Repository-with-Integrations.png?resize=730%2C543&ssl=1)

다음 화면으로 이동합니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Create-Git-Repository-Import-Project.png?resize=730%2C531&ssl=1)

기본 설정을 그대로두고 배포를 클릭합니다. 이제 기다립니다!
 배포에는 5 분 미만이 소요됩니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Next.js-Import-Project-Deploy.png?resize=730%2C426&ssl=1)

참고로, 화면이 "배포 완료 중…"에서 그보다 오래 멈춘 경우 vercel.com의 개발자 대시 보드로 이동하여 바로 방문 할 수있는 URL로 배포가 완료되었는지 확인하십시오.
 

프로젝트가 배포되면 Vercel 대시 보드에서 프로젝트를 클릭합니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Vercel-Dashboard-After-Deployment-.png?resize=730%2C479&ssl=1)

프로젝트를 클릭하면 프로젝트 개요 페이지를 볼 수 있습니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Product-Deployment-Page-Project-Overview.png?resize=730%2C478&ssl=1)

여기에서 애플리케이션과 연결된 도메인뿐만 아니라 빌드 로그를 볼 수 있습니다.
 지금은 방문을 클릭하기 만하면 앱을 실시간으로 볼 수 있습니다.
 

그리고 거기에서 전자 상거래 상점이 활성화되어 사용자 정의 할 준비가되었습니다!
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Demo-E-commerce-Site-Live-App.png?resize=730%2C411&ssl=1)

이제 빠르게 수정 해 보겠습니다. 기본 스토어 로고를 자체 로고로 변경하겠습니다.
 이 예에서는 의류 아이콘을 사용합니다.
 

선호하는 IDE에서 저장소를 열고`components> ui> Logo`를 엽니 다.
 로고 구성 요소의 원래 모습은 다음과 같습니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/original-logo-component.png?resize=730%2C410&ssl=1)

- `로고`구성 요소를 선택한 이미지로 대체 해 보겠습니다.
 

```coffeescript
const Logo = ({ className = '', ...props }) => <img src={"[<https://icon-library.com/images/icon-clothing/icon-clothing-4.jpg>](<https://icon-library.com/images/icon-clothing/icon-clothing-4.jpg>)"} width="40" height="40" alt="Logo" /> 

16. Push the changes to Git. As soon as you push, Vercel will begin re-deploying your application:
```

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Vercel-Deploy-with-Git-Changes-.png?resize=730%2C261&ssl=1)

그리고 voilà!
 앱이 실행 중이며 자체 로고로 사용자 정의되었습니다.
 

## 결론
 

유사한 맥락에서, 다양한 장바구니, 위시리스트 및 제품 페이지 구성 요소를 포함하여 Next.js Commerce 템플릿에서 사용할 수있는 즉시 사용 가능한 전자 상거래 특정 구성 요소를 사용자 정의 할 수 있습니다.
 

Vercel의 사용하기 쉬운 배포 플랫폼과 결합 된 Next.js Commerce 프레임 워크는 개발자가 BigCommerce 백엔드를 사용하여 원활한 전자 상거래 경험을 구축 할 수 있도록 지원하며 곧 Shopify를 포함한 다른 많은 주요 전자 상거래 백엔드를 지원할 수있게 될 것입니다.
 , Swell, Saleor 등.
 