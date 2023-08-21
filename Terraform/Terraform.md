# Terraform 테라폼

```참고한 공식 문서```
* https://registry.terraform.io/providers/NaverCloudPlatform/ncloud/latest/docs
* https://registry.terraform.io/providers/hashicorp/aws/latest/docs

## IaC란?

> 코드로 인프라스트럭처를 관리하는 것

* VMWare, 하이퍼바이저 등으로 인해 여러 대의 서버를 만들 수 있게 되어, 각 서버에 대한 구축과 운영이 자동화 될 필요가 생김 
* 따라서 인프라를 사람이 읽을 수 있는 코드로 만들음 -> 테라폼 자체 언어인 HCL을 사용해 클라우드 리소스 선언

## IaC 종류

#### Provisioning Tool

* 클라우드 서버, 가상 머신, 컨테이너, 네트워크, 스토리지 등의 리소스를 프로비저닝하고 설정
* ex ) ```Terraform```

#### SCM Tool (구성 관리 도구)

*  소프트웨어 코드와 구성 요소의 변경 사항을 추적하고 관리
* ex ) ```Ansible```

## Terraform이란?

> IaC도구 중 하나로, 코드를 통해 인프라 서버를 구축/운영할 수 있는 소프트웨어
* 수동으로 서버를 생성하지 않고, 코드로 생성하기 때문에 서버 구축 및 운영이 ```자동화```될 수 있음
   * 모든 인프라가 코드로 기록 및 관리가 되므로 자동으로 문서화됨
   * git을 통해 형상관리가 가능하기 때문에 여러 팀원 간 협업이 용이하며, 변경 이력을 추적할 수 있음
* ```선언적 언어```
* ```멱등성```
  * 동일한 코드를 여러 번 적용하더라도 처음과 같은 결과를 얻는 것을 의미
  * ex ) EC2의 생성 개수를 5개로 설정하면 (5개까지만 만들 수 있도록 제한을 걸어두었음을 가정), 5개의 EC2 인스턴스가 추가로 생성되지 않음
  
## 설치법

```
$ brew install terraform
$ terraform --version
```

## command

