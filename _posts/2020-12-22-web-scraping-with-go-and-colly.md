---
layout: post
title: "Colly와 함께 이동에서 웹 크롤러 만들기"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/webscrapingingo.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/webscrapingingo.png?fit=730%2C487&ssl=1)

웹 스크래핑은 전용 API가 없어 액세스하기 어려웠을 웹 사이트의 데이터를 검사, 구문 분석 및 추출할 수 있는 기술입니다. 웹 크롤링은 "씨드" URL로 시작하여 인터넷을 체계적으로 탐색하고, 크롤러가 방문한 각 페이지에서 찾은 링크를 재귀적으로 방문하는 것을 포함한다.

Colly는 웹 스크래퍼와 크롤러 모두를 위한 바둑 패키지이다. Go의 net/HTTP(네트워크 통신용)와 goquery("jQuery 유사" 구문을 사용하여 HTML 요소를 대상으로 지정)를 기반으로 합니다.

이 기사에서는 생일이 일정 날짜인 연예인들의 세부 사항을 스크랩할 것이다. 우리는 IMDB 웹사이트에서 이 데이터를 얻기 위해 콜리의 힘을 이용할 것이다.

## 앱 종속성 시작 및 설치

계속하려면 Go(기본 설정 1.14 이상)가 설치된 시스템이 있어야 합니다.

> 참고: 아래 사용된 셸 명령은 Linux/macOS용이지만 운영 체제와 동일한 명령이 다를 경우 언제든지 사용하십시오.

이 코드를 사용할 디렉토리를 생성하고 새 Go 모듈을 초기화합니다.

```shell
$ mkdir birthdays-today && cd birthdays-today
$ go mod init gitlab.com/idoko/birthdays-today
```

외부 패키지는 HTTP 요청을 만들고 HTML DOM을 구문 분석할 수 있는 기능이 있어 Colly가 유일하게 설치해야 합니다. 아래 명령을 실행하여 애플리케이션 종속성으로 가져옵니다.

```shell
$ go get github.com/go-colly/colly
```

## Colly와 친해짐

콜리의 중심에는 콜렉터(Collector)라는 요소가 있다. 수집기는 네트워크 호출을 담당하며 구성할 수 있으므로 `사용자 에이전트` 문자열을 수정하거나 특정 도메인으로 탐색할 URL을 제한하거나 크롤러를 비동기식으로 실행할 수 있습니다. 아래 코드로 새 "Collector"를 초기화할 수 있습니다.

```undefined
c := colly.NewCollector(
  // allow only IMDB links to be crawled, will visit all links if not set
  colly.AllowedDomains("imdb.com", "www.imdb.com"),
  // sets the recursion depth for links to visit, goes on forever if not set
  colly.MaxDepth(3),
  // enables asynchronous network requests
  colly.Async(true),
)
```

또는 콜리에게 다음과 같은 전화만으로 기본 옵션을 사용하도록 할 수 있습니다.

```undefined
c := colly.NewCollector()
```

수집기는 OnRequest, On 등의 콜백도 할 수 있다.HTML을 첨부합니다. 예를 들어 콜리는 수집의 라이프사이클 방식과 유사하게 콜리(Colly)가 HTTP 요청을 하기 직전에 `OnRequest` 메소드를 호출한다. 지원되는 콜백의 전체 목록은 콜리의 godoc 페이지에서 확인할 수 있습니다.

보다 복잡한 스크레이퍼의 경우 방문한 URL 및 쿠키를 Redis에 저장하도록 수집기를 구성하거나 후드에서 어떤 일이 벌어지고 있는지 확인하기 위해 디버거를 연결할 수도 있습니다.

## 대상 웹 사이트로 콜리 설정

메인(main)과 크롤(crawl)이라는 두 가지 기능을 만들어 보자. 우리 프로그램은 자동으로 메인(main)으로 전화를 걸며, 이는 다시 크롤(craw)로 전화를 걸어 웹 페이지에서 필요한 정보를 추출한다. 나중에 원하는 월과 요일을 명령행 논조로 읽는 `메인`을 확장해 언제든지 생일 목록을 얻을 수 있도록 하겠습니다.

