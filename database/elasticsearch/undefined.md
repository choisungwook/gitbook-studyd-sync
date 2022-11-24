# 명령어 정리

## Node 상태



## Node 목록 조회

```shell
curl -X GET http://127.0.0.1:9200/_cat/nodes
```

<figure><img src="../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

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



예: helloworld2 shard 0번을 master-3 -> master-4로 이동

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
