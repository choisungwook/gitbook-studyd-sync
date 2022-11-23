# aws 리소스 모음

## S3

```hcl
resource "aws_s3_bucket" "mys3bucket" {
  bucket = "akbun-t101study-tfstate"
}
```



## S3 Backend

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



## VPC, Subnet, Route table, Internet Gateway

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

resource "aws_route_table" "myrt" {
  vpc_id = aws_vpc.akbun-vpc.id

  tags = {
    Name = "t101-rt"
  }
}

resource "aws_route_table_association" "myrtassociation1" {
  subnet_id      = aws_subnet.akbun-subnet1.id
  route_table_id = aws_route_table.myrt.id
}

resource "aws_route" "mydefaultroute" {
  route_table_id         = aws_route_table.myrt.id
  destination_cidr_block = "0.0.0.0/0"
  gateway_id             = aws_internet_gateway.akbun-igw.id
}

output "my_vpc_id" {
  value       = aws_vpc.akbun-vpc.id
  description = "vpc"
}

```



## EC2 Instance

```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c76973fbe0ee100c"
  instance_type = "t2.nano"
  associate_public_ip_address = true
  # subnet_id  = ""

  tags = {
    Name = "t101-week3"
  }
}

output "myec2_public_ip" {
  value       = aws_instance.example.public_ip
  description = "The public IP of the Instance"
}
```



## EC2 Instance With Security Group

```hcl
provider "aws" {
  region = "ap-northeast-2"
}

resource "aws_instance" "example" {
  ami                    = "ami-0e9bfdb247cc8de84"
  instance_type          = "t2.micro"
  vpc_security_group_ids = [aws_security_group.instance.id]

  user_data = <<-EOF
              #!/bin/bash
              echo "Hello, T101 Study" > index.html
              nohup busybox httpd -f -p 8080 &
              EOF

  tags = {
    Name = "Single-WebSrv"
  }
}

resource "aws_security_group" "instance" {
  name = var.security_group_name

  ingress {
    from_port   = 8080
    to_port     = 8080
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

variable "security_group_name" {
  description = "The name of the security group"
  type        = string
  default     = "terraform-example-instance"
}

output "public_ip" {
  value       = aws_instance.example.public_ip
  description = "The public IP of the Instance"
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
