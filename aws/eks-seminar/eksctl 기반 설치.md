
### 소개
- `eksctl`은 관리형 Kubernetes 서비스인 EKS에서 클러스터를 생성하기 위한 간단한 CLI 도구
- Go로 작성, Cloudformation을 사용하고 Weaveworks가 작성했으며 하나의 명령으로 몇 분안에 기본 클러스터를 만듦
- EKS를 구성하기 위한 도구이며, EKS UI, CDK, Terraform, Rancher 등 다양한 도구로도 구성 가능

### eksctl을 통한 EKS 구성

#### 1. eksctl 설치
```sh
# eksctl 설정
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin

# eksctl 자동완성 - bash
. <(eksctl completion bash)
eksctl version
```

#### 2. VPC/Subnet 정보 확인
- Cloudformation에서 생성한 VPC 자원들에 대한 고유 자원 값을 추출해서, Cloud9내 환경 변수에 저장
```sh
#!/bin/bash
# command ./eks_shell.sh

cd ~/environment/
#VPC ID export
export vpc_ID=$(aws ec2 describe-vpcs --filters Name=tag:Name,Values=eksworkshop | jq -r '.Vpcs[].VpcId')
echo $vpc_ID

#Subnet ID, CIDR, Subnet Name export
aws ec2 describe-subnets --filter Name=vpc-id,Values=$vpc_ID | jq -r '.Subnets[]|.SubnetId+" "+.CidrBlock+" "+(.Tags[]|select(.Key=="Name").Value)'
echo $vpc_ID > vpc_subnet.txt
aws ec2 describe-subnets --filter Name=vpc-id,Values=$vpc_ID | jq -r '.Subnets[]|.SubnetId+" "+.CidrBlock+" "+(.Tags[]|select(.Key=="Name").Value)' >> vpc_subnet.txt
cat vpc_subnet.txt

# VPC, Subnet ID 환경변수 저장 
export PublicSubnet01=$(aws ec2 describe-subnets --filters 'Name=tag:Name,Values=eksworkshop-PublicSubnet01' | jq -r '.Subnets[].SubnetId')
export PublicSubnet02=$(aws ec2 describe-subnets --filters 'Name=tag:Name,Values=eksworkshop-PublicSubnet02' | jq -r '.Subnets[].SubnetId')
export PublicSubnet03=$(aws ec2 describe-subnets --filters 'Name=tag:Name,Values=eksworkshop-PublicSubnet03' | jq -r '.Subnets[].SubnetId')
export PrivateSubnet01=$(aws ec2 describe-subnets --filters 'Name=tag:Name,Values=eksworkshop-PrivateSubnet01' | jq -r '.Subnets[].SubnetId')
export PrivateSubnet02=$(aws ec2 describe-subnets --filters 'Name=tag:Name,Values=eksworkshop-PrivateSubnet02' | jq -r '.Subnets[].SubnetId')
export PrivateSubnet03=$(aws ec2 describe-subnets --filters 'Name=tag:Name,Values=eksworkshop-PrivateSubnet03' | jq -r '.Subnets[].SubnetId')
echo "export vpc_ID=${vpc_ID}" | tee -a ~/.bash_profile
echo "export PublicSubnet01=${PublicSubnet01}" | tee -a ~/.bash_profile
echo "export PublicSubnet02=${PublicSubnet02}" | tee -a ~/.bash_profile
echo "export PublicSubnet03=${PublicSubnet03}" | tee -a ~/.bash_profile
echo "export PrivateSubnet01=${PrivateSubnet01}" | tee -a ~/.bash_profile
echo "export PrivateSubnet02=${PrivateSubnet02}" | tee -a ~/.bash_profile
echo "export PrivateSubnet03=${PrivateSubnet03}" | tee -a ~/.bash_profile

# eks cluster 환경변수 생성 
export ekscluster_name="eksworkshop"
export eks_version="1.22"
export instance_type="m5.xlarge"
export public_selfmgmd_node="frontend-workloads"
export private_selfmgmd_node="backend-workloads"
export public_mgmd_node="managed-frontend-workloads"
export private_mgmd_node="managed-backend-workloads"
export publicKeyPath="/home/ec2-user/environment/eksworkshop.pub"

echo ${ekscluster_name}
echo ${AWS_REGION}
echo ${eks_version}
echo ${PublicSubnet01}
echo ${PublicSubnet02}
echo ${PublicSubnet03}
echo ${PrivateSubnet01}
echo ${PrivateSubnet02}
echo ${PrivateSubnet03}
echo ${MASTER_ARN}
echo ${instance_type}
echo ${public_selfmgmd_node}
echo ${private_selfmgmd_node}
echo ${public_mgmd_node}
echo ${private_mgmd_node}
echo ${publicKeyPath}

# ekscluster name, version, instance type, nodegroup label 환경변수 저장.  
echo "export ekscluster_name=${ekscluster_name}" | tee -a ~/.bash_profile
echo "export eks_version=${eks_version}" | tee -a ~/.bash_profile
echo "export instance_type=${instance_type}" | tee -a ~/.bash_profile
echo "export public_selfmgmd_node=${public_selfmgmd_node}" | tee -a ~/.bash_profile
echo "export private_selfmgmd_node=${private_selfmgmd_node}" | tee -a ~/.bash_profile
echo "export public_mgmd_node=${public_mgmd_node}" | tee -a ~/.bash_profile
echo "export private_mgmd_node=${private_mgmd_node}" | tee -a ~/.bash_profile
echo "export publicKeyPath=${publicKeyPath}" | tee -a ~/.bash_profile

source ~/.bash_profile
```

