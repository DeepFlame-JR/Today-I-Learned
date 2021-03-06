# 목차
1. [AWS Architect Basic](#aws-architect-basic)
    - [AWS 서비스](#aws-서비스)
    - [AWS 인프라](#aws-인프라)
    - [AWS Well-Architected](#aws-well-architected)
1. [계정 보안](#계정-보안)
    * [보안 주체 (Principal)](#보안-주체-principal)
    * [보안 정책](#보안-정책)
    * [다중 계정 관리](#다중-계정-관리)
1. [네트워킹](#네트워킹)
    * [IP 주소 지정](#ip-주소-지정)
    * [VPC](#vpc)
    * [Transit Gateway](#transit-gateway)
    * [하이브리드 네트워킹](#하이브리드-네트워킹)
    * [Route 53](#route-53)
1. [컴퓨팅](#컴퓨팅)
    * [컴퓨팅 서비스](#컴퓨팅-서비스)
    * [EC2 인스턴스](#ec2-인스턴스)
    * [EC2 요금제 옵션](#ec2-요금제-옵션)
    * [인스턴스 스토리지](#인스턴스-스토리지)
    * [AWS 기반 HPC (High Performance Compute)](#aws-기반-hpc-high-performance-compute)
    * [AWS Lambda](#aws-lambda)
1. [스토리지](#스토리지)
    * [Amazon S3 (Simple Storage Service)](#amazon-s3-simple-storage-service)
    * [공유 파일 시스템](#공유-파일-시스템)
    * [데이터 마이그레이션](#데이터-마이그레이션)
1. [데이터베이스 서비스](#데이터베이스-서비스)
    * [관계형 DB](#관계형-db)
    * [비관계형 DB](#비관계형-db)
    * [데이터베이스 캐싱](#데이터베이스-캐싱)
1. [모니터링 및 스케일링](#모니터링-및-스케일링)
    * [모니터링](#모니터링)
    * [Load Balancing](#load-balancing)
    * [Auto Scaling](#auto-scaling)
1. [자동화](#자동화)
      * [배포 자동화](#배포-자동화)
1. [컨테이너](#컨테이너)
1. [서버리스](#서버리스)
    * [API Gateway](#api-gateway)
    * [Amazon SQS (Simple Queue Service)](#amazon-sqs-simple-queue-service)
    * [Amazon SNS (Simple Notification Service)](#amazon-sns-simple-notification-service)
    * [Kinesis](#kinesis)
    * [Step Functions](#step-functions)
1. [엣지 서비스](#엣지-서비스)
    * [CloudFront](#cloudfront)
    * [Global Accelerator](#global-accelerator)
    * [DDoS 보호](#ddos-보호)
    * [Outposts](#outposts)
1. [백업 및 복구](#백업-및-복구)
     * [AWS Backup](#aws-backup)


# Architecting on AWS
AWS 서비스를 파악하고 기능을 비교하며, 복원력있고 안전하며 가용성이 뛰어난 IT 솔루션을 AWS에서 설계한다.
   
# AWS Architect Basic
- 모범 사례에 따라 클라우드 인프라를 어떻게 구성?
- AWS 글로벌 인프라 어떻게 구성?
- 리소스에 대한 액세스 제어?

## AWS 서비스
  
### AWS 사용 이점
- 민첩성 증가: 출시 기간 단축, 혁신 확대, 원활한 스케일링
- 복잡성 및 위험성 감소: 비용 최적화, 보안 강화, 관리 복잡성 감소

### AWS 서비스 연결
1. AWS 관리 콘솔 (70% 정도 제공됨, ID/PW)
1. AWS CLI (Access key ID / Secret access key)
1. AWS SDK (Access key ID / Secret access key)

AWS 내부는 거의 모든 부분이 API로 구성되어 있다.  
명령들이 API로 전달되어 행해진다.

EC2 생성 방법
1. AWS 관리 콘솔: 일회용 또는 임시 인스턴스를 빠르게 시작
1. 스크립트: 반복 사용할 수 있고, 안정적인 방식으로 인스턴스 생성
1. CloudFormation: 관련 리소스를 함께 시작하는 경우, 코드형 인프라 개념을 사용하려는 경우

### 서비스 종류
1. 비관리형 서비스
    - OS 설치, 서버 유지 관리 등 인프라는 AWS가 관리.
    - 인프라에서 발생하는 상황은 고객이 제어.
    - ex. EC2

2. 관리형 서비스
    - 고객은 애플리케이션 최적화만 신경 쓰고, 나머지는 AWS가 관리.
    - ex. RDS

3. 서버리스 서비스
    - 사용자가 가상 머신이나 서버를 소유하지 않고, 필요에 따라 서비스를 프로비저닝.
    - VPC 인프라 내에서 작동하지 않음. (리전 서비스라고도 불림.)
    - 엔드 포인트 URL로 서비스에 접근. (엔드포인트를 통해서 다이렉트로 접근할 수 있다.)

## AWS 인프라
AWS 글로벌 클라우드 인프라는 업계에서 가장 안전하고 광범위하고 안정적인 클라우드 플랫폼으로, 전 세계 데이터 센터를 통해 완전한 기능을 갖춘 200가지가 넘는 서비스를 제공함

### 리전
- 리전 간의 완전히 독립적. (내결함성, 안정성 보장)
- 가용 영역 2개 이상을 가지고 있음. (서울 4개 AZ)
- 가버넌스(규칙), 지연 시간, 서비스 가용성, 비용에 따라 선택

### 가용 영역 (AZ)
- 리전 내 데이터 센터들의 집합. (1개 이상의 데이터 센터를 가지고 있음)
- 내결함성을 갖도록 설계.
- 고속 프라이빗 링크를 통해 상호 연결.
- 고가용성 달성에 사용됨.

### 엣지 로케이션
- 사용자로 부터 가장 가까운 AWS 데이터 센터
- CloudFront의 캐싱 콘텐츠가 한다. 
- Route 53, CloudFront, Global Accelerator, WAF, shield의 제한된 서비스만 제공. (EC2, RDS 서비스 등을 설치할 수 없음)
- 고객이 소비할 데이터가 상파울로의 S3에 있다면 접근할 때마다 시간이 많이 소요될 것이다. 이러한 데이터를 엣지 로케이션에 저장하여, 지연 시간과 비용적 측면에서 이점을 볼 수 있다.

### Local Zone
- 리전까지 너무 멀어서 속도가 너무 느릴 때, 엣지 로케이션에서 특정 서비스를 사용하고 싶을 때 사용.
- 엣지 로케이션에서 EC2, RDS 서비스 등을 제공하는 서비스.  
    (한국은 거리가 먼 지역이 없어서 존재하지 않음)
- 미디어 및 엔터테인먼트 콘텐츠, 실시간 게임, 기계 학습 추론, 스트리밍, AR/VR

### Wavelength
- 5G 사업자를 위한 서비스

## AWS Well-Architected
현재 구축된 아키텍쳐가 잘 구성되었는지 모범 사례와 비교하여 확인하기 위함 (QnA)

### WAF 확인 사항
1. 보안
    - 최소 권한의 원칙
    - MFA (다중 인증) 사용
1. 비용 최적화
    - 비용 효율적 리소스 사용
1. 안정성
    - 장애 복구 절차 테스트
    - 스케일링으로 가용성 최대화
1. 성능 효율성
    - 지연 시간 감소
    - 서버리스 아키텍처 사용
1. 운영 우수성
    - 운영 도구를 사용하는가
    - 예기치 않은 이벤트 대응
1. 지속 가능성
    - 환경에 대한 영향 이해

### AWS Well-Arhitected Tool
- 모범 사례와 비교하여 아키텍처 검토
- 워크로드 정의 > 아케턱처 검토 수행(QnA) > 모범사례 적용

### AWS Trusted Advisor
- 모범 사례를 따르는데 도움이 되는 권장 사항 제공.
   
# 계정 보안
- Access Advisor: 서비스에 대한 권한 액세스가 일어난 최근 파악
- AWS IAM Access Analyzer: 외부 entity와 공유되는 리소스 파악 용이

## 보안 주체 (Principal)
권한을 받는 주체
1. IAM 사용자
1. IAM Role
1. AWS 서비스

### 루트 사용자
- AWS 서비스에 대한 전체 액세스 권한 보유. 
- Account ID(12자리 숫자)가 주어짐. > 이 안에 많은 사용자를 생성할 수 있음
- AWS와의 일반적으로 사용하면 안 됨.
- MFA를 설정할 수 있음

### IAM
- 계정 내의 사용자 (EC2 인스턴스나 DB 내의 사용자 X)
- 사용자, 그룹 및 Role(임시 자격/모자)을 생성하고 관리
    - 그룹에 Policy를 연결하고, 속한 사용자들에 적용된다.
- AWS 서비스 및 리소스에 대한 접근 관리
- 액세스 제어 분석

#### IAM Group
User에 대한 권한 할당을 상속 형태로 간편화하기 위해 사용

#### IAM Role
- 임시로 서비스에 대한 특정 권한을 위임하여, 작업을 하는 동안만 유효하게 사용 가능.
    - 역할이 있을 때는 역할 정책이 우선적으로 처리됨   
    - 직원에게 교차 계정 접근 권한을 부여
    - AWS 또는 오프레미스에서 실행되는 애플리케이션이 AWS API 호출을 수행하도록 허용
    - AWS 서비스가 사용자 대신 AWS API 호출을 수행하도록 허용
- ex. EC2가 S3에 접근할 때마다 Role을 부여받아 사용한다. (EC2 메타데이터를 활용)  
ex. Account가 다를 때, switching role을 통해서 다른 Account의 권한을 변환할 수 있음
- STS (Security Token Service)를 통한 **임시 보안 자격 증명**을 생성  
    Access key, Security Key, Session Token  
    SAML, Web Identity

## 보안 정책
Json 형태로 액세스 권한을 평가

### 정책 유형

#### 권한 부여   
- 자격 증명 기반 정책: 사용자 중심 (Users, Groups, Role)
    - managed: 대상이 사라져도 유지가 됨 (1:n) (AWS, Customer)
    - inline: 대상이 사라지면 사라짐 (1:1)
- 리소스 기반 정책: 리소스 중심 (S3, Bucket), 모두 inline (리소스가 사라지면 사라짐)

#### 최대 권한 설정 (Grant permissions)
- IAM 권한 경계
- AWS Organizations 서비스 제어 정책 (SCP)

#### 권한 평가  
자격 증명 기반 정책과 리소스 기반 정책은 함께 평가된다.  
1. 명시적 거부? > DENY
1. 명시적 허용? > ALLOW
1. 묵시적 거부 > DENY

### 심층 방어
- 사용자 > (IAM 정책) > IAM Role
- IAM Role > (VPC 엔드포인트 정책) > Amazon S3 VPC 엔드포인트
- Amazon S3 VPC 엔드포인트 > (버킷 정책) > S3 버킷
- AWS KMS 키 > (AWS KMS 키 정책) > 문서

### 정책 요소
- Effect* : 정책에서 액세스를 Allow/Deny 표시
- Principle : 계정, 사용자, 역할 또는 페더레이션 사용자로 액세스를 허용할 것인가 (리소스 정책에서만)
- Action* : 정책이 허용하거나 거부하는 작업 목록
- Resource* : Action이 적용되는 리소스 목록 (arn: 리소스 식별)
- Condition : 정책에서 권한을 부여하는 상황 정의

```json
{
    "Version": "2012–10–17",
    "Statement": {
        // Allow 또는 Deny
        "Effect": "Allow", 
        // 해당되는 작업에 접근할 수 있다.
        "Action": [
            "dynamodb:*",
            "s3:*",
            "iam:GetGroup"
        ],
        // 해당 리소스에 접근할 수 있다.
        "Resource": [
            "arn:aws:dynamodb:region:account-number-without-hyphens:table/EXAMPLE-TABLE"
            "arn:aws:iam::609103258633:group/Developers",
            "arn:aws:iam::609103258633:group/Operators"
        ],
        // 사용자가 Owner일 때만 Action을 하겠다.
        "Condition" : {
            "StrginEquals":{
                "ec2:ResourceTag/Owner": "${aws:username}"
            }
        }
    }
}
```


## 다중 계정 관리 
- 다수의 팀
- 보안 및 규정 준수 제어
- 결제
- 격리

### 계정 페더레이션
- AWS와 사전에 서로 인증정보를 공유하겠다 Federation을 맺고 서비스 이용에 따른 권한은 Role로 제공
- 팀 인증을 단순화하기 위함
- 통합 인증(SSO)를 위해 회사 계정과 AWS 계정을 일치 시킴

### AWS Organizations
- 다중 계정 관리를 위해서 계정을 조직 단위(OU, Organization Unit)으로 그룹화하여 계층 구조를 생성
- OU 내 모든 계정에 SCP(Service Control Policy)가 적용
- 통합 과금 방식의 장점을 활용, 무료임.
- IAM과 Organizaion의 교집합만이 허용됨.
- 만약, Organization이 없다면 같은 정책임에도 계정이 다르면 중복 작업을 해줘야함.

AWS Organizations vs IAM Group  
- Organizations: 전체에 적용되는 최대 허용을 제한. Deny 위주
- IAM Group: IAM 그룹에 속한 사용자가 특정 서비스를 활용할 수 있다. Allow 위주

### AWS Control Tower
- 초기에 다중 계정을 모범사례를 활용하여 쉽게 구축.
- 거버넌스(규약)를 적용.
- 로그 아카이브, 프로비저닝된 계정 등의 여러 서비스를 추상화하여 적용.


# 네트워킹

## IP 주소 지정
AWS에서 IP 주소 공간 계획하기.

### CIDR 표기법
VPC 안에서 어떻게 IP를 사용할 것인가를 결정 (총 5개의 CIDR 블록이 있음)

- 192.168.1.0/24  
    앞에 3개는 고정하고(24/8=3) 맨 뒤만 변경해서 2^8 만큼 사용하겠다.
- 10.0.0.0/16  
    앞에 2개는 고정하고(16/8=2) 2 개만 변경해서 2^8 * 2^8 만큼 사용하겠다.

#### Public/Private IP
- Public IP: 인터넷을 통해서 연결할 수 있는 IPv4 주소
- Private IP: 인터넷에 접근할 수 없음

## VPC
- 워크로드의 논리적 격리를 제공
- 단일 리전 
- 2개 이상의 가용 영역을 포함함
- 초기에 기본 VPC가 제공됨

#### 여러 가용 영역에 VPC 배포
- 여러 가용 영역에 배포하여 고가용성 달성.
- 각 가용 영역에 서브넷을 생성, 리소스를 배포
- ELB를 통해서 가용 영역 간에 트래픽을 분산

#### 다중 VPC 이점
- 각 애플리케이션 환경의 모든 리소스 프로비저닝 및 관리를 완전히 제어하는 단일 팀 또는 조직에 적합
- 다중 VPC는 논리적 격리를 생성할 수 있음
- AWS Organizations, Control Tower 등을 통해

### 서브넷
- VPC CIDR 블록의 하위 집합 (VPC의 IP를 나누어 사용하겠다.)
- 하나의 가용 영역에 여러개가 포함될 수 있음
    - 생성시, 기본 Private 서브넷
    - 일반적으로 public 서브넷 개수 < Private 서브넷 개수

#### Public 서브넷
- 공인 IP 주소를 통한 외부 통신이 가능 (공인 IP와 사설 IP를 가짐)
- 라우팅 테이블에 (대상위치: 0.0.0.0/0 | 대상: igw-id) 추가
    - igw: 인터넷 게이트웨이
    - 라우팅 테이블 : 대상위치, 대상이 정의되어 있음  
- 인바운드/아웃바운드 가능

#### 인터넷 게이트웨이
- VPC 인스턴스와 인터넷 간 통신을 위해서 반드시 필요 
- VPC당 하나 설정할 수 있음
- 기본적으로 수평 확장, 중복, 고가용성
- 인터넷으로 라우팅 가능한 트래픽에 대한 서브넷 라우팅 테이블에 대상을 제공

<img src="https://blog.kico.co.kr/wp-content/uploads/2022/03/%EC%9D%B4%EB%AF%B8%EC%A7%80-10110.png" height = 350>

#### ENI(Elastic Network Interface)
- Priavet IP 주소, Elastic IP 주소, Mac 주소를 유지
- 동일한 가용영역의 리소스 간에 이동이 가능

#### NAT(Network Address Translation)
- Private 서브넷의 아웃바운드 트래픽을 보내기 위해서 필요 
- Public 서브넷를 거침

<img src="https://elegantcoder.com/wp-content/uploads/2019/07/nat-gateway-diagram-738x510.png" height = 400>

### 가상 방화벽
#### Network ACL (Access Control List)
- 1차 보안 계층
- VPC 서브넷의 가상 방화벽
- 모든 VPC에는 기본 NACL이 포함된다. (인바운드/아웃바운드 모두 허용)
- 상태 비저장. 모든 트래픽에 대한 명시적 규칙이 필요.
- 번호가 가장 낮은 규칙부터 평가

#### Security Group
- 2차 보안 계층
- 인스턴스의 가상 방화벽 (리소스의 ENI에 걸려있는 가상 방화벽)
- 인바운드/아웃바운드 수동 제어 (인바운드 모두 거부/아웃바운드 모두 허용)
- 보안 그룹 연결
    - source를 다른 보안 그룹으로 연결하여 마치 체인 연결처럼 구성
    - 인바운드/아웃바운드 규칙에 의해 설정 가능

<img src="https://lh6.googleusercontent.com/9YpYXmCU-aCyEyCgutFTNJd6hTbCp79I1qoz1Iz_ZY85IbbOU2Rv8Vg89_4jhbb3tnt6_YGf_RRKlqDmIbz4XGpl2cdtc8v1fIFoAx9S2U54NrMGns05XAJsLYGAAGA4CCU7K_8" width=500>

VPC Designer: https://vpcdesigner.com/

### VPC 엔드포인트
- 인터넷을 사용하지 않고, AWS 리전 서비스(S3, DynamoDB 등)에 연결할 수 있는 서비스
- 동일한 리전에 있어야함
- 완전관리형 서비스(수평 스케일링. 이중화. 가용성)

#### 게이트웨이 엔드포인트
- S3, DynamoDB 접근
- Private 서브넷의 라우팅 테이블에 S3.prefix.lsit/DDB.prefix.list 가 포함된다

#### 인터페이스 엔드포인트
- 그 외 AWS 서비스 접근
- ENI 형태로 제공됨 (라우팅 테이블)
- 온프레미스 <-> S3에서도 사용됨

### VPC 피어링
- 리전 간, 계정 간 VPC를 서로 연결
- Private IP 주소 사용
- 전이적 피어링 관계 없음 (VPC A <-> VPC B <-> VPC C : VPC A와 VPC C는 연결 X)
- 완전 관리형 서비스

#### 피어링 연결
- 라우팅 테이블에 VPC 피어링을 추가한다.
- 글로벌 AWS 서버에서 유지됨 (인터넷 연결이 필요없음)

## Transit Gateway
- 중앙에서 네트워크 트래픽을 라우팅 테이블을 통해서 어디로 보내야할지 지정
- 유연한 라우팅 및 세분화
- 리전당 1개 생성
- 완전관리형 서비스

#### 기능
- 최대 5000개의 VPC와 온프레미스 환경 연결
- 모든 트래픽의 허브 역할
- 멀티 캐스트 및 리전 간 피어링 허용

#### 구성 요소
- 연결: VPC, VPN 연결, Direct Connect 게이트웨이, Transit Gateway Connect or 피어링 
- Transit Gateway 라우팅 테이블

<img src="https://miro.medium.com/max/700/1*ghrEMYcgoEfzawAHq54v_A.png" width=500>

## 하이브리드 네트워킹
온프레미스 서버와 AWS Cloud 간의 연결

### AWS Site-to-Site VPN
- 공용 이터넷 사용
- 인터넷 프로토콜에 랩을 씌워 보안을 강화함
- 온프레미스 <-> *고객 Gateway <-> 공용 인터넷(VPN 연결) <-> *VPN 연결 옵션 <-> VPC
    1. 가상 Private Gateway
    1. EC2 인스턴스
    1. Transit Gateway

### AWS Direct Connect
- 데이터 센터에서 AWS 리소스까지 광 링크로 구축
- 인터넷을 거치지 않아 속도가 빠름
- 가상 Private Gateway 활용
- 고객 Gateway <-> 고객 라우터 <-> AWS 라우터 <-> 가상 Private Gateway

#### 연결 옵션
1. Private VIF(가상 인터페이스) > Private Gateway
1. Private VIF > Direct Connect Gateway
1. Private VIF > Direct Connect Gateway > Transit Gateway

## Route 53
AWS의 DNS 서비스
- 도메인 이름을 IP주소로 확인
- 도메인 이름을 등록

#### 라우팅 정책
1. 단순
1. 장애 조치: 헬스 체크를 해서 반응하지 않으면 트래픽을 보내지 않는다.
1. 지리적 위치: 사용자의 위치를 기준으로 가까운 리전에 트래픽을 보낸다.
1. 지리적 근접성
1. 지연 속도 기반: 트래픽을 보낼 때 테스트를 하며 지연속도가 빠른 쪽으로 트래픽을 보낸다.
1. 다중값 응답: DNS에 여러 IP주소가 있을 때, 여러 값에 반응한다.
1. 가중치: 트래픽에 %를 지정하여 트래픽을 보낼 수 있다.

#### Route 53 Resolver
- 하이브리드 클라우드용
- 온프레미스와 AWS 간에 원활한 DNS 질의 확인이 가능

# 컴퓨팅
- AWS 컴퓨팅 서비스 종류?
- AMI란 무엇인가?
- 비용 최적화는 어떻게?
- Lambda는 무엇인가?

## 컴퓨팅 서비스
1. Amazon EC2  
    AWS 클라우드 컴퓨팅의 중심 구성 요소. 가상화 서버
1. Amazon ECS(Elastic Container Service)  
    Docker 컨테이너를 사용하여 EC2 인스턴스의 관리형 클러스터에서 분산 애플리케이션 기능 도입
1. AWS Lambda  
    서버리스. 완전 관리형 서비스.
1. AWS Fargate  
    ECS는 결국 가상 서버에 올리는 것이다. (EC2에 올라감)  
    따라서 컨테이너를 좀 더 자동화하여 사용하기 위해서 나타남

## EC2 인스턴스
- 안전하고 크기 조정 가능한 컴퓨팅 용량을 제공함. 
- 수요 변화에 맞춰 컴퓨팅 용량 추가/제거 가능.
- 리전의 물리 서버가 EC2 인스턴스를 호스트.

### AMI (Amazon Machine Image)
- 인스턴스의 소프트웨어를 결정한다.
    - Linux, Windows, 64/32 bit 등
- 이점: 반복, 재사용, 복구 가능
- 작성된 인스턴스에서 생성할 수 있음. (Amazon EC2 Image Builder)
- AWS 기본 제공/Marketplace/community AMI

### Instance type
- 인스턴스의 하드웨어를 설정한다.
- F.G.A.S 유형 (t3n.large)
    - Family: CPU 타입을 저장한다. 
        - 범용: 메모리:vCPU = 4:1   
        (t시리즈: 기준 성능 이상의 버스팅을 할 수 있음. 트래픽이 들쑥날쑥할 때 사용 / m시리즈. 트래픽이 안정적일때)
        - 메모리 최적화: 메모리:vCPU = 8:1
        - 컴퓨팅 최적화: 메모리:vCPU = 2:1
        - 스토리지 최적화: 스토리지 읽고 쓰기가 빈번한 컴퓨팅
        - 액셀러레이티드 컴퓨팅: GPU 중심. 머신러닝 등을 위한 컴퓨팅.
    - Generation: 인스턴스 세대 (최신 CPU가 가성비가 좋음)
    - Attribute: 속성 (n: 네트워크 )
    - Size: 인스턴스 크기 
- Compute Optimizer: 기계 학습을 사용하여 최적의 인스턴스를 사용할 수 있도록 권고
    - EC2, EBS, Lambda

### 사용자 데이터
- 인스턴스가 시작할 때 루트 사용자로 스크립트를 실행
- 공통적인 자동화 구성 태스크를 수행하는 데에 사용

### 메타데이터
- 인스턴스가 가지고 있는 고유의 정보
- 자동화에 사용할 수 있음
- 해당 인스턴스에서만 참조할 수 있음

### Tag
- AWS 리소스에 태그를 할당함
- 리소스 관리, 검색 및 필터링
- 태그는 많은 것이 좋음

## EC2 요금제 옵션
아래 설명되는 인스턴스들을 섞어서 사용함

### 예약 인스턴스
- 약정 및 안정적 상태의 사용량
- 온디맨드 요금 대비 할인
- 1년 또는 3년

#### 표준 예약 인스턴스
- 온디맨드 대비 최대 72% 할인
- 사용량이 지속적인 경우

#### 컨버터블 예약 인스턴스
- 온디맨드 대비 최대 54% 할인
- 변경 결과가 동일하거나 더 높은 값이면 예약 인스턴스의 속성 변경 가능

### Savings Plans
- 예약 인스턴스에서 더 높은 유연성 (Fargate, Lambda 사용량에 적용됨)
- 1년 또는 3년

### 스팟 인스턴스
- 내결함성, 유연성, 무상태 워크로드
- AWS 용량을 필요할 때 가져다 쓸 수 있다.
- 온디맨드 요금 대비 최대 90% 할인
- AWS가 필요할 때 언제든지 가져감 (2분 Notice)

### 온디맨드 인스턴스 
- 들쭉날쭉 워크로드 또는 일시적 필요
- 약정없이 시간 단위로 컴퓨팅 용량을 구입

## 인스턴스 스토리지

### Amazon EBS
- 블록 단위로 데이터가 저장됨
- EC2 인스턴스와 독립적으로 지속됨 (비휘발성)
- 가용 영역 내에서 정의됨 (인스턴스와 연결하기 위해서)
- 99.999% 가용성
- 스냅샷을 통한 백업 (S3)

#### 유형
1. SSD
    - 메모리 중심. 속도가 빠르다.
    - 고성능, 범용 워크로드에 사용
1. HDD
    - 처리량 최적화
    - 빅데이터 및 자주 접근하지 않는 데이터

### 인스턴스 스토어 볼륨
- EC2에 포함된 스토리지
- 지속적이지 않음 (휘발성)
- 스냅샷을 지원하지 않음
- 모든 인스턴스에 있지는 않다

## AWS 기반 HPC (High Performance Compute)
컴퓨팅 집약도가 높은 워크로드를 실행하는 데 필요한 모든 요소를 지원
- 전산 유체 역할
- 에너지 및 공공 설비 시뮬레이션
- 자율 주행 차량

### HPS 네트워킹
1. 탄력적 네트워크 네트워크: 최대 10GB
1. 탄력적 네트워크 어댑터: 최대 100GB
1. Elastic Fabric Adapter: 최대 400GB

### 배치 그룹
- 클러스터: HPC, 낮은 대기 시간, 높은 처리량
- 분산형: 가용 영역을 여러 개로 나누어 가용성을 높인다. (동시 장애 위험 감소)
- 파티션: 고가용성 및 내구성 높이기 위한 데이터 복제 (Hadoop과 같은 분산 처리 환경 시스템)

## AWS Lambda
### 서버리스 컴퓨팅
- 관리나 운영을 AWS에서 해주는 서비스
- 어플리케이션만 신경쓰면 됨

### Lambda
- 서버리스 컴퓨팅
- 다양한 언어 지원
- 최대 15분간 실행. 최대 10GB 메모리 지원
- 이벤트 방식으로 작동 (ex. S3에 데이터가 들어올 때)

#### 사용 사례
1. 웹 어플리케이션 (자유 사용하지 않지만, 꼭 필요한 어플리케이션)
1. 백엔드
1. 데이터 처리
1. 챗봇
1. Amazon Alexa
1. IT 자동화

시뮬레이션된 브라우저 기반 슬롯 머신 게임    
https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/lambda-examples.html

# 스토리지
- 블록 스토리지: 원시 스토리지. 데이터가 관련없는 블록의 Array로 구성
- 파일 스토리지: 파일(서비스) 시스템이 관련 없는 데이터 블록을 관리
- 객체 스토리지: 데이터, 데이터 속성, 메타 데이터, 객체 ID 등을 객채화 후 저장

## Amazon S3 (Simple Storage Service)
내구성이 뛰어난 객체 스토리지 솔루션 
- 객체 = File + Meta data + key(고유 식별자, URL) 으로 저장
    - URL이 주어지기 때문에 바로 접근할 수 있음
- 정적 데이터에 적합 (조금만 바껴도 처음부터 다시 저장함)
- 비용 절감
- 보안 강화 
    - 저장되는 모든 데이터에 접근 권한을 줄 수 있음 (IAM)
    - 저장시 암호화를 할 수 있음 (SSE-S3, SSE-KMS, SSE-C)
- 버전 관리 가능

#### 버킷 및 객체
- S3에서는 데이터를 버캣 내에 객체 단위로 저장
- 버킷 무제한, 파일 하나당 5TB
- 리전 서비스. URL을 통한 외부에서 다이렉트 접근이 가능하다  
    예) https://(버킷).s3.amazonaws.com/(prefix)/(파일명)  
    https://my-bucket.s3.amazonaws.com/2006-01-01/pup.jpg

#### S3 접근 제어
- 버킷에 접근 권한을 설정할 수 있다.
- public, private, Access Policy
- 외부 접근 차단
    - 새 ACL을 통해 부여된 버킷 및 객체
    - 모든 ACL을 통해 부여된 버킷 및 객체

#### S3 스토리지 클래스
- Standard
    - 자주 접근하는 활성 데이터
    - 밀리초 내에 접근
- Intelligent-Tiering
    - 접근 패턴이 변경되는 데이터
    - 밀리초 내에 접근
- Standard-IA
    - 자주 접근하지 않는 데이터
    - 밀리초 내에 접근
- One Zone-IA
    - 3개의 AZ에 저장하지 않고, 1개만 사용
    - 재생성이 가능하고, 자주 접근하지 않는 데이터
    - 밀리초 내에 접근
- Glacier 
    - 가장 접근 빈도가 낮은 데이터 (분기당 1번)
    - 접근 속도가 느림
    - 분류
        1. Instant Retrieval(검색이 빠름)
        1. Fexible Retrieval(신속, 표준, 대량 검색으로 적당한 시간이 소요 검색 가능)
        1. Deep Archive(12시간 내에 검색이 가능)
    - 비용 효율성이 높음
    - 감사 저장소를 활용
    - 저장소 잠금을 통해서 지우지 못 하도록 할 수 있음
- 수명 주기 정책을 통해서 객체 사용 기간에 따라 조치를 취할 수 있다
    - 30일 이상 경과하면 S3 Standard-IA로 이동
    - 365일 이상 경과하면 S3 Glacier로 이동

#### S3 객체 복제
- 버킷 전체에 걸쳐서 비동기식으로 자동 복사 
- 동일한 리전 또는 다른 리전에 저장 가능(교차 리전 복제)
- 소유자 변경, 스토리지 클래서 설정을 할 수 있다

#### S3 Transfer Acceleration
- 데이터를 신속하게 장거리로 이동 처리한다
- 사용자로부터 가장 가까운 Amazon CloudFront 엣지 로케이션에 업로드 후 Amazon 전용망을 타고 S3로 업로드한다

#### S3 액세스 포인트 솔루션
- 대규모 접근을 관리
- 고유한 권한 및 네트워크 제어가 가능

#### S3 객체 Lambda
- 데이터 형식 간에 변환을 한다. (ex. XML > Json)
- 다른 서비스 또는 DB의 정보를 사용하여 데이터를 보강
- 다운로드할 때 파일을 압축 또는 해체한다

#### 비용
- 지불: 월별 GB, 다른 리전 또는 인터넷 전송, PUT/COPY/POST/LIST/GET 요청
- 비지불: S3로 수신, 동일 리전 또는 CloudFront로 전송

#### 예시 
- 백업 및 복원 (정적 데이터이기 때문)
- 비지니스 크리티컬 애플리케이션 (3개로 복제해서 저장하기 때문에 가용성이 뛰어나다)
- 아카이빙 및 규정 준수 (기업의 계약 등...)

## 공유 파일 시스템
- 여러 인스턴스가 동일한 스토리지를 사용해야할 경우
- Amazon EFS or FSx 
    - EBS: 인스턴스 하나에 연결됨 (최근에는 다중 연결 가능. 특별한 경우 사용.)
    - S3: 객체 스토리지는 파일 시스템용으로 적합하지 않음

### Amazon EFS (Elastic File Storage)
- 볼륨에 대한 백업이 가능하다. (AWS Backup)
- 리전 서비스 > 3개 이상의 가용영역(AZ)에 저장됨 (99.999% 가용성, 내구성)
- 스토리지 기준으로 오토 스케일링
- Throughput Mode: 데이터를 넣을 때 모드 (스토리지 사이즈 결정시 결정됨)
    - Busting: 부하가 많이 걸리면 순간적으로 성능을 늘림
    - Provisioned: 용량 크기에 상관없이 일정한 성능을 보임

#### 설정 및 구성
- 필요한 가용 영역, 서브넷에 탑재됨 
- ENI 형태로 제공 (IP 주소로 접근할 수 있음)

### Amazon FSx
완전 관리형 파일 시스템 제공
- Windows File Server
    - Window 7까지 호환성
    - AWS Directory Service와 연동
    - Samba를 사용하여 Linux 호스트에 탑재 가능
- Lustre
    - Linux 기반의 분산 파일 시스템
    - HPS, SSD 기반, 수십만 개 코어 지원
    - 빅데이터, 기계학습 등에 고성능을 제공
    - S3와 원활히 통합
- NETapp ONTAP
- OpenZFS

## 데이터 마이그레이션
**온프레미스 <> 클라우스** 또는 **클라우드 <> 클라우드**로 옮길 때 사용

### AWS Snowball
페타바이트 규모의 데이터를 오프라인으로 안전한 물리적 전송을 통해 이전
1. AWS Snowcone
1. AWS Snowball Edge: 페타바이트. 데이터를 처리하여 AWS 서버에 데이터를 올릴 수 있음
1. AWS Snowmobile: 초 대용량 엑사바이트

### DataSync
- 온프레미스 파일 스토리지에서 EFS 또는 S3로 동기화
- 여러 파일 시스템에 연결할 수도 있음

### Storage Gateway
- 파일, 볼륨, 테이프 3가지 게이트웨이로 나누어진다.
- HTTPS 포로토콜 사용

<img src="https://d1.awsstatic.com/pdp-how-it-works-assets/product-page-diagram_AWS-Storage-Gateway_HIW@2x.6df96d96cdbaa61ed3ce935262431aabcfb9e52d.png" width=600>


# 데이터베이스 서비스
AWS에서는 다양한 데이터베이스를 제공함

## 관계형 DB
- 정형화된 테이블로 구분됨
- 데이터 정확성 및 일관성을 제공 (데이터 관리에 용이)
- SQL을 활용하여 데이터에 접근할 수 있음 (분석을 위한 데이터의 경우)
- Amazon RDS, Amazon Redsshift, Aurora

### Amazon RDS
- 관계형 데이터베이스 
- 자동 다중 가용영역에 데이터 복제 (내결함성)
- 컴퓨팅 및 스토리지 크기 조정 (최소한의 애플리케이션 가동 중단)
- Aurora, PostgreSQL, MySQL, MariaDB, Oracle, SQL Server 지원

#### 다중 AZ 배포
- 다른 AZ에 대기 DB 인스턴스를 생성하고, 지속적으로 동기화된다.
- 기본 DB만 사용하며, 장애시 대기 DB 인스턴스 사용.

#### 읽기 전용 복제본
- 읽기 중심의 워크로드 처리를 위해 수평 스케일링
- 읽기 성능이 좋음

#### 저장 데이터 암호화
- AWS KMS에서 관리
- 고유 데이터 키로 데이터 암호화
- 모든 RDS 엔진에서 사용 가능

### Aurora
MySQL 및 PostgreSQL 호환되는 관계형 DB
- 성능 및 확장성 (3~5배)
- 가용성 및 내구성 (3개의 AZ에 2개씩 복제본을 가짐. 최대 15개)
- 뛰어난 보안
- 완전관리형

#### Serverless
- 간헐적으로 DB를 사용할 경우 사용
- 온디맨드로 시작하고, 사용하지 않으면 종료됨

### RedShift
- AWS의 데이터 웨어하우스 (쓰기용 스키마, 정형 데이터, SQL 호환)
- 페타바이트 규모의 완전관리형 DW
- 온라인 분석 처리 (OLAP)
- 대규모 병렬 처리
- Amazon S3로 지속적인 자동 백업

#### 사용 사례
1. 비지니스 인텔리전스
1. 이벤트에서 운영 분석
1. 서비스형 데이터
1. 예측 분석

#### 열 기반 스토리지
- 고도의 분석적 쿼리 모델
- 데이터 집계에 강점을 가지고 있음
- *행 기반은 쓰기에 강점이 있음

#### 동시성 스케일링
일시적인 읽기 쿼리 버스팅 시 자동으로 스케일 인/아웃
- 3세대 컴퓨팅 노드: 확장 가능한 스토리지. 데이터 양이 많으면 선택
- 고밀도 컴퓨팅 노드: 고성능. 컴퓨팅 양이 많으면 선택

## 비관계형 DB
- 비정형화된 데이터베이스
- 유동적인 스키마 (데이터가 기존 스키마에 적합하지 않을 때 사용)
- 읽기/쓰기 속도가 SQL보다 빠르다.
- Amazon DynamoDB, ElastiCache

### DynamoDB
완전관리형 NoSQL DB
- Key-value 구조
- 규모에 따른 성능 향상
- 관리할 서버 없음
  
> Key-Value 구조
> - 키-값 페어로 구조화
> - 장애에 대한 복원력이 뛰어남
> - 단순하기 때문에 읽기/쓰기에 최적화됨
> - 데이터가 수평적으로 확장되어 일관된 성능을 제공

#### 테이블
- 필수 키 값 접근 패턴
- 파티션 키: 데이터 분산을 결정
- 정렬 키: 다양한 쿼리 기능을 제공

#### 오토 스케일링
데이터 읽기/쓰기에 따른 용량을 자동으로 조정한다 (비용 절감)
- 프로비저닝: 최소/최대 소비량이 정해짐. 그 사이에서 오토 스케일링
- 온디맨드: 최소/최대 값이 주어지지 않음.

#### 일관성 옵션
- 최종 일관성: 복제가 끝나기 전에 데이터를 읽어오는 것 (Default)
- 강력한 일관성: 복제가 다 끝난 후, 복제본 2개를 읽어와서 데이터가 맞으면 읽어온다

#### 글로벌 테이블
- 리전 간 복제를 자동화
- 게임 서버의 경우 사용

## 데이터베이스 캐싱
- 캐싱은 더 빠른 스토리지를 사용하여 읽기 성능을 객선
- 인메모리 데이터를 사용한다.
- 캐시에서 읽어옴 > 없으면 DB에서 읽어옴 > 캐시 업데이트 (Write-through)

### AWS Elastic Cache
- 탁월한 성능
- 완전 관리형
- 간편하게 확장 가능
- Redis, Memcached와 호환성

#### 데이터 제거
- TTL(Time To Live) 값을 각 애플리케이션 쓰기에 추가함
- TTL이 만료되면 DB에 데이터를 쿼리한다

### DynamoDB Acclerator
- DynamoDB를 위한 완전관리형 고가용성 캐시 
- 초당 수백건의 읽기 요청으로 쉽게 확장

# 모니터링 및 스케일링

## 모니터링
- 운영 상태: 운영 가시성 및 인사이트 확보
- 애플리케이션 성능: 성능 스택의 모든 계층에서 데이터 수집
- 리소스 사용률: 리소스 최적화
- 보안 검사: 보안 및 무결성 관리

### CloudWatch
- 지표 및 로그를 거의 실시간으로 수집
- 경보를 생성하고, 알림을 전송
- 대시보드를 생성하고 확인

#### CloudWatch 경보
지표를 식별하여 경보를 생성하고, 정의된 작업을 실행한다.
- 지표 식별  
    임계값을 초과함/임계값을 초과하지 않음/정보가 부족함
- 경보 생성
- 작업 정의
    - Event Bridge  
    자동화된 워크플로를 시작 (AWS 서비스 외의 이벤트에도 반응하게 설정할 수 있음)  
    Lambda, Step Function(Lambda 병렬 처리), SNS 등의 규칙을 정해서 실행시킬 수 있음

### 로그 유형
CloudWatch Log, CloudTrail, VPC Flow Logs

#### CloudWatch Logs
로드 데이터, 저장소 및 액세스 로그 파일을 사용하여 앱을 모니터링
- 로그 스트림을 통해 로그 이벤트를 수집 (인스턴스 등에서 발생)
- 로그 그룹(필터, 액세스 제어 설정, 보존, 모니터링)으로 묶을 수 있음
- CloudWatch 경보를 유도할 수 있음

#### CloudTrail
- 사용자 활동 및 대부분의 API 사용량을 추적
- S3에 로그를 자동으로 저장 (로그는 정적 데이터이기 때문)
- ex. 누가 특정 인스턴스를 종료했는가?  
    ex. 누가 보안 그룹을 변경했는가?

#### VPC Flow Logs
- 네트워크 인터페이스에서 송수신되는 IP 트래픽에 대한 정보 수집
- ENI > VPC Flow Logs > Cloud Watch, S3
- Flow Log
    - AWS 계정, 네트워크 인터페이스 ID, 시간 등을 포함
    - 5분 간격으로 로그를 모은다 > S3에 저장

## Load Balancing
부하를 분산시킬 수 있는 솔루션  
리스터에 규칙과 대상을 정의한다

### ELB (Elastic Load balancing)
- 자동으로 트래픽을 여러 대상에 분산
- 고가용성 제공
- 보안 기능 통합 (SSL 오프로딩: 복호화를 해주고 인스턴스에 전달, TLS 리스너: 서버 인증)
- 상태 확인: 상태가 죽어있으면 트래픽을 보내지 않음

#### 유형
- Application Load Balancer  
    - HTTP, HTTPS (리디렉션: HTTP<>HTTPS)
    - 유연한 애플리케이션 관리 (Lambda 함수 대상도 가능)
    - 요청 수준(7계층)에서 작동
- Network Load Balancer
    - TCP, UDP
    - 탁월한 성능 및 고정 IP
    - 연결 수준(4계층)에서 작동
- Gateway Load Balancer
    - IP
    - 트래픽의 고급 로드 밸런싱
    - 요청 수준(3계층)에서 작동
    - 3rd-party도 가능

## Auto Scaling
- 부하가 늘어났을 때 자동적으로 서버를 줄이고, 늘릴 수 있는 기능
- Cloudwatch, ELB와 함께 동작함 
    - ex. 모니터링하다가 서버를 늘리고, 이를 ELB에 정보를 보내준다.
- 여러 서비스에서 짧은 간격으로 애플리케이션 스케일링을 제공
- AWS 전체 서비스에 적용 (AWS Auto Scaling) / EC2 인스턴스에 특화 (EC2 Auto Scaling)


#### EC2 Auto Scaling 구성 요소
1. 시작 구성 및 시작 템플릿
    - 어떤 리소스가 필요한가 (인스턴스를 생성하기 위한 요소들)
    - AMI, 인스턴스 유형, 보안 그룹, 역할
1. EC2 Auto Scaling 그룹
    - 어디서, 얼마나 필요한가
    - VPC 및 서브넷, 로드 밸런서
    - 최소/최대 인스턴스, 원하는 용량 
1. Auto Scaling 정책
    - 언제, 얼마 동안 필요한가
    - 예약(ex. 시간이 8~18까지 스케일인), 동적(온디맨드), 예측(스케일 인/아웃 정책)

# 자동화

## 배포 자동화
간편성 기준으로 오름차순
1. Elastic Beanstalk
1. CloudFormation
1. Systems Manager
1. EC2

### Elastic Beanstalk
- 인프라를 프로비저닝하고 운영
- 애플리케이션이 스택을 관리(자동으로 스케일 업 및 스케일 다운)

### CloudFormation
- AWS 리소스 스택을 정의하여 생성
- Json 또는 YAML 형식 템플릿을 업로드 > API 요청으로 변환 > 리소스 스택을 형성
- 여러 계층화된 템플릿을 활용한다

#### 코드형 인프라(IaC. Infrastructure as Code)
- 복제, 재배포 및 용도 변경에 용이
- 새 버전 관리 제어
- 장애 발생시 서비스를 마지막으로 양호한 상태로 롤백

#### 스택
- 인프라 리소스가 만들어지는 논리적인 모습
- 하나의 유닛으로 관리할 수 있는 AWS 리소스의 모음
- 리소스 생성 및 삭제가 가능
- 변경된 스택: 변경 세트 (Preview 가능)

#### AWS CDK (Cloud Development Kit)
- 지원되는 언어를 활용하여 템플릿 생성
- 자동 완성 및 인라인 문서 지원
- 동일한 기본값 및 재사용 가능

### Systems Manager
많은 데이터를 중앙에서 한번에 관리하기 위해서 사용
- 운영 관리 (Explorer, OpsCenter)
- 애플리케이션 관리 (Resource Groups, AWS AppConfig, Parameter Store)
- 작업 및 변경 (자동화, 유지 관리 기간)
- 인스턴스 및 노드 (Inventory, Run Command, 패치 관리자)
- 무료 서비스

#### 이점
- 문제 탐지에 걸리는 시간 단축
- 작업을 자동화하여 효율성 향상
- 가시성 및 제어 개선
- 하이브리드 환경 분리

# 컨테이너
- 반복 가능
- 독립적 환경
- VM보다 더 빠른 가동/중단
- 이동성, 확장성

### 마이크로 서비스
- 각각의 서비스들을 나누어서 개별성과 독립성을 넓힘
- 서비스 독립성은 장애 저항성을 높임
- 소결합: 로드밸런서와 메시지 대기열을 통해서 구성 요소들을 디커플링 한다.

#### AWS X-Ray
- 마이크로서비스 및 서버리스 아키텍처를 사용하여 구축된 애플리케이션을 분석. 디버깅.
- 데이터 수집, 트레이스 기록, 문제 분석

### VM vs Container
- VM: 격리되어 있지만 동일한 OS 및 바이너리/라이브러리를 공유하지 않음
    - OS가 각각 구축되어 있어야하기 때문에 무겁고, 느리다.
- Container: 격리되어 있지만 OS를 공유하고, 필요한 경우 바이너리/라이브러리 공유

### Amazon ECR (Elastic Container Registry)
- Container Image를 빌드 및 저장 

### Amazon ECS (Elastic Container Service)
- Container Image를 EC2에서 구동 및 관리
- Application Load Balancer에서 받은 작업을 여러 인스턴스에 분배한다. (장애가 나는 것에 대비)

### Amazon EKS (Elastic Kubernetes Service)
- 대규모 애플리케이션 실행
- 어디서든 실행
- 새로운 기능 추가

### AWS Fargate
- 기존: 컨테이너 이미지 빌드 > ECR에 저장 > ECS/EKS 수행 > EC2를 실행 + Fargate 관리
- EC2를 사용하면 사용자가 관리해야하는 부분이 많아지는 단점 해결
- 완전관리형 (스케일링, 최적화)

# 서버리스
- 프로비저닝 또느 관리할 인프라가 없음
- 오토 스케일링
- 내장된 보안, 고가용성 컴퓨팅
- 소결합, 확장성이 뛰어난 워크로드를 생성

## API Gateway
API 생성/관리/배포하는 완전관리형 서비스
- 여러 마이크로 서비스를 위한 통합 API 생성
- 수천 건의 동시 API 호출
- WAF와 연동하여 DDoS 보호 및 제한 기능을 제공
- 서드 파티 개발자에 의해 API 사용 조절

<img src="https://docs.aws.amazon.com/ko_kr/whitepapers/latest/microservices-on-aws/images/image3.png" width=650>
- CloudWatch로 모니터링 가능
- API Gateway response cache: 자주 사용하는 API를 관리

## Amazon SQS (Simple Queue Service)
- 완전관리형 메시지 대기열 서비스
- 처리 및 삭제될 때까지 메시지를 저장
- 발신자와 수신자 간 버퍼 역할 담당

#### SQS 사용 사례
- 대기열 유형
    - 표준: 순서 미지정. 최소 1회이상의 수행을 보장. 제한이 없는 초당 API 호출
    - FIFO: 순서 지정. 1회 수행. 제한된 초당 API 호출
- 요청 오프로딩: 오래 걸리는 것들을 별도로 빼서 처리할 수 있음
- 실패한 단계에 대한 허용 오차를 생성
- 성능 요구를 위한 오토 스케일링

#### SQS 메시지 수명주기
- 메시지 생산자 > SQS > 메시지 소비자
- 메시지 소비자가 처리 후, SQS에서 메시지를 삭제한다.
- 가시성 시간 제한: 메시지 소비자가 처리를 위해 가져간 후 큐에 메시지가 보이지 않는 최대 시간 (메시지 소비자가 처리후, 삭제하는 최대 시간)

#### DLQ (Dead-Letter Queues)
- 최대 처리 시도 수를 넘어간 메시지
- 다른 큐에 따로 처리한다.

## Amazon SNS (Simple Notification Service)
- 단일 메시지를 단방향으로 보냄
- 게시자와 구독자가 존재한다. (1:N)
    - 구독자 유형: Email/SMS/모바일 푸시 알림
- 메시지가 지속되지 않음

|기능|Amazon SNS|Amazon SQS|
|---|---|---| 
|메시지 지속성|X|O|
|전송 매커니즘|푸시(수동적)|풀링(능동적)|
|생산자 및 소비자|게시자 및 구독자|발송 또는 수신|
|배포 모델|1:N|1:1|

## Kinesis
실시간 데이터 수집 및 분석을 위함
- Kinesis Data Stream/Data Firehose/Data Analytics/Data Video Stream

#### Kinesis Data Stream
- 실시간 데이터 스트림 수집 및 저장
- 샤드가 시퀀스에 저장된 실시간 데이터를 보관
- 소비자가 샤드에서 데이터를 읽어 처리

#### Kinesis Data Firehose
- 데이터 스트림을 처리하여 데이터 스토어에 로드 
- 데이터 처리가 되기에 준실시간 데이터
- 데이터 분석 및 BI를 통해 스티리밍 데이터가 처리

## Step Functions
- 애플리케이션 기능을 단계별 실행
    - 각 단계를 자동으로 시작하고 추적
    - 단계가 실패할 경우, 간단한 오류 파악 및 로깅 제공
- 시각적 워크플로를 사용하여 마이크로서비스 조정
- 병렬 수행, 반복 수행도 가능

# 엣지 서비스
- 엣지 로케이션(사용자로 부터 가장 가까운 데이터 센터)에서 진행되는 솔루션
- 어플리케이션의 성능을 증가시키기위한 솔루션

## CloudFront
- AWS의 글로벌 콘텐츠 전송 네트워크(CDN, Content Delivery Network)
- 400개의 엣지로케이션 네트워크를 사용한다 > 지연 시간 감소 + 비용 감소
- 보안 강화(WAF, Shield와 통합)
- 동적 데이터, 정적 데이터 모두 캐싱.
    - 동적 데이터는 변경되는 데이터라 의미가 없어보이지만, CloudFront와 Origin 사이가 AWS 전용망으로 구성되어있어 더 빠름

#### 캐싱
1. 사용자 컨텐츠 요청
2. 엣지 로케이션 확인  
    1. 컨텐츠가 있으면 CloudFront에서 데이터를 가져옴  
    2. 컨텐츠가 없으면 Origin 에서 데이터를 가져옴 (멀리서 가져올 때 비용이 많이 발생함)  
    2-1. 데이터를 가져와서 엣지 로케이션에 데이터를 캐싱

#### 구성
1. Origin 선택
    - S3 버킷
    - 로드 밸런서
    - 사용자 지정 오리진 (EC2 인스턴스/온프레미스 서버)
1. 배포 생성: 캐시 동작 정의
    - 경로 패턴
    - 포로토콜 정책
    - HTTP 메서드
    - 서명된 URL
    - 캐시 정책(TTL, 이름, 객체 무효화<-비권장)
1. (선택 사항)
    - 함수 연결
    - AWS WAF 웹 ACL 연결
    - 사용자 지정 도메인 이름 추가

## Global Accelerator
- 네트워크 성능 최적화
- 사용자와 AWS 사이에서 인터넷을 사용할 필요 없이 **AWS 전용망**을 이용
- 글로벌 사용자들에게 애플리케이션 성능을 향상시켜주는 서비스

|CloudFront|Global Accelerator|
|---|---|
|7 계층 HTTP 또는 HTTPS|4 계층 TCP 또는 UDP|
|콘텐츠 캐싱 O|콘텐츠 캐싱 X|
|DNS 기반|AnyCast (최적의 경로의 IP에 연결)|
|동적 IP|2개의 글로벌 고정 IP주소|

## DDoS 보호
- DDoS: 감염된 호스트가 트래픽을 비정상적으로 높여 서비스를 이용하지 못 하게 공격
- DDos > Shield > AWS Firewall Manager (AWS WAF)
    - AWS WAF 와 연동: CloudFront, Route 53, API Gateway

### AWS Shield Standard
- 일반적인 DDos 공격을 막음.
- 3, 4 계층 네트워크 방어 (SYN 플러드, UDP 리플렉션 공격 방어)
- 무료. 기본 제공.

### AWS WAF
- 인바운드 트래픽에 대해서 웹 ACL을 만들어 보호
- 웹 ACL 규칙 구문을 사용하여 트래픽 제어
    - 공격 방지: XSS 탐지, AWS의 관리형 규칙
    - 트래픽 필터링: 비율 제한, IP 필터링
    - 패턴 일치: 정규식, 문자열 일치
    - 로깅(Kinesis Data Firehose), 지표(CloudWatch)
    
### AWS Firewall Manager
AWS WAF나 VPC 보안 그룹의 운영 및 유지 관리를 단순화
- 계정 및 리소스 수가 많을 때 
- 지속적으로 새 애플리케이션이 생성됨
- 조직 전체의 위협에 대한 가시성

## Outposts
- 사내 앱과 AWS 앱을 연결해준다
- 짧은 지연 시간을 구현

#### 리소스
- 컴퓨팅 및 스토리지: EC2, EBS, S3
- 네트워킹: VPC, Load Balancer
- DB: RDS, ElastiCache
- 컨테이너: ECS, EKS

# 백업 및 복구

#### 가용성 개념
- 고가용성: 앱의 가동 중단 시간 최소화 (빠른 재해 복구)
- 내결함성: 내장된 중복성
- 백업: 데이터를 안전하게 유지
- 재해 복구: 재해 발생 후 앱 및 데이터를 복원

#### RPO, RTO
- RPO (복구 시점 목표)
    - 얼마나 자주 데이터를 백업해야하는가?
- RTO (복구 시간 목표)
    - 앱을 얼마나 오래 사용할 수 없는가?

### 장애 조치
AWS 서비스는 각각의 다양한 장애 조치 서비스가 있다.

#### 다중 리전
- 1개의 리전 내 2개 이상의 가용 영역을 사용
- 리전을 중복으로 사용할 때도 있다. (글로벌 서비스의 경우)

#### 스토리지 복제
- Amazon S3: 교차 리전 복제 (기본적으로 리전에 3개의 AZ에 저장)
- Amazon EBS: 특정 시점 볼륨 스냅샷
- Snowball: 대용량 데이터 빠르게 전송
- DataSync: 온프레미스 또는 클라우드 파일 시스템의 파일을 EFS와 동기화

#### 복구용 AMI 구성
- 사용자 지정 AMI(골든 이미지) 사용
- EC2 인스턴스 또는 컨테이너가 죽었을 때, 이미지를 활용하여 복원한다.

#### 네트워크 설계
- Route 53: 트래픽 분산 및 장애 조치
- ELB: 로드 밸런싱, 상태 확인 및 장애 조치 
- VPC: 온프레미스 네트워크 토폴로지
- Direct Connect: 온프레미스 환경의 빠르고 일관적인 복제 및 백업

#### DB 백업 및 복제본
- Amazon RDS: 읽기 전용 복제본을 다중 AZ와에 저장, 데이터 스냅샷을 다른 리전에 저장 (자동 백업을 보존)
- DynamoDB: 전체 테이블을 몇 초만에 백업, 글로벌 테이블로 전 세계에 다중 마스터 테이블 생성

## AWS Backup
- 중앙 집중식 백업 전략
- AWS 리소스 간에 백업
- 완전 관리형 서버리스 서비스

#### 백업 계획 생성
1. 백업 일정의 시점 및 빈도를 정의
1. 수명 주기 정책을 사용하여 이동 시점 및 보존 기간을 결정
1. 태그 또는 리소스 이름을 사용하여 리소스 할당
1. 백업을 관리하고 모니터링