
### VPC구성 및 eksctl 기반 설치
- 구성
![[Pasted image 20230419083749.png]]


### Cloudformation 구성

#### 소개
- AWS Cloudformation에서는 클라우드 환경에서 리소스를 모델링하고 프로비저닝할 수 있도록 공용 언어를 제공

#### VPC 구성
- NAT Gateway 3개 사용
- NAT Gateway는 3개의 EIP를 사용
- 현재 계정의 EIP 최대 할당 숫자는 5개, EIP가 3개 이상 여유가 있어야 배포 가능

#### 1. VPC yaml 
```yaml

```