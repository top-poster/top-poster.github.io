---
layout: post
title: "Node.js 암호화 모듈: 튜토리얼"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/nodejs-crypto.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/nodejs-crypto.png?fit=730%2C487&ssl=1)

만약 범죄자들이 당신의 데이터베이스를 장악한다면 사용자 데이터는 어떻게 될까요? 사이버 범죄는 지속적인 위협이며, 나쁜 행위자들은 당신의 데이터베이스를 복제하기 위해 악의적인 스크립트를 전달하기 위해 모든 구석에 숨어 있다. 사용자 정보를 보호하기 위해 어떤 추가 단계를 수행할 수 있습니까?

예를 들어 사용자가 응용 프로그램에서 계정을 만들 때 암호와 사용자 이름을 암호화하여 데이터베이스에 안전하게 보관해야 합니다. 그러나 계정에 로그인하기 위해 사용자의 암호와 사용자 이름이 데이터베이스에 이미 있는 자격 증명 집합에 대해 검증됩니다.

데이터베이스의 암호(gibberish로 암호화됨)가 사용자가 입력한 암호/이메일을 비교하는 데 사용되는 경우에는 이 기능이 작동하지 않습니다. 따라서 데이터베이스의 자격 증명을 사용자의 입력과 비교하는 동안 암호를 해독해야 합니다. 암호는 해시하거나 암호화할 수 있습니다. 해시는 단방향 암호화 방법입니다.

가장 좋은 해결책은 중요한 정보를 데이터베이스로 보내기 전에 암호화 방식을 사용하는 것이다. 이런 식으로, 사이버 범죄자들이 당신의 데이터베이스를 파악하면, 그들이 보는 모든 것은 임의의 문자입니다.

이 튜토리얼에서는 Node.js에서 암호화의 기본 사항을 살펴보고 Node.js 암호화 모듈을 사용하여 사용자 데이터를 보호하는 방법을 시연합니다. 암호화를 사용하여 Node.js에서 사용자 이름과 암호를 암호화하고 해독하는 방법을 보여주는 샘플 앱을 구축하겠습니다.

다음 내용을 다루겠습니다.

- Node.js의 암호화란?
- Node.js 암호화 모듈이란 무엇입니까?
- Node.js 암호화 클래스
- Node.js 앱에서 암호화 사용
- Node.js 앱에 암호화 추가
- Node.js crypto를 사용해야 합니까?

이 튜토리얼을 따르려면 다음을 수행해야 합니다.

- 작업 환경에 설치된 Node.js
- 작업 환경에서 Git를 다운로드하고 설정합니다.
- 사용자 세부 정보를 저장할 MongoDB

## Node.js의 암호화란?

암호화는 일반 텍스트를 읽을 수 없는 텍스트로 변환하는 과정이며 그 반대의 경우도 마찬가지이다. 이러한 방식으로 정보의 발신자와 수신자만이 그 내용을 이해한다.

Node.js의 암호화 기능을 사용하면 암호를 해시하고 데이터베이스에 저장하여 해시된 후 데이터를 일반 텍스트로 변환할 수 없으며 확인만 할 수 있습니다. 악의적인 행위자가 데이터베이스를 장악하면 암호화된 정보를 디코딩할 수 없습니다. 전송 중에 암호를 해독할 수 있도록 다른 사용자 데이터도 암호화할 수 있습니다.

응용프로그램에 사용하는 암호화 유형은 사용자의 필요에 따라 다릅니다. 예를 들어, 암호화는 해시 같은 대칭 키, 공용 키(암호화 또는 암호 해독 같은)일 수 있습니다.

암호화된 데이터를 수신하는 최종 당사자는 해당 데이터를 일반 텍스트로 해독하여 사용할 수 있습니다. 사이버 범죄자는 키가 없으면 암호화된 데이터의 암호를 해독할 수 없습니다. 이것이 바로 Node.js crypto module이 하는 것이다.

## Node.js 암호화 모듈이란 무엇입니까?

