---
layout: post
title: "Swift에서 Firebase 사용 방법UI의 새로운 애플리케이션 수명 주기"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![SwiftUI and Firebase’s logos](https://miro.medium.com/max/3840/1*Q4DH5dGLTOGLpd0sVrmlKQ.png)

SwiftUI 2.0은 우리에게 많은 새로운 기능을 가져다 주었다. 제가 가장 좋아하는 것은 새 애플리케이션 수명 주기입니다. 이전 애플리케이션보다 간단하지만 Firebase로 구성하는 것은 어려울 수 있기 때문입니다.

이 기사에서는 Swift를 사용하여 Firebase를 구성하는 방법에 대해 설명합니다.UI의 새로운 라이프사이클입니다.

먼저, 새로운 앱 라이프사이클과 기존 앱 라이프사이클의 차이점을 살펴보겠습니다.

참고: 이 기사의 예시 프로젝트와 소스는 GitHub에서 다운로드할 수 있습니다.

# UIKit App Delegate 라이프사이클과 비교 SwiftUI 수명 주기

UIKit의 라이프사이클을 사용하여 "didFinish LaunchingWithOptions" 방법으로 Firebase의 "configure() 기능을 작성해야 합니다. 이 방법은 응용 프로그램을 처음 열 때 수행해야 하는 작업을 위한 것입니다.

SwiftUI의 라이프 사이클이 UIKit와 다른 이유는 Swift 때문입니다.UI는 선언적 프레임워크입니다. 그래서 방법과 계층이 다른 것이다. 분명히, 스위프트에서 우리가 사용할 방법은UI 라이프사이클은 약간 다릅니다.

Swift를 사용했다면UI와 UIK는 이전에 Swift에서 사용하고자 하는 일부 클래스 또는 메서드를 함께 사용했습니다.UI는 UIKit에서 제공되므로 Swift에서 지원되지 않는 것이 많습니다.UI. 우리는 스위프트를 적응시키면서 UIKit의 도움을 받을 것이다.UI 앱 수명 주기부터 Firebase까지입니다.

현재로서는 그들 사이에 상당한 차이가 있다고 말할 수 없다. 스위프트와 함께라면 상황이 달라질지도 모른다.UI 3.0.

# Swift 사용 시작UI의 새로운 라이프사이클

우선, 우리는 새로운 Xcode 프로젝트를 만들어야 합니다. (Swift 사용).UI의 새로운 라이프사이클, Xcode 12 필요) 새 프로젝트를 만드는 동안 "인터페이스"가 "Swift"인지 확인하십시오.UI와 "Life Cycle"은 "Swift"입니다.UI 앱"

![Options for new Xcode project](https://miro.medium.com/max/3024/1*HMqlUNTZcckTObAymJokkQ.png)

프로젝트를 만든 후 `SampleProjectApp.swift` 파일로 이동하십시오(프로젝트 이름은 나와 다를 수 있음). 새 앱 라이프사이클을 사용할 때 가장 먼저 라이프사이클에 참여할 수 있는 포인트는 앱의 기본 진입점에 이니셜라이저를 추가하는 것입니다.

이 프로젝트에서 Firebase를 구성한다고 가정해 보겠습니다. 이 파일에 Firebase를 가져와야 합니다.

그럼 어떻게 파이어베이스를 초기화할 수 있을까요? 답은 간단하다: `init` 방법이다.

이는 Cloud Firestore와 같은 Firebase SDK에는 잘 작동하지만, 애플리케이션 라이프사이클에 연결하기 위해 스위칭 방법을 사용하기 때문에 Firebase Cloud Messaging에는 충분하지 않습니다. 이 메커니즘을 통해 프레임워크는 특정 메서드에 대한 호출을 가로채고 호출을 앱에 전달하기 전에 처리할 수 있습니다.

이 경우 앱을 `AppDelegate` 인스턴스에 연결하려면 `@UIApplicationDelegateAdaptor` 속성 래퍼를 사용해야 합니다.

동시에 새로운 앱 수명 주기 모델로 점차 마이그레이션할 수 있습니다.

# 결론

이 기사가 도움이 되었기를 바랍니다. 앞으로도 파이어베이스에 대해서 쓸 테니까 잘 듣고 있어!