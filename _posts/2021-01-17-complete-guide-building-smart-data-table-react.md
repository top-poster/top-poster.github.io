---
layout: post
title: "React Table : 예제가 포함 된 완전한 튜토리얼
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2019/11/react-table-tutorial-examples.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2019/11/react-table-tutorial-examples.png?fit=730%2C487&ssl=1)

편집자 주 :이 React Table 자습서는 react-table v7에 포함 된 업데이트를 설명하기 위해 2021 년 1 월에 마지막으로 업데이트되었습니다.
 

테이블 UI는 UI에서 복잡한 데이터를 구성하는 가장 효율적인 방법 중 하나이기 때문에 웹 제품에서 매우 일반적입니다.
 

테이블 UI를 처음부터 빌드하는 것은 어려운 작업 일 수 있으며 특히 React 테이블은 개발자에게 골칫거리로 알려져 있습니다.
 다행스럽게도 React 테이블을 훨씬 더 간단하고 보람있게 만드는 경험을 할 수있는 다양한 도구와 라이브러리가 있습니다. 특히 React Table이 있습니다.
 

이 튜토리얼에서는 react-table을 사용하여 기본적인 정렬 및 검색 기능을 갖춘 스마트 React 데이터 테이블 UI를 구축하는 방법을 보여줍니다.
 

다음 내용을 자세히 다룰 것입니다.
 

- React 테이블은 무엇에 사용됩니까?
 
- React Table이란?
반응 표 v7
`반응 테이블`을 사용하는 경우
`react-table`을 사용하지 않는 경우
`반응 테이블`사용 사례
 
- 반응 표 v7
 
- `반응 테이블`을 사용하는 경우
 
- `react-table`을 사용하지 않는 경우
 
- `반응 테이블`사용 사례
 
- React 테이블 UI 기능
 
- React 테이블 UI의 일반적인 UX 문제
 
- 나만의 React 테이블 UI를 구축해야하는 경우
 
- React Table 예제 : `react-table`을 사용하여 React 테이블 구성 요소 빌드
Axios로 API 호출
앱에 `반응 표`추가
`반응 테이블`의 맞춤 스타일링
 
- Axios로 API 호출
 
- 앱에 `반응 표`추가
 
- `반응 테이블`의 맞춤 스타일링
 
- `useFilters`로 검색 기능 추가
 
- `useSortBy`로 정렬 추가
 
- React Table 대안
 

## React 테이블은 무엇에 사용됩니까?
 

React 테이블 UI의 일반적인 사용 사례에는 재무 보고서, 스포츠 리더 보드, 가격 및 비교 페이지에 대한 데이터 표시가 포함됩니다.
 

표를 광범위하게 사용하는 일부 제품은 다음과 같습니다.
 

- 에어 테이블
 
- Asana 목록보기
 
- 아사나 타임 라인
 
- Google 스프레드 시트
 
- 개념 표
 

React Table을 사용하는 기술 대기업 중에는 Google, Apple 및 Microsoft가 있습니다.
 

## React Table이란?
 

React Table은 React에서 가장 널리 사용되는 테이블 라이브러리 중 하나입니다.
 글을 쓰는 시점에 GitHub에 13,000 개 이상의 별이 있으며, 자주 업데이트를 받고, Hook을 지원합니다.
 `react-table` 라이브러리는 매우 가볍고 간단한 테이블에 필요한 모든 기본 기능을 제공합니다.
 

### 반응 표 v7
 

2020 년 3 월 React Table 제작자 인 Tanner Linsley는 React Table v7을 출시했으며,이를“전체 라이브러리를 후크 전용 UI / 스타일 / 마크 업에 구애받지 않는 테이블 구축 유틸리티로 리팩토링하기 위해 수년에 걸친 작업의 정점입니다.
 .”
 

React Table v7은 복잡한 데이터 그리드의 논리적 기능을 기본`useTable` 후크에서 반환하는 단일하고 성능이 뛰어나고 확장 가능하며 의견이없는 API로 구성하는 데 도움이되도록 설계된 React Hooks 및 플러그인 모음으로 구성됩니다.
 

