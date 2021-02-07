---
layout: post
title: "DigitalOcean에 AdonisJs 애플리케이션을 배포하는 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/digital-ocean.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/digital-ocean.png?fit=730%2C487&ssl=1)

속도와 안정성에 초점을 맞춘 AdonisJs 프레임워크는 Node.js 생태계의 다른 프레임워크에 대한 대안으로 만들어졌다.

통합 시스템의 힘으로, 개발자의 인체공학은 결합 라이브러리보다 훨씬 더 큰 목표를 달성한다는 사실에 기초한다.

DigitalOcean은 애플리케이션을 호스팅하고 관리할 수 있는 온디맨드 클라우드 플랫폼을 제공하는 클라우드 인프라 공급자입니다.이 튜토리얼에서는 서버를 설정하고 DigitalOcean에서 데이터베이스를 구성하는 단계를 설명합니다. 또한 DigitalOcean에 배포할 AdonisJs에 데모 애플리케이션을 만들었습니다.

다른 옵션은 Digital Ocean에서 사용할 수 있습니다. 언제든지 확인하거나 사용하십시오. 곧 출시될 개발자들을 위해, 우리는 이 기술 스택을 사용할 것입니다.

### 전제조건

본 자습서를 진행하기 전에 다음 사항이 있어야 합니다.

- Node.js의 기본 이해(특히 AdonisJs)
- 깃의 기본 지식
- npm 또는 Yarn이 설치되어 있습니다.
- 디지털 오션 계정

### 드롭릿 만들기

물방울은 서버입니다. Digital Ocean 계정에 로그인할 때 먼저 서버를 생성해야 합니다. 화면 상단에 있는 Create(만들기) 버튼을 클릭하면 Droplet이 생성됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/droplets-create.png?resize=730%2C252&ssl=1)

분포에서 이미지를 선택합니다. 메일 그룹 아래에 여러 가지 옵션이 있습니다. 그래서 우리는 Ubuntu 20.04 (LTS)가 일반적이기 때문에 함께 갈 것입니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/create-doplets.png?resize=730%2C314&ssl=1)

그런 다음 계획을 선택합니다. 우리는 아주 기초적인 애플리케이션을 배치할 것이기 때문에 가장 작은 계획을 선택하는 것이 좋습니다. 필요할 경우 언제든지 나중에 확장할 수 있습니다. 미래에는 메모리를 줄이는 것이 불가능하다는 것을 명심하세요.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/Choosing-plan.png?resize=730%2C309&ssl=1)

데이터 센터 영역을 선택합니다. 가까운 곳이나 사용자가 주로 살 것 같은 곳을 선택하세요.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/datacenter-region.png?resize=730%2C242&ssl=1)

다음으로, 우리는 우리의 드롭릿에 SSH 키를 추가해야 할 것이다. 이것은 매우 중요한 부분이고 혼란의 여지가 있다. SSH를 사용하면 컴퓨터에서 원격 서버(Digital Ocean)에 직접 연결할 수 있습니다.

이미 SSH 키를 DigitalOcean에 추가한 경우 다음을 선택할 수 있습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/authentication-page.png?resize=730%2C177&ssl=1)

추가되지 않은 경우 하나를 생성해야 합니다. SSH 키를 복사하여 붙여 넣고 이름을 지정합니다. Github의 가이드를 사용하여 하나를 생성하고 우리의 드롭릿에 복사할 수 있습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/add-public-key.png?resize=730%2C388&ssl=1)

임의의 이름을 생성하지 않도록 호스트 이름을 변경하여 완료하고 "Droplet 생성"을 클릭합니다.

### 서버 구성

다음으로, SSH를 서버에 추가하여 계속 진행하겠습니다. 하지만 관리 권한이 필요하므로 새 사용자를 생성하고 권한을 부여하겠습니다.

서버의 IP 주소로 루트로 로그인:

```css
ssh root@105.112.72.138
```

