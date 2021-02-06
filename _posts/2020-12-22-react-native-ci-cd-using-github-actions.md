---
layout: post
title: "GitHub 작업을 사용하여 기본 CI/CD 대응"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/react-native-ci-cd-github-actions.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/react-native-ci-cd-github-actions.png?fit=730%2C487&ssl=1)

시간은 우리 인간이 가진 가장 가치 있는 자산이다. 개발자로서, 우리는 두 가지 일을 하는데 너무 많은 시간을 소비합니다. 코드가 왜 작동하지 않는지 궁금해하며, 앱이 만들어지기를 기다리는 것입니다. 아무도 그 건물이 완성되기를 기다렸다가 집으로 돌아가기 전에 나눠주는 것을 좋아하지 않는다. 자동화하지 않는 이유는 무엇입니까? CI/CD? 예! React Native 앱도 마찬가지입니다.

현재 React Native 앱의 CI/CD에 사용할 수 있는 훌륭한 툴을 보유하고 있습니다. GitHub Actions, GitLab CI/CD, CircleCI, Travis CI 및 기타 많은 것들이 개발자들이 많은 시간과 자원을 절약하도록 돕고 있습니다. 오늘 우리는 React Native 애플리케이션의 CI/CD에 Github Actions를 사용할 것이다.

이 게시물은 React Native 응용 프로그램 테스트와 React Native 테스트 라이브러리에 대해 설명하는 이전 게시물의 계속입니다. 비록 그것이 정확히 선행조건 읽기는 아니지만, 우리는 코드의 품질을 유지하기 위한 몇 가지 사전 커밋 작업에 대해 토론했다. 같은 앱을 기반으로 CI/CD 파이프라인을 여기서 완료할 예정입니다.

## GitHub Actions를 사용한 CI/CD에 대한 정보

GitHub Actions를 사용하면 관련 조건에 따라 실행할 워크플로우를 정의할 수 있습니다. 모든 리포지토리에는 서로 다른 이벤트를 기반으로 다른 작업을 트리거하는 여러 워크플로가 포함될 수 있습니다. 모든 트리거에서 GitHub는 `.ghub/workflows/`의 모든 YAML 파일을 선택하고 필요한 워크플로우를 실행합니다. 워크플로가 갑자기 종료되면 작업이 실패로 표시됩니다.

프로젝트 확인이나 파일 캐싱과 같은 몇 가지 기본 작업을 수행하기 위해 GitHub는 공식적으로 유지되는 광범위한 작업을 가지고 있다. 우리는 또한 커뮤니티에 의해 유지되는 몇 가지 오픈 소스 행동을 활용할 수 있다.

## 우리의 목표

우리의 요구사항은 다음과 같습니다.

- 개발 분기의 모든 꺼내기 요청에 대해 모든 테스트 [CI]를 실행합니다.
- 개발 지사에서 모든 작업을 수행할 때마다 Firebase 앱 배포[CD]를 통해 Android 애플리케이션을 배포하십시오.
- 알파 분기를 누를 때마다 Google Play Console [CD]를 통해 응용 프로그램의 알파 릴리스를 게시하십시오.
- 메인 브랜치를 누를 때마다 Google Play Console [CD]를 통해 응용 프로그램의 베타 릴리스를 게시하십시오.

표시된 바와 같이 당사의 첫 번째 요구사항은 CI(연속 통합) 단계에 속합니다. 이를 통해 애플리케이션을 중단하지 않고 개발을 신속하게 진행할 수 있는 자동화된 방법을 구축할 수 있습니다. CD(연속 전송)는 CI가 끝나는 지점을 선택합니다. CD는 선택한 환경에 애플리케이션을 자동으로 제공합니다.

## React Native 앱의 CI 워크플로우 추가

요약하자면, CI와 CD의 두 가지 워크플로우가 있습니다. 모든 워크플로우는 서로 다른 트리거에서 실행될 수 있습니다. 이 경우 CI 워크플로우는 보호된 분기의 모든 풀 요청에서 실행되는 반면 CD 워크플로우는 보호된 분기의 모든 푸시에서 실행됩니다.