헤드리스 유틸리티 인 React Table v7은 데이터 테이블 UI 요소를 즉시 렌더링하거나 제공하지 않습니다.
 즉, React Table에서 제공하는 후크의 상태 및 콜백을 사용하여 자신의 테이블 마크 업을 렌더링해야합니다.
 

가장 최근의 안정된`react-table` 릴리스의 새로운 기능을 자세히 살펴 보려면`react-table` v7을 사용하여 테이블을 빌드하고 스타일 지정하는 포괄적 인 가이드를 확인하거나 다음을 통해 React 테이블 구성 요소를 렌더링하는 방법을 알아보십시오.
 `반응 테이블`.
 

### '반응 테이블'을 사용하는 경우
 

테이블 UI에 다음이 필요할 때 React Table을 사용하십시오.
 

- 정렬, 필터링 및 페이지 매김과 같은 기본 기능
 
- 기능에 영향을주지 않고 테이블에 대한 사용자 정의 UI 디자인
 
- 쉬운 확장 성.
 커스텀 플러그인 후크를 사용하여 라이브러리 위에 자신 만의 기능을 구축 할 수 있습니다.
 

### `react-table`을 사용하지 않는 경우
 

필요한 경우 다른 React 데이터 테이블 라이브러리를 고려하십시오.
 

- 고정 헤더 및 열에 대한 기본 지원
 
- 터치 및 비 터치 장치 모두에 대해 수평 및 수직 스크롤을위한 기본 지원.
 react-table은 UI를 지시하지 않습니다.
 헤드리스이므로 필요에 따라 UI를 정의하는 것은 우리의 책임입니다.
 
- 열의 인라인 편집을 지원합니다.
 우리는 반응 테이블에서이를 달성 할 수 있지만 우리 테이블에서 수행 할 수있는 범위를 벗어납니다.
 이러한 기능을 지원하려면 그 위에 플러그인 또는 구성 요소를 만들어야합니다.
 react-table은 이름 그대로 유지되며 간단한 테이블을 렌더링하는 데 가장 적합합니다.
 
- Google 스프레드 시트와 같은 무한 긴 테이블.
 성능 측면에서는 이렇게 큰 목록을 처리 할 수 없습니다.
 중간 크기의 테이블에는 잘 작동하지만 긴 테이블에는 적합하지 않습니다.
 

### '반응 테이블'사용 사례
 

- 검색, 정렬, 필터링 등과 같은 기본 기능이 필요한 간단한 테이블에 적합합니다.
 
- 스포츠 리더 보드 / 통계, 맞춤 요소가 포함 된 재무 데이터 표
 

이 React 테이블 가이드에서는 `react-table`을 사용하여 간단한 Airtable 클론을 빌드하는 방법을 보여줍니다.
 하지만 먼저 완전한 기능을 갖춘 React 테이블 UI의 기능을 빠르게 검토하고 React 데이터 테이블을 구축하는 것과 관련된 몇 가지 일반적인 문제에 대해 논의하겠습니다.
 

## React 테이블 UI 기능
 

React 데이터 테이블 UI의 기본 기능은 다음과 같습니다.
 

- 타협하지 않는 UX 및 UI.
 테이블 UI 내의 명확하게 이해할 수있는 타이포그래피 및 사용자 지정 요소
 
- 데이터로드를위한 원격 데이터 호출
 
- 테이블 또는 특정 열 검색
 
- 기본 필터링 및 정렬 옵션
 

React 데이터 테이블 UI의 고급 기능은 다음과 같습니다.
 

- 데이터 유형 (숫자, 문자열, 부울, 선택 입력 등)을 기반으로하는 열에 대한 사용자 정의 정렬 및 필터링 옵션
 
- 페이지 매김 지원 또는 무한 긴 테이블 (매우 큰 데이터 세트의 성능)
 
- 열 표시 및 숨기기
 
- 열 데이터의 인라인 편집 지원
 
- 모달 / 세부 정보 패널을 통해 전체 데이터 행 편집 지원
 
