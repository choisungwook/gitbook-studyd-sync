# 테라폼 설치

테라폼 설치는 간단합니다. 테라폼 바이너리를 다운로드 받으면 됩니다. 공식문서에 리눅스, 윈도우, 맥 운영체제에 테라폼 바이너리가를 설치하는 방법을 친절히 설명합니다. 저는 MAC을 사용하고 있어 homebrew로 설치 했습니다.

```
brew tap hashicorp/tap
brew install hashicorp/tap/terraform
```



테라폼이 정상적으로 설치되면 terraform 명령어를 실행할 수 있습니다. 아래 예제는 테라폼 바이너리 버전을 출력합니다.

```
terraform --version
```

<figure><img src="../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>
