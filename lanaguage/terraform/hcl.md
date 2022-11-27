# 테라폼 언어(HCL) 구조

테라폼 코드는 HCL라는 언어로 작성합니다. HCL은 Block이라는 기본단위를 사용합니다. 결국, 테라폼 코드는 Block단위의 집합입니다.

<figure><img src="../../.gitbook/assets/image (2) (3) (2).png" alt=""><figcaption></figcaption></figure>



Block의 설정(Body)은 Argument로 구성됩니다. Argument는 IDENTIFIER과 EXPRESSION으로 표현할 수 있습니다. 공식문서는 어렵게 설명되어 있는데, 단순하게 “필드이름”=”필드값”이라고 생각하면 됩니다.

<figure><img src="../../.gitbook/assets/image (5) (3).png" alt=""><figcaption><p>참고자료: <a href="https://developer.hashicorp.com/terraform/language">https://developer.hashicorp.com/terraform/language</a></p></figcaption></figure>



Argument 중에 특수한 기능을 담당하는 Argument는 Meta-Argument라고 부릅니다. Meta-arguments는 공식문서([https://developer.hashicorp.com/terraform/language](https://developer.hashicorp.com/terraform/language))에서 찾을 수 있습니다.

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>
