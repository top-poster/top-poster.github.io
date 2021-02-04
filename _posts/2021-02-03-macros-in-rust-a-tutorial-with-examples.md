---
layout: post
title: "Rust의 매크로 : 예제가있는 튜토리얼
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/02/Screen-Shot-2021-02-02-at-10.39.25-AM.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/Screen-Shot-2021-02-02-at-10.39.25-AM.png?fit=730%2C487&ssl=1)

이 튜토리얼에서는 Rust의 매크로에 대한 소개와 예제와 함께 Rust 매크로를 사용하는 방법에 대한 데모를 포함하여 Rust 매크로에 대해 알아야 할 모든 것을 다룰 것입니다.
 

다음 내용을 다룹니다.
 

- Rust 매크로 란 무엇입니까?
 
- Rust의 매크로 유형
 
- Rust의 선언적 매크로
선언적 매크로 만들기
선언적 매크로를 사용한 Rust의 고급 파싱
구조체의 이름과 필드 파싱
구조체에서 메타 데이터 구문 분석
선언적 매크로의 한계
 
- 선언적 매크로 만들기
 
- 선언적 매크로를 사용한 Rust의 고급 파싱
 
- 구조체의 이름과 필드 파싱
 
- 구조체에서 메타 데이터 구문 분석
 
- 선언적 매크로의 한계
 
- Rust의 절차 적 매크로
속성과 유사한 매크로
사용자 지정 파생 매크로
함수형 매크로
 
- 속성과 유사한 매크로
 
- 사용자 지정 파생 매크로
 
- 함수형 매크로
 

## Rust 매크로 란 무엇입니까?
 

Rust는 매크로를 훌륭하게 지원합니다.
 매크로를 사용하면 메타 프로그래밍이라고하는 다른 코드를 작성하는 코드를 작성할 수 있습니다.
 

매크로는 함수와 유사한 기능을 제공하지만 런타임 비용이 없습니다.
 그러나 컴파일 시간 동안 매크로가 확장되기 때문에 약간의 컴파일 시간 비용이 있습니다.
 

Rust 매크로는 C의 매크로와 매우 다릅니다. Rust 매크로는 토큰 트리에 적용되는 반면 C 매크로는 텍스트 대체입니다.
 

## Rust의 매크로 유형
 

Rust에는 두 가지 유형의 매크로가 있습니다.
 

- 선언적 매크로를 사용하면 인수로 제공하는 Rust 코드에서 작동하는 일치 표현식과 유사한 것을 작성할 수 있습니다.
 사용자가 제공 한 코드를 사용하여 매크로 호출을 대체하는 코드를 생성합니다.
 
- 절차 적 매크로를 사용하면 주어진 Rust 코드의 추상 구문 트리 (AST)에서 작업 할 수 있습니다.
 proc 매크로는`TokenStream` (또는 두 개)에서 다른`TokenStream`으로의 함수이며 출력이 매크로 호출을 대체합니다.
 

선언적 매크로와 절차 적 매크로를 모두 확대하고 Rust에서 매크로를 사용하는 방법에 대한 몇 가지 예를 살펴 보겠습니다.
 

## Rust의 선언적 매크로
 

이러한 매크로는`macro_rules!`를 사용하여 선언됩니다.
 선언적 매크로는 약간 덜 강력하지만 중복 코드를 제거하기 위해 매크로를 만드는 데 사용하기 쉬운 인터페이스를 제공합니다.
 일반적인 선언 매크로 중 하나는`println!`입니다.
 선언적 매크로는 일치 할 때 매크로가 일치하는 팔 내부의 코드로 대체되는 인터페이스와 같은 `일치`를 제공합니다.
 

### 선언적 매크로 만들기
 

```php
// use macro_rules! <name of macro>{<Body>}
macro_rules! add{
 // macth like arm for macro
    ($a:expr,$b:expr)=>{
 // macro expand to this code
        {
// $a and $b will be templated using the value/variable provided to macro
            $a+$b
        }
    }
}

fn main(){
 // call to macro, $a=1 and $b=2
    add!(1,2);
}
```

