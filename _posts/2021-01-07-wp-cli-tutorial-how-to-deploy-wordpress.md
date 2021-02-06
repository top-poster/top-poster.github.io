---
layout: post
title: "WP-CLI 자습서: WordPress를 배포하는 방법"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/wordpress-cli-tutorial.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/wordpress-cli-tutorial.png?fit=730%2C487&ssl=1)

워드프레스는 현재 세계에서 가장 인기 있는 콘텐츠 관리 시스템이며 모든 웹사이트의 39%가 사용하고 있다. 자체 컨텐츠를 관리하고자 하는 고객을 위한 훌륭한 솔루션입니다. 개발자는 설치 및 설정을 담당합니다.

Softaculous 스크립트와 같은 도구를 사용하면 기본 설치가 훨씬 쉬워지지만 여전히 명령줄에서 훨씬 빠른 지루한 설치 후 작업이 많이 있습니다. 명령줄에 입력한 모든 항목을 스크립트로 변환할 수 있으므로 모든 작업을 자동화할 수 있습니다. 기본 사항을 익히면 특히 정기적으로 WordPress를 설치하고 설정해야 하는 경우 많은 시간을 절약할 수 있습니다.

이 튜토리얼에서는 WP-CLI를 사용하여 원격 공유 서버에 WordPress를 설치하고 설정하는 방법에 대해 설명합니다. Linux 및 서버에 대한 지식이 부족한 프런트엔드 개발자에게 주로 적합합니다.

다음 사항에 대해 자세히 설명합니다.

- WP-CLI란?
- SSH란 무엇입니까?
- OpenSSH란 무엇입니까?
- SSH를 사용하여 서버 로그인
- 서버에 WP-CLI를 설치하는 방법
- WordPress 설치
- WordPress 설치 후 설정
- WP-CLI 시간 절약 명령

계속 진행하려면 다음이 필요합니다.

- SSH 액세스와 사용자 계정 및 암호가 있는 서버 - 루트 액세스 필요 없음
- 로컬 컴퓨터에 있는 SSH 보안 셸 소프트웨어입니다. 이렇게 하면 서버에 안전하게 로그인하고 명령을 실행할 수 있습니다.
- cPanel은 사용하기 좋은 제품입니다.

## WP-CLI란?

WP-CLI는 WordPress의 공식 명령줄 인터페이스이다. 웹 브라우저를 사용하지 않고도 명령줄 내에서 플러그인 업데이트, 다중 사이트 설치 구성 등과 같은 수많은 WordPress 개발 태스크를 수행할 수 있습니다.

WP-CLI의 주요 이점은 명령행을 벗어나지 않고 몇 줄의 코드만 필요한 간단한 작업을 수행할 수 있기 때문에 시간을 절약할 수 있다는 것이다. 이렇게 하면 사이트에 로그인하지 않고도 WordPress Admin 패널에서 많은 기능에 액세스할 수 있기 때문에 효율성을 높일 수 있습니다.

WP-CLI 핸드북에는 참조 가이드, 튜토리얼 및 도구를 사용하는 데 필요한 모든 정보가 포함되어 있습니다.

## SSH란 무엇입니까?

SSH는 암호화를 사용하여 사용자가 원격 서버에 로그인하고 명령을 안전하게 실행할 수 있도록 합니다. 비밀번호만 사용하여 로그온할 수 있지만 공용 및 개인 키를 사용하는 것이 가장 좋습니다. 개인 키는 컴퓨터에 저장되고 공용 키는 서버에만 저장됩니다.

