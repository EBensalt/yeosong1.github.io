# ft_server 서비스 목록 
---------------------------------------------------
## Docker
컨테이너 기반의 오픈소스 가상화 플랫폼.
* 가상 머신 비슷한 건데
* 호스트 OS를 덮지 않고 그보단 더 작은 영역에 빌드해서
* 가상 머신보다 가볍고 빠르고 간편한 것이 특징.
* [도커 자주 쓰는 명령어](도커-명령어-모음)

### Dockerfile
* **도커 이미지 빌드 자동화를 위한 지시문 세트**가 들어가는 파일. 메이크파일 비슷..
* [Dockerfile 문법](ft_server도커파일문법)

## Debian Buster
* OS 중 하나
* 데비안 GNU/리눅스라고도 알려져있다.
* 커뮤니티에서 개발한 무료 & 오픈소스 소프트웨어로 구성된 Linux 배포판 중 하나이다.

## Nginx
* 웹 서버 중 하나.
* 개발된 목적: apache의 C10K 문제(한 시스템에 동시접속자 수가 1만명이 넘을 때)를 해결하기 위해
* 특징: 비동기, Event-Driven 구조로 만든 웹 서버 SW.
* 사용 예 : OSI 7 6레벨인 Presentation Layer에서 NGINX 같은 웹 서버가 HTTP 통신을 제공하게 된다.

## Php-fpm
빠른 php언어 번역기.
* **php**-**F**astCGI **P**rocess **M**anger
* **CGI**: Common Gateway Interface
  - 두 컴퓨터(서버-클라이언트) 사이의 `HTML`등의 언어를 양방향으로 번역해주는 것
* `nginx`는 기본적인 마크업 언어 파일(정적 페이지)만 해석 가능
* `nginx`는 `php`파일(동적 페이지 = 조건에 따라 변하는 페이지)은 해석 불가(아파치는 가능)
* 그래서 `nginx + php` 조합에서는 `php-fpm`이 있어야 해석이 됨
* 참고: `php-fpm` 설치하면 `php`도 설치됨.

## MySQL
* SQL 데이터베이스 중 하나.
* 관계형 [데이터베이스](데이터베이스).
* 데이터베이스를 GUI로 관리할 수 있는 무료 소프트웨어.

## Wordpress
* 오픈소스 CMS(Contents Management System) 중 하나.
* 웹 사이트(페이지)형태로 DB에 접근할 수 있는 컨텐츠 관리 시스템?
* 플러그인 아키텍처 및 템플릿 시스템(WordPress 내에서 테마라고 함)이 포함된다.

## Phpmyadmin
* DB 관리 도구 중 하나.
* 데이터베이스 클라이언트(= 웹브라우저처럼 데이터베이스 브라우저 같은 것이고 데이터베이스 그 자체는 아님)의 한 종류.
* 웹 사이트(페이지)형태로 접속 가능.
* 설치 전 php와 MariaDB 혹은 MySQL이 설치되어 있어야 한다. 

## SSL protocole (Secure Socket Layer)

* = TLS
* HTTP 위에서 돌아가는 프로토콜.
* 서버<->클라이언트 간 인증에 사용.
* **암호화** 키를 송수신 한다.
* SSL은 보안과 성능상의 이유로 2가지 암호화 기법(대칭키, 공개키)을 혼용해서 사용

### 암호화
* 예시 : love <--- 1234 ---> mpwf 
  - -> 암호화
  - <- 복호화
  - 1234 키

#### 대칭키
* 암호화하는 사람과 복호화(암호해독)하는 사람이 같은 1개의 키값을 사용하는 것.
* 단점: 키 전송 과정에서 키가 노출될 경우 누구나 해독 가능하다
 
#### 공개키
* 키가 2개.
* 비공개 키(프라이빗 키, 비밀 키, 개인 키 등으로도 부름) 1개 / 공개 키 1개로 이루어짐.
* A키로 암호화하면 B키로만 복호화 가능, B키로 암호화 하면 A키로만 복호화 가능.
* 공개키 사용 예시
  1. 배포자 A가 (비밀 키 - 공개 키) 한 쌍을 만든다.
  2. 회원 B,C,D 에게 공개 키만 준다. 
  3. 회원 B가 배포자 A에 중요한 내용을 담은 답장을 보내고 싶다 -> 받았던 공개 키를 사용해서 보내고 싶은 내용을 암호화 한다.
  4. 암호화한 내용을 배포자 A가 받는다. 사용된 공개키와 한 쌍인 비밀키를 사용해서 복호화 하여 읽는다.
    4-1. 배포자 A가 아닌 이가 중간에서 감청에 성공해도, 공개키와 한 쌍인 비밀 키를 갖고있지 않으므로 안전하다.
  
![231B4738590C36BE1E](https://user-images.githubusercontent.com/53321189/80220462-d671b980-867e-11ea-98ec-09e89c8163df.jpeg)
<br>(SSL 아키텍처 구조. [출처](https://12bme.tistory.com/80))

### SSL 인증서
* 역할
  * (클라이언트)내가 접속한 서버가 신뢰할 수 있는 서버임을 보장.
  * SSL 통신에 사용할 공개키를 나(클라이언트)에게 제공.
* 인증서에 담긴 것(자물쇠 누르면 볼 수 있음)
  * 서비스의 정보 (인증서를 발급한 CA, 서비스의 도메인 등)
  * 서버 측 공개키 (공개키의 내용, 공개키의 암호화 방법)
* 정의: 클라-서버간 통신을 제3자(CA)가 보증해줄 수 있도록 전달되는 전자화된 문서
* CA(Certification Authority)의 정의: 공개키 소유자의 신뢰성을 인증(검증)해주는 제3자 인증기관.
  - 용어 사용 유의 : Authentication = 인증, Certification = 자격 검증
  
진짜 CA의 인증을 받으려면 비용이 드니까 우리는 우리가 CA가 되면서 SSL을 쓸 텐데,<br>
그러면 자물쇠표시+https:// 이 부분에 빨간색 엑스표가 쳐진다.<br>
이는 이 사이트가 SSL을 쓰긴 쓰는데, 공인된 CA기관의 인증을 받은 것은 아니라는 뜻이다.

### HTTP, HTTPS
* **HTTP**
  - = Hypertext Transfer Protocol
  - = HTML 전송 통신규약.
  - **암호화 되지 않은** 방법으로 데이터를 전송
  - 서버-클라이언트 간 오고가는 메시지 감청이 매우 쉽다.
* **HTTPS**
  - = Hypertext Transfer Protocol Over Secure Socket Layer
  - HTTP + SSL = 보안이 추가된 HTTP.




## url, redirection
## auto index / disable 스위치



출처 및 참고한 사이트..
[암호화 이것만 알면 된다.](https://www.slideshare.net/ssuser800974/ss-76664853)<br>
[데이터베이스란? - 생활코딩](https://opentutorials.org/course/195/1467)<br>
[SSL 암호화에 대해](https://12bme.tistory.com/80)<br>
[SSL 신청시 CSR (Certificate Signing Request) 생성 항목](https://www.securesign.kr/guides/kb/56)<br>
[NginX SSL 인증서 설치/적용 가이드](https://www.securesign.kr/guides/NGINX-SSL-Certificate-Install)
[생활코딩 - HTTPS와 SSL 인증서](https://opentutorials.org/course/228/4894)

