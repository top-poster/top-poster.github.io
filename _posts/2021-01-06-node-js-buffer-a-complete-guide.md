---
layout: post
title: "Node.js 버퍼: 전체 가이드"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/nodejs-buffer-module.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/nodejs-buffer-module.png?fit=730%2C487&ssl=1)

컴퓨터가 이진 언어만 이해한다는 것을 배웠을 때를 생각해 보십시오. 버퍼는 이진 파일을 저장하는 컴퓨터 메모리(일반적으로 RAM)의 공간임을 기억할 수 있습니다.

이 튜토리얼에서는 Node.js 버퍼 모듈의 기본 사항과 애플리케이션에서 Node.js 버퍼 방법을 사용하는 방법에 대해 설명합니다. 다음 사항에 대해 자세히 설명합니다.

- 이진 코드란?
- Node.js 버퍼란?
- Node.js의 버퍼 클래스
- Node.js 버퍼 메서드
`Buffer.alloc()`
`Buffer.byteLength()`
buffer.compare()
buffer.concat()
buff.➡➡
buffer.fill()
buffer.from()
buff.➡➡
Buffer.is 인코딩()
buff.➡➡
버퍼 스왑
buff.jsonname
버퍼 오프셋 읽기
버퍼 오프셋 쓰기
- `Buffer.alloc()`
- `Buffer.byteLength()`
- buffer.compare()
- buffer.concat()
- buff.➡➡
- buffer.fill()
- buffer.from()
- buff.➡➡
- Buffer.is 인코딩()
- buff.➡➡
- 버퍼 스왑
- buff.jsonname
- 버퍼 오프셋 읽기
- 버퍼 오프셋 쓰기

## 이진 코드란?

버퍼가 무엇이고 컴퓨터와 이진 파일 간의 관계를 이해하기 위해 일반적인 이진 코드 개념을 검토하겠습니다.

컴퓨터가 데이터를 이해하고 처리 및 저장하려면 데이터를 이진 코드로 변환해야 합니다. 이는 주로 컴퓨터 프로세서가 트랜지스터, 즉 ON/OFF(0) 신호에 의해 작동되는 전자 기계로 만들어졌기 때문이다.

컴퓨터로 전송된 모든 데이터는 결과를 처리하고 출력하기 전에 마이크로프로세서에 의해 먼저 바이너리로 변환된다. 따라서 서로 다른 데이터 유형을 구분할 수 있어야 합니다. 인코딩이라고 불리는 과정에 의해, 컴퓨터는 다른 타입과 구별하기 위해 다른 타입의 데이터 타입을 다르게 인코딩한다.

## Node.js 버퍼란?

이진 스트림은 많은 양의 이진 데이터의 집합이다. 이진 스트림은 크기가 크기 때문에 함께 전송되지 않고 전송하기 전에 더 작은 조각으로 분할됩니다.

데이터 처리 장치가 더 이상 데이터 스트림을 수용할 수 없는 경우, 데이터 처리 장치가 더 많은 데이터를 수신할 준비가 될 때까지 초과 데이터가 버퍼에 저장됩니다.

## Node.js의 버퍼 클래스

Node.js 서버는 파일 시스템을 읽고 써야 하는 경우가 가장 많으며, 파일도 이진 파일에 저장됩니다. 또한 Node.js는 작은 청크로 이진 데이터를 보내기 전에 수신기에 대한 연결을 보호하는 TCP 스트림을 다룬다.

수신기로 전송되는 데이터 스트림은 수신기가 처리를 위해 더 많은 양의 데이터를 가져올 준비가 될 때까지 어딘가에 저장되어야 한다(버퍼). 여기서 Node.js 버퍼 클래스는 V8 엔진 외부의 이진 데이터를 처리하고 저장합니다.

이제 다양한 버퍼 메소드와 Node.js 응용 프로그램에서 버퍼 메소드를 어떻게 사용할 수 있는지 알아보겠습니다.

