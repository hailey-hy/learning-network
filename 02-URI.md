### 1. URI

- Uniform Resource Identifier
    - Uniform → 리소스를 식별하는 통일된 방식
    - Resource → 자원, URI로 식별할 수 있는 모든 것 (제한 x)
    - Identifier → 다른 항목과 구분하는 데 필요한 정보
- URL(Resource Locator), URN(Resource Name)을 포함하는 개념

### 2. URL

- 요약
    - scheme://[userinfo@]host[:port][/path][?query][#fragment]
    - https://www.google.com:443/search?q=hello&hl=ko
    - 프로토콜, 호스트명, 포트 번호, 패스, 쿼리 파라미터
- 구조
    1. scheme
        - 주로 프로토콜 사용
            - 프로토콜: 어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙
            - ex) http, https, ftp
        - http는 80 포트, https는 443 포트를 주로 사용, 포트는 생략 가능
        - https는 http에 보안 추가된 것
    2. host
        - 호스트명
        - 도메인명 또는 IP 주소를 직접 사용 가능
    3. port
        - 접속 포트
        - 일반적으로 생략, 생략시 http는 80, https는 443
    4. path
        - 리소스 경로
        - 계층적 구조로 이루어져 있음
            - /home/file1.jpg
    5. query
        - key=value 형태
        - ?로 시작, &로 추가 가능
        - 모두 문자열로 넘어감
    6. fragment
        - html 내부 북마크 등에 사용
        - 서버 전송 x
    
    ### 3. 웹 브라우저 요청 흐름
    
    <aside>
    💡 https://www.google.com:443/search?1=hello&hl=ko를 요청했을 때
    
    </aside>
    
    1. 요청
        1. DNS 조회
            - IP와 포트 정보 탐색
        2. HTTP 요청 메시지 생성
            - GET /search?1=hello&hl=ko HTTP/1.1 ...
        3. 소켓 라이브러리를 통해 전달
            1. TCP/IP 연결(ip, port)
            2. 데이터 전달
        4. TCP/IP패킷 생성, HTTP메시지 포함
            - https://www.google.com:443/search?1=hello&hl=ko
    2. 응답
        1. 응답 TCP/IP 패킷 생성
            - HTTP 응답 메시지 포함