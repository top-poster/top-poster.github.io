---
layout: post
title: "CI / CD 및 React : Heroku 및 CircleCI를 사용하여 파이프 라인 생성
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/Screen-Shot-2021-01-28-at-2.49.29-PM.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Screen-Shot-2021-01-28-at-2.49.29-PM.png?fit=730%2C484&ssl=1)

React 애플리케이션, 특히 더 큰 앱을 빌드 할 때 다양한 Git 브랜치 및 다양한 환경 (스테이징, 개발, 프로덕션 등)에서 다양한 개발자 팀과 함께 작업하는 경우가 많습니다.
 많은 손이 가벼운 작업을 할 수 있지만 웹 개발에서는 많은 개별 코드가 엄청난 골칫거리를 만들 수 있습니다.
 

분기 병합 및 기존 코드베이스에 새로운 변경 사항 통합과 같은 인스턴스 처리는 일부 개발자가 "통합 지옥"이라고 부르는 것이 될 수 있습니다.
 고맙게도 이러한 번거 로움은 강력한 CI / CD 파이프 라인을 통해 쉽게 피하거나 최소한 최소화 할 수 있습니다.
 

## 기초 : CI 및 CD 이해
 

데모를 시작하기 전에 CI 및 CD의 기본 사항을 살펴 보겠습니다.
 메모리를 새로 고치기 위해 지속적 통합 (CI) 및 지속적 전달 (CD)은 DevOps에서 사용되는 일련의 자동화 된 단계입니다.
 

### 지속적인 통합
 

CI는 코드 변경 사항을 개발 팀의 여러 구성원을 하나의 중앙 저장소 또는 프로젝트로 통합하는 일관되고 자동화 된 방법을 만듭니다.
 

앞서 언급했듯이 대규모 프로젝트는 일반적으로 개발자가 다른 기능과 분기를 작업 한 다음 하나의 공유 분기로 병합해야합니다.
 개발자가 업데이트 된 작업을 확인하면 (예 : pull 요청을 통해) 애플리케이션을 자동으로 빌드하고 다른 테스트 (일반적으로 단위 및 통합 테스트 모두)를 실행하여 변경 사항이 유효성을 검사하여 변경 사항이
 신청.
 그런 다음 팀은 테스트 상태에 대해 즉시 알림을 받고 결과에 따라 변경 사항을 주요 작업에 병합 할 수 있습니다.
 

아래 다이어그램은 일반적인 CI 흐름을 보여줍니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Typical-Continuous-Integration-Flow-Diagram.png?resize=457%2C334&ssl=1)

좋은 CI 시스템은 개발자가 작성한 테스트에 크게 의존한다는 점에 유의해야합니다.
 여기에서 React에서 테스트 작성을 시작할 수 있습니다.
 

### 지속적인 전달
 

지속적 통합 이후에는 당연히 CD (지속적 배포)가 제공됩니다.
 CD는 새로운 기능 또는 분기가 성공적으로 병합 된 직후에 애플리케이션의 자동 배포를 보장합니다.
 

단위 및 통합 테스트를 통과하고 분기가 기본 공유 분기와 병합되면 CD는 애플리케이션을 대상 환경에 자동으로 배포합니다.
 이것은 프로덕션 환경이거나 라이브 테스트에 사용되는 스테이징 환경 일 수 있습니다.
 

## React 앱을위한 CI / CD 파이프 라인 생성
 

이제 CI 및 CD의 기본 사항을 이해 했으므로 지식을 실행하여 React 애플리케이션을위한 CI / CD 파이프 라인을 만들 수 있습니다.
 이 데모에서는 git 및 GitHub에 대한 기본 지식이 있다고 가정합니다.
 또한 CI / CD 용 CircleCI와 호스팅 서비스로 Heroku를 사용할 예정입니다.
 

시작하기 전에 몇 가지 기본 설정 단계가 있습니다.
 

- 아직 계정이없는 경우 GitHub 및 Heroku 계정을 만듭니다.
 
