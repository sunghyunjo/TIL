# Amazon VPC

## Virtual Private Cloud

- 사용자가 정의한 가상의 네트워크 환경 (논리적 격리)
- 완전한 네트워크 제어가능 (보안그룹, Network ACL)
- 다양한 연결 옵션 제공

### VPC구성 방법 정리
#### 1) VPC 생성 - Region 선택
- 용어정리 : CIDR (Classless Inter-Domain Routing)

#### 2) Subnet - 가용영역(AZ)내에 필요한 Subnet 정의

- 가용영역을 지정하여 인스턴스를 구성하는 것.
- Subnet 형태를 지정 가능 (Private/Public)

#### 3) Routing 생성 - 라우팅 규칙 정의

3-1) Routing 생성 - VPC Gateways

3-2) Routing 생성 - NAT Gateway

- AWS가 완전하게 관리하는 서비스

3-3) Routing - VPC Peering

- 동일 Region 내 VPC간 완전히 격리된 전용의 연결
- VPC간 하나의 Peering만 제공되며, VPC간 IP Address가 중복 될 수 없음.
    - 다수의 중계를 하고싶다면 하나의 VPC를 Transit VPC로 구성하여 확장할 수는 있음.
- 고가용성 및 Traffic에 대한 수평적 확장 제공
- Routing Table을 통하여 통제가 가능하고, Transitive Peering은 제공하지 않음.

3-4) Routing - VPC Endpoints (S3/Dynamo DB)

- 인터넷을 경유하지 않고 S3를 사용
- VPC와 Amazon S3간 전용의 연결을 제공
- 다양한 접근 제어 정책 적용

#### 4) VPC Flow Logs

- VPC, Subnet, EC2 Instance ENI 단위로 적용 가능한 Network Packet 수집
- 생성되는 모든 Packet에 대한 정보를 Cloud Watch 로 볼 수 있음.

# Networking Service

## Elastic IP (EIP)

- Account에 할당되어 변경되지 않는 Elastic IP(EIP)를 할당
- EC2 Instance 장애시, 다른 EC2 Instance로 EIP를 Re-Associate
- Region 당 5개의 Elastic IP Address 할당

## Elastic Load Balancer

- ELB의 IP Address는 지속적으로 변경될 수 잇음.
    - ELB의 DNS Name Record를 사용

## CloudFront

- CDN이란? 사용자를 가까운 PoP에 연결시켜 캐싱 및 가속 기술을 사용하여 접속을 도와주는 것
- DDos 방어 측면에서도 유리

## Security Group vs Network ACL

- 차이점 : 피티보고 정리하기
