---
layout: post
title: "개츠비.js 블로그에 의견을 추가할 수 있는 플러그인 4개"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/12/4pluginstoaddcommentstogatsbyblog.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/4pluginstoaddcommentstogatsbyblog.png?fit=730%2C487&ssl=1)

당신의 블로그에 댓글 달면 많은 이점이 있습니다. 독자와 대화하고 글에 대한 피드백을 받을 수 있는 기회를 제공합니다. 또한 사용자 생성 콘텐츠로 인해 SEO를 개선합니다.

많은 유료 댓글 플랫폼이 있지만, 이번 게시물에서는 무료로 사용할 수 있는 플러그인에 초점을 맞출 것입니다.

이 기사에서는 적은 구성만으로 원활하게 의견을 개츠비 블로그에 통합할 수 있는 방법에 대해 설명합니다. 아래 4가지 플러그인 중 하나를 사용하여 다음과 같습니다.

- 디스커스
- 깃토크
- 설명 상자
- 그래프 주석

## 1. 디스커스

Discus는 구성이 거의 없는 블로그에서 쉽게 주석을 추가하고 관리하며 중간 정도의 주석을 추가할 수 있는 인기 있는 타사 주석 플러그인입니다.

### 프로스

- Discus 계정 또는 소셜 로그인을 사용한 강력한 인증 옵션
- 쉽게 사용자 정의할 수 있고 사이트 테마에 맞게 조정
- 블로그 페이지 또는 관리 대시보드에서 직접 주석 관리
- 머신 러닝을 이용한 스팸 자동 조정
- 미디어(이미지, 비디오 및 gif)를 주석에 포함할 수 있음

### 콘스

- 비동기식으로 로드되지 않고 종속성이 많기 때문에 페이지 로드 속도가 느려지는 경우가 있습니다.

## 디스커스가 작동 중임

개츠비에 대한 디스커스를 시작하려면 먼저 계정을 등록하고 `내 사이트에 디스커스를 설치하고 싶다`를 클릭해야 한다. 다음 페이지에서 다음 단계에서 사용할 웹 사이트 이름을 입력하고 참고하십시오.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/website-name-field.png?resize=730%2C578&ssl=1)

다음으로 `gats by-plugin-discus`를 설치합니다.

```undefined
npm install gatsby-plugin-disqus --save
```

아니면

```undefined
yarn add gatsby-plugin-disqus
```

그런 다음 `gatsby-config.js`에서 플러그인을 추가하고 구성하십시오.

```js
// gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-plugin-disqus`,
      options: {
        shortname: `codewithlinda`
      }
    },
  ]
}
```

단축 이름은 이전 단계에서 제공한 웹 사이트 이름과 일치해야 합니다.

다음 단계는 Discus 주석 구성 요소를 블로그 페이지 템플릿 파일에 추가하는 것입니다.

```js
import Disqus from 'gatsby-plugin-disqus'

const PostTemplate = () => (
  <>
    /* Page Contents */
    <Disqus 
      identifier={post.id}
      title={post.title}
      url={`${config.siteUrl}${location.pathname}`}
    />
  </>
)

export default PostTemplate
```

그래프QL 쿼리의 구조에 따라 ID, 제목 및 경로 URL을 제공합니다. 주석 스레드를 해당 블로그 게시물에 올바르게 연결하는 데 도움이 됩니다. 사이트 URL을 배포하고 탐색합니다. 이제 디스크 설명이 사용 가능해야 합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/disqus-comments-activated.png?resize=730%2C429&ssl=1)

## 2. GitTalk

GitTalk는 GitHub 이슈와 Preact를 기반으로 한 댓글 구성 요소입니다. 사용자 인증은 GitHub를 통해 이루어지므로 대부분의 독자가 이미 GitHub 계정을 가지고 있기 때문에 기술 블로그에 적합합니다.

### 프로스

- 성능 저하 없음. GitTalk는 서버가 없으므로 설명이 빠르게 로드됩니다.
- 모든 코멘트는 GitHubrepo에 저장되므로 모든 데이터를 제어할 수 있으며 원할 때마다 쉽게 마이그레이션할 수 있습니다.
- 여러 언어를 지원합니다.

### 콘스

- GitHub 계정이 있는 사용자만 사용 가능
- 기본 제공 중재가 없습니다. 즉, 블로그 페이지에서 주석을 제거하거나 비활성화할 수 없으며 GitHub 발행 스레드에서 주석을 수동으로 삭제해야 합니다.

## GitTalk 실행 중

GitTalk를 개츠비 블로그에 통합하기 위해 Gatsby-plugin-gitalk를 사용할 것이다.

먼저 플러그인을 설치하십시오.

```undefined
npm install --save gatsby-plugin-gitalk
```

아니면

```undefined
yarn add gatsby-plugin-gitalk
```

그런 다음 블로그에 새 GitHub Oauth 응용프로그램을 등록하여 인증 및 인증을 사용 가능으로 설정합니다. 무엇이든 될 수 있는 응용프로그램 이름, 블로그의 URL이 되어야 하는 홈페이지 URL, 설명 및 블로그의 URL이 되어야 하는 권한 콜백 URL을 제공합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/authorization-callback-url-field.png?resize=730%2C602&ssl=1)

`응용 프로그램 등록`을 클릭하여 다음 단계에서 사용할 클라이언트 ID와 클라이언트 암호를 생성합니다.

다음으로 `gatsby-config.js`에서 플러그인을 추가하고 구성합니다.

```coffeescript
plugins: [
    {
      resolve: `gatsby-plugin-gitalk`,
      options: {
        config: {
          clientID: 'f16d485a306b836cabd1',
          clientSecret: '6ee5e2a6c2a4992fc49aeab2740e6493bbc9cfae',
          repo: 'gatsby-demo-comments',
          owner: 'Linda-Ikechukwu',
          admin: ['Linda-Ikechukwu']
        }

      }
    },
  ]
