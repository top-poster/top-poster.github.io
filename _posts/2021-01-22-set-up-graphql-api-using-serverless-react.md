---
layout: post
title: "Serverless 및 React를 사용하여 GraphQL API를 설정하는 방법
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/serverless-graphql-api-serverless-appsync-react.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/serverless-graphql-api-serverless-appsync-react.png?fit=730%2C487&ssl=1)

## 소개
 

소프트웨어 프로젝트가 계속 복잡 해짐에 따라 처음부터 프로젝트에 적합한 아키텍처를 선택하는 것은 중요한 결정입니다.
 프로젝트를 더 작고 이해하기 쉬운 구성 요소로 나누면 많은 이점이 있으므로 다음을 훨씬 쉽게 수행 할 수 있습니다.
 

- 새로운 기능에 대한 이유
 
- 코드베이스 탐색
 
- 새로운 기능을 빠르게 개발
 
- 구성 요소에 대한 테스트 작성
 
- 관심사 분리가 용이하여 단일 책임 원칙 적용
 

이러한 이점을 달성하는 데 사용할 수있는 아키텍처 중 하나는 서버리스 아키텍처입니다 (서버리스 컴퓨팅 또는 FaaS-서비스로서의 기능이라고도 함).
 

이 튜토리얼에서는 샘플 GraphQL 할일 API를 빌드하여 서버리스 프레임 워크를 사용하여 서버리스 프로젝트를 설정하는 방법을 배웁니다.
 AWS (Lambda, DynamoDB 및 AppSync)를 서비스로서의 백엔드 (BaaS)로 사용할 것입니다.
 

### 서버리스 아키텍처 란 무엇입니까?
 

서버리스 아키텍처에 대한 많은 설명이 있지만 제가 접한 최고의 설명은 Mike Roberts의 놀라운 작품입니다.
 

> 서버리스 아키텍처는 타사 "BaaS (Backend as a Service)"서비스를 통합하고 / 또는 "FaaS (Functions as a Service)"플랫폼의 관리되는 임시 컨테이너에서 실행되는 사용자 지정 코드를 포함하는 애플리케이션 설계입니다.
 

### 서버리스 프레임 워크
 

Serverless Framework는 개발자가 모든 FaaS 공급자에 클라우드 애플리케이션을 배포하는 데 도움이되는 오픈 소스 프로젝트입니다.
 

이 자습서에서는 서버리스 프레임 워크를 사용하여 AWS AppSync, AWS Lambda 및 DynamoDB 통합을 통해 샘플 서버리스 프로젝트를 AWS에 배포하는 방법을 다룰 것입니다.
 

### 전제 조건
 

- AWS 계정
 여기에서 AWS 프리 티어에 가입 할 수 있습니다.
 
- GraphQL의 기본 이해
 
- DynamoDB에 대한 중급 지식 (무서워하지 마십시오. 대부분의 경우 코드를 복사 / 붙여 넣기 만하면됩니다. 나중에 배울 수 있습니다.)
 
- 클라우드 형성 템플릿에 대한 중급 이해 (위와 동일, 이전에 CFT로 작업 한 적이없는 경우 겁내지 마십시오)
 

## 프로젝트 설정
 

이제 서버리스가 무엇이며 무엇을 달성하고자하는지에 대한 간략한 개요를 얻었으므로 이제 새로운 프로젝트를 시작하고 설정할 수 있습니다.
 

### 서버리스 프레임 워크 CLI 설정
 

CLI를 설정하려면 머신에 Node 및 npm이 설치되어 있는지 확인한 후 다음 명령을 실행합니다.
 

```shell
$ npm install -g serverless
```

성공적으로 설치 한 후 서버리스 대시 보드 계정을 등록한 다음 CLI에서 계정에 로그인합니다.
 

```shell
$ serverless login
```

그러면 계정에 로그인 할 수있는 링크가 브라우저에 열립니다.
 

### AWS CLI 설정
 

설정을 시작하려면 먼저 운영 체제에 따라 달라지는 AWS CLI를 다운로드해야합니다.
 이 튜토리얼에서는 macOS 버전을 다룰 것이지만 여기에서 다른 운영 체제에 대한 가이드를 찾을 수 있습니다.
 

다음 명령을 실행하여 CLI를 다운로드하고 설치합니다.
 

```shell
$ curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
$ sudo installer -pkg AWSCLIV2.pkg -target /
```

다음 명령은 셸이 경로에서`aws` 명령을 찾을 수 있는지 확인합니다.
 

```shell
$ which aws
/usr/local/bin/aws 
```

### IAM 사용자 및 액세스 키 생성
 

CLI 구성을 준비하려면 AWS에 서비스를 배포하기 위해 AWS 액세스 키에 대한 서버리스 프레임 워크 임시 액세스를 제공하는 AWS IAM 역할을 설정해야합니다.
 

IAM 콘솔로 이동 한 다음 탐색에서 사용자를 선택합니다.
 사용자 추가 버튼을 클릭하면 새 사용자 세부 정보를 입력 할 수있는 양식으로 리디렉션됩니다.
 

새 사용자의 적절한 사용자 이름을 선택한 다음 액세스 유형에서 프로그래밍 방식 액세스를 선택하고 다음 : 권한을 클릭합니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/set-user-details.png?resize=730%2C443&ssl=1)

