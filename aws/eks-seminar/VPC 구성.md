
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
---
AWSTemplateFormatVersion: '2010-09-09'
Description: 'Amazon EKS Sample VPC - 3 AZ, Private 3 subnets, Public 3 subnets, 1 IGW, 3 NATGateways, Public RT, 3 Private RT, Security Group for ControlPlane '

Metadata:
  AWS::CloudFormation::Interface:
    ParameterGroups:
      -
        Label:
          default: "Worker Network Configuration"
        Parameters:
          - VpcBlock
          - AvailabilityZoneA
          - AvailabilityZoneB
          - AvailabilityZoneC
          - PublicSubnet01Block
          - PublicSubnet02Block
          - PublicSubnet03Block
          - PrivateSubnet01Block
          - PrivateSubnet02Block
          - PrivateSubnet03Block
          - TGWSubnet01Block
          - TGWSubnet02Block
          - TGWSubnet03Block

Parameters:

  VpcBlock:
    Type: String
    Default: 10.11.0.0/16
    Description: The CIDR range for the VPC. This should be a valid private (RFC 1918) CIDR range.

  AvailabilityZoneA:
    Description: "Choose AZ1 for your VPC."
    Type: AWS::EC2::AvailabilityZone::Name
    Default: "ap-northeast-2a"

  AvailabilityZoneB:
    Description: "Choose AZ2 for your VPC."
    Type: AWS::EC2::AvailabilityZone::Name
    Default: "ap-northeast-2b"

  AvailabilityZoneC:
    Description: "Choose AZ1 for your VPC."
    Type: AWS::EC2::AvailabilityZone::Name
    Default: "ap-northeast-2c"

  PublicSubnet01Block:
    Type: String
    Default: 10.11.0.0/20
    Description: CidrBlock for public subnet 01 within the VPC  
  
  PublicSubnet02Block:
    Type: String
    Default: 10.11.16.0/20
    Description: CidrBlock for public subnet 02 within the VPC

  PublicSubnet03Block:
    Type: String
    Default: 10.11.32.0/20
    Description: CidrBlock for public subnet 03 within the VPC

  PrivateSubnet01Block:
    Type: String
    Default: 10.11.48.0/20
    Description: CidrBlock for private subnet 01 within the VPC

  PrivateSubnet02Block:
    Type: String
    Default: 10.11.64.0/20
    Description: CidrBlock for private subnet 02 within the VPC
  
  PrivateSubnet03Block:
    Type: String
    Default: 10.11.80.0/20
    Description: CidrBlock for private subnet 03 within the VPC

  TGWSubnet01Block:
    Type: String
    Default: 10.11.251.0/24
    Description: CidrBlock for TGW subnet 01 within the VPC

  TGWSubnet02Block:
    Type: String
    Default: 10.11.252.0/24
    Description: CidrBlock for TGW subnet 02 within the VPC
  
  TGWSubnet03Block:
    Type: String
    Default: 10.11.253.0/24
    Description: CidrBlock for TGW subnet 03 within the VPC

Resources:

#####################
# Create-VPC : VPC #
#####################

  VPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock:  !Ref VpcBlock
      EnableDnsSupport: true
      EnableDnsHostnames: true
      Tags:
      - Key: Name
        Value: !Sub '${AWS::StackName}'

########################################################
# Create-InternetGateway: 
########################################################

  InternetGateway:
    Type: "AWS::EC2::InternetGateway"

########################################################
# Attach - VPC Gateway 
########################################################

  VPCGatewayAttachment:
    Type: "AWS::EC2::VPCGatewayAttachment"
    Properties:
      InternetGatewayId: !Ref InternetGateway
      VpcId: !Ref VPC

########################################################
# Create-Public-Subnet: PublicSubnet01,02,03,04
########################################################

  PublicSubnet01:
    Type: AWS::EC2::Subnet
    Metadata:
      Comment: Public Subnet 01
    Properties:
      VpcId: !Ref VPC
      CidrBlock: !Ref PublicSubnet01Block
      AvailabilityZone: !Ref AvailabilityZoneA
      MapPublicIpOnLaunch: "true"
      Tags:
      - Key: Name
        Value: !Sub "${AWS::StackName}-PublicSubnet01"
      - Key: kubernetes.io/role/elb
        Value: 1

  PublicSubnet02:
    Type: AWS::EC2::Subnet
    Metadata:
      Comment: Public Subnet 02
    Properties:
      VpcId: !Ref VPC
      CidrBlock: !Ref PublicSubnet02Block
      AvailabilityZone: !Ref AvailabilityZoneB
      MapPublicIpOnLaunch: "true"
      Tags:
      - Key: Name
        Value: !Sub "${AWS::StackName}-PublicSubnet02"
      - Key: kubernetes.io/role/elb
        Value: 1

  PublicSubnet03:
    Type: AWS::EC2::Subnet
    Metadata:
      Comment: Public Subnet 03
    Properties:
      VpcId: !Ref VPC
      CidrBlock: !Ref PublicSubnet03Block
      AvailabilityZone: !Ref AvailabilityZoneC
      MapPublicIpOnLaunch: "true"
      Tags:
      - Key: Name
        Value: !Sub "${AWS::StackName}-PublicSubnet03"
      - Key: kubernetes.io/role/elb
        Value: 1

