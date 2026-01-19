# Amazon SageMaker MLOps와 MLFlow를 활용한 머신러닝 운영화

이 저장소는 Amazon SageMaker를 사용하여 ML 프로젝트를 구축, 훈련 및 운영하는 방법을 보여주는 일련의 노트북을 포함하고 있습니다.

이 워크샵은 ML 개발 환경으로 SageMaker Studio를 사용합니다. 또한 SageMaker Data Wrangler와 훈련 작업, 그리고 SageMaker Pipelines, SageMaker Feature Store, SageMaker Model Registry, SageMaker 관리형 MLflow 실험과 같은 SageMaker MLOps 기능을 활용합니다.

SageMaker Data Wrangler를 사용한 피처 엔지니어링과 처리 및 모델 훈련을 위한 노트북으로 실습을 시작합니다. 이후 각 노트북은 이전 노트북을 기반으로 하나 이상의 SageMaker MLOps 기능을 소개합니다:

![](img/SageMaker-MLOps-Pipeline.jpg)

각 노트북은 유용한 실습 리소스 링크를 제공하고 추가 개발을 위한 실제 아이디어를 제안합니다.

단계를 따라가며 권장 MLOps 사례에 따라 개발에서 프로덕션 준비 단계까지 ML 프로젝트를 개발합니다:

![](img/sagemaker-mlops-diagram.jpg)

## 추가 주제
A/B 테스팅, 커스텀 처리, 훈련 및 추론 컨테이너, 디버깅 및 프로파일링, 보안, 멀티 모델 및 멀티 컨테이너 엔드포인트, 직렬 추론 파이프라인과 같은 다른 SageMaker 기능 및 ML 주제에 대한 추가 실습 예제도 있습니다. `additional-topics` 폴더의 노트북을 탐색하여 이러한 기능을 테스트해 보세요.

## 시작하기
전체 지침과 계정의 상세 설정은 공개 AWS 워크샵 Amazon SageMaker MLOps: 아이디어에서 프로덕션까지 6단계를 참조하세요.

### 사전 요구 사항
AWS 계정이 필요합니다. 아직 계정이 없다면 AWS 환경 설정 시작 가이드를 참조하여 빠른 개요를 확인하세요.

### AWS 강사 주도 워크샵
AWS Immersion Day 또는 유사한 강사 주도 이벤트에 참여하고 제공된 AWS 계정을 사용하려면 임시 AWS 계정을 요청하고 SageMaker Studio를 시작하는 방법에 대한 지침을 따르세요.

❗ 제공된 AWS 계정을 사용하는 경우 다음 단계인 Amazon SageMaker 도메인 설정과 CloudFormation 템플릿 배포를 건너뛰세요.

### Amazon SageMaker 도메인 설정
노트북을 실행하려면 SageMaker 도메인이 필요한 SageMaker Studio를 사용해야 합니다.

#### 기존 SageMaker 도메인
이미 SageMaker 도메인이 있고 워크샵을 실행하는 데 사용하려면 SageMaker Studio 설정 가이드를 따라 Studio 사용자 프로필에서 사용하는 IAM 실행 역할에 필요한 AWS IAM 정책을 연결하세요. 이 워크샵에서는 워크샵을 실행하는 데 사용하는 사용자 프로필의 IAM 실행 역할에 다음 관리형 IAM 정책을 연결해야 합니다:
- `AmazonSageMakerFullAccess`
- `AWSCloudFormationFullAccess`
- `AWSCodePipeline_FullAccess`
- `AmazonSageMakerPipelinesIntegrations`

이 워크샵에 사용할 전용 IAM 실행 역할이 있는 새 사용자 프로필을 생성할 수도 있습니다.

#### 새 SageMaker 도메인 프로비저닝
SageMaker 도메인이 없거나 워크샵 전용 도메인을 사용하려면 새 도메인을 생성해야 합니다.

❗ 계정에 둘 이상의 도메인이 있는 경우 계정의 리전당 활성 도메인 제한을 고려하세요.

새 도메인을 생성하려면 개발자 가이드의 온보딩 지침을 따르거나 SageMaker 도메인, 사용자 프로필을 생성하고 제공된 노트북 실행에 필요한 IAM 역할을 추가하는 제공된 AWS CloudFormation 템플릿을 사용할 수 있습니다.

❗ AWS 콘솔을 통해 새 도메인을 생성하는 경우 사용자 프로필의 IAM 실행 역할에 다음 정책을 연결해야 합니다:
- `AmazonSageMakerFullAccess`
- `AWSCloudFormationFullAccess`
- `AWSCodePipeline_FullAccess`
- `AmazonSageMakerPipelinesIntegrations`

❗ 도메인 생성을 위해 제공된 CloudFormation 템플릿을 사용하는 경우 템플릿은 다음 정책이 연결된 IAM 실행 역할을 생성합니다:
- `AmazonSageMakerFullAccess`
- `AmazonS3FullAccess`
- `AWSCloudFormationFullAccess`
- `AWSCodePipeline_FullAccess`
- `AmazonSageMakerPipelinesIntegrations`

`sagemaker-domain.yaml` CloudFormation 템플릿을 다운로드하세요.