새 사용자는 서버리스 앱에서 배포하거나 CLI로 작업하는 동안 AWS 서비스와 상호 작용할 수있는 관리자 권한이 필요합니다.
 

기존 그룹이 없으므로 그룹 만들기를 클릭하여 새 그룹을 만듭니다.
 필터 정책 섹션에 "관리자"를 입력하고 AdministratorAccess 정책을 선택한 다음 그룹 이름을 선택합니다. 가급적`admin-users` (또는 원하는대로)를 선택합니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/create-group-page.png?resize=730%2C423&ssl=1)

새 그룹을 만들어야하는 그룹 만들기를 클릭하고 그룹을 미리 선택합니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/add-group-page.png?resize=730%2C429&ssl=1)

다음 : 태그를 클릭하면 태그 섹션으로 이동합니다.
 태그는 선택 사항이므로이 부분은 건너 뛰겠습니다.
 검토 페이지로 이동하면 제공 한 세부 정보의 개요가 표시됩니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/review-page.png?resize=730%2C435&ssl=1)

사용자 생성을 클릭하면 새 사용자가 생성되고 새로 생성 된 액세스 키가 표시됩니다.
 자격 증명은 한 번만 사용할 수 있으므로 잃어 버리지 않도록 각별히주의해야하므로 CSV 파일을 다운로드하여 안전한 곳에 보관하십시오.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/download-csv-page.png?resize=730%2C409&ssl=1)

이제 모든 액세스 키가 설정되었으므로 CLI를 성공적으로 구성 할 수 있습니다.
 

```python
$ aws configure
AWS Access Key ID [None]: NEW_KEY_ID
AWS Secret Access Key [None]: SECRET_ACCESS_KEY
Default region name [None]: us-east-2 or preferred region
Default output format [None]: json
```

이러한 자격 증명은`~ / .aws / credentials`에있는 사용자 디렉터리의 파일에 저장됩니다.
 서버리스 프레임 워크는 배포를 실행할 때이 파일에서 기본 프로필을 선택합니다.
 

파일은 편집 가능하며 여러 프로필을 가질 수 있으므로 다음과 같이 선택한 프로필에 따라 배포를 수행 할 수 있습니다.
 

```shell
$ serverless deploy --aws-profile your_profile_name_here
```

## 새 서버리스 프로젝트 만들기
 

그리고 여기에 여러분 모두가 기다려온 부분이 있습니다!
 🙂
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/man-playing-drums.gif?resize=384%2C384&ssl=1)

이제 모든 것이 설정되고 준비가 완료되었으므로 원하는 작업 공간 디렉터리로 이동하고 다음 명령을 실행하여 샘플 Node.js 템플릿으로 새 서버리스 프로젝트를 만들어 시작합니다.
 

```shell
$ serverless
```

이 자습서를 더 쉽게 수행하려면 다음 옵션을 선택하십시오.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/serverless-project.png?resize=730%2C370&ssl=1)

축하합니다!
 첫 번째 서버리스 프로젝트를 만들었습니다!
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/champagne-bottle-popping.gif?resize=640%2C480&ssl=1)

디렉토리 구조는 다음과 같아야합니다 (트리가 설치되지 않은 경우`brew install tree` 실행).
 

```cpp
$ tree . -L 2 -a
.
├── .gitignore
├── .serverless
│   ├── cloudformation-template-create-stack.json
│   ├── cloudformation-template-update-stack.json
│   ├── serverless-introduction.zip
│   └── serverless-state.json
├── handler.js
└── serverless.yml
```

새 파일을 만들 필요가없는 경우`handler.js` 및`serverless.yml` 파일 만 사용합니다.
 두 파일의 목적은 기사의 뒷부분에서 자세히 설명합니다.
 

### 프로젝트 배포
 

이제 다음 단계로 넘어갑니다. 다음을 실행하여 새 프로젝트를 클라우드에 배포합니다.
 

```shell
$ serverless deploy
```

프로젝트에 리소스가 많지 않으므로 완료하는 데 몇 분 정도 걸릴 수 있습니다.
 배포가 완료되면 다음과 유사한 화면이 표시됩니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/completed-serverless-project.png?resize=730%2C428&ssl=1)

기본적으로 서비스는`us-east-1` 지역에 배포됩니다.
 Lambda 대시 보드로 이동하면 목록의 일부로`handler.js`에 작성된 샘플`hello` 함수가 있으며, 프로젝트 이름은 접두사로, 함수 이름은 접미사로 사용됩니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/create-function-page.png?resize=730%2C456&ssl=1)

이를 통해 모든 것이 모두 설정되었음을 확인하고 이제 서버리스 프레임 워크의 다양한 구성 요소를 살펴볼 수 있습니다.
 

## 서버리스 프레임 워크 핵심 개념
 

프레임 워크의 주요 개념은 다음과 같습니다.
 

- 기능
 
- 이벤트
 
- 자원
 
- 서비스
 verified_user
- 플러그인
 

### 기능
 

