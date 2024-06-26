# Chap02. AWS 컴퓨팅 서비스
## 2. 1 AWS 컴퓨팅 서비스

### 컴퓨팅 정의
- **컴퓨트(compute)**
    - 사전적 정의는 ‘계산하다, 답을 구하다, 추정하다’ 이다.
- **컴퓨팅(computing)**
    - 어떤 것에 대해 계산하여 답을 구하고 추정하는 행위
- **서버(server)**
    - 컴퓨팅을 전문적으로 수행하기 위해 인간이 아닌 컴퓨팅을 목적으로 하는 특화된 장비

### AWS 컴퓨팅 서비스

**AWS 컴퓨팅 서비스**
퍼블릭 클라우드에서 컴퓨팅 자원을 활용하여 다양한 워크로드를 수행할 수 있는 서비스
- **EC2**(Elastic Compute Cloud)
    - 클라우드 환경에서 서버 자원을 인스턴스라는 가상 머신 형태로 제공하는 가장 기본적인 AWS 컴퓨팅 서비스
- **ECS**(Elastic Container Service)
    - EC2 기반 관리형 클러스터에서 실행되는 컨테이너 형태의 자원에 대해 배포, 스케줄링, 스케일링 등을 관리하는 서비스
- **Lambda**
    - 서버리스(serverless) 컴퓨팅 서비스로, 서버리스라는 말 그대로 별도의 서버 설정이 없는 환경을 제공하여 코드만 실행해 주는 서비스
- **Lightsail**
    - 독립적인 환경을 제공하며, 최소한의 설정만으로도 손쉽게 사용 가능한 컴퓨팅 서비스

## 2. 2 Amazon EC2 소개

- **Amazon EC2(Amazon Elastic Compute Cloud)**
    - AWS의 퍼블릭 클라우드 환경에서 확장 가능한 컴퓨팅 자원을 제공하여 가상의 서버를 운영할 수 있는 서비스

Amazon EC2는 인스턴스라는 가상 컴퓨팅 환경을 기반으로 하며, **AMI**(Amazon Machine Image)를 이용하여 인스턴스에 필요한 소프트웨어 정보를 정의

Amazon EC2 인스턴스는 사용자가 요구하는 CPU, 메모리, 디스크, 운영 체제, 소프트웨어 등을 제공하여 워크로드에 맞는 최적화된 가상의 서버를 생성하고 관리할 수 있다.

> 💡 **AMI(Amazon Machine Image)?**  
인스턴스를 시작할 때 필요한 정보를 제공하는 것으로 운영 체제와 소프트웨어를 적절히 구성한 상태로 제공되는 템플릿

### Amazon EC2 인스턴스

- 가상의 컴퓨팅 환경으로 CPU, 메모리, 스토리지, 네트워킹 용량을 결정하는 다양한 인스턴스 유형을 제공
- 클라우드 환경에서 컴퓨팅 자원을 필요한 만큼 사용하고, 쓰임을 다하면 자원을 반납하는 형태로 임의로 구성된 인스턴스
### 인스턴스 상태

- **최종적으로 도달하는 상태**
    - 실행 중(running)
    - 중지됨(stopped)
    - 종료됨(terminated)
- **진행 과정에 따른 상태**
    - 대기 중(pending)
    - 중지 중(stopping)
    - 재부팅(rebooting)
    - 종료 중(shutting-down)

✅ **‘중지됨’ 상태와 ‘종료됨’ 상태의 차이**
- **중지됨**  
‘인스턴스를 유지한채 일시적으로 중지한’ 상태이기 때문에 얼마든지 다시 구동할 수 있다.

- **종료됨**  
‘인스턴스를 영구 삭제’하기 때문에 다시 사용하기 어렵다.

### Amazon EC2 스토리지

**스토리지(storage)**
- 데이터를 저장하는 공간

**Amazon EC2 인스턴스용 스토리지 유형**
- 인스턴스 스토어(instance store)
- 블록 스토리지인 Amazon EBS(Elastic Block Store)

