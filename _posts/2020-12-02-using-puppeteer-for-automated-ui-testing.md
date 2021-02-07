---
layout: post
title: "자동 UI 테스트에 Puppeteer 사용"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/puppeteer-automated-ui-testing.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/puppeteer-automated-ui-testing.png?fit=730%2C487&ssl=1)

첨단기술의 시대에는 웹 스크래핑, 테스트, 모니터링을 위한 스크립트 작성이 까다로울 수 있다. 그래서 Google Chrome 팀은 Puppeteer라는 간단하고 사용하기 쉬운 API를 통해 JavaScript를 통해 Chromium 또는 Chrome 브라우저에서 일반적인 작업을 프로그래밍 방식으로 수행할 수 있는 도구를 제공했습니다.

이 블로그 게시물에서 Puppeteer에 대해 알아보고 이를 사용하여 웹 페이지를 스크래치하고 프로젝트에 대한 자동 UI 테스트를 기록하는 방법에 대해 알아봅니다.

## 전제조건

이 튜토리얼의 경우 JavaScript 및 Node.js에 대한 기본 지식이 필요합니다.

## 인형극이란 무엇인가?

구글에 따르면 퍼피터는 DevTools Protocol을 통해 헤드리스 크롬이나 크롬을 제어할 수 있는 고급 API를 제공하는 노드 라이브러리이다. 머리 없는 완전한 크롬이나 크롬을 사용하도록 구성할 수도 있습니다."

Puppeteer를 사용하면 웹 사이트를 스크래치하고, 페이지의 스크린샷 및 PDF를 생성하고, SPA의 크롤러 역할을 하며, 사전 렌더링된 콘텐츠를 생성하고, 폼 제출, 테스트 UI, 웹 페이지 및 추가 정보를 DOM API를 사용하여 자동화하고, 마지막으로 성능 분석을 자동화할 수 있습니다.

웹 스크래핑에 대한 전반적인 이해에 도움이 되는 취업 포털을 스크래핑하여 Puppeteer의 작동 방식을 설명하겠습니다.

준비됐어? 들어가자.

## 노드 프로젝트 설정

노드 프로젝트 설정부터 시작합니다.

- Node.js 12.12.0 이상 설치
실 또는 npm 설치
- Node.js 12.12.0 이상 설치
- 실 또는 npm 설치

```bash
mkdir JobScrapper
cd JobScrapper

yarn add puppeteer
```

여기에서 읽을 수 있는 크롬 브라우저를 다운로드하지 않으려면 `퍼피터 코어`를 사용할 수 있습니다.

## 'jobScript.js' 파일 생성

방금 만든 스크립트 파일 내에 다음 코드를 추가합니다.

```js
const puppeteer = require("puppeteer");
const jobUrl = process.env.JOB_URL;
let page;
let browser;
let cardArr = [];
class Jobs {
    static async init() {
        // console.log('Loading Page ...')
        browser = await puppeteer.launch();
        page = await browser.newPage();
        await page.goto(jobUrl, { waitUntil: "networkidle2" });
        await page.waitForSelector(".search-card");
    }
    static async resolve() {
        await this.init();
        // console.log('Grabbing List of Job URLS ...')
        const jobURLs = await page.evaluate(() => {
            const cards = document.querySelectorAll(".search-card");
            cardArr = Array.from(cards);
            const cardLinks = [];
            cardArr.map(card => {
                const cardTitle = card.querySelector(".card-title-link");
                const cardDesc = card.querySelector(".card-description");
                const cardCompany = card.querySelector(
                    'a[data-cy="search-result-company-name"]'
                );
                const cardDate = card.querySelector(".posted-date");
                const { text } = cardTitle;
                const { host } = cardTitle;
                const { protocol } = cardTitle;
                const pathName = cardTitle.pathname;
                const query = cardTitle.search;
                const titleURL = protocol + "//" + host + pathName + query;
                const company = cardCompany.textContent;
                cardLinks.push({
                    jobText: text,
                    jobURLHost: host,
                    jobURLPathname: pathName,
                    jobURLSearchQuery: query,
                    jobURL: titleURL,
                    jobDesc: cardDesc.innerHTML,
                    jobCompany: company,
                    jobDate: cardDate.textContent
                });
            });
            return cardLinks;
        });
        return jobURLs;
    }
    static async getJobs() {
        const jobs = await this.resolve();
        await browser.close();
        // console.log(jobs)
        return jobs;
    }
}
export default Jobs;
```

여기서 잡스 클래스에는 세 가지 중요한 방법이 있는데, `이니트`, `해결`, `겟잡스`이다.