함수는 특정 작업을 수행하는 코드 블록입니다.
 값이나 프로 시저를 반환 할 수 있습니다.
 함수는 관심사 분리를 따르고 더 깨끗한 코드를 얻기 위해 단일 목적을 가져야합니다.
 다음과 같은 작업에 함수를 사용할 수 있습니다.
 

- 이메일 알림 보내기
 
- 데이터베이스에 사용자 저장
 
- 타사 API와 상호 작용
 
- 그리고 더
 verified_user

함수는 독립적으로 배포됩니다.
 마이크로 서비스로 생각할 수 있습니다.
 

### 이벤트
 

이벤트는 실행할 AWS Lambda 함수를 트리거하는 모든 것입니다.
 샘플 이벤트는 다음과 같습니다.
 

- AWS S3 버킷 업로드 (예 : 이미지 용)
 
- AWS DynamoDB 스트림 (예 : DB에 저장된 새 데이터)
 
- AWS API Gateway HTTP 엔드 포인트 요청 (예 : REST API 용)
 
- 그리고 더…
 verified_user

AWS Lambda 함수에 정의 된 모든 이벤트에 대해 필요한 인프라가 자동으로 생성되고 (예 : API Gateway 엔드 포인트) AWS Lambda 함수가이를 수신하도록 구성됩니다.
 

### 자원
 

리소스는 함수가 다음과 같이 사용하는 AWS 인프라 구성 요소입니다.
 

- AWS DynamoDB 테이블
 
- AWS S3 버킷
 
- CloudFormation에서 정의 할 수있는 모든 것
 

모든 기능, 이벤트 및 AWS 인프라 구성 요소는 서버리스 프레임 워크에 의해 자동으로 배포됩니다.
 

### 서비스
 verified_user

서비스는 조직의 단위입니다.
 함수,이를 트리거하는 이벤트, 함수에 필요한 리소스를 정의하는 프로젝트 파일과 같습니다.
 이들은 모두`serverless.yml` 파일에 정의되어 있으며 다음과 같습니다.
 

```coffeescript
# serverless.yml

service: todo

functions:
  todoCreate:
    events: # Events that trigger this function
      - http: post todo/create
  todoDelete:
    events:
      - http: delete todo/delete

resources: # Resources needed by your functions, Raw AWS CloudFormation goes in here.
```

### 플러그인
 

대부분의 개발자 커뮤니티에서와 같이 라이브러리 / 플러그인은 프로젝트의 기능을 확장하거나 덮어 쓸 필요가있을 때 항상 유용합니다.
 `serverless.yml` 파일은 필요한 모든 플러그인을 추가 할 수있는`plugins :`속성을 지원합니다.
 AWS AppSync로 작업 할 때 다음 섹션에서이 문제를 다룰 것입니다.
 

## 코드에
 

서론에서 언급했듯이 우리는 AWS 인프라 구성 요소를 활용할 샘플 할 일 API를 설정합니다.
 

- AppSync – 프런트 엔드에서 사용할 GraphQL 서버를 설정합니다.
 
- DynamoDB – 수신 요청의 데이터 저장
 

### 새 AppSync API 생성
 

먼저, GraphQL 스키마를 작성할 루트 폴더에`스키마 파일`을 만들어야합니다.
 

```shell
$ touch schema.graphql
```

편집기에서 파일을 열고 다음 스키마를 추가하십시오.
 

```js
type ToDo {
  createdAt: AWSDateTime
  id: ID
  updatedAt: AWSDateTime
  description: String!
  completed: Boolean
  dueDate: AWSDateTime!
}

input ToDoCreateInput {
  description: String!
  dueDate: AWSDateTime!
}

input ToDoUpdateInput {
  id: ID!
  description: String
  dueDate: AWSDateTime
  completed: Boolean
}

type Mutation {
  createTodo(input: ToDoCreateInput): ToDo
  updateTodo(input: ToDoUpdateInput): ToDo
  deleteTodo(id: ID!): ToDo
}

type Query {
  listToDos: [ToDo!]
  getToDo(id: ID): ToDo
}

schema {
  query: Query
  mutation: Mutation
}
```

새 AppSync API를 생성하려면 AppSync API를 배포, 업데이트 및 삭제하는 데 도움이되는`serverless-appsync-plugin`을 설치해야합니다.
 

```shell
$ yarn add serverless-appsync-plugin
```

그런 다음`serverless.yml`을 다음으로 업데이트합니다.
 

```undefined
service: serverless-introduction
app: serverless-introduction
org: brayoh
provider:
  name: aws
  runtime: nodejs12.x
  stage: ${opt:stage, env:stage, 'dev'}

plugins:
  - serverless-appsync-plugin
custom:
  appSync: # appsync plugin configuration
    name: ${self:service}-appsync-${self:provider.stage}
    authenticationType: API_KEY # since we dont have user login for now

functions:
  hello:
    handler: handler.hello
```

`plugins` 속성은 Yarn 또는 npm을 통해 설치 한 후 사용할 플러그인을 정의하는 곳입니다.
 

`stage`속성은 다음 세 가지 옵션으로 결정됩니다.
 

- `opt : stage` : 서비스에 배포 할 때`stage` 플래그를 전달할 수 있음을 의미합니다.
 
- `env : stage`, 즉`stage`라는 환경 변수가 있음을 의미합니다.
 
