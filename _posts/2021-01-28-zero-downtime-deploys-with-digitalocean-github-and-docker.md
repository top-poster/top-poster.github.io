---
layout: post
title: "DigitalOcean, GitHub 및 Docker로 다운 타임없이 배포
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/Screen-Shot-2021-01-22-at-1.15.13-PM.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Screen-Shot-2021-01-22-at-1.15.13-PM.png?fit=730%2C489&ssl=1)

## 소개
 

DigitalOcean은 개발자에게 애플리케이션을 호스팅 할 수있는 장소를 제공하는 플랫폼입니다.
 그들은 "드롭 릿"이라고 부르는 겸손한 VPS (Virtual Private Server)와로드 밸런서 및 관리 형 데이터베이스와 같은 고급 제품을 모두 제공합니다.
 위의 모든 내용은 이후 섹션에서 설명합니다.
 

이 가이드를 따르려면 DigitalOcean 계정을 만들어야합니다.
 아직 GitHub 계정이 없다면 GitHub 계정도 만들어야합니다.
 저는 Node.js 개발자이기 때문에이 가이드는 기본 Node.js 서비스 (Docker)를 사용하지만 익숙한 플랫폼에 맞게 쉽게 조정할 수 있습니다.
 

## DigitalOcean에서 인프라 구축
 

이 데모가 끝날 때까지 $ 5 / 월 두 개를 생성합니다.
 물방울, $ 10 / 월.
 로드 밸런서 및 무료 컨테이너 레지스트리.
 DigitalOcean은 이러한 제품에 대해 시간 단위로 요금을 부과하므로 모든 것을 구축하고 작동하게되면 즉시 인프라를 해체하고 몇 달러 만 지불하면됩니다.
 

구축 할 인프라를 살펴보십시오.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Digital-Ocean-Infrastructure.png?resize=711%2C191&ssl=1)

모든 작업이 완료되면 저장소의 기본 브랜치를`api-1` 및`api-2` 드롭 릿 모두에 자동으로 배포하는 GitHub 작업이 생성됩니다.
 

일반 빌드에서는 새 코드가 배포됨에 따라 하나의 서비스가 중단되고 상태 확인에서 서비스가 중단되었는지 확인하는 데 걸리는 시간이 0이 아니기 때문에 이로 인해 약간의 중단 시간이 발생합니다.
 그러나이 가이드를 통해 다운 타임이없는 방식으로 배포하는 방법을 배우게됩니다.
 그리고이 예에서는 두 개의 드롭 릿에서 실행되는 서비스를 사용하지만이를 세 개 이상으로 쉽게 확장 할 수 있습니다.
 

## 배포 일정
 

이 섹션에서는 DigitalOcean뿐만 아니라 여러 플랫폼에 적용 할 수있는이 문서에서 다루는 접근 방식에 대한 높은 수준의 설명을 검토합니다.
 예를 들어 HAProxy를로드 밸런서로 사용하여 요청을 하나의 강력한 서버에있는 두 개의 Golang 프로세스로 라우팅하려면 절대 그렇게 할 수 있습니다.
 

다음은 수행 될 작업의 타임 라인입니다.
 공간을 절약하기 위해`api-2` 인스턴스보다`api-1` 인스턴스에 대해 훨씬 더 자세히 살펴 보겠습니다. 두 인스턴스는 동일한 프로세스를 거치게됩니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Digital-Ocen-Deployment-Timeline.png?resize=659%2C391&ssl=1)

위의 그래픽에서 x 축은 시간을 나타내며 왼쪽에서 오른쪽으로 이동합니다.
 배포 프로세스가 처음 시작되면 API 1과 API 2라는 두 개의 서비스 인스턴스가 실행되며 둘 다 코드베이스의 V1을 실행합니다.
 이 문제가 발생하는 동안로드 밸런서는 요청을 수신 할 수 있는지 확인하기 위해 둘 다에 상태 확인을 전송합니다.
 