이 템플릿은 새 SageMaker 도메인과 `studio-user-<UUID>`라는 사용자 프로필을 생성합니다. 또한 도메인에 필요한 IAM 실행 역할을 생성합니다.

❗ 이 스택은 계정에 이미 퍼블릭 VPC가 설정되어 있다고 가정합니다. 퍼블릭 VPC가 없는 경우 단일 퍼블릭 서브넷이 있는 VPC를 참조하여 퍼블릭 VPC를 생성하는 방법을 알아보세요.

❗ 템플릿은 `us-east-1`, `us-west-2`, `eu-central-1` 리전만 지원합니다. 배포를 위해 해당 리전 중 하나를 선택하세요.

AWS CloudFormation 콘솔을 엽니다. 링크는 AWS 계정의 AWS CloudFormation 콘솔을 엽니다. 선택한 리전을 확인하고 필요한 경우 변경하세요.
- 템플릿 파일 업로드를 선택하고 다운로드한 CloudFormation 템플릿을 업로드한 후 다음을 클릭합니다
- 스택 이름을 입력합니다(예: `mlops-with-sagemaker-mlflow`). 다음을 클릭합니다
- 이 창에서 모든 기본값을 그대로 두고 다음을 클릭합니다
- AWS CloudFormation이 IAM 리소스를 생성할 수 있음을 승인합니다를 선택하고 제출을 클릭합니다

![](img/cfn-ack.png)

CloudFormation 창에서 스택을 선택합니다. 스택이 생성되는 데 약 15분이 걸립니다. 스택이 생성되면 스택 상태가 `CREATE_IN_PROGRESS`에서 `CREATE_COMPLETE`로 변경됩니다.

![](img/cfn-stack.png)

### SageMaker Studio 시작
AWS 계정에 로그인한 후 Amazon SageMaker Studio 시작 지침을 따라 Studio를 엽니다.

AWS 주도 워크샵 이벤트에 참여하는 경우 다음 지침을 따르세요:

1. 먼저 Amazon SageMaker 콘솔로 이동합니다. 상단의 검색 상자에 `SageMaker`를 입력하면 됩니다.

   ![](img/aws-console-sagemaker.png)

2. 왼쪽의 `Applications and IDEs` 섹션에서 Studio를 선택합니다
3. `Get started` 상자에서 studio-user-xxxxxxxx가 선택되어 있는지 확인하고 `Open studio`를 선택합니다. 이제 SageMaker Studio UI가 새 브라우저 탭에서 열리고 해당 창으로 리디렉션됩니다.

   ![](img/launch-studio.png)

4. 선택적으로 `Take quick tour button`을 선택하여 SageMaker Studio 인터페이스의 빠른 둘러보기를 하거나 `Skip Tour for now`를 선택합니다
5. 선호도에 따라 쿠키 기본 설정을 수락하거나 거부합니다

### JupyterLab 스페이스 열기
이 워크샵에서는 JupyterLab 스페이스를 IDE로 사용합니다.

1. JupyterLab 스페이스를 시작하려면 왼쪽 상단의 `JupyterLab` 앱을 선택합니다

   ![JupyterLab selector](img/jupyterlab-app.png)
   
2. SageMaker Studio의 각 애플리케이션은 자체 스페이스를 갖습니다. 스페이스는 각 애플리케이션의 스토리지 및 리소스 요구 사항을 관리하는 데 사용됩니다. AWS 주도 워크샵에 참여하거나 제공된 CloudFormation 템플릿을 사용한 경우 필요한 스페이스가 이미 생성되어 있습니다. 그렇지 않으면 개발자 가이드에 설명된 대로 새 JupyterLab 스페이스를 생성하거나 기존 스페이스를 재사용해야 합니다

3. 오른쪽의 실행 버튼을 선택하여 스페이스를 실행합니다. 이 프로세스는 몇 초가 걸릴 수 있습니다.

   ![JupyterLab selector](img/space-run.png)

4. 스페이스가 실행되면 `Open`을 선택하여 JupyterLab 애플리케이션으로 이동합니다.

### 워크샵 시작
AWS 주도 워크샵에 참여하거나 제공된 CloudFormation 템플릿을 사용한 경우 워크샵 콘텐츠가 스페이스 EBS 볼륨에 자동으로 복제되므로 별도의 작업이 필요하지 않습니다. 자체 도메인과 사용자 프로필을 사용하거나 AWS 콘솔 UI를 통해 도메인을 생성한 경우 다음 섹션 JupyterLab 스페이스에 노트북 다운로드의 지침을 따라 콘텐츠를 복제하세요.

공개 GitHub 저장소 MLOps with SageMaker and MLFlow에는 모든 소스 코드가 포함되어 있습니다.

#### JupyterLab 스페이스에 노트북 다운로드
자체 도메인과 사용자 프로필을 사용하는 경우에만 노트북을 스페이스에 복제하면 됩니다. 이렇게 하려면 JupyterLab Launcher 창에서 `Terminal`을 선택하거나 **File** > **New** > **Terminal**을 선택하여 터미널을 열고 `git clone`을 실행합니다:

