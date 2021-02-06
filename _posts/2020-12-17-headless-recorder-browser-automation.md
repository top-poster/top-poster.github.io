---
layout: post
title: "헤드리스 레코더: 브라우저 자동화 속도를 높이는 새로운 툴"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/headless-recorder-browser-automation.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/headless-recorder-browser-automation.png?fit=730%2C487&ssl=1)

옛 격언에 따르면 개발 시간의 극히 일부만이 실제 개발에 들어간다고 한다. 소프트웨어를 작성하는 것은 확실히 중요한 일이지만 문서나 테스트와 같은 영역은 무시해서는 안 됩니다. 특히 후자는 큰 부담이 될 수 있고 개발자의 시간이 많이 소요됩니다.

네, 바로 그 부분에서 자동 테스트가 편리합니다. 단위 테스트는 확실한 기반을 형성하지만, 내부 변화에 대해서는 완전히 결정적이거나 강력하지 않습니다. 우리는 종종 보다 강력하고 결정적인 시험을 목적으로 지나치게 엔지니어링하거나 너무 많은 구현 세부 사항을 조롱해야 한다는 것을 알게 된다.

장치 테스트의 많은 함정을 둘러싼 좋은 방법은 구성 요소 또는 통합 테스트를 혼합하는 것입니다. 여기서, 완전한 엔드투엔드 테스트를 수행한다는 아이디어는 확실히 매력적이다.

마지막으로, 우리가 추구하는 80/20(적용 범위/노력) 분포를 제공하는 설정이 있을 수 있습니다. 불행하게도, 종단간 테스트는 유닛 테스트보다 훨씬 더 취약할 수 있으며 훨씬 더 복잡한 설정이 필요합니다. 설상가상으로, 그것들을 쓰는 것도 더 어려울 수 있습니다.

여기가 헤드리스 레코더가 구조하러 오는 곳입니다.

## 헤드리스 레코더란?

헤드리스라는 용어는 일반적으로 적절한 프런트 엔드 또는 UI가 없음을 나타내며, 대신 보다 기술적인 인터페이스를 사용합니다. Headless Recorder 프로젝트의 경우에는 그렇지 않습니다. 여기서, 저자들은 응용 프로그램의 모든 잠재적 목표의 공통적인 특징을 찾기를 원했다. 잠시 후에 다시 생각해 보겠습니다.

먼저, Headless Recorder가 실제로 무엇인지 살펴봅시다. 이 도구는 브라우저에서 웹 사이트에서 수행된 모든 작업을 기록하는 데 사용할 수 있는 작은 도구입니다. 그런 다음 이러한 동작은 플레이라이트 또는 퍼피테어와 함께 사용할 수 있는 자바스크립트 코드로 변환된다(실제로, 헤드리스 레코더는 이전에 퍼피테어 레코더로 알려져 있었다).

이러한 툴은 브라우저 자동화 애플리케이션입니다. 링크를 클릭하거나 양식을 작성하는 등 브라우저가 해야 할 작업을 프로그래밍하는 데 사용됩니다.

이는 이미 저자들이 왜 "헤드리스" 레코더라는 이름을 사용하기로 선택했는지에 대해 어느 정도 밝혀주고 있다. 이른바 브라우저 자동화 프레임워크(Browser Automation Framework)를 위한 레코더로, 일반적으로 UI 없이 실행할 때 헤드리스 모드의 브라우저와 함께 사용됩니다. 레코더에는 실제로 (매우 작은) UI가 있습니다. 녹화를 일시 중지하거나 수정할 수 있는 단추가 함께 제공됩니다.

이 도구는 브라우저 확장 형태로 제공되며 크롬 웹 스토어를 통해 설치할 수 있습니다. 한번 설치하면 쉽게 구르기 쉽습니다.

다음 섹션에서는 플레이라이트(Playwright)를 선택한 프레임워크로 사용합니다.

## 플레이라이트 사용

극작가는 인형극의 현대적 대안이다. API 방법은 대부분의 경우 거의 동일하지만, 다른 방법은 다른 목표를 염두에 두고 설계되었다.

Playwright를 실행하려면 기존 Node.js 프로젝트를 가져와 종속성으로 설치합니다.

```bash
npm i playwright
```

다음과 같습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/headless-recorder-install.png?resize=624%2C357&ssl=1)

보시는 바와 같이, 설치는 웹킷, 크롬 및 Firefox를 포함하여 필요한 모든 브라우저 드라이버도 제공합니다.

Playwright를 사용하는 간단한 스크립트는 다음과 같을 수 있습니다.