- 처음 두 옵션이 설정되지 않은 경우 기본 옵션
 

이를 통해 개발, 테스트 또는 프로덕션 등 여러 단계에 쉽게 배포 할 수 있습니다.
 설명서에서 환경 변수를 전달하는 방법에 대해 자세히 알아볼 수 있습니다.
 

`appSync` 속성은`serverless-appsync-plugin`에 대한 사용자 지정 구성을 정의하는 곳입니다.
 지금은 서비스 이름의 접두사가있는 API 이름을 추가했습니다.
 이 경우 AppSync 인 AWS 구성 요소
 및 현재 단계의 접미사 (예 :`serverless-introduction-appsync-prod`).
 

`authenticationType` 속성은 API에서 사용하려는 인증 방법입니다.
 사용자 로그인 기능이 없기 때문에 지금은 `API_KEY`로 설정되어 있습니다.
 그렇지 않으면`AMAZON_COGNITO_USERPOOL`로 설정했을 것입니다.
 `API 키`는 배포가 완료된 후 AppSync API 대시 보드에서 사용할 수 있습니다.
 

이제 앱을 다시 배포하여 새 AppSync API를 만들 수 있습니다.
 

```shell
$ serverless deploy
```

배포하는 데 몇 분 정도 걸립니다.
 배포가 완료되면 AWS AppSync 대시 보드로 이동하면 방금 배포 한 새 API가 표시됩니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/create-api-page.png?resize=730%2C456&ssl=1)

### 새 DynamoDB 데이터베이스 프로비저닝 및 생성
 

대박!
 이제 API가 배포되었으므로 데이터를 저장할 데이터 소스를 추가해야합니다.
 앞서 언급했듯이이 자습서에서는 DynamoDB를 사용합니다.
 

`resources`라는 폴더 안에 새 구성 파일을 만듭니다.
 

```shell
$ mkdir resources && touch resources/dynamo-table.yml
```

파일을 열고 DynamoDB 구성을 정의하는 다음 CloudFormation 템플릿을 추가합니다.
 

```php
Resources:
  PrimaryDynamoDBTable:
    Type: AWS::DynamoDB::Table
    Properties:
      AttributeDefinitions:
        - AttributeName: id
          AttributeType: S
      KeySchema: # primary composite key
        - AttributeName: id # partition key
          KeyType: HASH
      ProvisionedThroughput:
        ReadCapacityUnits: ${self:provider.tableThroughput}
        WriteCapacityUnits: ${self:provider.tableThroughput}
      BillingMode: PROVISIONED
      TableName: ${self:custom.resources.PRIMARY_TABLE}
      TimeToLiveSpecification:
        AttributeName: TimeToLive,
        Enabled: True
        Enabled: True
```

데이터베이스의 기본 키인 `ID`키를 지정합니다.
 또한 프로젝트 요구 사항 및 예산에 따라 선택하는 `읽기`및 `쓰기`용량 단위에 필요한 테이블 이름과 처리량도 지정합니다.
 

다음으로 위의 구성을 포함하도록`serverless.yml`을 업데이트해야합니다.
 

```php
# previous config....
provider:
  name: aws
  runtime: nodejs12.x
  region: eu-west-1
  stage: ${opt:stage, env:stage, 'dev'}
  tableThroughputs:
    default: 5
    prod: 10

custom:
  resources:
    PRIMARY_TABLE: ${self:service}-dynamo-table-${self:provider.stage}

  appSync: # appsync plugin configuration
    name: ${self:service}-appsync-${self:provider.stage}
    authenticationType: API_KEY # since we don't have user login for now
    dataSources:
      - type: AMAZON_DYNAMODB
        name: PrimaryTable
        description: "Primary Table"
        config:
          tableName: ${self:custom.resources.PRIMARY_TABLE}
          serviceRoleArn: { Fn::GetAtt: [AppSyncDynamoDBServiceRole, Arn] }
```

다음.
 새 IAM 역할을 프로비저닝하는 새 `CFT`를 추가해야합니다.
 이는 `AppSync`에서 새 데이터 소스를 생성하고 필요한 작업을 수행하는 데 필요한 권한을 부여하는 데 사용됩니다.
 

```shell
$ touch resources/appsync-dynamodb-role.yml
```

다음 내용을 추가하십시오.
 

```bash
Resources:
  AppSyncDynamoDBServiceRole:
    Type: "AWS::IAM::Role"
    Properties:
      RoleName: ${self:service}-dynamo-role-${self:provider.stage}
      AssumeRolePolicyDocument:
        Version: "2012-10-17"
        Statement:
          - Effect: "Allow"
            Principal:
              Service:
                - "appsync.amazonaws.com"
                - "dynamodb.amazonaws.com"
            Action:
              - "sts:AssumeRole"
      Policies:
        - PolicyName: ${self:service}-dynamo-policy-${self:provider.stage}
          PolicyDocument:
            Version: "2012-10-17"
            Statement:
              - Effect: "Allow"
                Action:
                  - dynamodb:DescribeTable
                  - dynamodb:Query
                  - dynamodb:Scan
                  - dynamodb:GetItem
                  - dynamodb:PutItem
                  - dynamodb:UpdateItem
                  - dynamodb:DeleteItem
                Resource:
                  - "arn:aws:dynamodb:${self:provider.region}:*:*" # allow creation of tables in our current region
```

