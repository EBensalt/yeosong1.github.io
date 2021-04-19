# 필수

## Check the code and ask questions

- [ ]  homebrew로 siege 설치 시작하기
    - `brew install siege`
    - siege는 트래픽 툴 (스트레스 테스터)
    
- [ ] select가 어떻게 작동하는지 설명하기

~~~
select(int maxfd, fd_set *readset, fd_set *writeset, fd_set *exceptset, const struct timeval * timeout)
~~~

- select는 한 번에 여러 fd의 변동사항(이벤트) 발생을 감지하여 반환하는 함수
    1. 인자로 들어갈 내용 점검
      - 파일 디스크립터 설정 (fd_set *readset, fd_set *writeset, fd_set *exceptset)
          - 매크로 함수를 통해 관리
              - FD_ZERO
              - FD_SET
              - FD_CLR
              - FD_ISSET
      - 검사 범위 지정 (int maxfd)
          - 자료형 fd_set은 비트 단위(0과 1)로 이루어진 배열
          - fd는 순서대로 지정되므로 가장 큰(가장 마지막에 생성된) fd값 + 1을 maxfd의 값으로 설정하면 된다.
      - 타임아웃 설정 (const struct timeval * timeout)
    2. select 함수 호출
    3. 호출 결과 확인
      - 관찰 중인 파일 디스크립터에 변화가 생겨야 반환한다 / 변화는 없지만 타임아웃 시간이 모두 지났다면 반환한다.
      - 반환값 n > 0인 경우, n개 만큼 fd에 변화가 생겼음.
      - 뭔가를 할 준비가 된(이벤트가 감지된) fd 개수를 반환. / 그러니까 타임아웃으로 아무 일 없이 함수가 반환된 경우에는 0을 반환.

- [ ]  Ask if they use only one select and how they ve managed the server accept and the client read/write.

    모든 소스코드에서 하나의 select()함수만 사용하는지, 서버 수락 및 클라이언트 읽기 / 쓰기를 어떻게 관리했는지 설명하시오

- [ ]  the select should be in the main loop and should check fd for read and write AT THE SAME TIME, if not please give a 0 and stop the evaluation.

    select는 메인 루프에 있어야 하며 동시에 읽기 및 쓰기에 대해 fd를 확인해야 합니다.

- [ ]  There should be only one read or one write per client per select. Ask to show you the code that goes from the select to the read and write of a client.

    select 당 클라이언드 당 하나의 읽기 또는 쓰기만 있어야 합니다. select에서 클라이언트의 읽기 및 쓰기에 이르는 코드를 보여주고 설명하시오

    select 한 번 당 클라이언트는 읽기나 쓰기 하나의 동작만 할 수 있도록



- [ ]  Search for all read/recv/write/send on a socket and check that if an error returned the client is removed

    소켓에서 모든 읽기 / 수신 / 쓰기 / 전송을 검색하고 오류가 반환되면 클라이언트가 제거되었는지 확인
- [ ]  Search for all read/recv/write/send and check if the returned value is well checked. (checking only -1 or 0 is not good you should check both)

    모든 읽기 / 수신 / 쓰기 / 전송 을 검색하고 반환된 값이 잘 확인 되어있는지 확인하시오 (-1 or 0만 확인하는 것은 좋지 않고 둘 다 확인해야 함)

    read() > 0 or read() ==0 or read() < 0 일때 예외처리 다 할 것

- [ ]  If a check of errno is done after read/recv/write/send. Please stop the evaluation and put a mark to 0

    읽기 / 수신 / 쓰기 / 전송 함수에서 errno 확인이 완료된 경우 평가를 중지하고 0으로 표시하시오

- [ ]  Writing or reading ANY file descriptor without going through the select is stricly FORBIDDEN

## Configuration

- [ ]  In the configuration file check if you can do the following and test the result:

    구성 파일에서 다음을 수행할 수 있는지 확인하고 결과를 테스트 하시오

- [ ]  setup multiple servers with different port

    다른 포트로 여러 서버 설정 체크
- [ ]  setup multiple servers with different host name (use something like: curl --resolve example.com:80:127.0.0.1 http://example.com/)

    호스트 이름이 다른 여러 서버 설정 ex) curl —resolve example.com:80:127:0.0.1 http://example.com/)

- [ ]  setup default error page (try to change the error 404)

    기본 오류 페이지 설정(오류 404 변경 시도)

- [ ]  limit the client body (use curl -X POST -H "Content-Type: plain/text" --data "BODY IS HERE write something shorter or longer than body limit")

    체크

- [ ]  setup routes in a server to different directories

    서버에서 다른 프로젝트로 경로 설정

- [ ]  setup a default file to search for if you ask for a directory

    디렉토리를 요청할 경우 검색 할 기본 파일 설정

- [ ]  setup a list of method accepted for a certain route (ex: setup only HEAD on a route and use curl with and without option -X HEAD)


## Run the tester

- [ ]  RUN
- [ ]  Download the tester in attachments and run it. It should not fail.
    - 42 테스터기를 체크해보시오

## Check Headers

- [ ]  Open the RFC 7231 and check the list of header of the subject, ask questions about it.

    RFC 7231 문서 

    7230 ~ 7235, CGI 3275

- [ ]  Use a browser, open the network part of it and try to connect to the server with it

    웹브라우저 접속 되는지 체크 http://localhost:8080

- [ ]  Look at the request header and response header
    - **HOST, Content-Length, Transfer-Encoding 헤더는 RFC규약 무조건 지켜야함**
    - Last-Modified : 디렉토리 요청 들어올 때는 last-Modified 헤더 누락 하고 보내줌 (nginx가 이렇게 동작함)
- [ ]  Try wrong URL on the server
- [ ]  Try things

## Port issues

- [ ]  In the configuration file setup multiple port and use different website, use a browser to check that the configuration is working as expected and show the right website.


- [ ]  In the configuration try to setup the same port multiple times. It should not work.


- [ ]  Launch multiple server at the same time with different configuration but with common ports. Is it working? If it is working, ask why the server should work if one of the configuration isnt working. keep going


## Siege

호스트 ↔ 서버 : 통신에서 완벽하게 100 메세지를 주고 받을 수 없음

- [ ]  Use Siege to run some stress test.
- [ ]  You should be able to use siege indefinitly without restarting the server (look at siege -b)


- [ ]  Availability should be above 99.5% for a simple get on an empty page with a siege -b on that page


- [ ]  Check if there is no memory leak (메모리 누수 체크)

    leaks Webserv를 Webserv 프로세스 id로 체크

- [ ]  Check if there is no hanging connection (fd 제대로 닫혔는지 체크)


# Bonus Part
