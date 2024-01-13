An Introduction to MongoDB Security
=

MongoDB Authentication and Authorization
-
authentication: 유저의 아이덴티티를 구분하는 것
authorization: 자원에 대한 유저의 접근 권한을 확인하는 것

몽고에서 authentication, authorization은 자동으로 생기는 것이 아니다.
반드시 명시적으로 설정해야한다.

**Authenticate에 사용되는 x.509 Certificate의 대략적인 플로우**

<img width="455" alt="스크린샷 2024-01-13 오후 10 42 13" src="https://github.com/bosungpark/mongodb2-markdown-document/assets/81157873/c82f600e-e66d-49b6-aaa7-47ff9063b223">



