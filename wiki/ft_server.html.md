---
published: true
---

# ft_server 서브젝트
요약: 이번 서브젝트는 [시스템 관리]입니다. Docker를 이해하고(이 서브젝트에서 도커 컴포즈 안씀, 도커파일만 씀), 첫 웹 서버를 설정하게 됩니다.

## 소개
* 이 서브젝트는 시스템 관리를 소개합니다.
* 이는 스크립트를 사용하여 작업을 자동화하는 것의 중요성을 알려줄 겁니다.
  이를 위해, 당신은 "docker"기술을 이해하고 이를 이용해 완전한 웹 서버를 설치하십시오.
* 이 서버는 동시에 여러 서비스를 실행할 것입니다. : `Wordpress`, `phpMyAdmin`, `SQL database`.

## 일반 지침
* ./srcs/서버_구성에_필요한_모든_파일_넣기
* ./srcs/워드프레스_웹사이트에_필요한_모든_파일_넣기
* ./Dockerfile
  - 도커파일이 당신의 컨테이너를 빌드 할 것입니다. docker compose는 사용 금지.

## 필수 파트
* 딱 하나의 도커 컨테이너에 `Nginx`가 있는 웹 서버를 설정해야합니다. 컨테이너 OS는 꼭 `debian buster`여야합니다
* 웹 서버는 여러 서비스를 동시에 실행할 수 있어야합니다.
  - 그 서비스들은 `WordPress` 웹 사이트, `phpMyAdmin`, `MySQL` 입니다.
  - `SQL 데이터베이스`가 `WordPress` 및 `phpMyAdmin`과 작동하는지 확인해야합니다.
* 서버는 `SSL 프로토콜`을 사용할 수 있어야합니다.
* URL에 따라 서버가 올바른 웹 사이트로 리디렉션되는지 확인해야합니다.
* 서버가 오토 인덱스로 실행 중인지 확인하고, 이를 비활성화 할 수 있어야합니다.

## 요약
~~~
| 워드프레스 | phpmyadmin  | -------------- |
| ------- MYSQL ------  | -- 다른 도커 --- |
| 엔진엑스 (오토인덱스, SSL) | --- 컨테이너 --- |      
|   데비안 OS 버스터 버전   | ---------------| 
| ---------------- 도커 ------------------| 
| ----- 내 컴퓨터(맥 OS 모하비 10.14.6) -----|
~~~

## 다시 요약
**LEMP 스택 + 워드프레스** + SSL, 오토인덱스 옵션이 있는 도커 컨테이너를 만드시오!
* LEMP 스택
  - 동적 웹 어플리케이션을 구현하기 위해서 필요한 Linux + Nginx + MySQL(or MariaDB) + PHP(or Perl, Python)를 모아서 부르는 말이다.
  - 데비안 9부터 [**MySQL -> MariaDB**](https://mariadb.com/kb/en/moving-from-mysql-to-mariadb-in-debian-9/)를 사용하게 한다는 듯 (데비안 버스터는 데비안 10이다)
* LAMP 스택
  - Linux + Apache + MySQL(or MariaDB) + PHP(or Perl, Python)
  

## ft_server 풀이에 필요한 서비스에 대해 알아보기
[바로 가기](ftserver-서비스목록)
## ft_server 풀이 기록
[바로 가기](ftserver-풀이기록)
## ft_server 채점 매뉴얼
[바로가기](ft_server-채점-방법)


[- sohpark님의 설명서 보기!!](https://stitchcoding.tistory.com/2)
