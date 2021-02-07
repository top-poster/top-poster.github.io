---
layout: post
title: "React에서 동영상 검색 앱 만들기"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/react-movie.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/react-movie.png?fit=730%2C487&ssl=1)

이 기사에서는 사용자가 OMDB API를 사용하여 좋아하는 동영상에 대한 정보를 검색하고 표시할 수 있는 간단한 웹 앱을 만드는 방법에 대해 알아봅니다. 등록하여 API 키를 얻습니다.

이 튜토리얼에서는 React를 사용하여 완전한 프런트 엔드 앱을 만들 것입니다. CSS는 Tailwind CSS와 Tailwind UI를 사용하겠습니다. 여기서 완성된 앱을 사용해 보세요.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/movie-search-app.png?resize=730%2C381&ssl=1)

앱의 작동 방식을 간략하게 알려주기 위해 앱은 사용자가 이름을 가진 동영상을 검색하고 사용자에게 해당 이름의 동영상 목록을 제공할 수 있도록 합니다.

PS: 이 글은 당신이 리액션을 읽고 조금 알고 있다고 가정한다. React.js를 사용하는 기본적인 방법을 알려주기 위한 것입니다. React에서 모든 것을 가르치거나 새로운 업데이트를 제공하는 것이 아닙니다.

이제 Retact 앱을 만들어 보겠습니다.

```undefined
npx create-react-app my-app
```

이렇게 하면 반응 앱이 만들어집니다. 프로젝트가 제대로 설정되었는지 확인하려면 `npm start`를 실행하십시오. 모든 것이 제대로 작동한다면 이것을 봐야 한다.

다음으로, Tailwind CSS와 Tailwind UI를 포함하겠습니다. 다음은 이러한 항목을 추가하는 단계입니다.

Tailwind 및 UI를 설치합니다. Tailwind는 빌드 작업을 위한 자체 CLI 툴과 함께 제공되므로 Tailwind 패키지만 있으면 됩니다.

```coffeescript
npm install tailwindcss
npm install @tailwindcss/ui
```

#### 건물에 테일윈드 추가

기존 시작 전에 Tailwind를 빌드하는 단계를 삽입하고 패키지로 스크립트를 빌드합니다.json이 리액트 앱에서 방출되지 않도록 하기 위해서이다. 스크립트는 다음과 같습니다.

```bash
"scripts": {
  "build:tailwind": "tailwindcss build src/tailwind.css -o src/tailwind.output.css",
  "prestart": "npm run build:tailwind",
  "prebuild": "npm run build:tailwind",
  "start": "react-scripts start",
  "build": "react-scripts build",
  "test": "react-scripts test",
  "eject": "react-scripts eject"
}
```

빌드:tailwind 스크립트는 src/tailwind.css 파일을 컴파일하여 src/tailwind에 저장합니다.output.duppet` – 앱이 가져올 항목입니다.

#### Tailwind 소스 CSS 파일 설정

src에서 `Tailwind.css`라는 파일을 생성하고 여기에 붙여 넣으십시오.

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

생성된 CSS 파일을 가져옵니다.

`index.js` 파일의 맨 위에서 다음과 같이 CSS 파일을 가져옵니다.

```js
import React from 'react';
import ReactDOM from 'react-dom';
import './tailwind.css';
import App from './App';
import * as serviceWorker from './serviceWorker';

ReactDOM.render(
  
    
  ,
  document.getElementById('root')
);

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();
```

이제 설정을 마쳤습니다. 코드 쓰기 시작하자.

### 구성 요소 생성

이번 프로젝트에서는 3가지 구성 요소를 갖추게 됩니다. React를 통해 UI의 각 부분에 대한 특정 구성 요소를 구축할 수 있습니다. 사용자가 동영상 제목을 입력할 수 있는 < 구성 요소와 동영상을 표시할 수 있는 < 구성 요소를 구축하겠습니다.

- `App.js` – 나머지 2의 상위 구성 요소가 됩니다. 기본적으로 나머지 두 개의 파일을 렌더링합니다.
- `Header.js` – 앱 헤더를 렌더링하고 제목 프로펠러를 받아들이는 간단한 구성 요소
- `Search.js` – 검색 입력이 포함된 양식을 포함하고 입력 요소를 처리하며 필드를 재설정하는 기능도 포함합니다. 검색 기능을 호출하여 동영상을 표시하는 기능도 포함되어 있습니다.

`src` 디렉토리에 새 폴더 만들기를 시작해 보겠습니다. 우리의 모든 부품이 거기 있을 것이기 때문에 우리는 그것을 `구성 요소`라고 이름 지을 것이다. 이제 `헤더`와 `검색` 구성 요소를 생성해 보겠습니다. 그런 다음 App.js 파일로 가져와야 합니다.

다음과 같이 표시됩니다.

```js
import React from 'react';
import Header from './Components/Header';
import './tailwind.css';
import SearchMovies from './Components/Search';
import Results from './Components/Results';

function App() {

  return (
      <div className="relative width-full">
        <div className="mx-auto overflow-hidden">
          <Header/>
          <SearchMovies/>
        </div>
      </div>
  );
}

export default App;
```

이제 `Header.js` 파일에서 다음 코드를 추가하십시오.

```js
import React from 'react';

class Header extends React.Component {
    render(){
        return ( 
          <nav className = "relative mx-auto bg-indigo-700 max-w-7xl py-4 px-4">
             <div class="container mx-auto">
                  <h1 class="text-white text-center text-3xl pb-4"> Movie Search </h1>
              </div>
          </nav>
    
        )
    }
}