이 코드는 두 숫자를 더하는 매크로를 만듭니다.
 `[macro_rules!]`는 매크로 이름,`add` 및 매크로 본문과 함께 사용됩니다.
 

매크로는 두 개의 숫자를 추가하지 않고 두 개의 숫자를 추가하는 코드로 자신을 대체합니다.
 매크로의 각 부분은 함수에 대한 인수를 취하며 여러 유형을 인수에 할당 할 수 있습니다.
 `add`함수가 단일 인수를 사용할 수도 있다면 다른 팔을 추가합니다.
 

```php
macro_rules! add{
 // first arm match add!(1,2), add!(2,3) etc
    ($a:expr,$b:expr)=>{
        {
            $a+$b
        }
    };
// Second arm macth add!(1), add!(2) etc
    ($a:expr)=>{
        {
            $a
        }
    }
}

fn main(){
// call the macro
    let x=0;
    add!(1,2);
    add!(x);
}
```

단일 매크로에 여러 분기가있을 수 있으며 다른 인수에 따라 다른 코드로 확장 될 수 있습니다.
 각 분기는 `$`기호로 시작하고 뒤에 토큰 유형이 오는 여러 인수를 사용할 수 있습니다.
 

- `item` — 함수, 구조체, 모듈 등과 같은 항목입니다.
 
- `block` — 블록 (즉, 중괄호로 둘러싸인 문장 및 / 또는 표현식 블록)
 
- `stmt` — 명령문
 
- `pat` — 패턴
 
- `expr` — 표현식
 
- `ty` — 유형
 
- `ident` — 식별자
 
- `path` — 경로 (예 :`foo`,`:: std :: mem :: replace`,`transmute :: <_, int>`,…)
 
- `meta` — 메타 항목.
 `# [...]`및`#! [...]`속성에 들어가는 것들
 
- `tt` — 단일 토큰 트리
 
- `vis` — 비어있을 수있는`Visibility` 한정자
 

이 예에서는 토큰 유형이 `ty`인 `$ typ`인수를 `u8`, `u16`등과 같은 데이터 유형으로 사용합니다.이 매크로는 숫자를 추가하기 전에 특정 유형으로 변환됩니다.
 

```php
macro_rules! add_as{
// using a ty token type for macthing datatypes passed to maccro
    ($a:expr,$b:expr,$typ:ty)=>{
        $a as $typ + $b as $typ
    }
}

fn main(){
    println!("{}",add_as!(0,2,u8));
}
```

Rust 매크로는 고정되지 않은 수의 인수도 지원합니다.
 연산자는 정규식과 매우 유사합니다.
 `*`는 0 개 이상의 토큰 유형에 사용되며 `+`는 0 개 또는 1 개의 인수에 사용됩니다.
 

```php
macro_rules! add_as{
    (
  // repeated block
  $($a:expr)
 // seperator
   ,
// zero or more
   *
   )=>{
       { 
   // to handle the case without any arguments
   0
   // block to be repeated
   $(+$a)*
     }
    }
}

fn main(){
    println!("{}",add_as!(1,2,3,4)); // => println!("{}",{0+1+2+3+4})
}
```

반복되는 토큰 유형은 `$ ()`로 묶여 있으며 그 뒤에 구분 기호와 `*`또는 `+`가 추가되어 토큰이 반복되는 횟수를 나타냅니다.
 구분 기호는 토큰을 서로 구별하는 데 사용됩니다.
 `$ ()`블록 뒤에`*`또는`+`는 반복되는 코드 블록을 나타내는 데 사용됩니다.
 위의 예에서`+ $ a`는 반복되는 코드입니다.
 

자세히 살펴보면 구문을 유효하게 만들기 위해 코드에 0이 추가되었음을 알 수 있습니다.
 이 0을 제거하고`add` 표현식을 인수와 동일하게 만들려면 TT muncher라는 새 매크로를 만들어야합니다.
 