#####################################################################
# Create-Public-RouteTable:
#####################################################################

  PublicRouteTable:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId: !Ref VPC
      Tags:
      - Key: Name
        Value: Public Subnets
      - Key: Network
        Value: PublicRT

################################################################################################
# Associate-Public-RouteTable: VPC_Private_Subnet_a,b Accsociate VPC_Private_RouteTable #
################################################################################################
  PublicSubnet01RouteTableAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref PublicSubnet01
      RouteTableId: !Ref PublicRouteTable

  PublicSubnet02RouteTableAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref PublicSubnet02
      RouteTableId: !Ref PublicRouteTable

  PublicSubnet03RouteTableAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref PublicSubnet03
      RouteTableId: !Ref PublicRouteTable

################################################################################################
# Create Public Routing Table
################################################################################################
  PublicRoute:
    DependsOn: VPCGatewayAttachment
    Type: AWS::EC2::Route
    Properties:
      RouteTableId: !Ref PublicRouteTable
      DestinationCidrBlock: 0.0.0.0/0
      GatewayId: !Ref InternetGateway

################################################################################################
# Create-NATGateway: NATGATEWAY01,02,03
################################################################################################
  NatGateway01:
    DependsOn:
    - NatGatewayEIP1
    - PublicSubnet01
    - VPCGatewayAttachment
    Type: AWS::EC2::NatGateway
    Properties:
      AllocationId: !GetAtt 'NatGatewayEIP1.AllocationId'
      SubnetId: !Ref PublicSubnet01
      Tags:
      - Key: Name
        Value: !Sub '${AWS::StackName}-NatGatewayAZ1'

  NatGateway02:
    DependsOn:
    - NatGatewayEIP2
    - PublicSubnet02
    - VPCGatewayAttachment
    Type: AWS::EC2::NatGateway
    Properties:
      AllocationId: !GetAtt 'NatGatewayEIP2.AllocationId'
      SubnetId: !Ref PublicSubnet02
      Tags:
      - Key: Name
        Value: !Sub '${AWS::StackName}-NatGatewayAZ2'

  NatGateway03:
    DependsOn:
    - NatGatewayEIP3
    - PublicSubnet03
    - VPCGatewayAttachment
    Type: AWS::EC2::NatGateway
    Properties:
      AllocationId: !GetAtt 'NatGatewayEIP3.AllocationId'
      SubnetId: !Ref PublicSubnet03
      Tags:
      - Key: Name
        Value: !Sub '${AWS::StackName}-NatGatewayAZ3'

  NatGatewayEIP1:
    DependsOn:
    - VPCGatewayAttachment
    Type: 'AWS::EC2::EIP'
    Properties:
      Domain: vpc

  NatGatewayEIP2:
    DependsOn:
    - VPCGatewayAttachment
    Type: 'AWS::EC2::EIP'
    Properties:
      Domain: vpc

  NatGatewayEIP3:
    DependsOn:
    - VPCGatewayAttachment
    Type: 'AWS::EC2::EIP'
    Properties:
      Domain: vpc

########################################################
# Create-Security-Group : ControlPlane
########################################################
  ControlPlaneSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Cluster communication with worker nodes
      VpcId: !Ref VPC

########################################################
# Create-Security-Group : Session Manager
########################################################
  SSMSG:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Open-up ports for HTTP/S from All network
      GroupName: SSMSG
      VpcId: !Ref VPC
      SecurityGroupIngress:
        - IpProtocol: tcp
          CidrIp: 0.0.0.0/0
          FromPort: "80"
          ToPort: "80"
        - IpProtocol: tcp
          CidrIp: 0.0.0.0/0
          FromPort: "443"
          ToPort: "443"
      Tags:
        - Key: Name
          Value: !Sub '${AWS::StackName}-SSMSG'