```sh
git clone https://github.com/aws-samples/mlops-sagemaker-mlflow
```

이렇게 하면 저장소가 로컬 JupyterLab 파일 시스템에 복제됩니다.

#### 설정 노트북 열기 및 실행
마지막 준비 단계로 `00-start-here.ipynb` 노트북을 실행해야 합니다. 이렇게 하려면

1. 파일 브라우저에서 `mlops-sagemaker-mlflow` 폴더를 더블 클릭하여 엽니다
2. `00-start-here.ipynb` 노트북을 열고 노트북의 지침을 따릅니다

![](img/mlops-jupyterlab-landing.jpg)

참고: 각 셀을 읽은 후 `Shift + Enter` 명령을 사용하여 실행하는 것을 권장합니다.

## 이 워크샵 사용 방법
이 워크샵은 두 가지 방법으로 진행할 수 있습니다:
- 제공된 노트북을 살펴보고 코드 셀을 순차적으로 실행하며 지침과 실행 흐름을 따릅니다
- 실습 과제와 연습을 통해 직접 코드를 작성합니다

다음 다이어그램은 워크샵의 가능한 흐름을 보여줍니다:

![](design/workshop-flow.drawio.svg)

### 실행 모드
Python 프로그래밍에 익숙하지 않고 Jupyter 노트북을 처음 사용하는 경우 이 모드를 사용하세요. 각 노트북 `00-...`, `01-...`, ..., `06-...`을 따라가며 `Shift` + `Enter`로 모든 코드 셀을 실행합니다. 제공된 지침은 코드가 무엇을 하고 왜 하는지 설명합니다. 모든 노트북의 모든 코드 셀을 실행하는 데 약 2시간 30분이 필요합니다.
모든 노트북과 모든 코드 셀은 멱등성을 가집니다. 모든 코드 셀을 위에서 아래로 순차적으로 실행해야 합니다.

### 과제 모드
Jupyter 노트북 작업 경험이 있고 SageMaker 기능과 SageMaker Python SDK에 대한 더 깊은 실습 이해를 위해 직접 코드를 작성하려는 경우 이 모드를 사용하세요.
워크샵 루트 폴더의 각 기본 지침 노트북 `00-...`, `01-...`, ..., `06-...`에는 `assignments` 폴더에 연습이 포함된 해당 "과제" 노트북이 있습니다. 먼저 루트 폴더 노트북의 지침을 살펴본 다음 해당 과제 노트북의 연습을 완료하세요. 노트북은 다음과 같이 매핑됩니다:
- `00-start-here` > `./assignments/00-assignment-setup`
- `01-idea-development` > `./assignments/01-assignment-local-development`
- `02-sagemaker-containers` > `./assignments/02-assignment-sagemaker-containers`
- `03-sagemaker-pipeline` > `./assignments/03-assignment-sagemaker-pipeline`
- `04-sagemaker-projects` > `./assignments/04-assignment-sagemaker-project`
- `05-deploy` > `./assignments/05-assignment-deploy`
- `06-monitoring` > `./assignments/06-assignment-monitoring`

## 정리
❗ AWS 강사 주도 워크샵을 실행하는 경우 정리를 수행할 필요가 없습니다.

요금이 부과되지 않도록 AWS 계정에서 프로젝트에서 프로비저닝하고 생성한 모든 리소스를 제거해야 합니다.

먼저 제공된 정리 노트북의 모든 단계를 실행합니다.
둘째, AWS 콘솔을 사용하여 이 워크샵용 도메인을 프로비저닝했고 도메인이 필요하지 않은 경우 이 지침을 따라 도메인을 삭제할 수 있습니다.

CloudFormation 템플릿을 사용하여 도메인을 프로비저닝한 경우 AWS 콘솔에서 CloudFormation 스택을 삭제할 수 있습니다.

도메인용으로 새 VPC를 프로비저닝한 경우 VPC 콘솔로 이동하여 프로비저닝된 VPC를 삭제합니다.

## 데이터셋
이 예제는 AWS Labs의 이 저장소를 사용하여 생성된 합성 게임 행동 데이터셋을 사용합니다.

## 리소스
다음 목록은 Amazon SageMaker에서 ML 개발을 시작하는 데 도움이 되는 유용한 실습 리소스를 제공합니다.

- Get started with Amazon SageMaker
- Deep Learning MLOps workshop with Amazon SageMaker
- Amazon SageMaker 101 workshop
- Amazon SageMaker 101 workshop code repository
- Amazon SageMaker Immersion Day
- Amazon SageMaker End to End Workshop
- Amazon SageMaker workshop with BYOM and BYOC examples
- End to end Machine Learning with Amazon SageMaker
- SageMaker MLOps Workshop
- Amazon SageMaker MLOps Workshop
- A curated list of awesome references for Amazon SageMaker
- AWS Multi-Account Data & ML Governance Workshop
- Amazon SageMaker MLOps - from idea to production

## 보안

자세한 내용은 CONTRIBUTING을 참조하세요.

## 라이선스

이 라이브러리는 MIT-0 라이선스에 따라 라이선스가 부여됩니다. LICENSE 파일을 참조하세요.