#### 3. eksctl 배포 yaml 수정
- 아래 shell을 실행하면 eksworkshop.yaml 파일이 자동 생성
- 해당 파일은 eksctl에서 eks cluster 구성을 위한 manifest파일로 사용됨
- vpc/subnet id, KMS CMK keyARN등이 다를 경우 설치 에러가 발생
- Cloud9의 publickeyPath 경로도 확인해야 함
```sh
#!/bin/bash
# command ./eksctl_shell.sh
# eksctl yaml 실행

source ~/.bash_profile
cat << EOF > ~/environment/myeks/eksworkshop.yaml
---
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: ${ekscluster_name}
  region: ${AWS_REGION}
  version: "${eks_version}"  
vpc: 
  id: ${vpc_ID}
  subnets:
    public:
      PublicSubnet01:
        az: ${AWS_REGION}a
        id: ${PublicSubnet01}
      PublicSubnet02:
        az: ${AWS_REGION}b
        id: ${PublicSubnet02}
      PublicSubnet03:
        az: ${AWS_REGION}c
        id: ${PublicSubnet03}
    private:
      PrivateSubnet01:
        az: ${AWS_REGION}a
        id: ${PrivateSubnet01}
      PrivateSubnet02:
        az: ${AWS_REGION}b
        id: ${PrivateSubnet02}
      PrivateSubnet03:
        az: ${AWS_REGION}c
        id: ${PrivateSubnet03}
secretsEncryption:
  keyARN: ${MASTER_ARN}

nodeGroups:
  - name: ng-public-01
    instanceType: ${instance_type}
    subnets:
      - ${PublicSubnet01}
      - ${PublicSubnet02}
      - ${PublicSubnet03}
    desiredCapacity: 3
    minSize: 3
    maxSize: 6
    volumeSize: 200
    volumeType: gp3
    volumeEncrypted: true
    amiFamily: AmazonLinux2
    labels:
      nodegroup-type: "${public_selfmgmd_node}"
    ssh: 
      publicKeyPath: "${publicKeyPath}"
    iam:
      attachPolicyARNs:
      withAddonPolicies:
        autoScaler: true
        cloudWatch: true
        ebs: true
        fsx: true
        efs: true

  - name: ng-private-01
    instanceType: ${instance_type}
    subnets:
      - ${PrivateSubnet01}
      - ${PrivateSubnet02}
      - ${PrivateSubnet03}
    desiredCapacity: 3
    privateNetworking: true
    minSize: 3
    maxSize: 9
    volumeSize: 200
    volumeType: gp3
    volumeEncrypted: true
    amiFamily: AmazonLinux2
    labels:
      nodegroup-type: "${private_selfmgmd_node}"
    ssh: 
      publicKeyPath: "${publicKeyPath}"
    iam:
      attachPolicyARNs:
      withAddonPolicies:
        autoScaler: true
        cloudWatch: true
        ebs: true
        fsx: true
        efs: true

managedNodeGroups:
  - name: managed-ng-public-01
    instanceType: ${instance_type}
    subnets:
      - ${PublicSubnet01}
      - ${PublicSubnet02}
      - ${PublicSubnet03}
    desiredCapacity: 3
    minSize: 3
    maxSize: 6
    volumeSize: 200
    volumeType: gp3
    volumeEncrypted: true
    amiFamily: AmazonLinux2
    labels:
      nodegroup-type: "${public_mgmd_node}"
    ssh: 
      publicKeyPath: "${publicKeyPath}"
    iam:
      attachPolicyARNs:
      withAddonPolicies:
        autoScaler: true
        cloudWatch: true
        ebs: true
        fsx: true
        efs: true
        
  - name: managed-ng-private-01
    instanceType: ${instance_type}
    subnets:
      - ${PrivateSubnet01}
      - ${PrivateSubnet02}
      - ${PrivateSubnet03}
    desiredCapacity: 3
    privateNetworking: true
    minSize: 3
    maxSize: 9
    volumeSize: 200
    volumeType: gp3
    volumeEncrypted: true
    amiFamily: AmazonLinux2
    labels:
      nodegroup-type: "${private_mgmd_node}"
    ssh: 
      publicKeyPath: "${publicKeyPath}"
    iam:
      attachPolicyARNs:
      withAddonPolicies:
        autoScaler: true
        cloudWatch: true
        ebs: true
        fsx: true
        efs: true
        
cloudWatch:
    clusterLogging:
        enableTypes: ["api", "audit", "authenticator", "controllerManager", "scheduler"]
EOF
```
- 생성된 eksctl yaml 파일을 `dry-run`을 실행시켜 확인
```sh
eksctl create cluster --config-file=/home/ec2-user/environment/myeks/eksworkshop.yaml --dry-run
```