Init 방법은 Puppeteer 인스턴스를 초기화하고 브라우저 개체를 생성하며, 이 개체는 `newPage() 메서드로 브라우저에 새 페이지를 만드는 데 사용됩니다. 브라우저가 방문하고자 하는 URL로 goto()를 호출하고 긴 폴링이나 다른 사이드 활동을 수행하는 페이지에 유용한 network idle2를 지정합니다. 그런 다음 지정된 클래스 `.search-card`의 특정 HTML 요소가 뷰포트에 로드될 때까지 기다립니다.

두 번째 방법인 리졸브 방법은 Init 방법을 호출하고, 열린 페이지를 `평가`하며, 모든 HTML 요소를 `.search-card`로 쿼리한다. 각 항목을 반복하고 작업 제목, 게시된 날짜, 회사 및 설명과 같은 특정 정보를 검색한 다음 표시할 배열로 푸시합니다.

마지막으로 get Jobs 방법은 발견된 모든 작업 목록을 가져오고 호출자에게 돌아가기 위한 해결 방법을 간단히 부른다.

이제 `server.js` 파일을 만들어 다음 코드를 입력하여 작업을 표시합니다.

```js
// Create a simple Express API
const express = require("express");

// Require the Job Scrapper
const Jobs = require("./jobScript");

// Instantiate Express server
const app = express();
const port = 9000;

// Get Jobs from with the Scrapper and return a Job with jobs
app.get("/", async (req, res) => {
  const jobs = await Jobs.getJobs();
  res.json(jobs);
});

// Listen to port 9000
app.listen(port, () => {});

// PS: If you encounter problem with `Module not found` run:
// npm i puppeteer
// again
```

## 스크래치된 작업 표시

아래는 제공된 작업 웹 사이트 URL을 기준으로 성공적으로 폐기된 모든 작업의 JSON 샘플입니다.

## 인형극 기록기란 무엇인가?

이제 우리는 Puppeteer가 무엇이고 그것이 무엇을 할 수 있는지 알았으니 Puppeteer의 중요한 사용법인 자동화된 테스트를 살펴보도록 하겠습니다.

Chrome 확장자인 Puppeteer Recorder를 사용하여 브라우저의 상호 작용과 활동을 기록한 다음 자동 테스트를 위한 Puppeteer 스크립트를 생성할 수 있습니다.

다음은 Puppeteer Recorder Chrome 확장에서 수행할 수 있는 유용한 작업 목록입니다.

- 웹 사이트 클릭과 다양한 이벤트 유형을 쉽게 기록할 수 있습니다.
실행된 이벤트와 현재 실행 중인 이벤트를 쉽게 표시할 수 있습니다.
웨이트 포 네비게이션, 세트뷰포트 등 유용한 조항이 있다.
내장 복사-클립보드 기능
구성 옵션이 서로 다릅니다.
`data-id` 특성을 가진 요소를 쿼리할 수 있습니다.
자동 Puppetter
- 웹 사이트 클릭과 다양한 이벤트 유형을 쉽게 기록할 수 있습니다.
실행된 이벤트와 현재 실행 중인 이벤트를 쉽게 표시할 수 있습니다.
웨이트 포 네비게이션, 세트뷰포트 등 유용한 조항이 있다.
내장 복사-클립보드 기능
구성 옵션이 서로 다릅니다.
`data-id` 특성을 가진 요소를 쿼리할 수 있습니다.
자동 Puppetter
- 웹 사이트 클릭과 다양한 이벤트 유형을 쉽게 기록할 수 있습니다.
실행된 이벤트와 현재 실행 중인 이벤트를 쉽게 표시할 수 있습니다.
웨이트 포 네비게이션, 세트뷰포트 등 유용한 조항이 있다.
내장 복사-클립보드 기능
구성 옵션이 서로 다릅니다.
`data-id` 특성을 가진 요소를 쿼리할 수 있습니다.
자동 Puppetter
- 웹 사이트 클릭과 다양한 이벤트 유형을 쉽게 기록할 수 있습니다.
- 실행된 이벤트와 현재 실행 중인 이벤트를 쉽게 표시할 수 있습니다.
- 웨이트 포 네비게이션, 세트뷰포트 등 유용한 조항이 있다.
- 내장 복사-클립보드 기능
- 구성 옵션이 서로 다릅니다.
- `data-id` 특성을 가진 요소를 쿼리할 수 있습니다.
- 자동 Puppetter

먼저, Chrome에 추가를 클릭하여 Puppeteer Recorder용 Chrome 확장을 설치합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/puppeteer-chrome-extension-recorder.png?resize=730%2C255&ssl=1)

## Puppeteer Recorder 탐색 중

다음은 Puppeteer Recorder에서 세션을 기록하는 기본 사항입니다. 시작하려면 아이콘을 선택하고 기록을 클릭합니다. 입력 요소를 입력한 후 탭을 누릅니다. 다른 링크를 클릭하고 요소를 입력하여 세션을 기록할 수 있습니다.

각 페이지를 클릭한 후 페이지가 완전히 로드될 때까지 기다리는 것이 중요합니다. 녹화를 중지하려면 일시 중지를 클릭합니다. [계속] 단추를 사용하여 녹화를 재개하고 [중지] 단추를 사용하여 녹화를 완전히 중지할 수 있습니다. 마지막으로 Copy to Clipboard(클립보드에 복사)를 클릭하여 생성된 스크립트를 복사할 수 있습니다.

## 자동 UI 테스트

자동화된 UI 테스트는 웹 사이트를 탐색하고 일반 사용자의 기능을 모방하는 방식으로 기술을 사용하여 응용 프로그램이 제대로 작동하는지 여부를 테스트하는 데 사용됩니다. 웹 사이트가 활성화되기 전에 웹 개발 중에 웹 사이트의 오류, 버그 및 손상된 링크를 식별할 수 있도록 도와줍니다.

셀레늄과 테스트 완료를 포함하여 이를 달성하기 위해 많은 도구가 개발되었지만, 우리는 Puppeteer를 통해 이를 달성하는 방법을 시연할 것이다.

이제 자동 UI 테스트를 기록하겠습니다.

녹화를 시작하기 전에 설정으로 이동하여 원활한 녹화를 위해 `헤드리스` 및 `찾아보기 대기` 옵션을 선택 취소합니다.

Puppeteer Extension 아이콘을 클릭한 다음 Record:를 클릭합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/puppeteer-chrome-extension-recording-e1605803142344.png?resize=730%2C407&ssl=1)

그런 다음 주소 표시줄에 http://www.google.com을 입력하고 브라우저 주변을 클릭하면 Puppeteer Recorder가 몇 가지 이벤트를 기록할 수 있습니다.

Puppeteer 아이콘 표시줄에서 `wait` 및 `rec` 상태를 확인하십시오. 다음 이벤트 또는 클릭 시 힌트를 제공합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/recorded-screen-puppeteer.png?resize=414%2C645&ssl=1)

UI 테스트에 필요한 이벤트를 충분히 기록했으면 중지를 클릭할 수 있습니다. 그런 다음 생성된 Puppeteer 스크립트를 복사하고 Node/Express 서버를 사용하여 테스트를 실행합니다.

아래 스크립트에 일부 이벤트를 기록했습니다.

```js
  const puppeteer = require("puppeteer");
  (async () => {
    const browser = await puppeteer.launch({ headless: false });
    const page = await browser.newPage();
    await page.goto(
      "https://www.google.com/search?q=what+is+puppeteer+js&oq=wh&aqs=chrome.0.69i59j69i64j0l3j5l3.7647j0j7&sourceid=chrome&ie=UTF-8"
    );
    await page.setViewport({ width: 1366, height: 669 });
    await page.waitForSelector(
      ".g:nth-child(3) > .rc:nth-child(1) > .r > a > .LC20lb"
    );
    await page.click(".g:nth-child(3) > .rc:nth-child(1) > .r > a > .LC20lb");
    await page.waitForSelector(
      ".blog_post-main_content > .blog_post-body > .blog_post_body > p> a:nth-child(3)"
    );
    await page.click(
      ".blog_post-main_content > .blog_post-body > .blog_post_body > p > a:nth-child(3)"
    );
    await browser.close();
  })();
