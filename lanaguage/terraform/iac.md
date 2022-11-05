# IAC와 테라폼 소개

## What is IAC

테라폼은 IAC도구입니다. 그러므로 IAC가 무엇인지 이해하는게 중요합니다. IAC(Infrastructure as Code, _**IaC**_)는 <mark style="color:red;">코드로 인프라를 관리</mark>하는 방법입니다.



코드로 인프라를 관리한다는 말이 무엇일까요? 사용자는 생성/수정/삭제하고 싶은 인프라 자원을 코드로 작성합니다. 그리고 사용자가 IAC도구를 실행하면, IAC도구가 코드를 해석해석 인프라 자원을 반영시킵니다.

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>



그렇다면 코드로 어떻게 인프라를 생성/수정/삭제 작업을 할 수 있을까요? 정답은 인프라를 제공하는 환경에서 코드관리 기능을 지원하기 때문입니다. 모든 인프라를 코드로 관리할 수 없고 코드관리기능을 지원하는 환경에서만 사용가능합니다. 대표적으로 가상머신(VirtualBox 등), 클라우드(aws, gcp) 등이 있습니다.

> 개인적 의견으로는 가상화의 발전 때문에 IAC가 가능한 것 같습니다. 가상화는 물리적 구성을 논리적으로 분리하는 것을 말하는데요. cpu, 메모리, 네트워크 등을 논리적으로 다룰 수 있어 코드로 관리해보자라는 아이디어가 점점 발전된 것 같습니다.



## pros and cons

## IAC&#x20;

인프라의 변경내용이 모두 코드로 기록되는 특성으로 얻는 장점이 있습니다.

인프라를 전체를 일일이 찾지 않아도 코드를 보고 인프라를 추적할 수 있습니다. 그리고 같은 환경을 어려움 없이 똑같이 만들 수 있고 코드를 추가하여 확장성을 가질 수 있습니다. 특히 git과 같이 사용하면 버전관리를 할 수 있어 롤백 등 버전관리 작업이 편합니다.



물론 단점도 많이 있습니다. 대표적인 단점은 IAC를 적용하게 되면, 조직이 IAC도구를 공부하는 러닝커브가 있습니다. 그리고 긴급상황 등 수동으로 인프라를 고치는 경우, 코드로 인프라를 수정하지 않으므로 동기화가 깨집니다. 동기화가 어긋난 상황에서 IAC를 하게 되면 오류가 발생할 수 있습니다.

가장 큰 단점이 IAC를 사용하도록 조직문화를 만드는 것이라고 생각합니다. 코드 동기화를 유지하기 위해 모든 조직원이 IAC를 사용하도록 문화를 장려해야 하는데, 쉬울것 같지는 않습니다.



## What is Terraform

테라폼(Terraform)은 Hashicorp 회사에서 만든 IAC도구입니다. 무료로 사용할 수 있으며 약 7200개 이상의 인프라 자원을 코드로 관리할 수 있습니다.



테라폼을 사용하기 위한 용어를 몇 개 알아볼게요!.

\== provider

코드를 인프라로 변경해주는 모듈을 Provider이라고 합니다. 테라폼은 AWS Provider 등 매우 많은 Provider을 제공합니다. 각 Provider은 대상의 API를 이용하여 코드를 인프라로 반영합니다. 예를 들어 AWS Provider은 AWS API를 이용하여 코드를 인프라로 반영합니다.



\== provisioning

Provider가 코드를 인프라로 반영되는 과정을 provisioning라고 합니다.

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption><p>참고자료: <a href="https://www.terraform.io/intro">https://www.terraform.io/intro</a></p></figcaption></figure>



\== HCL과 tf파일

테라폼이 작업할 코드는 HCL라는 언어로 작성됩니다. 그리고 코드는 tf확장자를 갖는 파일로 저장됩니다.

<figure><img src="../../.gitbook/assets/image (3) (2).png" alt=""><figcaption><p>참고자료: <a href="https://www.terraform.io/language">https://www.terraform.io/language</a></p></figcaption></figure>