#### 4. cluster 생성
- eksctl을 통해 EKS Cluster를 생성
```sh
# eksctl로 cluster 만들기 
eksctl create cluster --config-file=/home/ec2-user/environment/myeks/eksworkshop.yaml
```

#### 5.  Cluster 생성 확인
```sh
kubectl get nodes
```
```
$ kubectl get nodes
NAME STATUS ROLES AGE VERSION
ip-10-11-1-225.ap-northeast-2.compute.internal Ready <none> 2m16s v1.22.15-eks-fb459a0
ip-10-11-15-121.ap-northeast-2.compute.internal Ready <none> 5m26s v1.22.15-eks-fb459a0
ip-10-11-27-197.ap-northeast-2.compute.internal Ready <none> 2m17s v1.22.15-eks-fb459a0
ip-10-11-27-216.ap-northeast-2.compute.internal Ready <none> 5m14s v1.22.15-eks-fb459a0
ip-10-11-33-12.ap-northeast-2.compute.internal Ready <none> 2m17s v1.22.15-eks-fb459a0
ip-10-11-47-74.ap-northeast-2.compute.internal Ready <none> 5m25s v1.22.15-eks-fb459a0
ip-10-11-50-128.ap-northeast-2.compute.internal Ready <none> 83s v1.22.15-eks-fb459a0
ip-10-11-59-180.ap-northeast-2.compute.internal Ready <none> 5m34s v1.22.15-eks-fb459a0
ip-10-11-69-123.ap-northeast-2.compute.internal Ready <none> 5m23s v1.22.15-eks-fb459a0
ip-10-11-74-5.ap-northeast-2.compute.internal Ready <none> 84s v1.22.15-eks-fb459a0
ip-10-11-80-166.ap-northeast-2.compute.internal Ready <none> 79s v1.22.15-eks-fb459a0
ip-10-11-90-157.ap-northeast-2.compute.internal Ready <none> 5m30s v1.22.15-eks-fb459a0
```
- VPC, Subnet, Internet Gateway, NAT Gateway, Route Table등을 확인
- EC2 Worker Node들도 확인
- EKS와 eksctl을 통해 생성된 Cloudformation도 확인
- 다음과 같은 구성도 완성
![[Pasted image 20230419094302.png]]

#### 6. EKS 구성 확인
- EKS 콘솔을 통해 생성된 EKS Cluster를 확인할 수 있음
- 유저 권한이 없을 경우 Cloud9에서 추가