```undefined
macro_rules! add{
 // first arm in case of single argument and last remaining variable/number
    ($a:expr)=>{
        $a
    };
// second arm in case of two arument are passed and stop recursion in case of odd number ofarguments
    ($a:expr,$b:expr)=>{
        {
            $a+$b
        }
    };
// add the number and the result of remaining arguments 
    ($a:expr,$($b:tt)*)=>{
       {
           $a+add!($($b)*)
       }
    }
}

fn main(){
    println!("{}",add!(1,2,3,4));
}
```

TT muncher는 각 토큰을 재귀 적 방식으로 개별적으로 처리합니다.
 한 번에 하나의 토큰을 처리하는 것이 더 쉽습니다.
 매크로에는 세 개의 팔이 있습니다.
 

- 첫 번째 팔은 단일 인수가 전달되는 경우 케이스를 처리합니다.
 
- 두 번째 인수는 두 개의 인수가 전달되는 경우를 처리합니다.
 
- 세 번째 팔은 나머지 인수와 함께`add` 매크로를 다시 호출합니다.
 

매크로 인수는 쉼표로 구분할 필요가 없습니다.
 여러 토큰을 다른 토큰 유형과 함께 사용할 수 있습니다.
 예를 들어 괄호는`ident` 토큰 유형과 함께 사용할 수 있습니다.
 Rust 컴파일러는 일치 된 팔을 가져 와서 인자 문자열에서 변수를 추출합니다.
 

```php
macro_rules! ok_or_return{
// match something(q,r,t,6,7,8) etc
// compiler extracts function name and arguments. It injects the values in respective varibles.
    ($a:ident($($b:tt)*))=>{
       {
        match $a($($b)*) {
            Ok(value)=>value,
            Err(err)=>{
                return Err(err);
            }
        }
        }
    };
}

fn some_work(i:i64,j:i64)->Result<(i64,i64),String>{
    if i+j>2 {
        Ok((i,j))
    } else {
        Err("error".to_owned())
    }
}

fn main()->Result<(),String>{
    ok_or_return!(some_work(1,4));
    ok_or_return!(some_work(1,0));
    Ok(())
}
```

`ok_or_return` 매크로는 연산이`Err`을 반환하거나 연산 값이`Ok`를 반환하면 함수를 반환합니다.
 함수를 인수로 취하고 match 문 내에서 실행합니다.
 함수에 전달 된 인수의 경우 반복을 사용합니다.
 

종종 일부 매크로를 단일 매크로로 그룹화해야합니다.
 이러한 경우 내부 매크로 규칙이 사용됩니다.
 매크로 입력을 조작하고 깨끗한 TT muncher를 작성하는 데 도움이됩니다.
 

내부 규칙을 만들려면`@`로 시작하는 규칙 이름을 인수로 추가합니다.
 이제 매크로는 명시 적으로 인수로 지정 될 때까지 내부 규칙과 일치하지 않습니다.
 

```undefined
macro_rules! ok_or_return{
 // internal rule.
    (@error $a:ident,$($b:tt)* )=>{
        {
        match $a($($b)*) {
            Ok(value)=>value,
            Err(err)=>{
                return Err(err);
            }
        }
        }
    };

// public rule can be called by the user.
    ($a:ident($($b:tt)*))=>{
        ok_or_return!(@error $a,$($b)*)
    };
}

fn some_work(i:i64,j:i64)->Result<(i64,i64),String>{
    if i+j>2 {
        Ok((i,j))
    } else {
        Err("error".to_owned())
    }
}

fn main()->Result<(),String>{
   // instead of round bracket curly brackets can also be used
    ok_or_return!{some_work(1,4)};
    ok_or_return!(some_work(1,0));
    Ok(())
}
```

### 선언적 매크로를 사용한 Rust의 고급 파싱
 