- 데이터를 쉽게 볼 수 있도록 고정 헤더 및 고정 열
 
- 여러 장치 지원 (응답 성)
 
- 열 안에 긴 데이터 포인트를 수용 할 수 있도록 크기 조정이 가능한 열 (예 : 여러 줄 주석)
 
- 수평 및 수직 스크롤 지원
 
- 행에 대한 전체 데이터를 표시하는 확장 가능한 행
 

## React 테이블 UI의 일반적인 UX 문제
 

UI 측면에서 데이터 테이블은 복잡한 데이터를 체계적으로 표시하는 가장 좋은 옵션 중 하나입니다.
 하지만 UX 측면에서는 까다 롭습니다. 여러 장치를 지원할 때 쉽게 벗어날 수 있습니다.
 테이블에 대한 몇 가지 UX 문제는 다음과 같습니다.
 

### 민감도
 

더 작은 화면 크기에 맞게 레이아웃을 변경하지 않으면 표가 반응 형으로 만들기가 어렵습니다.
 

### 스크롤
 

테이블은 양방향으로 스크롤해야 할 수 있습니다.
 기본 브라우저 스크롤바는 전체 너비 테이블에서 잘 작동하지만 대부분은 사용자 지정 너비입니다.
 사용자 지정 스크롤바는 터치 및 비 터치 화면 모두에서 지원하기가 매우 까다 롭습니다.
 

### 열 너비 관리
 

데이터 길이를 기준으로 열 너비를 관리하는 것은 까다 롭습니다.
 테이블에 동적 데이터를로드 할 때 종종 UX 결함이 발생합니다.
 데이터가 변경 될 때마다 열 너비의 크기가 조정되고 정렬 결함이 발생합니다.
 UX를 디자인하는 동안 이러한 문제를 처리 할 때주의해야합니다.
 

## 나만의 React 테이블 UI를 구축해야하는 경우
 

고유 한 React 테이블 UI를 빌드하려는 시나리오는 다음과 같습니다.
 

- 테이블이 상호 작용이 많지 않은 쇼케이스 일 때
 
- 테이블에 대한 사용자 정의 UI가 필요한 경우
 
- 기능없이 매우 가벼운 테이블이 필요할 때
 

다음은 몇 가지 예를 들어 자신 만의 React 테이블 UI를 구축하는 일반적인 사용 사례입니다.
 

- 비교를위한 표가있는 제품 / 마케팅 페이지
 
- 가격표
 
- 단순한 팝 오버 텍스트 이외의 열에 대해 많은 상호 작용이 필요하지 않은 사용자 지정 스타일이 적용된 간단한 표
 

## React Table 예제 : 'react-table'을 사용하여 React 테이블 구성 요소 빌드
 

충분한 이론 — 실제 React Table 예제를 살펴 보겠습니다.
 `react-table`을 사용하여 React 테이블 구성 요소를 만드는 방법을 보여주기 위해 정렬 및 검색과 같은 기본 기능이 포함 된 간단한 테이블 UI를 빌드합니다.
 다음은 우리가 작업 할 React 테이블 예제입니다.
 

먼저 create-react-app을 사용하여 React 앱을 만듭니다.
 

```undefined
npx create-react-app react-table-demo
```

### Axios로 API 호출
 

이것이 API 엔드 포인트입니다.
 Axios를 사용하여 검색어 `snow`가 포함 된 프로그램 정보를 호출하고 가져 오겠습니다.
 

API를 호출하기 위해`axios`를 설치하겠습니다.
 

```undefined
yarn add axios
```

```js
// App.js

import React, { useState, useEffect } from "react";

import Table from "./Table";
import "./App.css";

function App() {
  // data state to store the TV Maze API data. Its initial value is an empty array
  const [data, setData] = useState([]);

  // Using useEffect to call the API once mounted and set the data
  useEffect(() => {
    (async () => {
      const result = await axios("https://api.tvmaze.com/search/shows?q=snow");
      setData(result.data);
    })();
  }, []);

  return (
    <div className="App"></div>
  );
}

export default App;
```

