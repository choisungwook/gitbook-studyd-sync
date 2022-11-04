# 테라폼의 시작 Provider

\[테라폼]챕터에서 잠깐 언급했지만, 테라폼을 사용하기 위해 Provider 설정이 필요합니다. 코드를 어떤 인프라 환경에 반영할 것인가를 설정합니다.



아래 예제는 aws에 코드를 반영하기 위해 aws provider를 설정했습니다. 그리고 region, access\_key, secret\_key를 설정했습니다.

> 참고자료: [https://registry.terraform.io/providers/hashicorp/aws/latest/docs](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)

```hcl
provider "aws" {
  region = "ap-northeast-2"
  access_key = "my-access-key"
  secret_key = "my-secret-key"
}
```



access\_key와 secret\_key는 외부에 노출될 위험이 있으므로 코드에 설정하는 것은 위험합니다. 환경변수나 provider가 지원하는 설정파일에 설정하는 것이 좋습니다. aws provider는 $HOME/.aws/credentials파일에 저장돈 access\_key와 secret\_key를 불러올 수 있습니다.

<figure><img src="../../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>



awscli가 설치되었다면 aws configure로 credentials파일을 쉽게 설정할 수 있습니다.

```shell
$ aws configure
AWS Access Key ID [None]: 
AWS Secret Access Key [None]: 
```
