# 명령어 정리

## Node 상태



## Node 목록 조회

```shell
curl -X GET http://127.0.0.1:9200/_cat/nodes
```

<figure><img src="../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

칼럼을 필터링하여 원하는 정보만 추출할 수 있습낟.

```sh
curl -X GET http://127.0.0.1:9200/_cat/_nodes?v=h=name,disTotal,diskUsed
```



## Cluster 상태

```shell
curl -X GET "http://127.0.0.1:9200/_cluster/health?wait_for_status=green&timeout=1s"
```



## Index 인덱스 생성

파라미터 예: primary shard 1개, replica shard 1개

```shell
curl -XPUT "http://localhost:9200/helloworld" -H 'Content-Type: application/json' -d'
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 1
  }
}'
```

<figure><img src="../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

## index 목록 조회

```shell
curl -X GET "http://127.0.0.1:9200/_cat/indices?v"
```

<figure><img src="../../.gitbook/assets/image (13) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Shard reroute

{% hint style="info" %}
참고자료: [https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-reroute.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-reroute.html)
{% endhint %}

파라미터 예: node1에 있는 test 인덱스를 node2로 이동

```shell
curl -X POST "localhost:9200/_cluster/reroute?pretty" -H 'Content-Type: application/json' -d'
{
  "commands": [
    {
      "move": {
        "index": "test", "shard": 0,
        "from_node": "node1", "to_node": "node2"
      }
    }
  ]
}
'
```



✅ 예: helloworld2 shard 0번을 master-3 -> master-4로 이동

<figure><img src="../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

## Document 생성&#x20;

```shell
curl -XPUT "http://localhost:9200/helloworld/_doc/1" -H 'Content-Type: application/json' -d'
{
  "message": "helloworld"
}'
```

<figure><img src="../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

## shard 목록 조회

```shell
curl -X GET "http://127.0.0.1:9200/_cat/shards?v"
```

<figure><img src="../../.gitbook/assets/image (12) (1) (1).png" alt=""><figcaption></figcaption></figure>



## shard 할당 에러 상황

shardㅇ할당에 대한 설명을 해주는 API입니다. 주로 할당에러가 발생했을 경우, 할당에러내용을 자세히 보고 싶을 때 사용합니다.

{% hint style="info" %}
참고자료: [https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-allocation-explain.html#cluster-allocation-explain](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-allocation-explain.html#cluster-allocation-explain)
{% endhint %}

```sh
curl -X GET _cluster/allocation/explain
```