`data`라는 상태를 만들고 구성 요소가 마운트되면 Axios를 사용하여 API를 호출하고`data`를 설정합니다.
 

### 앱에 '반응 표'추가
 

`반응 테이블`을 추가하려면 :
 

```undefined
yarn add react-table
```

`react-table`은 React Hooks를 사용합니다.
 `useTable`이라는 메인 테이블 후크가 있으며, 플러그인 후크를 추가하는 플러그인 시스템이 있습니다.
 따라서 react-table은 사용자의 필요에 따라 쉽게 확장 할 수 있습니다.
 

`useTable` Hook으로 기본 UI를 만들어 보겠습니다.
 두 개의 props,`data`와`columns`를 받아들이는 새로운`Table` 컴포넌트를 만들 것입니다.
 `data`는 API 호출을 통해 얻은 데이터이고`columns`는 테이블 열 (헤더, 행, 행 표시 방법 등)을 정의하는 개체입니다.
 곧 코드에서 볼 수 있습니다.
 

```js
// Table.js

export default function Table({ columns, data }) {
// Table component logic and UI come here
}
```

```js
// App.js
import React, { useMemo, useState, useEffect } from "react";

import Table from "./Table";

function App() {

  /* 
    - Columns is a simple array right now, but it will contain some logic later on. It is recommended by react-table to memoize the columns data
    - Here in this example, we have grouped our columns into two headers. react-table is flexible enough to create grouped table headers
  */
  const columns = useMemo(
    () => [
      {
        // first group - TV Show
        Header: "TV Show",
        // First group columns
        columns: [
          {
            Header: "Name",
            accessor: "show.name"
          },
          {
            Header: "Type",
            accessor: "show.type"
          }
        ]
      },
      {
        // Second group - Details
        Header: "Details",
        // Second group columns
        columns: [
          {
            Header: "Language",
            accessor: "show.language"
          },
          {
            Header: "Genre(s)",
            accessor: "show.genres"
          },
          {
            Header: "Runtime",
            accessor: "show.runtime"
          },
          {
            Header: "Status",
            accessor: "show.status"
          }
        ]
      }
    ],
    []
  );

  ...

  return (
    <div className="App">
      <Table columns={columns} data={data} />
    </div>
  );
}

export default App;
```

여기 열에서 헤더와 열의 여러 그룹을 만들 수 있습니다.
 우리는 두 가지 수준을 만들었습니다.
 

모든 열에는 `data`개체에있는 데이터 인 접근 자도 있습니다.
 데이터는 배열의`show` 객체 내부에 있습니다. 이것이 모든 접근자가`show.`를 접두사로 사용하는 이유입니다.
 

```undefined
// sample data array looks like this

[
  {
    "score": 17.592657,
    "show": {
      "id": 44813,
      "url": "http://www.tvmaze.com/shows/44813/the-snow-spider",
      "name": "The Snow Spider",
      "type": "Scripted",
      "language": "English",
      "genres": [
        "Drama",
        "Fantasy"
      ],
      "status": "In Development",
      "runtime": 30,
      "premiered": null,
      "officialSite": null,
      "schedule": {
        "time": "",
        "days": [

        ]
      }
      ...
  },
  {
    // next TV show
  }
...
]
```

`표`구성 요소를 마무리하겠습니다.
 

```js
// Table.js

import React from "react";
import { useTable } from "react-table";

export default function Table({ columns, data }) {
  // Use the useTable Hook to send the columns and data to build the table
  const {
    getTableProps, // table props from react-table
    getTableBodyProps, // table body props from react-table
    headerGroups, // headerGroups, if your table has groupings
    rows, // rows for the table based on the data passed
    prepareRow // Prepare the row (this function needs to be called for each row before getting the row props)
  } = useTable({
    columns,
    data
  });

  /* 
    Render the UI for your table
    - react-table doesn't have UI, it's headless. We just need to put the react-table props from the Hooks, and it will do its magic automatically
  */
  return (
    <table {...getTableProps()}>
      <thead>
        {headerGroups.map(headerGroup => (
          <tr {...headerGroup.getHeaderGroupProps()}>
            {headerGroup.headers.map(column => (
              <th {...column.getHeaderProps()}>{column.render("Header")}</th>
            ))}
          </tr>
        ))}
      </thead>
      <tbody {...getTableBodyProps()}>
        {rows.map((row, i) => {
          prepareRow(row);
          return (
            <tr {...row.getRowProps()}>
              {row.cells.map(cell => {
                return <td {...cell.getCellProps()}>{cell.render("Cell")}</td>;
              })}
            </tr>
          );
        })}
      </tbody>
    </table>
  );
}
```

