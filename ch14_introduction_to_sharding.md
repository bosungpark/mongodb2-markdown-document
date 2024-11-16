Introduction to Sharding
=

What Is Sharding?
-
샤딩(때때로 파티셔닝이라 불리는)은 기기마다 데이터를 나누는 방식을 이야기한다.
정보의 접근 정도, 지역에 따라 데이터를 최적화된 하드웨어에 위치시킬 수 있다는 장점이 있다.
샤딩의 목적은 거창한게 아니다. 데이터를 쪼개 비용 혹은 성능을 개선하고자 하는 것 뿐이다.
하지만 노드를 추가하거나 삭제하는 경우, 혹은 데이터 분표나 로드의 패턴이 변하는 경우 관리에 어려움이 생길 수 있다.
몽고는 오토 샤딩을 지원한다. 

primary shard는 샤딩을 구성하는 전체 레플리카 셋을 의미한다. (쓰기연산을 할 수 있는 레플리카 셋에서의 primary와는 다른 개념이다.)
primary shard는 랜덤으로 결정된다. distribute 설정을 하지 않으면 우선 여기에 데이터가 쌓인다.

데이터를 분산시키기 위해 특정 필드(신중하게 골라)에 인덱스를 걸고 필드를 shard key로 설정한다.
shard key를 이용해 데이터를 분산하고 targeted queries를 날려 접근한 노드를 특정할 수 있다.
여러 노드를 돌아 데이터를 가져와야한다면 scatter-gather 방식을 사용했다고 말한다.
샤딩이 되었다면 1개의 청크로 구성되었던 데이터들이 세분화된 청크로 나뉜 뒤, 각각의 노드로 배치된다.

<img width="590" alt="스크린샷 2024-01-03 오후 9 04 36" src="https://github.com/bosungpark/mongodb2-markdown-document/assets/81157873/8a45b750-9ba5-4fa9-8b31-b8d6fea86c0c">