repo에 대한 CI 워크플로우를 설정하기 위해 `.ghub/workflows/inc`에 새 파일 `ci.yml`을 추가합니다.

### CI 워크플로우 정의

CI 워크플로 트리거는 보호된 분기에서 꺼내기 요청입니다. 워크플로우 구성은 다음과 같습니다.

```undefined
name: Continuous Integration

on:
  pull_request:
    branches:
      - develop
      - alpha 
      - main
```

보시는 바와 같이, 정의된 분기의 모든 풀 요청에 대해 실행됩니다.

### 프로젝트 체크 아웃

GitHub 작업을 통해 정의한 모든 워크플로우는 별도의 가상 시스템에서 실행됩니다. 코드베이스에서 어떤 것이든 실행하려면, 우리는 할당된 인스턴스의 레포를 확인해야 합니다. 앞에서 논의한 바와 같이, GitHub는 그러한 작업을 처리하는 데 사용할 수 있는 매우 다양한 유틸리티 작업을 가지고 있다. 여기서는 Checkout 작업을 사용합니다.

```coffeescript
    - name: Checkout
      uses: actions/checkout@v2
```

### 종속성 설치

체크아웃이 완료되면 환경 및 프로젝트를 설정할 것입니다. GitHub VM에서 Node.js 프로젝트를 실행하려면 공식 Node 설정 작업을 사용할 수 있습니다.

반응 네이티브 테스트 라이브러리를 사용하여 테스트를 실행 중이므로 일부 npm 종속성을 설치해야 합니다. 이 단계에서는 Runner에 노드 환경을 설정하고 npm 종속성을 설치합니다.

```undefined
    - uses: actions/setup-node@master
    - uses: c-hive/gha-yarn-cache@v1

    - name: Install node modules
      run: |
        yarn install
```

후속 작업을 수행하기 위해 프로젝트에 대한 npm 종속성을 캐시할 수 있습니다. 우리는 C-Hive의 원라이너 Yarn 모듈 캐시 동작을 활용했다.

### 테스트 실행

이제 노드 환경과 종속성을 사용할 수 있게 되었으므로 테스트 스위트 실행을 시작하겠습니다. 이는 로컬 CLI를 사용하여 테스트를 실행하는 것만큼 간단합니다.

```bash
    - name: Run test
      run: |
        yarn test-ci
```

논의된 바와 같이 이 작업은 모든 풀 요청에서 실행됩니다. GitHub는 모든 PR에서 파이프라인 실행 결과를 표시합니다. 화면에는 어떻게 표시되는지 나와 있습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/successful-pipeline-execution.png?resize=730%2C219&ssl=1)

이렇게 하면 CI 워크플로가 마무리됩니다. 이 워크플로우는 당사의 보고서에 병합된 모든 코드가 당사의 품질 검사를 통과하기 때문에 코드 품질 측면에서 애플리케이션을 더욱 확장 가능하게 합니다.

## 빌드 작업 추가

결정적 CI 단계가 그대로 유지되기 때문에 보호된 분기에 병합된 모든 코드는 모든 테스트를 통과합니다. 따라서 PR이 병합되면 애플리케이션을 빌드하는 것이 안전합니다. CD 파이프라인에서는 요구 사항에 따라 앱을 만들고 게시할 예정입니다.

Android 앱의 경우 빌드 작업의 단계는 다음과 같습니다.

### Gradle 캐시 설정

npm 의존성이 없었던 것처럼, 빌드 속도를 높이기 위해 Gradle 종속성과 래퍼를 캐시할 것입니다. 여기서 GitHub의 캐시 작업을 사용합니다.

```undefined
    - name: Cache Gradle Wrapper
      uses: actions/cache@v2
      with:
        path: ~/.gradle/wrapper
        key: ${ runner.os }-gradle-wrapper-${ hashFiles('gradle/wrapper/gradle-wrapper.properties') }

    - name: Cache Gradle Dependencies
      uses: actions/cache@v2
      with:
        path: ~/.gradle/caches
        key: ${ runner.os }-gradle-caches-${ hashFiles('gradle/wrapper/gradle-wrapper.properties') }
        restore-keys: |
          ${ runner.os }-gradle-caches-
```

