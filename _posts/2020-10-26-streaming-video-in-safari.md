---
layout: post
title: "Safari에서 스트리밍 비디오: 왜 이렇게 어렵죠?"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/streaming-video-safari.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/streaming-video-safari.png?fit=730%2C487&ssl=1)

## 문제

저는 최근에 제 제품 Sortal에 있는 동영상의 AI 태깅 지원을 구현했습니다. 그런 다음 업로드한 동영상을 재생할 수 있습니다. 저는 비디오 스트리밍이 아주 간단해 보인다고 생각했습니다.

사실, 그것은 너무 간단해서 (코드의 몇 줄만 있으면) 나는 내 책 Bootstraping Microservices의 예제로 비디오 스트리밍을 선택했다.

하지만 우리가 사파리에서 시험을 보러 왔을 때, 나는 추악한 진실을 알게 되었어. 이전의 주장을 다시 설명하겠습니다. 비디오 스트리밍은 Chrome에게는 간단하지만 Safari에는 그렇게 많지 않습니다.

사파리가 왜 이렇게 어렵죠? 사파리에서 작동하려면 어떻게 해야 하나요? 이러한 질문에 대한 답은 이 블로그 게시물에 나와 있습니다.

## 직접 확인해 보십시오.

코드를 함께 보기 전에 직접 사용해 보십시오! 이 블로그 게시물에 수반되는 코드는 GitHub에서 이용할 수 있습니다. 코드를 다운로드하거나 Git를 사용하여 리포지토리를 복제할 수 있습니다. 테스트하려면 Node.js가 설치되어 있어야 합니다.

readme의 지침에 따라 서버를 시작하고 브라우저를 `http://localhost:3000`으로 이동합니다. Chrome 또는 Safari에서 페이지를 보고 있는지 여부에 따라 그림 1 또는 그림 2가 표시됩니다.

그림 2에서 웹 페이지를 Safari에서 볼 때 왼쪽의 비디오가 작동하지 않습니다. 그러나 오른쪽 예제는 효과가 있으며, 이 게시물은 어떻게 Safari용 비디오 스트리밍 코드의 작동 버전을 달성했는지 설명합니다.

## 기본 비디오 스트리밍

Chrome에서 작동하는 비디오 스트리밍의 기본 형식은 HTTP 서버에서 구현하기 어렵습니다. 그림 3에서와 같이 전체 비디오 파일을 백엔드로부터 프런트 엔드로 스트리밍하기만 하면 됩니다.

### 프런트 엔드에서

프런트 엔드에서 비디오를 렌더링하기 위해 HTML5 비디오 요소를 사용합니다. 많은 것이 없습니다. 목록 1은 그 작동 방식을 보여줍니다. 이것은 크롬에서만 작동하는 버전입니다. 동영상의 src는 백엔드에서 /works-in-chrome 경로에 의해 처리되는 것을 볼 수 있다.

```xml
<!doctype html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Video streaming example</title>
        </head>
    <body> 
        <video
            muted
            playsInline
            loop
            controls
            src="/works-in-chrome" 
            >
        </video>
    </body>
</html>
```

### 백엔드에서

이 예제의 백엔드는 Node.js에서 실행되는 Express 프레임워크에 구축된 매우 간단한 HTTP 서버입니다. 코드는 목록 2에서 확인할 수 있습니다. 이곳이 `/크롬에서 일하는` 루트가 구현되는 곳이다.

HTTP GET 요청에 따라 전체 파일을 브라우저로 스트리밍합니다. 도중에, 우리는 다양한 HTTP 응답 헤더를 설정합니다.

콘텐츠 유형 헤더는 비디오/mp4로 설정되어 있어 브라우저가 비디오를 수신하고 있음을 알 수 있습니다.

그런 다음 파일 길이를 얻기 위해 파일을 `stat`하고 브라우저가 수신하는 데이터의 양을 알 수 있도록 파일을 `콘텐츠 길이` 헤더로 설정합니다.

