# Dockerfile 버전선택

{% embed url="https://youtu.be/WfhnFLb1tg8" %}

## Dockerfile이란?

Dockerfile은 Docker build명령어를 실행할 때 필요한 파일이로서, **도커 데몬이 어떻게 빌드 할지를 정의한 문서**입니다. 도커 아키텍처는 도커 클라이언트와 도커 데몬으로 분리되어 있습니다. 사용자가 docker build명령어를 실행하면 실제 빌드는 도커 데몬이 실행합니다. 그리고 빌드 내용이 있는 문서가 Dockerfile입니다.

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>



## Dockerfile 버전이란? <a href="#head2" id="head2"></a>

**Dockefile도 소프트웨어버전 처럼 버전이 존재합니다. 버전에 따라 지원되는 문법이 다릅니다.** Dockerfile은 1.0부터 지금까지 꾸준히 업데이트 되고 있습니다.

2021 도커 웨비나에서 발표된 자료에 따르면, Dockefile 버전관리에 대한 문서는 비공식적으로 관리되고 있는 것 같습니다. 그래서 Dockerfile 버전에 대한 자료가 많이 업는 것 같습니다. 😭😭

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>출처: 2021년 docker 웨비나( https://youtu.be/Y5zuyTZg_Kw )</p></figcaption></figure>



추측으로는 도커데몬의 버전에 따라 Dockerfile버전도 결정되는 것 같습니다. 그러므로 도커 버전에 docker build가 성공할 수 있고 실패할 수 있습니다. 어떤 PC에서는 잘 되는 docker build가 다른 PC에서 docker build가 에러 날 수 있습니다. 물론, OS버전과 하드웨어도 고려해야 합니다.



## Buildkit의 등장 <a href="#head3" id="head3"></a>

**docker build의 한계가 있어 새로운 빌드엔진을 개선**하는 프로젝트가 여러 등장했습니다. 그 중 buildkit 프로젝트가 사람들에게 많이 알려졌고, 도커 18.09버전부터 buildkit을 사용하여 docker build명령어를 수행합니다.

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption><p>출처: https://kubesimplify.com/the-secret-gems-behind-building-container-images-enter-buildkit-and-docker-buildx</p></figcaption></figure>



buildkit이 도커와 호환되면서 Dockerfile을 문서를 dockerhub에 공식?으로 관리하고 있습니다.

* 링크: [https://hub.docker.com/r/docker/dockerfile](https://hub.docker.com/r/docker/dockerfile)

&#x20;

buildkit을 사용하여 빌드를 수행하면, 도커데몬을 사용할 때 보다 빌드 최적화를 수행할 수 있습니다. 병렬 빌드, 멀티 플랫폼, bind mount 등 도커데몬에 없는 기능을 사용하여 빌드 성능을 향상 시킵니다.



## Dockerfile 버전 선택 <a href="#head4" id="head4"></a>

**Dockerfile 버전 선택은 syntax문법을 사용**합니다. buildkit을 사용하는 도커데몬에만 syntax문법이 적용됩니다. buildkit을 사용하지 않으면 무시됩니다.

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption><p>출처:https://docs.docker.com/engine/reference/builder/#syntax</p></figcaption></figure>



dockerfile 버전은 도커 이미지로 관리됩니다. 그래서 도커 데몬 버전을 수정하지 않아도, 도커 이미지만 교체하여 쉽게 dockerfile 버전을 교체할 수 있습니다.

&#x20;

docker image는 docker hub로 관리되므로 syntax문법을 사용할 경우 dockerhub와 통신이 가능해야 합니다.

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption><p>출처:https://hub.docker.com/r/docker/dockerfile</p></figcaption></figure>



## (부록) buildkit 빌드 과정 <a href="#head5" id="head5"></a>

buildkit은 백엔드와 프론트엔드 아키텍처 구조로 되어 있습니다. 저희가 말하는 buildkit은 백엔드에 해당하고 dockerfile이 buildkit이 이해하는 포맷으로  변경하는 컴퍼넌트가 프론트엔드에 해당합니다.

<figure><img src="../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>



buildkit은 Dockerfile에 정의한 문법을 이해하지 못하고 LLB라는 포맷을 이해합니다. LLB를 해석하고 빌드 명령어를 수행합니다.

<figure><img src="../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>



LLB는 low level build definition format으로서 buildkit이 이해하는 중간포맷입니다.

<figure><img src="../.gitbook/assets/image (39).png" alt=""><figcaption><p>출처: https://github.com/FernandoMiguel/BuildKit/blob/master/README.md#llb</p></figcaption></figure>



LLB를 json으로 변경하면 아래와 같습니다.

<figure><img src="../.gitbook/assets/image (43).png" alt=""><figcaption><p>출처: https://kubesimplify.com/the-secret-gems-behind-building-container-images-enter-buildkit-and-docker-buildx</p></figcaption></figure>



그러므로, dockerfile을 관리하는 dockerhub을 보면 frontend image라고 표현하고 있습니다.

<figure><img src="../.gitbook/assets/image (35).png" alt=""><figcaption><p>출처:https://hub.docker.com/r/docker/dockerfile</p></figcaption></figure>



zpdlqltltm