`columns`와`data`를`useTable`에 전달합니다.
 후크는 헤더와 셀을 생성하기 위해 테이블, 본문 및 변환 된 데이터에 필요한 소품을 반환합니다.
 헤더는`headerGroups`를 반복하여 생성되고, 테이블 본문의 행은`rows`를 반복하여 생성됩니다.
 

```js
{rows.map((row, i) => {
  prepareRow(row); // This line is necessary to prepare the rows and get the row props from react-table dynamically

  // Each row can be rendered directly as a string using the react-table render method
  return (
    <tr {...row.getRowProps()}>
      {row.cells.map(cell => {
        return <td {...cell.getCellProps()}>{cell.render("Cell")}</td>;
      })}
    </tr>
  );
})}
```

이런 식으로 셀과 헤더를 렌더링했습니다.
 그러나 `cell`값은 문자열 일 뿐이며 배열 값도 쉼표로 구분 된 문자열 값으로 변환됩니다.
 예를 들면 :
 

```undefined
// Genres array

show.genres = [
 'Comedy',
 'Sci-fi',
]
```

표에서는 단순히 쉼표로 구분 된 문자열로 렌더링됩니다 (예 :`Comedy, Sci-fi`).
 이 시점에서 앱은 다음과 같아야합니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2019/11/basic-table-ui-demo.png?resize=730%2C377&ssl=1)

### '반응 테이블'의 맞춤 스타일링
 

이것은 대부분의 사용 사례에 적합한 테이블이지만 사용자 지정 스타일이 필요하면 어떻게해야합니까?
 react-table을 사용하면 각 셀에 대한 사용자 정의 스타일을 정의 할 수 있습니다.
 `column` 객체에서 이와 같이 정의 할 수 있습니다.
 각 장르를 보여주는 배지와 같은 맞춤 요소를 만들어 보겠습니다.
 

```undefined
// App.js

import React, { useMemo } from "react";
...

// Custom component to render Genres 
const Genres = ({ values }) => {
  // Loop through the array and create a badge-like component instead of a comma-separated string
  return (
    <>
      {values.map((genre, idx) => {
        return (
          <span key={idx} className="badge">
            {genre}
          </span>
        );
      })}
    </>
  );
};

function App() {
  const columns = useMemo(
    () => [
      ...
      {
        Header: "Details",
        columns: [
          {
            Header: "Language",
            accessor: "show.language"
          },
          {
            Header: "Genre(s)",
            accessor: "show.genres",
            // Cell method will provide the cell value; we pass it to render a custom component
            Cell: ({ cell: { value } }) => <Genres values={value} />
          },
          {
            Header: "Runtime",
            accessor: "show.runtime",
            // Cell method will provide the value of the cell; we can create a custom element for the Cell        
            Cell: ({ cell: { value } }) => {
              const hour = Math.floor(value / 60);
              const min = Math.floor(value % 60);
              return (
                <>
                  {hour > 0 ? `${hour} hr${hour > 1 ? "s" : ""} ` : ""}
                  {min > 0 ? `${min} min${min > 1 ? "s" : ""}` : ""}
                </>
              );
            }
          },
          {
            Header: "Status",
            accessor: "show.status"
          }
        ]
      }
    ],
    []
  );

  ...
}

...
```

위의 예에서 `Cell`메소드를 통해 값에 액세스 한 다음 계산 된 값 또는 사용자 지정 구성 요소를 반환합니다.
 