```js
const express = require("express");
const fs = require("fs");

const app = express();

const port = 3000;

app.use(express.static("public"));

const filePath = "./videos/SampleVideo_1280x720_1mb.mp4";

app.get("/works-in-chrome", (req, res) => {
    // Set content-type so the browser knows it's receiving a video.
    res.setHeader("content-type", "video/mp4"); 


    // Stat the video file to determine its length.
    fs.stat(filePath, (err, stat) => {
        if (err) {
            console.error(`File stat error for ${filePath}.`);
            console.error(err);
            res.sendStatus(500);
            return;
        }

        // Set content-length so the browser knows
        // how much data it is receiving.
        res.setHeader("content-length", stat.size);

        // Stream the video file directly from the 
        // backend file system.
        const fileStream = fs.createReadStream(filePath);
        fileStream.on("error", error => {
            console.log(`Error reading file ${filePath}.`);
            console.log(error);
            res.sendStatus(500);
        });

        // Pipe the file to the HTTP response.
        // We are sending the entire file to the 
        // frontend.
        fileStream.pipe(res);
    });
});

app.listen(port, () => {
    console.log(`Example app listening at http://localhost:${port}`)
});
```

## 하지만 사파리에서는 작동하지 않아요!

안타깝게도 전체 비디오 파일을 Safari에 보내서 제대로 작동하리라고 기대할 수는 없습니다. 크롬은 그것을 다룰 수 있지만 사파리는 그 게임을 하는 것을 거부한다.

### 뭐가 없어?

Safari는 전체 파일을 한 번에 배달하는 것을 원하지 않습니다. 그래서 파일 전체를 스트리밍하는 무차별적인 전략이 통하지 않는 겁니다.

Safari는 파일의 일부를 스트리밍하여 단편적으로 버퍼링할 수 있기를 원합니다. 또한 필요한 파일의 모든 부분에 대한 임의적인 특별 액세스도 원합니다.

이건 정말 말이 되네요. 사용자가 비디오를 약간 되감고 싶다고 상상해 보십시오. 전체 파일 스트리밍을 다시 시작하고 싶지는 않을 것입니다. 그렇죠?

대신 Safari는 조금 되돌아가서 파일의 해당 부분을 다시 요청하려고 합니다. 사실, 이것은 크롬에서도 작동한다. 기본 스트리밍 비디오는 크롬에서 작동하지만, 크롬은 실제로 스트리밍 비디오의 보다 효율적인 처리를 위해 HTTP 범위 요청을 발행할 수 있다.

그림 4는 이것이 어떻게 작동하는지 보여줍니다. 우리는 전체 비디오 파일을 프런트 엔드로 스트리밍하는 대신 브라우저의 요청에 따라 파일의 랜덤 액세스 부분을 서비스할 수 있도록 HTTP 서버를 수정해야 합니다.

### HTTP 범위 요청 지원

특히 HTTP 범위 요청을 지원해야 합니다. 하지만 어떻게 구현해야 할까요?

그것에 대한 읽을 만한 문서는 놀라울 정도로 거의 없다. 물론 HTTP 사양을 읽을 수 있지만, 이를 위해 시간과 동기를 부여한 사람은 누구입니까? (이 게시물의 마지막에 리소스에 대한 링크를 드리겠습니다.)

대신, 구현 개요를 안내해 드리겠습니다. 여기서 핵심은 접두사 "bytes="로 시작하는 HTTP 요청 `range` 헤더입니다.

이 헤더는 프런트 엔드가 비디오 파일에서 검색할 특정 범위의 바이트를 요청하는 방법입니다. 목록 3에서 이 헤더의 값을 구문 분석하여 바이트 범위의 시작 및 끝 값을 얻는 방법을 볼 수 있습니다.

```js
const options = {};

let start;
let end;

