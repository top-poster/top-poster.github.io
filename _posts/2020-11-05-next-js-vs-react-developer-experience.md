---
layout: post
title: "next.js vs. 반응: 개발자 경험"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/next-js-vs-react-developer-experience.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/next-js-vs-react-developer-experience.png?fit=730%2C487&ssl=1)

소프트웨어 라이브러리나 프레임워크를 선택할 때 일반적으로 개발자 경험을 고려한다. 제가 여기서 "개발자 경험"이라고 말할 때, 저는 개발자들이 실제로 그 일을 하는 것이 어떤 것인지 말하고자 합니다. 개발자들은 일반적으로 재미있고 사용하기 쉬운 도서관이나 틀을 선호한다. 이것이 오늘날 우리가 선도적인 도서관과 틀을 가지고 있는 주요한 이유이다.

리액트 세계에서 넥스트.js는 "그라운드를 강타하기" 위한 인기 있는 프레임워크가 되었다. 리액트 팬으로서 저는 몇 달 전에 Next.js를 선택했고 제 프로젝트에서 함께 일하는 것을 정말 즐겼습니다. 너무 좋아서 Next.js를 사용하여 개인 블로그를 다시 작성하기도 했습니다.

Next.js는 리액션 위에 구축되어 효율적인 개발 환경을 제공합니다. Next.js를 이용한 작은 학습 곡선이 있지만 프런트엔드의 세계에 처음 접하는 개발자들도 비교적 빠르게 일어나고 실행할 수 있다. 말하자면, Next.js 대 Next.js로 프로젝트를 빌드할 때 확실히 다른 경험이 있습니다. 반응하다.

이 게시물은 Next.js의 개발자 경험을 비교한다. 대응. 먼저 몇 가지 배경을 살펴본 후 자세한 내용을 살펴보고, 프로젝트를 시작하고, 페이지를 작성하고, 데이터를 검색하고, 설명서를 사용하고, Next.js와 React를 사용하여 고급 작업을 수행하는 방법에 대해 설명하겠습니다.

여기 GitHub repo에서 볼 수 있는 샘플 프로젝트를 참조하겠습니다. 이 프로젝트는 히트 쇼인 만달로리언에 관한 한 팬 사이트의 두 가지 다른 구현을 보여준다. 프로젝트의 `react` 폴더는 물론 React 버전이고 `next.js` 폴더는 Next.js 버전을 포함합니다. 두 프로젝트 모두 작동하며 일어나기 위해서는 npm 설치라는 표준만 있으면 된다.

## 어떤 배경

실제 개발자의 경험을 자세히 살펴보기 전에, 어느 정도의 배경지식을 갖추는 것이 도움이 됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/react-banner.png?resize=730%2C177&ssl=1)

리액트는 원래 페이스북에 의해 만들어졌고 오늘날 프런트엔드 세계에서 가장 인기 있는 도서관 중 하나가 되었다. React는 쉽게 확장할 수 있으며, Redux와 같은 라이브러리를 사용하는 상태 관리 패턴뿐만 아니라 라우팅과 같은 기능을 포함할 수 있습니다. 반응은 설치 공간에서는 미미하지만 거의 모든 프로젝트에 대해 사용자 정의할 수 있습니다. 높은 수준의 반응에 대한 자세한 내용은 공식 반응 문서를 참조하십시오.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/next-js-banner.png?resize=366%2C150&ssl=1)

Next.js는 사용하기 쉬운 개발 프레임워크를 구축하기 위한 노력으로 React 위에 생성되었다. 베르셀(Vercel, 이전의 Zeit)이 개발했으며 리액트의 인기 있는 기능들을 많이 활용했다. 즉시 Next.js는 사전 렌더링, 라우팅, 코드 분할 및 웹 팩 지원과 같은 기능을 제공합니다. Next.js에 대한 자세한 내용은 공식 Next.js 설명서를 참조하십시오.

## 반응 대 반응 next.js 프로젝트는 어떻게 생겼나요?

React를 사용하면 시스템에 Node.js를 설치하고 `npx create-react-app my-app`을 실행하여 시작하고 실행할 수 있습니다. 이렇게 하면 `src/App.js` 파일을 응용 프로그램의 진입점으로 사용하는 기본 프로젝트 구조가 생성됩니다.