Node.js 암호화 모듈은 Node.js 앱을 보호하는 데 도움이 되는 암호화 기능을 제공합니다. Open용 포장지 세트가 포함되어 있습니다.SSL의 해시, HMAC, 암호, 해독, 서명 및 확인 함수입니다.

crypto는 Node.js에 내장되어 있으므로 엄격한 구현 프로세스 및 구성이 필요하지 않습니다. 다른 모듈과 달리 Node.js 응용 프로그램에서 사용하기 전에 Crypto를 설치할 필요가 없습니다.

crypto를 사용하면 데이터베이스에 저장하기 전에 일반 텍스트를 해시할 수 있습니다. 이를 위해 고정 길이, 결정론적, 충돌 방지 및 단방향 해시를 생성할 수 있는 해시 클래스가 있습니다. 해시된 데이터의 경우 암호화된 데이터와 달리 미리 결정된 키로 암호를 해독할 수 없습니다. HMAC 클래스는 단일 최종 해시를 생성하기 위해 키와 값을 해시하는 해시 기반 메시지 인증 코드를 담당합니다.

전송 목적을 위해 나중에 다른 사용자 데이터를 암호화하고 해독해야 할 수도 있습니다. 여기서 암호기와 해독기가 나온다. 암호 클래스로 데이터를 암호화하고 암호 해독기로 암호를 해독할 수 있습니다. 데이터베이스에 저장하기 전에 데이터를 암호화하지 않는 경우가 있습니다.

암호화되거나 해시된 암호를 확인하여 유효한지 확인할 수도 있습니다. 당신이 필요한 것은 `확인` 클래스만 있으면 됩니다. 인증서도 서명 클래스로 서명할 수 있다.

이 모든 것들이 개발자들이 암호화 모듈을 사용하는 것을 좋아하는 이유이다. 다양한 암호화 클래스를 탐색하고 이를 통해 암호화를 구현하는 방법을 알아보자.

## Node.js 암호화 클래스

암호화를 구현할 수 있는 클래스를 암호로 살펴보겠습니다.

### 암호기

암호 클래스는 정보를 암호화하는 역할을 합니다. 사용자가 등록 중에 비밀번호를 입력하면 C 아이퍼 클래스가 호출되어 비밀번호를 암호화합니다.

먼저 알고리즘에서 키를 생성하겠습니다. 그런 다음 텍스트를 암호화하기 전에 임의 초기화 번호(iv)를 생성합니다.

이 클래스를 사용하려면 `crypto.createCipher() 또는 `crypto.createCiperiv()를 사용하여 암호 인스턴스를 생성해야 합니다. crypto.createCipher()는 감가상각되므로 crypto.createCipher()를 사용하는 것이 좋습니다.

아래 프로그램에서는 암호 방식을 사용하여 암호를 암호화하는 방법을 보여 줍니다.

```coffeescript
// Import module into your application
const crypto = require('crypto');

const algorithm = 'aes-192-cbc';
const password = '2001MyForever';

// We will first generate the key, as it is dependent on the algorithm.
// In this case for aes192, the key is 24 bytes (192 bits).
crypto.scrypt(password, 'salt', 24, (err, key) => {
  if (err) throw err;
  // After that, we will generate a random iv (initialization vector)
  crypto.randomFill(new Uint8Array(16), (err, iv) => {
    if (err) throw err;

    // Create Cipher with key and iv
    const cipher = crypto.createCipheriv(algorithm, key, iv);

    let encrypted = '';
    cipher.setEncoding('hex');

    cipher.on('data', (chunk) => encrypted += chunk);
    cipher.on('end', () => console.log(encrypted));// Prints encrypted data with key

    cipher.write('some clear text data');
    cipher.end();
  });
});
```

### 해독기

