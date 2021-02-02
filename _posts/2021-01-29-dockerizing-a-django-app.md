---
layout: post
title: "Django 앱 Dockerizing
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/dockerize-django-app.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/dockerize-django-app.png?fit=730%2C487&ssl=1)

Django 프로젝트를 실현하려면 대부분의 경우 라이브러리 또는 종속성 형태의 기성 솔루션이 필요합니다.
 

이것은 일반적으로 문제가되지 않으며 종종`requirements.txt` 파일에 문서화됩니다.
 

문제는 전체 프로젝트를 실행하고 테스트하려는 다른 개인과 공유하려고 할 때 시작됩니다. 안타깝게도 라이브러리와 종속성을 크게 변경할 때마다 사용자가 처음부터 설정을 수행해야하기 때문입니다.
 

## Docker 란 무엇입니까?
 

여기에서 컨테이너화와 Docker가 등장합니다. Docker는 라이브러리와 종속성 문제를 완전히 해결하는 매우 인기있는 컨테이너화 플랫폼입니다.
 

그러나 그것의 가장 좋은 특징은?
 호스트 또는 기본 인프라에 관계없이 컨테이너화 된 애플리케이션은 항상 동일한 방식으로 실행됩니다.
 

이 가이드는 Docker로 Django 프로젝트를 설정하는 과정을 안내합니다.
 

## Docker를 사용해야하는 이유는 무엇입니까?
 

Docker는 운영 체제 (OS) 수준에서 하드웨어 가상화를 제공하는 제품입니다.
 이 기능을 통해 개발자는 컨테이너로 배포하기 위해 소프트웨어 및 해당 종속성을 패키징하고 제공 할 수 있습니다.
 

간단히 말해서 이제 소프트웨어에 필요한 모든 부분을 도커 이미지라고하는 단일 단위로 포장 한 다음이 이미지를 다른 사람과 배송하거나 공유 할 수 있습니다.
 수신자가 Docker를 가지고있는 한 프로젝트를 실행하거나 테스트 할 수 있습니다.
 "하지만 내 컴퓨터에서 작동했습니다!"라는 시대는 지났습니다.
 

또한 Docker는 Docker 이미지를 개발자와 커뮤니티간에 공유하고 관리 할 수있는 DockerHub라는 서비스를 제공합니다. 기본적으로 Docker 이미지에 대한 "GitHub"입니다.
 Docker CLI에 포함 된 CLI 명령을 통한 이미지 업로드 및 다운로드와 같은 코드 저장소 플랫폼과 몇 가지 유사점을 공유합니다.
 

## Docker 사용을위한 필수 구성 요소
 

- Django 개발에 능숙
 
- CLI 및 bash를 사용하는 중급 수준
 

이 가이드에서는 Docker 스크립팅이`YAML `파일에서 수행되고 파일은 Docker CLI를 통해 실행됩니다.
 

이 가이드는 Ubuntu 머신에서 Docker 설정을 탐색합니다.
 

다른 일반적인 OS 플랫폼의 경우 :
 

- Windows,이 페이지를 따르십시오.
 
- MacOs,이 페이지를 따르십시오.
 

Docker를 다운로드하고 설정하려면 아래 지침을 실행하십시오.
 

```undefined
sudo apt-get update  
sudo apt-get install docker-ce docker-ce-cli containerd.io  
```

## 장고 앱
 

이 가이드에서는 사용자가 이미 Django에 능숙하다고 가정하므로 Docker에서 기본 Django Rest Framework 앱을 실행하고 기본 페이지를 표시하는 단계를 보여줍니다.
 

Django와 Docker의 `Hello world`라고 생각하세요.
 그 후에는 이전 또는 미래의 Django 프로젝트, 특히`requirements.txt`에 나열된 라이브러리가있는 프로젝트를 Dockerize 할 수 있습니다.
 

시작하려면 아래 명령을 실행하여 Django 앱 설정을 가져옵니다.
 

`django-admin startproject dj_docker_drf`
 

프로젝트 폴더로 이동하여`sample`이라는 앱을 시작한 다음`settings.py`의`INSTALLED_APPS` 목록에`rest_framework` 및`sample`을 추가합니다.
 

`views.py` 파일에 "HELLO WORLD FROM DJANGO AND DOCKER"메시지를 반환하는 함수를 작성합니다.
 

```python
from rest_framework.views import APIView  
from django.http import JsonResponse  

class HomeView(APIView):  

 def get(self, request, format=None):
    return JsonResponse({"message":
    'HELLO WORLD FROM DJANGO AND DOCKER'})  
```

