---
layout: post
title: "Blazor 대 반응
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/blazor-react.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/blazor-react.png?fit=730%2C487&ssl=1)

SPA (단일 페이지 애플리케이션)를 빌드 할 때 선택할 수있는 몇 가지 프레임 워크가 있습니다.
 

가장 인기있는 3 가지는 React, Vue, Angular입니다.
 이 세 가지 프레임 워크로 SPA를 구축하려면 JavaScript가 필요합니다.
 

개발자가 SPA 구축에 관심이 있지만 JavaScript의 이상 함을 다루고 싶지 않으면 어떻게됩니까?
 알아 보자.
 

## Blazor가 온다
 

Blazor는 C # .NET 및 WebAssembly 프레임 워크를 활용하여 웹 브라우저에서 실행되는 SPA를 만드는 고유 한 접근 방식을 사용하는 새로운 Microsoft UI 프레임 워크입니다.
 기본적으로 개발자는 HTML, CSS 및 C #을 사용하여 풍부한 대화 형 클라이언트 측 애플리케이션을 빌드 할 수 있습니다.
 

반면에 사용자 인터페이스 및 UI 구성 요소를 구축하기위한 선언적이고 효율적이며 유연한 JavaScript 라이브러리 인 React가 있습니다.
 

React와 Blazor는 풍부한 대화 형 최신 클라이언트 측 애플리케이션을 구축하기위한 클라이언트 측 프레임 워크 / 라이브러리라는 유사점을 공유합니다.
 

Blazor의 독특한 점 중 하나는 JavaScript 상호 운용성입니다.
 즉, Blazor 앱은 .NET 메서드에서 JavaScript 함수를 호출하고 JavaScript 함수에서 .NET 메서드를 호출 할 수 있습니다.
 

> Blazor는 Blazor WebAssembly (클라이언트 측)와 Blazor 서버 (서버 측)의 두 가지 주요 프로젝트로 구성됩니다.
 이 기사에서는 Blazor WebAssembly와 React를 비교하는 데 중점을 둡니다.
 

자세히 알아보기 전에 긴급한 질문에 답해 봅시다.
 

### 브라우저에서 실행되는 C #?
 

여기서 큰 질문은 웹 브라우저에서 C #이 어떻게 실행됩니까?
 이를 가능하게하는 기술을 데스크톱 및 모바일 플랫폼의 현재 브라우저에서 직접 지원하는 개방형 표준 인 WebAssembly라고합니다.
 일반 JavaScript 또는 React 대신 C # 코드를 작성하면 컴파일 된 코드가 최신 웹 브라우저에서 기본적으로 실행됩니다.
 

## 시작하기
 verified_user

이 기사에서는 다음 영역에서 React와 Blazor가 어떻게 다른지 살펴볼 것입니다.
 

- 폴더 구조
 
- 프로그래밍 언어
 verified_user
- 템플릿
 
- 공연
 
- 생태계
 
- 패키지 관리자
 
- 구성 요소 간의 통신
 
- 라우팅
 
- HTTP 다루기
 

### 폴더 구조
 

Blazor 앱과 React 앱을 만들고 바로 사용할 수있는 것과 정면으로 비교해 보겠습니다.
 

새 Blazor 프로젝트를 만들려면 .NET SDK를 다운로드 및 설치하고 터미널에서 다음 명령을 실행해야합니다.
 

`dotnet 새로운 blazorwasm -n BlazorApp`
 

새로운`blazorwasm` 명령은 WebAssembly를 사용하여 새 Blazor 프로젝트를 만들고 있음을 의미하며`-n` 플래그는 프로젝트 이름을 나타냅니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/blazor-webassembly-folder-structure.png?resize=235%2C458&ssl=1)

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/programs.cs-file.png?resize=730%2C500&ssl=1)

`program.cs` 파일에는 WebAssembly 앱을 시작하고 실행하는 데 필요한 주요 메서드가 포함되어 있습니다.
 