암호 해독기는 암호화된 텍스트의 암호를 해독하는 역할을 합니다. 정보를 다른 개발자에게 안전하게 보내려면 해당 정보를 암호화해야 합니다. 정보의 수신자가 정보를 읽을 수 있는 유일한 방법은 그것을 해독하는 것이다. 디시퍼(Decipher) 계층이 하는 일이 바로 이런 것이다.

새 키워드로 직접 해독 개체를 만들 수 없습니다. crypto.createDecipher() 또는 crypto.createDeciperriv() 메서드는 해독 인스턴스를 만드는 데 사용됩니다.

crypto.createDecipher()가 감가상각되었으므로 대신 crypto.created eCipheriv() 방법을 사용해야 합니다.

암호 해독기로 암호화된 텍스트를 해독하는 방법은 다음과 같습니다.

```php
// Import module into your application
const crypto = require('crypto');

const algorithm = 'aes-192-cbc';
const password = 'Password used to generate key';

// We will first generate the key, as it is dependent on the algorithm.
// In this case for aes192, the key is 24 bytes (192 bits).
// We will use the async `crypto.scrypt()` instead for deciphering.
const key = crypto.scryptSync(password, 'salt', 24);
// The IV is usually passed along with the ciphertext.
const iv = Buffer.alloc(16, 0); // Initialization vector.

// Create decipher with key and iv
const decipher = crypto.createDecipheriv(algorithm, key, iv);

let decrypted = '';
decipher.on('readable', () => {
  while (null !== (chunk = decipher.read())) {
    decrypted += chunk.toString('utf8');
  }
});
decipher.on('end', () => {
  console.log(decrypted);
  // Prints: some clear text data
});

// Encrypted with same algorithm, key and iv.
const encrypted =
  'e5f79c5915c02171eec6b212d5520d44480993d7d622a7c4c2da32f6efda0ffa';
decipher.write(encrypted, 'hex');
decipher.end();
```

### 해시

해시 클래스는 일반 텍스트 해시 용도로 사용됩니다. 해시는 단순히 일반 텍스트를 해시 함수로 변환합니다. 해시된 텍스트를 원래 버전으로 다시 변환할 수 없습니다. 해시 개체는 `new` 키워드로 직접 생성할 수 없습니다.

해시 인스턴스를 생성하려면 아래 예와 같이 `crypto.createHash()` 방법을 사용하십시오.

```js
// Import module into your application
const crypto = require('crypto');
// create hash algorithm 
const hash = crypto.createHash('sha256');

hash.on('readable', () => {
  // Only one element is going to be produced by the
  // hash stream.
  const data = hash.read();
  if (data) {
    console.log(data.toString('hex'));
    // Prints:
    //   6a2da20943931e9834fc12cfe5bb47bbd9ae43489a30726962b576f4e3993e50
  }
});

hash.write('some data to hash');
hash.end();
```

### '인증서'

인증서(Certificate)는 키 쌍과 전자 문서 암호화에 사용되는 기타 정보로 구성된다.

인증서는 인터넷을 통해 정보를 안전하게 전송할 목적으로 세션 키를 생성할 수 있습니다. Certificate `Certificate` 클래스로 Open을 사용하여 SPKAC(Signed Public Key and Challenge)로 작업할 수 있습니다.SSL의 SPKAC 구현.

다음은 `인증서` 클래스를 사용하는 방법의 예입니다.

```js
const { Certificate } = require('crypto');
const spkac = getSpkacSomehow();
const challenge = Certificate.exportChallenge(spkac);
console.log(challenge.toString('utf8'));
// Prints: the challenge as a UTF8 string
```

### 디피 헬만

암호 해독을 성공적으로 수행하려면, 특히 암호화된 암호 해독을 위한 키가 필요합니다. 열쇠는 송신자와 수신자 사이의 공유 비밀과 같다. 키를 안전하게 보관하지 않으면 해커가 키를 잡고 사용자 정보에 혼란을 초래할 수 있습니다.