```js
const { chromium } = require('playwright');

(async () => {
  const browser = await chromium.launch();
  const page = await browser.newPage();
  await page.goto('http://whatsmyuseragent.org/');
  await page.screenshot({ path: `example.png` });
  await browser.close();
})();
```

이 스크립트는 간단하고 빠르게 쓰이지만 실제 스크립트는 훨씬 길고 제대로 쓰기가 어렵습니다. 여기서 가장 큰 문제는 안정성입니다. 작은 변화나 타이밍이 변하더라도 툴링이 안정적으로 작동하도록 지시하는 것입니다.

Playwright와 같은 도구의 사용 사례는 여러 가지가 있습니다. 처음에 언급했듯이, 엔드투엔드 테스트 작성도 그 중 하나입니다. 여기서, 플레이라이트(Playwright)는 제스트(Jest)와 같은 테스트 프레임워크와 결합하여 모든 엔드 투 엔드 테스트 과제를 해결할 수 있는 강력한 접근 방식을 나타낼 수 있다.

## 공통 워크플로우

이제 우리는 Headless Recorder와 Playwright의 잠재적인 목표물 중 하나에 대해 조금 알게 되었으므로, 우리는 그것을 정확히 어떻게 사용하는지 볼 수 있습니다.

먼저 간단한 작업부터 시작하겠습니다. 아마존에서 항목을 검색하고 결과 수를 가져옵니다. 첫째, 우리는 이 작업을 수동으로 이해하고 수행할 수 있어야 한다. 그렇지 않으면 자동화 시도가 결국 실패할 것입니다.

식별된 단계는 다음과 같을 수 있습니다.

- Amazon 페이지 열기
- 검색 텍스트 입력
- 검색을 클릭합니다.
- 결과 수 선택

이러한 단계는 간단해 보일 수 있지만, 그 요령은 극작가에게 그것을 어떻게 하는지 가르치는 데 있다. 다행히, 우리는 머리 없는 녹음기를 이용해서 우리를 도울 수 있다.

다시 시작하고 단계를 기록하겠습니다. 4단계를 수행한 후 녹화를 중지하고 Playwright 버전을 사용하여 코드를 추출합니다.

다음과 같은 이점이 있습니다.

```js
const { chromium } = require('playwright');
(async () => {
  const browser = await chromium.launch()
  const context = await browser.newContext()
  const page = await context.newPage()

  const navigationPromise = page.waitForNavigation()

  await page.goto('https://www.amazon.com/')

  await page.setViewportSize({ width: 1920, height: 969 })

  await navigationPromise

  await page.waitForSelector('#nav-search #twotabsearchtextbox')
  await page.click('#nav-search #twotabsearchtextbox')

  await page.waitForSelector('.nav-searchbar > .nav-right > .nav-search-submit > #nav-search-submit-text > .nav-input')
  await page.click('.nav-searchbar > .nav-right > .nav-search-submit > #nav-search-submit-text > .nav-input')

  await navigationPromise

  await page.waitForSelector('.s-desktop-width-max > .sg-col-14-of-20 > .sg-col-inner > .a-section > span:nth-child(1)')
  await page.click('.s-desktop-width-max > .sg-col-14-of-20 > .sg-col-inner > .a-section > span:nth-child(1)')

  await browser.close()
})()
```

위의 코드는 그다지 예쁘지 않지만 몇 가지 문제도 있습니다.

- 입력이 누락되었습니다.
- 일부 셀렉터는 매우 취약해 보입니다.
- 네비게이션 약속은 여러 번 사용되지만 한 번만 사용할 수 있습니다.
- 결과가 클릭만 되고 선택되지는 않습니다.

좋은 소식은 생성된 코드가 이미 개선할 수 있는 훌륭한 기반을 제공한다는 것이다.

네비게이션 약속을 없애는 것부터 시작하겠습니다. 이 부분은 Headless Recorder의 설정에서도 수행될 수 있었습니다. 다음으로, 검색 텍스트 상자의 클릭만 사용하는 대신, 채우기 기능을 사용하여 "머리 없는" 단어를 검색합니다. 마지막으로, 결과 텍스트의 클릭을 텍스트 콘텐츠로 대체한다.

수정된 변형은 다음과 같습니다.

