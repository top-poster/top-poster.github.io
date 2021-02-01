---
layout: post
title: "정적 사이트 생성기와 CDN을 사용하여 블로그를 구축하는 방법
 "
author: 'Code Tower'
thumbnail: https://images.unsplash.com/photo-1432821596592-e2c18b78144f?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MXwxMTc3M3wwfDF8c2VhcmNofDd8fGJsb2d8ZW58MHx8fA&ixlib=rb-1.2.1&q=80&w=2000
tags: undefined
---


새로운 기술을 배우기 위해 재미있는 프로젝트를 만들고 싶었습니다.
 이번에는 SSG (Static Site Generator)에 대해 조금 배우고 싶었습니다.
 

내 목표는 SSG를 사용하여 블로그를 만들고 코드 저장소가 변경 될 때마다 배포하도록하는 것이 었습니다.
 https://caliburnsecurity.com에서 결과를 볼 수 있습니다.
 

## 블로그 요구 사항
 

다음은 내 블로그에서 지원하기를 원하는 것을 결정할 때 함께 설정 한 요구 사항입니다.
 

- 콘텐츠 생성을위한 마크 다운 지원
 
- 구문 강조
 
- 코드 라인 번호 매기기
 
- "서버리스"
 
- 준비된 테마 / 플러그인-저는 프론트 엔드 개발자가 아닙니다.
 
- SEO 기능
 
- 키워드 / 카테고리로 찾아보기
 
- 검색 지원 (정적으로 생성되기 때문에 검색은 Google 색인을 통해 이루어집니다. 동적 JavaScript 기반 검색을 추가하는 방법을 설명하는 다른 기사가 있습니다.)
 
- RSS 지원
 
- 버전 관리
 
- 정적-동적 콘텐츠 없음 (사이트의 공격 표면을 축소하고 보안을 향상시키는 깔끔한 부작용 포함)
 

## 그렇다면 실제로 정적 사이트 생성기는 무엇입니까?
 

간단히 말해 SSG는 웹 사이트를 관리하고 정적 페이지 만 제공하는 사이트로 변환하도록 설계된 프레임 워크입니다.
 

## SSG를 사용하여 블로그를 만드는 이유는 무엇입니까?
 

이해하는 데 시간이 걸리는 한 가지는 이미 CMS가있는 경우 정적 사이트를 사용하는 이유였습니다.
 헤드리스 CMS와 함께 SSG를 사용하는 방법에 대한 온라인 기사가 너무 많았습니다.
 

내가 말할 수있는 가장 좋은 점은 React 또는 Vue와 같은 익숙한 프레임 워크를 사용하는 동시에 CMS를 사용하여 모든 콘텐츠를 처리 할 수 있다는 것입니다.
 

그러나 나는 결코 프론트 엔드 개발자가 아니기 때문에이 프로젝트를 폐기 할 뻔했습니다.
 저는 "오, 저는 Ghost를 사용해야합니다. DigitalOcean과 같은 플랫폼에서 호스팅되는 경우 월 5 달러이며 콘텐츠를 제공하고 콘텐츠를 관리하는 올인원 플랫폼입니다."라고 생각했습니다.
 

그러나 저는 정말 새로운 것을 배우고 싶었고 Markdown 만 사용하여 블로그를 무료로 배포 할 수 있는지 확인하고 싶었습니다.
 

항상 그렇듯이, 제가 바랬던 일이 몇 시간이 걸리 길 바라던 일이 다양한 기술에 대한 연구의 토끼 굴을 따라 내려 가면서 상당히 시간이 더 걸렸습니다.
 

나는 다음과 같은 몇 가지 다른 기술을 가지고 놀았습니다. (모두 SSG는 아니지만 나중에 자세히 설명합니다) :
 

- Pelican (Python)
 
- 휴고 (Go)
 
- 헥소
 
- Gridsome (Vue-JS)
 
- VuePress (Vue-JS)
 
- 유령
 
- 개츠비 (React-JS)
 
- 지킬 (루비)
 

### 내가 휴고를 선택한 이유
 

내가 본 모든 기술에 대해 너무 자세하게 설명하지는 않겠습니다.
 그러나 일반적으로 Hugo는 설정 및 구축이 매우 빠르며 모든 옵션 중에서 가장 단순하다는 것을 알았습니다.
 

