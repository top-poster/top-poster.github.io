---
layout: post
title: "구성이없는 풀 스택 React 앱 빌드
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/Screen-Shot-2021-01-29-at-4.06.59-PM.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Screen-Shot-2021-01-29-at-4.06.59-PM.png?fit=730%2C488&ssl=1)

현대적인 프런트 엔드 애플리케이션을 구축하려면 일반적으로 많은 도구가 필요합니다.
 Babel, webpack, Parcel, Rollup 등을 생각해보십시오. 모듈 번 들러가 그렇게 인기있는 이유가 있습니다.
 

새로운 프런트 엔드 프로젝트를 시작하는 프로세스를 단순화하는 데 도움이되는 훌륭한 도구가 많이 있습니다.
 React에 대해 막연하게 잘 알고 있다면`create-react-app`을 사용했을 것입니다 (암석 아래에서 코딩하지 않는 한).
 쉽고 편리합니다.
 네, 의견이 있지만, 혼자서해야했던 고통스러운 설정을 많이 제거합니다.
 

그렇다면 제로 구성이란 무엇을 의미합니까?
 

이 기사에서는 백엔드에서 Node.js를 사용하여 풀 스택 React 앱을 빌드하는 방법을 안내하고 구성을 작성하지 않고이를 수행 할 것입니다!
 웹팩도, 복잡한 설정도 없습니다.
 나다.
 제로.
 

이러한 용이함을 제공하는 도구를 Zero라고합니다.
 Zero Server라고도하는이 제품은 구성이 필요없는 웹 프레임 워크라는 자부심을 가지고 있습니다.
 

잠재력이있는 괜찮은 프레임 워크라고 생각합니다.
 확실히 많은 스트레스를 덜어주고 매우 다른 프로젝트 설정을 처리 할 수 있습니다.
 백엔드는 Python 또는 Node 일 수 있습니다!
 프론트 엔드는 Vue, React 또는 Svelte에있을 수 있습니다!
 

모든 것과 마찬가지로 Zero에도 몇 가지 문제가 있습니다. 사용 사례에 따라 일부는 메이저, 마이너입니다.
 프로젝트를 빌드 할 때 기사에서 이러한 사항을 강조하도록하겠습니다.
 

## 풀 스택 애플리케이션
 

가상으로 유명한 Angela McReynolds를위한 애플리케이션을 구축 할 것입니다.
 그녀에 대한 모든 것을 알기 위해 응용 프로그램의 최종 버전을 살펴보십시오.
 주위를 클릭하십시오!
 

애플리케이션의 주요 부분에는 React로 구축 된 홈페이지가 포함됩니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Application-Hompage-Built-in-React.png?resize=730%2C294&ssl=1)

잠재 고객이 살펴볼 수있는 과거 프로젝트 목록 :
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Application-Past-Project-List-React.png?resize=730%2C223&ssl=1)

## Zero 설치 및 시작
 

코드의 첫 줄을 작성하고 화면에 무언가를 표시하는 것은 Zero를 사용하는 것만 큼 쉽습니다.
컴퓨터 어디에서나 새 폴더를 만들고 코드 편집기에서 열었습니다.
 

이 디렉토리 내에 새 파일`index.tsx`를 만듭니다.
 이것이 앱의 진입 점 인 홈페이지가 될 것입니다.
 어떻게 작동하는지 곧 설명하겠습니다.
 

계속해서 아래와 같이 기본`App` 구성 요소를 작성합니다.
 

```js
import React from "react";

const App = () => {
  return <h1>Hello!</h1>;
};

export default App;
```

이것은 단지`Hello!`텍스트를 렌더링합니다.
 아직 멋진 것은 없습니다.
 

현재 React 나 모듈을 설치하지 않았습니다.
 괜찮아.
 제로가 처리합니다.
 터미널에서`npx zero`를 작성하여 디렉토리에 대해 Zero를 실행하십시오.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/npx-zero-against-directory.png?resize=730%2C369&ssl=1)

`zero` 명령을 실행 한 후에 일어나는 일은 흥미 롭습니다.
 계속 진행하여`index.tsx`의 모듈을 해결하고 종속성을 설치하며 구성 파일을 자동으로 생성하므로 필요하지 않습니다!
 

이제`http : // localhost : 3000`으로 이동하면`index.tsx` 구성 요소가 제공되어야합니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Index-Component-Served-Localhost-Output-.png?resize=729%2C315&ssl=1)

