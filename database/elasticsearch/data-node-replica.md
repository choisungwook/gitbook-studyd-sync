# Data node Replica 줄일 때 주의사항

{% embed url="https://youtu.be/d_GTfuwOUWM" %}

## Replica가 애플리케이션 영향을 어떻게 주는지

<mark style="color:red;">쿠버네티스가 아닐지라도 애플리케이션 운영에서 가장 중요한 것은 아키텍처 이해</mark>라고 생각합니다. 애플리케이션 아키텍처가 인스턴스 수정할 때 기존 환경에 영향 있다면, 쿠버네티스 쿠버네티스 또한 Replica를 수정할 때 영항이 있습니다. MSA 아키텍처, 분산 아키텍처 등이 대표적인 예입니다.



## 분산 아키텍처 예

분산 아키텍처의 대표적인 애플리케이션은 데이터베이스입니다. Redis, MongoDB, ElasticSearch, Hadoop 등이 해당됩니다.



분산 아키텍처를 가진 데이터베이스는 여러 인스턴스에 데이터를 분산해서 저장합니다. 분산된 데이터 단위를 보통 Shard라고 부르고 분산 알고리즘을 Sharding이라고 합니다.

## 엘라스틱서치 Replica 줄일 때, 장애 상황

엘라스틱서치를 예제로 들면 index의 데이터를 여러 data인스턴스에 shard로 쪼개어 저장합니다. 그리고 장애 극복을 위해 Replica(복제) Shard를 0개 이상 가집니다. 인스턴스가 장애나면 다른 인스턴스에 Replica shard가 복사되고 서비스가 정상화 됩니다.

<figure><img src="../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>



쿠버네티스 Replica를 줄이는 상황이고 가정해봅시다. <mark style="color:red;">Replica를 5개에서 3개로 줄입겁니다. 우연히 Primary Shard와 Replica Shard가 종료되는 pod에 있으면 어떻게 될까요?</mark>

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>



<mark style="color:red;">무한히 기다리는 데드락 상황에 빠져듭니다</mark>. 장애극복을 위해 종료되는 pod의 데이터를 옮기려고 시도하겠지만, Replica Shard가 있는 pod도 종료되기 때문에 무한정 기다리게 됩니다. Shard상태를 조회하면 UNASSIGNED로 되어 있구요.

<figure><img src="../../.gitbook/assets/image (18) (1).png" alt=""><figcaption></figcaption></figure>



## 해결하려면?

2가지 방법이 있습니다.



<mark style="color:red;">첫 번째는, 관리자가 Replica 수정 과정에 개입하는 겁니다.</mark> 애플리케이션 공식 메뉴얼을 보면서 준비작업을 한 후, Replica를 수정하는 겁니다. 엘라스틱서치는 Shard Reroute 작업이 필요합니다.



<mark style="color:red;">두 번째는, 관리자가 개입하는 과정을 코드로 자동화합니다. 쿠버네티스는 놀랍게도 Operator</mark>라는 기능을 제공합니다. 관리자가 코드로 자동화할 수 있도록 인터페이스를 제공합니다. 엘라스틱서치는 ECK Operator라는 것을 제공하고 있습니다. 아직 ECK Operator를 사용하지 않아서 위 예제에 있는 문제가 해결되는지는 잘 모르겠네요



Operator를 개발하려면 쿠버네티스 이해, 쿠버네티스 코드 이해, 자동화 할려고하는 애플리케이션 이해, go언어 학습이 필요합니다.