런타임의 경우 시간을 계산하고 사용자 지정 값을 반환합니다.
 Genres의 경우 값을 반복하여 사용자 정의 구성 요소로 보내면 해당 구성 요소가 배지와 같은 요소를 만듭니다.
 

반응 테이블에서 모양과 느낌을 사용자 지정하는 것은 매우 쉽습니다.
 이 단계가 끝나면 테이블 UI는 다음과 같습니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/11/table-ui-demo-badges.png?resize=730%2C356&ssl=1)

이러한 방식으로 필요에 따라 각 셀의 스타일을 사용자 지정할 수 있습니다.
 데이터 값을 기반으로 각 셀에 대한 사용자 정의 요소를 표시 할 수 있습니다.
 

## `useFilters`로 검색 기능 추가
 

테이블에 더 많은 기능을 추가해 보겠습니다.
 react-table의 데모 페이지를 보면 이미 사용자 지정 스마트 테이블을 만드는 데 필요한 모든 것을 제공합니다.
 데모에서 빠진 한 가지는 글로벌 검색 기능입니다.
 그래서 react-table에서`useFilters` 플러그인 후크를 사용하여 생성하기로 결정했습니다.
 

먼저`Table.js`에 검색 입력을 만들어 보겠습니다.
 

```undefined
// Table.js

// Create a state
const [filterInput, setFilterInput] = useState("");

// Update the state when input changes
const handleFilterChange = e => {
  const value = e.target.value || undefined;
  setFilterInput(value);
};

// Input element
<input
  value={filterInput}
  onChange={handleFilterChange}
  placeholder={"Search name"}
/>
```

입력 상태를 관리하기위한 간단하고 간단한 상태입니다.
 하지만 이제이 필터 값을 테이블에 전달하고 테이블 행을 필터링하는 방법은 무엇입니까?
 

이를 위해 react-table에는`useFilters`라는 멋진 후크 플러그인이 있습니다.
 

```php
// Table.js

const {
    getTableProps,
    getTableBodyProps,
    headerGroups,
    rows,
    prepareRow,
    setFilter // The useFilter Hook provides a way to set the filter
  } = useTable(
    {
      columns,
      data
    },
    useFilters // Adding the useFilters Hook to the table
    // You can add as many Hooks as you want. Check the documentation for details. You can even add custom Hooks for react-table here
  );
```

이 예에서는 `이름`열에 대해서만 필터를 설정합니다.
 이름을 필터링하려면 입력 값이 변경 될 때 첫 번째 매개 변수를 열 접근 자 또는 ID 값으로 설정하고 두 번째 매개 변수를 검색 필터 값으로 설정해야합니다.
 

`handleFilterChange` 함수를 업데이트 해 보겠습니다.
 

```undefined
const handleFilterChange = e => {
  const value = e.target.value || undefined;
  setFilter("show.name", value); // Update the show.name filter. Now our table will filter and show only the rows which have a matching value
  setFilterInput(value);
};
```

다음은 검색 구현 후 UI가 표시되는 방식입니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/11/table-ui-demo-search.png?resize=730%2C151&ssl=1)

이것은 필터에 대한 매우 기본적인 예이며 react-table API에서 제공하는 몇 가지 옵션이 있습니다.
 여기에서 API 문서를 확인할 수 있습니다.
 

## `useSortBy`로 정렬 추가
 

테이블의 기본 기능인 정렬을 하나 더 구현해 보겠습니다.
 모든 열에 대해 정렬을 허용하겠습니다.
 다시 말하지만, 필터링과 마찬가지로 매우 간단합니다.
 `useSortBy`라는 플러그인 후크를 추가하고 테이블에 정렬 아이콘을 표시하는 스타일을 생성해야합니다.
 오름차순 / 내림차순 정렬을 자동으로 처리합니다.
 

```cpp
// Table.js

const {
    getTableProps,
    getTableBodyProps,
    headerGroups,
    rows,
    prepareRow,
    setFilter
  } = useTable(
    {
      columns,
      data
    },
    useFilters,
    useSortBy // This plugin Hook will help to sort our table columns
  );

// Table header styling and props to allow sorting

<th
  {...column.getHeaderProps(column.getSortByToggleProps())}
  className={
    column.isSorted
      ? column.isSortedDesc
        ? "sort-desc"
        : "sort-asc"
      : ""
  }
>
  {column.render("Header")}
</th>
```