Gradle 종속성과 래퍼를 별도로 캐싱하여 후속 빌드에 사용할 수 있는지 확인하고 귀중한 CI/CD 시간을 절약합니다.

### Android 릴리스 빌드 생성

이제 Android 앱의 빌드 프로세스를 시작합니다.

```bash
    - name: Make Gradlew Executable
      run: cd android && chmod +x ./gradlew

    - name: Build Android App Bundle
      run: |
        cd android && ./gradlew bundleRelease --no-daemon
```

일부 Linux 환경에서는 Gradle을 기본적으로 실행할 수 없습니다. 따라서, 안전한 편에 서기 위해, 우리는 무거운 제작 과정을 시작하기 전에 `gradlew`가 실행 가능한지 확인한다.

모든 작업이 별도의 인스턴스로 실행되므로 Gradle 데몬을 실행하는 것은 의미가 없습니다. 메모리에 추가 부담이 생깁니다. 따라서 빌드 단계에 --no-daemon 플래그를 추가했습니다.

### Android 릴리스 서명

앱의 보안을 위해 코드베이스 외부에 계속 서명하는 것이 좋습니다. 빌드가 완료되면 서명되지 않은 앱 번들을 사용할 수 있습니다. 이 번들에 서명하기 위해, 우리는 오픈 소스 서명 작업인 r0adkll/sign-android 릴리스를 사용할 것이다.

다음은 릴리스에 서명하기 위한 단계입니다.

```bash
    - name: Sign App Bundle
      id: sign_app
      uses: r0adkll/sign-android-release@v1
      with:
        releaseDirectory: android/app/build/outputs/bundle/release
        signingKeyBase64: ${ secrets.ANDROID_SIGNING_KEY }
        alias: ${ secrets.ANDROID_SIGNING_ALIAS }
        keyStorePassword: ${ secrets.ANDROID_SIGNING_STORE_PASSWORD }
        keyPassword: ${ secrets.ANDROID_SIGNING_KEY_PASSWORD }
```

이 작업은 우리 프로젝트의 비밀을 사용합니다. 다음 명령을 사용하여 `signingKeyBase64`를 생성할 수 있습니다.

```undefined
openssl base64 < some_signing_key.jks | tr -d '\n' | tee some_signing_key.jks.base64.txt
```

출력 .txt 파일의 내용은 Base64 서명 키가 됩니다.

### 아티팩트 업로드

GitHub Actions를 사용하면 모든 작업의 출력을 저장할 수 있습니다. 이러한 파일은 나중에 테스트 또는 백업 목적으로 다운로드할 수 있습니다. 이 작업의 아티팩트로 서명된 앱을 업로드할 것입니다.

```undefined
    - name: Upload Artifact
      uses: actions/upload-artifact@v2
      with:
        name: Signed App Bundle
        path: ${steps.sign_app.outputs.signedReleaseFile}
```

여기서 중요한 점은 서명된 APK의 경로인데, 이는 서명된 APK의 경로를 출력하는 이전 단계인 `sign_app`을 통해 생성되었다. 여기서, 우리는 같은 길을 사용할 것입니다.

작업 빌드 단계를 마칩니다! 이제 서명된 번들 또는 APK를 사용할 수 있습니다.

## Firebase를 통해 앱 배포

상기 요구사항에서 언급한 바와 같이 개발 지점으로의 모든 추진 시 우리는 Firebase App Distribution을 사용하여 애플리케이션을 배포해야 합니다. Firebase CLI를 사용하려면 인스턴스에 `firebase-tools`를 전체적으로 설치하고 해당 `PATH`를 사용할 수 있도록 할 것입니다.

