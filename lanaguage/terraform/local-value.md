# Local value

local value는 프로그래밍 언어 변수기능과 같습니다. 이름과 값을 설정하면 하나의 변수가 생성되고 테라폼 코드에서 변수를 참조하여 사용할 수 있습니다. 동<mark style="color:red;">일한 값을 여러 번 사용하거나 값에 이름을 부여하여 관리하고 싶을 때 local value를 사용</mark>하면 좋습니다.



locals block으로 local value를 정의할 수 있습니다. <mark style="color:red;">local value는 local.<이름>으로 참조</mark>할 수 있습니다.

```hcl
provider "aws" {
  region = "ap-northeast-2"
}

locals {
  bucket_name = "akbun-t101-week4-local-example"
}

resource "aws_s3_bucket" "mys3bucket" {
  bucket = local.bucket_name
}
```

<figure><img src="../../.gitbook/assets/image (2) (3).png" alt=""><figcaption></figcaption></figure>



local value는 input variable과 비교되는데요. local value가 외부에서 값을 정의할 필요가 있을 때는 input variable을 사용하면 됩니다.
