---
layout: post
title: "AVIF, 스쿼시 및 go-avif를 사용하여 품질 이미지를 포맷하는 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/AVIF.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/AVIF.png?fit=730%2C487&ssl=1)

AVIF(AV1 Image File Format)는 AV1 비디오 코덱의 키 프레임을 기반으로 하는 오픈 소스, 로열티 없는 이미지 포맷이다. 구글·아마존·마이크로소프트·넷플릭스 등 주문형비디오(VP9) 제공업체가 여럿 포함된 연합(Alliance for Open Media)이 VP9의 후속으로 개발했다.

2018년 출시 후 빠르게 톱 비디오 코덱으로 자리매김했다. Facebook과 Netflix와 같은 회사들은 자체 스트리밍 비디오 인프라에서 어떻게 작동하는지 알아보기 위해 테스트했고, 결과에 깊은 인상을 받았습니다.

AV1 코덱 뒤의 두뇌들도 동일한 압축 알고리즘을 사용하여 낮은 파일 크기로 고품질 이미지를 생성하는 이미지 파일 형식을 만들기로 결정했고, 결국 AVIF 포맷이 개발되어 2019년 2월에 공식 승인되었다.

이 자료에서는 이 이미지 형식을 사용하여 시각적 충실도를 유지하는 이미지를 압축하는 방법을 보여 주며, 이는 궁극적으로 사용자에게 더 나은 환경을 제공합니다.

## AVIF와 JPEG 및 WebP 비교

AVIF 포맷은 JPEG와 WebP 포맷에 비해 파일 크기를 크게 줄인다. 다음은 거의 동일한 시각적 품질에서 각 형식을 비교하는 것입니다.

위의 예는 시각적 차이가 무시해도 좋다는 사실에도 불구하고 파일 크기 간의 현저한 차이를 보여준다. JPEG 버전은 98.3kB로 가장 큰 반면, WebP 버전은 69.kB로 약 30% 더 작다. AVIF 버전은 JPEG 버전보다 58% 작은 42.1kB로 출시되며 이는 상당한 차이이다.

위의 이미지는 각 형식의 기본 설정을 사용하여 스쿼시를 사용하여 만들어졌습니다. 마스터 이미지를 직접 사용해 보고자 하는 경우, 여기에 마스터 이미지에 대한 링크가 있습니다.

## AVIF 이미지를 생성하는 방법

스푸쉬는 간단하고 사용하기 쉽다. 파일 시스템에서 이미지를 선택하거나 편집기에서 제공한 샘플 이미지 중 하나를 사용할 수 있습니다.

영상이 로드되면 MozJPEG, AVIF, WebP 및 OptiPNG를 포함한 여러 압축 방법 중에서 선택할 수 있습니다. 한 압축 방법을 다른 압축 방법 또는 원본 이미지와 비교할 수 있도록 편집기의 오른쪽과 왼쪽을 모두 사용하여 이 작업을 수행할 수 있습니다. 또한 압축 수준을 조정할 수 있을 뿐만 아니라 고급 설정으로 이동할 수도 있습니다.

설정을 변경하면 편집기의 반대쪽에 있는 설정과 비교하여 결과의 예상 파일 크기가 표시되며, 두 결과를 쉽게 비교할 수 있도록 중간에 슬라이더 단추 스맥이 있습니다.

AVIF 형식의 설정은 쉽게 이해할 수 있습니다. 최소 및 최대 영상 화질(0 ~ 62 사이)을 조정하고 작업 수준(값이 높을수록 결과를 생성하는 데 시간이 오래 걸림)을 선택할 수 있습니다. 이러한 설정을 조정하면 허용 가능한 품질 수준에서 인상적인 결과를 볼 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/2XZKdOB9.png?resize=400%2C758&ssl=1)

## 이미지 배치 변환

AVIF 파일을 만드는 데 스쿼시를 사용할 때의 단점은 한 번에 하나의 이미지만 변환할 수 있다는 점입니다. 이미지를 일괄 처리하려면 오픈 소스 `go-avif` 도구에 의존할 수 있습니다. Go로 작성되었으며 AVIF에 JPEG 및 PNG 파일 인코딩을 지원합니다.

먼저 도구를 설치해야 합니다. 컴퓨터에 Go가 설치되어 있는 경우 아래 명령을 사용하여 도구를 컴파일하고 바이너리를 `$GOPS/bin`에 설치합니다.

```coffeescript
go get github.com/Kagami/go-avif/...
```

그렇지 않으면 릴리스 페이지에서 Windows, macOS 또는 Linux용 미리 컴파일된 이진 파일을 다운로드하여 `$PATH`에 복사할 수 있습니다. go-avif의 가장 간단한 용도는 다음과 같다. 기본 설정을 사용하여 단일 JPEG 이미지를 AVIF로 변환합니다.

```css
avif -e cat.jpg -o kitty.avif
```

많은 이미지를 한 번에 쉽게 처리할 수 있도록 스크립트를 생성합시다. 새 `avif`를 만듭니다.sh` 파일을 입력하고 다음 코드를 입력합니다.

```bash
#!/bin/bash
for f in *.{jpg,jpeg,png}
  do
    name=$(echo "$f" | cut -f 1 -d '.') # Extract the filename without the extension
    avif -e $f -o $name.avif # Encode to AVIF

    # Fetch and print size information
    input=`wc -c $f | cut -d' ' -f1`
    output=`wc -c $name.avif | cut -d' ' -f1`
    echo "File $f — Original file size: $(($input/1000)) kB, AVIF file size: $(($output/1000)) kB."
  done