## Node.js 버퍼 메서드

Node.js 버퍼 모듈의 멋진 점은 메소드를 사용하기 전에 프로그램으로 가져올 필요가 없다는 것입니다.

다음은 알아야 할 몇 가지 중요한 Node.js 버퍼 방법입니다.

### 'Buffer.alloc()'

버퍼.alloc() 메서드는 모든 크기의 새 버퍼를 만듭니다. 이 방법을 사용할 때 버퍼 크기를 바이트 단위로 할당합니다.

아래 식은 바이트 크기가 `6`인 버퍼를 생성합니다.

```cpp
const buf = Buffer.alloc(6);

console.log(buf);
// This will print <Buffer 00 00 00 00 00 00>
```

### 'Buffer.byteLength()'

버퍼 개체의 길이는 `Buffer.byteLength()` 방법으로 확인할 수 있습니다.

버퍼를 만들고, 버퍼에 크기를 첨부하고, 방금 만든 버퍼의 크기를 확인하는 방법은 다음과 같습니다.

```js
var buf = Buffer.alloc(6);

//check the length of buffer created
var buffLen = Buffer.byteLength(buf);

//print buffer length
console.log(buffLen);
// This will print <6>
```

### buffer.compare()

Buffer.compare() 메서드를 사용하면 두 버퍼 개체를 비교하여 Buffer.compare() 메서드와 동일한지 확인할 수 있습니다. 이 방법은 비교 결과에 따라 -1, 0 또는 1을 반환합니다.

버퍼 개체를 `B`uffer.compare() 방법과 비교하는 방법은 다음과 같습니다.

```undefined
var buf1 = Buffer.from('xyz');
var buf2 = Buffer.from('xyz');
var a = Buffer.compare(buf1, buf2);

//This will return 0
console.log(a);

var buf1 = Buffer.from('x');
var buf2 = Buffer.from('y');
var a = Buffer.compare(buf1, buf2);

//This will return -1
console.log(a);

var buf1 = Buffer.from('y');
var buf2 = Buffer.from('x');
var a = Buffer.compare(buf1, buf2);

//This will return 1
console.log(a);
```

### '버퍼.

문자열 연결과 마찬가지로 두 개 이상의 버퍼 개체를 하나의 개체에 결합할 수 있습니다. 연결에서 얻은 새 개체의 길이도 확인할 수 있습니다.

```undefined
var buffer1 = Buffer.from('x');
var buffer2 = Buffer.from('y');
var buffer3 = Buffer.from('z');
var arr = [buffer1, buffer2, buffer3];

/*This will print buffer, !concat [ <Buffer 78>, <Buffer 79>, <Buffer 7a> ]*/
console.log(arr);

//concatenate buffer with Buffer.concat method
var buf = Buffer.concat(arr);

//This will print <Buffer 78 79 7a> concat successful
console.log(buf);
```

### buff.➡➡

buf.entries()를 사용하면 버퍼 개체의 컨텐츠에서 인덱스 및 바이트 루프를 반환할 수 있습니다. 버퍼 콘텐츠의 위치와 크기를 알기 위해 사용합니다.

```js
var buf = Buffer.from('xyz');

for (a of buf.entries()) {
/*This will print arrays of indexes and byte of buffer content \[ 0, 120 \][ 1, 121 ][ 2, 122 ]*/
  console.log(a);
} 
```

### buffer.fill()

Buffer.fill() 방법을 사용하여 버퍼를 지정된 값으로 채울 수 있습니다. buffer.fill()을 사용하면 버퍼를 생성하고 크기를 할당하며 값으로 채울 수 있습니다. 아래 식은 이 방법을 사용하는 방법을 보여줍니다.

```cpp
const b = Buffer.alloc(10).fill('a');

console.log(b.toString());
// This will print aaaaaaaaaa
```

### buffer.from()