const range = req.headers.range;
if (range) {
    const bytesPrefix = "bytes=";
    if (range.startsWith(bytesPrefix)) {
        const bytesRange = range.substring(bytesPrefix.length);
        const parts = bytesRange.split("-");
        if (parts.length === 2) {
            const rangeStart = parts[0] && parts[0].trim();
            if (rangeStart && rangeStart.length > 0) {
                options.start = start = parseInt(rangeStart);
            }
            const rangeEnd = parts[1] && parts[1].trim();
            if (rangeEnd && rangeEnd.length > 0) {
                options.end = end = parseInt(rangeEnd);
            }
        }
    }
}
```

### HTTP HEAD 요청에 응답

HTTP HEAD 요청은 프런트 엔드가 특정 리소스에 대한 정보를 백엔드에 조사하는 방법입니다. 우리가 이 문제를 어떻게 처리할지 좀 신경써야 해.

Express 프레임워크는 또한 HEAD 요청을 HTTP GET 처리기로 전송하므로, 우리는 HEAD 요청에 필요한 것보다 더 많은 작업을 수행하기 전에 요청 처리기에서 requ.method를 확인하고 일찍 반환할 수 있다.

목록 4는 우리가 HEAD 요청에 어떻게 대응하는지 보여줍니다. 파일에서 데이터를 반환할 필요는 없지만 HTTP 범위 요청을 지원하고 있음을 프런트 엔드에 알리고 비디오 파일의 전체 크기를 알리기 위해 응답 헤더를 구성해야 합니다.

여기서 사용되는 `승인 범위` 응답 헤더는 이 요청 처리기가 HTTP 범위 요청에 응답할 수 있음을 나타냅니다.

```cpp
if (req.method === "HEAD") {
    res.statusCode = 200;


// Inform the frontend that we accept HTTP 
// range requests.
    res.setHeader("accept-ranges", "bytes");

    // This is our chance to tell the frontend
    // the full size of the video file.
    res.setHeader("content-length", contentLength);

    res.end();
}
else {        
    // ... handle a normal HTTP GET request ...
}
```

### 전체 파일 대 부분 파일

이제 까다로운 부분까지. 전체 파일을 보내는 건가요, 아니면 파일의 일부를 보내는 건가요?

신중하게 요청 처리기가 두 가지 방법을 모두 지원하도록 할 수 있습니다. 목록 5에서 범위 요청이고 해당 변수가 정의되었을 때 시작 및 종료 변수에서 "RetrieveedLength"를 계산하는 방법을 볼 수 있습니다. 그렇지 않으면 "contentLength"(전체 파일의 크기)를 범위 요청이 아닐 때 사용합니다.

```js
let retrievedLength;
if (start !== undefined && end !== undefined) {
    retrievedLength = (end+1) - start;
}
else if (start !== undefined) {
    retrievedLength = contentLength - start;
}
else if (end !== undefined) {
    retrievedLength = (end+1);
}
else {
    retrievedLength = contentLength;
}
```

### 상태 코드 및 응답 헤더 전송

HEAD 요청을 처리했습니다. HTTP GET 요청만 처리하면 됩니다.

목록 6은 적절한 성공 상태 코드와 응답 헤더를 보내는 방법을 보여줍니다.

상태 코드는 전체 파일에 대한 요청인지 또는 파일 일부에 대한 범위 요청인지에 따라 달라집니다. 범위 요청인 경우 상태 코드는 206(부분 컨텐츠의 경우)이며, 그렇지 않은 경우 기존 성공 상태 코드인 200을 사용합니다.

```js
// Send status code depending on whether this is
// request for the full file or partial content.
res.statusCode = start !== undefined || end !== undefined ? 206 : 200;

res.setHeader("content-length", retrievedLength);

if (range !== undefined) {  
    // Conditionally informs the frontend what range of content
    // we are sending it.
    res.setHeader("content-range", 
           `bytes ${start || 0}-${end || (contentLength-1)}/${contentLength}`
       );
    res.setHeader("accept-ranges", "bytes");
}
```

### 파일의 일부 스트리밍

이제 가장 쉬운 부분은 파일 일부를 스트리밍하는 것입니다. 목록 7의 코드는 목록 2의 기본 비디오 스트리밍 예제의 코드와 거의 동일합니다.

지금 우리가 다른 점은 `옵션`이라는 목표를 통과하고 있다는 것이다. 편리하게 Node.js의 파일 시스템 모듈의 `createReadStream` 기능은 하드 드라이브에서 파일의 일부를 읽을 수 있는 `options` 객체의 `start`와 `end` 값을 취한다.

HTTP 범위 요청의 경우, Listing 3의 이전 코드는 헤더에서 `start`와 `end` 값을 구문 분석하여 `options` 객체에 삽입한다.

일반 HTTP GET 요청(범위 요청이 아님)의 경우 시작과 끝은 구문 분석되지 않고 옵션 객체에 포함되지 않으므로 파일 전체를 읽는 것입니다.

```coffeescript
const fileStream = fs.createReadStream(filePath, options);
fileStream.on("error", error => {
    console.log(`Error reading file ${filePath}.`);
    console.log(error);
    res.sendStatus(500);
});

