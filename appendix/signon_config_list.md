# 설정

다음은 Coinstack SignOn을 구동하기 위해 설정한 목록입니다.

| 설정 이름 | 의미 | 필수<br>유무 | 예제 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| coinstack.endpoint | Coinstack Node<br>Endpoint | 필수 | [http://localhost:3000](http://localhost:3000/) |
| jwt.privatekey | 토큰 생성을 위한 개인키 |  |  |
| signon.admin.address | 관리자 어드레스 | 필수 | 1KkM2NLZEK9SL4sChJVgg45k3godj7Tnix |
| signon.server.port | 서버 포트 |  | 8080 |
| signon.server.privatekeys | 서버가 사용하는 개인키들 | 필수 | L2CMbG8JqaAQpJDTgrmoeVq7fcpKnycCd9rQg5vCNBTxZFXNQC4T<br>KwacZMHwTX1p7VJ8mbc1KBnzQ7fyktA6SfoduogsErfihdwu1CTK |
| signon.server.token.validity | 토큰의 유효시간<br>단위는 초 |  | 600 |
| signon.server.user-repository | 사용자 정보에 접근하는<br>구현체의 클래스명 |  | coinstack.signon.repository.SimpleUserRepository |
| signon.peer.addresses | 다른 서버의 어드레스 |  | &nbsp; |