```

## Puppetter 스크립트 실행

Puppeteer 스크립트를 실행하는 것은 테스트 환경을 설정한 후 간단합니다. 노드/익스프레스를 사용하면 생성된 코드를 붙여넣고 실행할 수 있습니다.

```js
const express = require("express");
const app = express();
const port = 9000;
app.get("/test", (req, res) => {
  const puppeteer = require("puppeteer");
  (async () => {
    const browser = await puppeteer.launch({ headless: false });
    const page = await browser.newPage();
    await page.goto(
      "https://www.google.com/search?q=what+is+puppeteer+js&oq=wh&aqs=chrome.0.69i59j69i64j0l3j5l3.7647j0j7&sourceid=chrome&ie=UTF-8"
    );
    await page.setViewport({ width: 1366, height: 669 });
    await page.waitForSelector(
      ".g:nth-child(3) > .rc:nth-child(1) > .r > a > .LC20lb"
    );
    await page.click(".g:nth-child(3) > .rc:nth-child(1) > .r > a > .LC20lb");
    await page.waitForSelector(
      ".blog_post-main_content > .blog_post-body > .blog_post_body > p > a:nth-child(3)"
    );
    await page.click(
      ".blog_post-main_content > .blog_post-body > .blog_post_body > p > a:nth-child(3)"
    );
    await browser.close();
  })();
});
app.listen(port, () => {});
```

자, 이제 끝장이야! 축하해요.

## 결론

이 글에서, 우리는 Puppeteer에 대해 배웠고 그것을 가지고 웹페이지를 긁어내는 방법에 대해 배웠다. 또한 Puppeteer Recorder에 대해 알아보고 이를 사용하여 UI 테스트를 자동화했습니다. 항상 그렇듯이, 당신은 나의 GitHub Repository에서 이 블로그 게시물에 있는 모든 코드를 얻을 수 있다. 해피 코딩!