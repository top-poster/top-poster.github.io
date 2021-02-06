---
layout: post
title: "오늘날 이동 중인 암호화"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/cryptography-golang-today.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/cryptography-golang-today.png?fit=730%2C487&ssl=1)

암호학은 제3자 적대자가 있는 곳에서 안전한 통신을 위한 기술의 실천과 연구이다. 웹 애플리케이션에서 개발자들은 사용자 데이터가 안전하고 개인적인 이익을 위해 시스템의 허점을 이용하려는 나쁜 행위자들에 의해 시스템이 악용되지 않도록 하기 위해 암호화를 사용한다.

대부분의 프로그래밍 언어에는 공통 암호화 원시, 알고리즘 등의 구현이 있다. 이 기사에서는 바둑 프로그래밍 언어로 암호화가 어떻게 처리되는지, 현재 어떤 암호 패키지를 이용할 수 있는지 살펴본다.

먼저 표준 바둑 라이브러리의 암호화 패키지를 살펴보겠습니다.

## Go의 표준 암호화 패키지

만약 여러분이 꽤 오랫동안 바둑을 써왔다면, 표준 바둑 라이브러리가 HTTP에서 인코딩, 심지어 테스트까지 망라하여 놀랍도록 강력하다는 것에 동의할 것이다. 그래서 바둑이 자체 암호 패키지를 가지고 온다는 것은 놀랄 일이 아니다.

암호화 패키지 자체는 일반적인 암호화 상수, 기본 암호화 원칙을 위한 구현 등을 포함한다. 그러나 그것의 가치의 대부분은 그것의 하위 패키지에 있다. 암호화 패키지에는 다양한 하위 패키지가 있으며, 각 패키지는 단일 암호화 알고리즘, 원리 또는 표준에 초점을 맞춘다.

AES(Advanced Encryption Standard)에 초점을 맞춘 es 패키지, 디지털 서명 및 확인을 위한 HMAC(해시 기반 메시지 인증 코드)에 초점을 맞춘 hmac 및 기타 여러 가지가 있습니다. 이러한 패키지를 통해 암호화, 암호 해독, 해시 등과 같은 다양한 암호화 관련 작업을 수행할 수 있습니다. 어떻게 하면 좋을지 알아보도록 하죠.

### 해싱

해시(Hashing)는 기본적으로 임의의 크기의 입력을 취하여 고정된 크기의 출력을 생성하는 프로세스입니다. 적어도 좋은 해싱 알고리즘은 두 개의 다른 입력에 대해 동일한 출력을 생성하지 않으며 항상 주어진 입력에 대해 동일한 출력을 생성한다.

SHA-256, SHA-1 및 MD5와 같은 다양한 해싱 알고리즘이 있으며, Go 암호화 패키지에서 모두 지원됩니다. 다음은 SHA-256 해시 알고리즘을 사용하여 일반 텍스트를 해시하고 해시를 16진수 형식으로 반환하는 함수의 구현입니다.

```cpp
func hashWithSha256(plaintext string) (string, error) {
   h := sha256.New()
   if _, err := io.WriteString(h, plaintext);err != nil{
      return "", err
   }
   r := h.Sum(nil)
   return hex.EncodeToString(r), nil
}

func main(){
  hash, err := hashWithSha256("hashsha256")
  if err != nil{
     log.Fatal(err)
  }
  fmt.Println(hash)  //c4107b10d93310fb71d89fb20eec1f4eb8f04df12e3f599879b03be243093b14
}
```

보시다시피 sha256 하위 패키지의 `New` 함수는 해시 인터페이스를 구현하는 유형을 반환합니다. 이 인터페이스를 구현하는 모든 유형은 기록기 인터페이스도 구현합니다. 따라서, 우리는 간단한 텍스트를 여기에 쓰고, `Sum` 방법을 사용하여 체크섬을 얻고, 결과를 16진수 형식으로 인코딩할 수 있다.

이 코드는 다른 해시 알고리즘과도 작동하므로 적절한 패키지에서 해시를 만들면 됩니다. MD5 알고리즘을 사용하면 다음과 같은 이점이 있습니다.

```undefined
h := md5.New()
```

### 대칭키 암호

또한 Go 표준 라이브러리만 사용하여 대칭 키 암호화를 구현할 수 있다. 대칭 키 암호화는 단순히 일반 텍스트를 암호화하고 동일한 키로 해당 암호문을 해독하는 것을 포함한다.

Go crypto 패키지로 암호화 및 암호 해독에 스트림 및 블록 암호를 사용할 수 있습니다. CBC(Cipher Block Chain) 모드를 사용하여 AES를 사용하여 대칭 키 암호화를 구현하는 방법을 살펴봅시다.

먼저 주어진 키로 새로운 블록 암호를 만드는 함수를 작성한다. AES는 키 길이가 128, 192 또는 256비트인 키만 사용합니다. 그래서 우리는 주어진 키를 해시하고 우리의 블록 암호의 키로 해시를 전달할 것이다. 이 함수는 암호 하위 패키지에서 차단과 오류를 반환합니다.