또한 앱을 마운트하고 ID가 "app"인 태그를 선택하고 있습니다.
 또한 HTTP 클라이언트는 종속성 주입 기술을 사용하여로드됩니다.
 

> Blazor 앱의 HTTP 모듈은 JavaScript Fetch API를 기반으로 빌드됩니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/create-react-app-folder-structure.png?resize=182%2C413&ssl=1)

React.js 앱의`index.js`는 Blazor 앱의`program.cs`와 유사합니다.
 여기서 React DOM은 앱 구성 요소를 렌더링하고 ID가 "root"인 요소가 루트 구성 요소로 선택됩니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/index.js-in-react-app.png?resize=730%2C510&ssl=1)

### 프로그래밍 언어
 verified_user

Blazor는 JavaScript 대신 프로그래밍 언어로 C #을 사용하고 .NET 라이브러리의 기존 .NET 에코 시스템을 활용합니다.
 C #을 사용하여 백엔드 코드를 작성할 수있는 것에서 이제 프로그래밍 언어로 C #을 사용하여 풀 스택 웹 및 모바일 애플리케이션을 빌드 할 수있는 시점까지 기술을 확장 할 수 있기 때문에 C # 사용자에게는 확실히 장점입니다.
 

> 단점은 C #이 JavaScript 개발자에게 부자연스러워 보이고 느껴질 수 있다는 것입니다.
 그러나 C #을 잘 이해하면 C #으로 작성된 강력한 풀 스택 애플리케이션을 빌드 할 수 있습니다.
 

반면 React는 JavaScript를 프로그래밍 언어로 사용합니다.
 따라서 본질적으로 웹 개발자는 여전히 가장 익숙한 언어를 작성할 수 있습니다.
 그리고 React Native를 사용하면 동일한 코드 스 니펫을 공유하는 웹, Android 및 iOS 애플리케이션을 빌드 할 수 있습니다.
 

### 템플릿
 

React로 SPA를 생성 할 때 Create React App Toolchain을 권장합니다.
 기본적으로 JavaScript의 구문 확장 인 JSX로 구성된 React App을 초기화합니다.
 React에서 HTML을 작성하고 HTML로 JavaScript를 작성할 수있는 템플릿 엔진처럼 작동합니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/jsx-in-a-react-app.png?resize=730%2C644&ssl=1)

Blazor는 수년 동안 사용되어 온 Razor 템플릿 엔진을 사용합니다.
 템플릿 엔진은 C # 및 ASP.NET 에코 시스템에서 새로운 것이 아닙니다.
 C # 및 ASP.Net을 사용하여 웹 페이지에 서버 측 코드를 삽입하는 데 사용됩니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/razor-template-engine-in-a-blazor-app.png?resize=730%2C876&ssl=1)

> JSX와 마찬가지로 Razor 템플릿 엔진을 사용하면 마크 업에 C # 코드를 작성할 수 있습니다.
 

## 공연
 

Blazor 프로젝트는 브라우저에서 필요한`DLL` 라이브러리와 함께 전체 닷넷 런타임을 다운로드해야하기 때문에 클라이언트 측에서 느립니다.
 

Blazor 앱에는 지연 시간 문제가 있습니다.
 따라서 전 세계 사람들이 액세스 할 웹 애플리케이션을 구축하는 경우 Blazor는 기본 프레임 워크가 아닙니다.
 

주목해야 할 또 다른 중요한 사항은 Blazor 앱에서 개발하는 동안 React에있는 핫 리로드 기능을 즐길 수 없다는 것입니다.
 결과적으로 앱을 다시 시작하려면 다시 시작 버튼을 눌러야하므로 개발 프로세스가 느려질 수 있습니다.
 

등대 점수
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/blazor-app-lighthouse-score.png?resize=730%2C385&ssl=1)

위 다이어그램의 등대 점수는 Blazor 앱에 심각한 성능 문제가 있음을 분명히 보여줍니다.
 초기 페이지로드시 필요한 종속성을 다운로드해야하므로 초기 페이지로드 시간이 느립니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/react-app-lighthouse-score.png?resize=730%2C352&ssl=1)