이것이 Jekyll과 유사하다는 것을 알고 있지만, Ruby 환경을 구성하는 것을 정말로 다루고 싶지 않았고 Hugo의 속도는 다른 모든 것을 먼지 속에 남겨 두었습니다.
 

## 블로그 구축을 시작하는 방법
 

이 연습을 위해 Netlify에서 호스팅하는 정적 블로그 (무료!)를 만들어 보겠습니다.
 

참고 :이 자습서에서는 Windows 상자에서 PowerShell을 사용할 것이므로 복사 / 붙여 넣기를 수행하는 경우이를 기억하십시오.
 

### 다음 종속성이 있는지 확인하십시오.
 

- 힘내
 
- Visual Studio Code (또는 선호하는 편집기)
 
- 휴고 바이너리
 

### 다음은 워크 플로에 대한 높은 수준의 개요입니다.
 

- Hugo 다운로드 / 설치
 
- Hugo 프로젝트 만들기
 
- 테마 추가 및 구성
 
- Git에 추가
 
- Netlify에 배포
 
- (선택 사항) 무료 Netlify CMS 구성
 

### Hugo 다운로드 또는 설치
 

Hugo를 설치하기 위해 GitHub 릴리스 페이지로 이동하여 독립 실행 형 Windows x64 바이너리를 다운로드했습니다.
 우리 사이트를 만들 프로젝트 디렉토리에 배치했습니다 (항상 적절하게 설치하거나 PATH에 바이너리를 추가 할 수 있지만 빨리 원했습니다).
 

## Hugo 사이트를 만드는 방법
 

새 사이트를 만들려면 아래 명령을 실행하면됩니다.
 

```powershell
.\hugo.exe new site hugo-blog
mv .\hugo.exe .\hugo-blog
cd .\hugo-blog
.\hugo.exe server -D --gc

```

이제 프로젝트가 생성되었으며 방금 Hugo 서버를 시작했습니다.
 -D 플래그를 사용하여 Hugo에게 초안 콘텐츠를 표시하도록 지시했으며, 일반적으로 캐시를 지워 매번 정리가 실행되도록하기 위해 --gc를 추가합니다.
 

http : // localhost : 1313에서 사이트에 액세스 할 수 있습니다.
 

### 디렉토리 구조 이해
 

이제 다음 디렉토리 구조가 표시됩니다.
 

```undefined
|__archetypes
|__assets *this will not show up by default
|__config *this will not show up by default
|__content
|__data
|__layouts
|__static
|__themes
|__config.toml

```

- 아키 타입 : 사전 구성된 서문이있는 콘텐츠 템플릿 파일.
 우리는 이것을 실제로 만지지 않을 것입니다.
 
- 자산 : Hugo Pipes에서 처리 한 모든 파일을 저장합니다.
 이것은이 자습서의 범위를 벗어납니다.
 
- config : 구성 파일을 저장할 선택적 디렉토리입니다.
 우리는 너무 미친 짓을하지 않을 것이므로 기본 config.toml 파일을 사용합니다.
 
- 콘텐츠 : 콘텐츠가있는 곳, 즉 게시물과 페이지입니다.
 이 디렉토리 내의 최상위 폴더는 섹션으로 처리됩니다.
 
- 데이터 : Hugo에서 사용하는 구성 파일이 포함되어 있습니다.
 이 디렉토리를 건드릴 필요가 없었습니다.
 
- 레이아웃 : 사이트에 대한 부분 또는 전체 페이지 HTML 템플릿을 저장합니다.
 여기에있는 모든 항목은 테마의 해당 항목을 덮어 쓸 수 있으므로 테마의 실제 파일을 수정하지 않고도 테마를 사용자 지정할 수 있습니다.
 
- static : 이미지, CSS 또는 스크립트와 같은 정적 콘텐츠를 저장합니다.
 이 폴더의 모든 내용은 Hugo의 수정이나 해석없이 그대로 복사됩니다.
 블로그 게시물에서 참조 할 수 있도록 이미지와 같은 미디어를 여기에 저장합니다.
 
- 테마 : Hugo 테마를 설치할 디렉토리입니다.
 
