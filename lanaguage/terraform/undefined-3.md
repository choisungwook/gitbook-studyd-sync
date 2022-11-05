# 원격상태파일 참조

## Overview

terraform remote state는 <mark style="color:red;">원격 상태파일에 있는 값을 참조하는 방법</mark>입니다. 다른 팀이 작업한 리소스를 참조하거나 레이아웃을 여러 개 만든 상황에서 레이아웃끼리 리소스 참조 등에 사용할 수 있습니다.

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>



data block와 terraform\_remote\_state을 조합해서 원격상태파일을 가져올 수 있습니다.

```hcl
data "terraform_remote_state" "remote" {
  backend = "s3"
  config = {
    bucket = "..."
    key    = "terraform.tfstate"
    region = "ap-northeast-2"
  }
}
```



원격상태파일 값을 참조하려면 제공해주는 곳에서 output block을 선언해야 합니다.

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

## Example

{% hint style="info" %}
github 링크: [https://github.com/sungwook-practice/terraform-study/tree/main/week3/remote\_state\_datasource](https://github.com/sungwook-practice/terraform-study/tree/main/week3/remote\_state\_datasource)
{% endhint %}

원격파일을 저장할 s3와 vpc 코드를 작성합니다. vpc는 remote state data block으로 값을 참조할 리소스입니다.

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>



먼저 tmp폴더를 생성하고 s3코드를 작성합니다.

```hcl
provider "aws" {
  region = "ap-northeast-2"
}

resource "aws_s3_bucket" "mys3bucket" {
  bucket = "akbun-t101study-tfstate"
}

output "s3_bucket_arn" {
  value       = aws_s3_bucket.mys3bucket.arn
  description = "The ARN of the S3 bucket"
}
```



terraform apply명령어로 s3를 생성합니다.

```shell
# 코드 실행
terraform init & terraform apply
```



상위 경로로 이동하고 backend.tf파일을 생성하여 backend설정을 합니다.

```hcl
terraform {
  backend "s3" {
    bucket = "akbun-t101study-tfstate"
    key    = "terraform.tfstate"
    region = "ap-northeast-2"
  }
}
```



terraform init명령어로 backend설정을 적용합니다.

```
terraform init
```



상태파일에 샘플 값을 생성하기 위해 vpc코드를 작성합니다. <mark style="color:red;">output block 꼭 작성</mark>해야 합니다.

```hcl
provider "aws" {
  region = "ap-northeast-2"
}

resource "aws_s3_bucket" "mys3bucket" {
  bucket = "akbun-t101study-tfstate"
}

output "s3_bucket_arn" {
  value       = aws_s3_bucket.mys3bucket.arn
  description = "The ARN of the S3 bucket"
}

resource "aws_vpc" "akbun-vpc" {
  cidr_block       = "10.10.0.0/16"
  enable_dns_support   = true
  enable_dns_hostnames = true

  tags = {
    Name = "t101-study"
  }
}

resource "aws_subnet" "akbun-subnet1" {
  vpc_id     = aws_vpc.akbun-vpc.id
  cidr_block = "10.10.1.0/24"

  availability_zone = "ap-northeast-2a"

  tags = {
    Name = "t101-subnet1"
  }
}

resource "aws_internet_gateway" "akbun-igw" {
  vpc_id = aws_vpc.akbun-vpc.id

  tags = {
    Name = "t101-igw"
  }
}

output "subnetid" {
  value       = aws_subnet.akbun-subnet1.id
  description = "akbun-subnet1"
}

```



terraform apply명령어를 실행하여 vpc를 생성합니다.

```
terraform init; terraform apply
```



data block와 terraform\_remote\_state로 원격 상태파일에 있는 값을 가져올 수 있습니다. 읽기전용으로 값을 불러오므로 수정이 불가합니다.

```hcl
data "terraform_remote_state" "remote" {
  backend = "s3"
  config = {
    bucket = "akbun-t101study-tfstate"
    key    = "terraform.tfstate"
    region = "ap-northeast-2"
  }
}
```



data block참조 문법을 사용하여 원격 상태 값을 사용할 수 있습니다. outputs필드를 꼭 사용해야 합니다.

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c76973fbe0ee100c"
  instance_type = "t2.nano"
  subnet_id  = data.terraform_remote_state.remote.outputs.subnetid

  tags = {
    Name = "t101-week3"
  }
}
```



terraform apply명령어로 테라폼 코드를 인프라에 반영합니다.

```
terraform apply
```



aws EC2 Instance대시보드를 확인하면 EC2 instsnace가 원격상태파일의 subnet설정으로 잘 생성되었습니다.

<figure><img src="../../.gitbook/assets/image (12) (2).png" alt=""><figcaption></figcaption></figure>
