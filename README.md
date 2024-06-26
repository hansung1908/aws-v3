# aws-v3

### Springboot 2.6.6, JDK 11
- devtools
- springweb
- lombok

### 목표
- 엘라스틱 빈스톡을 통해 배포

### 엘라스틱 빈스톡
- ec2, jdk, os 등 프로젝트를 실행하는데 필요한 환경이나 설정들이 이미 설치되어 있음
- 오토 스케일링, 각종 소프트웨어 구성, 로드밸런서, 모니터링, 업데이트 버전 관리
- 내부 구성은 크게 ec2와 로드밸런서로 나뉘며, 각 파트는 서로 다른 도메인 주소 보유
- ec2에선 nginx 프록시 서버와 jdk + 자바 샘플코드가 설치된 서버로 나뉘며, 각각 80, 5000 포트를 부여받음
- 클라이언트가 ec2에 접속하려면 먼저 로드밸런서로 요청을 보내야 하함
- 로드밸런서는 보안 규칙에 맞게 허용된 요청만 ec2의 프록시 서버로 보냄, 이때 80 포트를 사용
- 요청을 받은 프록시 서버가 내부적으로 샘플코드가 있는 서버에 5000번 포트로 요청
- 로드밸런서는 요청을 받아 같은 보안 그룹에 있는 ec2 서버들 중에서 알맞은 서버에 요청을 보낼 수도 있음

### 엘라스틱 빈스톡 생성
- 시작 전 iam을 통해 역할을 부여해야 함
- 권한 추가시 AWSElasticBeanstalkWebTier, AWSElasticBeanstalkWorkerTier, AWSElasticBeanstalkMulticontainerDocker 선택
- 이후 elastic beanstalk 어플리케이션 생성
- 이름을 aws-v3로 설정하면 환경 이름을 자동으로 Aws-v3-env로 설정
- 플랫폼은 java 17, 어플리케이션 코드는 샘플, 프리셋은 프리 티어 단일 인스턴스
- 새 서비스 역할 생성 및 사용을 선택해 새로운 서비스 역할 생성
- 키 페어는 기존에 사용하던 키 페어중 하나 선택
- 이 다음 선택 사항 과정은 검토 단계로 건너뛰기로 생략 후 제출

### 앨라스틱 빈 스톡 배포
- 먼저 배포를 위한 jar 파일 생성을 위해 터미널에 해당 명령어 입력 
```shell
./gradlew clean build
```
- 실행 후 build/libs 경로에 jar 파일 생성됨
- 엘라스틱 빈스톡에서 생성한 환경에 jar 파일 업로드 및 배포
- 도메인 주소 + /aws/v3를 입력하여 동작 확인

### 앨라스틱 빈 스톡 접속
- mobaxterm을 이용해 ssh 접속
- 앨라스틱 빈 스톡에서 username은 ec2-user로 설정
- 주요 프록시 설정들은 해당 경로 파일에 있음
```shell
cd /etc/nginx/conf.d/elasticbeanstalk

vi 00_application.conf
```
- 내부 내용에서 location /는 내부에서 ip주소/로 요청이 오면 아래 설정들을 참고한다는 트리거
- 이때 proxy_pass를 참고하여 해당 포트로 접속
