# 중앙 집중식 비밀 저장소 연동

테라폼 코드에 불가피하게 민감정보를 작성해야 하는 경우, 중앙 집중식 비밀 저장소(aws Secret Manager, Valut 등)를 사용하여 민감정보 노출을 최소화 할 수 있습니다.



## 중앙 집중식 비밀 저장소란?

중앙 집중식 비밀 저장소는 민감정보를 안전하게 저장하는 저장소입니다. 운영자가 민감정보를 저장소에 저장하면 사용자는 저장소에 접근하여 민감정보를 조회합니다.

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>



## 테라폼 연동

테라폼에서는 data block으로 중앙 집중식 비밀 저장소에 저장된 민감정보를 조회할 수 있습니다.

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>



## 예제 - aws secret manager

aws secret manager를 이용하여 aws RDS 계정과 비밀번호를 설정하는 예제를 실습하겠습니다. 먼저 secret manager를 생성하고 RDS 계정과 비밀번호를 저장합니다. 계정과 비밀번호는 variable block으로 설정했습니다.

{% hint style="info" %}
테라폼 코드 링크: [https://github.com/sungwook-practice/terraform-study/tree/main/week6/aws\_secret\_manager/prerequisite](https://github.com/sungwook-practice/terraform-study/tree/main/week6/aws\_secret\_manager/prerequisite)
{% endhint %}

```hcl
provider "aws" {
  region = "ap-northeast-2"
}

variable "rdb_cred" {
  default = {
    username = "testuser"
    password = "helloworld"
  }

  type = map(string)
}

resource "aws_secretsmanager_secret" "example" {
  name = "t101-study"
}

resource "aws_secretsmanager_secret_version" "example" {
  secret_id     = aws_secretsmanager_secret.example.id
  secret_string = jsonencode(var.rdb_cred)
}
```



terraform apply명령어로 테라폼 코드를 aws에 반영합니다.

```
terraform apply
```



secret manager 대시보드를 확인하면 secret이 생성되었습니다.

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>



secret에서는 RDS 계정과 비밀번호가 key/value형식으로 저장되어 있습니다.

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>



이제 RDS를 생성해봅시다. dat block으로 aws secret manager에 접근하여 계정과 비밀번호를 조회합니다. 그리고 조회한 데이터로 RDS 계정과 비밀번호를 설정합니다.

{% hint style="info" %}
테라폼 코드 링크: [https://github.com/sungwook-practice/terraform-study/tree/main/week6/aws\_secret\_manager/main](https://github.com/sungwook-practice/terraform-study/tree/main/week6/aws\_secret\_manager/main)
{% endhint %}

```hcl
provider "aws" {
  region = "ap-northeast-2"
}

data "aws_secretsmanager_secret_version" "creds" {
  secret_id = "t101-study"
}

locals {
  db_creds = jsondecode(
    data.aws_secretsmanager_secret_version.creds.secret_string
  )
}

resource "aws_db_instance" "myrds" {
  identifier_prefix   = "t101-sensitivedata"
  engine              = "mysql"
  allocated_storage   = 10
  instance_class      = "db.t2.micro"
  skip_final_snapshot = true

  db_name  = "with_aws_secret_manager_demo"
  username = local.db_creds.username
  password = local.db_creds.password
}
```



terraform apply 명령어를 실행하여 aws RDS를 생성합니다.

```
terraform apply
```



aws RDS 대시보드에서 RDS가 생성되었는지 확인합니다. 그리고 RDS 속성에서 username이 aws secret manager의 username과 같은지 확인합니다.

<figure><img src="../../.gitbook/assets/image (2) (3).png" alt=""><figcaption></figcaption></figure>