```undefined
func newCipherBlock(key string) (cipher.Block, error){
   hashedKey, err := hashWithSha256(key)
   if err != nil{
      return nil, err
   }
   bs, err := hex.DecodeString(hashedKey)
   if err != nil{
      return nil, err
   }
   return aes.NewCipher(bs[:])
}
```

암호화와 암호 해독을 위한 함수를 쓰기 시작하기 전에 일반 텍스트의 패딩과 언패딩을 위한 두 가지 함수를 작성해야 합니다. 패딩(Padding)은 고정 크기(일반적으로 블록 크기)의 배수가 될 수 있도록 일반 텍스트의 길이를 증가시키는 행위이다. 이 작업은 일반적으로 일반 텍스트에 문자를 추가하여 수행됩니다.

패딩 방식은 여러 가지가 있는데, 바둑이 자동으로 일반 텍스트를 패딩하지 않기 때문에 우리가 직접 패딩을 해야 한다. 사용자 huyinghuan에 의한 GitHubgist는 RFC 2315의 섹션 10.3에 정의된 PKCS7 패딩 방식을 사용하여 일반 텍스트를 패딩하는 쉬운 방법을 보여준다.

```undefined
var (
   // ErrInvalidBlockSize indicates hash blocksize <= 0.
   ErrInvalidBlockSize = errors.New("invalid blocksize")

   // ErrInvalidPKCS7Data indicates bad input to PKCS7 pad or unpad.
   ErrInvalidPKCS7Data = errors.New("invalid PKCS7 data (empty or not padded)")

   // ErrInvalidPKCS7Padding indicates PKCS7 unpad fails to bad input.
   ErrInvalidPKCS7Padding = errors.New("invalid padding on input")
)

func pkcs7Pad(b []byte, blocksize int) ([]byte, error) {
   if blocksize <= 0 {
      return nil, ErrInvalidBlockSize
   }
   if b == nil || len(b) == 0 {
      return nil, ErrInvalidPKCS7Data
   }
   n := blocksize - (len(b) % blocksize)
   pb := make([]byte, len(b)+n)
   copy(pb, b)
   copy(pb[len(b):], bytes.Repeat([]byte{byte(n)}, n))
   return pb, nil
}

func pkcs7Unpad(b []byte, blocksize int) ([]byte, error) {
   if blocksize <= 0 {
      return nil, ErrInvalidBlockSize
   }
   if b == nil || len(b) == 0 {
      return nil, ErrInvalidPKCS7Data
   }

   if len(b)%blocksize != 0 {
      return nil, ErrInvalidPKCS7Padding
   }
   c := b[len(b)-1]
   n := int(c)
   if n == 0 || n > len(b) {
      fmt.Println("here", n)
      return nil, ErrInvalidPKCS7Padding
   }
   for i := 0; i < n; i++ {
      if b[len(b)-n+i] != c {
         fmt.Println("hereeee")
         return nil, ErrInvalidPKCS7Padding
      }
   }
   return b[:len(b)-n], nil
}
```

이제 그 내용을 적어보았으니 암호화와 암호 해독을 위한 기능을 쓸 수 있습니다.

```undefined
//encrypt encrypts a plaintext
func encrypt(key, plaintext string) (string, error) {
   block, err := newCipherBlock(key)
   if err != nil {
      return "", err
   }

  //pad plaintext
   ptbs, _ := pkcs7Pad([]byte(plaintext), block.BlockSize())

   if len(ptbs)%aes.BlockSize != 0 {
      return "",errors.New("plaintext is not a multiple of the block size")
   }

   ciphertext := make([]byte, len(ptbs))

  //create an Initialisation vector which is the length of the block size for AES
   var iv []byte = make([]byte, aes.BlockSize)
   if _, err := io.ReadFull(rand.Reader, iv); err != nil {
      return "", err
   }

   mode := cipher.NewCBCEncrypter(block, iv)

  //encrypt plaintext
   mode.CryptBlocks(ciphertext, ptbs)

  //concatenate initialisation vector and ciphertext
   return hex.EncodeToString(iv) + ":" + hex.EncodeToString(ciphertext), nil
}


//decrypt decrypts ciphertext
func decrypt(key, ciphertext string) (string, error) {
   block, err := newCipherBlock(key)
   if err != nil {
      return "", err
   }

  //split ciphertext into initialisation vector and actual ciphertext
   ciphertextParts := strings.Split(ciphertext, ":")
   iv, err := hex.DecodeString(ciphertextParts[0])
   if err != nil {
      return "", err
   }
   ciphertextbs, err := hex.DecodeString(ciphertextParts[1])
   if err != nil {
      return "", err
   }

   if len(ciphertextParts[1]) < aes.BlockSize {
      return "", errors.New("ciphertext too short")
   }

   // CBC mode always works in whole blocks.
   if len(ciphertextParts[1])%aes.BlockSize != 0 {
      return "", errors.New("ciphertext is not a multiple of the block size")
   }

   mode := cipher.NewCBCDecrypter(block, iv)


   // Decrypt cipher text
   mode.CryptBlocks(ciphertextbs, ciphertextbs)

  // Unpad ciphertext
   ciphertextbs, err = pkcs7Unpad(ciphertextbs, aes.BlockSize)
   if err != nil{
      return "", err
   }

   return string(ciphertextbs), nil
}
```

