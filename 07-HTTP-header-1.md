### 1. HTTP 헤더 개요

- 구조
    - field-name “:” OWS filed-value OWS
        - OWS: 띄어쓰기 허용
        - field-name은 대소문자 구문 없음
- 용도
    - HTTP 전송에 필요한 모든 부가정보
    - ex) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시 관리 정보 ..
    - 표준헤더가 너무 많음
    - 필요시 임의의 헤더 추가 가능
- 과거 (RFC2616)
    - HTTP 헤더
        - General 헤더: 메시지 전체에 적용되는 정보 (Connection: close)
        - Request 헤더: 요청 정보 (User-Agent: Moziilla/5.0)
        - Response 헤더: 응답 정보 (Server: Apache)
        - Entity 헤더: 엔티티 바디 정보 (Content-Type: text/html, Content-Length: 3423)
    - HTTP 바디
        - 메시지 본문은 엔티티 본문을 전달하는 데 사용
        - 엔티티 본문은 요청이나 응답에서 전달할 실제 데이터
        - 엔티티 헤더는 엔티티 본문의 데이터를 해석할 수 있는 정보 제공
            - 데이터 유형 ,데이터 길이, 압축 정보 등등
- 최신(RFC7230)
    - HTTP 바디
        - 메시지 본문을 통해 데이터 전달
        - 메시지 본문 = 페이로드
        - 표현은 요청이나 응답에서 전달할 실제 데이터
        - 표현 헤더는 표현 데이터를 해석할 수 있는 정보 제공
            - 데이터 유형, 데이터 길이, 압축 정보 등등
        - 표현 헤더는 표현 메타데이터와 페이로드 메시지를 구분해야 하지만, 생략

### 2. 표현

- 구성
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/47ddd88e-0367-44c1-987e-a4af6af748af/Untitled.png)
    
    - Content-Type
        - 표현 데이터의 형식
        - 미디어 타입, 문자 인코딩
        - ex) text/html; charset=utf-8
    - Content-Encoding
        - 표현 데이터의 압축 방식
        - 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
        - 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
        - ex) gzip, delfate, identity
    - Content-Language
        - 표현 데이터의 자연 언어
        - ex) ko, en, en-US
    - Content-Length
        - 표현 데이터의 길이
        - 바이트 단위
        - 전송 코딩을 사용하면 Content-Length 사용 불가
- 사용
    - 표현 헤더는 전송, 응답 둘 다 사용

### 3. 협상 Content Negotiation

- 협상이란?
    - 클라이언트가 선호하는 표현 요청
- 유형
    1. Accept
        - 클라이언트가 선호하는 미디어 타입 전달
    2. Accept-Charset
        - 클라이언트가 선호하는 문자 인코딩
    3. Accept-Encoding
        - 클라이언트가 선호하는 압축 인코딩
    4. Accept-Language
        - 클라이언트가 선호하는 자연 언어
- 사용
    - 협상 헤더는 요청시에만 사용
- 우선순위
    - Quality Values(q)값 사용
    - 0~1, 클수록 높은 우선순위
    - 생략하면 1
    - ex) Accept Language: ko-KR, ko;q=0.9, en-US;q=0.8, en;q=0.7
    - 구체적인 것이 우선함
        1. text/plain; format=flowed
        2. text/plain
        3. text/*
        4. */*

### 4. 전송 방식

1. 단순 전송
    - Content length를 지정
2. 압축 전송
    - Content Encoding을 추가하여 무엇으로 압축했는지 표시
    - 절반 이상 줄어듦
3. 분할 전송
    - 덩어리처럼 바이트 단위로 쪼개서 보냄
    - Content length를 넣으면 안됨
        - Content length를 예상할 수 없고, 어차피 보내는 정보의 바이트 수가 표시됨
4. 범위 전송
    - 클라이언트에서 범위를 지정해서 요청하면 서버가 범위 전송할 수 있음
        - 클라이언트
            
            ```java
            GET /event
            Range: bytes=1001-2000
            ```
            
        - 서버
            
            ```java
            HTTP/1.1 200 OK
            Content-Type: text/plain
            Content-Range: 1001-2000/2000
            
            asdasdasdasdasdaqweqasd1231
            ```
            

### 5. 일반 정보를 제공하는 헤더

1. From 
    - 의미
        - 유저 에이전트의 이메일 정보
    - 특징
        - 일반적으로 잘 사용되지 않음
        - 검색 엔진 같은 곳에서 주로 사용
        - 요청에서 사용