`buffer.from() 메서드를 사용하면 모든 개체(string, buffer, array 또는 `ArrayBuffer()`)에서 새 버퍼를 생성할 수 있습니다. 버퍼를 만들 개체를 지정하기만 하면 됩니다.

이 메서드를 사용하는 구문은 `Buffer.from(object[, offsetOrEncoding[, length])입니다. 오브젝트 매개 변수에는 문자열, 버퍼, 배열 또는 `ArrayBuffer()이 포함될 수 있습니다. 즉, `buffer.from` 메서드를 사용하여 문자열, 버퍼, 배열 및 `ArrayBuffer()`에서 새 버퍼를 생성할 수 있습니다.

오프셋 또는 인코딩 매개 변수는 선택 사항입니다. 버퍼로 변환하는 동안 문자열 인코딩을 지정하는 데 사용됩니다. 문자열에서 버퍼를 만들 때 인코딩을 지정하지 않으면 버퍼가 기본 인코딩인 `utf8`로 생성됩니다. 특히 `ArrayBuffer`에서 버퍼를 생성할 때 길이 매개 변수로 표시할 바이트 수를 지정할 수도 있습니다.

어레이에서 버퍼를 생성하는 동안 어레이 바이트는 `0`과 `255` 사이에 있어야 합니다. 그렇지 않으면 배열 항목이 적합하도록 단축됩니다.

아래의 예는 `buffer.from() 방법을 사용하여 문자열, 배열 및 `ArrayBuffer()`에서 버퍼를 생성하는 방법을 보여줍니다.

```undefined
// Create a buffer from a string
var mybuff = Buffer.from("Nigeria");
//Print Buffer Created
console.log(mybuff);

// Create a buffer from a buffer
// Create buffer from string
var mybuff = Buffer.from("Nigeria");
// Create buffer from the first buffer created
var buff = Buffer.from(mybuff);
// Print out final buffer created.
console.log(buff);

// create a buffer from an array
const buf = Buffer.from([0x62, 0x75, 0x66, 0x66, 0x65, 0x72]);

// Create a buffer from an arraybuffer
const ab = new ArrayBuffer(10);
// Specify offset and length
const buf = Buffer.from(ab, 0, 2);
console.log(buff);
```

### ➡.➡

버퍼 개체에 값이 포함되어 있는지 여부를 확인하려면 `buff.include()` 방법을 사용하십시오. 이 방법을 사용하면 버퍼를 검색하여 검색할 식이 포함되어 있는지 확인할 수 있습니다. 메소드는 값을 찾을 수 있는지 여부에 따라 부울 `true` 또는 `false`를 반환합니다.

```coffeescript
const buf = Buffer.from('this is a buffer');
console.log(buf.includes('this'));
// This will print true

console.log(buf.includes(Buffer.from('a buffer example')));
// This will print false
```

### Buffer.is 인코딩()

바이너리는 구별하기 위해 인코딩되어야 하므로, 특정 데이터 유형이 할당된 문자 인코딩을 지원하는지 어떻게 알 수 있습니까? Buffer.isEncoding() 방법을 사용하여 특정 문자 인코딩 유형이 지원되는지 여부를 확인할 수 있습니다.

```coffeescript
console.log(Buffer.isEncoding('hex'));
// This will print true

console.log(Buffer.isEncoding('utf-8'));
// This will print true

console.log(Buffer.isEncoding('utf/8'));
// This will print false

console.log(Buffer.isEncoding('hey'));
// This will print false
```

### buff.➡➡

자바스크립트의 슬라이스 메소드처럼 buf.slice ➡`는 배열의 선택된 요소에서 새 배열을 만드는 데 사용됩니다. 배열을 분할할 때 슬라이스에서 선택한 요소 목록이 있는 새 배열을 만듭니다.

```undefined
var a = Buffer.from('uvwxyz');
var b = a.slice(2,5);

console.log(b.toString());
//This will print wxy
```

### 버퍼 스왑

버퍼 스왑은 버퍼 개체의 바이트 순서를 스왑하는 데 사용됩니다. 이 방법은 고속 엔디안 변환에도 사용할 수 있습니다.

buf.swap16()saph `buf.swap32()saph 및 buf.swap64()를 사용하여 각각 16비트, 32비트 및 64비트 버퍼 개체의 바이트 순서를 바꿀 수 있습니다.

```cpp
const buf1 = Buffer.from([0x1, 0x2, 0x3, 0x4, 0x5, 0x6, 0x7, 0x8]);
console.log(buf1);
// This will print <Buffer 01 02 03 04 05 06 07 08>

//swap byte order to 16 bit
buf1.swap16();
console.log(buf1);
// This will print <Buffer 02 01 04 03 06 05 08 07>

//swap byte order to 32 bit
buf1.swap32();
console.log(buf1);
// This will print <Buffer 03 04 01 02 07 08 05 06>

//swap byte order to 64 bit
buf1.swap64();
console.log(buf1);
// This will print <Buffer 06 05 08 07 02 01 04 03>
```

### buff.jsonname

이 방법을 사용하여 버퍼 개체에서 JSON 개체를 반환할 수 있습니다. buf.json() 메서드는 버퍼 개체의 JSON 버전을 반환합니다.

```coffeescript
const buf = Buffer.from([0x1, 0x2, 0x3, 0x4, 0x5, 0x6, 0x7, 0x8]);

console.log(buf.toJSON());
// This will print {"type":"Buffer", data:[1,2,3,4,5,6,7,8]}
```

### 버퍼 오프셋 읽기

버퍼 개체에서 할당되지 않은 정수를 읽기 시작하기 전에 건너뛸 바이트 수를 결정할 수 있습니다. 건너뛸 바이트 수는 이 메서드에 연결한 오프셋에 따라 결정됩니다.

64비트 이중, 32비트 부동, 16비트 정수 및 32비트 정수를 읽을 수 있습니다. 사용하는 Node.js 방법에 따라 결과는 작은 엔디안 또는 큰 엔디안일 수 있습니다.

`buf.readInt8()을 사용하여 8비트 정수를 읽을 수도 있습니다.

### 버퍼 오프셋 쓰기

이 메서드는 메서드에 연결된 오프셋에 따라 버퍼 개체에 지정된 바이트를 쓰는 데 사용됩니다.

이 방법은 64비트 이중, 32비트 부동, 8비트 정수, 16비트 정수, 32비트 정수를 리틀 엔디안 또는 빅 엔디안 단위로 쓸 수 있다. 각 방법의 값은 메서드에서 지정한 정수와 일치해야 합니다. 예를 들어 `buf`입니다.writeFlatBE()의 값은 32비트 float이어야 합니다.

## 결론

프로그램을 실행할 때 후드에서 무슨 일이 일어나고 있는지 이해하는 것이 중요합니다. 이제 이진, 스트림 및 버퍼 간의 관계와 Node.js 버퍼의 작동 방식을 확실하게 이해해야 합니다. 또한 Node.js 버퍼 클래스와 다양한 Node.js 버퍼 방법을 사용해야 하는 이유도 이해해야 합니다.

Node.js 버퍼 클래스에서 사용할 수 있는 모든 메소드를 다룰 수 있는 기회를 얻지 못했습니다. 또한 `allocUnsafe()` 메서드로 버퍼 개체를 만들 수 있지만 생성된 버퍼는 0이 채우지 않습니다. 버퍼 개체를 비교하려면 Buffer.compare() 메서드가 적합하지만 비교 후 buff.quals()가 `1`, `-1`, `0`이 `Buffer.compare()에서 반환되는 대신 true 또는 false를 반환합니다.

Node.js 버퍼 모듈에 대한 자세한 내용은 Node.js 문서에서 확인할 수 있습니다.