---
layout: post
title: "MJML을 사용하여 응답성 전자 메일 작성"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/responsive-emails-mjml.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/responsive-emails-mjml.png?fit=730%2C487&ssl=1)

MJML은 개발자가 모든 장치 및 메일 클라이언트에서 아름답고 응답성이 뛰어난 이메일을 만들 수 있도록 지원하는 최신 이메일 도구입니다. 이 마크업 언어는 응답성 전자 메일을 코딩하는 고통을 줄이기 위해 고안되었습니다.

그것의 의미 구문은 사용하기 쉽고 간단하다. 또한 개발 시간을 단축하는 표준 구성 요소를 갖추고 있어 기능이 풍부합니다. 이 튜토리얼에서는 MJML과 함께 아름답고 응답성이 뛰어난 이메일을 작성하고 여러 이메일 클라이언트를 테스트합니다.

## MJML 설정

Node.js 또는 CLI에서 사용하기 위해 npm과 함께 MJML을 설치할 수 있습니다.

```shell
$ npm install -g mjml
```

## 이메일 작성

시작하려면 `email.mjml` 파일을 만드십시오. 단, 원하는 다른 이름을 선택할 수도 있습니다. 이제 파일을 생성했으므로 응답 전자 메일은 다음 섹션으로 나뉩니다.

- 회사헤더
- 이미지 헤더
- 이메일 소개
- 열 섹션
- 아이콘
- 소셜 아이콘

### 섹션

그 섹션들은 우리의 대응 이메일의 뼈대 역할을 한다. 위와 같이, 우리의 이메일은 6개의 섹션으로 나누어질 것입니다. `email.mjml` 파일의 경우:

```xml
<mjml>
  <mj-body>
    <!-- Company Header -->
    <mj-section background-color="#f0f0f0"></mj-section>
    <!-- Image Header -->
    <mj-section background-color="#f0f0f0"></mj-section>
    <!-- Email Introduction -->
    <mj-section background-color="#fafafa"></mj-section>
    <!-- Columns section -->
    <mj-section background-color="white"></mj-section>
    <!-- Icons -->
    <mj-section background-color="#fbfbfb"></mj-section>
    <!-- Social icons -->
    <mj-section background-color="#f0f0f0"></mj-section>
  </mj-body>
</mjml>
```

위의 내용을 보면 MJ-body와 mj-section이라는 두 가지 MJML 구성 요소를 사용하고 있음을 알 수 있습니다. e-body는 e-메일의 시작점을 정의하고, e-section은 다른 구성 요소를 포함하는 섹션을 정의합니다.

정의된 각 섹션에 대해 각각의 16진수 값을 갖는 `배경색` 속성도 정의된다. 이것은 독자들이 이메일의 섹션을 쉽게 식별할 수 있도록 도와줍니다.

#### 회사헤더

이메일의 이 섹션에는 회사/브랜드 이름만 중앙 배너 위치에 포함됩니다.

```xml
<!-- Company Header -->
<mj-section background-color="#f0f0f0">
  <mj-column>
    <mj-text  font-style="bold"
        font-size="20px"
        align="center"
        color="#626262">
    Central Park Cruise
    </mj-text>
  </mj-column>
</mj-section>
```

mj-column 구성 요소는 열을 정의하는 데 사용됩니다. mj-text 구성 요소는 우리 텍스트 콘텐츠용이며 글꼴 스타일, 글꼴 크기, 색상 등 스타일링 특성을 취한다.

#### 이미지 헤더

이 섹션에서는 배경 이미지와 텍스트 블록이 회사 슬로건을 나타내도록 하겠습니다. 또한 자세한 정보가 있는 페이지를 가리키는 콜 투 액션 버튼도 있습니다.

이미지 헤더를 추가하려면 섹션의 `배경색`을 `배경-url`로 바꾸어야 합니다. 첫 번째 머리글과 마찬가지로 텍스트를 세로 및 가로로 중앙에 배치해야 합니다. 패딩은 그대로입니다.

버튼의 `href`는 버튼 위치를 설정합니다. 배경을 열에 전체 너비로 렌더링하려면 열 너비를 `width="600px"로 600px로 설정하십시오.

이메일의 이 섹션에는 회사/브랜드 이름만 중앙 배너 위치에 포함됩니다.

```xml
<!-- Image Header -->
<mj-section background-url="https://ca-times.brightspotcdn.com/dims4/default/2af165c/2147483647/strip/true/crop/2048x1363+0+0/resize/1440x958!/quality/90/?url=https%3A%2F%2Fwww.trbimg.com%2Fimg-4f561d37%2Fturbine%2Forl-disneyfantasy720120306062055"
        background-size="cover"
        background-repeat="no-repeat">
        <mj-column width="600px">
            <mj-text  align="center"
                color="#fff"
                font-size="40px"
                font-family="Helvetica Neue">Christmas Discount</mj-text>
            <mj-button background-color="#F63A4D" href="#">
                See Promotions
            </mj-button>
        </mj-column>
    </mj-section>