이것은 특별히 흥미롭지는 않지만 여전히 여기서 배울 것이 있습니다!
 

> N.B., 우리는 전역 설치없이`zero` 서버 명령을 실행했습니다.
 이것은 우리가 npx를 사용했기 때문에 가능합니다.
 기사 전체에서 이것을 선호합니다.
 전체적으로`zero`를 설치하려면`npm install -g zero`를 실행하고`npx zero`가 아닌`zero` 만 실행하여 애플리케이션을 시작합니다.
 

## 제로 서버에서 라우팅이 작동하는 방식
 

Zero는 파일 기반 라우팅 시스템을 사용합니다.
 특정 정적 사이트 생성기로 작업 한 적이 있다면 이것은 새로운 것이 아닐 수도 있습니다.
 이것은 또한 Next.js가 수용하는 시스템입니다.
 

작동 방식은 간단합니다.
 현재 Zero Server 애플리케이션은`http : // localhost : 3000 /`에서 실행 중입니다.
 브라우저에 제공되는 페이지는 루트`index` 파일입니다.
 `index.html`,`index.jsx` 또는`index.tsx`가 될 수 있습니다. 상관 없습니다.
 Zero는 여전히 파일을 컴파일하고 제공합니다.
 

브라우저에서`/ about`을 방문한 경우 Zero는 지원되는 한 파일 유형에 관계없이 루트 디렉토리에서 연관된`about` 파일을 찾습니다 (예 :`.vue`,`.js`,`
 .ts`,`.svelte` 또는`.mdx`).
 마찬가지로`/ pages / about`을 방문하면 Zero는`pages` 디렉토리에서`about` 파일을 찾습니다.
 

간단하면서도 효과적인 라우팅.
 다음은 실용적인 예입니다.
 

루트 디렉터리에`about.tsx`라는 새 파일을 만들고 다음 기본 구성 요소를 반환합니다.
 

```js
import React from "react";

const About = () => {
  return <h1>About me!</h1>;
};

export default About;
```

물론`http : // localhost : 3000 / about`을 방문하면 다음과 같은 내용이 표시됩니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/About-Me-Basic-Component-Returned.png?resize=640%2C236&ssl=1)

> N.B., Zero는 렌더링되는 파일에서 내 보낸 기본 엔티티를 찾습니다.
 공개 React 파일에 'exports'라는 이름이없는 기본 내보내기가 있는지 확인합니다.
 

하위 디렉토리는 어떻습니까?
 

`blog` 디렉토리를 만들고 그 안에`hello.mdx` 파일을 만듭니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Blog-directory-hellomdx-file.png?resize=353%2C400&ssl=1)

다음을 작성하십시오.
 

```shell
# Hello there

## This is a new blog
```

Markdown입니다.
 그러나 여전히 Zero는 그것을 잘 렌더링합니다!
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Markdown-Rendered-by-Zero.png?resize=626%2C223&ssl=1)

파일 확장자가`.mdx` 인 것을 알 수 있습니다.
 이것은 Markdown`.md`의 수퍼 세트입니다.
 기본적으로 `mdx`는 마크 다운에 JSX를 더한 것입니다.
 Markdown 파일에서 React 컴포넌트를 렌더링 할 수있는 그림!
 예, 이것이 MDX로 할 수있는 것입니다.
 

## Zero 앱의 폴더 구조
 

라우팅은 파일 기반이므로 앱을 구성하는 방법에 대해 좀 더 고려해야합니다.
 개발하는 동안 모든 클라이언트 파일이 공개되는 것을 원하지 않을 것입니다.
 일부 구성 요소는 페이지로 구성되고 자체적으로 표시되지 않도록 존재합니다.
 

내 권장 사항은 공개하려는 파일을 기본 디렉토리 (2)에 배치하고 다른 모든 파일은`client` 디렉토리 (1)에 배치하는 것입니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Client-directory-visual-for-zero-app.png?resize=690%2C820&ssl=1)

이 디렉토리의 이름은 귀하에게 달려 있습니다.
 원한다면 `컴포넌트`라고 부를 수 있습니다.
 그러나 Zero 앱에서 이러한 우려 사항을 분리해야합니다.
 이것이 왜 금인지 잠시 알게 될 것입니다.
 

### `.zeroignore` 파일이있는 파일 무시
 