자산을 저장할 수 있는 공용 폴더도 있으며, 초기 비계에는 서비스 직공과 시험용 제스트를 끌어들이는 방법이 포함되어 있습니다. 초기 비계는 다음과 같습니다.

```css
.
├── README.md
├── package.json
├── public
│   ├── favicon.ico
│   ├── index.html
│   ├── logo192.png
│   ├── logo512.png
│   ├── manifest.json
│   └── robots.txt
├── src
│   ├── App.css
│   ├── App.js
│   ├── App.test.js
│   ├── index.css
│   ├── index.js
│   ├── logo.svg
│   ├── serviceWorker.js
│   └── setupTests.js
└── yarn.lock
```

Next.js를 사용하면 `npx create-next-app`을 실행하여 시작할 수 있습니다. 이렇게 하면 이미 페이지나 경로에 대한 페이지 폴더와 자산을 호스트하는 공용 디렉토리가 있는 프로젝트가 계획됩니다. 초기 비계는 다음과 같습니다.

```css
.
├── README.md
├── package.json
├── node_modules
├── pages
│   ├── _app.js
│   ├── api
│   └── index.js
├── public
│   ├── favicon.ico
│   └── vercel.svg
├── styles
│   ├── Home.module.css
│   └── globals.css
└── yarn.lock
```

`페이지` 디렉토리의 파일은 응용프로그램의 라우트와 관련이 있습니다. `public` 디렉토리는 서비스할 정적 파일이나 이미지를 저장하며, 직접 액세스할 수 있습니다. 따라서 사진을 구성요소로 가져오기 위해 `requires` 또는 다른 기존 Retact 방법을 사용할 필요가 없습니다.

페이지 디렉토리 내에 프로그램의 진입점인 `index.js` 파일이 표시됩니다. 다른 페이지로 이동하려면 다음과 같이 라우터를 `링크`와 함께 사용할 수 있습니다.

```xml
        <div className="header__links">
            <Link href="/">
                <a className="header__anchor">Home</a>
            </Link>
            <Link href="/about">
                <a className="header__anchor">About</a>
            </Link>
        </div>
```

개발자 경험과 관련하여 초기 비계 프로세스는 Next.js와 React에서 모두 매우 간단하다. 그러나 Retact는 라우팅에 대해 React Router와 같은 라이브러리를 추가해야 하는 반면, Next.js는 `Link` 구성 요소와 함께 즉시 해당 기능을 제공합니다.

또한 컨테이너 등을 보관할 수 있는 `페이지` 디렉토리가 있으면 응용프로그램의 전체 구조가 Next.js에 의해 이미 안내됩니다.

## 페이지 작성

이제 React 대 React의 실제 사례를 살펴보겠습니다. 처음에 말씀드린 샘플 어플리케이션으로 next.js. 다시, 당신은 그것을 레포에서 찾을 수 있습니다.

React로 페이지를 작성하려면 구성요소를 작성한 후 React Router를 끌어와 사이트의 전환을 조정해야 합니다. 샘플 응용 프로그램의 `반응` 폴더를 보면 기존 반응 응용 프로그램에서 기대하는 바를 알 수 있습니다.

```js
export default function App() {
    return (
        <Router>
            <section>
                <Header />
                <Switch>
                    <Route path="/episodes">
                        <EpisodesPage />
                    </Route>
                    <Route path="/season2">
                        <Season2Page />
                    </Route>
                    <Route path="/quotes">
                        <QuotesPage />
                    </Route>
                    <Route path="/">
                        <HomePage />
                    </Route>
                </Switch>
            </section>
        </Router>
    );
}
```

여기서 헤더, 에피소드 페이지, 시즌2페이지2, 인용 페이지, 홈페이지는 리액트 라우터가 렌더링할 URL 경로를 라우팅하는 모든 구성 요소입니다.

프로젝트의 `Next.js` 폴더를 보면 경로가 모두 `page` 폴더에 내장되어 있기 때문에 프로젝트가 훨씬 더 희박하다는 것을 알 수 있습니다. `헤더` 구성 요소는 다음과 같이 `링크`를 사용하여 다른 페이지로 이동합니다.