크립토(Crypto)의 디피헬만(DiffieHellman) 클래스는 디피헬만(Diffie-Hellman) 핵심 거래소를 활용한다. Diffie-Hellman 키 교환은 공개 채널에서 암호화 키를 안전하게 전달하는 방법이다. 이렇게 하면 정보 발신자 및 수신자 전용 키가 보호됩니다.

`Diffie Hellman` 방법을 사용하려면 아래 예제를 따르십시오.

```java
onst crypto = require('crypto');
const assert = require('assert');

// Generate Alice's keys...
const alice = crypto.createDiffieHellman(2048);
const aliceKey = alice.generateKeys();

// Generate Bob's keys...
const bob = crypto.createDiffieHellman(alice.getPrime(), alice.getGenerator());
const bobKey = bob.generateKeys();

// Exchange and generate the secret...
const aliceSecret = alice.computeSecret(bobKey);
const bobSecret = bob.computeSecret(aliceKey);

// OK
assert.strictEqual(aliceSecret.toString('hex'), bobSecret.toString('hex'));
```

### ECDH

타원곡선 디피-Hellman(ECDH)은 타원 곡선으로 공유된 공공-개인 키 쌍을 설정하는 데 사용된다.

아래 예제에서는 Node.js 응용 프로그램에서 `ECDH` 클래스를 사용하는 방법을 보여 줍니다.

```java
const crypto = require('crypto');
const assert = require('assert');

// Generate Alice's keys...
const alice = crypto.createECDH('secp521r1');
const aliceKey = alice.generateKeys();

// Generate Bob's keys...
const bob = crypto.createECDH('secp521r1');
const bobKey = bob.generateKeys();

// Exchange and generate the secret...
const aliceSecret = alice.computeSecret(bobKey);
const bobSecret = bob.computeSecret(aliceKey);

assert.strictEqual(aliceSecret.toString('hex'), bobSecret.toString('hex'));
// OK
```

### HMAC

해시 기반 메시지 인증 코드(HMAC)를 사용하면 공유 암호를 사용하여 디지털 서명을 제공할 수 있습니다. Crypto의 `HMAC` 클래스는 HMAC 방식을 디지털 서명에 사용합니다.

```js
const crypto = require('crypto');
const hmac = crypto.createHmac('sha256', 'a secret');

hmac.on('readable', () => {
  // Only one element is going to be produced by the
  // hash stream.
  const data = hmac.read();
  if (data) {
    console.log(data.toString('hex'));
    // Prints:
    //   7fd04df92f636fd450bc841c9418e5825c17f33ad9c87c518115a45971f7f77e
  }
});

hmac.write('some data to hash');
hmac.end();
```

### 기표기'

sign 클래스는 서명을 생성하기 위한 것입니다. 효율적인 암호화를 위해 암호화에 서명하고 나중에 인증 여부를 확인해야 합니다. 이렇게 하면 수신자가 암호문을 받았을 때, 그들은 그 암호에 적힌 서명을 확인함으로써 그것이 진짜인지 알 수 있다.

```js
const crypto = require('crypto');

const { privateKey, publicKey } = crypto.generateKeyPairSync('ec', {
  namedCurve: 'sect239k1'
});

// Create
const sign = crypto.createSign('SHA256');
sign.write('some data to sign');
sign.end();
const signature = sign.sign(privateKey, 'hex');
```

### ➡➡➡➡➡➡➡➡➡➡ ➡

해시된 암호기가 있는 경우 암호 값을 확인할 수 있는 유일한 방법은 `확인` 방법을 사용하는 것입니다.

예를 들어 등록 중에 사용자 암호를 해시하여 데이터베이스에 저장하는 경우 로그인하는 동안 사용자가 입력한 암호/사용자 이름을 확인해야 합니다. 해시된 암호를 해독할 수 없기 때문에 암호/사용자 이름 콤보가 올바른지 확인하는 유일한 방법은 `확인`을 사용하는 것입니다.

```coffeescript
// Verify signed token from `sign` example above
const verify = crypto.createVerify('SHA256');
verify.write('some data to sign');
verify.end();
console.log(verify.verify(publicKey, signature, 'hex'));
// Prints: true 
```