fileStream.pipe(res);
```

### 모든 것을 종합하다.

이제 모든 코드를 Chrome과 Safari에서 모두 작동하는 스트리밍 비디오에 대한 완전한 요청 처리기에 통합해 보겠습니다.

Listening 8은 Listening 3에서 Listening 7까지를 합친 코드이기 때문에 모든 상황을 볼 수 있습니다. 이 요청 처리기는 어느 쪽이든 작동할 수 있습니다. 브라우저에서 요청하면 비디오 파일의 일부를 검색할 수 있습니다. 그렇지 않으면 전체 파일을 검색합니다.

```js
app.get('/works-in-chrome-and-safari', (req, res) => {

    // Listing 3.
    const options = {};

    let start;
    let end;

    const range = req.headers.range;
    if (range) {
        const bytesPrefix = "bytes=";
        if (range.startsWith(bytesPrefix)) {
            const bytesRange = range.substring(bytesPrefix.length);
            const parts = bytesRange.split("-");
            if (parts.length === 2) {
                const rangeStart = parts[0] && parts[0].trim();
                if (rangeStart && rangeStart.length > 0) {
                    options.start = start = parseInt(rangeStart);
                }
                const rangeEnd = parts[1] && parts[1].trim();
                if (rangeEnd && rangeEnd.length > 0) {
                    options.end = end = parseInt(rangeEnd);
                }
            }
        }
    }

    res.setHeader("content-type", "video/mp4");

    fs.stat(filePath, (err, stat) => {
        if (err) {
            console.error(`File stat error for ${filePath}.`);
            console.error(err);
            res.sendStatus(500);
            return;
        }

        let contentLength = stat.size;

        // Listing 4.
        if (req.method === "HEAD") {
            res.statusCode = 200;
            res.setHeader("accept-ranges", "bytes");
            res.setHeader("content-length", contentLength);
            res.end();
        }
        else {       
            // Listing 5.
            let retrievedLength;
            if (start !== undefined && end !== undefined) {
                retrievedLength = (end+1) - start;
            }
            else if (start !== undefined) {
                retrievedLength = contentLength - start;
            }
            else if (end !== undefined) {
                retrievedLength = (end+1);
            }
            else {
                retrievedLength = contentLength;
            }

            // Listing 6.
            res.statusCode = start !== undefined || end !== undefined ? 206 : 200;

            res.setHeader("content-length", retrievedLength);

            if (range !== undefined) {  
                res.setHeader("content-range", `bytes ${start || 0}-${end || (contentLength-1)}/${contentLength}`);
                res.setHeader("accept-ranges", "bytes");
            }

            // Listing 7.
            const fileStream = fs.createReadStream(filePath, options);
            fileStream.on("error", error => {
                console.log(`Error reading file ${filePath}.`);
                console.log(error);
                res.sendStatus(500);
            });


            fileStream.pipe(res);
        }
    });
});
```

### 업데이트된 프런트 엔드 코드

비디오 요소가 HTTP 범위 요청을 처리할 수 있는 HTTP 경로를 가리키는지 확인하는 것 외에 프런트 엔드 코드에서 변경할 필요가 없습니다.

목록 9는 우리가 단순히 비디오 요소를 `/works-in-chrom-and-safari`라는 경로로 다시 설정했음을 보여준다. 이 프런트 엔드는 크롬과 사파리 모두에서 사용할 수 있습니다.

```xml
<!doctype html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Video streaming example</title>
        </head>
    <body> 
        <video
            muted
            playsInline
            loop
            controls
            src="/works-in-chrome-and-safari" 
            >
        </video>
    </body>
</html>
```

## 결론

비록 비디오 스트리밍이 크롬에서 작동하기는 간단하지만, 적어도 HTTP 사양을 통해 스스로 알아내려는 경우에는 Safari를 찾는 것이 훨씬 더 어렵습니다.

다행스럽게도, 저는 이미 그 길을 걷고 있습니다. 그리고 이 블로그 게시물은 여러분 자신의 스트리밍 비디오 구현을 위한 토대를 마련했습니다.

## 자원.

- 이 블로그 게시물의 예제 코드
- 누락된 항목을 이해하는 데 도움이 되는 스택 오버플로 게시물
- HTTP 사양
- 유용한 Mozilla 설명서:
요청 범위
범위
206 부분 컨텐츠 성공 여부
- 요청 범위
- 범위
- 206 부분 컨텐츠 성공 여부