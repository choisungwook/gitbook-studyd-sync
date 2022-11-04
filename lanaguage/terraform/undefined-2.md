# 원격상태파일 참조

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

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>



## &#x20;
