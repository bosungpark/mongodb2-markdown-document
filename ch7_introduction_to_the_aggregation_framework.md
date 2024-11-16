Introduction to the Aggregation Framework
=
몽고 디비는 aggregation framework를 이용한 분석 환경을 제공한다.

Pipelines, Stages, and Tunables
-
aggregation framework란 몽고에서 분석을 도와주기위한 도구를 모아놓은 것을 의미한다.
pipeline이라는 컨셉에 기반을 두고 있는데, pipeline을 사용하면 몽고에서 입력받은 데이터를 토대로 여러 스테이지를 거쳐 각각의 스테이지마다 데이터에 대해 다른 행동을 하도록 할 수 있다.
documents의 스트림을 입력으로 받아 출력을 내고, 그 출력은 다음 스테이지의 입력이 된다.

<img width="512" alt="스크린샷 2023-12-28 오후 10 59 04" src="https://github.com/bosungpark/mongodb2-markdown-document/assets/81157873/6547e9f1-9548-43c1-9b81-36616e479b8f">

pipeline의 각각의 스테이지는 데이터를 생성하는 유닛이다. 

<img width="511" alt="스크린샷 2023-12-28 오후 11 01 41" src="https://github.com/bosungpark/mongodb2-markdown-document/assets/81157873/b9daf8bd-b291-4d3f-96b9-ca999f562967">

각각의 스테이지는 knobs 혹은 tunables 셋을 제공하는데 이를 이용하면 작업에 대해 스테이지를 파라미터라이즈하는 것이 가능하다.
이런 tunable들은 연산자의 형태를 가진다.

또 각각의 스테이지는 재활용이 가능하다.

<img width="503" alt="스크린샷 2023-12-28 오후 11 19 55" src="https://github.com/bosungpark/mongodb2-markdown-document/assets/81157873/e01fa073-6221-4baa-afc0-8915d8debb5a">

Stages Operations
-
1. match: {$match: {founded_year: 2004}}
2. project: {$project: {founded_year: 1}}
3. limit: {$limit: 5},
4. sort: ...
5. skip: ...

스테이지 사용의 예시:

```
db.companies.aggregate([
        { $match: { founded_year: 2004 } },
        { $sort: { name: 1} },
        { $limit: 5 },
        { $project: {
_id: 0, name:1}}
])
```