또한 배포에 사용되는 CloudFormation 역할에 사용할 수있는 권한을 지정해야합니다.
 이렇게하려면`tableThroughput` 속성 바로 아래에있는`serverless.yml` 파일을 업데이트하여 다음 권한을 포함합니다.
 

```php
tableThroughput: ${self:provider.tableThroughputs.${self:provider.stage}, self:provider.tableThroughputs.default}  
iamRoleStatements:
    - Effect: Allow
      Action:
        - dynamodb:DescribeTable
        - dynamodb:Query
        - dynamodb:Scan
        - dynamodb:GetItem
        - dynamodb:PutItem
        - dynamodb:UpdateItem
        - dynamodb:DeleteItem
      Resource:
        - "arn:aws:dynamodb:${self:provider.region}:*:*" 
```

마지막 단계는 이제 두 템플릿을 모두 리소스 섹션에서 가져 와서`serverless.yml` 파일에 포함시키는 것입니다. 이상적으로는 파일의 마지막 섹션이어야합니다.
 

```bash
resources:
  - ${file(./resources/appsync-dynamo-role.yml)}
  - ${file(./resources/dynamo-table.yml)}
```

이제 모든 설정이 완료되었으므로 이제 새로운 변경 사항을 적용하기 위해 애플리케이션을 배포 할 수 있습니다.
 배포가 완료되면 DynamoDB 콘솔에 로그인합니다.
 다음을 보여주는 새 테이블이 있어야합니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/new-table-appears.png?resize=730%2C410&ssl=1)

### 매핑 템플릿 만들기
 

이제`AppSync API` 및`DynamoDB` 테이블이 모두 설정되었으므로 쿼리 및 변형을위한 매핑 템플릿 생성으로 이동합니다.
 

매핑 템플릿은 VTL (Velocity Template Language)로 작성되고 JSONPath 표현식을 사용하여 페이로드에 적용되는 스크립트입니다.
 매핑 템플릿을 사용하여 메서드 요청에서 해당 통합 요청으로, 통합 응답에서 해당 메서드 응답으로 페이로드를 매핑합니다.
 여기에서 DynamoDB 용 매핑 템플릿에 대해 자세히 알아볼 수 있습니다.
 

프로젝트에 매핑 템플릿을 추가하려면 먼저 폴더를 만듭니다.
 

```shell
$ mkdir mapping-templates
```

서버리스 파일에 새 매핑 템플릿을 추가하려면 다음 네 가지 속성이 필요합니다.
 

```coffeescript
- dataSource: our_data_source_here
  type: Mutation or Query
  field: the graphql mutation or query name  
  request: path_to_request_mapping_template
  response: path_to_response_mapping_template
```

`createToDo` 변형에 대한 요청 매핑 템플릿을 생성하는 것으로 시작합니다.
 

```shell
$ mkdir mapping-templates/create_todo && touch mapping-templates/create_todo/request.vtl
```

다음 코드를 추가합니다.
 

```undefined
$util.qr($ctx.args.input.put("createdAt", $util.time.nowISO8601()))
$util.qr($ctx.args.input.put("updatedAt", $util.time.nowISO8601()))
{
    "version" : "2017-02-28",
    "operation" : "PutItem",
    "key" : {
        "id": $util.dynamodb.toDynamoDBJson($util.autoId())
    },
    "attributeValues" : $util.dynamodb.toMapValuesJson($ctx.args.input)
}
```

여기서 우리가하는 일은 GraphQL 서버에서 들어오는 변형 요청에서 얻은 데이터가 DynamoDB에 저장되는 방식을 처리하는 것입니다.
 할 일 항목의 `ID`를 자동 생성 한 다음 페이로드에 `createdAt`및 `updatedAt`이라는 두 개의 타임 스탬프를 추가합니다.
 다른 속성 값, 즉`description` 및`dueDate`는 동등한 데이터베이스 행에 매핑됩니다.
 

모든 요청에 대해 응답 매핑 템플릿을 반복하는 대신 모든 요청에서 재사용 할 두 개의 파일을 만듭니다.
 중괄호로 묶인 쉼표로 구분 된 목록을 사용하여 동일한 확장자를 가진 여러 파일을 만들 수있는이 간단한 트릭을 사용하여이를 수행 할 수 있습니다.
 

```shell
$ touch mapping-templates/{common-item-response,common-items-response}.vtl
```

#### `common-item-response.vtl`
 

이것은 단일 항목을 반환하는 모든 데이터베이스 작업에 대해 다시 보내는`JSON `응답입니다.
 

```shell
$util.toJson($ctx.result)
```

#### `common-items-response.vtl`
 

이것은 여러 항목을 반환하는 모든 데이터베이스 작업에 대해 다시 보내는 JSON 응답입니다.
 

```shell
$util.toJson($ctx.result.items)
```

이제 매핑 템플릿 속성으로 서버리스 파일을 업데이트하고 위의 파일을 추가하여 새 할 일을 만들기위한 변형을 처리 할 수 있습니다.
 

