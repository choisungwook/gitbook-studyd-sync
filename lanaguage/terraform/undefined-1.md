# 변수와 입력

## 변수 선언 방법

variable BLOCK type을 사용하면 변수를 선언할 수 있습니다. 정의할 수 있는 속성은 테라폼 공식문서를 참고하시길 바랍니다.

{% hint style="info" %}
Terraform input 공식문서: [https://developer.hashicorp.com/terraform/language/values/variables#arguments](https://developer.hashicorp.com/terraform/language/values/variables#arguments)
{% endhint %}

```hcl
variable "<변수 이름>" {
  description = "변수 설명"
  type        = 변수타입
}
```



아래 예제는 server\_port라는 변수이름을 선언했습니다. server\_port 변수는 정수타입니다. 관례적으로 변수정의는 variables.tf파일에 저장합니다.

```hcl
variable "server_port" {
  description = "The port the server will use for HTTP requests"
  type        = number
}
```



## 변수값 초기화 방법

변수를 정의하면 초기화를 해야 사용할 수 있습니다. 초기화 실습을 하기 위해 number타입을 갖는 server\_port변수를 정의하겠습니다.

```hcl
variable "server_port" {
  description = "The port the server will use for HTTP requests"
  type        = number
}
```



### 대화형 프롬프트

variable block을 정의한 후, terraform plan명령어를 실행하면 **변수를 초기화하는 프롬프트 창**이 나옵니다. 8080을 입력하면 server\_port변수 값이 8080으로 설정됩니다.

```
terraform plan
```

<figure><img src="../../.gitbook/assets/image (38) (1).png" alt=""><figcaption></figcaption></figure>

### 환경변수

프롬프트를 입력하지 않고 환경변수로 변수 값을 설정할 수 있습니다. TF\_VAR\_{변수 이름}으로 환경변수를 설정하고 환경변수 값에 초기화 할 값을 설정하면 됩니다. 환경변수 초기화 방법은 외부에 노출시키면 안되는 민감정보를 초기화 할 때 사용하면 좋습니다.

```shell
# 리눅스 환경변수 정의방법
export TF_VAR_server_port=8080
```



terraform plan명령어를 실행하면 변수 값이 설정되었기 떄문에, 프롬프트가 나오지 않습니다.

```
terraform plan
```

<figure><img src="../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>



환경변수에 설정된 변수값을 사용하고 싶지 않으면 해당 환경변수를 삭제하면 됩니다.

```shell
# 리눅스 환경변수 삭제 방법
unset <환경변수 이름>
```



### 테라폼 명령어 인자

테라폼 명령어를 실행할 때 -var인자로 변수 값을 초기화 할 수 있습니다.

```shell
terraform plan -var "server_port=8081"
```



terraform plan명령어를 실행하면 변수 값이 설정되었기 떄문에, 프롬프트가 나오지 않습니다.

<figure><img src="../../.gitbook/assets/image (14) (1).png" alt=""><figcaption></figcaption></figure>



## Default 값 설정

variable BLOCK TYPE을 정의할 때, 변수 default 값을 설정할 수 있습니다.

```hcl
variable "server_port" {
  description = "The port the server will use for HTTP requests"
  type        = number
  default     = 8080
}
```



terraform plan명령어를 실행하면 변수 값이 설정되었기 떄문에, 프롬프트가 나오지 않습니다.

```
terraform paln
```

<figure><img src="../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>