결국 배포가 발생하여 종료 엔드 포인트가 호출됩니다.
 그때부터 상태 확인이 실패합니다.
 상태 확인이 실패하더라도 서비스는 여전히 요청을 처리 할 수 있으며 여전히 트래픽을 라우팅하고 있습니다.
 두 번의 검사가 실패하면 해당 서버 인스턴스가로드 밸런서에서 제거되고 코드베이스의 V2로 대체 된 후 백업됩니다.
 세 번의 상태 확인을 통과하면로드 밸런서가 요청을 인스턴스로 다시 라우팅하기 시작합니다.
 완료되면 배포 프로세스는 다음 서비스 인스턴스로 계속됩니다.
 

높은 수준에서 위에서 제거해야 할 두 가지 중요한 정보가 있습니다.
 

- 로드 밸런서가 라우팅 할 사용 가능한 인스턴스는 항상 하나 이상 있습니다.
 
- 인스턴스는 요청이 라우팅되는 동안 항상 응답을 제공 할 수 있습니다.
 

이러한 지식을 갖추면 이제 특정 DigitalOcean 가이드로 이동할 준비가되었습니다.
 

## 배포 가이드 : DigitalOcean을 사용하여 다운 타임 제로
 

### 토큰 생성
 

토큰을 사용하면 애플리케이션이 사용자를 대신하여 DigitalOcean API와 상호 작용할 수 있습니다.
 이 예에서는 GitHub 빌드 서버가 Docker 이미지를 컨테이너 레지스트리로 푸시하고 드롭 릿이 컨테이너 레지스트리에서 가져올 수 있도록 사용됩니다.
 

DigitalOcean API 설정 페이지를 방문하여 두 개의 새 토큰을 생성하십시오.
 첫 번째 "GitHub Actions"와 두 번째 "Droplet Registry Pull"의 이름을 지정합니다.
 이 예제에서는 둘 다 읽기 및 쓰기 액세스로 설정할 수 있습니다.
 나중에 필요하므로 이러한 API 토큰을 기록해 둡니다.
 

이러한 토큰은 제 3 자의 비밀로 유지되어야합니다.
 두 개의 토큰을 사용하므로 하나가 손상되면 다른 하나에 영향을주지 않고 삭제할 수 있습니다.
 

### SSH 키 생성
 

SSH를 통해 서버와 통신 할 때 암호를 사용하는 것보다 SSH 키를 사용하는 것이 훨씬 더 안전합니다.
 따라서 이제 드롭 릿에 액세스하기위한 SSH 키 (비밀번호보다 길고 무작위 임)를 생성합니다.
 

SSH 키를 사용하면 수동으로 연결하여 초기 설정을 수행 할 수 있으며 GitHub에서 파일을 드롭 릿으로 전송할 수도 있습니다.
 

SSH 키를 생성하려면 다음 명령을 실행하십시오.
 

```shell
$ ssh-keygen -t rsa -f ~/.ssh/api-droplets
# leave password blank
```

이 명령은 두 개의 키 파일을 생성합니다.
 첫 번째는 ~ / .ssh / api-droplets에 있으며 제 3 자와 공유해서는 안되는 개인 키입니다.
 두 번째 파일은 ~ / .ssh / api-droplets.pub에 있으며 공개 키입니다.
 이것은 당신이 덜 인색 할 수 있습니다.
 

### 물방울 (VPC) 생성
 

DigitalOcean 인터페이스를 사용하여 두 개의 물방울을 만듭니다.
 

이렇게하면 몇 가지 세부 정보를 제공하라는 메시지가 표시됩니다.
 배포의 경우 Debian 10을 선택하고 플랜의 경우 Basic $ 5 / mo를 선택합니다.
 데이터 센터 옵션의 경우 가장 가까운 데이터 센터를 선택하고 나중에 생성하는 부하 분산기가 동일한 데이터 센터에 있는지 확인합니다.
 저는 SFO2를 선택했습니다.
 

인증 섹션에서 새 SSH 키 버튼을 클릭합니다.
 SSH 키에 Droplet SSH Key와 같은 이름을 지정하고 ~`/ .ssh / api-droplets.pub` 파일의 내용을 SSH 키 입력에 붙여 넣은 다음 SSH 키 추가를 클릭합니다.
 생성 할 물방울 수를 2로 설정합니다.
 