지금 사용자를 추가하고 사용한 암호를 기록해 두십시오.

```undefined
adduser dominic
```

이제 이 사용자를 sudo 그룹에 추가하여 sudo라는 키워드로 `root` 명령을 실행할 수 있도록 합니다.

자, 이걸 실행:

```undefined
usermod -aG sudo dominic
```

이제 관리자 권한으로 sudo 그룹에 도메인을 추가했으므로 서버에 로그인할 수 있도록 사용자에게 SSH 키도 추가하려고 합니다.

```undefined
su - dominic
```

다음으로, `.ssh`라는 디렉토리를 생성하고, SSH 키가 저장될 파일 `authorized_keys`를 생성하고, 디렉토리의 권한을 변경하여 액세스 권한을 숨깁니다.

```undefined
mkdir ~/.ssh touch .ssh/authorized_keys chmod 700 .ssh
```

모든 사람이 읽을 수 있고 사용자 `도미닌크`만 변경할 수 있도록 파일 권한도 변경해야 합니다.

다음으로 가장 중요한 단계는 복사한 SSH 키를 이 파일에 붙여넣는 것입니다. 복사하지 않은 경우 컴퓨터로 돌아가서 복사하십시오.

파일을 열고 내용을 파일에 붙여넣습니다. 파일을 저장하고 `esc`를 클릭하여 편집을 중지하고, `:wq`를 클릭하여 쓰기(저장)하고 `Enter`를 누릅니다.

지금 파일 제한 및 종료:

```undefined
chmod 600 ~/.ssh/authorized_keys exit
```

SSH를 서버로 되돌리지만 새로 생성된 사용자는 SSH를 사용할 수 있습니다. 만약 그것이 효과가 있다면, 우리는 다음 단계로 넘어갈 준비가 되어 있습니다.

```css
ssh dominic@105.112.72.138
```

### MYSQL 설치 및 설정

데모 응용 프로그램을 보니 데이터베이스용 MYSQL을 사용했기 때문에 서버에 설치하고 구성해야 할 것 같습니다.

```undefined
sudo apt install mysql-server sudo mysql_server_installation
```

다음으로, 우리는 MYSQL 서버에 로그인하고 새로운 사용자와 데이터베이스를 만들어야 합니다.

```undefined
mysql -u root -p
```

USERname, localhost 및 PASSWORD를 대체합니다.

```undefined
CREATE USER 'USERNAME'@'localhost' IDENTIFIED BY 'PASSWORD';
```

다음 명령을 실행하여 MySQL을 연결할 때 암호를 사용할 수 있습니다.

```undefined
ALTER USER 'USERNAME'@'LOCALHOST'IDENTIFIED WITH mysql_native_password BY 'PASSWORD';
```

그런 다음 관련 사용자를 사용하여 데이터베이스를 생성합니다.

```undefined
CREATE DATABASE adonis_userAuth;
```

그런 다음 모든 권한을 부여하고 플러시하여 작업을 수행하고 다음을 종료합니다.

```undefined
GRANT ALL ON adonis_userAuth.* TO 'USERNAME'@'localhost'; FLUSH PRIVILEGES; exit
```

### 서버에서 애플리케이션 복제

우리의 마지막 작업은 Git을 사용하여 Github에서 서버로 AdonisJs 애플리케이션을 복제하는 것이다. SSH를 서버에 다시 넣고 리포트를 복제해 봅시다. 언제든지 SSL을 사용하여 복제할 수 있습니다.

```cpp
https://github.com/Vectormike/adonis_blogApi
```

복제된 디렉토리로 이동하여 종속성을 설치합니다.

```bash
cd adonis_blogApi npm install
```

파일에 아직 해결해야 할 환경 변수가 있습니다. AdonisJs는 `.env` 파일을 통해 모든 변수를 읽습니다. 이러한 변수는 구성 설정입니다.

AdonisJs 응용 프로그램에 대한 변수를 포함할 수 있도록 해당 파일 `.env`를 생성합니다.

