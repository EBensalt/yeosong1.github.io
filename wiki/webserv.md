# Webserv 서브젝트

- 드디어 url이 왜 HTTP로 시작하는지 이해하는 순간입니다.
- 요약: 내 HTTP 서버 만들기. 실제 HTTP RFC를 따르며, 실제 브라우저로 테스트 하게 될 것입니다. HTTP는 인터넷에서 가장 많이 쓰인 프로토콜입니다. 웹사이트에서 일하게 되지 않더라도 이것을 알게 되는 것은 유용할 것입니다.


- **소켓 프로그래밍 개념 정리** 및 얼개 코드 만들기 [윤성우](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9788996094036&orderClick=LEa&Kc=)
  - 오직 1번의 select 함수 사용으로 모든 fd를 관리할 것
- **RFC** 문서 7231 -> 구현 필수 사항 및 문법 확인 -> 클래스에 변수 & 기능 구현
  - 요청 메시지 파서
- [**nginx config**](https://12bme.tistory.com/366) 참고하여 내 프로그램에서 사용할 config 파일 문법 결정
  - config 파서
- [**cgi**](https://velog.io/@seanlion/cgi) [RFC 3875](https://tools.ietf.org/html/rfc3875) 명세에서 메타변수 내용 확인
  - cgi란? 내부는 볼 수 없지만, 클라이언트가 요청 메시지로 보낸 파일을 서버가 받아서 cgi에 넣으면, 필요한 무슨 처리(프로그램 별로 다름)를 해서 서버에게 다시 뱉어주는 프로그램이다.
  - cgi 프로그램 실행하는 부분 만들기. 그냥 fork() execve() 한다.. (FastCGI는 데몬. 요청마다 새 프로세스를 만들고 제거하는 기존 CGI의 성능 저하 문제를 없앴다.)
- **상태**(상태코드)별 html 파일
- 각 부분 **에러** 처리 항목과 문구

- ([SO_REUSEPORT와 SO_REUSEADDR](http://forum.falinux.com/zbxe/index.php?document_srl=448747&mid=network_programming), [2](http://www.unixguide.net/network/socketfaq/4.11.shtml))
- ([string 함수들](std_string.md))

## 소개

HTTP (Hypertext Transfer Protocol)는 분산된 협업 하이퍼 미디어 정보 시스템을 위한 애플리케이션 프로토콜입니다.<br>
HTTP는, 이를테면 마우스 클릭 또는 웹 브라우저에서 화면을 탭하는 것처럼 사용자가 쉽게 액세스 할 수 있는 다른 리소스에 대한 하이퍼 링크가 하이퍼 텍스트 문서에 포함되어있는 World Wide Web 데이터 통신의 기초입니다.<br>
HTTP는 하이퍼 텍스트와 월드 와이드 웹을 용이하게하기 위해 개발되었습니다.<br>
웹 서버의 주요 기능은 웹 페이지를 저장, 처리 및 클라이언트에 전달하는 것입니다.<br>
클라이언트와 서버 간의 통신은 Hypertext Transfer 프로토콜 (HTTP)를 사용하여 발생합니다.<br><br>

전달되는 페이지는 HTML 문서(이미지, 텍스트, 스타일 시트, 스크립트를 포함할 수도 있는)가 가장 많다.

트래픽이 많은 웹 사이트에 여러 웹 서버를 사용할 수 있습니다.

유저 에이전트(일반적으로 웹 브라우저 또는 웹 크롤러)는 다음을 통해 통신을 시작합니다:
HTTP를 사용하여 특정 리소스를 요청하면 서버가 해당 리소스의 내용으로 응답하거나, 그렇게 할 수 없는 경우에는 오류 메시지로 응답합니다.
리소스는 일반적으로 서버의 보조 스토리지에 있는 실제 파일이지만, 반드시 그런 것은 아니며 웹 서버가 구현되는 방법에 따라 다르다.

주요 기능은 콘텐츠를 제공하는 것이지만, HTTP의 전체 구현은 클라이언트로부터 콘텐츠를 받는 방법도 포함됩니다. 이 기능은 파일 업로딩을 포함한 웹폼 제출에 사용됩니다.

## 필수 파트

------------------ 프로그램에 관한 내용 ------------------

- [ ] 프로그램 이름 webserv
- [ ] Makefile
- [ ] 허용 가능 함수
  - [ ] malloc, free, write, open, read, close
  - [ ] mkdir, rmdir, unlink, fork, wait, waitpid, wait3, wait4, signal, kill, exit, getcwd, chdir,
  - [ ] stat 파일 상태를 조회, lstat, fstat
  - [ ] lseek, opendir, readdir, closedir, execve, dup, dup2, pipe, strerror, errno, gettimeofday, strptime, strftime, usleep
  - [ ] select, socket, accept, listen, send, recv, bind, connect, inet_addr, setsockopt, getsockname, fcntl 파일 제어
- [ ] C++98에 맞춰서 작성, C++98로 컴파일 가능해야함.
- [ ] rfc 7230 ~ 7235 (http 1.1)를 준수하는 조건부여야합니다. 아래의 헤더들만 구현하려면 됩니다.
  - 일반 헤더 (요청, 응답에서 사용)
  - [ ] Date 메세지가 생성된 날짜
  - [ ] Transfer-Encoding
  - 엔터티 헤더 (요청, 응답에서 사용)
  - [ ] Content-Language
  - [ ] Content-Length
  - [ ] Content-Location
  - [ ] Content-Type
  - [ ] Last-Modified
  - 서버에서 보내는 응답 헤더
  - [ ] Allow ㅇㅇ에서 허용 가능한 메서드 목록. 허용 불가능한 메서드를 요청 받았을 경우 405 Method Not Allowed
  - [ ] [Location](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Location) 페이지를 리디렉션 할 URL을 나타낸다. (3xx redirection or 201 created인 경우)
  - [ ] [Retry-After](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Retry-After) ---
  - [ ] Server 서버의 소프트웨어 정보. 예) Server: Apache/2.2.22 (Debian)
  - [ ] WWW-Authenticate
  - 클라이언트에서 보내는 요청 헤더
  - [ ] Accept-Charsets
  - [ ] Accept-Language
  - [ ] Authorization
  - [ ] Host 하나의 서버에 도메인이 여러개일 경우, 어느 호스트에 요청하는 내용인지 명시
  - [ ] Referer 어느 웹페이지에서 여기로 왔는지 (현재 요청된 페이지의 이전 웹페이지 주소)
  - [ ] User-Agent 클라이언트의 어플리케이션 정보 (브라우저에서 사용하는 소프트웨어 이름이나 크롤링 봇, curl 등 요청한 소프트웨어의 이름)


- 원한다면 모든 헤더를 구현해도 됨
- nginx는 HTTP 1.1을 준수하며 헤더 및 응답 동작을 비교하는 데 사용할 수 있습니다.
- 논블로킹이어야하며, 클라이언트와 서버 사이의 모든 IO에 대해 select 1개만 사용해야합니다(listen 포함).
- select()는 읽기와 쓰기를 동시에 체크해야합니다.
- 서버를 절대 블로킹이 되면 안되고, 필요한 경우 클라이언트가 제대로 바운스되어야 합니다.
- select()를 통하지 않고 읽기 작업이나 쓰기 작업을 수행하지 마십시오.
- 읽기 또는 쓰기 작업 후 errno 값 확인은 엄격히 금지됩니다.
- A request to your server should never hang forever 서버에 대한 요청이 영원히 중단되지 않아야합니다?
- none이 주어질 경우, 당신의 서버에는 기본 오류 페이지가 있어야합니다.
- 프로그램이 누수 되어서는 안되며 터지지 않아야합니다 (모든 초기화가 완료되고 메모리가 부족한 경우에도?)
- CGI 이외의 용도(예 : php, perl, ruby 등..)에는 fork를 사용할 수 없습니다.
- "iostream" "string" "vector" "list" "queue" "stack" "map" "algorithm" "exception"에 있는 모든 것을 포함하고 사용할 수 있습니다.
- 프로그램은 인자로 config파일을 받거나, 기본 경로(default path)를 사용해야합니다.

~~~
정보) Mac OS X는 write를 다른 유닉스 OS와 같은 방식으로 구현하지 않기 때문에 fcntl을 사용을 허가합니다.
     다른 OS와 유사한 결과를 얻으려면 논블로킹 FD를 사용해야합니다.
~~~

~~~
주의) 당신이 non-blocking FD를 사용하고 있기 때문에 read/recv 또는 write/send 함수를 select 없이 사용하면 서버가 블로킹되지 않을 것입니다.
     다시 한 번 알려드립니다. select를 통하지 않고 어느 FD에서든 read/recv 또는 write/send를 시도하는 것은 평가에서 0점 처리 됩니다.
~~~

~~~
주의) fcntl은 다음과 같이만 사용할 수 있습니다: fcntl(fd, F_SETFL, O_NONBLOCK);
     (= fd의 플래그를 O_NONBLOCK로 세팅함)
     다른 모든 플래그는 금지됩니다.
