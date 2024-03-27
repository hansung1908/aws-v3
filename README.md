# aws-v3

### 엘라스틱 빈스톡
- ec2, jdk, os 등 프로젝트를 실행하는데 필요한 환경이나 설정들이 이미 설치되어 있음
- 오토 스케일링, 각종 소프트웨어 구성, 로드밸런서, 모니터링, 업데이트 버전 관리

### 엘라스틱 빈스톡 생성
- 시작 전 iam을 통해 역할을 부여해야 함
- 권한 추가시 AWSElasticBeanstalkWebTier, AWSElasticBeanstalkWorkerTier, AWSElasticBeanstalkMulticontainerDocker 선택
- 이후 elastic beanstalk 어플리케이션 생성
- 이름을 aws-v3로 설정하면 환경 이름을 자동으로 Aws-v3-env로 설정
- 플랫폼은 java 17, 어플리케이션 코드는 샘플, 프리셋은 프리 티어 단일 인스턴스
- 새 서비스 역할 생성 및 사용을 선택해 새로운 서비스 역할 생성
- 키 페어는 기존에 사용하던 키 페어중 하나 선택
- 이 다음 선택 사항 과정은 검토 단계로 건너뛰기로 생략 후 제출