- GitHub 계정을 CircleCI에 연결
 
- 이 프로젝트에 대한 예제 저장소를 포크하고 복제합니다.
 

### CircleCI와 Heroku는 무엇입니까?
 

CircleCI는 저장소 (GitHub, GitHub Enterprise 또는 Bitbucket)에 통합 할 수있는 CI / CD 서비스입니다.
 리포지토리에 코드를 커밋 할 때마다 config.yml 파일에서 생성 한 파이프 라인을 생성 및 실행하고 테스트에 대한 상태 보고서를 제공합니다.
 CircleCI는 오픈 소스 프로젝트 (GitHub에 공개 된 프로젝트 포함)에 대해 무료입니다.
 

Heroku는 클라우드 호스팅 서비스입니다.
 Heroku에서 응용 프로그램을 호스팅하고 업데이트 될 때마다 응용 프로그램을 Heroku URL에 자동으로 배포하도록 CircleCI를 구성 할 것입니다.
 

### React 애플리케이션 설정
 

애플리케이션을 복제 한 후 다음 명령을 실행하여 시작하십시오.
 

```coffeescript
npm install
npm start
```

앱은 포트 3000 (사용 가능한 경우)에서 시작해야합니다.
 성공하면 다음과 같은 견적 생성기가 표시됩니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/React-Application-Setup-Quote-Generator.png?resize=730%2C411&ssl=1)

이 앱은 파일에서 The Simpsons의 따옴표를 검색하여 무작위로 제공합니다.
 화살표 버튼을 클릭하여 새 견적을 생성하거나 Twitter 아이콘을 클릭하여 견적을 트윗 할 수 있습니다.
 

### Heroku 호스팅 환경 설정
 

먼저 React 앱을 호스팅 할 새로운 Heroku 애플리케이션을 만듭니다.
 나머지 배포에서는 명령 줄을 통해 작업 할 것입니다.
 

아직 설치하지 않은 경우 다음 명령을 실행하여 Heroku CLI를 전역으로 설치해야합니다.
 

```coffeescript
npm i -g heroku
```

그런 다음 다음을 실행하여 명령 줄 (브라우저 창이 열림)을 통해 Heroku에 로그인 할 수 있습니다.
 

```undefined
heroku login
```

빌드 팩을 사용하여 React 앱을 배포 할 수 있습니다.
 빌드 팩은 Heroku에서 앱을 컴파일하는 데 사용되는 스크립트 세트입니다.
 배포를 더 쉽게 만들고 일반적으로 오픈 소스입니다.
 React 앱의 경우 create-react-app 용 빌드 팩을 사용할 것입니다.
 

다음 단계는 다음 명령과 함께 빌드 팩을 사용하여 명령 줄을 통해 Heroku 애플리케이션을 만드는 것입니다 (`$ APP_NAME`을 선호하는 앱 이름으로 변경).
 

```undefined
heroku create $APP_NAME --buildpack https://github.com/mars/create-react-app-buildpack.git
```

이 데모의 경우 https://APP_NAME.herokuapp.com URL에서 애플리케이션을 만듭니다.
 URL을 방문하면 스톡 Heroku 페이지가 환영합니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Stock-Heroku-Welcome-Page.png?resize=730%2C98&ssl=1)

### Git 저장소 확인
 

명령 줄에서 알 수 있듯이 Heroku는 애플리케이션에 대한 git 저장소도 생성합니다.
 Git remote -v를 입력하여이를 확인할 수 있습니다.
 오리진 및 Heroku 원격 분기가 모두 표시됩니다.
 

이 시점에서 코드 (수정 된 경우)를 추가하고 커밋 한 후 다음 단계를 통해 원격 Heroku git 저장소로 푸시합니다.
 

```undefined
git add .
git commit -m 'commit message here'
```

현재 마스터 브랜치에있는 경우 다음을 사용하여 Heroku로 직접 푸시 할 수 있습니다.
 

```undefined
git push heroku master
```