**인스턴스 스토어**
- 인스턴스에 바로 붙어 있는 저장소로, Amazon EC2 인스턴스를 생성하면 기본적으로 존재하는 스토리지

> 장점: 직접 붙어 있는 구조 덕분에 매우 빠른 I/O(Input/Output)을 보장
단점: Amazon EC2 인스턴스를 중지하거나 종료하면 저장된 데이터가 모두 손실

**Amazon EBS**
- 외장 하드디스크와 비슷한 개념으로 인스턴스에 연결 및 제거를 하는 형태로 구성되는 블록 스토리지

> 장점: Amazon EC2 인스턴스가 중지되거나 종료되어도 Amazon EBS에 보존하는 데이터는 그대로 유지, Amazon EBS 연결을 해제한 후 다른 인스턴스에 연결 가능

### Amazon EC2 네트워킹
Amazon EC2 네트워킹 요소

- **Amazon VPC(Virtual Private Cloud)**

    - AWS 퍼블릭 클라우드 안에서 논리적으로 격리된 가상의 클라우드 네트워크

- **네트워크 인터페이스**
    - 인터페이스는 ‘서로 연결한다’는 의미로, 네트워킹을 수행하려면 네트워크 인터페이스가 필요
    - AWS에서는 ENI(Elastic Network Interface)라는 논리적 네트워크 인터페이스가 VPC 내 생성되며, ENI를 EC2 인스턴스에 연결해서 네트워킹을 수행
- **IP 주소**
    - 네트워크 인터페이스에는 IP(Internet Protocol) 주소가 있으며, IP 주소로 대상을 구분하고 네트워킹을 수행

> 프라이빗 IP: 내부 구간의 통신을 위한 사설 IP  
퍼블릭 IP: 외부 구간의 통신을 위한 공인 IP


### Amazon EC2 보안

Amazon EC2 보안 기능

- **보안 그룹**
    - Amazon EC2 인스턴스의 송수신 트래픽을 제어하는 가상의 방화벽 역할
    - 수신 트래픽에 대한 인바운드(inbound) 규칙과 송신 트래픽에 대한 아웃바운드(outbound) 규칙이며, 대상 트래픽에 대한 허용과 거부를 결정
- **키 페어(key pair)**
    - Amazon EC2 인스턴스에 연결할 때 자격을 증명하는 보안 키
    - **퍼블릭 키**와 **프라이빗 키**로 구성
        
> 퍼블릭 키는 Amazon EC2 인스턴스에 저장  
프라이빗 키는 사용자 컴퓨터에 별도로 저장
        

### Amazon EC2 모니터링

서비스 안전성과 가용성, 성능 유지에 필요한 중요한 영역

실제 모니터링 및 정보를 수행할 때는 다음과 같은 모니터링 계획을 정의해서 생성

- 모니터링 목표
- 모니터링 대상 자원
- 모니터링 빈도
- 모니터링 수행 도구
- 모니터링 작업을 수행할 사람
- 문제가 발생할 때 경보를 알려야 할 대상

Amazon EC2 모니터링은 다양한 지표(metric)에 대해 **수동 모니터링**과 **자동 모니터링**으로 분류

- **수동 모니터링 도구**
    - 말 그대로 관리자가 직접 관리 콘솔을 이용하여 모니터링을 수행하는 것
- **자동 모니터링 도구**
    - 대상 자원의 지표에 대해 임곗값을 정하고, 임곗값을 초과하면 경보(alarm)를 내리는 형태인 동적인 모니터링 방법

### Amazon EC2 인스턴스 구입 옵션
**온디맨드 인스턴스**
- 시작하는 인스턴스에 대한 비용을 초 단위로 지불

**Savings Plans**
- 1년 또는 3년 동안 시간당 비용을 약정하여 일관된 컴퓨팅 사용량을 제공

**예약 인스턴스**
- 1년 또는 3년 동안 인스턴스 유형과 리전을 약정하여 일관된 인스턴스를 제공

**스팟 인스턴스**
- 미사용 중인 인스턴스에 대해 경매 방식 형태로 할당하는 방식