- config.toml : 기본 사이트 구성입니다.
 서로 다른 환경을 분리하려는 경우 별도의 디렉토리를 사용할 수 있습니다.
 

## 첫 번째 테마를 추가하는 방법
 

이 블로그에서는 Hugo의 이야기 테마를 사용합니다.
 프로젝트의 루트에서 다음 명령을 실행합니다.
 

```powershell
git submodule add https://github.com/EmielH/tale-hugo.git .\themes\tale

```

우리는 테마의 파일을 편집하지 않을 것이지만 위에서 설명한 레이아웃 폴더에서 모든 수정을 수행합니다.
 이렇게하면 변경 사항을 덮어 쓸 염려없이 테마를 업데이트하기 위해 항상 하위 모듈을 업데이트 할 수 있습니다.
 

테마를 초기화하려면 루트 디렉토리에서 config.toml을 편집하고 다음 행을 추가하십시오 (기본값도 편집 함).
 

```toml
# Theme Settings
theme = "tale"
[params]
  Author = "Aaron Katz" # Add the name of the author (this theme only supports one author)
[author]
  name = "Caliburn Security" # Used by the foot copyright

```

이제 테마가 활성화되었습니다!
 (대부분의 경우 테마는 테마의 theme.toml 파일을 config.toml에 복사하여 붙여 넣어야합니다.)
 

계속해서 페이지를 확인하세요. 파일을 저장할 때마다 Hugo는 사이트를 실시간으로 재 구축합니다.
 

### 테마 파일 수정 방법
 

현재 테마의 한 가지 문제는 게시물이 아닌 콘텐츠가 홈페이지 목록에 표시된다는 것입니다.
 

이를 변경하려면. \ themes \ tale \ layouts \ index.html 페이지를. \ layouts \ index.html로 복사 해 보겠습니다.
 

거기에서`{{range (.Paginate .Site.RegularPages) .Pages}}`를`{{range where .Paginator.Pages "Section" "post"}}`로 바꿉니다.
 이렇게하면 "게시물"섹션 만 목록에 표시됩니다.
 

또한 간단한 바닥 글을 추가하고 싶었으므로. \ layouts \ footer.html에 새 파일을 만들고 다음 코드를 추가합니다.
 

```html
<footer>
    <span>
    &copy; <time datetime="{{ now }}">{{ now.Format "2006" }}</time> {{ .Site.Author.name }}
    </span>
</footer>
```

### Google Analytics를 추가하는 방법
 

또한 블로그에 Google Analytics를 추가하고 싶었는데 테마에이 기능이 포함되어 있지 않다는 것을 알았습니다.
 

운 좋게도 Hugo는 분석을 매우 간단하게 추가합니다.
 config.toml 파일을 열고 다음 행을 추가하십시오.
 

```toml
googleAnalytics = "" # The UA-XXX number from Google Analytics

```

구성이 저장되면. \ themes \ tale \ layouts \ partial \ head.html 파일을. \ layouts \ partial \ head.html에 복사하고 head 태그 바로 아래에 다음 코드를 추가합니다.
 

```go
{{ template "_internal/google_analytics_async.html" . }}

```

이제 Google Analytics가 작동합니다.
 멋있는!
 

## 콘텐츠 작성 방법
 

사람들이 나에 대한 모든 것을 알 수 있도록 멋진 정보 페이지를 추가합시다.
 :)
 

```powershell
.\hugo.exe new about.md

```

이 페이지가 기본 메뉴에 추가되었는지 확인하려면 페이지의 머리말에`menu : main` 행을 추가하십시오.
 

참고 :. \ public 폴더에 콘텐츠를 생성하는 Hugo를 빌드하려면`. \ hugo.exe`를 실행하면됩니다.
 

### 머리말은 무엇입니까?
 

이것은 저에게 새로운 용어였습니다.
 기본적으로 서문은 콘텐츠에 대한 구조화 된 메타 데이터입니다.
 

기본적으로 템플릿은 생성하는 각 페이지 또는 게시물에 다음 메타 데이터 필드를 추가합니다.
 

- 표제
 
- 데이트
 verified_user
- 초안
 verified_user

잠재적으로 유용한 기타 요소는 다음과 같습니다.
 