```

이 스크립트는 현재 폴더의 모든 JPEG 및 PNG 이미지를 처리하고 `go-avif` 도구의 기본 설정을 사용하여 AVIF 형식으로 변환합니다. 사용 가능한 다른 설정을 `avif -help`로 표시하고 다른 결과를 얻기 위해 실험할 수 있습니다.

스크립트를 이미지가 들어 있는 폴더로 이동하고 실행 파일로 표시한 다음 실행합니다.

```undefined
chmod +x avif.sh
./avif.sh
```

그러면 각 이미지가 처리되고 AVIF 출력이 동일한 폴더에 배치됩니다. 각 변환 후 파일 크기 비교도 표시됩니다. 아래 이미지에서 볼 수 있듯이 설정을 수정하지 않아도 상당한 비용 절감 효과를 얻을 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/AVIF-output-file-comparison-e1603729265161.png?resize=730%2C465&ssl=1)

## 웹에서 AVIF를 사용하는 방법

브라우저 지원 문제를 해결합시다. AVIF는 현재 지원이 매우 제한적입니다. 공식 Firefox 지원이 곧 도착할 예정이지만 데스크톱 크롬(버전 85 이상)에서만 지원됩니다(`about:config`의 image.avif.enabled` 플래그를 통해 Firefox 77 이상에서 실험 지원을 활성화할 수 있습니다). Safari의 경우 WebP와 같은 지원을 추가하는 데 10년이 걸리지 않습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/AVIF-supported-browsers-chrome-firefox.png?resize=730%2C290&ssl=1)

웹 사이트 및 응용 프로그램에서 AVIF를 사용하기 전에 모든 브라우저가 이를 지원할 때까지 기다릴 필요가 없습니다. AVIF가 지원되지 않는 경우 `<그림>` 요소를 사용하여 JPEG 또는 WebP 폴백을 제공할 수 있습니다.

```bash
<picture>
  <source srcset="image.avif" type="image/avif">
  <source srcset="image.webp" type="image/webp">
  <source srcset="image.jpg" type="image/jpeg">
  <img src="image.jpeg" alt="Description of the image">
</picture>
```

위의 조각은 `<그림> 요소(모든 버전의 Internet Explorer)를 지원하지 않는 브라우저에서도 모든 브라우저에서 작동합니다. 이 경우, `img` 태그에 지정된 소스가 표시됩니다. 여기 어디에서나 작업하기 위해 `그림`이 필요할 때 사용할 수 있는 폴리필이 있습니다.

## AVIF의 한계

AVIF 형식의 분명한 브라우저 지원 제한(시간 경과에 따라 좋아지기 마련) 외에도, 웹 사용을 위해 AVIF를 채택할 때의 두 가지 주요 단점이 여기에 있다.

### 진행형 렌더링을 지원하지 않습니다.

표준 JPEG 및 WebP 영상은 이미지가 완전히 로드될 때까지 위에서 아래로 라인별로 로드됩니다. JPEG 포맷은 또한 전체 이미지의 흐릿한 버전이 먼저 로드되도록 하는 진행형 인코딩 방식을 지원하며, 나머지 바이트가 도착할수록 점차 선명해진다.

점진적 렌더링의 이점은 파일의 일부만 다운로드된 경우에도 전체 이미지를 볼 수 있다는 것입니다. 또한 전체 이미지가 처음부터 끝까지 표시되므로 상하 방향 렌더보다 이미지 로딩 속도가 빨라집니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/avif-demo-1-1.gif?resize=730%2C367&ssl=1)

불행하게도 AVIF는 상하 방향 렌더링이나 진행 방향 렌더링을 지원하지 않습니다. 위의 동영상에서 설명한 대로 완전히 로드된 이미지를 보거나 아무것도 보이지 않습니다(데모를 만든 Jake Archibald에게 공로를 인정함). 이렇게 하면 대형 이미지에 적합하지 않은 형식이 될 수 있습니다. AVIF를 웹 사이트에 배포하기 전에 고려해야 할 사항입니다.

### 소프트웨어 지원이 부진함

AVIF는 새로운 포맷이기 때문에 JPEG와 PNG와 같은 오랜 포맷이 가지고 있는 유비쿼터스 지원이 부족하다. 현재로서는 AVIF 파일을 지원하는 이미지 뷰어는 극히 일부에 불과하며, 이는 곧 바뀔 것 같지 않습니다. 웹 사이트의 사용자가 아무것도 할 수 없는 파일을 다운로드하기 때문에 이미지를 장치에 저장하는 경향이 있는 경우 문제가 발생할 수 있습니다.

이 문제를 완화하는 한 가지 방법은 AVIF를 채택할 때에도 두 형식 중 하나의 호환성 이점이 유지되도록 이미지의 JPEG 또는 PNG 버전을 가리키는 명시적 다운로드 링크를 제공하는 것입니다.

## 결론.

AVIF 형식은 현재 제한된 브라우저 지원 및 위에서 논의된 기타 사항에도 불구하고 JPEG, PNG 및 WebP 파일과 비교할 만한 수준의 시각적 품질에서 파일 크기가 현저하게 감소했기 때문에 검토할 가치가 충분히 있다.

웹 사이트에서 AVIF를 채택하면 이미지 품질을 저하시키지 않고 페이지 로드 시간을 단축할 수 있습니다. 먼저 이미지 하위 집합에 대해 배포한 다음 시간이 지남에 따라 사용을 확장할 수 있습니다. 그러니 오늘 가서 한번 해 보세요. 그만한 가치가 있을 거예요!