공개하지 않으려는 파일이나 디렉토리는`.zeroignore` 파일을 통해 Zero에 전달할 수 있습니다.
`gitignore` 파일처럼 무시할 디렉토리 나 파일의 이름을 씁니다.
 

이 예에서`.zeroignore` 파일은 다음과 같습니다.
 

```undefined
client 
server 
```

`client` 및`server` 디렉토리를 무시합니다.
 `client` 디렉토리는 공개를 원하지 않는 클라이언트 측 파일을 보관합니다.
 `server`도 마찬가지입니다.
 

## 홈페이지 구축
 

지금 우리는 "안녕하세요"라고만 쓰여있는 홈페이지가 있습니다.
 누구도 그것에 감동하지 않을 것입니다!
 개선합시다.
 

이 게시물은 Zero Server 작업에 초점을 맞추고 있으므로 스타일 UI 변경 사항에 대해서는 설명하지 않겠습니다.
 신속한 프로토 타이핑을 위해 Chakra 및 스타일 구성 요소를 설치합니다.
 

```undefined
npx yarn add @chakra-ui/react @emotion/react @emotion/styled framer-motion styled-components
```

이제`index.tsx` 파일을 다음으로 업데이트합니다.
 

```js
import React from "react";
import {
  Flex,
  Box,
  Heading,
  Text,
  Button,
  Center,
  Image,
  Spacer,
} from "@chakra-ui/react";

const Home = () => {
  return (
    <Flex direction={["column", "column", "row", "row"]}>
      {/* Profile Card  */}
      <Box
        flex="1.5"
        p={[10, 10, 20, 20]}
        minH={["auto", "auto", "100vh", "100vh"]}
        bg="linear-gradient(180.1deg, #CCD0E7 69.99%, rgba(144, 148, 180, 0.73) 99.96%)"
      >
        <Center height="100%">
          <Box w="70%" maxW={650} minW={400} minH={400}>
            <Flex justify="center">
              <Box
                borderRadius={10}
                bg="rgba(209, 213, 230, 0.5)"
                w="70%"
                maxW={400}
                height={200}
              >
                <Flex
                  direction="column"
                  align="center"
                  justify="center"
                  height="100%"
                >
                  <Image
                    borderRadius="full"
                    boxSize="100px"
                    src="https://i.imgur.com/95knkS8.png"
                    alt="My Avatar"
                  />
                  <Text textStyle="p" color="black">
                    Angela McReynolds
                  </Text>
                </Flex>
                <Flex mt={4} color="rgba(110, 118, 158, 0.6)">
                  <Button
                    borderRadius={6}
                    py={6}
                    px={8}
                    bg="linear-gradient(96.91deg, rgba(255, 255, 255, 0.44) 5.3%, #BDC3DD 83.22%)"
                  >
                    Read my blog
                  </Button>
                  <Spacer />
                  <Button
                    borderRadius={6}
                    py={6}
                    px={8}
                    bg="linear-gradient(96.91deg, rgba(255, 255, 255, 0.44) 5.3%, #BDC3DD 83.22%)"
                  >
                    About me{" "}
                  </Button>
                </Flex>
                <Box mt={6}>
                  <Text
                    textStyle="p"
                    textAlign="center"
                    color="black"
                    opacity={0.1}
                  >
                    &copy; 2020 Angela McReynolds
                  </Text>
                </Box>
              </Box>
            </Flex>
          </Box>
        </Center>
      </Box>
      {/* Details */}
      <Box flex="1" bg="black" p={[10, 10, 20, 20]}>
        <Heading as="h1" color="white" textStyle="h1">
          THE <br />
          WORLD'S BEST
          <br /> FRONTEND
          <br /> ENGINEER
        </Heading>
        <Text textStyle="p">
          Forget about hype, self affirmation and other bullshit. I don’t do
          those.
        </Text>

        <Text textStyle="p">
          I’ve got results. in 2015, 2016, 2017, 2018 and 2020 I was voted the
          world’s best frontend engineer by peers and designers all around the
          world.
        </Text>

        <Text textStyle="p">
          A thorough election was conducted, and I came out on top. I’ve got
          brains and I use them, You’re lucky to have stumbled here.
        </Text>

        <Text textStyle="p">
          While living on Mars i spent decades mastering the art of computer
          programming. On arriving earth in 2013, I constantly laughed at our
          pathetic the developers on earth were. You're all lucky to have me.
        </Text>

        <Box>
          <Button
            bg="linear-gradient(96.91deg, #BDC3DD 5.3%, #000000 83.22%)"
            w={"100%"}
            color="white"
            _hover={{ color: "black", bg: "white" }}
          >
            See past projects
          </Button>
        </Box>
      </Box>
    </Flex>
  );
};

export default Home;
```