- 설명-콘텐츠에 대한 설명을 입력 할 수 있습니다.
 
- expiryDate-콘텐츠가 더 이상 게시되지 않아야하는 날짜 / 시간을 설정합니다.
 
- 키워드-콘텐츠에 대한 메타 키워드입니다.
 
- lastmod-콘텐츠가 마지막으로 수정 된 날짜 시간 (enableGitInfo 구성 명령을 사용하는 경우 파일이 Git에서 마지막으로 수정 된 시간으로 자동 설정 됨)
 
- markup- "rst"로 설정하면 Markdown 대신 reStructuredText를 사용할 수 있습니다 (이 기능은 시험용입니다).
 
- publishDate-콘텐츠를 표시 할 미래 날짜를 설정합니다.
 
- slug-출력 URL의 꼬리.
 지정되지 않은 경우 기본값은 파일 이름입니다.
 
- 요약-기사 요약을 제공 할 때 사용되는 텍스트입니다.
 일반적인 기본값 인 요약에 첫 번째 단락을 표시하지 않으려는 경우 유용합니다.
 
- <taxonomies>-태그 또는 카테고리와 같은 분류 색인의 복수 형식을 사용합니다.
 

계속해서 블로그 게시물에 표시되는 기본 머리말을 변경해 보겠습니다.
 archetypes 폴더에서 posts.md라는 새 파일을 만들고 다음을 추가합니다.
 

```yaml
---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: true

slug: {{ .File.BaseFileName }} # Will take the filename as the slug. Feel free to change this to any format you like.  I like including this, so that I remind myself I have the option to change if I want.

summary: "" # Remove this if you want Hugo to just use the first 70 (configurable) characters of the post as the summary.
description: ""

# Lists
keywords:
tags:
categories:
---

```

이제`. \ hugo.exe`로 하나의 최종 빌드를 수행하고 Git 저장소를 구성 할 준비를합니다.
 

## Git 구성 방법
 

Git 용 프로젝트를 구성 할 시간 :
 

```powershell
git init
git remote add origin <YOUR GIT URL>
git push -u origin master

```

이제 저장소에 코드가 저장되었으며 배포 할 준비가되었습니다!
 

## 블로그를 배포하는 방법
 

이제 사이트가 Git에 저장되었으므로 배포 할 차례입니다!
 거의 완료되었습니다. 이제 10 분 이내에 Netlify에 배포하는 방법을 보여 드리겠습니다.
 

먼저 Netlify 애플리케이션을 만들어야합니다 (사용 가능한 모든 방법을 사용하여 계정을 만들 수 있습니다).
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-192.png)

다음으로 사이트를 만들고 Hugo 콘텐츠를위한 Git 저장소의 위치를 알려줘야합니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-193.png)

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-194.png)

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-195.png)

다음으로 사이트에 대한 사용자 지정 도메인을 설정합니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-196.png)

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-197.png)

이제 DNS 구성 확인 메시지와 함께 도메인이 표시됩니다.
 이를 클릭하고 제공된 DNS 레코드 정보를 DNS 레코드를 관리하는 서비스 제공 업체에 입력하십시오.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-198.png)

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-200.png)

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-201.png)

완료되면 DNS 설정이 전파 될 때까지 몇 분 정도 기다린 다음 DNS 구성 확인을 선택합니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-202.png)

이제 귀하의 사이트가 활성화되었습니다!
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-203.png)

마지막으로 사이트에 대한 SSL을 모범 사례로 설정해야합니다.
 Netlify는 Let `s Encrypt를 사용하여 애플리케이션에 대한 인증서를 자동으로 프로비저닝하는 옵션을 제공합니다.
 이렇게하려면 인증서 제공을 선택하면됩니다.
 

![image](https://www.freecodecamp.org/news/content/images/2021/01/image-204.png)

참고 : 인증서가 생성되는 데 시간이 꽤 걸릴 수 있으므로 잠시만 기다려주십시오.
 

### Netlify 배포 설정
 

Netlify를 진정으로 사용할 준비가되기 전에 마지막 단계가 있습니다.
 안타깝게도 Netlify에서 사용하는 Hugo 버전은 기본적으로 다소 구식입니다.
 그러나 Netlify가 사이트를 배포 할 때 따를 수 있도록 자체 구성을 만들어이 문제를 해결할 수 있습니다.
 

먼저 저장소의 루트에`netlify.toml`이라는 파일을 만들고 다음 구성을 추가합니다.
 

```toml
[build]
publish = "public"
command = "hugo --gc --minify"