```js
import Link from "next/link";
const Header = () => {
    return (
        <nav className="header">
            <span>
                <Link href="/">Home</Link>
            </span>
            <span>
                <Link href="/episodes">Episodes</Link>
            </span>
            <span>
                <Link href="/season2">Season 2</Link>
            </span>
            <span>
                <Link href="/quotes">Quotes</Link>
            </span>
        </nav>
    );
};
export default Header;
```

Next.js 프로젝트의 상위 레벨 뷰는 얼마나 쉽게 따라갈 수 있는지 보여줍니다.

```css
.
├── README.md
├── package-lock.json
├── package.json
├── pages
│   ├── _app.js
│   ├── _document.js
│   ├── components
│   │   └── Header.js
│   ├── episodes.js
│   ├── index.js
│   ├── quotes.js
│   ├── season2.js
│   └── styles
│       ├── _contact.scss
│       ├── _episodes.scss
│       ├── _header.scss
│       ├── _home.scss
│       ├── _quotes.scss
│       ├── _season2.scss
│       └── styles.scss
├── public
│   ├── HomePage.jpg
│   └── favicon.ico
└── yarn.lock
```

대응 프로젝트에 대한 페이지를 작성하려면 구성요소를 작성한 후 라우터에 추가해야 합니다. Next.js 프로젝트를 위한 페이지를 작성하려면 `page` 폴더에 페이지를 추가하고 필요한 `Link`를 `Header` 구성 요소에 추가합니다. 이렇게 하면 코드를 적게 쓰고 프로젝트를 쉽게 수행할 수 있기 때문에 생활이 더 쉬워집니다.

## 데이터 가져오기

어떤 응용 프로그램에서도 데이터를 항상 검색할 필요가 있습니다. 정적 사이트든, 여러 API를 활용하는 사이트든, 데이터는 중요한 구성요소입니다.

샘플 프로젝트의 `react` 폴더를 보면 다음과 같이 `EpisodesPage` 구성 요소가 Redux 작업을 사용하여 `Episodes` 데이터를 검색하는 것을 볼 수 있습니다.

```js
    const dispatch = useDispatch();
    // first read in the values from the store through a selector here
    const episodes = useSelector((state) => state.Episodes.episodes);
    useEffect(() => {
        // if the value is empty, send a dispatch action to the store to load the episodes correctly
        if (episodes.length === 0) {
            dispatch(EpisodesActions.retrieveEpisodes());
        }
    });
    return (
        <section className="episodes">
            <h1>Episodes</h1>
            {episodes !== null &&
                episodes.map((episodesItem) => (
                    <article key={episodesItem.key}>
                        <h2>
                            <a href={episodesItem.link}>{episodesItem.key}</a>
                        </h2>
                        <p>{episodesItem.value}</p>
                    </article>
                ))}
            <div className="episodes__source">
                <p>
                    original content copied from
                    <a href="https://www.vulture.com/tv/the-mandalorian/">
                        here
                    </a>
                </p>
            </div>
        </section>
    );
```

Redux 작업은 로컬 파일에서 값을 검색합니다.

```js
import episodes from '../../config/episodes';

// here we introduce a side effect
// best practice is to have these alongside actions rather than an "effects" folder
export function retrieveEpisodes() {
    return function (dispatch) {
        // first call get about to clear values
        dispatch(getEpisodes());
        // return a dispatch of set while pulling in the about information (this is considered a "side effect")
        return dispatch(setEpisodes(episodes));
    };
}
```

Next.js를 사용하면 기본 제공 데이터 가져오기 API를 활용하여 데이터를 포맷하고 사이트를 미리 렌더링할 수 있습니다. 또한 React Hooks 및 API 호출을 통해 평소와 같은 모든 작업을 수행할 수 있습니다. Next.js를 통해 데이터를 가져올 수 있는 추가적인 이점은 결과로 생성된 번들이 미리 제공되기 때문에 사이트 소비자가 쉽게 사용할 수 있다는 것입니다.

제 샘플 프로젝트에서는 `nextjs` 폴더와 `pises.js` 페이지로 이동하면 `getStaticProps` 호출에 의해 실제로 만달로리아 에피소드에 대한 정보가 구성되므로, 실제 데이터 검색은 사이트가 처음 구축되었을 때만 수행됩니다.

