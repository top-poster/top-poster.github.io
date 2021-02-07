---
layout: post
title: "Deno에서 Redis 사용"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/12/using-deno-redis.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/using-deno-redis.png?fit=730%2C487&ssl=1)

캐시는 나중에 사용하기 위해 정보가 보관되는 임시 데이터 저장소입니다. 캐싱 시스템을 구현하면 리소스를 가져오는 데 시간이 덜 걸리기 때문에 Deno 응용 프로그램의 속도를 높일 수 있습니다.

이 튜토리얼에서는 데이터 캐싱 개념을 살펴보고 Deno를 사용하여 Redis 기능을 통합하는 방법에 대해 설명합니다.

## Deno가 뭐죠?

Deno는 V8 엔진을 사용하는 JavaScript 및 TypeScript를 위한 최신 보안 런타임입니다. Deno는 TypeScript에 대한 기본 지원을 제공합니다. 즉, 앱에서 TypeScript를 설정하는 데 별도의 웹 팩 구성을 쓸 필요가 없습니다.

Deno는 기본적으로 보안을 채택합니다. 즉, 사용자가 명시적으로 허용하지 않는 한 파일, 네트워크 및 환경 액세스를 허용하지 않습니다.

## 레디스란 무엇인가?

Redis는 메모리 내 키-값 분산 데이터베이스를 구현하기 위한 매우 빠른 속도의 메모리 내 데이터 구조 프로젝트입니다. Redis는 캐싱 시스템과 메시지 차단기로도 사용될 수 있습니다.

데이터베이스와 마찬가지로 Redis는 문자열, 해시, 목록, 집합, 범위 쿼리가 있는 정렬된 집합 및 스트림과 같은 데이터 구조를 지원합니다. 기본적으로 Redis는 RAM을 사용하여 데이터를 저장하는데 매우 빠릅니다. 그러나 서버를 재부팅하면 지정된 간격으로 데이터 세트의 지정 시점 스냅샷을 수행하는 Redis 지속성이 활성화되지 않는 한 값이 손실됩니다.

## Deno와 함께 Redis를 사용하는 방법

Deno 코드 작성을 시작하기 전에 로컬 컴퓨터에 Redis를 설치해야 합니다.

Mac에 Redis를 설치하려면 다음 명령을 실행하여 홈브루를 사용할 수 있습니다.

```undefined
brew install redis
```

Redis를 설치한 후 로컬 시스템에서 서비스로 실행합니다.

```undefined
brew services start redis
```

Redis 서비스를 중지하려면 다음을 실행하십시오.

```undefined
brew services stop redis
```

Redis를 다시 시작하려면 다음을 실행하십시오.

```undefined
brew services restart redis
```

Redis가 로컬 컴퓨터에서 올바르게 실행되고 있는지 확인하려면 다음을 실행하십시오.

```undefined
redis-cli ping
```

이 명령이 터미널에 `퐁`을 반환하면 바로 사용할 수 있습니다.

다음 단계는 Deno가 로컬 컴퓨터에 올바르게 설치되었는지 확인하는 것입니다. 터미널을 열고 다음을 입력합니다.

```undefined
deno --version
```

만약 이 `deno`, `V8`, TypeScript의 버전을 표시합니다, 그때 갈 수 있다. 그렇지 않으면 홈브루를 사용하여 설치할 수 있습니다.

```undefined
brew install deno
```

이제 프로젝트의 디렉터리를 만들 수 있습니다. 우리는 우리의 redis.ts 파일에 있는 redis 기능을 시험해 볼 것이다.

## Redis 연결 만들기

프로젝트에서 Redis를 사용할 때마다 첫 번째 단계는 Redis 연결을 생성하는 것입니다. 기본적으로 Redis는 포트 `6379`에서 실행됩니다.

연결을 만들려면 `redis.ts` 파일에 다음을 추가하십시오.

```js
import { connect } from "https://denopkg.com/keroxp/deno-redis/mod.ts";
const redis = await connect({
  hostname: "127.0.0.1",
  port: 6379
});
console.log(await redis.ping())
```

지정된 포트를 사용하여 Redis CLI에 연결하려면 `connect` 방법을 사용하십시오. Redis 연결을 테스트하려면 기다려야 하는 약속을 반환하는 `redis.ping()` 방법을 사용하십시오.