이것이 React가 빛나는 곳입니다.
 UI 라이브러리이기 때문에 핵심 React 패키지는 린만큼 얇습니다.
 구성 요소 기반 패러다임을 사용하여 초고속 최신 클라이언트 측 애플리케이션을 구축 할 수 있도록 완전히 최적화되어 있습니다.
 

## 생태계
 

이 기사를 작성할 당시 Microsoft는 Blazor WebAssembly 및 Blazor 서버를 포함하여 5 개의 새로운 Blazor 버전을 발표했습니다.
 

반면에 React 생태계는 구현하려는 거의 모든 것에 대한 패키지를 npm에서 찾을 수있을 정도로 매우 큽니다.
 

React는 Facebook에서 전적으로 지원하며 클라이언트 측 애플리케이션이 빌드되는 방식에 대해 많은 변화를 겪었 기 때문에 많은 커뮤니티 지원을 받았습니다.
 또한 여전히 JavaScript이기 때문에 웹 개발자에게 자연스러운 느낌입니다.
 

React는 "한 번 배우고 어디에서나 쓰기"라는 판매 포인트가 있습니다.
 기본적으로 React, ReactDOM 및 React Native를 사용하면 고도로 대화 형이고 풍부한 프런트 엔드 웹, Android 및 iOS 기본 애플리케이션을 구축 할 수 있습니다.
 

따라서 대부분의 회사는 React.js 및 React Native에 능숙한 개발자를 고용하기 때문에 비교적 저렴한 비용으로 견고한 제품과 플랫폼을 쉽게 구축 할 수 있습니다.
 

### GitHub 평가
 

이 글을 쓰는 시점에서 React는 GitHub에서 16 만 개 이상의 별을 가지고 있습니다.
 일반적으로 가장 사랑받는 JavaScript 프레임 워크 중 하나입니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/react-stars-on-github.jpeg?resize=730%2C431&ssl=1)

한편 Blazor는 GitHub에서 약 9.3k + 별을 보유하고 있습니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/blazor-stars-on-github.jpeg?resize=730%2C441&ssl=1)

Blazor가 2018 년에 처음 출시되었으며 개발자 커뮤니티에 비교적 새로운 것이기 때문에이를 표시 할 타당한 이유가 아닐 수 있습니다.
 

### Blazor PWA와 React의 PWA 지원
 

출시되면 Blazor PWA는 개발자가 고급 프로그레시브 웹 앱을 빌드 할 수 있도록 지원합니다.
 React.js 애플리케이션에서 PWA 지원을 추가하는 것은 매우 쉽습니다.
 

다음 명령어를 실행하면 서비스 워커 파일이 추가 된 React 앱이 초기화됩니다.
 

`npx create-react-app my-app --template cra-template-pwa`
 

### Blazor 네이티브 (실험용), Blazor 하이브리드 대 React Native
 

Blazor Native를 사용하면 개발자가 Mobile Blazor 바인딩을 사용하여 Blazor로 기본 모바일 앱을 빌드 할 수 있습니다.
 C # 및 .Net을 사용하면 Blazor를 통한 Android 및 iOS 앱 개발이 실제로 가능합니다.
 

Blazor 하이브리드 앱은 단일 앱에서 기본 및 웹 UI의 조합입니다.
 즉, Blazor를 사용하여 앱의 기본 UI를 작성하고 Blazor를 사용하여 앱에서 웹 UI를 만들 수도 있습니다.
 

이렇게하면 Blazor를 사용하여 웹 및 모바일 애플리케이션을 모두 빌드 할 수 있습니다.
 

기본적으로 웹 및 모바일 애플리케이션에서 코드 스 니펫을 공유하게됩니다.
 이것은 확실히 C # .Net 개발자가되기에 좋은시기입니다.
 