호스트 이름은 api-1 및 api-2라고합니다.
 마지막으로 두 방울 모두에 http-api라는 새 태그를 지정합니다.
 이 태그는 나중에로드 밸런서에서 요청을 드롭 릿에 일치시키는 데 사용됩니다.
 

방울을 만든 후에는 다음과 같이 인터페이스에 나열되어야합니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Droplet-Interface-Visual.png?resize=730%2C137&ssl=1)

여기에 나열된 IP 주소는 물방울의 공용 IP 주소입니다.
 이 주소는 인터넷에서 물방울을 고유하게 식별합니다.
 이제 이러한 IP 주소를 사용하여 개발 컴퓨터에서 두 개의 드롭 릿에 SSH를 사용합니다.
 

첫 번째 드롭 릿에 대해 다음 명령을 실행합니다.
 

```shell
$ ssh root@<DROPLET_IP_ADDRESS> -i ~/.ssh/api-droplets
# for first connection, type 'yes' and press enter
```

연결되면 몇 가지 작업을 수행해야합니다.
 

먼저 VPS에 Docker를 설치하십시오.
 이것은 애플리케이션을 캡슐화하고 실행하는 데 사용됩니다.
 또한`doctl` 바이너리를 설치하고 인증해야합니다. 그러면 VPS가 DigitalOcean과 상호 작용할 수 있습니다.
 이 설정을 수행하려면 다음 명령을 실행하십시오.
 

```shell
$ sudo apt install curl xz-utils
# type 'y' and press enter
$ curl -fsSL https://get.docker.com -o get-docker.sh && sh get-docker.sh
$ wget https://github.com/digitalocean/doctl/releases/download/v1.54.0/doctl-1.54.0-linux-amd64.tar.gz
$ tar xf ~/doctl-1.54.0-linux-amd64.tar.gz
$ mv doctl /usr/local/bin/
$ doctl auth init
# Paste the <DROPLET_REGISTRY_PULL_TOKEN> and press enter
$ exit
```

다시 말씀 드리지만, 두 방울 모두에서 이러한 명령 세트를 실행해야하므로 종료 한 후 두 번째 방울의 IP 주소에 대해`ssh` 명령을 다시 실행하십시오.
 이것은 나중에 돌아갈 필요가없는 일회성 부트 스트랩 프로세스입니다.
 

### 로드 밸런서 생성
 

이제 방울을 만들었으므로 다음 단계에서는 DigitalOcean UI를 사용하여로드 밸런서를 만들 수 있습니다.
 이 데모에서는 Small $ 10 / mo 옵션이 좋습니다.
 앞서 언급했듯이 두 방울을 만든 동일한 영역에 있는지 확인하십시오.
 

“Add Droplets”필드에`http-api` 태그를 입력하고 결과를 클릭하여 물방울을 동적으로 일치시킵니다.
 이 프로젝트에는 포트 80에서 포트 80으로 HTTP를 전달하는 옵션이면 충분합니다.
 

고급 설정을 편집하여 상태 확인 엔드 포인트를 구성하십시오.
 일반적으로 부하 분산기는 / 엔드 포인트에 요청을하지만이 프로젝트에는 상태 확인 전용 엔드 포인트가 필요합니다.
 

이 전용 엔드 포인트를 설정하려면 `경로`를 `/ health`로 변경하고 `비정상 임계 값`을 2로 설정하고 `정상 임계 값`을 3으로 설정합니다. 이제 구성이 다음과 같이 표시됩니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Load-Balancer-DigitalOcean-UI.png?resize=705%2C160&ssl=1)

로드 밸런서의 이름을 기억하기 쉽고 기억하기 쉬운 이름으로 지정하십시오.
 제 경우에는`sfo2-api`를 선택했습니다.
 