이제 다음과 같은 기능을 테스트할 수 있습니다.

```cpp
func main() {
  pt := "Highly confidential message!"
  key := "aSecret"

   ct, err := encrypt(key, pt)
   if err != nil{
      log.Fatalln(err)
   }
   fmt.Println(ct)  //00af9595ed8bae4c443465aff651e4f6:a1ceea8703bd6aad969a64e7439d0664320bb2f73d9a31433946b81819cb0085

   ptt, err := decrypt(key, ct)
   if err != nil{
      log.Fatalln(err)
   }
   fmt.Println(ptt)  //Highly confidential message!

}
```

### 공개 키 암호화

공용 키 암호화는 다른 키가 암호화 및 암호 해독에 사용된다는 점에서 대칭 키 암호화와는 다르다. 암호 해독에 사용되는 개인 키와 암호화에 사용되는 공개 키의 두 가지 키가 있습니다.

RSA는 공개 키 암호 시스템의 일반적인 예로, RSA 하위 패키지를 사용하여 Go에서 구현할 수 있습니다.

RSA를 구현하려면 먼저 프라이빗 및 퍼블릭 키를 생성해야 합니다. 이를 위해 우리는 `GenerateKey`를 사용하여 개인 키를 생성한 다음 개인 키에서 공개 키를 생성할 수 있습니다.

```cpp
func main(){
//create an RSA key pair of size 2048 bits
  priv, err := rsa.GenerateKey(rand.Reader, 2048)
  if err != nil{
     log.Fatalln(err)
  }

  pub := priv.Public()
}
```

이제 RSA를 OAEP와 함께 사용하여 일반 텍스트 및 암호문을 원하는 대로 암호화 및 해독할 수 있습니다.

```undefined
func main(){
    ...
    options := rsa.OAEPOptions{
     crypto.SHA256,
     []byte("label"),
  }

  message := "Secret message!"

  rsact, err := rsa.EncryptOAEP(sha256.New(), rand.Reader, pub.(*rsa.PublicKey), []byte(message), options.Label)
  if err != nil{
     log.Fatalln(err)
  }

  fmt.Println("RSA ciphertext", hex.EncodeToString(rsact))

  rsapt, err := priv.Decrypt(rand.Reader,rsact, &options)
  if err != nil{
     log.Fatalln(err)
  }

  fmt.Println("RSA plaintext", string(rsapt))

}
```

### 디지털 서명

디지털 서명은 암호화의 또 다른 응용 프로그램입니다. 기본적으로 디지털 서명을 통해 네트워크를 통해 전송되는 메시지가 공격자가 조작하지 않았는지 확인할 수 있습니다.

디지털 서명을 구현하는 일반적인 방법은 MAC(Message Authentication Code), 특히 HMAC를 사용하는 것입니다. HMAC는 해시 함수를 사용하며 메시지의 신뢰성을 보장하는 안전한 방법이다. 우리는 hmac 하위 패키지를 사용하여 Go에서 HMAC를 구현할 수 있습니다.

이 작업이 수행되는 예는 다음과 같습니다.

```undefined
/*hmacs make use of an underlying hash function so we have to specify one*/
mac := hmac.New(sha256.New, []byte("secret"))
mac.Write([]byte("Message whose authenticity we want to guarantee"))
macBS := mac.Sum(nil) //

falseMac := []byte("someFalseMacAsAnArrayOfBytes")
equal := hmac.Equal(falseMac, macBS)
fmt.Println(equal)  //false - therefore the message to which this hmac is attached has been tampered
```

## 비크립트

바둑 표준 암호 라이브러리 외에도 바둑 생태계에는 다른 암호 관련 패키지가 있다. 이것들 중 하나는 bcrypt이다.

bcrypt 패키지는 널리 사용되는 해시 알고리즘 bcrypt의 Go 구현이다. Bcrypt는 해시 암호에 대한 업계 표준 알고리즘이며, 대부분의 언어에는 bcrypt 구현의 형태가 있다.

이 패키지에서는 암호 생성 기능을 사용하여 암호에서 bcrypt 해시를 얻을 수 있으며, 비용 지불을 할 수 있습니다.

```bash
hash, err := bcrypt.GenerateFromPassword("password", bcrypt.DefaultCost)
```

그런 다음 나중에 다음 작업을 수행하여 지정된 암호가 지정된 해시와 일치하는지 확인할 수 있습니다.

```undefined
err := bcrypt.CompareHashAndPassword([]byte("hashedPassword"), []byte("password"))
```

## 결론

이 기사는 여기까지입니다! 바라건대, 이 기사는 적어도 암호와 관련하여 바둑 생태계가 얼마나 강력한지 알려줍니다. 여기서 바둑 표준 라이브러리의 내용을 확인하면 바둑과 함께 구울 수 있는 또 다른 기능이 무엇인지 확인할 수 있습니다.