그렇지 않은 경우 다음 명령을 사용하여 현재 브랜치에서 Heroku 마스터 브랜치로 푸시합니다 (‘$ BRANCH_NAME’을 현재 브랜치로 바꾸는 것을 잊지 마십시오).
 

```undefined
git push heroku $BRANCH_NAME:master
```

그런 다음 Heroku가 앱을 푸시하고 https://APP_NAME.herokuapp.com에 배포합니다.
 웹 사이트를 방문하여 앱이 라이브인지 확인할 수 있습니다.
 

이제 애플리케이션을 Heroku에 성공적으로 배포 했으므로 CircleCI를 설정할 차례입니다.
 

### CircleCI로 React에서 CI / CD 구성
 

다음 단계는 CircleCI를 구성하는 것입니다.
 시작하려면 GitHub 프로필로 CircleCI에 로그인합니다.
 

프로젝트 사이드 탭에 모든 공개 GitHub 프로젝트 목록이 표시됩니다.
 CircleCI를 설정하려면 "Set up projects"를 클릭합니다.
 "구성 파일 추가"또는 "기존 구성 사용"옵션이있는 편집기 팝업이 표시되어야합니다.
 "기존 구성 사용"옵션을 선택합니다.
 이는 표시된 샘플을 사용하지 않고 구성 파일을 수동으로 설정하고 있음을 나타냅니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Manual-Setup-of-Configuration-File-Pop-Up.png?resize=730%2C329&ssl=1)

“기존 구성 사용”을 선택하면 위 이미지와 같은 창이 나타납니다.
 "구축 시작"을 클릭하십시오.
 아직 구성 속성을 설정하지 않았으므로 빌드 시작을 클릭하면 처음에 실패 할 빌드가 시작됩니다.
 

올바른 구성을 설정하려면 Heroku 애플리케이션에 대한 환경 변수를 만들어야합니다.
 프로젝트의 오른쪽 상단에있는 프로젝트 설정 버튼을 클릭 한 다음 환경 변수 사이드 메뉴를 클릭하면됩니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Project-Settings-Environmental-Variables-Display.png?resize=730%2C43&ssl=1)

여기에서`HEROKU_APP_NAME` 및`HEROKU_API_KEY`를 설정합니다.
 `HEROKU_APP_NAME`은 Heroku 애플리케이션 (빌드 팩을 사용하여 생성 한 애플리케이션)의 이름입니다. `HEROKU_API_KEY`는 Heroku에 가입 한 후 생성 된 키이며 비밀로 유지해야합니다. Heroku API 키는
 계정 설정, 페이지 끝에 가깝습니다. 숨겨져 있지만 옆에있는 공개 버튼을 클릭하면 표시 할 수 있습니다.
 

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Heroku-API-Key-Location.png?resize=730%2C92&ssl=1)

나중에 왜 필요한지 다시 설명하겠습니다.
 

이제 완료되었으므로 CircleCI 구성을 로컬로 설정합니다.
 프로젝트 폴더의 루트에`.circleci`라는 이름의 폴더를 만듭니다 (마침표 참고).
 그 폴더 안에`config.yml` 파일을 생성합니다.
 

.yml 파일 내부에 다음 코드를 붙여 넣습니다.
 

```undefined
version: 2.1

orbs:
  heroku: circleci/heroku@0.0.10

jobs:
  build:
    docker:
      - image: circleci/node:10.16.3
    working_directory: ~/repo
    steps:
      - checkout
      # install dependencies
      - run:
          name: Install Dependencies
          command: npm install
      - run:
          name: Run tests
          command: npm run test

workflows:
  heroku_deploy:
    jobs:
      - build
      - heroku/deploy-via-git: # Use the pre-configured job, deploy-via-git
          requires:
            - build
          filters:
            branches:
              only: main
```

처음에는이 코드 스 니펫이 많이 보일 수 있지만 분석하면 실제로는 매우 간단합니다.
 