로드 밸런서를 저장하면 UI에 나열됩니다.
 내로드 밸런서는 다음과 같습니다 (일치하는 물방울 2 개 중 0 개는 서버가 실행되고 있지 않기 때문에 정상입니다).
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Load-Balancer-Visual-Healthy-Droplets.png?resize=730%2C88&ssl=1)

물방울의 경우와 마찬가지로 IP 주소는로드 밸런서를 식별하는 고유 한 IP 주소입니다.
 이 시점에서 개발 머신에서로드 밸런서에 요청하여 작동하는지 확인할 수 있습니다.
 터미널에서 다음 명령을 실행하십시오.
 

```undefined
$ curl -v http://<LOAD_BALANCER_IP_ADDRESS>/
```

이렇게하면 "이 요청을 처리하는 데 사용할 수있는 서버가 없습니다"라는 응답 본문과 함께 HTTP`503 Service Unavailable `오류가 반환됩니다.
 이것은 예상됩니다.
 이 시점에서는 정상적인 서버가 없습니다.
 

### 컨테이너 레지스트리 만들기
 

다음으로 DigitalOcean UI를 사용하여 컨테이너 레지스트리를 생성합니다.
 Docker 이미지가 저장되는 곳입니다.
 

기본적으로 무료 저장 용량은 500MB로 제한되며이 실험에 충분합니다.
 대규모 프로젝트의 경우이 숫자를 상당히 빠르게 초과 할 수 있습니다.
 실제로이 프로젝트의 첫 번째 배포는 약 300MB의 스토리지를 사용하지만 추가 배포는 몇 메가 바이트 만 추가합니다.
 

레지스트리를 만들 때 고유 한 이름을 지정해야합니다.
 이 예에서는 `foo`라는 이름을 선택했지만 모든 DigitalOcean 고객에서 전 세계적으로 고유 한 이름을 선택해야합니다.
 

### GitHub 저장소 만들기
 

DigitalOcean으로 다운 타임없는 배포를 계속 설정하기 위해 GitHub UI를 사용하여 새 리포지토리를 만듭니다.
 

저장소를 가리 키도록 로컬 디렉토리를 구성하십시오.
 `master`대신 새로운 `main`분기 규칙을 사용해야합니다.
 GitHub 새 리포지토리 화면은이를 수행하는 데 필요한 모든 명령을 제공합니다.
 

완료되면 다음 파일을 저장소에 추가합니다.
 

### `.github / workflows / main-deploy.yml`
 

GitHub 작업은`.github / workflows /`디렉터리를 사용하여 프로젝트에서 사용할 다양한 작업에 대한 설명을 찾습니다.
 예를 들어, linter 및 일부 테스트 실행과 같이 Pull Request가 생성 될 때 수행 할 작업을 설명하는 파일이있을 수 있습니다.
 

이 경우 코드가 메인 브랜치에 병합 될 때 배포 프로세스를 설명하기 위해 단일 파일 만 필요합니다.
 다음 파일을 템플릿으로 사용하여`<REGISTRY_NAME>`을 내가 사용했던`foo` 값과 같이 DigitalOcean 레지스트리의 이름으로 바꾸고 싶다는 점에 유의하십시오.
 