이제`localhost : 3000`을 방문하면 다음과 같은 내용이 표시됩니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Homepage-updated-with-stylistic-UI-.png?resize=730%2C393&ssl=1)

## 글로벌 중앙 집중식 페이지 구성
 

이것은 바로 여기 Zero의 가장 큰 단점 중 하나입니다.
 기본적으로 중앙 집중식 페이지 구성을 처리 할 방법은 없습니다.
 창의력을 발휘해야합니다.
 많은 경우에 당신은 이것을 알아낼 수 있고, 다른 사람들은 해 키로 판명 될 수 있습니다.
 

이 특정 시나리오에서는 Chakra UI 라이브러리에 대한 중앙 집중식 설정을 추가하려고합니다.
앱에 이와 같은 케이스가있을 것이므로 다음과 같이 권장합니다.
 

공개적으로 노출 된 파일과 독립적으로 각 페이지를 수용 할 수있는 구조로`client` 디렉토리를 채우는 것으로 시작하십시오.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Create-centralized-page-configurations-in-zero.png?resize=690%2C820&ssl=1)

혼동하지 마십시오. 여기에 제가 의미하는 바가 있습니다.
 `pages` 디렉토리를 만들고`Home` 및`About` 하위 디렉토리를 만듭니다.
 공개`index.tsx` 및`About.tsx`의 코드를 각 디렉토리로 이동합니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Move-code-to-subdirectories-visual.png?resize=498%2C392&ssl=1)

이 예에서는 `Home`에 대한 모든 코드를 다음과 같이 이동했습니다.
 

```js
// Home/Home.tsx

export const Home = () => {
  // copy code over
}
// Home/index.ts
export {Home as HomePage} from './Home'
```

계속해서`About` 페이지에 대해 동일한 작업을 수행하고`pages / index.tsx`에서 둘 다 내 보냅니다.
 

```coffeescript
export { AboutPage } from "./About";
export { HomePage } from "./Home";
```

자, 여기에 좋은 부분이 있습니다.
 

`client`디렉토리 내의 별도 파일에있는 중앙 페이지 생성 로직을 중앙 집중화합니다.
 저는 이것을`makePages.tsx`라고 불렀습니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Centralized-Page-Creation-Logic.png?resize=440%2C440&ssl=1)

테마, 메타 데이터, 사용자 정의 글꼴… 모두 한곳에 추가되었습니다.
 예제 앱에 필요한 것은 다음과 같습니다.
 

react-helmet-async를 설치합니다.
 

```undefined
npx yarn add react-helmet-async
```

그런 다음`makePages.tsx`에 다음을 추가하십시오.
 

```coffeescript
import React from "react";
import { Helmet } from "react-helmet-async";
import { ChakraProvider, Box } from "@chakra-ui/react";
import { extendTheme } from "@chakra-ui/react";

const appTheme = extendTheme({
  colors: {
    brand: {
      100: "#CCD0E7",
      200: "6E769E60",
      800: "BDC3DD",
      900: "#9094B4",
    },
  },
  fonts: {
    heading: `"Roboto Condensed", sans-serif`,
    body: "Roboto, sans-serif",
    mono: "Menlo, monospace",
  },
  textStyles: {
    h1: {
      fontSize: ["4xl", "5xl"],
      fontWeight: "bold",
      lineHeight: "56px",
    },
    p: {
      fontWeight: "bold",
      py: 4,
      color: "rgba(204, 208, 231, 0.5)",
    },
  },
});

type PageWrapperProps = {
  children: React.ReactNode;
  title: string;
};

export const PageWrapper = ({
  children,
  title,
}: PageWrapperProps & React.ReactNode) => {
  return (
    <>
      <Helmet>
        <meta charset="UTF-8" />
        <title>{title}</title>
        <link rel="preconnect" href="https://fonts.gstatic.com" />
        <link
          href="https://fonts.googleapis.com/css2?family=Roboto+Condensed:wght@700&display=swap"
          rel="stylesheet"
        />
        <link
          href="https://fonts.googleapis.com/css2?family=Roboto+Condensed:wght@700&family=Roboto:wght@700&display=swap"
          rel="stylesheet"
        />
      </Helmet>
      <ChakraProvider theme={appTheme}>
        <Box w="100%" h="100vh">
          {children}
        </Box>
      </ChakraProvider>
    </>
  );
};
```