```css
touch .env
```

생성되면 애플리케이션 키 `APP_KEY`를 생성합니다. 이 프로그램이 없으면 응용 프로그램이 실행되지 않습니다.

```css
adonis key:generate
```

이렇게 하면 일부 난수가 생성됩니다. 응용 프로그램 키를 포함한 모든 변수를 붙여넣을 수 있도록 키를 복사하고 `.env` 파일을 엽니다.

```css
vim .env
```

그런 다음 다음 변수를 붙여넣습니다.

```undefined
HOST=127.0.0.1
PORT=3333
NODE_ENV=production
APP_NAME=AdonisJs
APP_URL=http://${HOST}:${PORT}
CACHE_VIEWS=false
APP_KEY=mAqOZJTaXhbcvxPGVckSyUI1UeCeYIUM
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_USER=root
DB_PASSWORD=
DB_DATABASE=adonis_userAuth
HASH_DRIVER=bcrypt
```

테이블이 생성되려면 마이그레이션을 실행해야 합니다. 사용자 환경이 프로덕션 상태인 경우 "--force 플래그"를 추가해야 할 수 있습니다. 그렇지 않으면 테이블이 생성되지 않습니다.

```css
node ace migration:run --force
```

마지막으로 응용 프로그램을 실행하기 시작합니다. 하지만 만약 충돌한다면요? 그렇기 때문에 PM2가 충돌할 때 자동으로 다시 시작하도록 해야 합니다.

`NODE_ENV`가 프로덕션인 경우 이 명령을 실행합니다.

```css
pm2 start server.js
```

### Nginx 구성

역방향 프록시를 설정하려면 Nginx를 사용해야 합니다. 이렇게 하면 로컬 서버 네트워크를 통해 방문할 수 있는 것이 아니라 외부에서 애플리케이션에 연결할 수 있습니다. 이렇게 하면 우리는 단지 `PORT`가 아닌 IP 주소나 도메인 이름을 통해 접속할 수 있다.

따라서 Nginx 구성 파일을 열고 다음을 편집합니다.

```coffeescript
sudo vim /etc/nginx/sites-available/default
```

아래에 구성을 붙여넣습니다. 이렇게 하면 Nginx가 수신 도메인과 관련된 수신 도메인을 수신하고 로컬 서버 네트워크의 `PORT`로 모든 요청을 전달하도록 지시합니다.

```undefined
server_name DOMAIN_NAME_OR_IP_ADDRESS;

location / {
    proxy_pass http://localhost:3333;
    proxy_http_version 1.1;
    proxy_set_header Connection "upgrade";
    proxy_set_header Host $host;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
```

`server_name`을(를) IP 주소 또는 도메인 이름(설정된 경우)으로 바꿉니다. 변경 사항이 적용되려면 Nginx를 다시 시작해야 합니다.

```undefined
sudo service nginx restart
```

이제 서버의 IP를 눌러 애플리케이션에 액세스할 수 있습니다.

## 결론

이 튜토리얼에서 우리는 AdonisJs 애플리케이션을 DigitalOcean에 배포하는 방법에 대해 배웠다. 또한 Nginx를 구성하여 역방향 프록시를 설정하고, Droplet을 구성하고, DigitalOcean에 데이터베이스를 설정하는 방법도 배웠습니다.

이 문서에서는 애플리케이션을 구축하는 데 필요한 각 단계를 설명했어야 합니다.

프로세스에 자동 배포 스크립트를 추가할 수 있습니다. 이렇게 하면 리포지토리에서 최근 코드를 가져오고 종속성을 설치하고 마이그레이션을 실행하고 응용 프로그램을 다시 시작합니다.

DigitalOcean에 대해 자세히 알아보려면 DigitalOcean으로 이동하여 자세한 튜토리얼을 받으십시오.

이 튜토리얼의 데모로 사용되는 소스 코드는 Github에 있다. 얼마든지 복제하세요.