네이티브 모바일 애플리케이션을 빌드하는 React 방식은 React Native입니다.
 React 개발자는 React로 모바일 앱을 빌드 할 수 있습니다.
 이를 통해 네이티브 UI 컨트롤을 사용하고 네이티브 플랫폼에 대한 전체 액세스 권한을 가질 수 있습니다.
 

> React Native는 안정적이며 현재 프로덕션에 사용되고 있지만 Blazor Native는 아직 개발 실험 단계에 있습니다.
 React Native와 비교할 때 커뮤니티 지원이 거의 없습니다.
 

## 패키지 관리자
 

React는 다른 JavaScript 프레임 워크 및 라이브러리와 마찬가지로 npm 및 yarn을 패키지 관리자로 사용하여 프로젝트가 올바르게 작동하는 데 필요한 종속성 (사용자 또는 다른 사람이 작성한 외부 코드)을 관리합니다.
 

Blazor WebAssembly 앱에서 패키지는 다음 방법 중 하나로 쉽게 설치할 수 있습니다.
 

- PackageReference
 
- .NET CLI
 
- 패키지 관리
 
- Paket CLI
 

PackageReference로 패키지를 설치하려면`Blazorize.csproj` 파일로 이동하여`ItemGroup` 태그 내부에 패키지를 추가합니다.
 

```xml
<PackageReference Include="System.Net.Http.Json" Version="5.0.0" />
```

.NET CLI를 사용하여 패키지를 설치하려면 터미널에서 앱 디렉터리의 루트로 이동하고 그에 따라 다음 명령을 실행합니다.
 

```css
dotnet add package Microsoft.AspNetCore.Blazor.HttpClient --version 3.2.0-preview3.20168.3
```

> .NET CLI 및 Paket CLI에 대해 알아 보려면 각각이 가이드와이 가이드를 확인하세요.
참고 : 여기서는 Blazor HttpClient 패키지를 설치합니다.
 

## 구성 요소 간의 통신
 

### 반응 방식
 

React는 기본적으로 구성 요소의 상태를 처리하는 두 가지 주요 접근 방식을 제공합니다.
 

컴포넌트는 자체 상태 또는 데이터를 처리하거나 소품을 통해 데이터를받을 수 있습니다.
 

> HTTP 섹션을 다룰 때 구성 요소가 자체 상태를 처리하는 방법을 살펴 보겠습니다.
 

컴포넌트가 React 앱의 상위 컴포넌트에서 props를 통해 데이터를받는 방법은 다음과 같습니다.
 

```js
// Parent Component

export default function Blog() {
    const blogPosts = [
        {
            id: 1,
            title: 'This is the title',
            content: 'This is some random content',
        },
        {
            id: 2,
            title: 'This is the title',
            content: 'This is some random content',
        },
        {
            id: 3,
            title: 'This is the title',
            content: 'This is some random content',
        },
        {
            id: 4,
            title: 'This is the title',
            content: 'This is some random content',
        },
    ]
    return (
        <>
            <BlogCard blogPosts={blogPosts}/>
        </>
    )
}
```

여기에서 블로그 게시물을 포함하는 객체의 배열 인`blogPosts`를`BlogCard` 구성 요소 (하위 구성 요소)에 전달했습니다.
 

```js
// Child Component

export default function BlogCard( { blogPosts } ) {
    return (
        <>
            {blogPosts.map(blogPost => (
                <div className="blog-post" key={blogPost.id}>
                    <h1>{blogPost.title}</h1>
                    <p>{blogPost.content}</p>
                </div>
                ))}
        </>
    )
}
```

이제 하위 컴포넌트`BlogCard`를 렌더링 할 때 소품을 통해 블로그 게시물 목록을 전달할 수 있습니다.
 

> React의 소품에 대해 자세히 알아 보려면 공식 문서를 확인하세요.
 

### Blazor 방식
 

Blazor에서 하위 구성 요소는 매개 변수를 통해 상위 구성 요소로부터 데이터를받을 수 있습니다.
 

```undefined
// Child Component
<h2>@Title</h2>
<p>@Content</p>
@code {    
   // Demonstrates how a parent component can supply parameters
    [Parameter]
    public string Title { get; set; }
    [Parameter]
    public string Content { get; set; }
}
```

