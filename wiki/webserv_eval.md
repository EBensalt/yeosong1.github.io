# Webserv 

# 필수

## 코드 확인과 질문
- [ ]  homebrew로 siege 설치 시작하기
    - `brew install siege`
    - siege는 트래픽 툴 (스트레스 테스터)
    
- [ ] select가 어떻게 작동하는지 설명하기
    - [select](select.md) 
- [ ] select를 딱 한 번만 사용했는지, 어떻게 서버가 클라이언트의 read/write를 accept 하도록 관리했는지 설명하기
- [x] select는 메인 루프 안에 있어야하고, read와 write를 **동시에** 체크 해야합니다. 만약 그렇지 않다면 0점이고, 평가를 멈추세요.
- [ ] There should be only one read or one write per client per select. Ask to show you the code that goes from the select to the read and write of a client. select 한 번에 1 클라이언트가 하나의 읽기 또는 쓰기만 있어야 합니다. select에서 클라이언트의 읽기 및 쓰기에 이르는 코드를 보여주고 설명하시오?
- [ ] 소켓에서 모든 read/recv/write/send를 검색하고 오류가 반환시 클라이언트가 제거되는지 확인
- [ ] 모든 read/recv/write/send를 검색하고 반환된 값이 잘 확인 되어있는지 확인하시오 (-1이나 0만 확인하는 것은 좋지 않고, 둘 다 확인하세요)
    - read() > 0 or read() ==0 or read() < 0 일때 예외처리 다 할 것
- [ ]  If a check of errno is done after read/recv/write/send. Please stop the evaluation and put a mark to 0
    - 읽기 / 수신 / 쓰기 / 전송 함수에서 errno 확인이 완료된 경우 평가를 중지하고 0으로 표시하시오?
- [ ]  select를 통하지 않은 그 어떤 fd의 읽기나 쓰기도 엄격히 **금지**되어 있습니다.

## config

config 파일에서 다음을 수행할 수 있는지 확인하고 결과를 테스트 하시오:

- [ ] 여러 서버를 다른 포트로 설정
- [ ] 여러 서버를 다른 host name으로 설정 (이런 걸 써보세요: curl --resolve example.com:80:127.0.0.1 http://example.com/)
- [ ] default error 페이지 설정(404 에러를 변경하려고 해보세요)
- [ ] 클라이언트 body를 제한해보세요. (curl -X POST -H "Content-Type: plain/text" --data "BODY는 여기고 제한보다 짧거나 길게 뭔가 써보세요")
- [ ] 서버의 루트를 다른 디렉토리로 설정
- [ ] 디렉토리를 요청할 경우 검색할 default 파일 설정    
- [ ] 특정 루트를 accept할 메소드 리스트 설정 setup a list of method accepted for a certain route (ex: setup only HEAD on a route and use curl with and without option -X HEAD)

## 테스터 돌리기

- [ ] 서브젝트 첨부파일에 있는 테스터를 다운받고 돌려보세요. fail 뜨면 안됩니다.

## 헤더 확인

- [ ] RFC 7231를 열고 header에 관한 부분을 확인하고 그에 관해 질문하기
    - 7230 ~ 7235, CGI 3275
    - [어떤 분의 RFC 번역..](https://giju.gitbook.io/rfc7231/5.request-header-fields)
- [ ]  Use a browser, open the network part of it and try to connect to the server with it
    - 웹브라우저를 통해 접속 되는지 체크 http://localhost:8080
- [ ] 요청 헤더와 응답 헤더를 보세요
- [ ] 서버에 틀린 URL를 넣어보세요
- [ ] 이것 저것 해보세요.

## 포트 이슈

- [ ] config에서 여러 포트/다른 웹사이트를 설정해보고, 브라우저를 이용해서 config가 예상한대로 작동하고 맞는 웹사이트를 보여주는지 체크하세요. In the configuration file setup multiple port and use different website, use a browser to check that the configuration is working as expected and show the right website. 

- [ ] config에서 같은 포트를 여러 번 설정해보세요. 작동되지 않아야 합니다. In the configuration try to setup the same port multiple times. It should not work.
    - 한 config 파일 내에 있는 여러 서버 블록의 포트 번호를 같은 번호로 바꾼 뒤 테스트 해봄.

- [ ] 여러 서버를 config로, common 포트에 동시에 시작해보세요. 되나요? 된다면, 왜 서버가 작동해야하는지 물어보세요. Launch multiple server at the same time with different configuration but with common ports. Is it working? If it is working, ask why the server should work if one of the configuration isnt working. keep going
    - REUSEPORT 썼으면 대기에 기다리면서 될 것이고,  REUSEADDR 썼으면 안될 것임..


## Siege

- [ ] 몇가지 스트레스 테스트를 위해 Siege를 사용하세요
    - `siege -b 127.0.0.1:8080` 등
- [ ] 메모리 누수가 없는지 확인하세요
    - `-fsanitize=address -g3`
- [ ] 행잉 커넥션이 없는지 확인하세요. -- fd 제대로 닫혔는지 체크?
    - `lsof -Pni4 | grep LISTEN` 연결상태인 포트 확인, `ps`로 켜져있는 프로세스도 확인
- [ ] 서버를 다시 시작하지 않고 무기한으로 siege를 사용할 수 있어야합니다 (siege -b 참조).
- [ ] 해당 페이지에서 siege -b를 사용하여 간단한 GET을 빈 페이지에 적용하면 가용성Availability이 99.5 % 이상이어야합니다. Availability should be above 99.5% for a simple get on an empty page with a siege -b on that page


# Bonus Part
## 로더블플러그인
## 워커스
## config file