`버전`부터 시작하겠습니다.
 이것은 우리가 실행중인 CircleCI의 버전을 나타냅니다.
 각 버전에는 다른 버전에는 없을 수있는 기능이 있으므로 빌드 버전을 명시해야합니다.
 데모에서는 버전 2.1을 사용하고 있습니다.
 

다음은 `구슬`입니다.
 Heroku 용 빌드 팩을 기억하십니까?
 Orbs는 CircleCI와 비슷합니다.
 Orbs는 프로젝트간에 재사용 할 수있는 재사용 가능한 CircleCI 구성을 포함하는 준비된 패키지입니다.
 프로젝트에서는 Git 저장소를 통해 앱을 배포하고 많은 구성을 건너 뛸 수 있도록 도와주는`heroku / deploy-via-git`과 같은 작업이 포함 된 Heroku orb를 사용할 것입니다.
 

다음 단계 :`jobs`.
 작업은 CircleCI 빌드 프로세스에서 실행되는 단계 모음입니다.
 구성 파일에서 종속성 설치와 테스트 실행의 두 단계로 구성된 하나의 작업 (빌드)을 만들었습니다.
 우리가 사용하고있는 Heroku 오브에는 워크 플로우 섹션에서 사용하는 작업도 있습니다 :`heroku / deploy-via-git`.
 

마지막으로 워크 플로는 작업 모음 및 실행 순서를 정의하기위한 규칙 집합을 나타냅니다.
 워크 플로는 또한 다른 작업을 실행하는 데 필요한 작업을 나타냅니다.
 예를 들어`heroku / deploy-via-git` 작업을 수행하려면 빌드 작업이 성공해야합니다.
 따라서 테스트를 통과하지 않으면 애플리케이션이 Heroku에 배포되지 않습니다.
 또한 작업을 메인 브랜치로 제한했기 때문에 코드는이 브랜치의 변경 사항에만 배포됩니다.
 

### React 및 Enzyme을 사용하여 테스트 작성
 

이 단계에서 파일을있는 그대로 커밋하고 푸시하면 테스트가 실패하므로 오류가 발생합니다.
 이 섹션에서는 React 테스트 라이브러리와 Enzyme을 사용하여 App.test.js 파일에 몇 가지 테스트를 작성합니다.
 

먼저 Enzyme을 설치합니다.
 

```coffeescript
npm i enzyme
```

그런 다음 React 버전에 따라 Enzyme 용 어댑터를 설치해야합니다 (아래 참조).
 npm 홈페이지에서 더 많은 어댑터를 찾을 수 있습니다.
 

```coffeescript
# For React 17
npm i @wojtekmaj/enzyme-adapter-react-17

# For react 16.4 
npm i enzyme-adapter-react-16
```

이제 App.test.js에 다음 테스트를 추가합니다.
 

```coffeescript
import { render } from '@testing-library/react';
import Enzyme, { mount } from 'enzyme';
import App from './App';
import Quotes from './components/Quotes';

// Add your adapter version below
import Adapter from '@wojtekmaj/enzyme-adapter-react-17';

Enzyme.configure({ adapter: new Adapter() });

test('displays a quote', () => {
  render(<App />);
  const quote = document.querySelector('#text p');
  expect(quote).toBeInTheDocument();
  expect(quote).not.toBeEmptyDOMElement();
});

it('calls generateRandomQuote prop function when next button is clicked', () => {
  const generateRandomQuoteFn = jest.fn();
  const quote = mount(
    <Quotes generateRandomQuote={generateRandomQuoteFn} quote={{}} />
  );
  const generateBtn = quote.find('#new-quote');

  generateBtn.simulate('click');
  expect(generateRandomQuoteFn).toHaveBeenCalledTimes(1);
});
```

테스트가 설정되었으므로 이제 코드를 커밋하고 저장소로 푸시 할 수 있습니다.
 

```undefined
git add .
git commit -m 'add circleci config'
git push
```

