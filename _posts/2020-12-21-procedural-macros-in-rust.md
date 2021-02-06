---
layout: post
title: "Rust의 절차 매크로"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/rust-procedural-macros.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/rust-procedural-macros.png?fit=730%2C487&ssl=1)

Rust를 사용한 지 적어도 얼마 안 되었다면, 이를 실현했는지 여부에 관계없이 절차적 매크로, 즉 proc 매크로를 접하게 될 것입니다. 가장 인기 있는 프로 매크로들은 Serde가 파생한 `serialize`와 `deserialize` 매크로일 것이다. 로켓 웹 프레임워크, 와스름-바인젠 프로젝트 또는 다른 수많은 라이브러리에서 작업한 경우 이러한 매크로를 우연히 발견했을 수도 있다.

이 튜토리얼에서는 Rust의 절차 매크로에 대해 알아야 할 모든 사항을 설명합니다. 다음 내용을 다루겠습니다.

- Rust의 절차적 매크로란?
- 선언적 매크로 및 절차적 매크로
- 함수형 및 속성 매크로 도출
- Rust에서 파생 매크로 생성
- Rust에서 절차 매크로 작성

## Rust의 절차적 매크로란?

간단히 말해 절차 매크로를 사용하면 Rust 코드의 일부를 가져와서 분석하고 Rust 코드를 더 생성할 수 있습니다. 이건 메타프로그래밍의 한 형태에요. 러스트를 이용해서 러스트 코드를 쓰는 방식이죠.

스타워즈 에피소드 2: 복제인간들의 공격, 드로이드 C-3PO가 드로이드 제조 공장에서 비틀거리며 "기계들이 기계를 만드는가? 얼마나 비뚤어졌는가."

개인적으로, 저는 "멋지다"와 함께 갔겠지만, 그건 취향의 문제일 수도 있어요.

어쨌든 proc 매크로를 사용하면 컴파일 시 Rust 코드를 생성할 수 있습니다. 꽤 발전된 주제이지만, 러스트 언어의 매우 강력한 특징입니다.

### 선언적 매크로 및 절차적 매크로