```js
function EpisodesPage({ episodes }) {
    return (
        <>
            <section className="episodes">
                <h1>Episodes</h1>
                {episodes !== null &&
                    episodes.map((episodesItem) => (
                        <article key={episodesItem.key}>
                            <h2>
                                <a href={episodesItem.link}>{episodesItem.key}</a>
                            </h2>
                            <p>{episodesItem.value}</p>
                        </article>
                    ))}
                <div className="episodes__source">
                    <p>
                        original content copied from
                        <a href="https://www.vulture.com/tv/the-mandalorian/">here</a>
                    </p>
                </div>
            </section>
        </>
    );
}
export default EpisodesPage;
export async function getStaticProps(context) {
    const episodes= [...];
    return {
        props: { episodes }, // will be passed to the page component as props
    };
}
```

## 고급 작업

여기서 다루었던 기본적인 기능들 외에도, 여러분은 결국 더 복잡한 것을 해야 할 것입니다.

React 애플리케이션을 확장하면 나타나는 가장 일반적인 패턴 중 하나는 Redux를 사용하는 것입니다. 중복 제거 기능은 응용 프로그램 상태를 사용하는 일반적인 방법을 확장하기 때문에 유용합니다. 작업, 감산기, 선택기 및 부작용을 생성하는 프로세스는 응용 프로그램이 어떤 작업을 수행하든 간에 잘 확장됩니다.

React의 경우, 이는 저장소를 정의한 후 애플리케이션 전체에 걸쳐 흐름을 구축하는 문제입니다. 제가 가장 먼저 한 일 중 하나는 Next.js와의 프로젝트에서 이 작업을 수행할 수 있는지 확인하는 것이었습니다.

몇 번의 구글링(그리고 몇 번의 시도 실패) 후, 저는 Next.js가 각 페이지를 미리 렌더링하고 다시 렌더링하는 방법 때문에 스토어를 사용하는 것이 매우 어렵다는 것을 알게 되었습니다. Next.js를 사용하여 Redux를 구현한 사람이 몇 명 있지만, Vanilla React 앱에서 볼 수 있는 것처럼 간단하지는 않습니다.

Redux를 사용하는 대신 Next.js는 사전 렌더링이 가능한 데이터 가져오기 API를 사용합니다. 이러한 기능은 웹 크롤러가 쉽게 읽을 수 있는 정적 조각 집합이 되어 사이트의 SEO가 향상되기 때문에 유용합니다.

이는 일반적으로 JS 번들이 크롤러가 이해하기 어려웠기 때문에 큰 성공입니다. 또한 RSS 피드와 같이 빌드 시 이러한 API 및 생성된 자산에 대해 보다 교묘하게 사용할 수 있습니다.

제 개인 블로그 사이트는 Next.js로 작성되었습니다. 실제로 Next.js와 함께 제공되는 getStaticProps API를 사용하여 나만의 RSS 피드를 구축했습니다.

```php
export async function getStaticProps() {
  const allPosts = getAllPosts(["title", "date", "slug", "content", "snippet"]);

  allPosts.forEach(async (post) => {
    unified()
      .use(markdown)
      .use(html)
      .process(post.content, function (err, file) {
        if (err) throw err;
        post.content = file;
      });
  });

  const XMLPosts = getRssXml(allPosts);
  saveRSSXMLPosts(XMLPosts);

  return {
    props: { XMLPosts },
  };
}
```

getAllPosts 및 getRssXml 기능은 Markdown을 RSS 표준으로 변환합니다. 그런 다음 내 사이트와 함께 배포하여 RSS 피드를 활성화할 수 있습니다.

React와 Next.js는 Redux나 Pre-rendering과 같은 고급 기능을 모두 갖추고 있다. React에서 작동하는 패턴은 Next.js에서 반드시 작동하지 않습니다. Next.js에는 자체적인 장점이 있기 때문에 나쁘지 않습니다.

전반적으로, 고급 작업을 구현할 때 Next.js를 사용한 개발자 경험은 React 프로젝트에서 일반적으로 볼 수 있는 것보다 더 많은 지침을 제공할 수 있습니다.

## 설명서 작업

어떤 소프트웨어 프로젝트든 좋은 설명서가 있으면 도구를 쉽게 사용하고, 어떤 라이브러리를 사용할지 이해하는 데 도움이 됩니다. React 및 Next.js 모두에 대해 사용 가능한 환상적인 문서 옵션이 있습니다.