export default Header
```

기본적으로 제목과 함께 머리글 보기를 렌더링하는 클래스 구성 요소입니다.

일단 그렇게 되면, 다음 일은 `검색` 구성 요소를 작업하는 것이다. 다음 코드를 추가하겠습니다.

먼저, 우리는 OMDB에 API 호출을 하는 기능을 만들 것이다.

```js
import React, {useState} from 'react';
  function SearchMovies(){
    const [searching, setSearching] = useState(false);
    const [message, setMessage] = useState(null);
    const [query, setQuery] = useState('');
    const [movies, setMovies] = useState([]);
    const searchMovies = async(e) =>{
        e.preventDefault();
        setSearching(true);
        const url `http://www.omdbapi.com/?&apikey=e1a73560&s=${query}&type="movie"`;
         try{
            const response = await fetch(url);
            const data = await response.json();
            setMessage(null);
            setMovies(data.Search);
            setSearching(false);
         }catch(err){
            setMessage('An unexpected error occured.')
            setSearching(false);
         }
    }
```

이 구성 요소에는 `searchMovies`라는 기능이 있습니다. 그런 다음 API에서 결과를 얻을 수 있도록 sync-ait를 사용하여 API 호출을 합니다. 그 후, 우리는 API가 반환하는 응답을 얻기 위해 `try-catch` 블록을 사용한다. 우리는 또한 useState라고 불리는 리액트 후크를 사용했다.

이름에서 알 수 있듯이, 클래스를 작성하지 않고도 상태 및 기타 대응 기능을 사용할 수 있습니다. useState 후크는 초기 상태인 하나의 인수를 수락한 다음 현재 상태(클래스 구성요소의 "this.state"와 동일)와 이를 업데이트하는 함수를 포함하는 배열을 반환합니다.

우리는 4개의 주를 가지고 있습니다. 첫째는 검색이 수행될 때 검색 프로세스를 처리하는 데 사용되는 검색 상태(검색 시 `로딩…` 텍스트가 `true`로 설정됨)입니다.

두 번째는 메시지 상태이며, API 요청을 할 때 검색 결과(오류)에 따라 메시지를 렌더링합니다. 세 번째는 입력을 API URL로 전달하는 상태를 설정하는 데 사용됩니다. 마지막은 서버에서 가져온 영화 배열을 처리하는 데 사용됩니다.

searchMovies(검색무비)는 말 그대로 사용자의 입력에 불과하다. API URL로 전달하여 서버에 요청하여 결과를 얻습니다.

이제 API에서 데이터를 가져올 수 있게 되었으므로 사용자를 위해 앱에 데이터를 표시해야 합니다. 사용자가 좋아하는 영화의 이름을 입력할 수 있는 매우 간단한 검색 입력을 구축합니다. API에서 동영상 데이터를 조회하여 UI에 응답을 표시합니다.

```xml
return (
    <div className="container mx-auto pt-6">
       <div class="flex justify-center max-w-screen-sm mx-auto overflow-hidden px-10">
       <form class="w-full h-10 pl-3 pr-2 bg-white border rounded-full flex justify-between items-center relative" onSubmit={searchMovies}>
         <input type="text" name="query" placeholder="Search movies by name..."
                 class="appearance-none w-full outline-none focus:outline-none active:outline-none" value={query} onChange={(e) =>setQuery(e.target.value)}/>
         <button type="submit" class="ml-1 outline-none focus:outline-none active:outline-none">
         <svg fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" viewBox="0 0 24 24" class="w-6 h-6">
        <path d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
        </button>
       </form>
     </div>

     <div class="container mx-auto">{searching && !message ? ( <span> loading... </span>): message ? ( <div className = "message"> {message} </div>): (movies.map(movie => ( 
        <div class="inline-block px-2 w-64 h-64">
             <div class="bg-white rounded-lg overflow-hidden shadow-xl my-8 py-4"key={movie.imdbID}>
              <img src={movie.Poster} alt="movieimage" class="w-full h-64"/>
              <div class="p-4">
                  <p class="font-medium text-lg">Title: <span class="font-normal text-base leadin-relaxed">{movie.Title}</span></p>
                   <p class="font-medium text-lg">Year of Release: <span class="font-normal text-base">{movie.Year}</span></p></div>
                    </div>
                  </div> 
                )))}
            </div>
        </div>
    )
```

`return` 섹션에는 이름을 받아들이고 이벤트를 청취하여 검색을 하거나 API를 호출하는 검색 입력이 있습니다.

그런 다음 API에서 반환된 영화 목록을 통해 반복된 다음 div가 있습니다. 이것은 영화 변수에 저장됩니다. 그런 다음 우리는 이름, 연도 등과 같은 영화의 세부 사항을 표시합니다. 원하는 만큼 세부 정보를 표시할 수 있습니다.

일부 검색의 경우 일부 결과에 이미지가 첨부되지는 않습니다. 따라서, 사물을 보기 좋게 만들기 위해서는, 당신은 단지 이미지를 가지고 결과만을 여과하고 표시해야 합니다.

PS: 이미지를 사용자에게 표시할 경우에만 이 작업을 수행합니다.

## 결론

대단해! 이제 우리 앱은 모든 걸 다 해. 요약하면 다음과 같습니다.

우리는 OMDB API를 위한 API 키를 받았습니다. 또한 사용자가 영화를 제목별로 검색한 다음 동영상 제목을 해당 구성 요소의 상태에 저장할 수 있는 구성 요소를 만들었습니다. 다음으로, 기능을 검색 양식으로 전달하여 버튼이나 Enter를 누를 때 적용됩니다. 그런 다음 응답을 영화 상태로 저장하고 API에서 얻은 응답 데이터를 표시하는 검색 구성 요소를 구축했습니다. 그런 다음 사용자에게 결과를 표시합니다.