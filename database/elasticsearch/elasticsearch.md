# 쿠버네티스에 ElasticSearch 설치

## 설치 종류

쿠버네티스에서 ElaseticSearch를 설치하는 방법은 3가지가 있습니다. 1->3번 순서로 쿠버네티스 이해도가 많이 요구됩니다.

1. yaml
2. helm Chart
3. ECK Operator



## Helm 차트 설치

Helm Chart는 ElasticSearch기업에서 공식으로 제공합니다. pod는 Statefulset으로 관리되므로 Dynamic Provisioning옵션이 설정되어야 설치가 편합니다. 아래 예제 설치명령어는 openebs StorageClass를 설정하여 Dynamic Provisioning를 사용했습니다.

```shell
helm upgrade --install -n elasticsearch --create-namespace --version 7.8 \
--set replicas=3 \
--set imageTag=7.10.2 \
--set volumeClaimTemplate.storageClassName=openebs-hostpath \
elasticsearch-test elastic/elasticsearch
```



### 주의사항

1. ElasticSearch 컨테이너 이미지 태그와 helm version이 비슷해야 합니다. 버전 차이가 심하다면 helm chart설정과 컨테이너 이미지 설정이 안맞아 설치가 실패할 수 있습니다.
2. <mark style="color:red;">최소 쿠버네티스 worker 노드가 3개 필요</mark>합니다. 장애대비를 위해 각 노드당 1개씩 pod가 생성되고 총 3개 pod가 생성됩니다. 같은 노드에 pod를 여러개 생성하고 싶으면 antiAffinity=soft설정을 오버라이딩 하시면 됩니다.&#x20;
3. statefulset replica를 수정하면 기존 pod가 재실행됩니다. 엘라스틱서치 Master목록 설정때문에 재실행됩니다.
4. Data node replica를 줄인다면, 줄일 수있는지 확인 후에 줄이시길 바랍니다.

