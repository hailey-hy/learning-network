### 1. HTTP API

- API URI 설계
    - 리소스 식별, URI 계층 구조 활용
        - 리소스와 리소스를 대상으로 하는 행위를 분리해서 생각하기
        - ex) 리소스: 회원
        행위: 조회, 등록, 삭제, 변경
        - 리소스는 명사, 행위는 동사

### 2. HTTP 메서드

- 주요 메서드
    - GET: 리소스 조회
    - POST: 요청 데이터 처리, 주로 등록에 사용
    - PUT: 리소스를 대체, 해당 리소스가 없으면 생성
    - PATCH: 리소스 부분 변경
    - DELETE: 리소스 삭제
- 기타 메서드
    - HEAD: GET과 동일, 메시지 부분을 제외하고 상태 줄과 헤더만 반환
    - OPTIONS: 대상 리소스에 대한 통신 가능 옵션을 설명
    - CONNECT: 대상 자원으로 식별되는 서버에 대한 터널을 설정
    - TRACE: 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수행

### 3. HTTP 메서드: GET과 POST

1. GET
    - 리소스 조회
        - 캐싱이 가능하므로 조회를 할 땐 GET 메서드가 유리함
    - 서버에 전달하고 싶은 데이터는 query(쿼리 파라미터, 쿼리 스트링)
    - 메시지 바디를 사용해 데이터를 전달할 수 있지만, 지원하지 않는 곳이 많음
2. POST
    - 주로 전달된 데이터로 신규 리소스 등록, 프로세스 처리에 사용
    - 메시지 바디를 통해 서버로 요청 데이터 전달
    - 서버는 요청 데이터를 처리
        - 메시지 바디를 통해 들어온 데이터를 처리하는 모든 기능을 수행
    1. 새 리소스 생성 (등록)
        - 서버가 아직 식별하지 않은 새 리소스 생성
    2. 요청 데이터 처리
        - 단순히 데이터를 생성하거나 변경하는 것을 넘어서, 프로세스를 처리해야 하는 경우
        - 결과로 새로운 리소스가 생성되지 않을 수도 있음
            - 컨트롤 URI (동사인 메서드 등)
    3. 다른 메서드로 처리하기 애매한 경우
        - 애매할 땐 POST

### 4. HTTP 메서드: PUT, PATCH, DELETE

1. PUT
    - 리소스를 완전히 대체
        - 리소스가 있으면 대체
        - 리소스가 없으면 생성
    - 클라이언트가 리소스를 식별
        - 클라이언트가 리소스 위치를 알고 URI 지정
        - POST와의 차이점
2. PATCH
    - 리소스 부분 변경
3. DELETE
    - 리소스 제거

### 5. HTTP 메서드의 속성

1. 안전
    - 의미
        - 호출해도 리소스를 변경하지 않음
    - 해당 메서드
        - ex) get, head, option, trace
    - 한계
        - 로그가 쌓여 장애가 일어나는 것 까지는 고려하지 않음
2. 멱등
    - 의미
        - 1번 호출한 것과 n번 호출한 것이 결과가 같음
    - 해당 메서드
        - ex) get, put, delete, ...
        - post는 멱등하지 않음
            - 두 번 호출하면 같은 행위가 중복해서 발생할 수 있음
    - 활용
        - 자동 복구 메커니즘에 활용
            - 서버가 TIMEOUT 등으로 정상 응답을 주지 못했을 때, 클라이언트가 같은 요청을 다시 해도 되는가에 대한 판단 근거로써 사용 가능
    - 한계
        - 외부 요인으로 중간에 리소스가 변경되는 것 까지는 고려하지 않음
3. 캐시 가능
    - 의미
        - 응답 결과 리소스를 캐시해서 사용할 수 있음
    - 해당 메서드
        - ex) get, haed, post, patch
        - 실제로는 get, head만 사용
            - post, patch는 본문 내용까지 캐시 키로 고려해야 하므로 구현이 쉽지 않음