이제 앱은 물론 다를 수 있지만 여기에 설명 된 구조의 혜택을 계속 누릴 수 있습니다.
 또한 코드를 디버깅하고 복제하는 데 많은 시간을 절약 할 수 있습니다.
 

이제`makePages.tsx`의`PageWrapper` 구성 요소를 사용해야합니다.
 `PageWrapper`는 페이지 구성 요소를 가져 와서 모든 중앙 집중식 논리를 갖도록합니다.
 

`Home / index.tsx`로 이동하여 아래와 같이`PageWrapper`를 사용합니다.
 

```js
// client/pages/Home/index.tsx

import { PageWrapper } from "../../makePages";
import { Home } from "./Home";

export const HomePage = () => (
  <PageWrapper title="Home">
    <Home />
  </PageWrapper>
);
```

위에서 설정 한 패턴을 따라`About` 페이지에 대해 동일한 작업을 수행하십시오.
 

노출 된`index.tsx` 홈페이지, 즉`localhost : 3000`에 제공된 루트 파일에서 새로운`HomePage` 구성 요소를 사용합니다.
 

```js
// index.tsx
import { HomePage } from "./client/pages";

export default () => <HomePage />;
```

이제 클라이언트 페이지에 대한 모든 중앙 집중식 구성이 있습니다.
 결과는 다음과 같습니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Client-Pages-Centralized-Configuration-Display.png?resize=730%2C393&ssl=1)

모든 것이 잘 진행되고 있습니다!
 

계속해서`정보`페이지를 만드십시오.
 위와 동일한 구조를 사용하고 어떻게 작동하는지 확인하십시오!
 

## 404 페이지 사용자 지정
 

계속해서`http : // localhost : 3000 / efdfdweg`와 같은 임의의 페이지를 방문하면 다음이 표시됩니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Default-404-Page-from-Zero.png?resize=730%2C258&ssl=1)

괜찮아.
 이것은 Zero의 기본 404 페이지입니다.
 이를 사용자 정의하려면 지원되는 모든 파일 형식으로`404` 파일을 생성하기 만하면 Zero에서 제공합니다.
 

시도해 봅시다.
 

`404.jsx` 파일을 만들고 다음을 복사합니다.
 

```js
// 404.jsx

import React from "react";
import {
  Container,
  Heading,
  Text,
  Link,
  Center,
  Image,
} from "@chakra-ui/react";
import { PageWrapper } from "./client/makePages";

export default () => (
  <PageWrapper>
    <Container bg="black">
      <Heading textStyle="h1" mt={7} textAlign="center" color="white">
        You seem lost :({" "}
      </Heading>
      <Text textStyle="p" textAlign="center">
        <Link href="/" color="brand.900">
          Go home
        </Link>
      </Text>
      <Image src="https://i.imgur.com/lA3vpFh.png" />
    </Container>
  </PageWrapper>
);
```

그리고 확실히 더 멋진 404 페이지가 있습니다.
 창의적 이죠?
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Stylized-404-Page-in-Zero.png?resize=730%2C424&ssl=1)

## Zero를 사용한 서버 측 개발
 

클라이언트에 대해 알아야 할 필수 기능이 있습니다.
 여기에는 페이지 설정 중앙 집중화와 같은 까다로운 부분이 포함됩니다.
 이제 포커스를 백엔드로 잠시 전환 해 보겠습니다.
 저는 Node.js를 사용하여 익숙해 지도록 할 것입니다.
 

코드를 구현하기 전에 라우팅이 여기서 똑같이 작동한다는 것을 알아야합니다!
 클라이언트 구현과 마찬가지로 Zero는 Python 및 Node.js와 같은 다양한 백엔드 언어를 지원합니다.
 

좋아, 먼저 먼저.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/See-past-projects-button-display-.png?resize=730%2C390&ssl=1)

사용자가 이전 프로젝트보기를 클릭하면 백엔드에서 제공되는 프로젝트 목록이 Zero로 작성된 새 페이지를 표시하려고합니다.
 기본 Node.js 백엔드를 설정해 보겠습니다.
 