매크로는 때때로 Rust 언어 자체의 구문 분석이 필요한 작업을 수행합니다.
 

지금까지 다룬 모든 개념을 모아서`pub` 키워드를 접미사로 붙여 구조체를 공개하는 매크로를 만들어 보겠습니다.
 

먼저, 구조체의 이름, 구조체의 필드, 필드 유형을 얻기 위해 Rust 구조체를 파싱해야합니다.
 

### 구조체의 이름과 필드 파싱
 

`struct` 선언은 시작 부분에 가시성 키워드 (예 :`pub`)와`struct` 키워드,`struct`의 이름과`struct`의 본문이 뒤 따릅니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/Parsing-Struct-Name-Field-Diagram.png?resize=661%2C251&ssl=1)

```undefined
macro_rules! make_public{
    (
  // use vis type for visibility keyword and ident for struct name
     $vis:vis struct $struct_name:ident { }
    ) => {
        {
            pub struct $struct_name{ }
        }
    }
}
```

`$ vis`는 가시성을 가지며`$ struct_name`은 구조체 이름을 갖습니다.
 구조체를 공개하려면`pub` 키워드를 추가하고`$ vis` 변수를 무시하면됩니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/Make-Struct-Public-with-Keyword.png?resize=521%2C321&ssl=1)

`struct`는 데이터 유형과 가시성이 동일하거나 다른 여러 필드를 포함 할 수 있습니다.
 `ty`토큰 유형은 데이터 유형으로, `vis`는 가시성으로, `ident`는 필드 이름으로 사용됩니다.
 0 개 이상의 필드에`*`반복을 사용합니다.
 

```bash
 macro_rules! make_public{
    (
     $vis:vis struct $struct_name:ident {
        $(
 // vis for field visibility, ident for field name and ty for field data type
        $field_vis:vis $field_name:ident : $field_type:ty
        ),*
    }
    ) => {
        {
            pub struct $struct_name{
                $(
                pub $field_name : $field_type,
                )*
            }
        }
    }
}
```

### `struct`에서 메타 데이터 구문 분석
 

종종`struct`에는`# [derive (Debug)]`와 같은 일부 메타 데이터가 첨부되거나 절차 적 매크로가 있습니다.
 이 메타 데이터는 그대로 유지되어야합니다.
 이 메타 데이터 파싱은`meta` 유형을 사용하여 수행됩니다.
 

```php
macro_rules! make_public{
    (
     // meta data about struct
     $(#[$meta:meta])* 
     $vis:vis struct $struct_name:ident {
        $(
        // meta data about field
        $(#[$field_meta:meta])*
        $field_vis:vis $field_name:ident : $field_type:ty
        ),*$(,)+
    }
    ) => {
        { 
            $(#[$meta])*
            pub struct $struct_name{
                $(
                $(#[$field_meta:meta])*
                pub $field_name : $field_type,
                )*
            }
        }
    }
}
```

이제`make_public` 매크로가 준비되었습니다.
 `make_public`이 어떻게 작동하는지보기 위해 Rust Playground를 사용하여 매크로를 컴파일 된 실제 코드로 확장 해 보겠습니다.
 

```undefined
macro_rules! make_public{
    (
     $(#[$meta:meta])* 
     $vis:vis struct $struct_name:ident {
        $(
        $(#[$field_meta:meta])*
        $field_vis:vis $field_name:ident : $field_type:ty
        ),*$(,)+
    }
    ) => {

            $(#[$meta])*
            pub struct $struct_name{
                $(
                $(#[$field_meta:meta])*
                pub $field_name : $field_type,
                )*
            }
    }
}

fn main(){
    make_public!{
        #[derive(Debug)]
        struct Name{
            n:i64,
            t:i64,
            g:i64,
        }
    }
}
```

확장 된 코드는 다음과 같습니다.
 