응용 프로그램을 실행하려면 먼저 `--allow-net` 플래그를 전달하여 네트워크 권한을 허용해야 합니다. deno run--allow-net redis.ts를 실행하여 응용 프로그램을 시작합니다. 콘솔에 `Pong`이 기록됩니다(연결이 성공했음을 나타냄).

## 키-값 쌍 설정

집합 및 get 방법을 사용하여 Redis에 데이터를 저장하고 검색할 수 있습니다. 설정 방법에는 두 가지 매개 변수, 즉 이름 및 저장할 값이 포함됩니다.

```undefined
await redis.set('name', 'Wisdom Ekpot');
let name = await redis.get('name')
console.log(name)
```

`리디스`는 항상 약속을 되돌려 주기 때문에 항상 `기다려` 한다.

## 데이터 저장

hmset 등 제공된 방법을 사용하여 Redis에 데이터를 저장할 수 있습니다.

hmset은 해시 저장 키로 지정된 필드의 값을 설정하는 데 사용됩니다. 이 메서드는 기존 필드를 덮어씁니다. 키가 없으면 해시를 보유하는 새 키가 생성됩니다.

Redis에 추가할 간단한 기능을 쓸 수 있습니다.

```undefined
let add = async(key:string,name:string,email:string) => {
    let addPerson = await redis.hmset(key, {
        'name': name,
        'email': email
    })
    return addPerson
}
console.log(await add('key1','Wisdom Ekpot','wisdomekpot@gmail.com'))
```

키1 키를 눌러 레디스에 새 아이템을 추가하고 콘솔에 OK를 반환한다.

## 키를 사용하여 데이터 가져오기

hgetall은 특정 키에 대한 해시 필드와 값을 모두 반환합니다.

다음 키를 사용하여 Redis에 저장된 데이터를 가져올 수 있습니다.

```undefined
let getParticular = async (id:string) => {
   return await redis.hgetall(id);
}
console.log(await getParticular('key1'))
```

## 키가 있는 항목 삭제

del 메소드를 사용하여 키를 삭제할 수 있으며, 키 이름을 매개 변수로 사용해야 합니다.

```undefined
let deleteKey = async (id:string) => {
    let deleted = await redis.del(id);
    return deleted
}

console.log(await deleteKey('key1'))
```

## 클러스터 및 구성 재지정

Redis 클러스터는 여러 Redis 노드에서 데이터를 자동으로 영구 삭제하는 메커니즘입니다. Redis `meet` 메서드는 클러스터 모드가 활성화된 상태에서 여러 Redis 노드를 연결합니다.

새 클러스터를 생성하려면 `포트`를 매개 변수로 사용하는 `redis.meet() 메서드를 사용하십시오.

```css
await redis.cluster_meet("127.0.0.1", <port>);
```

이제 redis를 사용할 수 있습니다.생성된 모든 노드를 나열하는 노드 방법:

```coffeescript
 await redis.cluster_nodes();
```

클러스터는 기본적으로 사용 안 함으로 설정되어 있기 때문에 실제로 작동하지 않습니다. `이 인스턴스에 클러스터 지원이 비활성화되었습니다.`라는 오류가 발생할 수 있습니다.

Redis를 사용하면 구성을 확인할 수 있습니다. 다음과 같이 클러스터를 사용할 수 있는지 여부를 확인할 수 있습니다.

```js
let config = await redis.config_get("cluster-enabled");
console.log(config)
```

콘솔에서 [["클러스터 사용", "아니오"]를 반환합니다. 이를 활성화하려면 `config_name`과 구성 값을 사용하는 `config_set` 방법을 사용하십시오.

클러스터를 사용하도록 설정하려면 다음을 수행합니다.

```coffeescript
await redis.config_set('cluster-enabled', 'yes')
```

## Redis raw

Deno를 사용하면 원시 Redis 명령도 실행할 수 있습니다. 모든 원시 명령은 실행자 클래스를 통과해야 합니다. 이 명령은 약속으로 응답을 반환하므로 요청을 기다리는 것이 좋습니다.

```undefined
await redis.executor.exec("SET", "name", "Wisdom Ekpot")
let get = await redis.executor.exec("GET", "name");
console.log(get)
```

## 결론

Redis는 애플리케이션 확장을 지원하도록 설계된 모든 기능을 제공합니다. Redis를 Deno 애플리케이션에 통합하면 캐시에서 데이터를 호출하는 것이 매우 효율적이기 때문에 훨씬 더 빠르게 데이터를 호출할 수 있습니다.

이 튜토리얼에 사용된 소스 코드는 GitHub에서 사용할 수 있습니다.