사용자가 브라우저에서 앱에 액세스 할 때 액세스되는 기본보기가`HomeView`가되도록 메인 URL 파일과 앱 URL 파일을 연결합니다.
 

많은 사람들이 잊어 버리는 중요한 단계는 모든 IP에서 Django 애플리케이션에 대한 액세스를 허용하기 위해`ALLOWED_HOSTS`를 `*`로 설정하는 것입니다.
 코드 스 니펫은 아래에서 공유됩니다.
 

```undefined
ALLOWED_HOSTS = [‘*’]
```

마지막으로 일반적으로 존재하는 루트 프로젝트 폴더에`requirements.txt` 파일을 만들고 DRF 라이브러리를 추가합니다.
 

```undefined
django-rest-framework==0.1.0  
```

이제 앱을 도킹 할 준비가되었습니다.
 

## Docker 파일 및 Docker CLI 생성
 

Docker 파일의 이름이 지정됩니다.
 이는 Docker CLI가이를 추적 할 수 있도록하기위한 것입니다.
 

프로젝트 루트에서`Dockerfile`이라는 파일을 만들고 엽니 다.
 Docker 지시문은 주석으로 설명되었습니다.
 

```shell
# base image  
FROM python:3.8   
# setup environment variable  
ENV DockerHOME=/home/app/webapp  

# set work directory  
RUN mkdir -p $DockerHOME  

# where your code lives  
WORKDIR $DockerHOME  

# set environment variables  
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1  
# install dependencies  
RUN pip install --upgrade pip  
# copy whole project to your docker home directory. COPY . $DockerHOME  
# run this command to install all dependencies  
RUN pip install -r requirements.txt  
# port where the Django app runs  
EXPOSE 8000  
# start server  
CMD python manage.py runserver  
```

## Docker에서 앱 실행
 

앱을 실행하려면 두 단계 만 수행하면됩니다.
 

- 이미지 빌드 : 방금 만든`Dockerfile`을 사용하는`build` 명령을 사용하여 수행합니다.
 이미지를 빌드하려면`docker build.
 -t 도커-장고 -v0.0`.
 이 명령은 Docker 파일이있는 디렉토리에서 실행해야합니다.
 `-t` 플래그는 컨테이너를 실행할 때 참조 할 수 있도록 이미지에 태그를 지정합니다.
 
- 이미지 실행 : 이것은`docker run` 명령을 사용하여 수행됩니다.
 이렇게하면 빌드 된 이미지가 실행중인 컨테이너로 변환됩니다.
 

이제 앱을 사용할 준비가되었습니다!
 

앱을 실행하려면`docker run docker-django-v0.0` 명령을 실행하고 브라우저에서 0.0.0.0:8000으로 앱을 봅니다.
 

## Docker Compose로 여러 컨테이너 실행
 

Docker에서 얻은 숙련도를 바탕으로 논리적 다음 단계는 여러 컨테이너를 실행하는 방법과 순서를 아는 것입니다.
 

이는 모든 종류의 다중 컨테이너 애플리케이션을 정의하고 실행하는 데 사용되는 도구 인 Docker Compose의 완벽한 사용 사례입니다.
 간단히 말해 애플리케이션에 여러 컨테이너가있는 경우 Docker-Compose CLI를 사용하여 필요한 순서대로 모두 실행합니다.
 

예를 들어 다음 구성 요소가 포함 된 웹 애플리케이션을 살펴 보겠습니다.
 

- NGINX와 같은 웹 서버 컨테이너
 
- Django 앱을 호스팅하는 애플리케이션 컨테이너
 
- POSTGRES와 같은 프로덕션 데이터베이스를 호스팅하는 데이터베이스 컨테이너
 
- RabbitMq와 같은 메시지 브로커를 호스팅하는 메시지 컨테이너
 

이러한 시스템을 실행하려면 Docker-compose`YAML` 파일에서 지시문을 선언합니다. 여기서 이미지를 빌드하는 방법, 이미지에 액세스 할 수있는 포트, 가장 중요한 것은 컨테이너가 배치되는 순서를 명시합니다.
 실행 됨 (즉, 어떤 컨테이너가 프로젝트를 실행할 컨테이너에 따라 달라지는 지).
 

이 특정 예에서 계산 된 추측을 할 수 있습니까?
 가장 먼저 회전해야하는 컨테이너와 다른 컨테이너에 의존하는 컨테이너는 무엇입니까?
 

이 질문에 답하기 위해 Docker Compose를 살펴 보겠습니다.
 먼저이 가이드에 따라 호스트 운영 체제에 CLI 도구를 설치하십시오.
 

