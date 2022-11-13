# Data Block

## 개념

Data Block은 <mark style="color:red;">존재하고 있는 인프라를 테라폼 코드로 매핑</mark>시키고 싶을 때 사용합니다.



## 예제 - default vpc

예제로 default vpc 정보를 테라폼 코드로 읽어오겠습니다.

```hcl
provider "aws" {
  region  = "ap-northeast-2"
}

data "aws_vpc" "default" {
  default = true
}

data "aws_subnets" "default" {
  # 인프라를 가져오기 위해 필터링
  filter {
    name   = "vpc-id"
    values = [data.aws_vpc.default.id]
  }
}
```



terraform apply명령어를 실행하여 테라폼 코드를 실행합니다.

```shell
terraform apply -auto-approve
```

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>



상태를 조회하면 aws\_vpc상태가 보입니다. terraform state show명령어로 상태를 자세히보면, aws default vpc id와 상태에 저장된 id와 동일한 것을 알 수 있습니다.

```
terraform state list
terraform state show {상태}
```

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>



## 예제 - 최신 ami

```hcl
data "aws_ami" "my_amazonlinux2" {
  most_recent = true
  
  filter {
    name   = "owner-alias"
    values = ["amazon"]
  }

  filter {
    name   = "name"
    values = ["amzn2-ami-hvm-*-x86_64-ebs"]
  }

  owners = ["amazon"]
}
```