########################################################
# Create-Private-Subnet: PrivateSubnet01,02
########################################################

  PrivateSubnet01:
    Type: AWS::EC2::Subnet
    Metadata:
      Comment: Private Subnet 01
    Properties:
      VpcId: !Ref VPC
      CidrBlock: !Ref PrivateSubnet01Block
      AvailabilityZone: !Ref AvailabilityZoneA
      Tags:
      - Key: Name
        Value: !Sub "${AWS::StackName}-PrivateSubnet01"
      - Key: kubernetes.io/role/internal-elb
        Value: 1

  PrivateSubnet02:
    Type: AWS::EC2::Subnet
    Metadata:
      Comment: Private Subnet 02
    Properties:
      VpcId: !Ref VPC
      CidrBlock: !Ref PrivateSubnet02Block
      AvailabilityZone: !Ref AvailabilityZoneB
      Tags:
      - Key: Name
        Value: !Sub "${AWS::StackName}-PrivateSubnet02"
      - Key: kubernetes.io/role/internal-elb
        Value: 1

  PrivateSubnet03:
    Type: AWS::EC2::Subnet
    Metadata:
      Comment: Private Subnet 03
    Properties:
      VpcId: !Ref VPC
      CidrBlock: !Ref PrivateSubnet03Block
      AvailabilityZone: !Ref AvailabilityZoneC
      Tags:
      - Key: Name
        Value: !Sub "${AWS::StackName}-PrivateSubnet03"
      - Key: kubernetes.io/role/internal-elb
        Value: 1

#####################################################################
# Create-Private-RouteTable: PrivateRT01,02
#####################################################################
  PrivateRouteTable01:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId: !Ref VPC
      Tags:
      - Key: Name
        Value: Private Subnet AZ1
      - Key: Network
        Value: PrivateRT01

  PrivateRouteTable02:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId: !Ref VPC
      Tags:
      - Key: Name
        Value: Private Subnet AZ2
      - Key: Network
        Value: PrivateRT02

  PrivateRouteTable03:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId: !Ref VPC
      Tags:
      - Key: Name
        Value: Private Subnet AZ3
      - Key: Network
        Value: PrivateRT03

       
################################################################################################
# Associate-Private-RouteTable: VPC_Private_Subnet_a,b Accsociate VPC_Private_RouteTable #
################################################################################################

  PrivateSubnet01RouteTableAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref PrivateSubnet01
      RouteTableId: !Ref PrivateRouteTable01

  PrivateSubnet02RouteTableAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref PrivateSubnet02
      RouteTableId: !Ref PrivateRouteTable02

  PrivateSubnet03RouteTableAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref PrivateSubnet03
      RouteTableId: !Ref PrivateRouteTable03

################################################################################################
# Add Prviate Routing Table
################################################################################################

  PrivateRoute01:
    DependsOn:
    - VPCGatewayAttachment
    - NatGateway01
    Type: AWS::EC2::Route
    Properties:
      RouteTableId: !Ref PrivateRouteTable01
      DestinationCidrBlock: 0.0.0.0/0
      NatGatewayId: !Ref NatGateway01

  PrivateRoute02:
    DependsOn:
    - VPCGatewayAttachment
    - NatGateway02
    Type: AWS::EC2::Route
    Properties:
      RouteTableId: !Ref PrivateRouteTable02
      DestinationCidrBlock: 0.0.0.0/0
      NatGatewayId: !Ref NatGateway02
  
  PrivateRoute03:
    DependsOn:
    - VPCGatewayAttachment
    - NatGateway03
    Type: AWS::EC2::Route
    Properties:
      RouteTableId: !Ref PrivateRouteTable03
      DestinationCidrBlock: 0.0.0.0/0
      NatGatewayId: !Ref NatGateway03
########################################################
# Create-TGW-Subnet: TGWSubnet01,02,03
########################################################

  TGWSubnet01:
    Type: AWS::EC2::Subnet
    Metadata:
      Comment: TGW Subnet 01
    Properties:
      VpcId: !Ref VPC
      CidrBlock: !Ref TGWSubnet01Block
      AvailabilityZone: !Ref AvailabilityZoneA
      Tags:
      - Key: Name
        Value: !Sub "${AWS::StackName}-TGWSubnet01"
      - Key: kubernetes.io/role/internal-elb
        Value: 1

  TGWSubnet02:
    Type: AWS::EC2::Subnet
    Metadata:
      Comment: TGW Subnet 02
    Properties:
      VpcId: !Ref VPC
      CidrBlock: !Ref TGWSubnet02Block
      AvailabilityZone: !Ref AvailabilityZoneB
      Tags:
      - Key: Name
        Value: !Sub "${AWS::StackName}-TGWSubnet02"
      - Key: kubernetes.io/role/internal-elb
        Value: 1

  TGWSubnet03:
    Type: AWS::EC2::Subnet
    Metadata:
      Comment: TGW Subnet 03
    Properties:
      VpcId: !Ref VPC
      CidrBlock: !Ref TGWSubnet03Block
      AvailabilityZone: !Ref AvailabilityZoneC
      Tags:
      - Key: Name
        Value: !Sub "${AWS::StackName}-TGWSubnet03"
      - Key: kubernetes.io/role/internal-elb
        Value: 1