```js
const { chromium } = require('playwright');
(async () => {
  const browser = await chromium.launch()
  const context = await browser.newContext()
  const page = await context.newPage()

  await page.goto('https://www.amazon.com/')

  await page.setViewportSize({ width: 1920, height: 969 })

  await page.waitForSelector('#nav-search #twotabsearchtextbox')
  await page.fill('#nav-search #twotabsearchtextbox', 'headless')

  await page.waitForSelector('.nav-searchbar > .nav-right > .nav-search-submit > #nav-search-submit-text > .nav-input')
  await page.click('.nav-searchbar > .nav-right > .nav-search-submit > #nav-search-submit-text > .nav-input')

  await page.waitForSelector('.s-desktop-width-max > .sg-col-14-of-20 > .sg-col-inner > .a-section > span:nth-child(1)')
  const content = await page.textContent('.s-desktop-width-max > .sg-col-14-of-20 > .sg-col-inner > .a-section > span:nth-child(1)')

  console.log(content)

  await browser.close()
})();
```

다시 말하지만, 대부분의 통화는 그대로입니다. 따라서 대부분의 업무는 여기서 보류되었습니다.

이에 따라 2000개가 넘는 결과 중 1-48개가 콘솔에 인쇄된다.

다른 일반적인 사용 사례는 테스트를 위해 브라우저를 자동화하는 것입니다. 여기서는 제스트와 같은 테스트 주자를 사용하여 실제로 결과를 수집할 수 있습니다.

Jest를 추가하는 한 가지 방법은 두 가지 패키지를 설치하는 것입니다.

```bash
npm i jest jest-cli --save-dev
```

.test.js로 접미사가 붙은 파일을 추가하면 기본 구성을 사용하여 테스트로 인식됩니다. 이전 코드를 사용하면 다음과 같은 결과를 얻을 수 있습니다.

```undefined
const { chromium } = require('playwright');

test('has the right search results', async () => {
    const browser = await chromium.launch()
    const context = await browser.newContext()
    const page = await context.newPage()

    await page.goto('https://www.amazon.com/')

    await page.setViewportSize({ width: 1920, height: 969 })

    await page.waitForSelector('#nav-search #twotabsearchtextbox')
    await page.fill('#nav-search #twotabsearchtextbox', 'headless')

    await page.waitForSelector('.nav-searchbar > .nav-right > .nav-search-submit > #nav-search-submit-text > .nav-input')
    await page.click('.nav-searchbar > .nav-right > .nav-search-submit > #nav-search-submit-text > .nav-input')

    await page.waitForSelector('.s-desktop-width-max > .sg-col-14-of-20 > .sg-col-inner > .a-section > span:nth-child(1)')
    const content = await page.textContent('.s-desktop-width-max > .sg-col-14-of-20 > .sg-col-inner > .a-section > span:nth-child(1)')

    await browser.close()

    expect(content).toBe('1-48 of over 2,000 results for');
});
```

더 많은 주장도 가능하다. 일반적으로, 우리는 모든 테스트에서 재사용되는 방법에 공유 기능을 넣어 이러한 테스트의 일부를 단순화할 수 있다.

최적화하기 전에 다음 명령줄에서 출력을 확인하십시오.

```bash
$ npx jest
 PASS  ./my.test.js (14.371 s)
  ✓ has the right search results (2160 ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        25.854 s
Ran all test suites.
```

이제 위의 코드를 세분화해 보겠습니다. 다음과 같은 이점이 있습니다.

```undefined
const { chromium } = require('playwright');

let browser, context, page;

beforeEach(async () => {
    browser = await chromium.launch()
    context = await browser.newContext()
    page = await context.newPage()
});

afterEach(async () => {
    await browser.close()
});

test('has the right search results', async () => {
    await page.goto('https://www.amazon.com/')

    // ... (as above)

    expect(content).toBe('1-48 of over 2,000 results for');
});
```

훌륭합니다. E2E 테스트 프레임워크를 사용하지 않고도 E2E 테스트를 활용하여 회귀를 방지하고 신뢰할 수 있는 연기 테스트를 사용할 수 있습니다.

브라우저 자동화의 또 다른 좋은 용도는 향상된 로깅입니다. 여기서는 버그 보고를 단순화하고 개발을 용이하게 하기 위해 사용자 작업의 재생을 허용하고자 합니다.

## 결론

브라우저 자동화는 많은 발전을 이루었지만, 여전히 올바른 작업이 자동으로 수행되도록 하기 위해 많은 작업이 필요하다는 문제에 직면해 있습니다. Headless Recorder를 사용하면 기술 대신 기능에 집중할 수 있도록 상황을 상당히 개선할 수 있습니다. 기술적인 세부 사항에 주의를 기울여야 하지만 툴링은 생산성을 크게 향상시킵니다.

헤드리스 레코더를 어떻게 하시겠습니까?