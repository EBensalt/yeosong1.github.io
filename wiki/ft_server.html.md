---
published: true
layout: "git-wiki-bs-united"
---

# ft_server
요약: 이번 서브젝트는 [시스템 관리]입니다. Docker를 이해하고, 첫 웹 서버를 설정하게 됩니다.

## 소개
* 이 서브젝트는 시스템 관리를 소개합니다.
* 이는 스크립트를 사용하여 작업을 자동화하는 것의 중요성을 알려줄 겁니다.
  이를 위해, 당신은 "docker"기술을 이해하고 이를 이용해 완전한 웹 서버를 설치하십시오.
* 이 서버는 동시에 여러 서비스를 실행할 것입니다. : `Wordpress`, `phpMyAdmin`, `SQL database`.

## 일반 지침
* 서버 구성에 필요한 모든 파일을 srcs 폴더에 넣기!
* `Dockerfile`을 루트에 두기! 도커파일이 당신의 컨테이너를 빌드 할 것입니다. docker compose는 사용 금지.
* 워드프레스 웹사이트에 필요한 모든 파일은 srcs 폴더에 넣기!

## 필수 파트
* 딱 하나의 도커 컨테이너에 `Nginx`가있는 웹 서버를 설정해야합니다. 컨테이너 OS는 꼭 `debian buster`여야합니다
* 웹 서버는 여러 서비스를 동시에 실행할 수 있어야합니다. 그 서비스들은 `WordPress` 웹 사이트, `phpMyAdmin`, `MySQL` 입니다.
  `SQL 데이터베이스`가 `WordPress` 및 `phpMyAdmin`과 작동하는지 확인해야합니다.
* 서버는 `SSL 프로토콜`을 사용할 수 있어야합니다.
* URL에 따라 서버가 올바른 웹 사이트로 리디렉션되는지 확인해야합니다.
* 서버가 오토 인덱스로 실행 중인지 확인하고, 이를 비활성화 할 수 있어야합니다.


### Nginx
* 개발자 Igor Sysoev씨가
* apache의 C10K 문제(한 시스템에 동시접속자 수가 1만명이 넘을 때..)를 해결하기 위해
* 웹사이트 확장성을 위해 설계한 비동기, Event-Driven 구조로 만든 웹 서버 SW. 2004년 첫 배포.
* reverse proxy, load balancer, mail proxy, HTTP cache 로도 사용할 수 있는 웹 서버이다.
* 예시 : 개발된 응용 프로그램이 OSI 7 Layer 중 application Level에서 동작하고 그 아래 Level에서 NGINX 같은 웹 서버가 HTTP 통신을 제공하게 된다.

### Debian Buster
* 데비안 GNU/리눅스라고도 알려진 데비안은 커뮤니티에서 개발한 무료 & 오픈소스 소프트웨어로 구성된 Linux 배포판입니다.

### Wordpress
* Content Management System.
* PHP로 작성 되었음.
* MySQL 또는 MariaDB 데이터베이스와 쌍을 이루는 무료 오픈 소스 컨텐츠 관리 시스템.
* 플러그인 아키텍처 및 템플릿 시스템 (WordPress 내에서 테마라고 함)이 포함한다.

### Phpmyadmin
* MySQL 및 MariaDB를위한 무료 오픈 소스 관리 도구.
* 주로 PHP로 작성된 휴대용 웹 응용 프로그램으로, 특히 웹 호스팅 서비스를 위한 가장 인기있는 MySQL 관리 도구 중 하나.
* 설치 전 php와 MariaDB가 설치되어 있어야 한다. 


### MySQL
* mysql을 GUI로 관리할 수 있는 무료 소프트웨어 도구

### Dockerfile
### SSL protocole
### url, redirection
### auto index / disable