```bash
name: Deploy to Production
on:
  push:
    branches:
      - main
  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Check Out Repo 
        uses: actions/checkout@v2
      - name: Install DigitalOcean Controller
        uses: digitalocean/action-doctl@v2
        with:
          token: ${ secrets.DIGITALOCEAN_ACCESS_TOKEN }
      - name: Set up Docker Builder
        uses: docker/setup-buildx-action@v1
      - name: Authenticate with DigitalOcean Container Registry
        run: doctl registry login --expiry-seconds 180
      - name: Build and Push to DigitalOcean Container Registry
        uses: docker/build-push-action@v2
        with:
          context: .
          push: true
          tags: |
            registry.digitalocean.com/<REGISTRY_NAME>/api:latest
            registry.digitalocean.com/<REGISTRY_NAME>/api:sha-${ github.sha }

  deploy-api-1:
    needs: build
    runs-on: ubuntu-latest
    steps:
      # Droplets already have docker, doctl + auth, and curl installed
      - name: Deploy api to DigitalOcean Droplet
        uses: appleboy/ssh-action@v0.1.4
        with:
          host: ${ secrets.DO_API1_HOST }
          username: root
          key: ${ secrets.DO_API_KEY }
          port: 22
          script: |
            doctl registry login --expiry-seconds 180
            docker pull registry.digitalocean.com/<REGISTRY_NAME>/api:latest

            echo "calling shutdown endpoint..."
            curl --silent http://localhost/shutdown || true

            echo "giving healthcheck time to fail..."
            sleep 30 # ((unhealthy + 1) * interval)

            docker stop api || true
            docker rm api || true

            echo "starting server instance..."
            docker run -d \
              --restart always \
              -p 0.0.0.0:80:80 \
              --name api \
              registry.digitalocean.com/<REGISTRY_NAME>/api:latest

            echo "giving healthcheck time to recover..."
            sleep 40 # ((healthy + 1) * interval)

            curl --silent --fail http://localhost/health

  deploy-api-2:
    needs: deploy-api-1 # rolling deploy
    runs-on: ubuntu-latest
    steps:
      # Droplets already have docker, doctl + auth, and curl installed
      - name: Deploy api to DigitalOcean Droplet
        uses: appleboy/ssh-action@v0.1.4
        with:
          host: ${ secrets.DO_API2_HOST }
          username: root
          key: ${ secrets.DO_API_KEY }
          port: 22
          script: |
            doctl registry login --expiry-seconds 180
            docker pull registry.digitalocean.com/<REGISTRY_NAME>/api:latest

            echo "calling shutdown endpoint..."
            curl --silent http://localhost/shutdown || true

            echo "giving healthcheck time to fail..."
            sleep 30 # ((unhealthy + 1) * interval)

            docker stop api || true
            docker rm api || true

            echo "starting server instance..."
            docker run -d \
              --restart always \
              -p 0.0.0.0:80:80 \
              --name api \
              registry.digitalocean.com/<REGISTRY_NAME>/api:latest

            echo "giving healthcheck time to recover..."
            sleep 40 # ((healthy + 1) * interval)

            curl --silent --fail http://localhost/health
```

이 파일에는 세 가지 작업이 포함되어 있습니다.
 첫 번째는 Ubuntu 가상 머신 내부에 도커 컨테이너를 빌드하는`build`입니다.
 또한 컨테이너에 태그를 지정하고 컨테이너 레지스트리로 푸시합니다.
 

`deploy-api-1` 및`deploy-api-2` 작업은 Ubuntu 가상 머신에서도 실행되지만 SSH를 통해 모든 작업을 수행합니다.
 특히 드롭 릿에 연결하고 새 도커 이미지를 가져오고 서비스를 종료하도록 지시하고 상태 확인이 실패 할 때까지 기다립니다.
 그 후 이전 컨테이너가 제거되고 새 이미지를 기반으로하는 새 컨테이너가 시작됩니다.
 

새 컨테이너가 시작되면 새 상태 확인이 실행됩니다.
 안전을 위해 상태 확인 엔드 포인트도 호출됩니다.
 이렇게하면 호출이 실패하면 작업이 실패하고 후속 배포가 수행되지 않습니다.
 

물론이 파일의 눈에 띄는 문제는 각 배포의 전체 콘텐츠를 복사하여 붙여넣고이를 구성 / 재사용 가능한 GitHub 작업으로 변환 할 수 있다는 것입니다. 이는 다른 날을위한 가이드입니다.
 

## 관련 파일 설명
 

### `Dockerfile`
 

이 파일은 Docker 이미지를 빌드하는 방법을 설명합니다.
 제작 준비가 된 것은 아니지만,이 예제에서는 충분합니다.
 

```undefined
FROM node:14

EXPOSE 80

WORKDIR /srv/api
ADD . /srv/api

RUN npm install --production

CMD ["node", "api.mjs"]
```