```bash
    - name: Distribute app via Firebase App Distribution
      env:
          firebaseToken: ${ secrets.FIREBASE_TOKEN }
          firebaseGroups: ${ secrets.FIREBASE_GROUPS }
          firebaseAppId: ${ secrets.FIREBASE_APP_ID }
          notes: ${ github.event.head_commit.message }
      run: |
        yarn global add firebase-tools
        export PATH="$(yarn global bin):$PATH"
        firebase \
          appdistribution:distribute android/app/build/outputs/apk/release/app-release.apk \
          --app $firebaseAppId \
          --release-notes "$notes" \
          --groups "$firebaseGroups" \
          --token "$firebaseToken"
```

여기서는 `fire base`를 사용합니다.토큰, 파이어베이스 그룹, 파이어베이스 앱아이드는 모두 우리의 비밀에서 나온 것이다. 이러한 값은 env의 비밀에서 설정한 다음 CLI 명령에서 env 변수를 사용했습니다. 이것은 비밀과 다른 변수에 접근하는 또 다른 방법이다.

또한, 우리는 GitHub 이벤트에서 헤드 커밋의 메시지에 액세스했습니다. 작업, 커밋 등에 대한 광범위한 정보를 워크플로 컨텍스트에서 사용할 수 있습니다.

## Play Console을 통해 배포

홈런을 냅시다. 이 단계에서는 Google Play Console을 통해 서명된 앱을 배포합니다. 이 프로세스를 수행하려면 Google Play Developer API를 설정하고 Play Console과 연결해야 합니다. 이 설정을 완료하면 서비스 계정에 대한 모든 정보가 포함된 JSON 파일이 제공됩니다. 우리는 그것을 우리의 프로젝트 비밀에 넣을 것이다.

이 단계에서는 Play API와 관련된 모든 무거운 리프팅 작업을 하는 오픈 소스 r0adkll/업로드-google-play를 사용할 것이다.

```undefined
    - name: Deploy to Play Store (BETA)
      uses: r0adkll/upload-google-play@v1
      with:
        serviceAccountJsonPlainText: ${ secrets.ANDROID_SERVICE_ACCOUNT }
        packageName: com.testedapp
        releaseFile: a${steps.sign_app.outputs.signedReleaseFile}
        track: beta
        inAppUpdatePriority: 3
        userFraction: 0.5
        whatsNewDirectory: android/release-notes/
```

테스트한 앱이 수동 작업 없이 최종 사용자에게 제공됩니다.

## 다음에 할 일이 뭐죠?

처음에 논의한 바와 같이, CI/CD의 주요 목표는 개발자의 시간을 절약하고 코드베이스와 결과 애플리케이션의 품질을 유지하는 것이다. GitHub Actions를 활용하면 애플리케이션을 수동으로 구축하고 배포하는 데 소모되는 많은 시간을 절약할 수 있습니다.

또한 CI를 통해 개발자는 당사의 모든 품질 표준 및 테스트를 통과한 코드만 추출할 수 있게 되며, 이는 버그가 줄어들고 반복 시간이 단축된다는 것을 의미합니다. 공짜 피자를 제외하고 무엇이 여러분을 더 행복하게 할 수 있을까요?

하지만 GitHub Actions와 함께 일하면서 놓쳤던 중요한 기능 중 하나는 다른 직업들 사이에서 단계를 공유할 수 있는 능력이다. 이 경우 모든 작업에서 복제, 종속성 설치 및 테스트 실행을 수행해야 했습니다.

현재 모든 스크립트에 단계가 정의되어 있습니다. 서로 다른 작업 간에 단계를 공유하면 이러한 중복을 방지할 수 있습니다. 이 논의를 계속 추적하고 이것이 GitHub Actions에 빨리 도착하기를 바랍니다.

자세한 내용을 살펴보고 여러 작업을 병렬 및 상호 종속적으로 어떻게 실행할 수 있는지 확인할 수 있습니다. 또한 서로 다른 프로젝트 간에 공유할 워크플로 템플릿을 생성할 수도 있습니다. 아래 코멘트에서 GitHub Actions 및 React Native와 함께 멋진 작업을 공유하십시오!