`api` 디렉토리와`projects.ts` 파일을 만듭니다.
 모든 엔드 포인트는이`api` 디렉토리에 작성됩니다.
 기본적으로 엔드 포인트는 `$ {APP_BASE_URL} / api / projects`와 같은 형식입니다. 이는 의미 론적입니다!
 

TypeScript를 사용하고 있으므로 다음과 같이 Node 용 타이핑을 설치합니다.
 

```css
npx yarn add @types/node -D
```

이제`projects.ts` 파일에 다음을 붙여 넣습니다.
 

```js
const PROJECTS = [
  {
    id: 1,
    client: "TESLA",
    description: "Project Lunar: Sending the first humans to Mars",
    duration: 3435,
  },
  {
    id: 2,
    client: "EU 2020",
    description:
      "Deploy COVID tracking mobile and TV applications for all of Europe",
    duration: 455,
  },
  {
    id: 3,
    client: "Tiktok",
    description:
      "Prevent US app ban and diffuse security threat claims by hacking the white house",
    duration: 441,
  },
];

module.exports = function (req, res) {
  res.send({ data: PROJECTS });
};
```

이것은 기본 구현이지만 Express 스타일 구문에 유의하십시오.
 

```js
module.exports = function (req, res) {
  res.send({ data: PROJECTS });
};
```

여기서`req` 및`res`는 요청 및 응답 객체를 나타냅니다.
 `localhost : 3000 / api / projects`를 방문하면 이제 JSON 객체를 받아야합니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/JSON-object-received-code-.png?resize=730%2C450&ssl=1)

이제 우리가해야 할 일은이 API 끝점을 호출하는 것입니다.
 

`pages` 디렉토리에 새`Projects` 폴더를 추가합니다.
 계속해서이 폴더의`projects.tsx`에 다음을 붙여 넣으십시오.
 걱정하지 마세요. 중요한 부분을 설명하겠습니다.
 

```js
import {
  Thead,
  Tbody,
  Table,
  Tr,
  Th,
  Td,
  Heading,
  TableCaption,
  Box,
} from "@chakra-ui/react";
import { useState, useEffect } from "react";

export const Projects = () => {
  const [projects, setProjects] = useState([]);

  useEffect(() => {
    const fetchData = async () =>
      await fetch("/api/projects")
        .then((res) => res.json())
        .then(({ data }) => setProjects(data));

    fetchData();
  }, []);

  return (
    <Box
      flex="1.5"
      p={[10, 10, 20, 20]}
      minH="100vh"
      bg="linear-gradient(180.1deg, #CCD0E7 69.99%, rgba(144, 148, 180, 0.73) 99.96%)"
    >
      <Heading textStyle="h1"> Past Projects</Heading>

      <Table size="sm" my={10}>
        <TableCaption>Mere mortals can't achieve what I have </TableCaption>
        <Thead>
          <Tr>
            <Th>Client</Th>
            <Th>Description</Th>
            <Th isNumeric>Hours spent</Th>
          </Tr>
        </Thead>
        <Tbody>
          {projects.map((project) => (
            <Tr key={project.id}>
              <Td>{project.client}</Td>
              <Td>{project.description}</Td>
              <Td isNumeric>{project.duration}</Td>
            </Tr>
          ))}
        </Tbody>
      </Table>
    </Box>
  );
};
```

여기서 가장 중요한 것은 데이터 가져 오기 로직입니다.
 

```coffeescript
const [projects, setProjects] = useState([]);

  useEffect(() => {
    const fetchData = async () =>
      await fetch("/api/projects")
        .then((res) => res.json())
        .then(({ data }) => setProjects(data));

    fetchData();
  }, []);
```

`/ api / projects`라는 URL을 확인합니다.
 Zero는 SSR과 잘 작동하는 또 다른 형태의 데이터 가져 오기를 지원하지만 여기의 예는 클라이언트 측 데이터 가져 오기를 보여줍니다.
 

이제 `프로젝트`페이지로 연결하려면 홈페이지에서 `버튼`을 수정하여이 페이지로 연결하면됩니다.
 

```undefined
// client/pages/Home/Home.tsx
// add as and href props to the button. 
... 
<Button
   as="a"
   href="/projects"
   ...
>
    See past projects
</Button>
```

이제 다음이 있어야합니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Finished-Zero-Homepage-with-Data-Fetching.gif?resize=730%2C413&ssl=1)

### 쿼리 매개 변수
 