```java
package main

import (
  "encoding/json"
  "flag"
  "fmt"
  "github.com/gocolly/colly"
  "log"
  "strings"
)

func main() {
  crawl()
}

func crawl() {
  c := colly.NewCollector(
    colly.AllowedDomains("imdb.com", "www.imdb.com"),
  )
  infoCollector := c.Clone()

  c.OnRequest(func(r *colly.Request) {
    fmt.Println("Visiting: ", r.URL.String())
  })

  infoCollector.OnRequest(func(r *colly.Request) {
    fmt.Println("Visiting Profile URL: ", r.URL.String())
  })

  c.Visit("https://www.imdb.com/search/name/?birth_monthday=12-20")
}
```

위의 코드 조각은 수집기를 초기화하고 "IMDB" 도메인으로 제한한다. 우리의 스크레이퍼는 두 개의 하위 작업(생일 목록 가져오기 및 개별 연예인 페이지 가져오기)으로 구성되기 때문에, 우리는 `c`를 사용하여 생성된 수집기를 복제한다.복제()포함 우리는 또한 수집가들이 언제 실행되기 시작하는지 알 수 있도록 서로 다른 `On Request` 구현체도 첨부했다. 마지막으로 `c`라고 부른다.12월 20일 출생한 연예인이 모두 적힌 씨(seed) URL로 방문하면 된다.

## Colly를 사용하여 HTML 페이지 이동

기본적으로 IMDB 목록에는 페이지당 50개의 항목이 표시되고 다음 페이지로 이동하기 위한 다음 링크가 표시됩니다. 다음 페이지를 반복적으로 방문하여 `On`을 첨부하여 전체 목록을 확인할 수 있습니다.HTML 콜백은 c를 호출하기 직전에 아래의 코드 블록을 `크롤` 함수의 끝에 연결하여 원래 수집기 객체에 콜백합니다.방문:

```css
c.OnHTML("a.lister-page-next", func(e *colly.HTMLElement) {
   nextPage := e.Request.AbsoluteURL(e.Attr("href"))
   c.Visit(nextPage)
})
```

코드는 Next 링크를 대상으로 하며 완전한 절대 URL로 변환합니다. 그런 다음 URL을 방문하면 다음 페이지에서 동일한 현상이 발생합니다. 이와 같은 빠르고 자동화된 웹 사이트 방문으로 IP 주소가 차단될 수 있습니다. Colly의 제한 규칙을 탐색하여 요청 간의 임의의 지연을 시뮬레이션할 수 있습니다.

마찬가지로 다른 `On`을 첨부합니다.첫 번째 수집가에게 개별 연예인 페이지를 방문하기 위한 HTML 수신기:

```css
c.OnHTML(".mode-detail", func(e *colly.HTMLElement) {
   profileUrl := e.ChildAttr("div.lister-item-image > a", "href")
   profileUrl = e.Request.AbsoluteURL(profileUrl)
   infoCollector.Visit(profileUrl)
})
```

위의 코드 조각에서, 우리는 `infoCollector`가 개별 페이지를 방문하도록 위임한다. 이렇게 하면 페이지가 준비되면 듣고 필요한 데이터를 추출합니다.

## 구조물에 HTML 정리

다음으로 각 연예인의 데이터를 담을 영화, 스타 등의 구도를 세워보자. 영화 구조는 페이지에 나열된 그 사람의 최고 영화들의 세부 사항을 나타내며, 스타 구조는 그들의 생체 데이터를 포함하고 있다. main.go 파일의 main 함수 바로 앞에 다음 조각을 추가합니다.

```cpp
type movie struct {
   Title string
   Year string
}
type star struct {
   Name  string
   Photo string
   JobTitle string
   BirthDate string
   Bio string
   TopMovies []movie
}
```

그런 다음 새 `On`을 첨부합니다.HTML은 크롤 함수의 infoCollector를 수신합니다. 콜백은 프로필 컨테이너(ID가 `콘텐츠 2-와이드`인 디브)를 통해 그 안에 들어 있는 연예인 데이터를 추출해 출력한다.