![image](https://github.com/yaezzin/TIL/assets/97823928/5cc2e6fd-69d2-41ec-9207-7d8fc2f9306c)

```init```
- terraform이 코드를 스캔하여, 어느 공급자(provider)인지 확인하고, 필요한 코드를 다운로드
- 해당 코드는 .terraform 폴더에 저장
- 루트 모듈을 수정할 때마다 init을 진행해야 함

```plan```
- 실제로 resource를 변경하기 전에 terraform이 수행할 작업을 확인
- 테라폼 코드와 실제 인프라의 diff를 출력해 줌

```apply```
- 실제로 terraform 코드를 실행하여 provider에 해당 변경사항 반영

```destroy```
- 해당 코드로 생성된 모든 resource를 제거

## 구성 요소

#### provider

```terraform
provider "aws" {

  region = "ap-northeast-2"
  version = "~> 3.0"

}
```
* 테라폼으로 생성할 인프라의 종류 의미
* 일반적으로 provider.tf 파일에 정의

#### resource

```
resource "aws_vpc" "example" {
    cidr_block = "10.0.0.0/16"
}
```
* 테라폼으로 생성할 실제 인프라 자원을 의미

#### variable

```terraform
variable "http_port" {
      description = "This is the http_port"
      default     = 80
}

...

# 사용법
resource "aws_security_group" "web_sg" {
    name = "standalone_web_sg"
    ingress {
          # var."변수명"
      from_port   = var.http_port
      to_port     = var.http_port
      protocol    = "tcp"
      cidr_blocks = ["${var.cidr_blocks["cidr_all"]}"]
    }
  }
```
* 모듈에서 사용할 변수를 정의할 때 사용 (입력 변수)

#### output

``` terraform
# 모듈A에 특정 리소스 생성
  resource "aws_vpc" "default" {
      cidr_block = "10.0.0.0/16"
  }

  # 모듈A에서 만든 aws_vpc 정보를 state 파일에 저장
  output "vpc_id" {
      value = aws_vpc.default.id
  }
```
* 테라폼으로 만든 자원을, state 파일에 변수 형태로 저장하는 것
* 동일한 워크페이스에서 서로 다른 모듈간 리소스를 참고해야 할 때 사용

#### data

```terraform
data "aws_availability_zones" "available" {
    state = "available"
}

resource "aws_subnet" "primary" {
  availability_zone = data.aws_availability_zones.available.names[0]
}
```
* 다양한 출처로부터 이미 생성 되어있는 리소스 정보를 가져오거나, 특정 워크스페이스의 리소스 정보를 가져와야 할 때 사용

## 컨테이너 만들기

```terraform
terraform {
    required_providers {
        docker = {
            source = "kreuzwerker/docker"
            version = "~> 3.0.1"
        }
    }
}

provider "docker" {

}

resource "docker_image" "nginx" {
  name = "nginx"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name = "tutorial"

  ports {
      internal = 80
      external = 8888
  }
}
```

```
$ terraform plan
$ terraform apply
```
* provider는 테라폼으로 생성할 인프라의 종류를 지정
* nginx라는 이미지와 tutorial이라는 도커 컨테이너가 생성됨

## Ncloud 서버 만들기

```terraform
terraform {
  required_providers {
      ncloud = {
          source ="NaverCloudPlatform/ncloud"
      }
  }
  required_version = ">= 0.13"
}

provider "ncloud" {
  region = "KR"
  site = "PUBLIC"
  support_vpc = true
}

# login key 생성 - 인스턴스 root 비밀번호를 얻기 위해 필요한 인증키를 생성하기 위한 설정
resource "ncloud_login_key" "loginkey" {
  key_name = "test-key"
}

# vpc 생성
resource "ncloud_vpc" "test" {
  ipv4_cidr_block = "10.1.0.0/16"
  name = "lion-tf"
}

# subnet 생성
resource "ncloud_subnet" "test" {
  vpc_no         = ncloud_vpc.test.vpc_no
  subnet         = cidrsubnet(ncloud_vpc.test.ipv4_cidr_block, 8, 1)
  zone           = "KR-2"
  network_acl_no = ncloud_vpc.test.default_network_acl_no
  subnet_type    = "PUBLIC"
  usage_type     = "GEN"
  name = "lion-tf-sub"
}

# 서버 인스턴스 생성
resource "ncloud_server" "server" {
  subnet_no                 = ncloud_subnet.test.id
  name                      = "my-tf-server2" #"my-tf-server"
  server_image_product_code = "SW.VSVR.OS.LNX64.UBNTU.SVR2004.B050"
  server_product_code = data.ncloud_server_products.spec.server_products[0].product_code
  login_key_name            = ncloud_login_key.loginkey.key_name
  init_script_no            = ncloud_init_script.init.init_script_no
}

# init script 생성
resource "ncloud_init_script" "init" {  
  name     = "set-docker-tf"
  content = <<EOT
        #!/bin/bash

      USERNAME="{user}"
      PASSWORD="{password}"
      REMOTE_DIRECTORY="/home/{directory}/"

      echo "Add user"
      useradd -s /bin/bash -d $REMOTE_DIRECTORY -m $USERNAME

      echo "Set password"
      echo "$USERNAME:$PASSWORD" | chpasswd

      echo "Set sudo"
      usermod -aG sudo $USERNAME
      echo "$USERNAME ALL=(ALL:ALL) NOPASSWD: ALL" >> /etc/sudoers.d/$USERNAME

      echo "Update apt and Install docker & docker-compose"
      sudo apt-get update
      sudo apt install -y docker.io docker-compose

      echo "Start docker"
      sudo service docker start

      echo "Add user to 'docker' group"
      sudo usermod -aG docker $USERNAME

      echo "done"
      EOT
}

# 공인 ip를 인스턴스에 할당
resource "ncloud_public_ip" "test" {
  server_instance_no = ncloud_server.server.instance_no
}

# 서버 스펙
data "ncloud_server_products" "spec" {
  server_image_product_code = "SW.VSVR.OS.LNX64.CNTOS.0703.B050"  

  filter {
    name   = "product_code"
    values = ["SSD"]
    regex  = true
  }

  filter {
    name   = "cpu_count"
    values = ["2"]
  }

  filter {
    name   = "base_block_storage_size"
    values = ["50GB"]
  }

  filter {
    name   = "product_type"
    values = ["HICPU"]
  }

  output_file = "product.json"
}

# 생성한 서버 인스턴스의 ip 확인
output "products" {
  value = {
    for product in data.ncloud_server_products.spec.server_products:
    product.id => product.product_name
  }
}

output "server_ip" {
  value = ncloud_server.server.public_ip
}

output "public_ip" {
  value = ncloud_public_ip.test.public_ip
}
```

## AWS

```terraform
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

# Configure the AWS Provider
provider "aws" {
  access_key = "{ACCESS-KEY}" 
  secret_key = "{SECRET-KEY}"
  region = "ap-northeast-2"
}

# Create a VPC
resource "aws_vpc" "example" {
  cidr_block = "10.1.0.0/16"

  tags = {
    Name = "lion-vpc"
  }
}

# Create IAM
resource "aws_iam_user" "lion" {
  name = "lion-tf"
  path = "/"

  #tags = {
  #  tag-key = "tag-value"
  #}
}

# lion 유저의 access key 생성
resource "aws_iam_access_key" "lion" {
  user = aws_iam_user.lion.name
}

# 권한 생성
data "aws_iam_policy_document" "lion_ro" {
  statement {
    effect    = "Allow"
    actions   = ["ec2:Describe*"]
    resources = ["*"]
  }
}

# 권한 부여
resource "aws_iam_user_policy" "lion_ro" {
  name   = "tf-test"
  user   = aws_iam_user.lion.name
  policy = data.aws_iam_policy_document.lion_ro.json
}

# 로그인을 위한 프로필 생성
resource "aws_iam_user_login_profile" "example" {
  user =  aws_iam_user.lion.name
}

# 패스워드 확인
output "password" {
  value = aws_iam_user_login_profile.example.password
  sensitive = true # 콘솔에서 안보여줌
}
```
* aws에는 여러 인증 방법이 있는데 로컬에 aws cli 인증 시 설치해둔 크리덴셜로 로그인이 됨
* 그렇지 않은 경우 직접 변수를 지정해주자!

## 참고 

* https://wooono.tistory.com/666