[context.production.environment]
HUGO_VERSION = "0.74.3"
HUGO_ENV = "production"
HUGO_ENABLEGITINFO = "true"

[context.split1]
command = "hugo --gc --minify --enableGitInfo"

[context.split1.environment]
HUGO_VERSION = "0.74.3"
HUGO_ENV = "production"

[context.deploy-preview]
command = "hugo --gc --minify --buildFuture -b $DEPLOY_PRIME_URL"

[context.deploy-preview.environment]
HUGO_VERSION = "0.74.3"

[context.branch-deploy]
command = "hugo --gc --minify -b $DEPLOY_PRIME_URL"

[context.branch-deploy.environment]
HUGO_VERSION = "0.74.3"

[context.next.environment]
HUGO_ENABLEGITINFO = "true"
```

이제 남은 일은 Netlify 콘솔에서 사이트 배포를 선택하는 것뿐입니다. 이제 사이트가 SSL을 사용하는 사용자 지정 도메인에 있습니다!
 

## 마무리
 

아휴!
 긴 블로그 게시물 이었지만 "서버리스"블로그를 시작하고 실행하는 것이 얼마나 빠른지 보여줄 수 있기를 바랍니다.
 내가 배운 것을 보자 :)
 

### 내가 사랑하는 것
 

- 빌드가 매우 간단하고`hugo 서버`만 실행하면됩니다.
 
- 실시간 새로 고침-변경하고 저장하면 페이지가 새로 고침됩니다.
 
- 일반적으로 간단합니다. grunt, gulp, webpack 등을 다룰 필요가 없었습니다.
 
- 사용자 정의 가능한 출력 형식을 사용하면 정적 사이트는 물론 Google AMP 사이트, JSON 파일 등을 생성 할 수 있습니다.
 
- 빠른.
 빨리 언급 했나요?
 
- Netlify (현재 선택), Amazon S3 및 Cloudfront, Heroku, GitHub Pages 등을 사용하여 거의 모든 곳에 배포 할 수 있습니다.
 
- Markdown이 충분하지 않은 경우 단축 코드를 사용할 수 있습니다.
 
- 연속 배포-모든 것이 버전 제어되며 마스터 브랜치에 게시 할 때 배포됩니다.
 
- 댓글 및 게시물 공유 허용
 

### 도전
 

- 휴고는 때때로 너무 단순합니다.
 플러그인이나 확장 프로그램이 전혀 없습니다.
 
- Go 사용은 덜 직관적이며 Vue와 같은 것보다 단축 코드가 더 지저분합니다.
 
- 사용 가능한 테마가 너무 많지는 않지만 매우 활동적인 사용자 기반이 있기 때문에 라이브러리가 계속 성장할 것으로 예상합니다.
 

### 그래서 CMS가 필요합니까?
 

이 모든 후에도 여전히이 질문이 마음 속에 남아있었습니다.
 대답은 "상황에 따라 다릅니다"입니다.
 

업로드해야하는 이미지 나 비디오와 같은 많은 미디어를 통합한다면 확실히 이미지 폴더에 모든 미디어를 정적으로 추가하고 구성하는 것이 지루할 것입니다.
 

이때 Markdown을 사용하여 게시물을 작성할 수있는 한 Ghost, Netlify 또는 Sanity와 같은 헤드리스 CMS를 조사하여 콘텐츠를 관리했습니다.
 

### 참고 문헌
 

- https://medium.com/backticks-tildes/hugo101-getting-started-with-hugo-and-deploying-to-netlify-9a813fe23b94
 launch
- https://blog.risingstack.com/static-site-generator-hugo-netlify/
 launch
- http://cloudywithachanceofdevops.com/posts/2018/05/17/setting-up-google-analytics-on-hugo/
 launch
- https://www.sitepoint.com/premium/books/a-beginner-s-guide-to-creating-a-static-website-with-hugo/read/1
 launch