```

이미지 헤더를 사용하기 위해 mj-section 구성 요소에 background-url 특성을 추가한 다음 background-size와 background-repeat 속성을 사용하여 이미지를 스타일링할 것이다.

슬로건 텍스트 블록의 경우, 텍스트를 `align` 속성을 사용하여 중앙에 오도록 수직 및 수평으로 정렬했습니다. 또한 텍스트 색상, 글꼴 크기, 글꼴 패밀리 등을 원하는 대로 설정할 수 있습니다.

콜 투 액션 버튼은 `mj-button` 구성 요소를 사용하여 구현됩니다. background-color 속성으로 버튼 배경색을 지정한 다음 href를 지정하여 링크나 페이지 위치를 지정할 수 있습니다.

#### 이메일 소개

소개 텍스트는 제목, 본문 텍스트 및 통화 수행 단추로 구성됩니다.

```xml
<!-- Intro text -->
<mj-section background-color="#fafafa">
    <mj-column width="400px">
      <mj-text font-style="bold"
        font-size="20px"
        font-family="Helvetica Neue"
        color="#626262">Ultimate Christmas Experience</mj-text>
        <mj-text color="#525252">
            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin rutrum enim eget magna efficitur, eu semper augue semper. Aliquam erat volutpat. Cras id dui lectus. Vestibulum sed finibus lectus, sit amet suscipit nibh. Proin nec commodo purus. Sed eget nulla elit. Nulla aliquet mollis faucibus.
        </mj-text>
        <mj-button background-color="#F45E43" href="#">Learn more</mj-button>
    </mj-column>
</mj-section>
```

#### 열 섹션

이 전자 메일의 섹션에는 두 개의 열이 있습니다. 하나는 설명 이미지를 포함하고 다른 하나는 첫 번째 섹션에서 이미지를 보완하기 위한 텍스트 블록이 포함됩니다.

```xml
<!-- Side image -->
<mj-section background-color="white">
  <!-- Left image -->
  <mj-column>
      <mj-image width="200px"
          src="https://navis-consulting.com/wp-content/uploads/2019/09/Cruise1-1.png"/>
  </mj-column>
  <!-- right paragraph -->
  <mj-column>
      <mj-text font-style="bold"
          font-size="20px"
          font-family="Helvetica Neue"
          color="#626262">
          Amazing Experiences
      </mj-text>
      <mj-text color="#525252">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. 
          Proin rutrum enim eget magna efficitur, eu semper augue semper. 
          Aliquam erat volutpat. Cras id dui lectus. Vestibulum sed finibus 
          lectus.
      </mj-text>
  </mj-column>
</mj-section>
```

왼쪽의 첫 번째 열은 `mj-image` 구성 요소를 사용하여 사용할 이미지를 지정합니다. 이미지는 로컬 파일일 수도 있고, 우리의 경우처럼 원격으로 호스팅되는 이미지가 될 수도 있습니다.

오른쪽의 두 번째 열에는 두 개의 텍스트 블록이 들어 있습니다. 하나는 제목용이고 다른 하나는 본문 텍스트입니다.

#### 아이콘

아이콘 섹션에는 세 개의 열이 있습니다. 전자 메일의 모양에 따라 더 추가할 수도 있습니다.

```xml
<!-- Icons -->
<mj-section background-color="#fbfbfb">
    <mj-column>
        <mj-image width="100px" src="https://191n.mj.am/img/191n/3s/x0l.png" />
    </mj-column>
    <mj-column>
        <mj-image width="100px" src="https://191n.mj.am/img/191n/3s/x01.png" />
    </mj-column>
    <mj-column>
        <mj-image width="100px" src="https://191n.mj.am/img/191n/3s/x0s.png" />
    </mj-column>
</mj-section>
```

각 열에는 아이콘 이미지를 렌더링하는 데 사용되는 고유한 `mj-image` 구성 요소가 있습니다.

#### 소셜 아이콘

이 섹션에는 소셜 미디어 계정을 가리키는 아이콘이 포함됩니다.

```xml
<mj-section background-color="#e7e7e7">
    <mj-column>
        <mj-social>
            <mj-social-element name="instagram" />
        </mj-social>
    </mj-column>
