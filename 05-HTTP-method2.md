### 1. 클라이언트에서 서버로 데이터 전송

- 종류
    1. 쿼리 파라미터를 통한 데이터 전송
        - GET
        - 주로 정렬 필터(검색어)
    2. 메세지 바디를 통한 데이터 전송
        - POST, PUT, PATCH
        - 회원 가입, 상품 주문, 리소스 등록, 리소스 변경
- 예시
    1. 정적 데이터 조회
        - 쿼리 파라미터 없이 리소스 경로로 단순 조회 가능
        - GET 사용
        - ex) 이미지, 정적 텍스트 문서
    2. 동적 데이터 조회
        - 주로 검색, 게시판 목록에서 정렬 필터 (검색어)
        - 조회 조건을 줄여주는 필터, 조회 결과를 정렬하는 정렬 조건에 주로 사용
        - GET 사용
        - GET이 쿼리 파라미터를 사용해 데이터를 전달
            - 실무 권장 x
    3. HTML form을 통한 데이터 전송
        - submit 시 POST로 전송
            - ex) 회원 가입, 상품 주문, 데이터 변경 등
        - Content-type: application/x-www.form-urlencoded 사용
            - form의 내용을 메시지 바디를 통해서 전송
            - key=value, 쿼리 파라미터 형식
            - 전송 데이터를 url encoding 처리
                - abc김 → abc%EA%B9%80
        - HTML은 GET, POST만 지원
        - Content-Type: multipart/form-data
            - 파일 업로드 같은 바이너리 데이터 전송시 사용
            - 다른 종류의 여러 파일과 폼의 내용 함께 전송 가능
                - 이름이 multipart인 이유
    4. HTTP API를 통한 데이터 전송
        - 활용처
            - 서버 → 서버
                - 백엔드 시스템 통신
            - 앱 클라이언트
                - 아이폰, 안드로이드
            - 웹 클라이언트
                - HTML에서 Form 전송 대신 자바 스크립트를 통한 통신에 사용(Ajax)
                - React, Vue.js 같은 웹 클라이언트와 API 통신
        - POST, PUT, PATCH: 메시지 바디를 통해 데이터 전송
        - GET: 조회, 쿼리 파라미터로 데이터 전달
        - Content-Type: applicaiton/json을 주로 사용(사실상 표준)
        - 회원 가입, 상품 주문, 데이터 변경

### 2. HTTP API 설계 개념

1. 문서
    - 단일 개념
        - ex) 파일 하나, 객체 인스턴스, 데이터베이스 row
        - /members/100, /files/star.jpg
2. 컬렉션 - POST
    - 클라이언트는 등록될 리소스의 URI를 모름
        - /memebers → PUT
    - **서버가 새로 등록된 리소스 URI를 생성**
    - 컬렉션이란?
        - 서버가 관리하는 리소스 디렉토리
        - 서버가 리소스의 URI를 생성하고 관리
        - 여기서 컬렉션은 /members
    - 대다수 경우에 사용
3. 스토어 - PUT
    - 클라이언트는 리소스 URI를 알고 있어야 함
        - 파일 등록 시 /files/{filename} → Put
        - PUT /files/star.jpg
    - **클라이언트가 직접 리소스의 URI를 지정**
    - 스토어란?
        - 클라이언트가 관리하는 리소스 저장소
        - 클라이언트가 리소스의 URI를 알고 관리
        - 여기서 스토어는 /files
4. 컨트롤러, 컨트롤 URI
    - 문서, 컬렉션, 스토어로 해결하기 어려운 추가 프로세스 실행
    - 컨트롤 URI
        - GET, POST만 지원하여 생기는 제약을 해결
        - 동사로 된 리소스 경로 사용
        - POST의 /new, /edit, /delete가 컨트롤 URI
        - HTTP 메서드로 해결하기 애매한 경우 사용