```bash
appSync: # appsync plugin configuration
  name: ${self:service}-appsync-${self:provider.stage}
  authenticationType: API_KEY # since we don't have user login for now
  dataSources:
    - type: AMAZON_DYNAMODB
      name: PrimaryTable
      description: "Primary Table"
      config:
        tableName: ${self:custom.resources.PRIMARY_TABLE}
        serviceRoleArn: { Fn::GetAtt: [AppSyncDynamoDBServiceRole, Arn] }
  mappingTemplates:
    - dataSource: PrimaryTable
      type: Mutation
      field: createTodo
      request: "create_todo/request.vtl"
      response: "common-item-response.vtl"
```

그런 다음 이제 서비스를 배포하여 새 리졸버를 만들 수 있습니다.
 

```shell
$ serverless deploy
```

배포가 완료되면 GraphQL 플레이 그라운드의 로컬 인스턴스를 회전시켜 API를 테스트 할 수 있습니다.
 

```shell
$ serverless graphql-playground
```

그러면`localhost : 3000`에서 실행되는 새 플레이 그라운드가 설정됩니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/new-playground.png?resize=730%2C453&ssl=1)

다음 변형을 추가합니다.
 

```bash
mutation createTodo($input: ToDoCreateInput!) {
  createTodo(input: $input) {
    id
    createdAt
    updatedAt
    completed
    description
    dueDate
  }
}
```

그런 다음 쿼리 변수 섹션 아래에 페이로드를 추가합니다.
 `dueDate`의 경우 DynamoDB의 예상 날짜 형식과 일치하도록 다음 날짜 형식`YYYY-MM-DDThh : mm : ss.sssZ`를 사용해야합니다.
 

변형을 성공적으로 실행 한 후 DynamoDB 콘솔로 이동하여 테이블을 엽니 다.
 다음 항목이 표시되어야합니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/one-item-for-selection.png?resize=730%2C455&ssl=1)

우리는 해냈다!
 첫 번째 항목을 데이터베이스에 성공적으로 저장했습니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/spongebob-squarepants-and-patrick-star.gif?resize=320%2C198&ssl=1)

대부분의 템플릿은 위의 템플릿과 동일하므로 한 번에 모두 추가합니다.
 

#### `get_todo / request.vtl`
 

할 일 항목을 가져 오려면 쿼리 페이로드에 ID가 필요합니다. 그런 다음 DynamoDB 해석기에 전달합니다.
 

```undefined
{
    "version": "2017-02-28",
    "operation": "GetItem",
    "key": {
        "id": $util.dynamodb.toDynamoDBJson($ctx.args.id),
    }
}
```

#### `update_todo / request.vtl`
 

항목을 업데이트하는 것은 다소 복잡하지만 가능한 한 쉽게 분류하도록 노력하겠습니다.
 항목을 가져 오는 것과 마찬가지로 업데이트 할 때 ID 필드가 페이로드의 일부 여야합니다.
 그런 다음 업데이트 표현식을 동적으로 생성합니다.
 값이 null이면 생략합니다.
 제공된 값 중 하나라도 생략되면 해당 값도 제거됩니다.
 

마지막으로 작업에는 현재 DynamoDB에있는 항목에`expectedVersion`으로 설정된`version` 필드가 있는지 여부를 확인하는 조건이 있습니다.
 

```undefined
$util.qr($ctx.args.input.put("updatedAt", $util.time.nowISO8601()))
{
    "version" : "2017-02-28",
    "operation" : "UpdateItem",
    "key" : {
        "id" : { "S" : "${context.arguments.input.id}" }
    },
    ## Set up some space to keep track of things we're updating **
    #set( $expNames  = {} )
    #set( $expValues = {} )
    #set( $expSet = {} )
    #set( $expRemove = [] )
    #set($ar=[])

    ## Iterate through each argument, skipping "id" and "expectedVersion" **
    #foreach( $entry in $context.arguments.input.entrySet() )
        #if( $entry.key != "id" )
            #if( (!$entry.value) && ("$!{entry.value}" == "") )
                ## If the argument is set to "null", then remove that attribute from the item in DynamoDB **
                #set( $discard = ${expRemove.add("#${entry.key}")} )
                $!{expNames.put("#${entry.key}", "$entry.key")}
            #else
                ##Otherwise set (or update) the attribute on the item in DynamoDB **
                $!{expSet.put("#${entry.key}", ":${entry.key}")}
                $!{expNames.put("#${entry.key}", "$entry.key")}
                $!{expValues.put(":${entry.key}", { "S" : "${entry.value}" })}
            #end
        #end
    #end
    ## Start building the update expression, starting with attributes we're going to SET **
    #set( $expression = "" )
    #if( !${expSet.isEmpty()} )
        #set( $expression = "SET" )
        #foreach( $entry in $expSet.entrySet() )
            #set( $expression = "${expression} ${entry.key} = ${entry.value}" )
            #if ( $foreach.hasNext )
                #set( $expression = "${expression}," )
            #end
        #end
    #end
    ## Continue building the update expression, adding attributes we're going to REMOVE **
    #if( !${expRemove.isEmpty()} )
        #set( $expression = "${expression} REMOVE" )
        #foreach( $entry in $expRemove )
            #set( $expression = "${expression} ${entry}" )
            #if ( $foreach.hasNext )
                #set( $expression = "${expression}," )
            #end
        #end
    #end
    ## Finally, write the update expression into the document, along with any expressionNames and expressionValues **
    "update" : {
        "expression" : "${expression}"
        #if( !${expNames.isEmpty()} )
            ,"expressionNames" : $utils.toJson($expNames)
        #end
        #if( !${expValues.isEmpty()} )
            ,"expressionValues" : $utils.toJson($expValues)
        #end
    }
}
```