우리가 지금 가지고있는 Node 백엔드는 정말 기본입니다.
 그러나 Zero는 더 많은 것을 지원합니다.
 예를 들어 `req.body`에서 검색하여 API 함수의 프런트 엔드에서 보낸 쿼리 매개 변수를 처리 할 수 있습니다.
 

```js
// api/projects.ts

module.exports = function (req, res) {
  const {id} = req.query.id 
  res.send({ data: PROJECTS });
};

// frontend call e.g. /api/projects?id=1
```

### GET 이외의 HTTP 메서드
 

내 보낸 API 함수가 다른 HTTP 메서드에 대해 호출된다는 점을 언급 할 가치가 있습니다.
 예 : POST, PUT, PATCH 및 DELETE 메소드.
 이것들은 특별히 처리되어야합니다.
 예를 들어 `req.body`는 POST 요청을 통해 전송 된 데이터로 채워집니다.
 

### 글로벌 API 엔드 포인트 구성
 

프런트 엔드 구현과 마찬가지로 Zero에서는 기본적으로 전역 구성 옵션이 제공되지 않습니다.
 백엔드에서 이에 대한 일반적인 사용 사례는 미들웨어를 통해 논리를 중앙 집중화하는 것입니다.
 이것은 일반적인 Express 패턴입니다.
 

이를 위해 권장되는 방법은 모든 미들웨어를 중앙 디렉토리 또는 파일 (예 : 이전에 만든`server` 디렉토리)로 이동하는 것입니다.
 

다음은 미들웨어의 예입니다.
 

```js
// server/middleware.ts

const corsMiddleware = (req, res, next) => {
  res.header("Access-Control-Allow-Origin", "*");
  res.header(
    "Access-Control-Allow-Headers",
    "Origin, X-Requested-With, Content-Type, Accept"
  );
  next(); 
};
```

`next ()`호출에 유의하십시오.
 Express와 마찬가지로 이는 체인의 다음 미들웨어가 호출되도록합니다.
 

중앙 집중화의 이점은 미들웨어가 두 개 이상있을 때 나타납니다.
 

```js
//server/middleware.ts

// middleware 2 
const basicLoggerMiddleware = function(req, res, next) {
  console.log("method", req.method);
  next();
};
```

이제 각 미들웨어를 개별적으로 내보내는 대신 중앙에서 각 미들웨어를 호출 한 다음 모든 핸들러가 전달됩니다.
 

```coffeescript
// server/middleware.ts
module.exports = (req, res, handler) => {
  basicLoggerMiddleware(req, res, () => {
    corsMiddleware(req, res, () => {
      handler(req, res);
    });
  });
};
```

그런 다음 핸들러에서 미들웨어를 호출 할 수 있습니다 (예 :`api / projects.ts`).
 

```coffeescript
const middleware = require("./server/middleware");

const handler = (req, res) => {
    res.send({data: PROJECTS});
  }

module.exports = (req, res) =>
  middleware(req, res, handler);
```

가장 우아한 해결책은 아닙니다. 동의합니다.
 

## 결론
 

이것이 Zero로 빌드 된 풀 스택 앱을 얻기위한 기본 사항입니다.
 이 기사에서 언급하지 않은 사례에 대해서는 공식 문서를 확인하는 것이 좋습니다.
 

Zero의 전제는 특히 인상적이며, 주로 프런트 엔드와 백엔드에서 지원되는 다양한 파일 형식 (React, Vue, Svelte, Python에 이르기까지) 때문입니다.
 

그러나 널리 채택되기 위해서는 특히 프로덕션 케이스에 아직해야 할 일이 있습니다.
몇 가지 빠른 단점은 다음과 같습니다.
 

- 느린 컴파일 시간
 
- 잘못된 오류 처리 (예 : 기본값이 아닌 명명 된 내보내기를 사용하는 경우 브라우저가 계속로드 됨)
 
- 기사에서 언급했듯이 전역 기본값 처리 불량
 
- 낮은 OS 지원.
 프로젝트는 몇 달 동안 합당한 업데이트를받지 못했습니다.
 답이없는 수많은 문제도
 

그럼에도 불구하고 "간단한"웹 개발을 영리하게 다루는 잠재적으로 훌륭한 라이브러리라고 말해야합니다.
 다행입니다. 오픈 소스이기 때문에 저와 같이 동기를 부여받은 개인이 시간을 내면 개선에 기여할 수 있습니다.
 