정렬에 따라`sort-desc` 또는`sort-asc`라는 클래스 이름을 추가합니다.
 또한 열 헤더에 정렬 소품을 추가합니다.
 

```undefined
{...column.getHeaderProps(column.getSortByToggleProps())}
```

이렇게하면 모든 열에 대한 정렬이 자동으로 허용됩니다.
 열에서`disableSortBy` 옵션을 사용하여 특정 열에서 정렬을 비활성화하여이를 제어 할 수 있습니다.
 이 예에서는 모든 열에 대한 정렬을 허용했습니다.
 데모를 가지고 놀 수 있습니다.
 

정렬 구현 후 UI가 다음과 같이 보입니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2019/11/table-ui-demo-sorting.png?resize=730%2C229&ssl=1)

물론이 데모를 더 확장 할 수 있습니다. 도움이 필요하면 의견 섹션에서 알려주십시오.
 이를 확장하기위한 몇 가지 아이디어는 다음과 같습니다.
 

- 전역 필터를 사용하여 여러 열 필터링 (힌트 :`setFilter` 대신`setAllFilters` 사용)
 
- 페이지 매김을 만들고 테이블에로드 할 더 많은 데이터를 호출합니다.
 
- 특정 필드에 대해서만 정렬 허용 (열에 대해 `sortby`비활성화)
 
- 하드 코딩 된 검색 값을 TV Maze API에 전달하는 대신 TV Maze API를 직접 검색하는 입력을 생성합니다 (예 : 클라이언트 측 필터링을 제거하고 API를 통해 TV 프로그램의 서버 측 검색을 추가하고 데이터를 변경).
 

이 데모를 확장하려면 react-table의 광범위한 예제 페이지를 확인하세요.
 가지고 놀기 좋은 주방 싱크대가 있으며 대부분의 사용 사례에 대한 솔루션을 제공합니다.
 

## React Table 대안
 

`react-table`은 가장 인기있는 React 테이블 라이브러리이지만 React에서 테이블을 구축하는 데 항상 최상의 솔루션은 아닙니다.
 특정 프로젝트 및 사용 사례에 따라 필요에 맞는 대안이 많이 있습니다.
 

다음은 체크 아웃 할 가치가있는 대체 React 테이블 라이브러리입니다.
 

### 1. 반응 데이터 그리드
 

react-data-grid는 스마트 테이블을 만드는 데 사용됩니다.
 4,000 개 이상의 GitHub 스타를 보유하고 있으며 잘 관리되고 있습니다.
 

#### react-data-grid를 사용하는 경우
 

데이터 테이블에 다음이 필요할 때 react-data-grid를 사용하세요.
 

- 열 그룹화, 정렬, 검색 및 필터링과 같은 기본 기능
 
- 열 인라인 편집
 
- 열 내부의 드롭 다운 (예 : Google 스프레드 시트) 또는 열 내부의 맞춤 입력 요소
 
- 더 많은 데이터를 표시하기 위해 열 확장 지원
 
- 성능을 위해 미세 조정하기 위해, 즉 무한히 긴 테이블 행에 대한 가상 렌더링을 지원합니다.
 
- 행이 없을 때 빈 상태 지원
 

#### react-data-grid를 사용하지 않는 경우
 

react-data-grid는 데이터 테이블에 대한 거의 모든 기본 요구 사항을 다룹니다.
 그러나 기본적으로 페이지 매김을 지원하지 않으므로 테이블에 페이지 매김이 필요한 경우 수동으로 구현하고 처리해야합니다.
 기본적으로 react-data-grid는 더 긴 테이블 UI를 지원하고 성능에 최적화되어 있으므로 UX에서 요구하지 않는 한 페이지 매김이 필요하지 않을 수 있습니다.
 