Rust에는 두 가지 종류의 매크로가 있습니다. 선언형 매크로(`macro_rules`로 선언됨!`) 및 절차 매크로. 두 종류의 매크로 모두 더 많은 코드를 작성하고 컴파일할 때 평가되는 코드를 작성할 수 있지만 매우 다른 방식으로 작동합니다.

선언적 매크로(구어적으로도 알려져 있고 다소 헷갈리기도 함)는 인수로 제공하는 Rust 코드에서 작동하는 "일치" 식과 유사한 것을 쓸 수 있게 한다. 사용자가 제공한 코드를 사용하여 매크로 호출을 대체하는 코드를 생성합니다. 어떤 의미에서 선언적 매크로란 정교한 텍스트 대체에 불과하다.

반면에 절차 매크로를 사용하면 제공된 Rust 코드의 추상 구문 트리(AST)에서 작업할 수 있습니다. 함수 서명에 관한 한, proc macro는 `TokenStream`(또는 두 개)에서 다른 `TokenStream`으로의 함수이며, 여기서 출력은 매크로 호출을 대신한다. 이를 통해 컴파일 시 Rust 코드를 검사할 수 있으며 프로그램의 AST를 기반으로 정교한 코드를 생성할 수 있습니다.

간단히 말해, 선언적 매크로를 사용하면 코드 패턴과 일치시키고 패턴을 기반으로 코드를 생성할 수 있습니다. 절차 매크로를 사용하면 출력을 생성하기 전에 제공된 코드를 검사하고 작동할 수 있으므로 훨씬 더 많은 전력을 제공할 수 있습니다.

### 함수형 및 속성 매크로 도출

절차 매크로에는 파생 매크로, 함수형 매크로, 속성 매크로의 세 가지 종류가 있습니다. 이들은 모두 토큰스트림으로 운영되지만 조금씩 다른 방식으로 운영된다.

- 파생 매크로에는 구조, 열거형 및 유니언에서 작동하며 "#[파생(MyMacro)]" 선언으로 주석이 달려 있습니다. 또한 항목 멤버(예: 열거형 또는 구조 필드)에 연결할 수 있는 도우미 특성을 선언할 수 있습니다.
- 함수형 매크로는 매크로 호출 연산자 `!`로 호출되고 함수 호출처럼 보인다는 점에서 선언형 매크로와 유사하다. 괄호 안에 넣는 코드로 동작합니다.
- 속성 매크로에서는 항목에 연결할 수 있는 새 외부 속성을 정의합니다. 매크로 도출과 유사하지만 특성 정의 및 기능과 같은 더 많은 항목에 연결할 수 있습니다.

## Rust에서 파생 매크로 생성

우리 손을 좀 더럽게 하자. 이제 프로크 매크로가 무엇인지 대략적으로 파악했으므로 간단한 매크로를 만들어 보겠습니다. 우리는 구조, 열거형 또는 조합에 대한 정보를 출력하는 파생 매크로를 만들 것입니다. 여기에는 구조, 열거형 또는 조합에 대한 정보가 포함되며, 어떤 종류의 항목이 있는지, 어떤 멤버나 변형이 있는지 등이 포함됩니다.

절차적 매크로 상자를 작성할 때 가장 먼저 해야 할 일은 Cargo.toml 파일에 이 사실을 Cargo에게 알리는 것입니다. 다음과 같은 항목을 추가하여 이 작업을 수행합니다.

```toml
[lib]
proc-macro = true
```

우리의 경우 매크로를 테스트하기 위해 Cargo의 워크스페이스 기능을 사용하여 메인 실행 파일(main.rs)과 proc 매크로(lib.rs)를 보관하는 라이브러리(lib.rs)를 만들 것이다. 전체 디렉토리 구조는 다음과 같습니다.

```css
.
├── Cargo.toml
├── derive-macro
│   ├── Cargo.toml
│   └── src
│       └── lib.rs
└── main.rs
>
```

최상위 수준 `Cargo.toml` 파일에는 다음과 같은 바이너리 및 종속성에 대한 정보가 들어 있습니다.

```toml
[package]
name = "proc_macros"
version = "0.1.0"
edition = "2018"
publish = false

[workspace]

[[bin]]
name = "proc-macros"
path = "main.rs"

[dependencies]
derive-macro = { path = "derive-macro" }
```

deriv-macro/Cargo.toml은 proc 매크로 프로젝트에 대한 의존성과 앞서 언급한 "deriv-macro = true" 주석 내용을 포함하고 있다.

```toml
[package]
name = "derive-macro"
version = "0.1.0"
edition = "2018"

[lib]
proc-macro = true

[dependencies]
syn = { version = "1.0", features = ["full"] }
quote = "1.0"
proc-macro2 = "1.0"
```

## Rust에서 절차 매크로 작성

작성 과정 매크로에 대한 철저한 소개는 이 기사의 범위를 벗어난다. 대신 이 튜토리얼의 끝에 언급된 리소스를 참조하십시오.

하지만 제가 할 일은 싱, 따옴표, proc_macro2 상자를 사용한 간단한 예를 보여드리는 것입니다. 이것은 프로크 매크로를 사용할 때 사용하는 빵과 버터입니다.

언급한 바와 같이, 우리는 매크로가 첨부된 아이템을 설명하기를 원합니다. 이 문제가 어떻게 처리되는지 `src/src/lib.rs`에서 살펴보겠습니다.

```undefined
use proc_macro::{self, TokenStream};
use quote::quote;
use syn::{parse_macro_input, DataEnum, DataUnion, DeriveInput, FieldsNamed, FieldsUnnamed};

#[proc_macro_derive(Describe)]
pub fn describe(input: TokenStream) -> TokenStream {
    let DeriveInput { ident, data, .. } = parse_macro_input!(input);

    let description = match data {
    syn::Data::Struct(s) => match s.fields {
        syn::Fields::Named(FieldsNamed { named, .. }) => {
        let idents = named.iter().map(|f| &f.ident);
        format!(
            "a struct with these named fields: {}",
            quote! {#(#idents), *}
        )
        }
        syn::Fields::Unnamed(FieldsUnnamed { unnamed, .. }) => {
        let num_fields = unnamed.iter().count();
        format!("a struct with {} unnamed fields", num_fields)
        }
        syn::Fields::Unit => format!("a unit struct"),
    },
    syn::Data::Enum(DataEnum { variants, .. }) => {
        let vs = variants.iter().map(|v| &v.ident);
        format!("an enum with these variants: {}", quote! {#(#vs),*})
    }
    syn::Data::Union(DataUnion {
        fields: FieldsNamed { named, .. },
        ..
    }) => {
        let idents = named.iter().map(|f| &f.ident);
        format!("a union with these named fields: {}", quote! {#(#idents),*})
    }
    };

    let output = quote! {
    impl #ident {
        fn describe() {
        println!("{} is {}.", stringify!(#ident), #description);
        }
    }
    };

    output.into()
}
```

첫 번째 단계는 우리가 받는 토큰 스트림을 논쟁으로 삼는 것이다. 우리는 `syn`의 `parse_macro_input` 매크로를 사용하여 구문 분석한다. 이를 통해 우리는 `ident`(항목의 식별자, 이름, 이름) 및 항목에 포함된 데이터에 액세스할 수 있습니다. 이 데이터와 비교하여 항목이 구조체인지, 열거형인지 또는 유니언인지 확인합니다. 구조체일 경우 필드가 있는지 여부와 이름이 지정되었는지 확인하기 위해 추가 일치를 수행합니다. 항목의 종류에 따라 해당 항목의 필드 또는 변형을 검사하고 이를 설명하는 문자열을 만듭니다.

"syn" 상자는 들어오는 "토큰 스트림"을 구문 분석하여 작업할 수 있는 힘을 주지만, "따옴표" 상자는 우리가 새로운 Rust 코드를 생성할 수 있게 해준다. 인용 매크로를 사용하면 거의 정규 Rust 코드를 작성할 수 있으며, 이 코드를 실제 Rust 코드로 변환할 수 있습니다. 이상한 구문(예: 따옴표!) {#(#idents), *}`)은 가변 보간 및 반복을 수행하는 방법입니다.

이 매크로를 사용하여 `main.rs` 파일에 저장해 보겠습니다.

```undefined
use derive_macro::Describe;

#[derive(Describe)]
struct MyStruct {
    my_string: String,
    my_enum: MyEnum,
    my_number: f64,
}

#[derive(Describe)]
struct MyTupleStruct(u32, String, i8);

#[derive(Describe)]
enum MyEnum {
    VariantA,
    VariantB,
}

#[derive(Describe)]
union MyUnion {
    unsigned: u32,
    signed: i32,
}

fn main() {
    MyStruct::describe();
    MyTupleStruct::describe();
    MyEnum::describe();
    MyUnion::describe();
}
```

프로그램을 실행하면 이와 같은 출력이 생성됩니다.

```python
MyStruct is a struct with these named fields: my_string, my_enum, my_number.
MyTupleStruct is a struct with 3 unnamed fields.
MyEnum is an enum with these variants: VariantA, VariantB.
MyUnion is a union with these named fields: unsigned, signed.
```

## 추가 읽기

절차적 매크로는 Rust 언어의 매우 강력한 특징이며 놀라운 잠재력을 가지고 있다. 하지만 그들은 또한 매우 복잡하고 진정으로 이해하고 편안하게 되기 위한 시간과 의도적인 노력을 필요로 한다.

이 가이드에서는 이러한 기능이 무엇이며 어떻게 시작할 수 있는지에 대해 개괄적으로 설명합니다. 토끼굴로 더 깊이 들어가려면 다음 리소스를 사용하는 것이 좋습니다.

- Rust 2018의 Proc 매크로에 대한 알렉스 크라이튼의 블로그에 올린 글은 절차적 매크로에 대한 훌륭한 소개로, `TokenStream` 타입과 함께 작업하며, 야생에서 proc 매크로를 찾을 수 있다.
- 항상 그렇듯이 Rust Book에는 매크로에 대한 섹션이 있으며, Rust Reference의 Proc 매크로에 대한 Rust Reference 장은 단지 확실한 사실만을 원한다면 훌륭한 옵션이다.
- Zach Mitchell은 proc 매크로에 대한 보다 길고 철저한 소개를 썼는데, 매크로를 만들어 불법 필드 구조를 검사하고, 매크로가 발견되면 컴파일러가 정신을 잃게 만들어야 한다.
- 실제 경험을 쌓고 직접 Proc 매크로를 사용하고 싶다면 다양한 프로젝트(테스트 완료)가 있는 David Tolnay의 "Rust Latam: Procedical 매크로 워크샵"을 적극 추천합니다.