```undefined
// some imports


macro_rules! make_public {
    ($ (#[$ meta : meta]) * $ vis : vis struct $ struct_name : ident
     {
         $
         ($ (#[$ field_meta : meta]) * $ field_vis : vis $ field_name : ident
          : $ field_type : ty), * $ (,) +
     }) =>
    {

            $ (#[$ meta]) * pub struct $ struct_name
            {
                $
                ($ (#[$ field_meta : meta]) * pub $ field_name : $
                 field_type,) *
            }
    }
}

fn main() {
        pub struct name {
            pub n: i64,
            pub t: i64,
            pub g: i64,
    }
}
```

### 선언적 매크로의 한계
 

선언적 매크로에는 몇 가지 제한 사항이 있습니다.
 일부는 Rust 매크로 자체와 관련이 있고 다른 일부는 선언적 매크로에 더 구체적입니다.
 

- 매크로 자동 완성 및 확장에 대한 지원 부족
 
- 선언적 매크로 디버깅이 어렵습니다.
 
- 제한된 수정 기능
 
- 더 큰 바이너리
 
- 더 긴 컴파일 시간 (선언 및 절차 매크로 모두에 적용됨)
 

## Rust의 절차 적 매크로
 

절차 적 매크로는보다 고급 버전의 매크로입니다.
 절차 적 매크로를 사용하면 기존 Rust 구문을 확장 할 수 있습니다.
 임의의 입력을 받아 유효한 Rust 코드를 반환합니다.
 

절차 적 매크로는 `TokenStream`을 입력으로 받아 다른 `Token Stream`을 반환하는 함수입니다.
 절차 적 매크로는 입력`TokenStream`을 조작하여 출력 스트림을 생성합니다.
 

절차 매크로에는 세 가지 유형이 있습니다.
 

- 속성과 유사한 매크로
 
- 매크로 파생
 
- 함수형 매크로
 

아래에서 각 절차 매크로 유형에 대해 자세히 살펴 보겠습니다.
 

### 속성과 유사한 매크로
 

속성과 유사한 매크로를 사용하면 항목에 자신을 연결하고 해당 항목을 조작 할 수있는 사용자 지정 속성을 만들 수 있습니다.
 인수를 사용할 수도 있습니다.
 

```undefined
#[some_attribute_macro(some_argument)]
fn perform_task(){
// some code
}
```

위 코드에서`some_attribute_macros`는 속성 매크로입니다.
 `perform_task`함수를 조작합니다.
 

속성과 같은 매크로를 작성하려면 먼저`cargo new macro-demo --lib`를 사용하여 프로젝트를 만듭니다.
 프로젝트가 준비되면`Cargo.toml`을 업데이트하여 프로젝트가 절차 적 매크로를 생성 할 것임을화물에 알립니다.
 

```undefined
# Cargo.toml
[lib]
proc-macro = true
```

이제 우리는 모두 절차 적 매크로에 도전 할 준비가되었습니다.
 

절차 적 매크로는 `TokenStream`을 입력으로 받아 다른 `TokenStream`을 반환하는 공개 함수입니다.
 절차 적 매크로를 작성하려면 `TokenStream`을 구문 분석하는 파서를 작성해야합니다.
 Rust 커뮤니티는`TokenStream`을 파싱하기위한 아주 좋은 상자`syn`을 가지고 있습니다.
 

`syn`은`TokenStream`을 구문 분석하는 데 사용할 수있는 Rust 구문에 대해 기성품 구문 분석기를 제공합니다.
 `syn`을 제공하는 저수준 파서를 결합하여 구문을 파싱 할 수도 있습니다.
 

`syn` 및`quote`를`Cargo.toml`에 추가합니다.
 

```undefined
# Cargo.toml
[dependencies]
syn = {version="1.0.57",features=["full","fold"]}
quote = "1.0.8"
```

이제 우리는 프로 시저 매크로를 작성하기 위해 컴파일러에서 제공하는`proc_macro` 크레이트를 사용하여`lib.rs`에 속성과 같은 매크로를 작성할 수 있습니다.
 절차 매크로 상자는 절차 매크로 외에는 내보낼 수 없으며 상자에 정의 된 절차 매크로는 상자 자체에서 사용할 수 없습니다.
 