```

구성 옵션에서 `클라이언트`ID와 clientSecret은 이전 단계의 값이다. repo는 코멘트 문제를 제출하고자 하는 레포의 이름이며, 가급적이면 블로그의 코드가 들어 있는 레포입니다. 소유자 구성은 GitHub 사용자 이름이고, admin은 보고서 소유자 및 공동작업자의 배열입니다. 여기서 찾을 수 있는 다른 선택적 구성 매개 변수가 있습니다.

마지막으로 블로그 게시물 템플릿 파일에 주석 구성 요소를 추가하십시오.

```js
//in template/blog-post.js
import Gitalk from 'gatsby-plugin-gitalk'
import '@suziwen/gitalk/dist/gitalk.css'

const PostTemplate = () => {
  let gitalkConfig = {
    id: post.slug || post.id,
    title:post.frontmatter.title,
  }
  return (
     <Gitalk options={gitalkConfig}/>
  )
}

export default PostTemplate
```

CSS 파일에는 플러그인에 대한 스타일이 포함되어 있습니다. `node_modules/@suziwen/gitalk/dist/gitalk.css`의 내용을 복사하여 수정하고 대체품으로 가져올 수 있습니다. 그래프QL 쿼리의 구조에 따라 주석 구성 요소의 ID와 제목을 제공합니다.

특정 기사에 대한 첫 번째 코멘트가 제출되면 GitTalk는 구성에 제공된 제목을 이슈 제목으로 열고 해당 기사에 대한 코멘트를 해당 이슈에 첨부합니다. GitHub 이슈에 대한 설명을 삭제하여 페이지에서 설명을 삭제할 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/deleting-comment-on-github-issue.png?resize=730%2C426&ssl=1)

사이트 URL을 테스트, 배포 및 방문하려면 주석 초기화를 클릭하면 사이트에서 주석을 사용할 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/commenting-activated-example.png?resize=730%2C423&ssl=1)

## 3. 댓글창

CommentBox는 매월 100개의 의견을 무료로 제공하는 개인 정보 중심의 타사 주석 솔루션입니다.

### 프로스

- 소셜 또는 이메일 로그인을 통한 인증
- 익명 코멘트 사용 가능
- 블로그 페이지 또는 관리 대시보드에서 직접 주석 관리
- Discus에서 주석 가져오기 및 마이그레이션

### 콘스

- 매월 100개의 코멘트로 구성된 제한된 무료 계층, 그 이후에는 $15를 지불해야 합니다.

## 설명 상자 실행 중

CommentBox를 시작하려면 계정을 만들어야 합니다. https 및 www 없이 웹 사이트 도메인을 입력합니다. 그 후에 프로젝트 ID가 생성됩니다. 향후 단계에서 사용될 예정이므로 유의하십시오.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/basic-info-commentbox.png?resize=730%2C523&ssl=1)

그런 다음 npm을 통해 CommentBox를 설치합니다.

```undefined
npm install commentbox.io --save
```

그런 다음 블로그 페이지 템플릿 파일에 주석 구성 요소를 추가하십시오.

```coffeescript
import commentBox from 'commentbox.io';
const BlogPostTemplate = ()=>{
  useEffect(() =>{
    commentBox('5632596591509504-proj')
  },[])

  return(
     <div className="commentbox" />
  )

}
```

`commentBox` 함수에 제공된 인수는 이전 단계에서 생성된 프로젝트 ID여야 합니다. 기본적으로 설명은 자동으로 승인되지만 대시보드에서 수동으로 승인하도록 선택할 수도 있습니다.

설명 플러그인의 테마를 변경하려면 다음 매개 변수를 가진 개체를 `commentBox` 함수에 제공하십시오.

```coffeescript
commentBox('5632596591509504-proj', {
   backgroundColor: null,  // default transparent
   textColor: null,  // default black
    subtextColor: null,  // default grey
})
```

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/different-theme-comment.png?resize=730%2C475&ssl=1)

## 4. 그래프 주석

Graph Comments는 단순한 주석 플랫폼 그 이상입니다. 블로그에 바로 내장된 커뮤니티 시스템입니다.

### 프로스

- 소셜 또는 이메일 로그인을 통한 인증
- 익명 댓글. 관리 대시보드에서 승인됨
- 푸시 알림을 사용할 수 있습니다. 사용자가 응답 또는 의견의 업보트를 받을 경우 사용자에게 알립니다.
- 미디어(이미지, 비디오 및 GIF)를 주석에 포함할 수 있음
- 블로그 페이지 또는 관리 대시보드에서 직접 주석 관리
- 사용자가 주석을 편집할 수 있음

### 콘스

- 월 최대 100만 개의 데이터 로드가 있는 사용 가능한 계층 제한 즉, 사이트의 월 총 페이지 뷰 수는 100만 개로 제한되며, 이후 나머지 기간 동안 그래프 설명이 자동으로 비활성화됩니다. 트래픽이 최소화된 사이트의 경우 이 문제는 걱정할 필요가 없습니다.

## 그래프 주석 실행 중

시작하려면 먼저 대시보드에 새 사이트를 등록하고 추가합니다. 향후 단계에서 사용할 고유 ID를 기록해 두십시오.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/unique-id-field.png?resize=730%2C505&ssl=1)

그런 다음 대시보드로 리디렉션됩니다. My site > your site name > setup을 클릭하여 설치 스크립트를 찾습니다.

`react-inline-script` 또는 `use Effect`를 사용하여 블로그 게시물 템플릿 파일에 스크립트를 추가할 수 있습니다.

```coffeescript
import Script from "react-inline-script"

