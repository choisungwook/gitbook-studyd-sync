# count를 이용한 반복문

{% embed url="https://youtu.be/xOSshyyJoZk" %}



## 사용방법

&#x20;meta-argument 중 <mark style="color:red;">count는 block을 count 값 만큼 반복</mark>합니다. 아래 예제에서 EC2 instace가 2대 생성됩니다. count가 2개로 설정되어서 2대 생성됩니다.

```hcl
resource "aws_instance" "server" {
  count = 2
 
  ...  
}
```



{% hint style="info" %}
예제코드 링크: [https://github.com/sungwook-practice/terraform-study/tree/main/week5/for-loop/count-meta-argument](https://github.com/sungwook-practice/terraform-study/tree/main/week5/for-loop/count-meta-argument)
{% endhint %}

아래 예제에서 EC2 instace가 2대 생성됩니다. count가 2개로 설정되어서 2대 생성됩니다.

```hcl
provider "aws" {
  region  = "ap-northeast-2"
}
 
data "aws_ami" "last_ami" {
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
 
resource "aws_instance" "server" {
  count = 2
 
  ami           = data.aws_ami.last_ami.id
  instance_type = "t2.nano"
 
  tags = {
    Name = "Server ${count.index}"
  }
}
```



terraform apply명령어를 실행하고 EC2 Instance를 확인하면 2개가 잘 생성되었습니다.

```
terraform apply
```

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>



## 반복 index 접근

<mark style="color:red;">count.index를 사용하면 index(반복횟수)에 접근</mark>할 수 있습니다. 예제에서는 tag.name을 설정할 때 index를 사용했습니다.

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>



{% hint style="info" %}
예제코드 링크: https://github.com/sungwook-practice/terraform-study/tree/main/week5/for-loop/count-meta-argument
{% endhint %}

```hcl
provider "aws" {
  region  = "ap-northeast-2"
}
 
data "aws_ami" "last_ami" {
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
 
resource "aws_instance" "server" {
  count = 2
 
  ami           = data.aws_ami.last_ami.id
  instance_type = "t2.nano"
 
  tags = {
    Name = "Server ${count.index}"
  }
}
```



terraform apply명령어를 실행 후 EC2 Instance의 tag.name을 확인해봅시다.

```
terraform apply
```



EC2 Instance tag.name이 의도한 대로 index가 포함되어 있습니다.

<figure><img src="../../.gitbook/assets/image (4) (4).png" alt=""><figcaption></figcaption></figure>



## variable 연동

variable연동은 쉽습니다. variable를 참조만 하면 됩니다. 주의사항은 생성하려고 하는 리소스가 중복된 값을 허용하는지를 알아야하고 참조하는 variable타입에 맞게 코드를 작성해야 합니다.



아래 예제서는 iam user를 생성합니다. iam user는 variable을 참조하고 variable타입이 list입니다. iam user는 중복을 허용하지 않으므로 count.index를 활용하여 iam user가 중복되지 않게 설정했습니다.

```hcl
provider "aws" {
  region = "us-east-2"
}
 
variable "user_names" {
  description = "Create IAM users with these names"
  type        = list(string)
  default     = ["aaa", "bbb", "ccc"]
}
 
resource "aws_iam_user" "example" {
  count = length(var.user_names)
  name = var.user_names[count.index]
}
```



## 주의사항 - block argument이해&#x20;

count는 block argument를 이해하고 잘 사용해야 합니다. 예를 들어 s3는 bucket이름을 중복해서 생성할 수 없으므로 count를 그대로 사용하면 오류가 발생합니다.



{% hint style="info" %}
예제코드: https://github.com/sungwook-practice/terraform-study/blob/main/week5/for-loop/count-meta-argument-s3/main.tf
{% endhint %}

```hcl
resource "aws_s3_bucket" "mys3bucket" {
  count  = 2
  bucket = "akbun-t101study-tfstate"
}
```



terraform apply 명령어를 실행하면 2번째 bucket생성 할 때, bucket 생성 오류가 발생합니다.

```
terraform apply
```

<figure><img src="../../.gitbook/assets/image (1) (2).png" alt=""><figcaption></figcaption></figure>