이 이미지는 Node.js 14 LTS 라인을 기반으로합니다.
 내부 서비스가 포트 80에서 수신함을 암시합니다. 애플리케이션 코드는 이미지 내의 / srv / api / 디렉토리에 복사됩니다.
 그런 다음 최종적으로 api.mjs 파일을 실행하기 전에 프로덕션 설치를 수행합니다.
 

### `.dockerignore`
 

이 파일에는 이미지로 복사하면 안되는 파일과 디렉토리가 나열됩니다.
 

```css
.git
.gitignore
node_modules
npm-debug.log
test
```

여기서 가장 중요한 라인은`node_modules /`디렉토리입니다.
 이러한 파일은 OS에서 복사되지 않고 이미지 빌드 프로세스 중에 생성되어야하기 때문에 중요합니다.
 

### `.gitignore`
 

이 파일은 주로`node_modules /`가 커밋되지 않도록하기위한 것입니다.
 

```undefined
node_modules
npm-debug.log
```

### `api.mjs`
 

이 파일은로드 밸런서 뒤에서 사용할 수있는 매우 간단한 API를 나타내며 서비스의 진입 점입니다.
 

```js
#!/usr/bin/env node

import fastify from 'fastify';
const server = fastify();
let die = false;
const id = Math.floor(Math.random()*1000);

server.get('/', async () => ({ api: 'happy response', id }));

server.get('/health', async (_req, reply) => {
  if (die) {
    reply.code(503).send({ status: 'shutdown' });
  } else {
    reply.code(200).send({ status: 'ok' });
  }
});

server.get('/shutdown', async () => {
  die = true;
  return { shutdown: true };
});

const address = await server.listen(80, '0.0.0.0');
console.log(`listening on ${address}`);
```

`GET /`경로는 주로 식별자 역할을하는 임의의 숫자를 생성하여 서비스를 실행할 수 있음을 보여줍니다.
 이 숫자는 인스턴스 수명 내내 일관되게 유지됩니다.
 

`GET / health`는로드 밸런서가 애플리케이션이 정상이고 요청을 수신 할 수 있는지 알기 위해 사용하는 것입니다.
 `GET / shutdown`은`die` 변수를`true`로 설정합니다.
 그런 일이 발생하면`GET / health`에 대한 후속 요청은 이제 불행한`503` 상태 코드를 반환합니다.
 이것이로드 밸런서에서 서비스를 삭제해야한다고 우아하게 선언 할 수있는 메커니즘입니다.
 

### `package.json 및 package-lock.json`
 

이 두 파일은 다음 명령을 실행하여 생성 할 수 있습니다.
 

```shell
$ npm init -y
$ npm install fastify@3
```

이렇게하면`node_modules /`디렉토리가 생성되고 두 개의 패키지 파일이 생성됩니다.
 이러한 패키지 파일은 나중에 Docker 빌드 프로세스 중에 npmjs.com 패키지 저장소에서 필요한 패키지 파일을 다운로드하는 데 사용됩니다.
 

## GitHub 프로젝트 비밀
 

배포를 실행하려면 일부 GitHub 프로젝트 비밀도 만들어야합니다.
 GitHub 작업 YAML 파일에서 사용할 수있는 변수입니다.
 

프로젝트 비밀을 만들려면 GitHub 프로젝트의 설정 탭으로 이동하여 4 개의 항목을 추가합니다.
 

첫 번째 항목은`DIGITALOCEAN_ACCESS_TOKEN`입니다.
 이는 이전 단계에서 생성 한 GitHub 작업 액세스 토큰의 값입니다.
 

두 번째 항목은`DO_API_KEY`입니다.
 이것은 이전에 생성 한`~ / .ssh / api-droplets` 개인 키 파일의 내용입니다.
 줄 바꿈이 유지되도록 콘텐츠를 붙여 넣을 때주의하십시오.
 

마지막으로`DO_API1_HOST` 및`DO_API2_HOST`의 두 항목을 추가합니다.
 둘 다 생성 한 두 API 드롭 릿의 IP 주소를 포함합니다.
 이제 비밀 화면이 다음과 같이 보일 것입니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/GitHub-Project-Secrets-Screen.png?resize=721%2C312&ssl=1)