```undefined
// lib.rs
extern crate proc_macro;
use proc_macro::{TokenStream};
use quote::{quote};

// using proc_macro_attribute to declare an attribute like procedural macro
#[proc_macro_attribute]
// _metadata is argument provided to macro call and _input is code to which attribute like macro attaches
pub fn my_custom_attribute(_metadata: TokenStream, _input: TokenStream) -> TokenStream {
    // returing a simple TokenStream for Struct
    TokenStream::from(quote!{struct H{})
}
```

추가 한 매크로를 테스트하려면`tests`라는 폴더를 만들고 폴더에`attribute_macro.rs` 파일을 추가하여 감사 테스트를 만듭니다.
 이 파일에서 테스트를 위해 속성과 유사한 매크로를 사용할 수 있습니다.
 

```cpp
// tests/attribute_macro.rs

use macro_demo::*;

// macro converts struct S to struct H
#[my_custom_attribute]
struct S{}

#[test]
fn test_macro(){
// due to macro we have struct H in scope
    let demo=H{};
}
```

`cargo test`명령어를 사용하여 위 테스트를 실행합니다.
 

이제 절차 적 매크로의 기초를 이해 했으므로 고급`TokenStream` 조작 및 구문 분석에`syn`을 사용하겠습니다.
 

`syn`이 파싱 및 조작에 어떻게 사용되는지 알아보기 위해 `syn`GitHub 저장소에서 예를 들어 보겠습니다.
 이 예제는 값이 변경 될 때 변수를 추적하는 Rust 매크로를 만듭니다.
 

먼저 매크로가 첨부 된 코드를 조작하는 방법을 식별해야합니다.
 

```bash
#[trace_vars(a)]
fn do_something(){
  let a=9;
  a=6;
  a=0;
}
```

`trace_vars` 매크로는 추적해야하는 변수의 이름을 취하고 입력 변수 즉`a`의 값이 변경 될 때마다 print 문을 삽입합니다.
 입력 변수의 값을 추적합니다.
 

먼저 속성과 유사한 매크로가 첨부되는 코드를 구문 분석합니다.
 `syn`은 Rust 함수 구문을위한 내장 파서를 제공합니다.
 `ItemFn`은 함수를 구문 분석하고 구문이 유효하지 않은 경우 오류를 발생시킵니다.
 

```php
#[proc_macro_attribute]
pub fn trace_vars(_metadata: TokenStream, input: TokenStream) -> TokenStream {
// parsing rust function to easy to use struct
    let input_fn = parse_macro_input!(input as ItemFn);
    TokenStream::from(quote!{fn dummy(){})
}
```

이제 파싱 된 `입력`이 있으므로 `메타 데이터`로 이동하겠습니다.
 `metadata`의 경우 내장 파서가 작동하지 않으므로`syn`의`parse` 모듈을 사용하여 직접 작성해야합니다.
 

```php
#[trace_vars(a,c,b)] // we need to parse a "," seperated list of tokens
// code
```

`syn`이 작동하려면`syn`이 제공하는`Parse` 특성을 구현해야합니다.
 `Punctuated`는`,`로 구분 된`Indent`의`vector`를 만드는 데 사용됩니다.
 

```js
struct Args{
    vars:HashSet<Ident>
}

impl Parse for Args{
    fn parse(input: ParseStream) -> Result<Self> {
        // parses a,b,c, or a,b,c where a,b and c are Indent
        let vars = Punctuated::<Ident, Token![,]>::parse_terminated(input)?;
        Ok(Args {
            vars: vars.into_iter().collect(),
        })
    }
}
```

`Parse` 트레이 트를 구현하면`metadata` 파싱을 위해`parse_macro_input` 매크로를 사용할 수 있습니다.
 

