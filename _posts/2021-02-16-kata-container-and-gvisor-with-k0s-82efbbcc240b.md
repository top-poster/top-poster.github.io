---
layout: post
title: "K0이 장착된 Kata 컨테이너 및 GVisor"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![People standing on an ice floe.](https://miro.medium.com/max/8544/1*Cl45YAbWEYljeITf8b_rYQ.jpeg)

모든 포드를 신뢰할 수 있는 것은 아니기 때문에 이 자료에는 기본 포드가 아닌 컨테이너 실행 시간(runc)을 사용하여 프로세스 격리를 강화하기 위한 다양한 옵션이 나와 있습니다. 우리는 이 모든 것을 설명하기 위해 Kubernetesk0s 분포를 사용할 것이다. k0을 모르면 이 글에서 빠른 소개를 찾을 수 있습니다.

# K0s 클러스터 생성

소개 기사에서는 k0s 클러스터를 쉽게 설정하는 데 필요한 단계를 자세히 설명했습니다. 최근 미란티스(Mirantis)는 일부 리눅스 서버에 대한 ssh 액세스만 요청하는 k0sctl 동반 바이너리를 도입하여 클러스터를 자동으로 설치하였다.

GitHub에서 최신 릴리스를 가져오면 기본 구성 파일을 생성할 수 있습니다.

```undefined
$k0sctl in > k0sctl.decl
```

다음 속성을 포함합니다.

```undefined
apiVersion: k0sctl.k0sproject.io/v1beta1
종류: 군집
메타데이터:
이름: k0s-클러스터
사양:
호스트:
- ssh:
주소: 10.0.0.1
사용자: 루트
포트: 22
keyPath: /Users/luc/.ssh/id_rsa
역할: 서버
- ssh:
주소: 10.0.0.2
사용자: 루트
포트: 22
keyPath: /Users/luc/.ssh/id_rsa
역할: 작업자
k0s:
버전: 0.10.0
```

이 파일을 수정하고 제어 계획의 프로세스(API Server, kube-Controller, kube-proxy), 네트워크 플러그인(기본적으로 Calico) 및 기타 구성 요소에 대한 추가 구성 옵션을 추가할 수 있습니다. 현재 예제에서는 기본 구성을 유지하고 미리 프로비저닝된 호스트와 일치하도록 IP 주소를 변경했습니다.

간단한 명령으로 클러스터 생성을 시작합니다(k0sctl.yaml은 기본적으로 사용되는 구성 파일입니다).

```undefined
$k0sctl 적용
```

![Image for post](https://miro.medium.com/max/2952/1*d53o_a__MMdsVQZKulzDCQ.png)

생성된 `쿠베 구성` 파일을 검색하고 로컬 `쿠베틀`을 구성합니다.

```undefined
$k0sctl kubeconfig -k0sctl.decl > kubeconfig
$ 내보내기 KUBECONFIG=$PWD/쿠베 구성
```

한 개만 볼 수 있는 클러스터의 노드를 확인하는 작업이 나열됩니다. 마스터는 제어부 관리를 전담하며 애플리케이션 포드를 예약할 수 없습니다.

```undefined
$쿠벡틀 no
이름 상태 역할 연령 버전
worker Ready < 5m v1.20.2-k0s1
```

참고: 제어면의 격리는 k0s 생성의 주요 원인 중 하나이다(Jussi Nummelin이 CNCF Ismalabad meetup에서 k0s에 대해 설명했듯이).

각 노드에서 실행 중인 에이전트인 kubelet은 컨테이너를 생성하기 위해 컨테이너 런타임과 통신해야 합니다. 이를 위해 K0s는 `containerd`를 제공하지만 다음과 같은 다른 컨테이너 런타임이 사용될 수 있습니다.

참고: Docker는 CRY와 호환되지 않으므로 Kubelet 내부에 Dockersheim 프로세스를 구현하여 Docker와 연결할 수 있습니다. 여러분은 아마도 뉴스에서 이 도커심(dockersheim)이 쿠베르네츠 1.20에서 더 이상 사용되지 않으며 향후 출시될 예정이라는 것을 들어봤을 것이다. 그러나 쿠베르네츠에서 도커를 고급 컨테이너 런타임으로 사용하는 것은 새로운 독립 실행형 도커심(dockersheim)이 개발되고 있기 때문에 여전히 가능할 것이다. 자세한 내용은 도커의 블로그에서 확인하십시오.

공식 문서에서 `containerd`는 "이미지 전송 및 저장, 컨테이너 실행 및 감독, 저수준 스토리지, 네트워크 첨부 파일 등 호스트 시스템의 전체 컨테이너 라이프사이클을 관리하는 프로세스"로 정의된다.

아래 다이어그램에서 볼 수 있듯이 이 제품은 여러 컨테이너 플랫폼에서 사용되는 중앙 부품입니다.

![Image for post](https://miro.medium.com/max/3564/1*5FxzkGddVGtYsCPxgvIBDQ.png)

"d"는 높은 수준의 컨테이너 런타임으로 간주되며 스키마 맨 아래에 나열된 낮은 수준의 런타임과 상호 작용할 수 있습니다.

![Image for post](https://miro.medium.com/max/3148/1*hj9ZC8V1nDytugcTWMij7Q.png)

컨테이너 d가 컨테이너의 전체 라이프사이클을 관리하는 경우, 낮은 수준의 컨테이너 실행 시간은 컨테이너 이미지 매니페스트와 루트 파일 시스템(컨테이너 d가 제공하는)에서 컨테이너를 실행하는 역할을 합니다. `runc`는 OCI(Open Container Initiative)에서 지정한 컨테이너 런타임의 참조 구현으로, 많은 Kubernetes 배포판(k0s 포함)에서 기본적으로 설치되고 사용되지만, 아래에서 볼 수 있듯이 다른 낮은 수준의 컨테이너 실행 시간을 설치 및 사용하는 것은 매우 쉽습니다. 이는 워크로드 격리를 통해 보안을 강화하기 위해 필요할 수도 있습니다.

# 기본 런타임

표준 Kubernetes 클러스터에서 단순 포드를 실행할 때 낮은 수준의 런타임 컨테이너는 runc가 됩니다. 다음 사양의 팟을 생성하면서 이것을 확인해 보겠습니다.

```undefined
$cat < 큐빅틀 적용 -f -
apiVersion: v1
종류: 포드
메타데이터:
이름: www-runc
사양:
컨테이너:
- image: nginx:1.18
이름: www
포트:
- 컨테이너 포트: 80
EOF
```

현재 worker node에서 실행 중인 프로세스를 살펴보면, "container-shim-runc-v2"라는 이름의 7개의 프로세스를 찾을 수 있는데, 이는 새로 생성된 팟과 우리를 위해 출시된 각 팟마다 하나씩이다. 7개의 포드가 현재 실행 중이며, 아래 명령을 통해 확인됩니다.

```undefined
$kubtl get po -A | awk '{print $1" "$2}'
기본 www-runc
큐브 시스템 칼리코큐브 컨트롤러-5f6546844f-tw8t6
큐브 시스템 칼리코노드-4p98g
큐브 시스템 코어ns-5c98d7d4d8-9z425
큐브 시스템 연결-에이전트-m4gst
쿠베 시스템 쿠베-코베-jq6qd
kube-system 메트릭스 서버-6fbcd86f7b-m8cnd
```

간단히 말하자면, 프로세스를 체인(chain)으로 불렀다고 할 수 있는 것은 다음과 같습니다.

다음 섹션에서는 클러스터에서 다른 낮은 수준의 컨테이너 실행 시간을 사용하는 방법에 대해 설명합니다.

# GVisor

간단히 말해서, gVisor는 Go로 작성되고 사용자 공간에서 실행되는 Linux 커널의 새로운 구현을 사용하여 컨테이너를 실행한다. 이 커널은 리눅스 시스템 호출 인터페이스의 상당 부분을 구현하고 응용 프로그램의 시스템 호출을 가로채 호스트 커널 취약성으로부터 추가적인 보호를 제공한다. gVisor는 보안, 효율성, 사용 편의성에 중점을 두고 있으며, OCI(Open Container Initiative) 런타임인 runsc도 포함되어 있습니다.

![Image for post](https://miro.medium.com/max/2792/1*GgMF6pZyUG-mPpUqQrwDlA.png)

다음에서는 컨테이너를 runsc를 통해 실행하도록 컨테이너d를 구성할 것입니다.

먼저 아래 명령(k0s 공식 설명서 참조)을 사용하여 모든 gVisor 패키지를 설치합니다.

다음으로, gVisor를 추가 런타임으로 사용할 수 있도록 `containerd` 구성 파일에 항목을 추가합니다.

```undefined
$cat cat서두티/k0s/containerd.toml
disabled_discs = ["discount"]
[linux.linux]
shim_sim = true
[s.s.sk.skd.runtimes.runsc]
runtime_type = "io.duppd.runsc.v1"
EOF
```

다음으로, `containerd`가 단일 작업자 노드에 해당 구성을 다시 로드하도록 강제 설정합니다.

```undefined
$kill -s HEASUP 컨테이너_PID
```

참고: containerd의 구성은 단일 worker 노드만 있으므로 쉽게 변경할 수 있습니다. 다중 노드 클러스터에서 이 작업은 자동화된 프로세스를 사용하여 전체적으로 수행되어야 합니다.

다음으로, 우리는 gVisor의 `runsc` 런타임과 관련된 새로운 `런타임 클래스`를 정의한다.

```undefined
$cat < 큐빅틀 적용 -f -
apiVersion: node.k8s.io/v1beta1
kind: RuntimeClass
메타데이터:
이름: gvisor
처리기: runsc
EOF
```

그런 다음 이 새로운 `런타임 클래스`를 사용하여 Pod를 실행합니다.

```undefined
$cat < 큐빅틀 적용 -f -
apiVersion: v1
종류: 포드
메타데이터:
레이블:
앱: 신뢰할 수 없음
이름: www-gvisor
사양:
runtimeClassName: gvisor
컨테이너:
- image: nginx:1.18
이름: www
포트:
- 컨테이너 포트: 80
EOF
```

app=the label과 함께 Pod를 나열하면 `www-gvisor`라는 팟이 정상적으로 실행되고 있음을 알 수 있습니다.

```undefined
$쿠벡틀 get po-l app=contract
이름 준비 상태 재시작 기간
www-gvisor 1/9 실행 09초
```

후드 아래에서는 gVisor/runsc를 사용하며, 사용자 및 커널에 의해 먼저 처리될 때 추가적인 보호 계층을 추가한다. 프로세스는 다음 체인으로 호출됩니다.

다음 섹션에서는 마이크로 VM에서 컨테이너를 실행하는 또 다른 로우 레벨 런타임의 사용에 대해 설명합니다.

# 카타 컨테이너

![Image for post](https://miro.medium.com/max/3668/1*UW2J69ETGwpI2B327dj5Nw.png)

이전 버전에서는 여러 심 프로세스(컨테이너드심, 카타심, 카타런타임, 카타프록시)를 사용했지만, 현재 버전에서는 단일 프로세스(컨테이너심-카타-v2)만 사용하므로 이 방식을 단순화했다.

![Image for post](https://miro.medium.com/max/5188/1*PDmbPZKMFSuwbR9YWy6Drg.png)

사전 요구 사항으로, 작업자 노드의 각 컨테이너에 대해 새 VM이 생성되므로 중첩된 가상화가 활성화되어야 합니다. 현재 예에서는 클러스터가 스케일웨이 가상 시스템에 생성됩니다. 이러한 VM은 AMD 기반이며 다음 명령으로 확인된 중첩된 가상화를 지원합니다.

```undefined
$cat/sys/sys/syponse_syponse/parameter/syp
1
```

먼저 Kata 컨테이너 패키지를 설치합니다.

```undefined
$bash -c "$(bash -fsSL https://raw.githubusercontent.com/kata-containers/tests/master/cmd/kata-manager/kata-manager.sh) install-discount)"
```

다음으로, 우리는 `containerd` 구성 파일을 수정하여 kata를 추가 런타임으로 사용하도록 한다(이전 단계에서 추가된 `runc`와 `gVisor`/`runsc` 위).

```undefined
$cat catsudoe -a /etc/k0s/containerd.toml
[s.s.s.d.runtime.d.runtime]카타]
runtime_type = "io.dpgd.kata.v2"
EOF
```

다음으로, `containerd`가 단일 작업자 노드에 해당 구성을 다시 로드하도록 강제 설정합니다.

```undefined
$kill -s HEASUP 컨테이너_PID
```

다음으로, 우리는 kata 런타임과 관련된 새로운 `런타임 클래스`를 정의한다.

```undefined
$cat < 큐빅틀 적용 -f -
kind: RuntimeClass
apiVersion: node.k8s.io/v1beta1
메타데이터:
이름: 카타
처리기: kata
EOF
```

그런 다음 이 새로운 런타임 클래스를 사용하여 팟을 실행합니다.

```undefined
$cat < 큐빅틀 적용 -f -
apiVersion: v1
종류: 포드
메타데이터:
레이블:
앱: 신뢰할 수 없음
이름: www-kata
사양:
runtimeClassName: kata
컨테이너:
- image: nginx:1.18
이름: www
포트:
- 컨테이너 포트: 80
EOF
```

app=dp 레이블에 `팟`을 나열하면 `www-kata`라는 팟이 정상 작동하는 것을 볼 수 있다(이전 팟 `www-gvisor` 참조).

```undefined
$쿠벡틀 get po-l app=contract
이름 준비 상태 재시작 기간
www-gvisor 1/8 8m1s 실행 중
www-kata 1/1 3h25m 달리기
```

후드 아래에서 해당 컨테이너에 대해서만 생성된 전용 가상 시스템(흔히 `microVM`으로 정의됨)에 연결됩니다. 이제 `퀘무` 프로세스가 실행 중인 것도 확인할 수 있습니다.

```undefined
root@worker:~#ps -ef | grepqemu | awk '{print $11}'
/usr/bin/qemu-closed-system-x86_64
```

이는 `qemu`가 작업자 노드에 새 가상 시스템을 생성했음을 나타냅니다. 프로세스는 다음 체인으로 호출됩니다.

# 키 테이크어웨이즈

앞서 살펴본 바와 같이, 추가 컨테이너 실행 시간을 사용하는 것은 복잡할 필요가 없습니다. gVisor와 Kata Containers는 이미 널리 사용되는 두 가지 기술이지만 다른 솔루션도 존재합니다. 이러한 모든 `runc` 대안의 주요 목표는 팟/컨테이너 간의 격리를 개선하여 보안을 강화하는 것이다.