2. Referer
    - 의미
        - 현재 요청된 페이지의 이전 웹 페이지 주소
    - 특징
        - A → B로 이동하는 경우 B를 요청할 때 Referer:A를 포함해서 요청
        - Referer를 사용해서 유입 경로 분석 가능
        - 요청에서 사용
        - 참고: referer는 단어 referrer의 오타

### 6. 특별한 정보를 제공하는 헤더

1. Host
    - 의미
        - 요청한 호스트 정보
    - 특징
        - 요청에서 사용
        - 필수
        - 하나의 서버가 여러 도메인을 처리해야 할 때
        - 하나의 IP주소에 여러 도메인이 적용되어 있을 때
    - 클라이언트 요청
        
        ```java
        GET/hello HTTP/1.1
        HOST: B.com
        ```
        
2. Location
    - 의미
        - 페이지 리다이렉션
    - 특징
        - 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면 Location 위치로 자동 이동
        - 201 (Created): Location 값은 요청에 의해 생성된 리소스 URI
        - 3xx (Redirection) : Location 값은 요청을 자동으로 리디렉션하기 위한 대상 리소스를 가리킴
3. Allow
    - 의미
        - 허용 가능한 HTTP 메서드
    - 특징
        - 405(Method Not Allowed)에서 응답에 포함해야 함
4. Retry-After
    - 의미
        - 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간
    - 특징
        - 503(Service Unavailable): 서비스가 언제까지 불능인지 알려줄 수 있음
        - Retry-After: Fri, 31 DEC 2022 23:59:59 GMT (날짜 표기)
        - Retry-After: 120 (초단위 표기)

### 7. 인증과 관련한 헤더

1. Authorization
    - 의미
        - 클라이언트 인증 정보를 서버에 전달
    - 특징
        - Authorization: Basic xxxxx
2. WWW-Authenticate
    - 의미
        - 리소스 접근시 필요한 인증 방법 정의
    - 특징
        - 401 Unauthorized 응답과 함께 사용
        - WWW-Authenticate: Newauth realm=”app”, type=1, title=”Login to \”apps\””, Basic realm=”simple”

### 8. 쿠키

- 종류
    - Set-Cookie
        - 서버에서 클라이언트로 쿠키 전달(응답)
    - Cookie
        - 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달
- 필요성
    - HTTP는 무상태 프로토콜
        - 클라이언트와 서버가 요청과 응답을 주고 받으면 연결이 끊어진다
        - 클라이언트가 다시 요청하면 서버는 이전 요청을 기억하지 못한다
        - 클라이언트와 서버는 서로 상태를 유지하지 않는다
    - 모든 요청에 사용자 정보를 포함하면 보안, 비용 등 단점이 많음
- 특징
    - 예
        - set-cookie: sessionId=abced1234; expires=Sat, 26-Dec-2020 00:00:00 GMT; path=/; domain=.google.com; Secure
    - 사용자 로그인 세션 관리, 광고 정보 트래킹에 사용
    - 모든 요청에 쿠키 정보를 자동으로 포함
        - 네트워크 트래픽 추가 유발
        - 최소한의 정보만 사용 (세션 id, 인증 토큰)
        - 서버에 전송하지 않고, 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지 참고
    - 주의! 보안에 민감한 데이터는 저장하면 안됨
- 생명주기
    - Set-Cookie: expires=Sat, 26-Def-2020 04:39:21 GMT
        - 만료일이 되면 쿠키 삭제
    - Set-Cookie: max-age=3600(초)
        - 0이나 음수를 지정하면 쿠키 삭제
    - 세션 쿠키의 경우
        - 만료 날짜를 생략하면 브라우저 종료까지만 유지
    - 영속 쿠키의 경우
        - 만료 날짜를 입력하면 해당 날짜까지 유지
- 도메인
    - 도메인을 지정하여 해당 도메인에 요청시에만 쿠키를 포함시킬 수 있음
    - 명시할 경우
        - 명시한 문서 기준 도메인 + 서브 도메인 포함
    - 생략할 경우
        - 현재 문서 기준 도메인만 적용
- 쿠키
    - 경로를 포함한 하위 경로 페이지만 쿠키 접근 가능
    - 일반적으로 path=/루트로 지정
- 보안
    - Secure
        - 원래 쿠키는 http, https를 구분하지 않고 전송
        - Secure를 적용하면 https인 경우에만 전송
    - HttpOnly
        - XSS 공격 방지
        - 자바스크립트에서 접근 불가
        - HTTP 전송에만 사용
    - SameSite
        - XSRF 공격 방지
        - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송