이 네 가지 비밀 이름은 모두 이전에 생성 한 GitHub 작업 YAML 파일에서 참조됩니다.
 

## 첫 번째 배포 실행
 

첫 번째 배포를 실행하려면 다음 단계를 따르세요.
 

- 풀 요청을 만들고 병합하거나 기본 브랜치에 직접 추가하고 푸시하여 파일 변경 사항을 GitHub 기본 브랜치에 병합합니다.
 완료되면 배포 프로세스가 시작됩니다.
 
- GitHub 저장소에서 작업 탭을 확인합니다.
 메인 브랜치로의 코드 병합과 관련된 활성 액션이 실행되는 것을 볼 수 있습니다.
 자세한 정보를 보려면 클릭하십시오.
 내 화면에서 다음과 같이 보입니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/First-Deploy-Process-Output.png?resize=730%2C359&ssl=1)

### 문제 해결
 verified_user

프로세스의이 단계에서 오류가 발생하면 이전 단계를 수정해야 할 수 있습니다.
 

텍스트로 변환 한 코드에 문제가있는 경우 수정 한 후 다시 기본 브랜치에 커밋합니다.
 그러면 다른 빌드가 자동으로 시작됩니다.
 

GitHub 보안 비밀을 변경해야하는 경우 GitHub UI를 사용하여 변경하세요. 이렇게하면 다른 배포가 시작되지 않는다는 점만 알아 두세요.
 대신 Actions 탭을 다시 방문하여 왼쪽의 "Deploy to Production"버튼을 클릭하고 오른쪽의 "Run workflow"드롭 다운을 사용하여 기본 브랜치에서 빌드를 다시 시작하십시오.
 

이 예에서는 `build`가 성공적으로 완료된 후 2 단계에서 `api-1`이 배포 된 것을 볼 수 있습니다.
 `api-2`를 배포하는 다음 단계는 `api-1`이 완료되기를 기다리고 있기 때문에 아직 수행되지 않았습니다.
 배포가 실패하면 `api-2`가 배포되지 않습니다.
 이렇게하면 문제를 수정하고 수정 사항을 배포 할 시간이 생깁니다.
 또한 이러한 단계 중 하나라도 실패하면 해당 단계를 클릭하여 자세한 정보를 얻을 수 있습니다.
 

### 애플리케이션 상태 모니터링
 

로드 밸런서에 대한 DigitalOcean 그래프는 시간 경과에 따른 애플리케이션의 상태를 표시하며 내 경험상 1 분마다 애플리케이션의 상태를 폴링합니다.
 

타이밍에 따라 하나의 서비스가 작동 중지되었다가 작동하고 다른 하나가 작동 중지되었다가 작동하는 것을 볼 수 있습니다.
 첫 번째 변경 사항을 배포 한 후 몇 분을 기다렸다가 다른 배포를 트리거하면 DigitalOcean 그래프에서 효과를 볼 수 있습니다.
 

제 경우는 다음과 같습니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Monitor-Application-Health-DigitalOcean-Graph.png?resize=730%2C188&ssl=1)

다운 타임 그래프는 다운 타임이있는 `app-1`(녹색)을 명확하게 보여줍니다.
 다른 `app-2`(갈색)는 적절한 순간에 폴링되지 않아 그래프가 급증했습니다.
 상태 확인 그래프는`app-2`가 약간 영향을 받았다는 것을 보여줍니다.
 

`빌드`단계는 Docker 이미지를 컨테이너 저장소로 푸시합니다.
 이런 일이 발생할 때마다 이미지에 두 번 태그가 지정됩니다.
 한 번은`latest` 태그를 포함하고 다른 하나는 빌드가 발생했을 때 메인 브랜치의 git 커밋 해시를 포함합니다.
 

다음은 두 가지 빌드를 수행 한 후 내 컨테이너 레지스트리의 모습입니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Container-Registry-Two-Builds.png?resize=730%2C292&ssl=1)

