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
* 커뮤니티에서 개발한 무료 & 오픈소스 소프트웨어로 구성된 Linux 배포판이다.

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
* `nginx`는 기본적인 마크업 언어 파일만 해석 가능, `php`파일은 해석 불가.(아파치는 가능)
* 그래서 `nginx + php` 조합에서는 `php-fpm`이 있어야 해석이 됨.
* 참고: `php-fpm` 설치만으로도 `php`도 설치됨.

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

# 🚧 공사중.......


## SSL protocole (Secure Socket Layer)

* 서버<->클라이언트 간 인증에 사용.
* **암호화** 키를 송수신 한다.
* SSL은 
보안과 성능상의 이유로 2가지 암호화 기법(대칭키, 공개키)을 혼용해서 사용

![231B4738590C36BE1E](https://user-images.githubusercontent.com/53321189/80220462-d671b980-867e-11ea-98ec-09e89c8163df.jpeg)
<br>(SSL 아키텍처 구조. [출처](https://12bme.tistory.com/80))

### SSL 인증서
* 클라-서버간 통신을 제3자(CA)가 보증해줄 수 있도록 전달되는 전자화된 문서
* CA(Certification Authority): 공개키 소유자의 신뢰성을 인증(검증)해주는 제3자 인증기관.
* Authentication = 인증, Certification = 자격 검증

| 사용시 표기 | 의미 | 내용 |
|:---|:---|:---|
| CN | Common Name | 일반 이름 (인증서 고유 이름).<br>대부분의 인증기관 CA에서는 SSL인증서 신청시에 도메인명을 CN으로 지정.|
| O | Organization | 기관명 |
| OU | Organization Unit | 회사/기관 내의 '사업부, 부문, 부서, 본부, 과, 팀' 정도. |
| L | City/Locality | 시/도 |
| S | State/County/Region | 구/군 |
| STREET | Street | 나머지 상세 주소. (OV,EV 인증시에만 필요) |
| C | Country | 국가를 나타내는 ISO 코드를 지정. 한국은 KR, 미국은 US 등 2자리 코드 |



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



[암호화 이것만 알면 된다.](https://www.slideshare.net/ssuser800974/ss-76664853)<br>
[데이터베이스란? - 생활코딩](https://opentutorials.org/course/195/1467)<br>
[SSL 암호화에 대해](https://12bme.tistory.com/80)<br>
[SSL 신청시 CSR (Certificate Signing Request) 생성 항목](https://www.securesign.kr/guides/kb/56)<br>
[NginX SSL 인증서 설치/적용 가이드](https://www.securesign.kr/guides/NGINX-SSL-Certificate-Install)