![image](https://blog.logrocket.com/wp-cli-tutorial-how-to-deploy-wordpress/exported-images/4d07feea1d655f27f0ec66e2051af854eeb82abd.svg)

이 설정을 마치 자신의 로컬 컴퓨터인 것처럼 터미널을 통해 서버에 액세스할 수 있습니다.

## OpenSSH란 무엇입니까?

OpenSSH는 대부분의 Linux 배포판, macOS 및 Windows 10과 함께 제공되는 SSH 프로토콜의 오픈 소스 구현체이다.

로컬 컴퓨터에 OpenSSH가 설치되어 있는지 확인하려면 `ssh-V`를 입력하십시오. 다음과 유사한 결과를 얻을 수 있습니다.

```css
OpenSSH_8.0p1, OpenSSL 1.1.1c FIPS  28 May 2019
```

이 버전은 오래된 것 같지만 보안 백포트가 업데이트되었습니다.

## SSH를 사용하여 서버 로그인

SSH를 사용하려면 서버에서 다음 정보를 얻어야 합니다.

- `호스트 이름` — 네트워크에서 서버를 식별하는 데 사용되는 이름
- `사용자` — 서버의 사용자 이름
- `포트` — 통신 프로토콜 유형과 연결된 서버의 주소(이 경우 SSH)
- 정체성파일` — 서버의 공용 키와 일치하는 개인 키

우리는 단순성을 위해 서버가 우리가 사용할 공개 및 개인 키를 가지고 있다고 가정하고 있습니다. 로컬 컴퓨터에 만들어지고 공용 컴퓨터가 서버에 업로드되는 경우가 많습니다.

널리 사용되는 cPanel GUI를 사용하여 필요한 정보를 찾을 수 있습니다. cPanel이 없는 경우 호스팅 공급자에게 문의하여 필요한 정보를 얻어야 합니다. 다음 사항을 지원 티켓으로 보내는 것이 좋습니다.

> SSH를 사용하여 서버에 로그온하고 '호스트 이름', '사용자', '포트', '아이덴티티' 등의 정보가 필요합니다.파일 ''(공용 및 개인 키를 다운로드할 위치에 대한 정보 포함) 고마워요.

### cPanel을 사용하여 SSH 로그인 세부 정보 찾기

#### 1. 호스트명과 포트 찾기

cPanel에서 필요한 정보는 `security > ssh` 아래에 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/cpanel-security-ssh-access.png?resize=720%2C92&ssl=1)

호스트 이름은 SSH 호스트, 포트 이름은 SSH 포트로 표시됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/cpanel-ssh-access.png?resize=591%2C180&ssl=1)

#### 2. '사용자' 찾기

서버에 있는 사용자 이름입니다. `환경설정 > 사용자 관리자`로 이동합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/cpanel-preferences.png?resize=720%2C61&ssl=1)

사용자 관리자(user manager)에서는 모든 사용자를 볼 수 있으며, 여기에는 아무런 목적도 없는 것처럼 보이는 호스팅 회사가 설정한 이상하게 생긴 사용자가 포함될 수 있습니다. 어느 사용자가 자신인지 분명히 알 수 있기를 바랍니다. 안전한 곳에 복사하여 붙여넣으십시오.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/cpanel-user-manager.png?resize=720%2C322&ssl=1)

#### 3. 정체성 찾기파일

security > ssh

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/cpanel-manage-ssh-kerys.png?resize=720%2C79&ssl=1)

SSH 키 관리 버튼을 누르면 현재 사용 가능한 모든 공개 키와 개인 키가 포함된 페이지가 나타납니다. 공용 및 개인 키를 다운로드하여 `~/.ssh` 디렉토리에 복사합니다.

아래 이미지에는 개인 키가 없습니다. 서버에 더 이상 필요하지 않았기 때문에 삭제되었습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/cpanel-public-private-ssh-keys.png?resize=720%2C171&ssl=1)

직접 키를 생성하고 사용하려면 이 안내서에서 SSH 키 설정을 참조하십시오.

### 'config' 파일 생성

SSH를 사용하여 서버에 로그온하는 가장 빠른 방법은 `~/.ssh` 디렉토리에 `config`라는 파일을 만드는 것입니다. 이 템플릿을 복사하여 붙여넣고 세부 정보를 입력하십시오.

```undefined
Host any-alias-you-like
    HostName xxxxxxxx.xxxxxxx.xxx
    User xxxxxxxx
    Port xxxxx
    IdentityFile ~/.ssh/private-key-file
```

이제 다음을 통해 언제든지 로그온할 수 있습니다.

```undefined
ssh any-alias-you-like
```

## 서버에 WP-CLI를 설치하는 방법

아래 단계에 따라 서버에 WP-CLI를 설치하십시오.

### Linux 파일 시스템 개요

리눅스는 다중 사용자 운영 체제입니다. 많은 사용자 중 한 명일 수 있습니다. 시스템 관리자는 루트 권한을 가진 특수 사용자입니다. 그들은 그들이 원하는 것은 무엇이든 할 수 있다. 또한 파일 시스템의 `루트`는 별도의 개념이지만 관련 개념인 `/`이다.

일반적인 공유 서버 파일 시스템은 아래 다이어그램과 같습니다. 일반 사용자로 로그인하기 때문에 홈 디렉토리 내에서 유일하게 문제가 되는 영역이 있습니다.

```bash
/                       < type `cd /` to go to the root of file system
├── tmp
├── etc
├── run
├── root
├── dev
├── sys
├── proc
├── mnt
├── boot
├── var
├── home
│   ├── user1           < type `cd` to go to your home directory
│   │    └── www        < root directory of your webserver
│   │        └── blog   < subdirectory
│   └── user2           < other users you can't see unless you are the root user
├── usr
├── lost+found
├── srv
├── sbin -> usr/sbin
├── opt
├── media
├── lib64 -> usr/lib64
├── lib -> usr/lib
└── bin -> usr/bin
```

서버에 성공적으로 로그인하면 셸 명령 프롬프트가 `username@host`로 변경됩니다. 공유 서버를 사용하고 있으며 루트 액세스 권한이 없는 것으로 가정합니다.

홈 디렉토리로 이동하려면 `cd`를 입력하십시오. 내용을 보려면 ls를 입력하고 숨겨진 파일을 보려면 ls -la를 입력하고 긴 목록 형식을 사용할 수 있습니다.

### 홈 디렉토리에 WP-CLI 실행 파일 설치

사용할 수 있는 권한이 있는 디렉토리가 필요하며 이는 `$PATH`에 있습니다. `$PATH`를 보려면 `echo $PATH`를 입력하십시오. 각 위치는 `:(으)로 구분됩니다. 어수선해 보여 tr 명령을 사용하여 :을 새 줄인\n으로 바꾸면 결과를 더 명확하게 알 수 있다.

```bash
echo $PATH | tr ':' '\n'
```

홈 디렉토리에서 위치를 찾고 있기 때문에 `grep`을 사용하여 결과를 필터링할 수 있습니다.

```undefined
echo $PATH | tr ':' '\n' | grep "home"
```

새로 설치된 Centos에서는 다음을 확인할 수 있습니다.

```bash
/home/user-name/.local/bin
/home/user-name/bin
/home/user-name/.local/bin
/home/user-name/bin
```

항목이 여러 개 보이더라도 걱정하지 마십시오. 다른 사용자 또는 일부 임의 스크립트에 의해 여러 번 추가되었음을 의미합니다. /home/username/bin이 있는 경우 사용합니다. 그렇지 않은 경우 직접 생성하여 `$PATH`에 추가해야 합니다.

다음 명령을 실행하여 서버에 WP-CLI를 `~/bin/` 디렉토리에 설치합니다.

```undefined
curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar # download
chmod +x wp-cli.phar # make executable
mv wp-cli.phar ~/bin/wp # move and rename
```

설치 및 작동하는지 테스트하려면:

```undefined
wp --info
```

언제든지 최신 버전으로 쉽게 업데이트할 수 있습니다.

```undefined
wp cli update
```

참고: WP-CLI 명령에 대한 도움말이 필요한 경우 `wp [name of command] --help`를 사용하십시오.

## WordPress 설치

WordPress를 설치하려면 아래 단계를 따르십시오.

### WordPress 설치 위치 선택

모든 서버는 약간 다른 방식으로 설정됩니다. 일반적으로 홈 디렉토리에 있는 경우 "www"라는 디렉토리가 나타납니다. 웹 사이트가 있는 위치이며 웹 서버의 루트 디렉토리입니다. 여기에 설치한다면 사이트는 루트 위치(예: `http://example.com/`)에 저장됩니다. 사이트를 하위 디렉토리에 표시하려면 해당 사이트를 만들어 다음 디렉토리에 설치합니다.

```bash
cd www
mkdir blog
# Install into the blog directory
```

이제 워드프레스는 http://example.com/blog/에 출연한다.

홈 디렉토리에서 examplesite.com과 같은 웹사이트의 이름을 가진 디렉토리도 볼 수 있다. 이렇게 호스팅을 설정할 수 있습니다. 설치할 위치를 잘 모를 경우 호스팅의 기술 지원부에 문의하십시오.

### cPanel MySQL 데이터베이스 마법사를 사용하여 데이터베이스 생성

WordPress를 사용하려면 MySQL 데이터베이스가 필요합니다. 이 데이터베이스를 만들고 액세스할 수 있는 사용자를 추가해야 합니다. cPanel이 없는 경우 phpMyAdmin이 있을 수 있으며 이를 사용할 수도 있고 호스팅 기술 지원팀에 데이터베이스를 만들어 달라고 요청할 수도 있습니다.

cPanel에서는 데이터베이스를 간단하게 만들 수 있습니다. 마법사를 따라가서 사용자에게 `모든 권한`을 부여하기만 하면 됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/mysql-database-wizard-create-database.png?resize=720%2C183&ssl=1)

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/mysql-create-database-users.png?resize=720%2C303&ssl=1)

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/mysql-database-wizard-add-user.png?resize=720%2C433&ssl=1)