그러면 성공해야하는 CircleCI에서 다른 빌드가 트리거됩니다.
 그렇다면 다음과 같은 모든 녹색 확인 표시가 나타납니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Successful-CircleCI-Display-.png?resize=730%2C331&ssl=1)

### 풀 요청 관리
 

다음으로 빌드가 통과하지 않는 한 pull 요청이 병합되지 않도록하여 기본 GitHub 브랜치가 보호되는지 확인합니다.
 

이렇게하려면 GitHub 저장소에서 설정 탭으로 이동합니다.
 거기에서 분기 탭에서 규칙 추가를 클릭하십시오.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Manage-Pull-Requests-in-GitHub-Repository.png?resize=730%2C187&ssl=1)

분기 이름 패턴에`main`을 추가합니다.
 

병합하기 전에 통과하려면 상태 확인 필요 메뉴에서 ci / circleci : 빌드 옵션을 선택합니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/CeCircleci-Build-Option-Location.png?resize=730%2C371&ssl=1)

이제 저장소의 새 브랜치로 체크 아웃하고 풀 요청을 생성하고 테스트 할 수 있습니다.
 

예를 들어 index.css 파일에서 배경색과 같은 CSS 값을 변경하여 응용 프로그램의 배경색을 변경합니다.
 그런 다음 변경 사항을 커밋하고 PR을 만듭니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/CSS-Value-Change-PR-Creation.png?resize=730%2C315&ssl=1)

성공!
 녹색이 있고 합칠 수 있습니다!
 

브랜치를 main에 병합하면 빌드 및 heroku / deploy-via-git 작업이 실행되기 시작합니다.
 성공하면 앱이 Heroku에 배포됩니다.
 실패한 테스트에서도 똑같이 시도 할 수 있습니다.
 

CircleCI 구성이 포함 된 전체 프로젝트는 GitHub에서 견적 생성기 데모를 확인하십시오.
 

### 마지막 생각들
 verified_user

이 튜토리얼에서는 React로 CI / CD 파이프 라인을 설정할 수있었습니다.
 우리가 만든 구성 파일은 Heroku에서 호스팅하려는 모든 React 애플리케이션에 적용 할 수 있습니다.
 환경이 다른 애플리케이션의 경우 여러 작업을 만들고 특정 브랜치에 대해 필터링 할 수 있습니다 (위의 main에서했던 것처럼).
 

이와 같은 작업을 수행하는 구성은 다음과 같습니다.
 

```undefined
version: 2.1

orbs:
  heroku: circleci/heroku@0.0.10

jobs:
  build:
    docker:
      - image: circleci/node:10.16.3
    working_directory: ~/repo
    steps:
      - checkout
      # install dependencies
      - run:
          name: Install Dependencies
          command: npm install
      - run:
          name: Run tests
          command: npm run test

workflows:
  heroku_deploy:
    jobs:
      - build
      - heroku/deploy-via-git: # Use the pre-configured job, deploy-via-git
          app-name: $HEROKU_PRODUCTION_APP_NAME
          requires:
            - build
          filters:
            branches:
              only: main
      - heroku/deploy-via-git: # Use the pre-configured job, deploy-via-git
          app-name: $HEROKU_STAGING_APP_NAME
          requires:
            - build
          filters:
            branches:
              only: develop
```

`HEROKU_PRODUCTION_APP_NAME` 및`HEROKU_STAGING_APP_NAME`은 환경 변수이며 CircleCI에서 설정해야합니다.
 

## 결론
 

최신 앱 개발에서 지속적인 통합과 지속적인 배포의 중요성은 아무리 강조해도 지나치지 않습니다.
 여기에서 앱 요구 사항에 맞게 CircleCI 구성을 조정하거나 TravisCI, Azure DevOps 및 Jenkins와 같은 다른 CI / CD 도구를 사용해 볼 수 있습니다.
 

개발 운영 팀과의 최적의 협업을 위해 CI와 CD를 사용하여 통합 악몽을 피하는 것이 좋습니다.
 