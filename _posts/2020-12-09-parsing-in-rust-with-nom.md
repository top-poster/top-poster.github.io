---
layout: post
title: "nom을 사용한 Rust에서 구문 분석"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/12/parsing-rust-nom.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/parsing-rust-nom.png?fit=730%2C487&ssl=1)

이 튜토리얼에서는 `nom` 파서 콤비네이터 라이브러리를 사용하여 Rust에서 매우 기본적인 URL 파서를 작성하는 방법에 대해 설명합니다. 다음 사항에 대해 자세히 설명합니다.

- 파서 콤비네이터란?
nom은 어떻게 작동합니까?
nom 설정
데이터 유형
`nom`에서 오류 처리
Rust에서 구문 분석기 쓰기
권한으로 URL 구문 분석
녹 분석: 호스트, IP 및 포트
Rust에서 `경로` 구문 분석
쿼리 및 조각
`nom`을 사용한 Rust의 구문 분석: 마지막 시험
- 파서 콤비네이터란?
- nom은 어떻게 작동합니까?
- nom 설정
- 데이터 유형
- `nom`에서 오류 처리
- Rust에서 구문 분석기 쓰기
- 권한으로 URL 구문 분석
- 녹 분석: 호스트, IP 및 포트
- Rust에서 `경로` 구문 분석
- 쿼리 및 조각
- `nom`을 사용한 Rust의 구문 분석: 마지막 시험

## 파서 콤비네이터란?

파서 콤비네이터는 여러 파서를 입력으로 받아들이고 새 파서를 출력으로 반환할 수 있는 고차 함수이다.

이 접근 방식을 사용하면 특정 문자열이나 숫자를 구문 분석하는 것과 같은 간단한 작업에 대한 파서를 작성하고 결합기 함수를 사용하여 전체 재귀 하강 파서로 합성할 수 있습니다.

조합 파싱의 이점은 테스트 가능성, 유지보수성 및 가독성을 포함한다. 각 이동 부품은 다소 작고 자체 격리되어 전체 파서가 모듈 구성 요소로 구성된다.

이 컨셉을 처음 접하셨다면 Rust의 파서 콤비네이터에 대한 Bodil Stokke의 훌륭한 튜토리얼을 읽어보시길 강력히 추천합니다.

## nom은 어떻게 작동합니까?

nom은 Rust로 작성된 파서 콤비네이터 라이브러리로, 메모리를 보유하거나 성능을 저하시키지 않고도 안전한 파서를 만들 수 있습니다. Rust의 강력한 타이핑과 메모리 안전성에 의존하여 정확하고 성능이 뛰어난 파서를 만들 뿐만 아니라 오류 발생 가능성이 높은 배관을 추상화하기 위한 기능, 매크로 및 특성도 제공한다.

nom`의 작동 방식을 보여주기 위해 기본 URL 구문 분석기를 만듭니다. 전체 URL 사양을 구현하지 않습니다. 이 코드 예제의 범위를 훨씬 초과하는 것입니다. 대신, 몇 가지 지름길로 가겠습니다.

최종 목표는 `https://www.zupzup.org/about/?someVal=5와 같은 유효한 URL을 구문 분석할 수 있는 것입니다.

테스트 가능성이 파서 결합기의 이점 중 하나로 권장되기 때문에, 우리는 대부분의 구성 요소에 대한 테스트를 작성하여 이러한 이점을 실제로 확인할 것입니다.

시작해 봅시다!

## nom 설정

따라서 최신 Rust 설치(1.44+)만 있으면 됩니다.

먼저 새 Rust 프로젝트를 만듭니다.

```bash
cargo new --lib rust-nom-example
cd rust-nom-example
```

다음으로 `Cargo.toml` 파일을 편집하고 필요한 종속성을 추가합니다.

```undefined
[dependencies]
nom = "6.0"
```

네, 우리에게 필요한 것은 최신 버전(작성 당시 6.0)의 nom 라이브러리입니다.

## 데이터 유형

파서를 작성할 때, 어떤 부품이 필요하고 어떤 모양인지 느끼기 위해 출력 구조를 먼저 정의하는 것이 이치에 맞는 경우가 많습니다.

이 경우 URL을 구문 분석하므로 URL에 대한 구조를 정의해 보겠습니다.

