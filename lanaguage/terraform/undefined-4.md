# 레이아웃

## what is layout

애플리케이션을 개발/운영을 나누듯이 인프라 환경도 개발/운영 등 분리해서 관리합니다. 인프라 환경을 분리함에 따라 테라폼 코드도 적절하게 구조를 만들어서 관리해야 합니다. 이 때, <mark style="color:red;">코드 구조를 “레이아웃”이라고 부릅니다</mark>.



## Use case

레이아웃은 정답이 없습니다. 주어진 상황에 맞게 적절히 레이아웃을 만들면 되는데요. 이 글에서는 스터디에서 다룬 레이아웃을 소개합니다.



레이아웃은 크게 2종류로 분리할 수 있을 것 같습니다.

1. 최상위(root): 환경(개발, 스테이지)과 global리소스3(SS, IAM 등)로 분류했습니다.

<figure><img src="../../.gitbook/assets/image (4) (2).png" alt=""><figcaption></figcaption></figure>

하위구성요소: 최상위 아래에는 모듈과 tf파일 집합으로 구성되어 있습니다.

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>



## Example

{% hint style="info" %}
github 링크: [https://github.com/sungwook-practice/terraform-study/tree/main/week3/layout](https://github.com/sungwook-practice/terraform-study/tree/main/week3/layout)
{% endhint %}

실습에서 구성할 aws전체 구성도입니다.

* 테라폼 backend
  * S3
  * DynamoDB
* VPC
  * Vpc
  * Subnet
  * Route table
  * Internet gateway
* EC2
  * Auto scaling group
  * Application Load Balancer
  * Security group
* RDS

<figure><img src="../../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>



이 예제에서는 레이아웃 환경분리를 2가지했습니다. global aws리소스와 global이 아닌 aws리소스입니다. global아닌 리소스는 dev/prd로 분리했습니다.

<figure><img src="../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>



역할별로 모듈을 만들었습니다. 그리고 main.tf에는 모듈을 import하여 최종적으로 aws 리소스를 테라폼 코드로 작성했습니다.

<figure><img src="../../.gitbook/assets/image (8) (2).png" alt=""><figcaption></figcaption></figure>



main.tf의 내용은 아래와 같습니다. my-service 모듈을 사용할 때, 원격 상태파일을 참조할 수 있다는 것을 나타냈습니다.

```hcl
module "my-vpc" {
  source = "./vpc"

  vpc_name = "t101-study-week-layout"
}

module "my-db" {
  source  = "./db"
  depends_on = [module.my-vpc]

  subnet1 = module.my-vpc.mysubnet3
  subnet2 = module.my-vpc.mysubnet4
  security_group = module.my-vpc.mysg
  db_username = var.db_username
  db_password = var.db_password
}

module "my-service" {
  source  = "./service"
  depends_on = [module.my-vpc, module.my-db]

  # vpc_id = module.my-vpc.my_vpc_id
  # db_address = module.my-db.db_address
  # db_port = module.my-db.db_port
  vpc_id = data.terraform_remote_state.dev_remote.outputs.my_vpc_id
  db_address = data.terraform_remote_state.dev_remote.outputs.db_address
  db_port = data.terraform_remote_state.dev_remote.outputs.db_port
}
```



테라폼 코드를 aws에 반영해보겠습니다. <mark style="color:red;">먼저 backend 리소스를 배포</mark>합니다.

```
cd global
terraform init && terraform apply
```



backend 리소스 생성 후 main리소스를 반영합니다.

```
cd ../dev
terraform init && terraform apply
```



aws 콘솔에서 직접 확인해봅시다. Load Balancer가 잘 생성되었는지 확인합니다.

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>



Load Balacner에 연결된 Targer group에는 ASG가 생성한 EC2 Instance 2개가 연결되어 있습니다. 2개인 이유는 테라폼 코드에서 최소 인스턴스 개수를 2개로 설정했기 때문입니다.

<figure><img src="../../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>



RDS가 잘 생성되었는지 확인합니다.

<figure><img src="../../.gitbook/assets/image (1) (2) (1).png" alt=""><figcaption></figcaption></figure>



Load Balacner를 호출하면 Auto Scaling Group이 생성한 EC2 Instnace 2대로 부하분산이 됩니다.

```shell
ALBDNS=$(terraform output -raw myalb_dns)
for i in {1..100}; do curl -s http://$ALBDNS:8080/ ; done | sort | uniq -c | sort -nr
```

<figure><img src="../../.gitbook/assets/image (4) (1) (3).png" alt=""><figcaption></figcaption></figure>