컨텍스트의 경우 샘플 IMDB 프로파일 페이지는 다음과 같습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/imdbprofilepageofpersiawhite.png?resize=730%2C411&ssl=1)

```cpp
infoCollector.OnHTML("#content-2-wide", func(e *colly.HTMLElement) {
   tmpProfile := star{}
   tmpProfile.Name = e.ChildText("h1.header > span.itemprop")
   tmpProfile.Photo = e.ChildAttr("#name-poster", "src")
   tmpProfile.JobTitle = e.ChildText("#name-job-categories > a > span.itemprop")
   tmpProfile.BirthDate = e.ChildAttr("#name-born-info time", "datetime")

   tmpProfile.Bio = strings.TrimSpace(e.ChildText("#name-bio-text > div.name-trivia-bio-text > div.inline"))

   e.ForEach("div.knownfor-title", func(_ int, kf *colly.HTMLElement) {
      tmpMovie := movie{}
      tmpMovie.Title = kf.ChildText("div.knownfor-title-role > a.knownfor-ellipsis")
      tmpMovie.Year = kf.ChildText("div.knownfor-year > span.knownfor-ellipsis")
      tmpProfile.TopMovies = append(tmpProfile.TopMovies, tmpMovie)
   })
   js, err := json.MarshalIndent(tmpProfile, "", "    ")
   if err != nil {
      log.Fatal(err)
   }
   fmt.Println(string(js))
})
```

이 페이지에서 바이오 데이터를 추출하는 것 외에도, 위의 코드는 해당 사람이 출연한 상위 영화(클래스가 `제목`으로 알려진 divs로 식별됨)를 순환시켜 영화 목록에 저장한다. 그런 다음 `별` 구조의 포맷된 JSON 표현을 인쇄합니다. 유명인사에 추가하거나 데이터베이스에 저장할 수도 있습니다.

## 플래그를 사용하여 CLI 인수 수신

스크레이퍼는 특정 날짜(01/11)의 생일 목록만 가져오지만 거의 준비되었습니다. 보다 동적으로 만들기 위해 CLI 플래그에 대한 지원을 추가하여 언제든지 명령줄 인수로 전달할 수 있습니다.

현재 `메인` 기능을 아래 코드로 교체합니다.

```css
func main() {
   month := flag.Int("month", 1, "Month to fetch birthdays for")
   day := flag.Int("day", 1, "Day to fetch birthdays for")
   flag.Parse()
   crawl(*month, *day)
}
```

위의 코드 블록을 사용하면 "go run./main.go--month=10 --day=10"과 같이 관심 있는 월과 날짜를 지정할 수 있습니다.

다음으로 크롤 기능을 수정하여 서명을 펑크롤()에서 펑크롤(월, 일)로 변경하여 월 및 일 인수를 수락합니다.

`c`가 포함된 줄을 교체하여 시드 URL의 함수 인수를 사용하십시오.아래 코드와 함께 (https://www.imdb.com/search/name/?birth_monthday=10-25")를 방문하십시오.

```undefined
startUrl := fmt.Sprintf("https://www.imdb.com/search/name/?birth_monthday=%d-%d", month, day)
c.Visit(startUrl)
```

다음 명령을 사용하여 스크래퍼를 빌드하고 실행합니다.

```shell
$ go build ./main.go
$ ./main --month=10 --day=10
```

아래 스크린샷과 유사한 응답을 받아야 합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/webcrawlerrunning.png?resize=1912%2C1168&ssl=1)

## 결론

이 기사에서는 웹 사이트를 탐색하고 우리의 요구를 충족하기 위해 방문하는 페이지에서 정보를 추출하는 방법에 대해 배웠습니다. 전체 소스 코드는 GitLab에서 사용할 수 있다. 콜리를 더 탐험하고 싶으십니까? 다음은 도움이 될 수 있는 몇 가지 링크입니다.

- Colly의 모범 사례 - Colly를 통해 훨씬 더 복잡한 작업을 수행할 수 있는 방법
- GoQuery - 콜리가 사용하는 이 제품은 net/HTML 및 cascadia 패키지를 활용하여 HTML DOM 트리를 이동하고 조작합니다.