보안을 매우 중요시하는 경우 설치 후 초과 권한을 모두 제거할 수 있습니다.

### 선택한 언어의 핵심 WordPress 파일 다운로드

첫 번째 단계는 WordPress를 설치할 디렉토리에 `cd`를 입력한 다음 올바른 언어로 최신 WordPress를 다운로드하는 것입니다. 이 예에서는 `en_`을 사용합니다.GB:

```bash
cd www
wp core download --locale=en_GB

Downloading WordPress 5.5.3 (en_GB)...
md5 hash verified: 1c2c3d7bde057d99a869cd33331b2114
Success: WordPress downloaded.

# and look inside the directory with `ls`

user@host [~/www]$ ls

index.php    readme.html      wp-admin            wp-comments-post.php  wp-content   wp-includes        wp-load.php   wp-mail.php      wp-signup.php     xmlrpc.php
license.txt  wp-activate.php  wp-blog-header.php  wp-config-sample.php  wp-cron.php  wp-links-opml.php  wp-login.php  wp-settings.php  wp-trackback.php
```

### wp-config를 설정하는 중입니다.데이터베이스 세부 정보를 추가하여 php

다음으로 `wp-config`를 설정합니다.php 파일 생성한 데이터베이스의 세부 정보를 추가합니다.

```undefined
wp config create --dbname=exampledb --dbuser=exampledbuser --dbpass='securepswd'
```

