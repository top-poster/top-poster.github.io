---
layout: post
title: "Deno의 파일 시스템 이해"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/12/deno-file-system.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/deno-file-system.png?fit=730%2C487&ssl=1)

Deno는 자체 파일 시스템을 핵심 모듈로 제공합니다. 이 파일 시스템 모듈은 모든 종류의 파일 또는 디렉터리 조작에 사용할 수 있습니다.

이 튜토리얼에서는 Deno 응용 프로그램에서 사용할 수 있는 파일 시스템 방법에 대해 설명합니다.

현재 일부 방법은 여전히 실험적이며 매우 불안정합니다. 이러한 방법을 사용할 때는 항상 `-불안정` 플래그를 추가해야 합니다.

다음 사항에 대해 설명해 드리겠습니다.

- Deno에서 파일을 쓰는 방법
Deno에서 파일을 읽는 방법
Deno에서 파일 제거(`remove` 및 `removeSync`)
디렉토리에서 폴더 확인(`sureDir`)
파일 내용 복사(`복사`)
디렉토리 확인(`존재`)
파일이 있는지 확인합니다(`확인` 파일)
Deno에서 디렉토리를 비우는 방법(`emptyDir`)
Deno에서 CLI 애플리케이션 생성
- Deno에서 파일을 쓰는 방법
- Deno에서 파일을 읽는 방법
- Deno에서 파일 제거(`remove` 및 `removeSync`)
- 디렉토리에서 폴더 확인(`sureDir`)
- 파일 내용 복사(`복사`)
- 디렉토리 확인(`존재`)
- 파일이 있는지 확인합니다(`확인` 파일)
- Deno에서 디렉토리를 비우는 방법(`emptyDir`)
- Deno에서 CLI 애플리케이션 생성

## Deno에서 파일을 쓰는 방법

데노에서 파일을 쓰는 방법은 여러 가지가 있다. 모두 --allow-write 플래그가 필요하며 플래그가 발생하면 오류를 발생시킵니다.

`Deno`를 사용할 수 있습니다.텍스트 파일 쓰기 또는 `Deno`를 선택합니다.파일에 텍스트를 쓰기 위해 파일을 씁니다. `Deno.writeTextFile`은 동기식 및 비동기식으로 제공되며 파일의 위치와 아래 파일에 기록될 내용의 두 가지 매개 변수를 사용합니다.

```undefined
// using the async method
await Deno.writeTextFile('./file.txt', 'This is the content to be written');

//using sync method
Deno.writeTextFileSync('./file.txt', 'This is the content to be written');
```

Deno를 사용할 수도 있습니다.파일 메서드를 `텍스트 인코더`로 씁니다. 텍스트 인코더는 문자열을 Uint8Array로 변환합니다.

```undefined
const encoder = new TextEncoder(); // to convert a string to Uint8Array
await Deno.writeFile("./file.txt", encoder.encode("Content to be written"));
```

마지막으로 Deno.open과 Deno를 사용할 수 있습니다.아래와 같이 파일을 열고 내용을 작성한 후 닫는 모든 방법을 씁니다.

```undefined
const file = await Deno.open("./image.png", { write: true, create: true }); //this opens the file
await Deno.writeAll(file, imageBytes); //writes to file
file.close(); // closes the file
```

존재하지 않는 파일에 쓰려고 하면 Deno가 자동으로 파일을 만듭니다.

## Deno에서 파일을 읽는 방법

파일 작성과 마찬가지로 Deno에서 파일을 읽는 방법은 무수히 많은데, 이 모든 방법은 `--읽기 허용` 플래그를 필요로 합니다.

Deno.readTextFile 및 Deno.readFile 방법을 사용하여 Deno에서 파일을 읽을 수 있습니다. `Deno.readTextFile`은 동기 및 비동기 형식으로 제공되며 파일 경로를 매개 변수로 사용합니다.

```js
// using the async method
const text = await Deno.readTextFile("file.txt");
console.log(text);

//using the sync method
const sync = Deno.readTextFileSync("file.txt");
console.log(sync);
```

다른 방법은 `Deno.readFile`입니다. 먼저 `TextDecoder` 방법을 사용하여 파일을 읽기 쉬운 형식으로 디코딩해야 합니다.

```js
const decoder = new TextDecoder("utf-8");
const text = decoder.decode(await Deno.readFile("file.txt"));

console.log(text);
```

## Deno에서 파일 제거('remove' 및 'removeSync')

Deno에서 파일을 제거하려면 `remove` 또는 `removeSync` 방법을 사용하십시오.

```undefined
// Deno remove file asynchronous (non-block)
await Deno.remove("file.txt");

// Deno remove file synchronous (blocking way)
Deno.removeSync("image.png");
```

존재하지 않는 파일을 삭제하려고 하면 Deno가 오류를 발생시킵니다.

```undefined
Uncaught NotFound: No such file or directory (os error 2)
```

## 디렉토리에서 폴더 확인('sureDir')

ansureDir 메서드는 작업 디렉터리에 폴더가 있는지 확인합니다. 간단한 프로그램을 작성할 때 사용할 수 있습니다.