```php
#[proc_macro_attribute]
pub fn trace_vars(metadata: TokenStream, input: TokenStream) -> TokenStream {
    let input_fn = parse_macro_input!(input as ItemFn);
// using newly created struct Args
    let args= parse_macro_input!(metadata as Args);
    TokenStream::from(quote!{fn dummy(){})
}
```

이제 변수가 값을 변경할 때`println!`을 추가하도록`input_fn`을 수정합니다.
 이를 추가하려면 할당이있는 개요를 필터링하고 해당 줄 뒤에 print 문을 삽입해야합니다.
 

```php
impl Args {
    fn should_print_expr(&self, e: &Expr) -> bool {
        match *e {
            Expr::Path(ref e) => {
 // variable shouldn't start wiht ::
                if e.path.leading_colon.is_some() {
                    false
// should be a single variable like `x=8` not n::x=0 
                } else if e.path.segments.len() != 1 {
                    false
                } else {
// get the first part
                    let first = e.path.segments.first().unwrap();
// check if the variable name is in the Args.vars hashset
                    self.vars.contains(&first.ident) && first.arguments.is_empty()
                }
            }
            _ => false,
        }
    }

// used for checking if to print let i=0 etc or not
    fn should_print_pat(&self, p: &Pat) -> bool {
        match p {
// check if variable name is present in set
            Pat::Ident(ref p) => self.vars.contains(&p.ident),
            _ => false,
        }
    }

// manipulate tree to insert print statement
    fn assign_and_print(&mut self, left: Expr, op: &dyn ToTokens, right: Expr) -> Expr {
 // recurive call on right of the assigment statement
        let right = fold::fold_expr(self, right);
// returning manipulated sub-tree
        parse_quote!({
            #left #op #right;
            println!(concat!(stringify!(#left), " = {:?}"), #left);
        })
    }

// manipulating let statement
    fn let_and_print(&mut self, local: Local) -> Stmt {
        let Local { pat, init, .. } = local;
        let init = self.fold_expr(*init.unwrap().1);
// get the variable name of assigned variable
        let ident = match pat {
            Pat::Ident(ref p) => &p.ident,
            _ => unreachable!(),
        };
// new sub tree
        parse_quote! {
            let #pat = {
                #[allow(unused_mut)]
                let #pat = #init;
                println!(concat!(stringify!(#ident), " = {:?}"), #ident);
                #ident
            };
        }
    }
}
```

위의 예에서 `quote`매크로는 Rust를 템플릿 화하고 작성하는 데 사용됩니다.
 `#`은 변수 값을 삽입하는 데 사용됩니다.
 

이제`input_fn`을 통해 DFS를 수행하고 print 문을 삽입합니다.
 `syn`은 모든`Item`에 대해 DFS 용으로 구현할 수있는`Fold` 특성을 제공합니다.
 조작하려는 토큰 유형에 해당하는 특성 메서드를 수정하기 만하면됩니다.
 

```php
impl Fold for Args {
    fn fold_expr(&mut self, e: Expr) -> Expr {
        match e {
// for changing assignment like a=5
            Expr::Assign(e) => {
// check should print
                if self.should_print_expr(&e.left) {
                    self.assign_and_print(*e.left, &e.eq_token, *e.right)
                } else {
// continue with default travesal using default methods
                    Expr::Assign(fold::fold_expr_assign(self, e))
                }
            }
// for changing assigment and operation like a+=1
            Expr::AssignOp(e) => {
// check should print
                if self.should_print_expr(&e.left) {
                    self.assign_and_print(*e.left, &e.op, *e.right)
                } else {
// continue with default behaviour
                    Expr::AssignOp(fold::fold_expr_assign_op(self, e))
                }
            }
// continue with default behaviour for rest of expressions
            _ => fold::fold_expr(self, e),
        }
    }

// for let statements like let d=9
    fn fold_stmt(&mut self, s: Stmt) -> Stmt {
        match s {
            Stmt::Local(s) => {
                if s.init.is_some() && self.should_print_pat(&s.pat) {
                    self.let_and_print(s)
                } else {
                    Stmt::Local(fold::fold_local(self, s))
                }
            }
            _ => fold::fold_stmt(self, s),
        }
    }
}
```