- 이스케이프해야 하는 문자가 있는 경우 암호를 작은 따옴표로 묶으십시오.
- 공유 서버에서는 데이터베이스 이름 및 암호에 대해 선택한 이름 앞에 사용자 접두사를 강제 적용하므로 혼동하지 말고 필요한 경우 ti8jjsdf_dbname이 아닌 dbname을 사용하십시오.

### WordPress 설치

이것이 마지막 단계입니다. 여기서는 다음을 포함하여 필요한 나머지 세부 정보를 추가합니다.

```undefined
--url=The address of the new site, start with https:// and end with /subdirectory if needed
--title=The title of the new site
--admin_user=The name of the admin user, don't pick 'admin' for security reasons
[--admin_password=] The password for the admin user. Defaults to randomly generated string.
--admin_email=The email address for the admin user

wp core install --url=https://example.com --title="My Site" --admin_user=exampleAdmin --admin_password=securepass --admin_email=exampleAdmin@nowhere.org
```

이제 https://example.com/wp-admin에서 관리자로 로그인할 수 있습니다.

## WordPress 설치 후 설정

이제 WordPress 기본 설치가 완료되었으므로 사용자 정의해 보겠습니다.

### 예쁜 URL 설정

이거 SEO한테 좋은 거예요. 즉, URL이 게시된 날짜와 같이 별도의 추가 없이 사용자의 게시물 이름이 됩니다.