## Node.js 앱에서 암호화 사용

암호화를 사용하여 Node.js 앱에서 사용자 정보를 암호화 및 해독하는 방법을 시연하기 위해 사용자가 사용자 이름과 암호로 등록한 다음 이러한 자격 증명을 사용하여 로그인하는 샘플 Node.js 앱을 사용합니다.

사용자가 등록할 때, 자신의 세부 정보가 일반 텍스트로 저장되므로 해커가 데이터를 쉽게 훔칠 수 있습니다. 애플리케이션에 암호화를 추가하여 이 문제를 해결하는 방법을 보여드리겠습니다.

이 응용 프로그램은 사용자 인증에 Passport(여권)를 사용하여 사용자 암호/이메일 조합을 비교합니다.

먼저 샘플 Node.js 응용 프로그램을 다운로드하고 다음 명령을 사용하여 복제합니다.

```php
git clone https://github.com/hannydevelop/Crypto.git
```

응용 프로그램을 복제한 후 터미널을 사용하여 시스템에서 응용 프로그램 위치로 이동하십시오.

```bash
cd crypto
```

종속성을 설치하고 터미널에서 아래 명령을 실행하여 응용 프로그램을 시작하십시오.

```shell
# Install dependencies 
npm install
# Start application
node index.js
```

웹 브라우저에서 `http://localhost:3000/register`로 이동하여 응용프로그램을 확인합니다. 계정을 등록하고 MongoDB Compass를 사용하여 데이터베이스를 보면 사용자 암호가 일반 텍스트로 표시됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/crypto-nodejs-passwords-plaintext.png?resize=720%2C384&ssl=1)

우리는 이것을 피하고 싶다. 암호를 암호로 변환하기 위해 응용 프로그램에 암호(crypto)

## Node.js 앱에 암호화 추가

Node.js 응용 프로그램에 암호화를 추가하려면 아래 단계를 따르십시오.

