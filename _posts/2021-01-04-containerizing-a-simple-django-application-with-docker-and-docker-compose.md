---
layout: post
title: "Docker 및 Docker Composite를 사용하여 간단한 Django 애플리케이션 컨테이너화"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/dockeranddockercompose.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/dockeranddockercompose.png?fit=730%2C487&ssl=1)

만약 여러분이 한동안 코딩에 몰두해 왔다면, 여러분은 아마 도커나 용기 같은 더 멋진 용어들을 들어봤을 것이다. 이 기사에서는 간단한 짱고 웹 애플리케이션을 컨테이너화하도록 안내하면서 도커를 통한 컨테이너화의 개념을 소개합니다. 이 항목의 끝부분에서는 다음 사항에 대해 잘 알고 있어야 합니다.

- 가상화
- 컨테이너화(도커 사용)
- 도커
- 도커 파일 작성
- 도커 컴포지션
- Docker 파일과 Docker-compose를 사용하여 Docker 환경에서 Django 앱 설정

## 요구 사항들

이 튜토리얼을 수행하려면 컴퓨터에 다음을 설치하는 것이 좋습니다.

- 깃 / 깃허브
- Visual Studio Code(또는 원하는 텍스트 편집기)
- 짱고 실무 지식

## 가상화 이해

기존에는 웹 애플리케이션을 DigitalOcean 또는 Linode와 같은 서버 호스팅 서비스 공급자에 배포하려면 가상 시스템이나 가상 컴퓨터를 설정한 다음 git, FTP 또는 다른 방법을 통해 작성된 코드를 전송해야 합니다. 이 프로세스를 가상화라고 합니다.

시간이 지날수록 많은 개발자들이 (운영체제 변화를 수용하기 위해 변경 시 소요된 시간을 고려할 때) 비용이 많이 들면서 이 프로세스의 단점을 인식하기 시작했다. 개발자들은 컨테이너화 아이디어가 나온 곳인 개발과 생산 환경을 통합하는 방법을 원했다.

## 컨테이너는 무엇이고 왜 그렇게 멋질까요?

컨테이너는 간단한 용어로 말하면 개발 환경, 즉 애플리케이션을 실행해야 하는 종속성만 포함하고 있습니다.

컨테이너를 사용하면 개발자로서 애플리케이션의 종속성을 패키징하고 많은 변경 사항 없이 한 컴퓨팅 환경에서 다른 환경으로 애플리케이션을 이동할 수 있습니다.

컨테이너화는 비교적 더 이동성이 높고, 확장성이 뛰어나며, 효율적이기 때문에 Docker와 같은 플랫폼은 나날이 애플리케이션을 개발하는 데 인기 있는 선택이 되고 있습니다.

## 도커 소개

도커는 컨테이너를 사용하여 응용프로그램을 만들고 관리 및 실행할 수 있도록 개발된 도구 상자입니다. 개발자는 거의 모든 곳에서 실행할 수 있는 휴대용, 자급자족, 경량 컨테이너로 어떤 애플리케이션도 쉽게 포장, 배송 및 실행할 수 있습니다.

## 도커 설치

컴퓨터에서 Docker 설정을 시작하려면 해당 호스트 OS에 대한 공식 설명서를 따르는 것이 좋습니다. Windows 사용자는 창용 도커 데스크톱을 설치한 후 도커를 사용할 수 있습니다. 리눅스 및 OSX 사용자는 리눅스용 도커와 Mac용 도커를 각각 설치할 수 있다.

## 응용 프로그램 설정

이 튜토리얼을 위해, 나는 Django로 작성된 개발 중 폴링 애플리케이션의 소스 코드를 포함하는 스타터 저장소를 설정했다. 이 워크스루(워크스루)를 더 진행하면서 애플리케이션이 실행될 컨테이너에 대한 지시사항을 개략적으로 설명하는 도커파일(Docker-compose.yml)과 작업흐름을 단순화하는 도커콤포즈(Docker-compose.yml) 파일을 만들 예정이다.

git가 설치된 컴퓨터에서 선택한 폴더(예: /desktop)로 이동한 후 다음을 실행하여 GitHub 저장소에서 스타터 파일을 복제합니다.

```php
git clone https://github.com/theafolayan/django-docker-starter.git
```

이 작업이 완료되면 프로젝트의 루트 폴더로 이동하고 다음을 실행하여 VSCode에서 여십시오.

```bash
cd django-docker-starter && code .
```

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/structuredownloadedprojectshouldinvscode.png?resize=770%2C1489&ssl=1)

이 디렉토리에서 확장자 없이 `Dockerfile`이라는 대/소문자를 구분하는 파일을 작성합니다. 이 도커 파일은 도커 컨테이너 구성이 지정된 곳입니다. 기본적으로 "docker build" 명령을 실행할 때마다 컨테이너가 부팅될 때마다 실행할 명령 집합을 작성하는 파일입니다.

다음으로 `요구사항`을 작성합니다.애플리케이션에 필요한 모든 종속성이 나열되는 txt 파일입니다. 이 파일은 나중에 도커 파일에 사용되어 컨테이너에 설치할 종속성을 기록합니다.