`BlogCard`구성 요소를 렌더링 할 때 `Title`및 `Content`를 전달할 수 있으며 이에 따라 렌더링됩니다.
 

```xml
<BlogCard Title="What is Blazor?" Content="Blazor is a web UI framework using C#/Razor and HTML..." />
```

## 라우팅
 

React.js 애플리케이션에서 라우터는 미리 구성되거나 설치되지 않습니다.
 React Router 패키지는 주로 클라이언트 측 탐색을 구현하는 데 사용됩니다.
 

문서에 따르면 React Router는 React Native 프로젝트를 포함하여 React가 렌더링되는 모든 곳에서 작동합니다.
 API는 정말 간단하지만 URL 매개 변수, 구성 요소 리디렉션, 지연로드, 페이지 전환, 중첩 라우팅 등과 같은 많은 강력한 기능을 처리합니다.
 

React.js 앱에서 라우팅을 설정하려면 일반적으로`react-router-dom` 패키지를 설치하고 React Router DOM 패키지의 브라우저 라우터 모듈을 사용하여`index.js` 파일에 전체 앱을 래핑합니다.
 그런 다음`App.js` 구성 요소에서 React Router DOM의 경로 모듈을 사용하여 페이지를 렌더링합니다.
 

> 다음은 React.js에서 라우팅이 작동하는 방식을 보여주는 이미지입니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/routing-in-react.js.png?resize=730%2C570&ssl=1)

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/routing-path.png?resize=730%2C693&ssl=1)

Blazor WebAssembly (클라이언트 측) 애플리케이션에서 라우팅 시스템은 ASP.NET의 기존 라우팅 엔진에 의존합니다.
 `@ page` 지시문을 사용한 다음 파일 맨 위에 연결하려는 경로를 사용하여 Blazor 구성 요소에 경로를 매핑 할 수 있습니다.
 

Bam!
 그렇게 간단합니다.
 다른 페이지로 이동하려면 `NavLink`구성 요소를 사용해야합니다.
 이는`react-router-dom`에서`NavLink` 구성 요소가 작동하는 방식과 유사합니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/routing-in-blazor.png?resize=730%2C283&ssl=1)

다음 코드를 사용하여 파일 상단에`NavigationManager`를 삽입하여 Blazor 앱의 페이지 사이를 프로그래밍 방식으로 탐색합니다.
 

`@inject NavigationManager NavManager`
 

그런 다음 삽입 한`NavManager`의 함수에서`NavigateTo` 메서드를 호출합니다.
 

```coffeescript
@inject NavigationManager NavManager
<p>Learn more about us</p>
<button @onclick="navigateHome">Go back home</button>

@code {
    private void navigateHome()
    {
        NavManager.NavigateTo("");
    }
}
```

## HTTP 다루기
 

### 블레이저
 

Blazor 애플리케이션에서 HTTP 요청을 처리하는 방법을 설명하기 위해 pages 디렉터리에 `FetchPost.razor`파일을 만듭니다.
 

```undefined
@page "/http"
@inject HttpClient Http

<h1>Blog post</h1>

@if (posts == null)
{
    <p><em>Loading...</em></p>
}
else
{
    <table class="table">
        <thead>
            <tr>
                <th>Title</th>
                <th>Body</th>
            </tr>
        </thead>
        <tbody>
            @foreach (var post in posts)
            {
                <tr>
                    <td>@post.title</td>
                    <td>@post.body</td>
                </tr>
            }
        </tbody>
    </table>
}
@code {
    private BlogPost[] posts;
    protected override async Task OnInitializedAsync()
    {
        posts = await Http.GetJsonAsync<BlogPost[]>("https://jsonplaceholder.typicode.com/posts");
        Console.WriteLine(posts);
    }
    public class BlogPost
    {
        public int id { get; set; }
        public String title { get; set; }
        public String body { get; set; }
    }
}
```