~~~


------------------ config 파일에 관한 내용 ------------------

이 config 파일에서 우리는 아래의 것들을 할 수 있어야 함:

~~~
전구모양) nginx config 파일의 "server"파트에서 영감을 받으세요.
~~~

- [ ] 각 "서버"의 포트와 호스트 선택
- [ ] server_names를 설정 할지 말지 
- [ ] 첫번째 서버의 host:port는 이 host:port의 default가 될 것입니다. (그것은 다른 서버에 속하지 않는 모든 요청에 응답할 거라는 뜻) The first server for a host:port will be the default for this host:port (meaning it will answer to all request that doesn’t belong to an other server)

- [ ] default error 페이지 설정
- [ ] 클라이언트 body 크기 제한
- [ ] 아래의 규칙/config 중 하나 이상을 사용하여 경로 설정(라우트는 regexp 사용하지 않음):
  - 라우트에 대해 허용되는 HTTP 메서드 목록 정의 define a list of accepted HTTP Methods for the route
  - 파일을 검색해야하는 디렉토리 or 파일 정의(예를 들어 url /kapouet의 루트가 /tmp/www로 되어있으면, url /kapouet/pouic/toto/pouet 는 /tmp/www/pouic/toto/pouet다)
  - 디렉토리 목록 켜기, 끄기
  - 요청이 디렉토리일 경우 응답할 default 파일
  - 특정 파일 확장자에 기반한 CGI 실행 (예를 들면 .php)
    - CGI가 뭔지 궁금한가요? [https://en.wikipedia.org/wiki/Common_Gateway_Interface](https://en.wikipedia.org/wiki/Common_Gateway_Interface)로
    - cgi를 직접 호출하지 않기 때문에 전체 경로로 PATH_INFO를 사용하십시오.
    - chunked 요청의 경우 서버에서 해당 요청을 unchunked 해야하며 CGI는 EOF를 body의 끝으로 예상합니다.
    - CGI의 출력도 마찬가지입니다. CGI에서 content_length가 반환되지 않는 경우, EOF는 반환된 데이터의 끝을 의미합니다.
    - 프로그램은 다음 [메타 변수meta variables](메타변수.md)를 설정해야합니다.
      - AUTH_TYPE 
      - CONTENT_LENGTH
      - CONTENT_TYPE
      - GATEWAY_INTERFACE
      - PATH_INFO
      - PATH_TRANSLATED
      - QUERY_STRING
      - REMOTE_ADDR
      - REMOTE_IDENT
      - REMOTE_USER
      - REQUEST_METHOD
      - REQUEST_URI
      - SCRIPT_NAME
      - SERVER_NAME
      - SERVER_PORT
      - SERVER_PROTOCOL
      - SERVER_SOFTWARE
    - 당신의 프로그램은 첫 번째 인수에 요청된 파일로 cgi를 호출해야합니다.
    - cgi는 상대 경로 파일 액세스를 위한 올바른 디렉토리에서 실행되어야합니다.
    - 서버가 php-cgi와 함께 작동해야합니다.
  - 라우트가 업로드 된 파일들을 수락하고, save되어야할 곳에 configure 할 수 있도록 해야 합니다.

당신은 평가를 위해 몇 가지 config 파일을 제공해야합니다.

~~~
정보) 어떤 작동에 의문이 생기면 nginx의 작동과 비교해야합니다. 예를 들면 server_name이 어떻게 작동하는지를 확인한다든지..
~~~

~~~
주의) 이 프로젝트를 시작하기 전에 RFC를 읽고 telnet과 nginx로 몇 가지 테스트를 해보세요.
     ~~~~~~~~~~~~~~~~~~~~ ~~~~~~~~~ ~~~~~~~~~~~~~~~ ~~~~~~~~~~~~~~~~~~~

주의) 우리는 작은 테스터를 공유드렸습니다. 이 테스터 파일에서 실패한다면 평가 받기를 시도 하지 마세요.

주의) 중요한 것은 resilience 탄력성입니다. 서버는 절대 죽지 않아야합니다. 서버는 스트레스 테스트를 받게 될 것입니다.

주의) 하나의 프로그램만으로 테스트하지 말고 python, golang, javascript, 또는 rails와 같이 빠르게 작성/사용할 수 있는 언어로
     당신만의 테스트를 작성하세요. 심지어 C 또는 C++로 테스트를 수행 할 수도 있습니다.
~~~






## [Config 파일에 대해..](server_conf.md)

## RFC를 읽고 telnet과 nginx로 몇 가지 테스트를

## [실제 평가 준비](webserv_eval.md)
