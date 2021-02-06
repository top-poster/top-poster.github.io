---
layout: post
title: "Deno에서 cron 작업 설정"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/Deno-crono-jobs-automation.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/Deno-crono-jobs-automation.png?fit=730%2C487&ssl=1)

자동화는 프로세스와 반복 작업을 개선하고 효율화하여 시간을 절약할 수 있습니다. 유닉스 계열 시스템에서는 cron 작업을 사용하여 태스크를 자동화할 수 있습니다.

## 크론 일이란 무엇인가?

cron 작업은 지정된 권한 집합에서 나중에 실행되도록 태스크를 예약할 수 있는 시스템 프로세스(cron)의 유닉스 용어입니다. 기본적으로, 응용프로그램이 특정 날짜 또는 시간에 자동으로 작업을 실행하도록 예약할 수 있는 스케줄러입니다. 이 기사에서는 크론 작업을 Deno 응용 프로그램에 통합합니다.

## 전제조건

- JavaScript에 대한 이해
- 텍스트 편집기(우리의 경우 VS 코드를 사용합니다)
- 로컬 컴퓨터에 설치된 POSTMAN

## Deno 설치

로컬 컴퓨터에 Deno가 아직 설치되어 있지 않은 경우 홈브루를 사용하여 설치할 수 있습니다.

```undefined
brew install deno
```

설치가 완료되면 터미널에서 `deno`를 실행하여 설치가 성공했는지 확인합니다.

Deno에는 우리가 사용할 스마트 작업 스케줄러 라이브러리가 있습니다. 먼저 홈 디렉토리에 응용프로그램 디렉토리를 작성합니다.

```bash
cd desktop && mkdir denocron && cd denocron
touch index.ts
```

응용 프로그램에서 크론 작업을 구현하려면 응용 프로그램으로 모듈을 가져와야 합니다.

```coffeescript
import {cron, daily, monthly, weekly} from 'https://deno.land/x/deno_cron/cron.ts';
```

태스크를 실행할 사용자 정의 시간을 정의하는 것 외에도, Denocron은 이미 매주, 매월 및 매일 예약을 생성하는 몇 가지 방법을 제공합니다.

## 거부에서 사용자 정의 예약 정의

우리는 이 모듈을 사용하여 `cron` 방법을 사용하여 작업을 실행할 사용자 지정 시간을 만들 수 있다. 이 방법은 다음과 같은 형식을 가진 크론 패턴을 사용하여 작업을 예약합니다.

```coffeescript
cron('* * * * * *', () => {
    // run some task
});
```

여기서 무슨 일이 일어나고 있는지 설명하겠습니다.

- 첫 번째 별표에는 초가 걸립니다. 0-59 사이의 값을 사용합니다.
- 두 번째 별표는 분이며 0-59 사이의 값도 사용합니다.
- 세 번째 별표는 0-23 사이의 값을 갖는 시간(시간)입니다.
- 네 번째 별표는 월의 날짜로 0-31 사이의 값을 갖습니다.
- 다섯 번째 별표는 0-31 사이의 값을 갖는 연도의 달을 나타냅니다.
- 여섯 번째 별표는 요일에 포함되며 0-7 사이의 값을 갖습니다.

매초 실행되는 간단한 작업을 작성할 수 있습니다.

```coffeescript
cron('*/1 * * * * *', () => {
    // run some task
    console.log('This is a same thing')
});
```

애플리케이션을 실행하려면 터미널을 열고 deno run index.ts를 실행하십시오.

## Denon을 사용하여 애플리케이션 실행

Node.js에 데몬이 없듯이, Deno는 또한 변경이 이루어질 때마다 우리의 애플리케이션을 다시 로드하는 `denon` 패키지를 가지고 있다.

터미널을 열고 다음 명령을 실행하십시오.

```undefined
deno install -qAf --unstable https://deno.land/x/denon@2.4.4/denon.ts
```

이 명령은 개발 시스템에 `denon` 패키지를 전세계에 설치합니다.