폴더가 있는지 확인하는 간단한 프로그램을 작성합시다. 폴더가 있으면 새 파일을 만들고 일부 텍스트 내용을 추가합니다.

이 방법을 사용하려면 프로그램으로 가져와야 합니다. 작업 디렉토리 내부에 `notes` 폴더를 작성합니다. 노트를 저장할 위치입니다.

```coffeescript
import { ensureDir, ensureDirSync } from "https://deno.land/std/fs/mod.ts";
```

지금까지 논의한 다른 방법들과 마찬가지로, 이 방법은 비동기 형식과 동기 형식으로 제공됩니다. 비동기식 메서드를 사용하는 방법은 다음과 같습니다.

```coffeescript
ensureDir("./notes")
  .then(() => Deno.writeTextFile("./notes/note1.txt", "This is the content for note 1")); 
```

## 파일 내용 복사('복사')

복사 방법을 사용하면 파일 내용을 다른 파일에 복제할 수 있습니다.

Deno 응용 프로그램으로 `복사` 메서드를 가져오려면:

```coffeescript
import { copy } from "https://deno.land/std/fs/mod.ts";
copy("file.txt", "test.txt", {
  overwrite: true,
});
```

이 메서드는 `파일`에서 파일 내용을 복사합니다.txt에서 test.txt로 이동합니다. 또한 몇 가지 옵션이 필요합니다. 예를 들어, `덮어쓰기` 옵션은 내용을 복사하는 파일의 내용을 덮어씁니다.

## 디렉토리 확인('존재')

`존재` 메서드는 디렉터리가 있는지 확인합니다. existsSync는 부울을 반환하는 반면 exists는 약속을 반환합니다.

디렉터리가 있는지 여부를 확인하는 간단한 프로그램을 작성하겠습니다.

```coffeescript
import { exists, existsSync } from "https://deno.land/std/fs/mod.ts";

exists("./notes").then((res) => console.log(res)); //res here returns a boolean
 //or do this
let fileExists = existsSync("./notes"); 
console.log(fileExists);// returns boolean
```

## 파일이 있는지 확인합니다('확인' 파일)

파일이 존재하는지 확인합니다.파일`은 파일이 존재하는지 확인합니다.

간단한 실험 프로그램을 작성해 보겠습니다.

```js
import { ensureFile, ensureFileSync } from "https://deno.land/std/fs/mod.ts";
let ok = ensureFileSync("./read.ts");
```

파일이 없으면 Deno가 자동으로 파일을 만듭니다. 이러한 방법 중 일부는 여전히 실험적인 것이므로 "-불안정" 플래그를 추가해야 합니다.

## Deno에서 디렉토리를 비우는 방법('emptyDir')

`emptyDir` 메서드는 디렉터리가 비어 있는지 확인합니다. 지정된 디렉토리에 파일 또는 디렉토리가 있는 경우, [거부]는 디렉토리를 비웁니다.

```coffeescript
import { emptyDir } from "https://deno.land/std/fs/mod.ts";

emptyDir("./notes").then((res) => console.log("res", res)); // this method return a promise.
```

## Deno에서 CLI 애플리케이션 생성

이제 간단한 CLI 애플리케이션을 생성하여 파일을 생성해 보겠습니다. 터미널을 사용하여 이 파일을 만듭니다.

Deno.args는 명령어로 전달된 모든 인수를 반환합니다. 이를 통해 간단한 애플리케이션을 만들 수 있습니다. 생성하려는 파일의 이름이 명령에 포함됩니다.

다음을 실행합니다.

```css
deno run main.ts create-file test.ts.
```

이 명령에는 `create-file`과 `test.ts`라는 두 가지 인수가 있습니다. Deno.args를 기록하면 인수가 배열로 반환됩니다.

우리는 이것을 이용해서 u를 확인할 수 있다.

ser는 `create-file` 매개 변수를 통과하고 파일 이름을 제공했습니다.

```js
let params = Deno.args;
console.log(params);
let createFile = () => {
  if (params[0] !== "create-file") {
    console.log(
      `${params[0]} is not a valid command, Did you mean 'create-file'`,
    );
    return false;
  } else if (!params[1]) {
    console.log(
      `You need to provide a name of a file`,
    );
    return false;
  } else {
    Deno.writeTextFileSync(`./${params[1]}`, "//This is your created file");
  }
};
createFile();
```

이제 응용 프로그램을 실행하려면 터미널을 열고 다음을 실행하십시오.

```undefined
deno run --allow-all main.ts create-file <name of file>
```

`--allow-all` 또는 `--allow-write` 파일 플래그를 추가해야 합니다.

## 결론

보시다시피 Deno의 파일 시스템은 매우 다재다능하며 Deno에 파일을 쓸 때 고려해야 할 여러 접근 방식과 시나리오가 있습니다. 동기식 및 비동기식 방법 모두 사용 사례가 있으므로 사용하는 방법을 아는 것이 좋습니다. 더 큰 파일에 쓸 때는 쓰기 스트림의 청크된 쓰기 기능을 활용하는 것을 고려해야 합니다.

이 데모에 사용된 소스 코드는 GitHub에서 사용할 수 있습니다.