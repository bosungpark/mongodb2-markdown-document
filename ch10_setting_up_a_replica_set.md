Setting Up a Replica Set
=

Introduction to Replication
-
쓰기연산을 담당하는 하나의 primary와 primary와 싱크를 맞추는 다수의 secondaries로 구성된 집합. primary에 문제가 생기면 secondaries 중 하나가 새로운 primary로 선출될 수 있다.

primary와 secondaries를 선출하는 기준은 최초의 primary + secondaries이다. 만약 최초의 5개의 노드가 있고 그 중 3개가 죽었다면, 남은 2개의 과반을 충족한다고 해도 새로운 primary의 선출은 불가능하다. 왜냐하면 실제로 3개의 노드가 죽었는지는 신뢰할 수 없기 때문(network partition)이다. 이 경우라면 3개의 노드 사이에서도 선출이 일어날 수 있는데, 이는 primary가 두 개 선출될 위험이 있음을 암시한다.

How Elections Work
-
RAFT 합의 프로토콜에 기반을 두고 있다.
레플리카 셋의 멤버들은 heartbeats (pings) 매 2초마다 서로서로 보낸다.
만약 10초안에 응답이 오지 않으면 응답이 오지 않은 노드를 부적합하다 마킹한다.
이 알고리즘은 “best-effort”이다. 일반적으로 높은 우선순위를 가진 노드가 선출될 가능성이 높지만, 그렇지 않은 노드 역시 잠시동안은 선출 될 수 있다. 이 때 노드들은 높은 우선 순위의 노드가 선출될 때까지 지속적인 투표를 하게 된다.

몽고에는 Election Arbiters라는 특수 포지션의 노드가 있다. 이 노드는 오직 투표에만 역할을 하는 노드로 운영상 레플리카가 3개까지 불필요하다 느끼는 경우 사용할 수 있다. 단, Arbiter는 최대 1개를 사용하는 것이 좋다. 또 작은 레플리카 셋에서 데이터 노드와 Arbiter 사이에 고를 수 있는 선택지가 있다면 데이터 노드를 고르는 것이 좋다.