##### configmap 인증 정보 수정
- Cloud9 터미널에 `kubectl`명령을 통해 aws-auth 파일을 확인
```sh
kubectl get configmap -n kube-system aws-auth -o yaml
```
```
apiVersion: v1
data:
  mapRoles: |
    - groups:
      - system:bootstrappers
      - system:nodes
      rolearn: arn:aws:iam::277828472567:role/eksctl-eksworkshop-nodegroup-mana-NodeInstanceRole-KBNN6VOZRBON
      username: system:node:{{EC2PrivateDNSName}}
    - groups:
      - system:bootstrappers
      - system:nodes
      rolearn: arn:aws:iam::277828472567:role/eksctl-eksworkshop-nodegroup-mana-NodeInstanceRole-8HVX716IUG6H
      username: system:node:{{EC2PrivateDNSName}}
    - groups:
      - system:bootstrappers
      - system:nodes
      rolearn: arn:aws:iam::277828472567:role/eksctl-eksworkshop-nodegroup-ng-p-NodeInstanceRole-1GZXTMDGOWYF2
      username: system:node:{{EC2PrivateDNSName}}
    - groups:
      - system:bootstrappers
      - system:nodes
      rolearn: arn:aws:iam::277828472567:role/eksctl-eksworkshop-nodegroup-ng-p-NodeInstanceRole-1IJ72XYIRPHTK
      username: system:node:{{EC2PrivateDNSName}}
  mapUsers: []
kind: ConfigMap
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","data":{"mapRoles":"- groups:\n  - system:bootstrappers\n  - system:nodes\n  rolearn: arn:aws:iam::277828472567:role/eksctl-eksworkshop-nodegroup-mana-NodeInstanceRole-KBNN6VOZRBON\n  username: system:node:{{EC2PrivateDNSName}}\n- groups:\n  - system:bootstrappers\n  - system:nodes\n  rolearn: arn:aws:iam::277828472567:role/eksctl-eksworkshop-nodegroup-mana-NodeInstanceRole-8HVX716IUG6H\n  username: system:node:{{EC2PrivateDNSName}}\n- groups:\n  - system:bootstrappers\n  - system:nodes\n  rolearn: arn:aws:iam::277828472567:role/eksctl-eksworkshop-nodegroup-ng-p-NodeInstanceRole-1GZXTMDGOWYF2\n  username: system:node:{{EC2PrivateDNSName}}\n- groups:\n  - system:bootstrappers\n  - system:nodes\n  rolearn: arn:aws:iam::277828472567:role/eksctl-eksworkshop-nodegroup-ng-p-NodeInstanceRole-1IJ72XYIRPHTK\n  username: system:node:{{EC2PrivateDNSName}}\n","mapUsers":"- userarn: arn:aws:iam::277828472567:user/juho\n  username: juho\n  groups:\n    - system:masters\n"},"kind":"ConfigMap","metadata":{"annotations":{},"name":"aws-auth","namespace":"kube-system"}}
  creationTimestamp: "2023-04-17T02:47:01Z"
  name: aws-auth
  namespace: kube-system
  resourceVersion: "30651"
  uid: 44c04fd5-4726-4048-a555-69cbcc1a05d6
```

- aws-auth.yaml파일을 아래 디렉토리에 생성
```sh
kubectl get configmap -n kube-system aws-auth -o yaml | grep -v "creationTimestamp\|resourceVersion\|selfLink\|uid" | sed '/^  annotations:/,+2 d' > ~/environment/aws-auth.yaml
cp ~/environment/aws-auth.yaml ~/environment/aws-auth_backup.yaml
```
- ~/environment/aws-auth.yaml파일을 열고 아래 값을 입력
```sh
  mapUsers: |
    - userarn: arn:aws:iam::277828472567:user/juho
      username: juho
      groups:
        - system:masters
```
- user arn값은 account id, 이미 Shell 환경변수에 저장해 둠
```
echo $ACCOUNT_ID
```
- 아래 새로운 사용자 권한을 mapRoles뒤에 추가
```sh
apiVersion: v1
data:
  mapRoles: |
    - groups:
      - system:bootstrappers
      - system:nodes
      rolearn: arn:aws:iam::277828472567:role/eksctl-eksworkshop-nodegroup-mana-NodeInstanceRole-KBNN6VOZRBON
      username: system:node:{{EC2PrivateDNSName}}
    - groups:
      - system:bootstrappers
      - system:nodes
      rolearn: arn:aws:iam::277828472567:role/eksctl-eksworkshop-nodegroup-mana-NodeInstanceRole-8HVX716IUG6H
      username: system:node:{{EC2PrivateDNSName}}
    - groups:
      - system:bootstrappers
      - system:nodes
      rolearn: arn:aws:iam::277828472567:role/eksctl-eksworkshop-nodegroup-ng-p-NodeInstanceRole-1GZXTMDGOWYF2
      username: system:node:{{EC2PrivateDNSName}}
    - groups:
      - system:bootstrappers
      - system:nodes
      rolearn: arn:aws:iam::277828472567:role/eksctl-eksworkshop-nodegroup-ng-p-NodeInstanceRole-1IJ72XYIRPHTK
      username: system:node:{{EC2PrivateDNSName}}
  mapUsers: |
    - userarn: arn:aws:iam::277828472567:user/juho
      username: juho
      groups:
        - system:masters
kind: ConfigMap
.....
```

##### EKS Cluster 결과 확인
- 콘솔에서 생성한 클러스터 선택
- 컴퓨팅탭을 선택하고 생성된 WokerNode들을 확인
![[Pasted image 20230419095243.png]]
- Kubernetes의 Resource들을 선택하고 확인
![[Pasted image 20230419095341.png]]
- `kubectl`명령을 통해서도 확인 가능
```sh
#kube-system namespace에 생성된 자원 확인 
kubectl -n kube-system get all

#주요 Pod의 상세 정보 확인 
kubectl -n kube-system pods <pod-name> -o wide

# node 상세 정보 확인 
kubectl get nodes -o wide
```
