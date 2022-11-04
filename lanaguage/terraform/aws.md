# aws 리소스 모음

## s3 backend

```hcl
terraform {
  backend "s3" {
    bucket = "akbun-t101study-tfstate-week3-files"
    key    = "terraform.tfstate"
    region = "ap-northeast-2"
    # dynamodb_table = "terraform-locks-week3-files"
  }
}
```



## VPC, Subnet, Internet Gateway

```hcl
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

```



## RDS Mysql

약 4분 소요됩니다.

```hcl
provider "aws" {
  region = "ap-northeast-2"
}

variable db_username {
  type        = string
  description = "사용자 이름"
}

variable db_password {
  type        = string
  description = "비밀번호"
}

resource "aws_db_instance" "myrds" {
  identifier_prefix      = "t101-sensitivedata"
  engine                 = "mysql"
  allocated_storage      = 10
  instance_class         = "db.t2.micro"
  skip_final_snapshot    = true

  db_name                = "sensitive_example"
  username               = var.db_username
  password               = var.db_password
}
```
