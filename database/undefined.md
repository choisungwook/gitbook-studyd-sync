# 공통기능

데이터베이스는 정말 많이 있지만, 엔지니어 관점에서 변하지 않는 공통기능들이 있습니다.&#x20;

* 어떻게 데이터를 저장할 것인지?
  * TSDB, RDMS, KEY/VALUE 등
* 데이터 분산 기능(샤딩)이 존재하는지?
* 어떻게 복제인스턴스(Replica)를 관리하는지?
* 장애극복(Failover)는 어떻게 하는지?
* 자체 라우터기능이 있는지?



<mark style="color:red;">데이터베이스가 다양할지라도 공통기능에 크게 벗어지 않으므로, 한개를 깊게 공부하면 다른 데이터베이스는 금방 배웁니다.</mark>
