# 1. IAM

> **인증 및 권한 관리 서비스 (Identity and Access Management)**


- 사용자를 생성하고, 그룹에 배치하는 **글로벌 서비스**
- Root account가 기본적으로 생성되며, 생성 후에는 루트 계정을 더 이상 사용 및 공유 X
- 구성 요소 : Users, Groups, Roles, Policies

## 1. User

- 하나의 사용자는 조직 내의 한 사람(= AWS 계정 하나)에 해당하며, 사용자들을 그룹으로 묶을 수 있음
- 사용자는 여러 그룹에 속할 수 있으며, 사용자 그룹에 속할 필요도 없음
- IAM Policy가 인라인으로 사용자에게 직접 연결될 수 있음 (굳이 그룹에 속하지 않더라도!)

## 2. Group

- 다수의 IAM 사용자를 모아둔 것
- 사용자를 그룹에 넣는 이유는 **여러 사람에게 동일한 액세스 권한 부여 및 관리**하기 위해서
- 그룹에는 사용자만 배치 가능 (contains users only)

## 3. Policy

- 사용자, 서비스에 대한 **권한 부여**
- 사용자 또는 그룹에게 **Json 문서**로 이루어진 정책(권한)을 지정할 수 있음

```python
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:Describe*",
            "Resource": "*",
        }, 
      	{
            "Effect": "Allow",
            "Action": "elasticloadbalancing:Describe*",
            "Resource": "*",
        },
      	{
            "Effect": "Allow",
            "Action": [
              "cloudwatch:ListMetrics",
              "cloudwatch:GetMetricStatistics",
              "cloudwatch:Describe*"
            ],
            "Resource": "*",
        },
    ]
}
```

### **Policy Conists of**

- **version :** 정책의 언어 버전
- **Id** : 정책을 식별하는 id
- **Statement :** 한개 이상의 statement로 구성

### **Statement consist of**

- Sid : Statement의 식별자
- **Effect** : statement가 특정 api에 접근하는 것을 허용할지 거부할지를 적어둔 것 (Allow / Deny)
- **Principal** : 특정 정책이 적용될 사용자, 계정 혹은 역할로 구성됨
- **Action** : effect에 기반해 허용 및 거부되는 API 호출의 목록
- **Resource** : 적용될 action의 리소스 목록
- Condition : statement가 언제 적용될지 결정

### **Password Policy**

- AWS에는 다양한 옵션을 이용하여 비밀번호 정책의 생성이 가능
    - 비밀번호 최소 길이 설정
    - 특정 유형의 글자 사용 요구(대문자, 소문자, 숫자, 특수문자 등)
    - IAM 사용자들의 비밀번호 변경을 허용 또는 금지
    - 일정기간이 지난 후 비밀번호 만료, 새 비밀번호 설정 요구
    - 비밀번호의 재사용 금지

### **MFA** (**Multi Factor Authentication)**

- 비밀번호 + 보안장치를 같이 사용하는 방식

1 ) `virtual MFA device` 

- Google Authenticator, Authy
- 하나의 장치에서 토큰을 여러개 지원 → 루트 계정, IAM 사용자, 또 다른 계정 등 원하는 수 만큼의 계정 및 사용자 등록이 가능

2 ) `U2F` (Universal 2nd Factor)

- 물리적인 장치로 하나의 보안 키에서 여러 계정을 지원 (YubiKey)

3 ) `Hardware Key Fob`

4 ) `Aws GovGloud` 


# 2. AWS CLI

### 1. 사용자가 AWS에 접근할 수 있는 방법

- **Management Console (관리 콘솔)**
    - 패스워드 + MFA
- **AWS Command Line Interface (CLI) → access key**
    - 명령줄 인터페이스로, 명령어를 통해 AWS 서비스들과 상호작용할 수 있는 도구
    - AWS CLI를 사용하면 AWS의 공용 API로 직접 액세스 가능
- **AWS Software Developer Kit (SDK) → access key**
    - 특정 언어로 된 라이브러리의 집합으로, 프로그래밍 언어에 따라 개별 SDK가 있음.
    - AWS SDK를 사용하면 AWS의 서비스나 API에 프로그래밍을 위한 액세스 가능
    - 터미널에서 사용하는 것이 아니라 코딩을 통해 앱 내에 심어두는 것
    - ex ) AWS CLI는 Boto라는 Python용 AWS SDK에 구축
    

# 3. IAM Role

## 1. IAM Roles for Services

> **AWS Service를 위한 Role**
> 
- EC2 인스턴스는 AWS에서 어떤 작업을 수행하려고 할 수 있음 → 그러려면 인스턴스에 권한을 부여해야하는데, 이때 IAM롤 을 만들어서 EC2와 함께 하나의 개체로 만듦 → EC2가 어떤 AWS에 있는 정보에 접근하려고 할때 IAM Role을 사용하게 됨
- we will assign **permission** to AWS services with **IAM Roles**.

## 2. IAM Security Tools (IAM 보안 도구)

- IAM Credentials Report (IAM 보안 증명 보고서, account-level)
    - 계정에 있는 사용자와 다양한 자격 증명의 상태 포함
- IAM Access Advisor (IAM 보안 관리자, user-level)
    - 사용자에게 부여된 서비스의 권한과 해당 서비스에 마지막으로 액세스한 시간을 볼 수 있음.
    - 해당 도구를 사용하여 어떤 권한이 사용되지 않는지 볼 수 있고, 이를 통해 사용자의 권한을 줄여 최소 권한의 원칙을 지킬 수 있음.

## 3. IAM Guidlines & BP

- 루트계정 사용하지 않기
- 비밀번호 정책을 강력하게 한다 + MFA를 사용한다.
- AWS 서비스에서 권한을 부여할 때마다 Role를 만들고 사용한다.
- 계정의 권한을 감사할 때에는 IAM Credential Report를 사용한다.