#### `delete_todo / request.vtl`
 

항목을 가져 오는 것과 마찬가지로 업데이트 할 때 ID 필드가 페이로드의 일부가되어야합니다. 그런 다음 DB에서 항목을 하드 삭제하기 위해 새 페이로드를 DynamoDB에 전달합니다.
 

```undefined
{
    "version" : "2017-02-28",
    "operation" : "DeleteItem",
    "key" : {
        "id":  $util.dynamodb.toDynamoDBJson($ctx.args.id),
    }
}
```

이제 모든 매핑 템플릿이 준비되었으므로 서버리스 파일에 추가 할 수 있습니다.
 

```bash
- dataSource: PrimaryTable
  type: Mutation
  field: updateTodo
  request: "update_todo/request.vtl"
  response: "common-item-response.vtl"
- dataSource: PrimaryTable
  type: Mutation
  field: deleteTodo
  request: "delete_todo/request.vtl"
  response: "common-item-response.vtl"
- dataSource: PrimaryTable
  type: Query
  field: getToDo
  request: "get_todo/request.vtl"
  response: "common-item-response.vtl"
- dataSource: PrimaryTable
  type: Query
  field: listToDos
  request: "list_todos/request.vtl"
  response: "common-items-response.vtl"
```

서비스를 다시 배포하면 이제 `CRUD`작업과 함께 완전히 작동하는 `API`가 생깁니다.
 

CRUD 작업을 테스트하려면 플레이 그라운드에서 테스트 할 수있는 변형과 쿼리가 필요합니다.
 

## 쿼리 및 변형
 

### `updateToDo`
 

이 변형을 테스트하려면 기존 항목의 ID를 선택한 다음 `description`, `dueDate`및 `completed`와 같은 업데이트 가능한 필드 중 하나를 업데이트해야합니다.
 

```bash
mutation UpdateToDo($input: ToDoUpdateInput) {
  updateTodo(input: $input) {
    id
    createdAt
    description
    updatedAt
    dueDate
    completed
  }
}
```

그런 다음 업데이트하려는 값을 전달하고 변형을 실행합니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/running-the-mutation.png?resize=730%2C455&ssl=1)

### `listToDos`
 

DB의 모든 할 일 항목을 가져 오려면 다음 쿼리를 실행하십시오.
 

```undefined
query ListTodos {
  listToDos {
    id
    createdAt
    description
    updatedAt
    dueDate
    completed
  }
}
```

사용 가능한 모든 할 일의 배열을 반환해야합니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/array-of-to-dos.png?resize=730%2C453&ssl=1)

### `getToDo`
 

단일 항목을 가져 오려면 다음 쿼리를 실행하십시오.
 

```undefined
query GetToDo($id: ID) {
  getToDo(id: $id) {
    id
    createdAt
    description
    updatedAt
    dueDate
    completed
  }
}
```

주어진 ID로 항목을 반환해야합니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/item-with-given-id.png?resize=730%2C457&ssl=1)

### `deleteToDo`
 

DB에서 항목을 삭제하려면 다음 쿼리를 실행하십시오.
 

```undefined
mutation DeleteToDo($id: ID!) {
  deleteTodo(id: $id) {
    id
    createdAt
    description
    updatedAt
    dueDate
    completed
  }
}
```

성공적인 실행시 할 일 항목을 반환해야합니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/return-to-do-item.png?resize=730%2C455&ssl=1)

## 프런트 엔드 구축
 

이 기사의 마지막 부분에서는 CRUD 작업을위한 API와 상호 작용할 새로운 React 앱을 만들 것입니다.
 

이것은 단순한 데모 일 뿐이므로 create-react-app을 사용하여 프로젝트 폴더 내에서 새로운 반응 앱을 부트 스트랩하는 것으로 시작합니다.
 

```shell
$ create-react-app client
```

그 후 필요한 모든 종속성을 설치합니다.
 

```undefined
$ cd client && yarn add antd styled-components aws-appsync-subscription-link aws-appsync-auth-link @apollo/client graphql aws-appsync
```

`index.js` 파일을 열고 다음과 같이 앱 구성을 추가합니다.
 