Docker Compose (및 Docker와 유사)를 사용하면 특별한 이름을 가진 특정 파일이 필요합니다.
 이것이 CLI 도구가 이미지를 스핀 업하고 실행하는 데 사용하는 것입니다.
 

Docker Compose 파일을 만들려면 YAML 파일을 만들고 이름을 `docker-compose.yml`로 지정합니다.
 이상적으로는 프로젝트의 루트 디렉토리에 있어야합니다.
 이 프로세스를 더 잘 이해하기 위해 위에 설명 된 시나리오 (Postgres 데이터베이스가있는 Django 앱, RabbitMQ 메시지 브로커 및 NGINX로드 밸런서)를 사용하여 Docker Compose를 살펴 보겠습니다.
 

## Django 앱과 함께 Docker Compose 사용
 

```undefined
version: '3.7'

services: # the different images that will be running as containers
  nginx: # service name
    build: ./nginx # location of the dockerfile that defines the nginx image. The dockerfile will be used to spin up an image during the build stage
    ports:
      - 1339:80 # map the external port 1339 to the internal port 80. Any traffic from 1339 externally will be passed to port 80 of the NGINX container. To access this app, one would use an address such as 0.0.0.0:1339
    volumes: # static storages provisioned since django does not handle static files in production
      - static_volume:/home/app/microservice/static # provide a space for static files
    depends_on:
      - web # will only start if web is up and running
    restart: "on-failure" # restart service when it fails
  web: # service name
    build: . #build the image for the web service from the dockerfile in parent directory.
    # command directive passes the parameters to the service and they will be executed by the service. In this example, these are django commands which will be executed in the container where django lives.
    command: sh -c "python manage.py makemigrations &&
                    python manage.py migrate &&
                    gunicorn microservice_sample_app.wsgi:application --bind 0.0.0.0:${APP_PORT}" # Django commands to run app using gunicorn
    volumes:
      - .:/microservice # map data and files from parent directory in host to microservice directory in docker container
      - static_volume:/home/app/microservice/static
    env_file: # file where env variables are stored. Used as best practice so as not to expose secret keys
      - .env # name of the env file
    image: microservice_app # name of the image

    expose: # expose the port to other services defined here so that they can access this service via the exposed port. In the case of Django, this is 8000 by default
      - ${APP_PORT} # retrieved from the .env file
    restart: "on-failure"
    depends_on: # cannot start if db service is not up and running
      - db
  db: # service name
    image: postgres:11-alpine # image name of the postgres database. during build, this will be pulled from dockerhub and a container spun up from it
    volumes:
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql
      - postgres_data:/var/lib/postgresql/data/
    environment: # access credentials from the .env file
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
      - POSTGRES_DB=${DB_NAME}
      - PGPORT=${DB_PORT}
      - POSTGRES_USER=${POSTGRES_USER}
    restart: "on-failure"
  rabbitmq:
        image: rabbitmq:3-management-alpine # image to be pulled from dockerhub during building
        container_name: rabbitmq # container name
        volumes: # assign static storage for rabbitmq to run
           rabbitmq: - ./.docker/rabbitmq/etc/:/etc/rabbitmq/
            - ./.docker/rabbitmq/data/:/var/lib/rabbitmq/
           rabbitmq_logs:  - ./.docker/rabbitmq/logs/:/var/log/rabbitmq/
        environment: # environment variables from the referenced .env file
            RABBITMQ_ERLANG_COOKIE: ${RABBITMQ_ERLANG_COOKIE}
            # auth cretendials
            RABBITMQ_DEFAULT_USER: ${RABBITMQ_DEFAULT_USER} 
            RABBITMQ_DEFAULT_PASS: ${RABBITMQ_DEFAULT_PASS}
        ports: # map external ports to this specific container's internal ports
            - 5672:5672
            - 15672:15672
        depends_on: # can only start if web service is running
            - web


volumes:
  postgres_data:
  static_volume:
  rabbitmq:
  rabbitmq_logs:
```

Docker Compose의 하이라이트 중 하나는`depends_on` 지시문입니다.
 위의 스크립트에서 다음과 같이 추론 할 수 있습니다.
 

- NGINX는 웹에 의존
 
- 웹은 db에 의존
 
- RabbitMQ는 웹에 의존합니다
 

이 설정을 사용하면 db가 가장 먼저 시작되고 웹, RabbitMQ, 마지막으로 NGINX가 차례로 시작됩니다.
 환경을 파괴하고 실행중인 컨테이너를 중지하기로 결정하면 순서가 반대가됩니다.
 NGINX는 가장 먼저 실행되고 db는 마지막입니다.
 

## Docker Compose 스크립트 빌드 및 실행
 