`요구사항에서.txt` 파일을 아래 예와 같이 종속성으로 Django 버전 3.1.2를 추가하고 저장을 누릅니다.

```undefined
Django==3.1.2
```

## 도커 파일이란?

Docker 파일을 작성하는 아이디어는 다소 복잡해 보일 수 있지만, 사용자 지정 Docker 이미지를 만드는 데 사용되는 문서 레시피일 뿐입니다. 도커 파일에는 당연히 다음이 포함됩니다.

- 사용자 자신의 이미지를 구축하려는 기본 이미지입니다. 이 이미지를 컨테이너의 기초 역할을 하는 또 다른 이미지로 생각해 보십시오. 운영 체제, 프로그래밍 언어(우리의 경우 파이썬) 또는 프레임워크일 수 있습니다.
- 도커 이미지에 설치해야 하는 패키지 및 필수 유틸리티
- 도커 이미지에 복사할 스크립트 및 파일. 일반적으로 이 코드는 응용 프로그램의 소스 코드입니다.

Docker 파일을 읽거나 쓸 때 다음 사항에 유의하면 편리합니다.

- 지침이 포함된 줄은 RUN, FROM, Copy, WORKDIR 등과 같은 해당 키워드로 시작합니다.
- 설명을 포함하는 행은 "#" 기호로 시작합니다. 이러한 줄은 도커 파일 명령이 실행되기 전에 제거됩니다.

## 도커 파일 작성

저희 짱고 앱이 공식 파이썬 도커 이미지에 들어갈 예정입니다.

Docker 파일에서 다음 지침을 작성하고 저장을 누릅니다.

```undefined
#Tells Docker to use the official python 3 image from dockerhub as a base image
FROM python:3
# Sets an environmental variable that ensures output from python is sent straight to the terminal without buffering it first
ENV PYTHONUNBUFFERED 1
# Sets the container's working directory to /app
WORKDIR /app
# Copies all files from our local project into the container
ADD ./app
# runs the pip install command for all packages listed in the requirements.txt file
RUN pip install -r requirements.txt
```

## 도커 작성 이해

Docker Composite는 여러 서비스를 실행해야 하는 애플리케이션을 정의하고 실행할 수 있는 훌륭한 개발 도구입니다.

Docker Composite는 일반적으로 `docker-composes.yml` 파일을 사용하여 프로그램에서 사용할 서비스를 구성하고 `docker composition`을 실행하면 구성 파일에서 이러한 서비스를 생성하고 시작할 수 있습니다. 대부분의 일반적인 웹 애플리케이션에서 이러한 서비스는 웹 서버(예: Nginx)와 데이터베이스(Postgre)로 구성됩니다.예를 들어 SQL). 이 예에서는 애플리케이션이 SQLite 데이터베이스를 사용하므로 Postgres와 같은 외부 데이터베이스 서비스를 설정할 필요가 없습니다.

Docker Composite에서 제공하는 기능을 사용하려면 Docker-composes.yml 파일을 Docker 파일과 동일한 루트 폴더에 생성하고 다음 코드를 추가하십시오.

```bash
version: '3.8'
services:
   web:
       build: .
       command: python manage.py runserver localhost:8000
       ports:
           - 8000:8000
```

`docker-compose.yml` 파일의 내용은 다음과 같이 줄줄이 설명되어 있습니다.

```bash
version: '3'
```

이것은 도커에게 우리 파일을 실행하기 위해 어떤 버전의 `도커 합성`이 사용되어야 하는지 알려준다. 이 기사를 쓸 때 사용 가능한 최신 버전은 "3.8"이며, 일반적으로 향후 이 글을 읽을 때 다음 두 버전의 구문은 거의 동일하게 유지되어야 한다.

`docker-compose` 파일이 설정되면 터미널을 열고 `docker-compose` 명령을 실행하여 응용프로그램을 만들고 제공합니다. 그런 다음 브라우저의 "localhost:8000"으로 이동하여 컨테이너형 짱고 애플리케이션을 확인합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/pollsindex.png?resize=1204%2C993&ssl=1)

컨테이너를 종료하려면 새 터미널을 열고 `docker-composure down`을 실행하십시오.

## 결론.

지금까지 이 과정에서 많은 내용을 다루었으며, 가상화, 컨테이너화 및 기타 Docker 관련 용어를 숙지하는 것으로 시작했습니다. 다음으로 도커 파일이 무엇인지, 짱고 앱을 담기 위해 도커 파일을 만드는 방법에 대해 알아보았습니다. 마지막으로, "docker-compose.yml" 파일을 통해 "docker-compose"를 설정하여 애플리케이션이 사용할 서비스를 구성합니다.

Django 애플리케이션에서 Docker를 사용할 수 있는 올바른 방법은 단 한 가지도 없지만, 가능한 안전하게 응용프로그램을 사용할 수 있도록 하기 위해 아래에 나열된 공식 지침을 사용하는 것이 좋습니다. 여기 GitHub에서 완전히 컨테이너화된 앱이 포함된 repo를 확인하실 수 있습니다.
추가 리소스:

- 도커 파일 작성 방법
- 도커 작곡과 짱고