<div id="graphcomment"></div>
      <Script>
        {`
          window.gc_params = {
              graphcomment_id: 'codewithlinda',
              fixed_header_height: 0,
          };

          (function() {
            var gc = document.createElement('script'); gc.type = 'text/javascript'; gc.async = true;
            gc.src = 'https://graphcomment.com/js/integration.js?' + Math.round(Math.random() * 1e8);
            (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(gc);
          })();
        `}
      </Script>
```

아니면

```js
useEffect(() => {
    window.gc_params = {
      graphcomment_id: 'codewithlinda',
      fixed_header_height: 0,
   };

  (function() {
    var gc = document.createElement('script'); gc.type = 'text/javascript'; gc.async = true;
    gc.src = 'https://graphcomment.com/js/integration.js?' + Math.round(Math.random() * 1e8);
    (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(gc);
  })();
 }, [])
```

`graphcomment_id` 값이 이전 단계에서 지정한 값과 일치하는지 확인하십시오. 바로 그거예요. 이제 그래프 설명이 블로그 페이지에 삽입됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/graphcomments-enabled-example.png?resize=730%2C574&ssl=1)

## 결론

이 기사에서는 디스커스, 코멘트박스, 깃톡, 그래프 코멘트 등 4가지 플러그인을 사용하여 개츠비 블로그에 대한 코멘트를 설정하는 방법에 대해 논의하고 설명했습니다.

통제되는 것을 좋아하고 하나의 GitHub 계정에서 블로그, 코드, 기사 및 코멘트에 대한 모든 것을 관리하려면 GitTalk로 이동합니다. 타사 도구(댓글 콘텐츠를 소유하고 있을 가능성이 있음)에 개의치 않고 대부분의 웹 사용자가 익숙하고 이미 계정을 갖고 있을 수 있는 도구를 계속 사용하고 싶다면 Discus를 사용하십시오. Disquus의 성능 단점에 신경 쓰지 않으려면 GraphComments와 CommentBox 모두 멋진 무료 계층 제품과 깔끔한 인터페이스를 갖추고 있어야 합니다.

결국, 어떤 코멘트 플러그인을 완전히 사용할지 선택하는 것은 당신에게 달려 있다. 위에서 언급한 4개의 플러그인은 모두 훌륭하지만, 다른 옵션을 사용할 수 있습니다. 탐색하고, 자신에게 적합한 것을 찾고, 사용합니다.

그래서, 어떤 코멘트 플러그인이 더 좋으세요? 얼마든지 댓글을 달아주세요.