```undefined
#[derive(Debug, PartialEq, Eq)]
pub struct URI<'a> {
    scheme: Scheme,
    authority: Option<Authority<'a>>,
    host: Host,
    port: Option<u16>,
    path: Option<Vec<&'a str>>,
    query: Option<QueryParams<'a>>,
    fragment: Option<&'a str>,
}

#[derive(Debug, PartialEq, Eq)]
pub enum Scheme {
    HTTP,
    HTTPS,
}

pub type Authority<'a> = (&'a str, Option<&'a str>);

#[derive(Debug, PartialEq, Eq)]
pub enum Host {
    HOST(String),
    IP([u8; 4]),
}

pub type QueryParam<'a> = (&'a str, &'a str);
pub type QueryParams<'a> = Vec<QueryParam<'a>>;
```

이 선으로 한 줄 건너자.

필드는 정규 URI에서 발생하는 순서에 따라 배열됩니다. 첫째, 우리는 계획을 가지고 있다. 이 경우 http://로 제한하고 https://htp로 제한하지만 다른 많은 스키마를 사용할 수 있습니다.

다음은 사용자 이름과 선택적 암호로 구성되며 일반적으로 완전히 선택적인 권한 부분이 나옵니다.

호스트는 IP(우리의 경우 IPv4만 해당) 또는 example.org와 같은 호스트 문자열이 될 수 있으며, 로컬 호스트:8080과 같은 선택적 포트(port)가 뒤따른다.

포트 뒤에 /some/important/path와 같이 `/`로 구분된 문자열의 시퀀스인 경로가 있습니다. 이 옵션은 `?query=some-value`를 나타내는 쿼리 및 조각 부품과 마찬가지로 선택 사항입니다.

만약 여러분이 이러한 유형에서 사용되는 수명 (`a`)에 대해 혼란스럽다면, 그것들에 대해 너무 걱정하지 마세요; 그것들은 우리가 코드를 작성하는 방법에 별로 영향을 미치지 않을 것입니다. 기본적으로, URL의 각 부분에 새 문자열을 할당하는 대신, 입력 문자열의 일부에 대한 포인터를 사용할 수 있으며, URI 구조만 유효하다면 괜찮습니다.

구문 분석을 시작하기 전에 유효한 스키마를 `Scheme` 열거형으로 변환하기 위한 도우미를 정의하겠습니다.

```php
impl From<&str> for Scheme {
    fn from(i: &str) -> Self {
        match i.to_lowercase().as_str() {
            "http://" => Scheme::HTTP,
            "https://" => Scheme::HTTPS,
            _ => unimplemented!("no other schemes supported"),
        }
    }
}
```

그런 식으로 하면, 위에서부터 그것을 취해서 `계획`을 분석해보자.

## 'nom'에서 오류 처리

시작하기 전에 nom에서 오류 처리에 대해 이야기해 보겠습니다. 여기까지 오지는 않겠지만, 우리는 적어도 호출자에게 어떤 파싱 단계에서 무엇이 잘못되었는지에 대한 일반적인 정보를 주려고 할 것입니다.

우리의 목적을 위해 nom의 context combinator를 사용할 것이다. nom에서 파서는 일반적으로 다음 유형을 반환합니다.

```undefined
type IResult<I, O, E = (I, ErrorKind)> = Result<(I, O), Err<E>>;
```