인트로에서 언급했듯이 Next.js에는 라우팅 및 구성 요소 구축과 같은 작업을 수행하는 방법에 대해 설명하는 "Learn-by-doing" 설명서가 있습니다. React는 기본 사항을 설명하는 여러 튜토리얼과 함께 유사한 설정도 제공합니다.

React를 사용하면 블로그 게시물, YouTube 비디오, Stack Overflow, 심지어 React 문서 자체에도 콘텐츠를 생성한 개발자 커뮤니티에 의존할 수 있습니다. 이것은 도서관이 성숙함에 따라 수년 동안 개발되어 왔다.

Next.js를 사용하면 공식 자습서의 방법이 줄어들고 GitHub 문제와 대화의 방법이 더 많아진다. 개인 블로그 사이트를 구축하면서 Next.js 문제를 해결하기 위해 상당한 구글링을 해야 하는 경우가 있었습니다. 그러나 Next.js의 팀원들은 오픈 소스 세계에서 매우 쉽게 접근할 수 있다.

Next.js 팀 멤버 중 한 명인 팀 뉴트켄스는 제가 올 여름 초 Next.js에 글을 썼을 때 트위터에 직접 답했습니다. 그는 내가 어떤 문제에 대해 일하는 것을 도왔고 함께 일하기에 정말 훌륭했다. 접근 가능한 커뮤니티 구성원을 갖는 것은 큰 강점이다.

리액트 커뮤니티에는 액세스 가능한 주요 구성원들이 많이 있습니다. React와 Next.js 모두에서 액티브 커뮤니티는 매우 긍정적인 개발자 경험을 제공한다.

## Next.js vs.에 대한 최종 생각 반응하다.

개발자의 경험은 엔지니어들이 그들이 하는 일을 사랑하게 만드는 것이다. 저는 전문적으로 엔지니어로 일하지만, 전문 환경이 항상 최고의 개발자 경험을 제공하는 것은 아닙니다.

완벽한 세상에서는 우수한 제품, 강력한 사용자 커뮤니티 및 강력한 툴을 갖춘 훌륭한 엔지니어 팀에 소속되어 있습니다. 현실 세계에서는 그런 것을 일부 발견하지만, 대개 하나 또는 그 이상의 것이 부족하다.

React와 Next.js는 둘 다 그들만의 방식으로 훌륭한 개발자 경험을 제공한다. 리액션은 당신이 원하는 방식으로 사물을 건설할 수 있도록 하며 강력한 커뮤니티의 지원을 받습니다. next.js는 포장에서 꺼내어 사용할 수 있는 몇 가지 툴과 규칙을 통해 여러분의 삶을 더 쉽게 만들어 주고, 매우 활발한 오픈 소스 커뮤니티도 지원합니다.

툴링 자체만 놓고 보면 React와 Next.js 모두 쉽게 시작할 수 있었습니다. 단순한 "안녕하세요 세계" 프로젝트를 넘어, 문서나 커뮤니티 리소스에서든 도움을 찾기가 꽤 쉬웠습니다.

React와 Next.js 모두 그들이 잘 하는 특정한 것들을 가지고 있다. 제가 언급했듯이, 저는 팬이었기 때문에 Next.js로 제 개인 블로그를 다시 썼습니다. 정적 사이트에 사용할 수 있는 훌륭한 도구이며 제가 설정한 CI/CD 파이프라인과 쉽게 연결됩니다. 또한 다른 페이지에 대한 구성요소를 사이트에 추가하고 싶을 때 쉽게 탐색할 수 있습니다.

리액션은 모든 프로젝트에 훌륭한 추가 사항이며 기회가 주어지면 확장할 수도 있습니다. 리액트는 라이브러리이기 때문에 Next.js보다 더 다용도적이다.

결국, React와 Next.js는 모두 탄탄한 개발자 경험을 제공한다. 제가 여기에 포함시킨 비교와 토론에서 프로젝트를 어떻게 활용할 수 있는지 알 수 있기를 바랍니다. 나는 당신이 그것들을 확인하고, 나의 샘플 프로젝트도 함께 확인해 보길 권합니다.

내 게시물을 읽어줘서 고마워! 저를 따라와 vans.dev를 수정하고 트위터에서 @Andrew Evans0102로 연결하세요.