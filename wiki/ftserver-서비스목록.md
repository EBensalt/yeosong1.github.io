## ft_server 서비스 목록 

### [도커 명령어 모음](도커-명령어-모음)
### Dockerfile
### Nginx
* Igor Sysoev가 개발
* 목적: apache의 C10K 문제(한 시스템에 동시접속자 수가 1만명이 넘을 때)를 해결하기 위해
* 특징: 비동기, Event-Driven 구조로 만든 웹 서버 SW.
* 사용 예 : OSI 7 6레벨인 Presentation Layer에서 NGINX 같은 웹 서버가 HTTP 통신을 제공하게 된다.

### Debian Buster
* 데비안 GNU/리눅스라고도 알려져있다.
* 커뮤니티에서 개발한 무료 & 오픈소스 소프트웨어로 구성된 Linux 배포판이다.

### Wordpress
* 컨텐츠 관리 시스템
* MySQL 또는 MariaDB 데이터베이스와 쌍을 이루는 무료 오픈 소스 컨텐츠 관리 시스템.
* 플러그인 아키텍처 및 템플릿 시스템(WordPress 내에서 테마라고 함)이 포함된다.

### Phpmyadmin
* 웹 페이지로 접속 가능한 DB관리툴.
* 데이터베이스 클라이언트(= 웹브라우저처럼 데이터베이스 브라우저 같은 것이고 데이터베이스 그 자체는 아님)의 한 종류.
* MySQL 및 MariaDB를위한 무료 오픈 소스 관리 도구.
* 웹 호스팅 서비스를 위한 가장 인기있는 MySQL 관리 도구 중 하나.
* 설치 전 php와 MariaDB가 설치되어 있어야 한다. 

### MySQL
* 데이터베이스 시스템의 한 종류.
* 관계형 [데이터베이스](데이터베이스).
* 데이터베이스를 GUI로 관리할 수 있는 무료 소프트웨어.

### SSL protocole (Secure Socket Layer)

* 서버<->클라이언트 간 인증에 사용.
* **암호화** 키를 송수신 한다.
* SSL은 보안과 성능상의 이유로 2가지 암호화 기법(대칭키, 공개키)을 혼용해서 사용

![231B4738590C36BE1E](https://user-images.githubusercontent.com/53321189/80220462-d671b980-867e-11ea-98ec-09e89c8163df.jpeg)
<br>(SSL 아키텍처 구조. [출처](https://12bme.tistory.com/80))

#### SSL 인증서
* 클라-서버간 통신을 제3자(CA - Certification Authority)가 보증해줄 수 있도록 전달되는 전자화된 문서


#### HTTP(Hypertext Transfer Protocol), HTTPS( " + Over Secure Socket Layer)
* HTTP: HTML 전송 통신규약.
  - **암호화 되지 않은** 방법으로 데이터를 전송
  - 서버-클라이언트 간 오고가는 메시지 감청이 매우 쉽다.
* HTTPS: SSL + HTTP = 보안이 강화된 HTTP.





### url, redirection
### auto index / disable




[데이터베이스란? - 생활코딩](https://opentutorials.org/course/195/1467)
[SSL 암호화에 대해](https://12bme.tistory.com/80)