우리의 경우 입력 값의 두 배를 반환하는 결과 유형이 있습니다(`).

표준 `IRESULT`에서는 Nom의 기본 제공 오류 유형만 사용할 수 있지만 사용자 정의 오류를 만들거나 이러한 오류에 컨텍스트를 추가하려면 어떻게 해야 합니까?

ParseError 특성 및 VerboseError 유형을 통해 자체 오류를 작성하고 기존 오류에 컨텍스트를 추가할 수 있습니다. 이 간단한 예제의 경우 구문 분석 오류에 컨텍스트를 추가합니다. 이를 편리하게 하기 위해 자체 결과 유형을 정의해 보겠습니다.

```undefined
type Res<T, U> = IResult<T, U, VerboseError<T>>;
```

`버보스 에러`가 걸린다는 점만 빼면 기본적으로 똑같다. 이것은 우리가 임의의 구문 분석기에 오류 컨텍스트를 암시적으로 추가할 수 있는 Nom의 컨텍스트 결합기를 사용할 수 있다는 것을 의미한다.

nom의 공식 문서에는 이러한 옵션이 포함되어 있지만 오류 처리가 가장 직관적인 것은 아닙니다.

그것을 실제로 보기 위해, 계획에 대한 우리의 첫 번째 파서를 만들어 봅시다.

## Rust에서 구문 분석기 쓰기

URL 스키마를 구문 분석하기 위해 `http://phit 또는 `https://phitmother nothing`에서 일치시키기를 원한다. 그리고 우리는 강력한 파서 콤비네이터 라이브러리를 사용하고 있기 때문에, 우리는 그런 낮은 수준의 파서를 손으로 쓸 필요가 없습니다. "nom"은 우리를 커버했다.

이 매크로 구문 분석기 및 조합기 목록은 특정 사용 사례에 사용할 내용에 대한 개요를 제공합니다.

기본적으로 `tag_no_case` 구문 분석기와 `alt` 조합기를 사용하여 "소문자(입력)는 http:// 또는 https://여야 합니다."라고 말합니다. 이 튜토리얼에서는 일반 함수만 사용하되, `nom`의 많은 구문 분석기와 결합기는 매크로로도 사용할 수 있습니다.

Rust에서 nom을 사용하는 방법은 다음과 같습니다.

```undefined
fn scheme(input: &str) -> Res<&str, Scheme> {
    context(
        "scheme",
        alt((tag_no_case("HTTP://"), tag_no_case("HTTPS://"))),
    )(input)
    .map(|(next_input, res)| (next_input, res.into()))
}
```

보시다시피 컨텍스트 결합기를 사용하여 실제 구문 분석기를 포장하고 체계 컨텍스트를 추가하므로 여기서 트리거된 모든 오류는 결과에서 "구성표"로 표시됩니다.

파서 및 콤비네이터를 사용하여 전체 파서를 조립하면, 유일한 입력 매개 변수인 `입력` 문자열로 호출됩니다. 그런 다음 위에서 언급한 대로 나머지 입력과 구문 분석 출력으로 구성된 결과를 앞에서 언급한 .to() 특성 구현으로 구문 분석된 체계를 `Scheme` 열거형으로 변환하여 다른 형태로 `맵핑`한다.

몇 가지 테스트를 작성해서 효과가 있는지 확인해 보겠습니다.

```php
#[cfg(test)]
mod tests {
    use super::*;
    use nom::{
        error::{ErrorKind, VerboseError, VerboseErrorKind},
        Err as NomErr,
    };

    #[test]
    fn test_scheme() {
        assert_eq!(scheme("https://yay"), Ok(("yay", Scheme::HTTPS)));
        assert_eq!(scheme("http://yay"), Ok(("yay", Scheme::HTTP)));
        assert_eq!(
            scheme("bla://yay"),
            Err(NomErr::Error(VerboseError {
                errors: vec![
                    ("bla://yay", VerboseErrorKind::Nom(ErrorKind::Tag)),
                    ("bla://yay", VerboseErrorKind::Nom(ErrorKind::Alt)),
                    ("bla://yay", VerboseErrorKind::Context("scheme")),
                ]
            }))
        );
    }
}
```

보시다시피 성공 사례에서 우리는 구문 분석된 "Scheme" 열거형과 구문 분석될 나머지 문자열("yay")을 돌려받습니다. 또한 오류가 발생하면 트리거된 여러 오류 목록과 정의한 컨텍스트(`구성표`)가 표시됩니다.

이 경우 태그 호출이 모두 실패했고, 그 때문에 alt 결합기도 단일 값을 생성할 수 없어 실패했다.

그렇게 어렵지 않았어요. 한편, 우리는 기본적으로 일정한 줄을 구문 분석했습니다. 권한 부분을 구문 분석해서 좀 더 발전된 것을 시도해보자.

## 권한으로 URL 구문 분석

이전부터의 URI 구조, 특히 권한 부분을 기억한다면 완전히 선택적인 구조를 찾고 있음을 알 수 있습니다. 사용자 이름과 선택적 암호가 있어야 합니다.

이것이 우리가 사용한 형식 별칭입니다.

```bash
pub type Authority<'a> = (&'a str, Option<&'a str>);
```

어떻게 이런 일을 할 수 있지? URL에서는 다음과 같이 표시됩니다.

https://https:password@example.org

:password는 선택 사항이지만 어쨌든 마지막에 @가 있기 때문에 종료된 파서를 사용하는 것부터 시작할 수 있다. 이것은 우리에게 다른 문자열로 끝나는 문자열을 제공한다.

권한 내에서 ":"를 구분 기호로 봅니다. 이 편리한 문서에 따르면, 우리는 특정한 문자열로 분리된 두 개의 값을 제공하는 `분리_쌍` 결합기를 사용할 수 있을 것으로 보인다. 하지만 실제 문자는 어떻게 다루어야 할까요? 몇 가지 옵션이 있는데, 그 중 하나는 영숫자1 파서를 사용하는 것이다. 이렇게 하면 하나 이상의 문자가 있는 영숫자 문자열이 생성됩니다.

URL의 여러 부분에서 사용할 수 있는 문자는 쉽게 확인할 수 있습니다. 그것은 파서를 쓰고 구조화하는 것과는 관련이 없고 단지 모든 것을 더 길고 더 불편하게 만들 것이다. 따라서 URL의 대부분은 영숫자, 하이픈 및 점으로 구성될 수 있습니다. 이는 URL 표준에 따라 당연히 잘못된 것입니다.

모인 `권한` 파서를 보자.

```undefined
fn authority(input: &str) -> Res<&str, (&str, Option<&str>)> {
    context(
        "authority",
        terminated(
            separated_pair(alphanumeric1, opt(tag(":")), opt(alphanumeric1)),
            tag("@"),
        ),
    )(input)
}
```

몇 가지 테스트를 실행하여 이 기능이 작동하는지 살펴보겠습니다.

```php
    #[test]
    fn test_authority() {
        assert_eq!(
            authority("username:password@zupzup.org"),
            Ok(("zupzup.org", ("username", Some("password"))))
        );
        assert_eq!(
            authority("username@zupzup.org"),
            Ok(("zupzup.org", ("username", None)))
        );
        assert_eq!(
            authority("zupzup.org"),
            Err(NomErr::Error(VerboseError {
                errors: vec![
                    (".org", VerboseErrorKind::Nom(ErrorKind::Tag)),
                    ("zupzup.org", VerboseErrorKind::Context("authority")),
                ]
            }))
        );
        assert_eq!(
            authority(":zupzup.org"),
            Err(NomErr::Error(VerboseError {
                errors: vec![
                    (
                        ":zupzup.org",
                        VerboseErrorKind::Nom(ErrorKind::AlphaNumeric)
                    ),
                    (":zupzup.org", VerboseErrorKind::Context("authority")),
                ]
            }))
        );
        assert_eq!(
            authority("username:passwordzupzup.org"),
            Err(NomErr::Error(VerboseError {
                errors: vec![
                    (".org", VerboseErrorKind::Nom(ErrorKind::Tag)),
                    (
                        "username:passwordzupzup.org",
                        VerboseErrorKind::Context("authority")
                    ),
                ]
            }))
        );
        assert_eq!(
            authority("@zupzup.org"),
            Err(NomErr::Error(VerboseError {
                errors: vec![
                    (
                        "@zupzup.org",
                        VerboseErrorKind::Nom(ErrorKind::AlphaNumeric)
                    ),
                    ("@zupzup.org", VerboseErrorKind::Context("authority")),
                ]
            }))
        )
    }
```

좋아 보인다! 두 가지 값 모두 존재, 암호 누락, @ 누락 및 기타 몇 가지 오류 사례가 있습니다.

호스트 부분으로 넘어갑시다.

## 녹 분석: 호스트, IP 및 포트

호스트 부분은 호스트 문자열 또는 IP로 구성될 수 있으므로 이 단계는 더 복잡합니다. 설상가상으로 마지막에 옵션인:포트도 가질 수 있다.

단순성을 유지하기 위해 IPv4 IP만 지원합니다. 호스트부터 시작하겠습니다. 구현에 대해 살펴보고 단계별로 살펴보겠습니다.

```php
fn host(input: &str) -> Res<&str, Host> {
    context(
        "host",
        alt((
            tuple((many1(terminated(alphanumerichyphen1, tag("."))), alpha1)),
            tuple((many_m_n(1, 1, alphanumerichyphen1), take(0 as usize))),
        )),
    )(input)
    .map(|(next_input, mut res)| {
        if !res.1.is_empty() {
            res.0.push(res.1);
        }
        (next_input, Host::HOST(res.0.join(".")))
    })
}
```

가장 먼저 눈에 띄는 것은 두 가지 선택(`alt(`alt`). 두 경우 모두 파수 사슬인 튜플(tuple)이 있는데, 모두 효과가 있어야 한다.

첫 번째 경우 하이픈을 포함한 하나 이상의 (`many1`) 영숫자 문자열이 `sigen`으로 종료되고 TLD(alpha1)로 종료되기를 원한다.

`alphanumerichyphen1` 구문 분석기는 다음과 같습니다.

```php
fn alphanumerichyphen1<T>(i: T) -> Res<T, T>
where
    T: InputTakeAtPosition,
    <T as InputTakeAtPosition>::Item: AsChar,
{
    i.split_at_position1_complete(
        |item| {
            let char_item = item.as_char();
            !(char_item == '-') && !char_item.is_alphanum()
        },
        ErrorKind::AlphaNumeric,
    )
}
```

이것은 다소 복잡하지만 기본적으로 nom의 영숫자1 파서를 복사한 버전일 뿐이며,-가 추가되었다. 이게 최선의 방법인지는 모르겠지만 효과가 있어요!

어쨌든 호스트에는 localhost와 같은 단일 문자열의 경우 두 번째 옵션이 있습니다.

왜 우리는 1과 1을 가진 쓸모없어 보이는 이 `many_m_n`으로 이것을 그렇게 복잡하게 표현하고 있는가? 여기서 문제는 `alt` 결합기 내에서 모든 옵션이 동일한 유형(이 경우 문자열 벡터 및 다른 문자열의 튜플)을 반환해야 한다는 것이다.

우리는 또한 이것을 `지도` 함수에서 볼 수 있는데, 여기서 튜플의 두 번째 부분(최상위 도메인)이 비어 있지 않으면 튜플의 첫 번째 부분에 추가합니다. 마지막에 HOST 열거형을 생성하여 문자열 부분을 `xenze`로 결합하고 원래 호스트 문자열을 생성한다.

몇 가지 테스트를 작성하겠습니다.

```php
    #[test]
    fn test_host() {
        assert_eq!(
            host("localhost:8080"),
            Ok((":8080", Host::HOST("localhost".to_string())))
        );
        assert_eq!(
            host("example.org:8080"),
            Ok((":8080", Host::HOST("example.org".to_string())))
        );
        assert_eq!(
            host("some-subsite.example.org:8080"),
            Ok((":8080", Host::HOST("some-subsite.example.org".to_string())))
        );
        assert_eq!(
            host("example.123"),
            Ok((".123", Host::HOST("example".to_string())))
        );
        assert_eq!(
            host("$$$.com"),
            Err(NomErr::Error(VerboseError {
                errors: vec![
                    ("$$$.com", VerboseErrorKind::Nom(ErrorKind::AlphaNumeric)),
                    ("$$$.com", VerboseErrorKind::Nom(ErrorKind::ManyMN)),
                    ("$$$.com", VerboseErrorKind::Nom(ErrorKind::Alt)),
                    ("$$$.com", VerboseErrorKind::Context("host")),
                ]
            }))
        );
        assert_eq!(
            host(".com"),
            Err(NomErr::Error(VerboseError {
                errors: vec![
                    (".com", VerboseErrorKind::Nom(ErrorKind::AlphaNumeric)),
                    (".com", VerboseErrorKind::Nom(ErrorKind::ManyMN)),
                    (".com", VerboseErrorKind::Nom(ErrorKind::Alt)),
                    (".com", VerboseErrorKind::Context("host")),
                ]
            }))
        );
    }
```

IP 케이스로 넘어갑시다. 먼저 IPv4 IP의 각 개별 부분을 구문 분석할 수 있어야 합니다(예: 127.0.0.1).

```undefined
fn ip_num(input: &str) -> Res<&str, u8> {
    context("ip number", n_to_m_digits(1, 3))(input).and_then(|(next_input, result)| {
        match result.parse::<u8>() {
            Ok(n) => Ok((next_input, n)),
            Err(_) => Err(NomErr::Error(VerboseError { errors: vec![] })),
        }
    })
}

fn n_to_m_digits<'a>(n: usize, m: usize) -> impl FnMut(&'a str) -> Res<&str, String> {
    move |input| {
        many_m_n(n, m, one_of("0123456789"))(input)
            .map(|(next_input, result)| (next_input, result.into_iter().collect()))
    }
}
```

각각의 개별 번호를 얻기 위해, 우리는 n_to_m_digits 파서를 사용하여 1에서 3자리의 연속된 숫자를 찾아 u8로 변환하려고 한다.

이를 통해 다음과 같은 `u8` 배열에 전체 IP를 구문 분석하는 방법을 살펴볼 수 있습니다.

```undefined
fn ip(input: &str) -> Res<&str, Host> {
    context(
        "ip",
        tuple((count(terminated(ip_num, tag(".")), 3), ip_num)),
    )(input)
    .map(|(next_input, res)| {
        let mut result: [u8; 4] = [0, 0, 0, 0];
        res.0
            .into_iter()
            .enumerate()
            .for_each(|(i, v)| result[i] = v);
        result[3] = res.1;
        (next_input, Host::IP(result))
    })
}
```

이 경우 정확히 세 개의 `ip_num`을 찾고, `yp_num`과 `ip_num`을 차례로 찾습니다. 그런 다음 매핑 함수에서 이러한 개별 결과를 `호스트::u8 배열이 있는 IP 열거형입니다.

다시 한 번 몇 가지 테스트 결과를 작성하겠습니다.

```php
    #[test]
    fn test_ipv4() {
        assert_eq!(
            ip("192.168.0.1:8080"),
            Ok((":8080", Host::IP([192, 168, 0, 1])))
        );
        assert_eq!(ip("0.0.0.0:8080"), Ok((":8080", Host::IP([0, 0, 0, 0]))));
        assert_eq!(
            ip("1924.168.0.1:8080"),
            Err(NomErr::Error(VerboseError {
                errors: vec![
                    ("4.168.0.1:8080", VerboseErrorKind::Nom(ErrorKind::Tag)),
                    ("1924.168.0.1:8080", VerboseErrorKind::Nom(ErrorKind::Count)),
                    ("1924.168.0.1:8080", VerboseErrorKind::Context("ip")),
                ]
            }))
        );
        assert_eq!(
            ip("192.168.0000.144:8080"),
            Err(NomErr::Error(VerboseError {
                errors: vec![
                    ("0.144:8080", VerboseErrorKind::Nom(ErrorKind::Tag)),
                    (
                        "192.168.0000.144:8080",
                        VerboseErrorKind::Nom(ErrorKind::Count)
                    ),
                    ("192.168.0000.144:8080", VerboseErrorKind::Context("ip")),
                ]
            }))
        );
        assert_eq!(
            ip("192.168.0.1444:8080"),
            Ok(("4:8080", Host::IP([192, 168, 0, 144])))
        );
        assert_eq!(
            ip("192.168.0:8080"),
            Err(NomErr::Error(VerboseError {
                errors: vec![
                    (":8080", VerboseErrorKind::Nom(ErrorKind::Tag)),
                    ("192.168.0:8080", VerboseErrorKind::Nom(ErrorKind::Count)),
                    ("192.168.0:8080", VerboseErrorKind::Context("ip")),
                ]
            }))
        );
        assert_eq!(
            ip("999.168.0.0:8080"),
            Err(NomErr::Error(VerboseError {
                errors: vec![
                    ("999.168.0.0:8080", VerboseErrorKind::Nom(ErrorKind::Count)),
                    ("999.168.0.0:8080", VerboseErrorKind::Context("ip")),
                ]
            }))
        );
    }
```

이 모든 것을 종합하자면, 우리는 IP와 호스트를 모두 시도하고 `호스트`를 반환하는 또 다른 파서가 필요하다.

```undefined
fn ip_or_host(input: &str) -> Res<&str, Host> {
    context("ip or host", alt((ip, host)))(input)
}
```

## Rust에서 '경로' 구문 분석

다음 단계는 `길`이다. 여기서는 경로 내의 문자열에 하이픈과 점이 있는 영숫자 문자열만 포함할 수 있다고 가정하고 다음 도우미를 사용하여 문자열을 구문 분석합니다.

```undefined
fn url_code_points<T>(i: T) -> Res<T, T>
where
    T: InputTakeAtPosition,
    <T as InputTakeAtPosition>::Item: AsChar,
{
    i.split_at_position1_complete(
        |item| {
            let char_item = item.as_char();
            !(char_item == '-') && !char_item.is_alphanum() && !(char_item == '.')
            // ... actual ascii code points and url encoding...: https://infra.spec.whatwg.org/#ascii-code-point
        },
        ErrorKind::AlphaNumeric,
    )
}
```

`path`를 구문 분석하려면 `/` 구분된 문자열을 문자열 벡터에 구문 분석해야 합니다.

```undefined
fn path(input: &str) -> Res<&str, Vec<&str>> {
    context(
        "path",
        tuple((
            tag("/"),
            many0(terminated(url_code_points, tag("/"))),
            opt(url_code_points),
        )),
    )(input)
    .map(|(next_input, res)| {
        let mut path: Vec<&str> = res.1.iter().map(|p| p.to_owned()).collect();
        if let Some(last) = res.2 {
            path.push(last);
        }
        (next_input, path)
    })
}
```

항상 `/`로 시작하겠습니다. 이 경로는 이미 유효하지만, 최종 선택 문자열(예: index.php)을 사용하여 0 또는 그 이상(많은 0)/`종료 문자열도 추가로 가질 수 있습니다.

매퍼에서, 우리는 튜플의 세 번째 부분(마지막 부분)이 거기 있는지 확인하고, 만약 그렇다면, 그것을 경로 벡터로 밀어낸다.

경로에 대한 몇 가지 테스트도 작성하겠습니다.

```undefined
    #[test]
    fn test_path() {
        assert_eq!(path("/a/b/c?d"), Ok(("?d", vec!["a", "b", "c"])));
        assert_eq!(path("/a/b/c/?d"), Ok(("?d", vec!["a", "b", "c"])));
        assert_eq!(path("/a/b-c-d/c/?d"), Ok(("?d", vec!["a", "b-c-d", "c"])));
        assert_eq!(path("/a/1234/c/?d"), Ok(("?d", vec!["a", "1234", "c"])));
        assert_eq!(
            path("/a/1234/c.txt?d"),
            Ok(("?d", vec!["a", "1234", "c.txt"]))
        );
    }
```

좋아보여! 우리는 경로의 다른 부분들과 나머지 끈들을 얻었고 모든 것은 유용한 벡터에 있습니다.

쿼리와 URL의 조각 부분을 구문 분석하여 강하게 마무리합시다.

## 쿼리 및 조각

이 경로는 기본적으로 키-값 쌍으로, 첫 번째 경로는 `?` 다음에 오고 나머지는 `?`로 구분된다.

```js
fn query_params(input: &str) -> Res<&str, QueryParams> {
    context(
        "query params",
        tuple((
            tag("?"),
            url_code_points,
            tag("="),
            url_code_points,
            many0(tuple((
                tag("&"),
                url_code_points,
                tag("="),
                url_code_points,
            ))),
        )),
    )(input)
    .map(|(next_input, res)| {
        let mut qps = Vec::new();
        qps.push((res.1, res.3));
        for qp in res.4 {
            qps.push((qp.1, qp.3));
        }
        (next_input, qps)
    })
}
```

파서가 직관적이고 읽기 쉽기 때문에 이것은 사실 꽤 좋다. `?=`와 `0` 이상으로 구분된 `??` 뒤에 나오는 첫 번째 키-값 쌍의 두 쌍을 분석합니다.

그런 다음 매퍼에서 모든 키-값 쌍을 벡터에 넣고 초기에 구조를 정의하면 됩니다.

```bash
pub type QueryParam<'a> = (&'a str, &'a str);
pub type QueryParams<'a> = Vec<QueryParam<'a>>;
```

다음은 몇 가지 기본 테스트입니다.

```undefined
    #[test]
    fn test_query_params() {
        assert_eq!(
            query_params("?bla=5&blub=val#yay"),
            Ok(("#yay", vec![("bla", "5"), ("blub", "val")]))
        );

        assert_eq!(
            query_params("?bla-blub=arr-arr#yay"),
            Ok(("#yay", vec![("bla-blub", "arr-arr"),]))
        );
    }
```

마지막 부분은 `#`에 불과한 조각이며, 그 뒤에 문자열이 조각은 다음과 같다.

```undefined
fn fragment(input: &str) -> Res<&str, &str> {
    context("fragment", tuple((tag("#"), url_code_points)))(input)
        .map(|(next_input, res)| (next_input, res.1))
}
```

우리가 취재한 이 복잡한 파서들 이후로, 이건 아이들의 놀이입니다. 적절한 측정을 위해 몇 가지 위생 검사 테스트를 작성해 보겠습니다.

```undefined
    #[test]
    fn test_fragment() {
        assert_eq!(fragment("#bla"), Ok(("", "bla")));
        assert_eq!(fragment("#bla-blub"), Ok(("", "bla-blub")));
    }
```

## 'nom'을 사용한 Rust의 구문 분석: 마지막 시험

위에서 정의한 모든 부품을 사용하는 최상위 수준 `우리` 파서 함수로 모두 통합해 보겠습니다.

```undefined
pub fn uri(input: &str) -> Res<&str, URI> {
    context(
        "uri",
        tuple((
            scheme,
            opt(authority),
            ip_or_host,
            opt(port),
            opt(path),
            opt(query_params),
            opt(fragment),
        )),
    )(input)
    .map(|(next_input, res)| {
        let (scheme, authority, host, port, path, query, fragment) = res;
        (
            next_input,
            URI {
                scheme,
                authority,
                host,
                port,
                path,
                query,
                fragment,
            },
        )
    })
}
```

우리는 `계획`을 필수로 하고, `권한` 선택과 `ip or host` 선택과 함께 `ip or host`를 의무적으로 시행한다. 그러면 포트, 경로, 쿼리 매개 변수 및 조각이 선택 사항으로 나타납니다.

지도에서 남은 것은 파싱된 요소들로 우리의 `URI` 구조를 만드는 것뿐이다.

이것이 이 구조 전체가 얼마나 멋지고 모듈적인지 알 수 있는 지점입니다. 만약 `우리` 기능이 여러분의 출발점이라면, 여러분은 모든 것이 무엇을 하고 있는지 이해하기 위해 각각의 파서를 보면서 위에서 아래로 내려갈 수 있습니다.

물론, "우리"에 대한 몇 가지 테스트도 필요합니다.

```python
    #[test]
    fn test_uri() {
        assert_eq!(
            uri("https://www.zupzup.org/about/"),
            Ok((
                "",
                URI {
                    scheme: Scheme::HTTPS,
                    authority: None,
                    host: Host::HOST("www.zupzup.org".to_string()),
                    port: None,
                    path: Some(vec!["about"]),
                    query: None,
                    fragment: None
                }
            ))
        );

        assert_eq!(
            uri("http://localhost"),
            Ok((
                "",
                URI {
                    scheme: Scheme::HTTP,
                    authority: None,
                    host: Host::HOST("localhost".to_string()),
                    port: None,
                    path: None,
                    query: None,
                    fragment: None
                }
            ))
        );

        assert_eq!(
            uri("https://www.zupzup.org:443/about/?someVal=5#anchor"),
            Ok((
                "",
                URI {
                    scheme: Scheme::HTTPS,
                    authority: None,
                    host: Host::HOST("www.zupzup.org".to_string()),
                    port: Some(443),
                    path: Some(vec!["about"]),
                    query: Some(vec![("someVal", "5")]),
                    fragment: Some("anchor")
                }
            ))
        );

        assert_eq!(
            uri("http://user:pw@127.0.0.1:8080"),
            Ok((
                "",
                URI {
                    scheme: Scheme::HTTP,
                    authority: Some(("user", Some("pw"))),
                    host: Host::IP([127, 0, 0, 1]),
                    port: Some(8080),
                    path: None,
                    query: None,
                    fragment: None
                }
            ))
        );
    }
```

됐다! 전체 예제 코드는 GitHub에서 찾을 수 있습니다.

## 결론

정말 멋지다! 이 기사가 파서, 특히 러스트의 파서 콤비네이터에 대해 당신을 흥분시켰기를 바랍니다.

nom 라이브러리는 많은 프로덕션 수준의 라이브러리와 시스템에 터무니없이 빠르고 기초적인 것 외에도 훌륭한 API와 몇 가지 정말 좋은 문서를 제공한다.

Rust 생태계는 파서 콤비네이터를 구현하는 콤바인 라이브러리, pest와 같은 더 많은 구문 분석 옵션을 제공한다.