또한 스타일링을 위해 Bootstrap을 사용합니다.
 react-data-grid 없이도 사용할 수 있지만 테이블에 고유 한 스타일을 추가해야합니다.
 테이블 구조를 만들 수있는 react-table에 비해 쉽게 맞춤 설정할 수 없습니다.
 여기 react-data-grid에서 라이브러리는 테이블 UI를 생성하므로 UI가 많은 사용자 지정 페이지에는 적합하지 않습니다.
 

위의 사항이 정확히 단점은 아니지만 react-data-grid 사용을 시작하기 전에 알아두면 좋습니다.
 

#### 반응 데이터 테이블 사용 사례
 

좋은 UX로 Google 스프레드 시트 또는 Airtable과 유사한 미니 편집 가능한 데이터 테이블을 만들어야 할 때 중급이 필요합니다.
 

### 2. 반응 데이터 시트
 

react-datasheet는 react-data-grid와 유사합니다.
 약 4,500 개의 GitHub 별을 보유하고 있으며 마찬가지로 잘 관리 된 라이브러리입니다.
 

주로 Google 스프레드 시트와 유사한 애플리케이션을 만드는 데 중점을 둡니다.
 이러한 UX가 많은 응용 프로그램을 만들기 위해 기본 기능이 내장되어 있습니다.
 다시 말하지만, 테이블이있는 범용 페이지 UI를 만드는 데 적합하지 않을 수 있습니다.
 

그러나 react-data-grid와 달리 대규모 데이터 세트에 최적화되어 있지 않으므로 스프레드 시트와 같은 기능이 필요한 소규모 애플리케이션에 사용하세요.
 이 사용 사례는 하나 뿐이며 react-data-grid의 기능에 비해 기능이 매우 제한적입니다.
 

### 3. 반응 가상화
 

이름에서 알 수 있듯이 react-virtualized는 데이터 세트가 클 때 성능에 최적화되어 있습니다.
 이 라이브러리는 정확히 테이블 라이브러리가 아닙니다.
 훨씬 더 많은 일을합니다.
 그리드, 테이블 및 목록과 같은 다양한 형식으로 UI에 대규모 데이터 세트를 표시하기위한 것입니다.
 

#### 반응 가상화를 사용하는 경우
 

데이터 세트가 매우 큰 경우 렌더링 성능이 테이블의 핵심 측정 항목입니다.
 이 경우 반응 가상화를 선택하십시오.
 일반적인 사용 사례의 경우이 라이브러리는 과도하고 API는 너무 발전합니다.
 

#### 반응 가상화 사용 사례
 

사용자 지정 타임 라인, 무한히 긴 달력이 포함 된 차트 및 대규모 데이터 세트에 대한 무거운 UI 요소에 반응 가상화를 사용합니다.
 

## React 테이블 라이브러리를 선택하는 방법
 

- 제한된 데이터, 사용자 정의 스타일, 정렬 및 필터링과 같은 최소한의 상호 작용이있는 간단한 페이지의 경우`react-table`을 사용하십시오.
 
- 데이터가 제한된 미니 Google 스프레드 시트와 유사한 애플리케이션을 빌드하려면 react-data-grid 또는 react-datasheet를 사용하세요.
 
- 대규모 데이터 세트가있는 Google 스프레드 시트 또는 Airtable과 같은 애플리케이션의 경우 react-data-grid를 사용하세요.
 
- 매우 큰 데이터 세트로 작업하고 테이블, 그리드 및 더 많은 옵션을 제공하는 사용자 지정 UI가 필요한 경우 반응 가상화를 선택합니다.
 

## 결론
 

이것이 정렬을 추가 한 후 최종 React Table 데모 모습입니다.
 데모를 가지고 놀 수 있고 React Table 예제의 코드베이스를 확인할 수 있습니다.
 

React를 사용하여 테이블 UI를 구축하는 방법을 배웠습니다.
 기본 사용 사례에 대한 고유 한 테이블을 만드는 것은 어렵지 않지만 가능한 한 바퀴를 재발 명하지 않도록하십시오.
 테이블 UI에 대해 즐겁게 배우 셨기를 바랍니다. 주석에서 테이블 사용 경험에 대해 알려주세요.
 