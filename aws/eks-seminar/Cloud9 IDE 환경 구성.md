
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
- AWS CLI를 최신으로 업그레이드
```sh
# AWS CLI Upgrade
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
source ~/.bashrc
aws --version        # 버전 확인
```
- AWS CLI 자동완성 설치
```sh
# AWS CLI 자동완성 설치
which aws_completer
export PATH=/usr/local/bin:$PATH
source ~/.bash_profile
complete -C '/usr/local/bin/aws_completer' aws
```


### Kubectl 설치
- EKS를 위한 `kubectl`바이너리를 다운로드
- Kubernetes 버전 1.23출시부터 공식적으로 Amazon EKS AMI에는 containerd가 유일한 런타임으로 포함
- Kubernetes 버전 1.18-1.21은 Docker를 기본 런타임으로 사용
- `kubectl`바이너리 버전은 1.22.6 설치
- 추가로 바이너리에 실행권한을 적용, 자동완성을 설치
```sh
cd ~
curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.22.6/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc
kubectl version --short --client
```


### 기타 유틸리티 설치
- GNU gettext, jq, bash 자동완성등을 설치
```sh
sudo yum -y install jq gettext bash-completion moreutils
for command in kubectl jq envsubst aws
  do
    which $command &>/dev/null && echo "$command in path" || echo "$command NOT FOUND"
  done
```
- k9s는 쿠버네티스 클러스터와 상호작용을 통해 직관적인 UI 터미널을 제공
- 참조 : https://github.com/derailed/k9s
```sh
K9S_VERSION=v0.26.7
curl -sL https://github.com/derailed/k9s/releases/download/${K9S_VERSION}/k9s_Linux_x86_64.tar.gz | sudo tar xfz - -C /usr/local/bin 
```
- 실행은 `k9s`를 입력하고 엔터