이제 애플리케이션을 실행할 수 있게 되었으므로 denon index.ts 명령을 사용할 수 있습니다. MacBook을 사용하는 경우 다음과 같이 `command not found:denon`이라는 오류가 발생할 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/error-command-not-found.png?resize=730%2C36&ssl=1)

이 오류가 발생하면 다음 중 하나를 수행할 수 있습니다.

`zsh` 터미널을 사용하는 경우 다음을 실행하여 이 단자를 구성할 수 있습니다.

```bash
export PATH="/Users/<user>/.deno/bin:$PATH"
```

여기서 `사용자` 디렉토리는 로컬 컴퓨터에 있는 계정의 이름입니다.

또한 bash 터미널을 사용하는 경우 다음 명령을 사용하여 구성할 수 있습니다.

```undefined
echo 'export PATH="$HOME/.deno/bin:$PATH"' >> ~/.bashrc
```

## cron 작업 사용 사례: 자동 이메일

크론 작업의 일반적인 사용 사례는 전자 메일 및 뉴스레터의 자동 전송을 생성하는 것입니다. 우리는 매월 초하루 자정 기능을 실행할 수 있는 간단한 함수를 작성할 수 있습니다. 이를 위해 `크론` 방법을 사용할 것이다.

```coffeescript
cron('1 0 0 1 */1 *', () => {
    // run a function
});
```

이 메서드는 예약 구성 및 예약이 실행될 때 호출할 메서드를 사용합니다.

30초마다 실행되는 간단한 cron 작업을 작성할 수 있습니다.

```coffeescript
let task = cron('*/30 * * * * *', () => {
    // run some task
    console.log('This is a same thing')
});
```

다음은 30분마다 실행되는 간단한 작업입니다.

```coffeescript
cron('1 */30 * * * *', () => {
    checkStock();
});
```

Denocron은 사용자 지정 작업을 정의하는 것 외에도 고유한 기본 제공 방법을 제공합니다. 예를 들어 지정된 시간에 실행되는 일별, 주별 및 월별 방법이 있습니다.

```coffeescript
daily(() => {
    console.log('I run on daily basis')
});

weekly(() => {
console.log('This method will run on weekly bases')
});

everyMinute(()=> {
  console.log('This method will run on 60 seconds')  
})
```

이 방법을 사용하려면 먼저 가져와야 합니다.

```coffeescript
import { cron, everyMinute, daily, weekly } from 'https://deno.land/x/deno_cron/cron.ts';
```

모든 cron 작업을 중지하려면 stop() 방법을 사용할 수 있습니다. 모든 cron 작업을 시작하려면 `start()` 방법을 사용하십시오.

이 작업의 작동 방식을 보려면 몇 가지 부울 값을 설정해 보겠습니다. 먼저 다음 방법을 가져옵니다.

```coffeescript
import { cron, start,stop } from 'https://deno.land/x/deno_cron/cron.ts';
let task = cron('*/1 * * * * *', () => {
    // run some task
    console.log('This is the same thing')
});
let someBool = false
if (someBool) {
    start()
} else {
    stop()
}
```

먼저 URL에서 cron(크론), start(시작), stop(중지) 방법을 가져온 다음 cron(크론) 방법을 사용하여 시시각각 실행될 작업을 만듭니다.

응용 프로그램을 실행하려면 some Bool 값을 true로 설정하십시오. 작업이 완료되면 1분마다 콘솔에 "이것은 동일한 것"을 기록합니다. 또한 작업의 흐름을 제어하기 위해 시작 및 중지 방법이 사용됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/log-running-automatically.png?resize=730%2C164&ssl=1)

## 결론

크론 작업은 예약된 뉴스레터를 모든 고객에게 보내거나 메시지를 자동화하거나 자동화된 작업을 완료하려는 대규모 애플리케이션을 구축할 때 유용합니다.

여기 이 프로젝트의 소스 코드가 있습니다.