`wp 다시 쓰기 구조 `/%postname%/` --hard`

### 원치 않는 플러그인 삭제

WP에는 당신이 원하지 않는 플러그인이 몇 개 포함되어 있다. 삭제하기

wp 플러그인 삭제 mismet hello

### WordPress 플러그인 설치 및 활성화

원하는 플러그인의 목록이 많을 것입니다. 다음 목록에 추가합니다.

wp 플러그인 설치 안티스팸비 --activate

다음을 사용하여 현재 플러그인을 나열할 수 있습니다.

wp 플러그인 목록

### WordPress 테마 설치

WordPress에 테마를 설치하려면:

wp 테마 설치 20 - 활성화

`wp 테마 설치.../my-contest.지퍼를 채우다

## WP-CLI 시간 절약 명령

WP-CLI 내의 다음 명령을 사용하면 많은 시간을 절약할 수 있습니다.

### 파일 변조 확인

wp 플러그인 확인-체크섬 --all

### 백업할 데이터베이스 내보내기

이는 전체 WP 사이트 백업을 자동화하는 백업 스크립트의 일부로 만들 수 있습니다.

`wpdb 수출`

나중에 삭제해야 합니다. 공용 폴더에 삭제하지 마십시오.

### 모든 미디어 파일 크기 표시

이 기능은 사이트에서 사용하기 위해 이미지를 자를 때 매우 유용합니다.

wp 미디어 이미지 크기

## 결론

이제 SSH에 비해 WP-CLI를 사용하는 사람들이 누릴 수 있는 큰 이점을 알아주셨으면 합니다. 명령줄을 잘 모르면 배울 점이 많아 보일 수 있지만, 기본 사항을 알고 나면 개발자의 초능력을 갖게 됩니다.

WP-CLI에는 많은 명령이 있으며, 우리는 이 튜토리얼에서 표면만 긁었다. 이 환상적인 소프트웨어에 대해 자세히 알아보려면 WP-CLI 핸드북을 참조하십시오.

처음에는 분명하지 않은 것처럼 보일 수 있는 큰 이점은 이러한 모든 명령을 스크립트에 배치하여 자동화할 수 있다는 것입니다. 사이트를 설치 및 설정한 후에는 스크립트에 모든 명령을 붙여넣고 버튼을 눌러 모든 명령을 다시 수행할 수 있습니다.

예를 들어, 스크립트에서 이 문서에서 사용한 명령은 다음과 같습니다.

```bash
#!/bin/bash

wp core download --locale=en_GB
wp config create --dbname=exampledb --dbuser=exampledbuser --dbpass='securepswd'
wp core install --url=https://example.com --title="My Site" --admin_user=davidAdmin --admin_password=securepass --admin_email=davidAdmin@nowhere.org

wp rewrite structure '/%postname%/' --hard
wp plugin delete akismet hello
wp plugin install antispam-bee --activate
```