여기서 진행되고있는 세 가지 흥미로운 일이 있습니다.
 

먼저 HttpClient를 삽입합니다.
 이것은 HTTP 클라이언트를 사용하여 HTTP 호출을하는 데 도움이되는 서비스입니다.
 

> 참고 : Blazor 버전 3.1.0부터는 Blazor HTTP 클라이언트를`Blazorize.csproj` 파일에 추가해야합니다.
 

그런 다음`GetJsonAsync ()`메서드를 사용하여 HttpClient를 호출하여 정의 된 엔드 포인트에서 JSON을 가져옵니다.
 

마지막으로 수신 JSON 구조를 기반으로 `post`속성에서 찾은 관련 데이터를 선택할 수있는 `BlogPost`유형의 결과 유형을 만듭니다.
 

### HttpClient 주입
 

이 작업은 페이지 상단에서`@ inject``HttpClient Http` 코드로 수행됩니다.
 

### HttpClient 호출
 

여기서 우리는 적극적으로 데이터를 가져오고 있습니다.
 페이지가 초기화 될 때 실행되도록 보장되는 수명주기 메서드 `OnInitializedAsync ()`를 정의하여이를 수행합니다.
 

### 반응
 

React와 같은 라이브러리를 사용하는 이점 중 하나는 도구의 유연성입니다.
 React는 Blazor와 같은 http 클라이언트를 제공하지 않으므로 React 개발자는 Fetch API, Axios 또는 XHR을 사용하여 http 요청을 할 수 있습니다.
 

> http 요청에 JavaScript Fetch API를 사용할 것입니다.
 

기본 제공 JavaScript Fetch API를 사용하여 React 앱에서 간단한 http 요청을 만드는 방법은 다음과 같습니다.
 

```js
// post.js

import React, { useState, useEffect } from 'react'
export default function Post() {
  const [posts, setPosts] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
        fetch("https://jsonplaceholder.typicode.com/posts")
            .then(response => response.json())
            .then(post=> {
                setPosts(post)
                setLoading(false)
            })
    }, [])

   return (
        <div>
            {
            loading ?
                (
                    <p>Loading...</p>
                ) : (
                    posts.map((post, i) => (
                        <div>
                            <p>{post.title}</p>
                            <p>{post.body}</p>
                        </div>
                    ))
                )
            }
        </div>
    )
}
```

`useEffect` 후크에서 엔드 포인트를 호출하고 있습니다.
 이것은 우리 컴포넌트가 렌더링 후 API에서 데이터를 가져와야한다고 React에게 알려주고, 요청이 성공하면`setPosts`를 호출하고 API에서 post 객체를 전달해야합니다.
 

> React에서`useState` 후크가 작동하는 방법에 대한 자세한 내용은 공식 문서를 확인하세요.
React App의 http 요청에 대한 자세한 가이드는 이것을 확인하십시오.
 

## 결론
 

SPA 구축을위한 프런트 엔드 프레임 워크를 선택하는 것은 팀 선호도, 에코 시스템, 성능 및 확장 성을 포함한 많은 요소에 따라 달라집니다.
 

이 기사에서는 React와 Blazor를 나란히 비교하여이 두 가지 멋진 프레임 워크가 어떻게 작동하는지 확인했습니다.
 

React와 Blazor는 몇 가지면에서 비슷하며 동일한 작업을 수행하는 데 사용할 수 있습니다.
 그렇다면 어떤 것을 선택해야합니까?
 인기도, 구축중인 프로젝트 유형, 확장 성 및 유지 관리 가능성을 고려해야 할 수도 있습니다.
 

위의 일반적인 관점에서 다음 프로젝트를위한 프론트 엔드 프레임 워크를 선택할 때 정보에 입각 한 결정을 내릴 수 있어야합니다.
 

이 기사에 대해 어떻게 생각했는지 아래의 의견 섹션에 알려주십시오.
 저는 Twitter와 GitHub에서 소셜입니다.
 읽어 주셔서 감사합니다.
 