---
layout: post
title: "젠킨스를 사용하여 도커 이미지를 Amazon ECR로 푸시하는 방법"
author: "Logger"
thumbnail: "undefined"
tags: 
---


![Person taking in mountain view](https://miro.medium.com/max/8192/0*MZDWUHUr8cKXatIW)

1년 전, 내 웹사이트는 헤로쿠에서 실행되고 있었다. Herku는 동일한 인스턴스에서 여러 애플리케이션을 허용하지 않기 때문에 매달 7달러를 지불해야 했습니다(백엔드 및 프런트엔드 애플리케이션). 각각의 추가 신청에 대해, 매달 7달러의 비용이 듭니다.

사내에서 하는 것이 훨씬 저렴할 것이라고 확신하고 낡은 노트북을 가지고 가서 모든 것을 설치했어요. Linux와 Docker-composes를 구성한 후, 다른 무엇보다도 제 웹 사이트가 다시 실행되었습니다. 이것은 내가 매달 14달러를 저축할 수 있게 해주었다.

문제가 생기기 전까지는요 실제로 일어날 것으로 기대하지 않는 문제들. 발생할 수 있는 문제:

따라서 필요에 따라 클라우드 환경으로 돌아가서 안정성을 개선하는 것이 유일한 선택이었습니다. 하지만 이번에는 Herku와는 다른 클라우드 프로바이더가 될 것입니다. 내 Docker 이미지를 클라우드로 전환하는 것이 이 솔루션의 첫 번째 부분입니다. 이 글은 다음과 같은 내용을 담고 있습니다. Docker 이미지를 Amazon AWS ECR과 같은 클라우드 저장소로 푸시하는 중입니다.

내가 약속할게: 이 기사에는 단지 몇 분밖에 시간이 걸리지 않을 것이다. ECR에 배포하기 위한 세 가지 단계를 처리합니다.

# AWS ECR 저장소 생성

첫 번째 단계는 쉽습니다. AWS 개발자 콘솔에 로그인하고 AWS 서비스 Elastic Container Registry(AWS ECR)로 이동하기만 하면 됩니다.

이 서비스에서는 제공된 스크린샷과 같이 도커 컨테이너 저장소를 생성합니다. 기본 설정을 모두 켜둘 필요는 없습니다. 그들에게는 숨겨진 비용이 없다.

입력해야 하는 필드는 "도커 리포지토리 이름"입니다. "웹 사이트 프런트 엔드"와 같은 이름 또는 애플리케이션에 더 적합한 이름을 선택하십시오. "리포지토리 만들기"를 클릭하면 Docker 이미지를 밀어넣을 리포지토리가 나타납니다.

아주 간단하죠?

![Creating a repo](https://miro.medium.com/max/1282/1*N3m9mApkwF3-E4NeMUlIBw.png)

# Jenkins에서 AWS 자격 증명 구성

Amazon ECR 플러그인을 사용하여 Jenkins에 Amazon AWS 자격 증명을 저장할 수 있으며 CloudBees Docker Build and Publish 플러그인을 사용하여 이를 사용할 수 있습니다. 이 섹션의 나머지 부분을 진행하기 전에 이 두 개의 플러그인이 설치되어 있는지 확인하십시오.

대시보드에서 AWS 자격 증명을 추가합니다.

참고: 개인 자격 증명을 전체 조직과 공유하지 마십시오. 문제가 발생할 수 있습니다. 또한 조직 내에서 이 작업을 수행하는 경우 ECR에만 액세스할 수 있는 별도의 IAM 사용자를 생성하는 것이 좋습니다. 누군가가 당신의 AWS 계정에 로그인하여 잠재적으로 많은 돈을 쓴다고 상상해 보십시오. 그들은 결국 당신에게 청구서를 지불하러 올 것입니다.

다음 네 가지 필드를 입력해야 합니다.

나중에 "확인"을 누르면 Jenkins ID를 참조하여 파이프라인의 자격 증명을 사용할 수 있습니다!

![Credentials settings](https://miro.medium.com/max/1738/1*toK_T9R97asH-JzaWn4vGQ.png)

# 젠킨스와 함께 푸시할 빌드 단계 만들기

이제 모든 것이 구성되었으므로 개발 파이프라인을 약간만 조정할 수 있습니다. Angular에서 이러한 파이프라인을 설정하는 방법의 예는 이 문서에서 찾을 수 있습니다.

"Deploy"라는 이름을 지정할 마지막 단계에서는 Docker 이미지를 Amazon ECR에 푸시하는 스크립트를 추가합니다.

아래 이미지에는 이 작업을 수행할 수 있는 방법이 나와 있습니다. 모든 항목을 올바른 구성으로 바꾸십시오.

바로 그거야! 파이프라인이 "Deploy" 단계에 도달할 때마다 이미지가 ECR로 푸시됩니다!

![Deploy stage in Jenkins](https://miro.medium.com/max/3500/1*FxWEtgGJbZCcvh4qK3nwVA.png)

# 마무리 중

이 기사에서는 Jenkins를 사용하여 Docker 이미지를 Amazon ECR로 바로 푸시하는 방법을 살펴봤습니다. Amazon CodeBuild 또는 CodeDeploy와 같은 서비스에 대해 비용을 지불하지 않으려면, 그럴 필요가 없습니다.

이 기사는 당신이 이미 가지고 있는 것을 재사용하는 것에 관한 것입니다. 클라우드 제공자가 제한하지 않도록 하십시오. 파이프라인이 100% 온라인 상태여야 하는 경우에도 나중에 AWS 또는 다른 클라우드 공급자가 제공하는 서비스로 전환할 수 있습니다!

결국, 이 솔루션을 사용하면 추가 비용 없이 Docker 이미지를 클라우드에 제공할 수 있으며 Amazon AWS의 무료 계층 내에 완전히 남아 있게 됩니다. 곧 클라우드에서 이러한 도커 이미지로 무엇을 할 수 있는지에 대한 기사를 더 많이 쓰겠습니다!

어떻게 하면 이러한 도커 이미지를 사용하여 Amazon EC2 인스턴스에서 웹 사이트를 설정하고 매달 총 클라우드 비용을 단 몇 달러로 낮출 수 있는지 보여드리겠습니다.

채널 고정!