`Fold`특성은 `Item`의 DFS를 수행하는 데 사용됩니다.
 다양한 토큰 유형에 대해 다른 동작을 사용할 수 있습니다.
 

이제`fold_item_fn`을 사용하여 구문 분석 된 코드에 인쇄 문을 삽입 할 수 있습니다.
 

```php
#[proc_macro_attribute]
pub fn trace_var(args: TokenStream, input: TokenStream) -> TokenStream {
// parse the input
    let input = parse_macro_input!(input as ItemFn);
// parse the arguments
    let mut args = parse_macro_input!(args as Args);
// create the ouput
    let output = args.fold_item_fn(input);
// return the TokenStream
    TokenStream::from(quote!(#output))
}
```

이 코드 예제는 절차 매크로에 대해 배울 수있는 훌륭한 리소스 인`syn` 예제 저장소에서 가져온 것입니다.
 

### 사용자 지정 파생 매크로
 

Rust의 커스텀 파생 매크로는 자동 구현 특성을 허용합니다.
 이 매크로를 사용하면`# [derive (Trait)]`를 사용하여 트레이 트를 구현할 수 있습니다.
 

`syn`은`derive` 매크로를 훌륭하게 지원합니다.
 

```cpp
#[derive(Trait)]
struct MyStruct{}
```

Rust에서 커스텀 파생 매크로를 작성하려면 입력을 구문 분석하여 매크로를 파생하는 데`DeriveInput`을 사용할 수 있습니다.
 또한`proc_macro_derive` 매크로를 사용하여 사용자 정의 파생 매크로를 정의합니다.
 

```php
#[proc_macro_derive(Trait)]
pub fn derive_trait(input: proc_macro::TokenStream) -> proc_macro::TokenStream {
    let input = parse_macro_input!(input as DeriveInput);

    let name = input.ident;

    let expanded = quote! {
        impl Trait for #name {
            fn print(&self) -> usize {
                println!("{}","hello from #name")
           }
        }
    };

    proc_macro::TokenStream::from(expanded)
}
```

더 진보 된 절차 적 매크로는`syn`을 사용하여 작성할 수 있습니다.
 `syn`의 저장소에서이 예를 확인하세요.
 

### 함수형 매크로
 

함수형 매크로는 매크로 호출 연산자`! `로 호출되고 함수 호출처럼 보인다는 점에서 선언적 매크로와 유사합니다.
 괄호 안에있는 코드에서 작동합니다.
 

Rust에서 함수와 유사한 매크로를 작성하는 방법은 다음과 같습니다.
 

```css
#[proc_macro]
pub fn a_proc_macro(_input: TokenStream) -> TokenStream {
    TokenStream::from(quote!(
            fn anwser()->i32{
                5
            }
))
}
```

함수형 매크로는 런타임이 아니라 컴파일 타임에 실행됩니다.
 Rust 코드의 어느 곳에서나 사용할 수 있습니다.
 함수형 매크로는 또한`TokenStream`을 취하고`TokenStream`을 반환합니다.
 

절차 적 매크로 사용의 장점은 다음과 같습니다.
 

- `span`을 사용한 더 나은 오류 처리
 
- 출력에 대한 더 나은 제어
 
- 커뮤니티 구축 상자`syn` 및`quote`
 
- 선언적 매크로보다 더 강력 함
 

## 결론
 

이 Rust 매크로 튜토리얼에서 우리는 Rust에서 매크로의 기초를 다루고 선언적 매크로와 절차 적 매크로를 정의했으며 다양한 구문과 커뮤니티에서 만든 상자를 사용하여 두 유형의 매크로를 작성하는 방법을 살펴 보았습니다.
 또한 각 유형의 Rust 매크로를 사용할 때의 이점도 설명했습니다.
 