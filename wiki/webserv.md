# Webserv 서브젝트

- 드디어 url이 왜 HTTP로 시작하는지 이해하는 순간이다.
- 요약: 나만의 HTTP 서버 만들기. 실제 HTTP RFC를 따르며, 실제 브라우저로 테스트 하게 될 것입니다. HTTP는 인터넷에서 가장 많이 쓰인 프로토콜입니다. 웹사이트에서 일하게 되지 않더라고 이것을 알게 되는 것은 유용할 것입니다.

- 이 책 좋네요 [네트워크 프로그래밍](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9788965400820&orderClick=LAG&Kc=) 아니네요 [윤성우](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9788996094036&orderClick=LEa&Kc=)가 더 좋네요

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

- [x] 프로그램 이름 webserv
- [x] Makefile
- [ ] 허용 가능 함수
  - [ ] malloc, free, write, open, read, close
  - [ ] mkdir, rmdir, unlink, fork, wait, waitpid, wait3, wait4, signal, kill, exit, getcwd, chdir,
  - [ ] stat 파일 상태를 조회, lstat, fstat
  - [ ] lseek, opendir, readdir, closedir, execve, dup, dup2, pipe, strerror, errno, gettimeofday, strptime, strftime, usleep
  - [ ] select, socket, accept, listen, send, recv, bind, connect, inet_addr, setsockopt, getsockname, fcntl
- [ ] C++98에 맞춰서 작성, C++98로 컴파일 가능해야함.
- [ ] rfc 7230 ~ 7235 (http 1.1)를 준수하는 조건부여야합니다. 아래의 헤더들만 구현하려면 됩니다.
  - [ ] Accept-Charsets
  - [ ] Accept-Language
  - [ ] Allow
  - [ ] Authorization
  - [ ] Content-Language
  - [ ] Content-Length
  - [ ] Content-Location
  - [ ] Content-Type
  - [ ] Date
  - [ ] Host
  - [ ] Last-Modified
  - [ ] Location
  - [ ] Referer
  - [ ] Retry-After
  - [ ] Server
  - [ ] Transfer-Encoding
  - [ ] User-Agent
  - [ ] WWW-Authenticate
- 원한다면 모든 헤더를 구현해도 됨
- nginx는 HTTP 1.1을 준수하며 헤더 및 응답 동작을 비교하는 데 사용할 수 있습니다.
- 논블로킹이어야하며, 클라이언트와 서버 사이의 모든 IO에 대해 select 1개만 사용해야합니다(listen 포함).
- select()는 읽기와 쓰기를 동시에 체크해야합니다.
- 서버를 절대 블로킹이 되면 안되고, 필요한 경우 클라이언트가 제대로 바운스되어야 합니다.
- select()를 통하지 않고 읽기 작업이나 쓰기 작업을 수행하지 마십시오.
- 읽기 또는 쓰기 작업 후 errno 값 확인은 엄격히 금지됩니다.
- A request to your server should never hang forever 서버에 대한 요청이 영원히 중단되지 않아야합니다.
- none이 주어질 경우, 당신의 서버에는 기본 오류 페이지가 있어야합니다.
- 프로그램이 누수 되어서는 안되며 터지지 않아야합니다 (모든 초기화가 완료되고 메모리가 부족한 경우에도 ?)
- CGI 이외의 용도(예 : php, perl, ruby 등..)에는 fork를 사용할 수 없습니다.
- "iostream" "string" "vector" "list" "queue" "stack" "map" "algorithm" "exception"에 있는 모든 것을 포함하고 사용할 수 있습니다.
- 프로그램은 인자로 config파일을 받거나, 기본 경로(default path)를 사용해야합니다.

~~~
Mac OS X는 write를 다른 유닉스 OS와 같은 방식으로 구현하지 않기 때문에 fcntl을 사용을 허가합니다.
다른 OS와 유사한 결과를 얻으려면 논블로킹 FD를 사용해야합니다.
~~~

~~~
당신이 non-blocking FD를 사용하고 있기 때문에 read/recv 또는 write/send 함수를 select 없이 사용하면 서버가 블로킹되지 않을 것입니다.
다시 한 번 알려드립니다. select를 통하지 않고 어느 FD에서든 read/recv 또는 write/send를 시도하는 것은 평가에서 0점 처리 됩니다.
~~~

~~~
fcntl은 다음과 같이만 사용할 수 있습니다: fcntl(fd, F_SETFL, O_NONBLOCK);
다른 모든 플래그는 금지됩니다.
~~~