`latest` 태그는 각 빌드로 대체됩니다.
 Docker 이미지를 프로덕션에 배포하는 데 사용되는 태그입니다.
 커밋 해시를 사용하는 태그는 작동 중임을 표시하는 데 편리합니다.
 보다 강력한 시스템은이를 사용하여 이전 커밋으로 배포를 롤백 할 수 있습니다.
 

## 부하 분산 된 요청 만들기
 

프로젝트의이 시점에서 이제 코드가 기본 브랜치에 병합 될 때 프로덕션에 자동으로 배포되는 서비스가 있습니다.
 무엇보다도 향후 배포시 다운 타임이 없어야합니다.
 

이제 애플리케이션이 중복 방식으로 실행되고 있음을 증명할 준비가되었습니다.
 다음 명령을 몇 번 실행하면됩니다.
 

```shell
$ curl http://<LOAD_BALANCER_IP_ADDRESS>/
# {"api":"happy response","id":930}
$ curl http://<LOAD_BALANCER_IP_ADDRESS>/
# {"api":"happy response","id":254}
```

응답에서 두 개의 서로 다른 `id`값이 반환되는 것을 볼 수 있습니다.
 요청할 때마다 반환 된 ID가 번갈아 나타납니다.
 이는로드 밸런서가 기본적으로 "라운드 로빈"알고리즘을 사용하여 요청을 라우팅하도록 구성되어 있기 때문입니다.
 

서버 중 하나가 충돌하면 순환에서 제거됩니다.
 상태 확인 구성에서로드 밸런서가 인스턴스 중 하나가 다운되었음을 인식하는 데 11 ~ 20 초가 걸릴 수 있습니다.
 이 시간 동안로드 밸런서로 전송되는 요청의 50 %가 실패합니다.
 보다 적극적인 상태 확인은이 시간을 단축 할 수 있지만 장애에 대해 100 % 탄력적 인 시스템을 구축하는 것은 어렵습니다.
 

물론 IP 주소를 전달하는 것이 그렇게 편리하지는 않지만 도메인이 IP 주소를 가리 키도록 DNS 설정을 구성 할 수 있습니다.
 다시, 다른 날을위한 또 다른 가이드.
 

## 생산 화
 

모든 것을 고려할 때이 가이드는 다운 타임없는 배포를 달성하는 방법을 보여주기위한 매우 간단한 가이드입니다.
 특히 보안과 관련하여 많은 중요한 세부 사항을 설명합니다.
 포괄적이지 않고 인프라를보다 안전하게 만들기 위해 취해야 할 몇 가지 추가 단계는 다음과 같습니다.
 

- 포트`: 80`에서 종료 엔드 포인트를 노출하지 마세요.
 대신`127.0.0.1` (로컬 인터페이스)의 다른 포트에서만 수신합니다.
 현재 누구나`http : // <LOAD_BALANCER_IP> / shutdown`을 호출하여 드롭 릿을 비활성화 할 수 있습니다.
 
- `healthcheck` 엔드 포인트의 이름을 추측하기 더 어려운 이름으로 바꿉니다.
 
- 실제 앱의 경우로드 밸런서의 HTTPS 요청을 API의 HTTP로 전달합니다.
 
- 물방울에 루트가 아닌 계정 사용
 

마지막으로 API 서비스는 `0.0.0.0`(모든 인터페이스)에서 수신하므로 클라이언트는 Droplet IP를 직접 요청하여로드 밸런서를 우회 할 수 있습니다.
 각 드롭 릿은 두 개의 네트워크 인터페이스 (공용 및 개인용)를 노출하고 Node.js 서비스는로드 밸런서가 도달 할 수있는 개인 인터페이스에서 수신해야합니다.
 

## 결론
 

일반 빌드에서 배포는 일반적으로 어느 정도의 다운 타임을 발생시킵니다.
 이 가이드에서는 DigitalOcean, GitHub 및 Docker를 사용하여 다운 타임을 없애고 2 개 이상의 드롭 릿에서 실행되는 서비스에 대해 확장 가능한 방식으로 배포하는 방법을 검토했습니다.
 