</mj-section>
```

MJML은 소셜네트워크서비스(SNS) 아이콘을 쉽게 표시할 수 있는 mj소셜 컴포넌트가 탑재됐다. 저희 이메일에는 `mj-social-요소`라는 트위터를 사용해 왔습니다.

## 모든 것을 종합하다.

이제 모든 섹션을 구현했으므로 전체 `email.mjml`은 다음과 같이 표시됩니다.

```xml
<mjml>
  <mj-body>
    <!-- Company Header -->
    <mj-section background-color="#f0f0f0">
        <mj-column>
            <mj-text  font-style="bold"
                font-size="20px"
                align="center"
                color="#626262">
            Central Park Cruises
            </mj-text>
        </mj-column>
    </mj-section>
    <!-- Image Header -->
    <mj-section background-url="https://ca-times.brightspotcdn.com/dims4/default/2af165c/2147483647/strip/true/crop/2048x1363+0+0/resize/1440x958!/quality/90/?url=https%3A%2F%2Fwww.trbimg.com%2Fimg-4f561d37%2Fturbine%2Forl-disneyfantasy720120306062055"
        background-size="cover"
        background-repeat="no-repeat">
        <mj-column width="600px">
            <mj-text  align="center"
                color="#fff"
                font-size="40px"
                font-family="Helvetica Neue">Christmas Discount</mj-text>
            <mj-button background-color="#F63A4D" href="#">
                See Promotions
            </mj-button>
        </mj-column>
    </mj-section>
    <!-- Email Introduction -->
    <mj-section background-color="#fafafa">
        <mj-column width="400px">
          <mj-text font-style="bold"
            font-size="20px"
            font-family="Helvetica Neue"
            color="#626262">Ultimate Christmas Experience</mj-text>
            <mj-text color="#525252">
                Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin rutrum enim eget magna efficitur, eu semper augue semper. Aliquam erat volutpat. Cras id dui lectus. Vestibulum sed finibus lectus, sit amet suscipit nibh. Proin nec commodo purus. Sed eget nulla elit. Nulla aliquet mollis faucibus.
            </mj-text>
            <mj-button background-color="#F45E43" href="#">Learn more</mj-button>
        </mj-column>
    </mj-section>
    <!-- Columns section -->
    <mj-section background-color="white">
        <!-- Left image -->
        <mj-column>
            <mj-image width="200px"
                src="https://navis-consulting.com/wp-content/uploads/2019/09/Cruise1-1.png"/>
        </mj-column>
        <!-- right paragraph -->
        <mj-column>
            <mj-text font-style="bold"
                font-size="20px"
                font-family="Helvetica Neue"
                color="#626262">
                Amazing Experiences
            </mj-text>
            <mj-text color="#525252">
                Lorem ipsum dolor sit amet, consectetur adipiscing elit. 
                Proin rutrum enim eget magna efficitur, eu semper augue semper. 
                Aliquam erat volutpat. Cras id dui lectus. Vestibulum sed finibus 
                lectus.
            </mj-text>
        </mj-column>
    </mj-section>
    <!-- Icons -->
    <mj-section background-color="#fbfbfb">
        <mj-column>
            <mj-image width="100px" src="https://191n.mj.am/img/191n/3s/x0l.png" />
        </mj-column>
        <mj-column>
            <mj-image width="100px" src="https://191n.mj.am/img/191n/3s/x01.png" />
        </mj-column>
        <mj-column>
            <mj-image width="100px" src="https://191n.mj.am/img/191n/3s/x0s.png" />
        </mj-column>
    </mj-section>
    <!-- Social icons -->
    <mj-section background-color="#e7e7e7">
        <mj-column>
            <mj-social>
                <mj-social-element name="instagram" />
            </mj-social>
        </mj-column>
    </mj-section>
  </mj-body>
</mjml>
```

### 응용 프로그램 실행

이제 전자 메일 작성을 완료했으므로, 어떻게 생겼는지 확인하기 위해 컴파일할 수 있습니다. 이를 위해 터미널에 다음을 입력합니다.

```css
mjml -r email.mjml -o .
```

- MJML이 `-r` 파일을 읽고 컴파일할 수 있도록 합니다.
- -o.는 MJML에 컴파일된 `mjml` 출력을 동일한 디렉토리에 저장하도록 지시합니다.

MJML이 컴파일을 마치면 동일한 디렉토리에 `email.html` 파일이 표시됩니다. 즐겨찾는 전자 메일 클라이언트 또는 브라우저로 열어 보면 아래 이미지와 비슷할 것입니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/completed-email-file-example.png?resize=592%2C909&ssl=1)

## 결론

방금 살펴본 것처럼 MJML은 여러 브라우저와 클라이언트에서 응답하는 우수한 HTML 전자 메일을 만들 수 있도록 도와줍니다.