Docker 스크립트와 마찬가지로 Docker Compose 스크립트는 빌드 및 실행 명령이 있다는 점에서 유사한 구조를 가지고 있습니다.
 빌드 명령어는 종속성 계층 구조의 순서로`services` 아래에 정의 된 모든 이미지를 빌드합니다.
 실행 명령은 종속성 계층 구조의 순서대로 컨테이너를 회전합니다.
 

다행히 빌드와 실행을 결합하는 명령이 있습니다.
 `업`이라고합니다.
 이 명령을 실행하려면 아래 명령을 실행하십시오.
 

```undefined
 docker-compose up
```

빌드 플래그를 추가 할 수도 있습니다.
 이것은 이전에이 명령을 실행 한 적이 있고 새 이미지를 다시 빌드하려는 경우에 유용합니다.
 

```undefined
docker-compose up —build
```

컨테이너 작업이 끝나면 모든 컨테이너를 종료하고 사용 중이던 모든 정적 저장소 (예 : postgres 정적 볼륨)를 제거 할 수 있습니다.
 이렇게하려면 아래 명령을 실행하십시오.
 

```undefined
docker-compose down -V
```

`-V` 플래그는 볼륨을 나타냅니다.
 이렇게하면 컨테이너와 연결된 볼륨이 종료됩니다.
 

다양한 Docker Compose 명령 및 사용법에 대해 자세히 알아 보려면 공식 문서를 따르십시오.
 

## Docker에서 파일 지원
 

위의 스크립트에서 참조 된 일부 파일은 파일의 부피를 줄여서 코드 관리를 더 쉽게 만듭니다.
 여기에는 ENV 파일과 NGINX Docker 및 구성 파일이 포함됩니다.
 다음은 각각에 수반되는 샘플입니다.
 

ENV 파일
이 파일의 주요 목적은 키 및 자격 증명과 같은 변수를 저장하는 것입니다.
 이것은 개인 키가 노출되지 않도록하는 안전한 코딩 방법입니다.
 

```undefined
#Django
SECRET_KEY="my_secret_key"
DEBUG=1
ALLOWED_HOSTS=localhost 127.0.0.1 0.0.0.0 [::1] *


# database access credentials
ENGINE=django.db.backends.postgresql
DB_NAME=testdb
POSTGRES_USER=testuser
POSTGRES_PASSWORD=testpassword
DB_HOST=db
DB_PORT=5432
APP_PORT=8000
#superuser details
DJANGO_SU_NAME=test
DJANGO_SU_EMAIL=admin12@admin.com
DJANGO_SU_PASSWORD=mypass123
#rabbitmq
RABBITMQ_ERLANG_COOKIE: test_cookie
RABBITMQ_DEFAULT_USER: default_user
RABBITMQ_DEFAULT_PASS: sample_password
```

NGINX Docker 파일
 

이것은 루트 디렉토리의`nginx` 폴더에서 호스팅됩니다.
 주로 Docker 허브에서 가져올 이미지 이름과 구성 파일의 위치라는 두 가지 지시문이 포함됩니다.
 다른 Docker 파일 인`Dockerfile`로 이름이 지정됩니다.
 

```coffeescript
FROM nginx:1.19.0-alpine

RUN rm /etc/nginx/conf.d/default.conf
COPY nginx.conf /etc/nginx/conf.d
```

NGINX 구성 파일
 

여기서 NGINX 구성 로직을 작성합니다.
 NGINX 폴더의 NGINX Docker 파일과 동일한 위치에 있습니다.
 

이 파일은 NGINX 컨테이너가 작동하는 방식을 지정합니다.
 다음은 일반적으로`nginx.conf`라는 파일에있는 샘플 스크립트입니다.
 

```undefined
upstream microservice { # name of our web image
    server web:8000; # default django port
}

server {

    listen 80; # default external port. Anything coming from port 80 will go through NGINX

    location / {
        proxy_pass http://microservice_app;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $host;
        proxy_redirect off;
    }
    location /static/ {
        alias /home/app/microservice/static/; # where our static files are hosted
    }

}
```

## 결론
 

이 가이드의 Docker 팁과 트릭은 모든 조직의 DevOps 및 풀 스택 개발자 위치에 필수적이며 Docker는 백엔드 개발자를위한 편리한 도구이기도합니다.
 

Docker는 종속성 및 라이브러리를 패키지하기 때문에 새로운 개발자는 여러 종속성을 설치할 필요가 없으며 라이브러리 및 종속성이 작동하도록하는 데 귀중한 시간을 낭비 할 필요가 없습니다.
 읽어 주셔서 감사합니다.
 