```js
import React from "react";
import ReactDOM from "react-dom";
import { createAuthLink } from "aws-appsync-auth-link";
import { createSubscriptionHandshakeLink } from "aws-appsync-subscription-link";
import { AUTH_TYPE } from "aws-appsync";
import {
  ApolloProvider,
  ApolloClient,
  ApolloLink,
  InMemoryCache,
} from "@apollo/client";

/** Ant design */
import "antd/dist/antd.css";

/** App entry */
import App from "./App";

/** AWS config */
import AppSyncConfig from "./aws-exports";

const config = {
  url: AppSyncConfig.GraphQlApiUrl,
  region: process.env.REACT_APP_REGION,
  auth: {
    type: AUTH_TYPE.API_KEY,
    apiKey: AppSyncConfig.GraphQlApiKeyDefault,
  },
};
const client = new ApolloClient({
  link: ApolloLink.from([
    createAuthLink(config),
    createSubscriptionHandshakeLink(config),
  ]),
  cache: new InMemoryCache(),
  defaultOptions: {
    watchQuery: {
      fetchPolicy: "cache-and-network",
    },
  },
});

ReactDOM.render(
  <ApolloProvider client={client}>
    <App />
  </ApolloProvider>,
  document.getElementById("root")
);
```

위의 구성을 추가 한 후 앱 URL로 돌아가 기능이 손상되지 않았는지 확인합니다.
 

이 기사는 학습용이므로 가능한 한 최소한의 설계를 유지하고 데모 용으로 만 단일 구성 요소로 작업 할 것입니다.
 우리는 항상 가능한 한 간단하게 구성 요소를 분해해야합니다.
 

React Apollo 버전 3을 사용할 것이므로 Hooks 기능을 활용하여 구현을 매우 깔끔하게 유지합니다.
 구성 요소는 다음과 같습니다.
 

```js
import React from "react";
import styled from "styled-components";
import { List, Checkbox, Input, Button, Popconfirm, message } from "antd";
import { useMutation } from "@apollo/client";

/** App theme */
import Colors from "../../theme/colors";

/** GraphQL Queries */
import updateToDo from "../../graphql/mutations/updateToDo";
import createToDo from "../../graphql/mutations/createToDo";
import deleteToDo from "../../graphql/mutations/deleteToDo";
import listToDos from "../../graphql/queries/listToDos";

const DataList = (props) => {
  const [description, updateDescription] = React.useState("");
  const [updateToDoMutation] = useMutation(updateToDo);
  const [createToDoMutation] = useMutation(createToDo);
  const [deleteToDoMutation] = useMutation(deleteToDo);
  const { data } = props;

  function handleCheck(event, item) {
    event.preventDefault();
    const completed =
      typeof item.completed === "boolean" ? !item.completed : true; 

    updateToDoMutation({
      variables: { input: { completed, id: item.id } },
      refetchQueries: [
        {
          query: listToDos,
        },
      ],
    })
      .then((res) => message.success("Item updated successfully"))
      .catch((err) => {
        message.error("Error occurred while updating item");
        console.log(err);
      });
  }

  function handleSubmit(event, item) {
    event.preventDefault();
    createToDoMutation({
      variables: { input: { description } },
      refetchQueries: [
        {
          query: listToDos,
        },
      ],
    })
      .then((res) => message.success("Item created successfully"))
      .catch((err) => {
        message.error("Error occurred while creating item");
        console.log(err);
      });
  }

  function handleKeyPress(event) {
    if (event.keyCode === 13) {
      // user pressed enter
      createToDoMutation({
        variables: { input: { description } },
        refetchQueries: [
          {
            query: listToDos,
          },
        ],
      })
        .then((res) => {
          message.success("Item created successfully");
        })
        .catch((err) => {
          message.error("Error occurred while creating item");
          console.log(err);
        });
    }
  }

  function handleDelete(event, item) {
    deleteToDoMutation({
      variables: { id: item.id },
      refetchQueries: [
        {
          query: listToDos,
        },
      ],
    })
      .then((res) => {
        message.success("Deleted successfully");
      })
      .catch((err) => {
        message.error("Error occurred while deleting item");
        console.log(err);
      });
  }

  return (
    <ListContainer>
      <List
        header={
          <div style={ display: "flex" }>
            <Input
              placeholder="Enter todo name"
              value={description}
              onChange={(event) => updateDescription(event.target.value)}
              style={ marginRight: "10px" }
              onKeyDown={handleKeyPress}
            />
            <Button name="add" onClick={handleSubmit}>
              add
            </Button>
          </div>
        }
        bordered
        dataSource={data}
        renderItem={(item) => (
          <List.Item>
            <Checkbox
              checked={item.completed}
              onChange={(event) => handleCheck(event, item)}
            >
              {item.description}
            </Checkbox>
            <Popconfirm
              title="Are you sure to delete this item?"
              onConfirm={(event) => handleDelete(event, item)}
              okText="Yes"
              cancelText="No"
            >
              <DeleteAction>Delete</DeleteAction>
            </Popconfirm>
          </List.Item>
        )}
      />
    </ListContainer>
  );
};

export default DataList;
```

앞서 언급했듯이 아래에 표시된 것처럼 디자인을 최소화하기로 선택했습니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/frontend-app.png?resize=730%2C445&ssl=1)

## 결론
 

이 튜토리얼을 즐겼고 유용하다는 것을 알게 되었기를 바랍니다. 이러한 모든 리소스를 모으기 위해 얼마나 많은 작업을 수행해야했는지 기억합니다. 시작할 때 항상 바쁘기 때문에 모든 것을 하나의 기사로 컴파일하려고했습니다.
 전체 프로젝트에 대한 저장소는 여기에서 찾을 수 있습니다.
 