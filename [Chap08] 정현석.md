# Chap08. AWS IAM 서비스
## 8.1 배경 소개

**사용자 인증**과 **권한 통제**는 온프레미스 환경과 더불어 클라우드 환경에서도 가장 중요한 보안 요소이다.  
이 때문에 AWS는 더욱 안전하게 사용할 수 있도록 AWS IAM 기능을 제공한다.

### AWS 리소스 생성하고 관리하기

**AWS 관리 콘솔(AWS management console)**

- AWS 리소스를 생성하고 관리하는 데 사용할 수 있는 **웹(web)** 기반 사용자 인터페이스를 제공
- 직관적이기 때문에 AWS 입문 단계에서 사용하기에 굉장히 편리

**AWS 명령줄 인터페이스(AWS Command Line Interface, AWS CLI)**

- AWS 서비스를 관리하는 통합 도구
- 운영 체제(윈도우, macOS, 리눅스)에 설치하면 **셸(shell)** 프로그램에서 AWS 서비스 사용 가능

**AWS 소프트웨어 개발 키트(Software Development Kit, SDK)**

- AWS 리소스를 프로그래밍적으로 사용하기 편리하도록 제공되는 라이브러리들
- 파이썬(Python), Go, 루비(Ruby), 자바(Java) 등 주요 프로그래밍 언어별로 다양한 라이브러리를 제공

> 💡 **소프트웨어 개발 키트(Software Development Kit, SDK)** - 특정 소프트웨어를 개발할 때 도움을 주는 개발 도구 집합

### AWS 리소스 사용법

AWS 리소스를 다루는 방법들은 AWS API에서 요청을 받아 온다.


### AWS API란

**API(Application Programming Interfaces)란**

- 두 애플리케이션이 상호 작용할 수 있게 도와주는 매개체
    - API를 이용하여 두 애플리케이션이 서로 통신하면서 정보를 주고받을 수 있는 것

**API를 사용할 때의 두 가지 규칙**

1. 외부에 공개된 API 서버가 아닐 때는 인증된 사용자만 접속할 수 있게 해야 한다.
- 인증된 사용자인지 확인하는 ‘인증’과 데이터를 내려받아도 된다고 허가하는 ‘인가’가 필요
2. 요청할 때 규칙을 정리한 문서인 ‘명세서’가 필요하다.
- 예시로 최대 글자 수 제한은 몇 글자까지인지, 전달하려는 데이터 유형은 무엇인지 등 정해진 규칙이 요청서에 담겨 있어야 함

![API 요청][image1]

[image1]: https://blog.kakaocdn.net/dn/kRto2/btrWI8oid8R/GOMG086uKTwjioznMDgKnK/img.png

- 사용자가 특정 데이터를 조회하면 공공 데이터 포털 API 서버에 API 요청이 전달
- API 서버는 사용자에 대한 인증과 인가를 확인
    - 인증된 사용자라고 판단하면 명세서 규칙을 확인하여 요청한 데이터의 결과 값을 사용자에게 보여줌

> 💡 허가 받은 사용자가 프로그래밍에서 API를 사용하려면 **인증**과 **인가**가 필요  
**인증(authentication)** - 사용자가 적합한 서명 값을 가졌는지 확인  
**인가(authorization)** - 인증이 확인된 사용자가 API 권한을 수행할 수 있는지 확인

AWS 클라우드에서 인프라, 보안, 데이터베이스, 분석, 배포 및 모니터링 등 모든 IT 리소스는 _**AWS API 호출**_ 로 제어 가능

**AWS API란**

- 사용자나 애플리케이션이 AWS 서비스를 사용하기 위해 도와주는 매개체

> 💡 **API 로깅** - AWS API의 활동 기록을 저장하는 것


> 💡 **AWS CloudTrail** - AWS 계정의 거버넌스, 규정 준수, 운영 감사, 위험 감사를 지원하는 서비스  
CloudTrail을 사용하면 AWS 인프라에서 계정 활동과 관련된 작업을 기록하고 지속적으로 모니터링하여 보관 가능

## 8.2 AWS IAM

앞에서 알아본 인증과 인가는 AWS IAM으로 동작

### AWS IAM이란

**AWS IAM(Identity & Access Management)**

- AWS 서비스와 리소스에 안전하게 접근할 수 있도록 관리하는 기능
- IAM을 이용하여 리소스를 사용하도록 ‘인증’과 ‘권한’을 통제
- AWS 사용자 및 그룹을 만들고 관리하거나 권한을 이용하여 AWS 리소스 접근을 허용하거나 거부

![인증, 인가 동작 순서][image2]

[image2]: https://blog.kakaocdn.net/dn/mexje/btrbosrKb46/i9yRK97OQqxFEYE9ySNlN0/img.png


### AWS IAM 구성 요소와 동작 방식

### 구성 요소

- AWS IAM은 사용자, 그룹, 역할, 정책으로 구성
- **허가 받은 사용자**는 AWS IAM 구성 요소 중 사용자와 그룹으로 확인 가능
- **허가 받은 사용자의 해당 서비스 사용 권한**은 AWS IAM 구성 요소 중 정책과 역할로 확인 가능

![AWS IAM 구성 요소][image3]

[image3]: https://miro.medium.com/v2/resize:fit:1083/0*jVhkcdC5VSYrBhwO.png

