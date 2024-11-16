Connecting to a Replica Set from Your Application
=

몽고 클라이언트는 stand‐alone과 레플리카 셋 모두와 통신하도록 만들어졌다.
레플리카 셋과 통신하는 경우에는 우선 프라이머리를 기준으로 명령이 이루어진다.

만약 프라이머리의 데이터가 다른 노드로 전이되기 전에 서버가 죽어 리커버의 과정을 통해 롤백이 이루어지는 경우에 프라이머리의 싱크가 맞지 않는 데이터는 유실되지 않는다.
프라이머리에만 존재하고 전파되지 못한 데이터는 특수한 롤백 파일에 저장되어 메뉴얼하게 관리할 수 있다.

Creating Other Guarantees
-
물론 전파가 적절히 되었다는 것을 사전에 보장하는 것이 더 좋으므로 writeConcern옵션을 사용할 수도 있다.
이 외에 읽기 성공 여부를 판단하기 전에, 충족해야할 조건을 세팅하는 방법은 다음과 같다.

1. Tag members by assigning them key/value pairs. The keys describe classifica‐ tions; for example, you might have keys such as "data_center" or "region" or "serverQuality". Values determine which group a server belongs to within a classification. For example, for the key "data_center", you might have some servers tagged "us-east", some "us-west", and others "aust".
2. Create a rule based on the classifications you create. Rules are always of the form {"name" : {"key" : number}}, where at least one server from number groups must have a write before it has succeeded. For example, you could create a rule {"twoDCs" : {"data_center" : 2}}, which would mean that at least one server in two of the data centers tagged must confirm a write before it is successful.

Sending Reads to Secondaries
-
일반적으로 읽기 요청은 프라이머리에 보내는 것(primary preferred)이 좋다.
이유는 아래와 같다.

1. Consistency Considerations: 강한 일관성(혹은 read your own write)을 필요로 하는 어플리케이션은 반드시 프라이머리에서 읽어야 한다. 
2. Load Considerations: 부하를 분산했을 때, 모든 노드가 강한 부하를 받게된다면 위험할 수 있다. 이 경우에는 샤딩을 사용하는 것이 좋다.
   
앞서 말한 경우에 해당하지 않는다면, 세컨더리에 읽기요청을 보내도 큰 문제는 없다.
인덱싱을 프라이머리와 다르게 하고 싶은 이유로 세컨더리를 사용하는 경우라면, 레플리카 셋 커넥션이 아니라 직접 맺어주어야 한다.
이러나저러나 큰 문제는 없지만 일관성 측면에서 프라이머리에서 읽는 것이 더 권장된다.(이게 싫고, 저지연의 읽기 쓰기를 원한다면 샤딩을 하면 된다.)
