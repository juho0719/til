
### 소개
- AWS Cloud9은 브라우저만으로 코드를 작성, 실행 및 디버깅할 수 있는 클라우드 기반 IDE
- 코드 편집기, 디버거 및 터미널이 포함 (EC2위에 VSCode가 올려져 있는 형태)
- 클라우드 기반이라 인터넷이 연결된 환경 필요

### 1. Cloud9 IDE 생성
- Cloud9으로 접속하여 `Create environment`를 클릭
- Cloud9의 이름과 Description 설정 후 New EC2 Instance 그대로 설정
![[Pasted image 20230417202347.png]]
- Intance type, Platform, Timeout을 설정
- Timeout의 경우 사용자입력이 없고 얼마 이후에 휴면상태로 들어가느냐의 값 (비용 절감 목적)
![[Pasted image 20230417202927.png]]
- 나머지는 그대로 두고 `Create` 클릭
- 최종 명세를 확인하고 이상이 없을 경우 `Create environment` 클릭


### Cloud9에 패키지 설치
- Cloud9에 연결 후 새로운 터미널 오픈
- AWS CLI