**AWS 계정 루트 사용자**

- 맨 처음 생성된 AWS 계정
- 해당 계정의 모든 권한을 소유

**IAM 사용자(user)**

- 별도의 AWS 계정이 아닌 계정 내 사용자
- 각 IAM 사용자는 자체 자격 증명을 보유
- IAM 사용자마다 특정 AWS 작업을 수행할 수 있게 권한 통제 가능

**IAM 그룹(group)**

- IAM 사용자 집합
- IAM 그룹에 권한을 지정해서 다수의 IAM 사용자의 권한을 쉽게 관리 가능

**IAM 정책(policy)**

- 자격 증명이나 리소스와 연결될 때 요청을 허용하거나 거부할 수 있는 권한을 정의하는 AWS 객체

**IAM 역할(role)**

- 특정 권한을 가진 계정에 생성할 수 있는 IAM 자격 증명
- 역할에는 그와 연관된 암호 또는 접근 키 같은 장기 자격 증명이 없다.
    - 대신 역할을 주면 역할 세션을 위한 임시 보안 자격 증명을 제공
- 역할을 이용하여 AWS 리소스에 접근할 수 없는 사용자, 애플리케이션, 서비스에 접근 권한 위임 가능

**보안 주체(principals)**

- AWS 계정 루트 사용자, IAM 사용자, IAM 역할을 이용하여 로그인하고 AWS에 요청하는 사람 또는 애플리케이션

### 인증, 인가 동작 방식

- IAM 사용자가 AWS 리소스를 사용할 때는 암호나 접근 키 같은 자격 증명을 사용하여 인증 필요
- IAM 사용자 계정에 따른 암호나 접근 키가 올바르다면 적합한 사용자로 간주되어 인증 동작이 마무리
- 인증이 처리되면 IAM 사용자는 적합한 권한이 있는지 확인하는 인가 동작을 진행

![AWS IAM 인증, 인가 동작 예시][image4]

[image4]: https://devio2023-media.developers.io/wp-content/uploads/2022/04/22-1-640x333.png

- IAM 사용자 이외에 IAM 그룹에 정책을 부여해서 그룹별로 좀 더 편리하게 관리 가능


### AWS IAM 사용자

**IAM 사용자는** 일반적으로 ‘**루트 사용자**’와 ‘**일반 IAM 사용자**’로 나눠진다.

**루트 사용자**

- AWS의 모든 리소스에 접근할 수 있는 전체 권한 보유
    - AWS 계정 생성 및 해지와 IAM 사용자 관리 등에 사용
- 루트 사용자로 AWS 리소스를 사용하는 것은 권장X
    - 루트 사용자 계정을 다수 사용자가 공용으로 사용하면 어떤 사용자가 어떤 행위를 했는지 구분 불가
    - 루트 사용자 계정이 해킹되면 공격자가 AWS 계정에 대한 모든 권한 보유

**IAM 사용자**

- IAM 사용자 계정이 해킹되면 루트 사용자의 계정으로 해킹된 IAM 사용자를 차단 가능
- AWS API 로깅으로 침해 행위 분석 가능

따라서 일반적으로 AWS 리소스를 사용할 IAM 사용자를 생성하고 권한을 부여한 후 사용하기를 권장

![IAM 사용자 vs 루트 사용자][image5]

[image5]: https://img1.daumcdn.net/thumb/R1280x0.fjpg/?fname=http://t1.daumcdn.net/brunch/service/user/d6Bs/image/6_F-sqgNMsXZs0T4-ymQ8iu9iz4



### AWS IAM 정책

사용자를 인가하고 나면, 사용자별로 권한에 대한 확인 절차를 진행한다.
사용자별 권한 검사는 AWS IAM 정책을 이용해서 진행한다.
사용자에 연결된 IAM 정책으로 사용자별 AWS 서비스와 리소스에 대한 인가(권한)을 얻을 수 있다.
사용자가 특정 AWS 서비스를 사용하려고 인가를 요청하면, IAM은 IAM 정책을 기반으로 AWS 요청을 검사, 평가한 후 최종적으로 허용할지 차단할지를 결정한다.

![AWS IAM Policy 설정 예시][image6]

[image6]: https://search.pstatic.net/sunny/?src=https%3A%2F%2Fi.stack.imgur.com%2FSOyxj.png&type=sc960_832

- Effect
    - 명시적 정책에 대한 허용 혹은 차단
- principal
    - 접근을 허용 혹은 차단하고자 하는 대상
- Action
    - 허용 혹은 차단하고자 하는 접근 타입
- Resource
    - 요청의 목적지가 되는 서비스
- Condition
    - 명시적 조건이 유효하다고 판단될 수 있는 조건


### AWS IAM 역할

**IAM 역할**

- 정의된 권한 범위 내 AWS API를 사용할 수 있는 임시 자격 증명을 의미

**Assume**

- 코드에 하드 코딩하지 않고 실행할 때 임시(+Token, 일정 시간이후 만료됨) 자격 증명을 사용

IAM 역할을 사용하면 사용자 권한을 공유하거나 매번 권한을 부여할 필요가 없다.

**IAM 역할을 활용한 예시**

- EC2 Instance Profile
- Federations(Cross-Account, SAML2.0, Web Identify Provider)
