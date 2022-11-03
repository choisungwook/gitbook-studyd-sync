# 상태파일 민감정보

## 상태파일에 평문으로 저장되는 민감정보

테라폼 상태파일은 디폴트 저장이 평문저장입니다. <mark style="color:red;">환경변수로 변수값을 넘겨도 상태파일의 값은 평문으로 저장</mark>됩니다. 만약 여러분이 public git에 state파일을 저장한다면 소중한 민감정보가 노출되게됩니다.



해결방법은 시크릿 저장소를 이용하여 민감정보를 암호화해야 합니다. 대표적으로 valut, 클라우드 솔루션(aws secret manager, GCP kms 등)이 있습니다.\\



## 예제

정말로 민감정보가 상태파일에 평문으로 저장되는지 실습해보겠습니다. 민감정보를 상태파일로 저장할 수 있는 aws RDS를 예제로 살펴볼겁니다. RDS는 로그인할 계정과 비밀번호가 필요합니다.

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>



RDS 계정과 비밀번호는 입력으로 받을 수 있도록 variable block을 사용했습니다.

{% hint style="info" %}
github 링크: [**https://github.com/sungwook-practice/terraform-study/tree/main/week3/sensitive\_data**](https://github.com/sungwook-practice/terraform-study/tree/main/week3/sensitive\_data)****
{% endhint %}

```hcl
provider "aws" {
  region = "ap-northeast-2"
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

variable db_username {
  type        = string
  description = "사용자 이름"
}

variable db_password {
  type        = string
  description = "비밀번호"
}
```



계정과 비밀번호는 테라폼 환경변수로 설정합니다.

```shell
export TF_VAR_db_username='user'
export TF_VAR_db_password='password'
```



terraform apply명령어로 RDS를 생성합니다.

```shell
terraform apply
```



RDS대시보드를 방문하여 RDS가 생성되었는지 확인합니다.

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>



상태파일에서 username과 password를 검색하면, 환경변수로 설정했던 계정과 비밀번호가 평문으로 저장된 것을 확인할 수 있습니다. 테라폼 코드에 민감정보를 저장하지 않기 위해 환경변수로 설정했지만 결국,상태파일에는 민감정보가 평문으로 저장됩니다.

```shell
egrep 'username|password' terraform.tfstate
```

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>
