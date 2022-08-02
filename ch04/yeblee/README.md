# [Travis CI] 프로젝트 연동하기

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbMIduW%2FbtrfWMtiPEC%2FENLpZFdHhIVcpV31IWNBcK%2Fimg.jpg">
</p>

~~~
Travis CI는 github에서 제공하는 무료 CI 서비스이다.
~~~

<br>

## CI와 CD

CI/CD는 애플리케이션 개발 단계를 자동화하여 애플리케이션을 보다 짧은 주기로 고객에게 제공하는 방법입니다.
새로운 코드 통합으로 인해 개발 및 운영팀에 발생하는 문제를 해결하기 위한 솔루션으로 애플리케이션의 통합 및 테스트 단계에서부터 제공 및 배포에 이르는 애플리케이션의 라이프 사이클 전체에 걸쳐 지속적인 자동화와 모니터링을 제공합니다.
일반적으로 CI/CD 파이프라인이라 부르며 개발 및 운영팀의 애자일 방식 협력을 통해 지원됩니다.

#### CI 란 ?

> 지속적 통합 (CI : Continuous Integeration)

지속적 통합은 지속적으로 품질 관리를 적용하는 프로세스를 실행하는 것 입니다. 즉, 개발 주기가 끝날 때 큰 변경 사항을 병합하는 것이 아니라 작은 코드 변경 사항을 자주 병합하는 방식입니다. 
소프트웨어의 질적 향상과 소프트웨어를 배포하는데 걸리는 시간을 줄이는데 초점이 맞추어져 있습니다. 

#### CD 란 ?

> 지속적 배포 (CD : Continuous Delivery)

팀이 짧은 주기로 소프트웨어를 개발하는 소프트웨어 공학적 접근의 하나로, 소프트웨어가 언제든지 신뢰 가능한 수준으로 출시될 수 있도록 보증하기 위한 것입니다.

#### 종류

- Jenkins
- TravisCI
- CircleCI
- Github Actions
- 등

<br>

## Travis CI 란 ?

Travis CI는 개발자가 코드를 빠르고 쉽게 빌드, 테스트 및 배포할 수 있는 CI/CD(지속적 통합/지속적 전달) 플랫폼입니다.
Jenkins의 경우 설치하여 사용해야 하지만 Travis CI는 오픈소스 웹 서비스이기 때문에 Github와의 편한 연동 및 public repository에 대해서는 무료로 사용할 수 있다는 장점이 있습니다. 하지만, private repository에 대해서는 일정량의 비용을 지불해야 합니다. 
설정이 많이 복잡하지 않고 Travis 서버를 이용하여 동작시키는 것이기 때문에 편리합니다. Travice CI의 웹 사이트에서 Github 계정으로 연동해 가입하면, 자신의 Github에 있는 저장소들 중 관리하고 싶은 것을 선택하여 자동화 테스트를 수행할 수 있습니다.

<br>

## Travis CI 웹 서비스 설정

#### CI 빌드 및 자동화

빌드를 실행하면 Travis CI는 Github 레포지토리를 새로운 가상 환경으로 복제하고 일련의 작업을 수행하여 코드를 빌드하고 테스트합니다. 
작업 중 하나 이상이 실패하면 빌드가 손상된 것으로 간주합니다. 어떤 작업도 실패하지 않으면 빌드가 통과된 것으로 간주하고 웹서버 또는 애플리케이션 호스트에 코드를 배포할 수 있습니다.

- `.travis.yml` : Travis CI에 등록한 저장소 루트에 해당 설정 파일에 정의되어 있는 방식대로 빌드 프로세스를 조정하는 파일입니다.
- `build` : 순서대로 실행되는 작업 그룹을 나타냅니다. `.travis.yml`을 작성했다면 git push 명령어를 실행하여 자동으로 빌드 테스트를 수행합니다.
- `stage` : 여러 단계로 구성된 순차 빌드 프로세스의 일부로 병렬로 구성되는 작업 그룹입니다.
- `job` : 저장소를 가상 환경으로 복제한 다음 코드 컴파일, 테스트 실행 등과 같은 일련의 단계를 수행하는 자동화된 프로세스입니다. script단계의 반환 코드가 0이 아닌 경우 작업이 실패합니다.
- `phase` : job의 순차적 단계입니다. 

<br>

## 프로젝트 설정

- [홈페이지 접속](https://travis-ci.com/)후 깃허브 계정으로 로그인합니다.
- `Settings`로 들어가 CI를 사용할 public repository를 선택합니다.
<img width="519" alt="스크린샷 2022-08-02 오전 10 56 42" src="https://user-images.githubusercontent.com/68188768/182275272-02a11c1b-3078-4c81-981d-2768b484794c.png">
<img width="520" alt="스크린샷 2022-08-02 오전 10 57 32" src="https://user-images.githubusercontent.com/68188768/182275367-a4b90e4d-139f-40f8-b4dc-28bd3027dedd.png">

- `.travis.yml`을 활용하여 작성합니다.

```yml
# 언어와 jdk의 버전을 지정한다.
language: java
jdk:
  - openjdk11

# 어느 브랜치가 push 될 때 수행할지 지정한다. 
# 오직 main 브랜치가 push될 때 수행하도록 지정하였다.
branches:
  only:
    - main

# 빌드 전에 gradlew의 권한을 추가한다.
before_install:
  - chmod +x gradlew

# Travis CI 서버의 Home
# gradle을 통하여 의존성을 받게 되면 이를 캐시하여 배포할 때 마다 다시 받지 않도록 설정한다.
cache:
  directories:
    - '$HOME/.m2/repository'
    - '$HOME/.gradle'

# main 브랜치에 push 되면 수행되는 명령어이다. 
# 프로젝트 내에 권한이 추가된 gradlew를 활용하여 clean, build를 진행한다.
script: "./gradlew clean build"

# CI 실행 완료 시 작성한 이메일로 알람
notifications:
  email:
    recipients:
      - email
```

- 작성한 후 main 브랜치에 commit과 push를 했을 때 Travis CI 저장소 페이지에 해당 `.travis.yml`내용을 기반으로 빌드가 진행됩니다.
- 작업을 성공 했을 경우 `passed`를 실패했을 경우 `errored`를 확인할 수 있습니다.

<br>

## 사용하는 이유

실무에서는 일일히 빌드와 테스트, 배포과정을 직접한다는 것은 리소스낭비이며 심한 경우에는 업무의 대부분을 빌드와 테스트, 배포에 투자해야 할 수도 있습니다.

<br>

### 참고

- [Travis CI](https://www.travis-ci.com/)
- [.travis.yml 방법](https://docs.travis-ci.com/user/job-lifecycle/)