- 암호화 모듈을 추가하고 모든 사용자에 대해 소금을 지정합니다.
// 응용 프로그램으로 모듈 가져오기
constrypto = 필수 항목`)
// 모든 사용자를 위한 소금 생성
소금 = `f844b09ff50c`로 함
- 레지스터 방법에 다음 코드를 추가합니다.
// User.create(userData) 바로 위에 추가

// 해시 사용자 암호 및 1000회 반복 사용
hash = crypto.pbkdf2Sync(userData.password, salt,
1000, 64, sha512).부터 스트링(헥스)까지;
userData.password = 해시
이제 `usercontroller.js`는 다음과 같이 표시됩니다.
//필요한 모든 종속성 가져오기
constexpress = require `;
contors = 필수 항목`;
constrypto = 필수 항목`)
//변수 사용자를 expressRouter로 설정
var 사용자 = express.라우터(;
//사용자 모델 가져오기
var {User} = require(`.../모델/사용자`);
//코스로 경로 보호
users.use(사용자)
// 모든 사용자를 위한 소금 생성
소금 = `f844b09ff50c`로 함
// 사용자 등록 처리
users.post https/register`, (req, res) = {
constuserData = {
//값은 사용자 모델에서 중요한 값이어야 합니다.
사용자 이름: req.body.dister,
암호: req.body.password,
first_name: req.body.first_name,
last_name: req.body.last_name,
}
User.findOne({)
//사용자 이름이 고유합니다. 즉, 사용자 이름이 데이터베이스에 아직 없습니다.
사용자 이름: req.body.contract
})
.then(사용자 => {
//사용자 이름이 고유할 경우
if (!user) {
hash = crypto.pbkdf2Sync(userData.password, salt,
1000, 64, sha512).부터 스트링(헥스)까지;
userData.password = 해시
//사용자 이름이 고유하면 해싱 암호 및 솔트 후 userData를 생성하십시오.
User.create(userData)
.then(사용자 => {
//사용자 생성 성공 후 데이터 표시 등록된 메시지 표시
res.cr/cr`)
})
.vp(예: = > {}
//userData를 생성하는 동안 오류가 발생한 경우, 계속하여 오류를 표시하십시오.
res.send 오류:` + 오류)
})
다른 {}개
//사용자 이름이 고유하지 않은 경우 사용자 이름이 이미 계정에 등록되어 있음을 표시합니다.
res.json({ 오류: `사용자 이름 ` +req.body.username + `이(가) 계정에 등록되었습니다.`)
}
})
.vp(예: = > {}
//오류가 발생한 경우 오류 표시
res.send 오류:` + 오류)
})
})
- 로그인 방법에 다음을 추가합니다.
//데이터베이스에 이와 같은 사용자 이름과 암호가 일치하는지 확인
사용자 이름: req.body.dister,
암호: crypto.pbkdf2Sync(req.body.password, salt,
1000, 64, sha512). to 문자열(`헥스`)
이제 users.post를 통해 다음과 같은 정보를 얻을 수 있습니다.
users.post https/field`, (req, res) = {
User.findOne({)
//데이터베이스에 이와 같은 사용자 이름과 암호가 일치하는지 확인
사용자 이름: req.body.dister,
암호: crypto.pbkdf2Sync(req.body.password, salt,
1000, 64, sha512). to 문자열(`헥스`)
})
.then(사용자 => {
//데이터베이스에 사용자 이름과 비밀번호가 일치하는 경우 사용자가 존재합니다.
(사용자) {인 경우
const 페이로드 = {
사용자 이름: user.contract,
암호: user.password
}
//로그인 성공 후 토큰 및 페이로드 데이터 표시
res.gnt/`;
다른 {}개
//사용자를 찾을 수 없는 경우 아래에 메시지를 표시합니다.
res.json 오류: `user not found` })
}
})
//사용자 로그인을 시도하는 동안 발생하는 모든 오류 발생 및 표시
.vp(예: = > {}
res.send 오류:` + 오류)
})
})

이제 Node.js 애플리케이션에 crypto의 해시 메서드를 추가했으므로 이를 실행하여 차이점을 살펴보겠습니다.

Node.js 애플리케이션을 실행하려면 터미널에서 아래 명령을 실행하십시오.

```css
node index.js
```

`http://localhost:3000/register`로 이동하여 응용 프로그램을 등록하고 로그인합니다. 이번에는 나침반의 조각이 다릅니다. (Cryptograph)).

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/crypto-node-mongodb-compass.png?resize=720%2C381&ssl=1)

## Node.js crypto를 사용해야 합니까?

이 기사에서는 Node.js 암호화 모듈로 사용자 데이터를 보호하는 방법을 시연했다.

JWT, Bcrypt 등 Node.js를 사용할 수 있는 다른 암호화 패키지가 있습니다. 그러나 이러한 패키지는 기본 제공이 아니며, 때때로 작업 암호화 자체에서 수행할 수 있는 추가 종속성이 필요합니다. 예를 들어 Bcrypt를 사용하는 경우 키에 JWT를 서명해야 합니다.

crypto는 bcrypt와 JWT와 같은 작업을 각각 수행하는 암호와 부호 방식을 가지고 있다. 그러나 작성하는 응용프로그램이 메시지를 암호화하지 않고 사용자 인증 전용인 경우 Bcrypt 및 JWT가 더 나은 옵션입니다. 그것은 모든 사용자에게 같은 소금을 사용하는 것이 좋은 관행이 아니기 때문이다. 각 사용자에 대해 고유한 솔트를 생성해야 합니다. 모범 사례 `this.critu = crypto.randomBytes(16).toString(`hex`);

이 프로세스에는 해시된 암호를 사용자가 입력한 암호와 비교하는 기능이 있습니다. Bcrypt의 compareSync() 방식은 해시된 암호와 일반 암호를 쉽게 비교할 수 있는 수단을 제공하므로 더 나은 대안이 된다.