#####################################################################
# Create-TGW-RouteTable: TGWRT01,02,03,04
#####################################################################
  TGWRouteTable01:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId: !Ref VPC
      Tags:
      - Key: Name
        Value: TGW Subnet AZ1
      - Key: Network
        Value: TGWRT01

  TGWRouteTable02:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId: !Ref VPC
      Tags:
      - Key: Name
        Value: TGW Subnet AZ2
      - Key: Network
        Value: TGWRT02

  TGWRouteTable03:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId: !Ref VPC
      Tags:
      - Key: Name
        Value: TGW Subnet AZ3
      - Key: Network
        Value: TGWRT03

       
################################################################################################
# Associate-TGW-RouteTable: VPC_TGW_Subnet_a,b Accsociate VPC_TGW_RouteTable #
################################################################################################

  TGWSubnet01RouteTableAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref TGWSubnet01
      RouteTableId: !Ref TGWRouteTable01

  TGWSubnet02RouteTableAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref TGWSubnet02
      RouteTableId: !Ref TGWRouteTable02

  TGWSubnet03RouteTableAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref TGWSubnet03
      RouteTableId: !Ref TGWRouteTable03

######################################################################
# Create-System-Manager-Endpoint: Create VPC SystemManager Endpoint #
######################################################################

  SSMEndpoint:
    Type: AWS::EC2::VPCEndpoint
    Properties:
      VpcId: !Ref VPC
      ServiceName: !Sub "com.amazonaws.${AWS::Region}.ssm"
      VpcEndpointType: Interface
      PrivateDnsEnabled: True
      SubnetIds: 
        - Ref: PrivateSubnet01
        - Ref: PrivateSubnet02
        - Ref: PrivateSubnet03
      SecurityGroupIds:
        - Ref: SSMSG

  SSMMEndpoint:
    Type: AWS::EC2::VPCEndpoint
    Properties:
      VpcId: !Ref VPC
      ServiceName: !Sub "com.amazonaws.${AWS::Region}.ssmmessages"
      VpcEndpointType: Interface
      PrivateDnsEnabled: True
      SubnetIds: 
        - Ref: PrivateSubnet01
        - Ref: PrivateSubnet02
        - Ref: PrivateSubnet03
      SecurityGroupIds:
        - Ref: SSMSG

Outputs:

  VpcId:
    Description: The VPC Id
    Value: !Ref VPC

  PublicSubnet01:
    Description: PublicSubnet01 ID in the VPC
    Value: !Ref PublicSubnet01

  PublicSubnet02:
    Description: PublicSubnet02 ID in the VPC
    Value: !Ref PublicSubnet02

  PublicSubnet03:
    Description: PublicSubnet03 ID in the VPC
    Value: !Ref PublicSubnet03

  PrivateSubnet01:
    Description: PrivateSubnet01 ID in the VPC
    Value: !Ref PrivateSubnet01

  PrivateSubnet02:
    Description: PrivateSubnet02 ID in the VPC
    Value: !Ref PrivateSubnet02

  PrivateSubnet03:
    Description: PrivateSubnet03 ID in the VPC
    Value: !Ref PrivateSubnet03

  SecurityGroups:
    Description: Security group for the cluster control plane communication with worker nodes
    Value: !Join [ ",", [ !Ref ControlPlaneSecurityGroup ] ]

  TGWSubnet01:
    Description: TGWSubnet01 ID in the VPC
    Value: !Ref TGWSubnet01

  TGWSubnet02:
    Description: TGWSubnet02 ID in the VPC
    Value: !Ref TGWSubnet02

  TGWSubnet03:
    Description: TGWSubnet03 ID in the VPC
    Value: !Ref TGWSubnet03
```


#### 2. Stack 생성
```sh
aws cloudformation deploy \
--stack-name "eksworkshop" \
--template-file "EKSVPC3AZ.yml" \
--capabilities CAPABILITY_NAMED_IAM
```


#### 3. Stack 완료 확인
- AWS Console - Cloudformation - 스택
![[Pasted image 20230419085052.png]]

#### 4. Output 정보 확인
- eksctl을 사용해서 EKS Cluster를 사용할 텐데 이 때 꼭 필요한 것이 VPC id, Subnet id
- subnet id는 public subnet 01,02,03, private subnet 01,02,03으로 출력
![[Pasted image 20230419085253.png]]
- 완료되면 다음과 같은 VPC가 구성
- 아직 Worker Node EC2는 미생성
![[Pasted image 20230419085339.png]]
