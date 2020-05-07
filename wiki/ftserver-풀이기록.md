---
published: true
tags: [nginx, docker, debian, php-fpm]
---

# ft_server 풀이 과정
( 쓰는 중~~~~~~~~~~ )
집에서 풀어서 제 컴퓨터인 맥(모하비 10.14.6) 기준으로 기록.

## 👨‍💻 목표
LEMP 스택 + 워드프레스 + SSL, 오토인덱스 옵션이 있는 도커 컨테이너를 만들고 실행해보기!

## 👇 풀이 계획
* [seolim님의 가이드라인](https://42born2code.slack.com/archives/CU6MTFBNH/p1584448720494300?thread_ts=1584360693.432100&cid=CU6MTFBNH)을 따라가 볼 예정!
* 데비안 이미지에서 하나씩 쌓아가는 과정을 기록할 것임.
* 컨테이너를 껐다 켤 때마다 앞에 했던 과정을 모두 반복하면 시간이 너무 오래 걸리므로, 컨테이너 끌 때마다 저장을 할 것임
* 도커 이미지 만들기 (저장):
  - 컨테이너 종료 후 `docker ps -a`
  - 방금 닫힌 CONTAINER ID를 복사
  - `docker commit [CONTAINER ID] 새이름`
  - `docker images` 해보면 짠 ~~~ 방금까지 한 것이 다 담긴 이미지가 생겼다!

## 👇 맥에 도커 설치
[Docker for mac 다운로드 하기](https://hub.docker.com/editions/community/docker-ce-desktop-mac/)
##### 💥주의: 지금은 `brew` 말고 `Docker for mac`
* `brew`로 `docker`를 설치할 경우, `docker-machine`, `virtualbox`도 깔아야 원활하게 실행할 수 있다. (-> 유저 설정이 귀찮아짐)
* `Docker for mac`을 설치하면 프로그램만 켜면 그냥 바로 시작할 수 있다.

## 👇 도커로 데비안 버스터 이미지 만들기
0. 데비안은 우분투 같은 리눅스 OS 종류 중에 하나다. [참고: 리눅스 OS 종류, 어떤게 좋을까?](https://secretpoten.tistory.com/31)
<br>1. `docker pull debian:buster` 
<br>2. `docker images` 입력해서 확인.

## 👇 도커로 데비안 버스터 환경에 들어가기

1. `docker run -it -p 80:80 -p 443:443 debian:buster`
  - 만약 그냥 `docker run -it debian`이라고 쓰면 자동으로 도커허브에서 `debian:latest`로 최신 버전을 불러온다.
  - `docker --name 컨테이너이름 run -it debian:buster` 이런 식으로 네임 옵션을 안주면 도커 데몬이 형용사+과학자이름?을 랜덤으로 짜서 지어준다.
  - -i 옵션은 입출력, -t는 tty활성화.. [도커 자주 쓰는 명령어 확인](도커-명령어-모음)
  - -p는 --publish의 약자인데, 80번 포트, 443번 포트 사용할 거라는 뜻.
    - [참고: http의 기본 포트가 80, https의 기본 포트가 443인 이유는 무엇일까?](https://johngrib.github.io/wiki/why-http-80-https-443/)
  - 포트 뭐 열어둘건지 설정 안해도 데비안 환경은 들어갈 수 있지만, 우리는 서버를 올려야하니까 넣었다.
<br>
2. 🕵‍♀ bash 입장 확인: 현재 위치가 `root@bda50ea6eb7e:/#` 이런 식으로 바뀐다. 데비안 bash에 들어가졌다!

## 👇 도커 x 데비안 버스터에 nginx 설치
0. 데비안에서는 패키지 관리자로 `apt-get`을 쓴다. 
1. `apt-get update` 해서 일단 패키지 목록을 최신으로 받는다. `apt-get upgrade`도 하자.
2. `apt-get install nginx` 입력. 그러면 이렇게 물어본다.
~~~
After this operation, 63.1 MB of additional disk space will be used. Do you want to continue? [Y/n]
설치 하면 63.1메가 사용되는데 괜찮니? [네/아니오]
~~~
3. y 입력.
4. 다음부터는 y 입력하기 귀찮으니까 `apt-get install -y nginx` 이렇게 yes 옵션을 넣어서 명령하자.

### 🕵‍♀ nginx 서버 연결 확인
1. `service nginx start`
2. `service nginx status`
3. 다른 터미널 창을 켜서 `curl localhost:80` 혹은 `curl localhost:443` 해보자
4. 인터넷 브라우저로 확인해보자. [localhost:80](localhost:80) 혹은 [localhost:443](localhost:443)에 들어가보자.
5. 짠 **Welcome to nginx!**가 나오면 성공~~~

### 💥 서버 응답 관련 오류 발생시 체크해볼 것들
* `service nginx status`하면 연결이 잘 되었는지 알려준다.
* `curl 127.0.0.1:443`
* `lsof -Pni4 | grep LISTEN` 연결상태인 포트 확인
* `lsof -i :80` 80번 포트 사용 상태 보기. 비사용중이면 아무것도 안나온다.
* `kill -9 [프로세스 번호]` 위 명령에서 발견한 활성 포트 죽이기
* `ping 127.0.0.1` 이런 식으로 특정 IP가 응답중인지 알 수 있다..

## 👇 도커 x 데비안 버스터 x nginx에 php-fpm 설치
* `apt-get -y install php-fpm vim`
* /etc/nginx/ 구성 살펴보기
  - sites-available = 설정 파일들이 들어있다.
  - sites-enabled = 실행시킬 파일들만 symlink로 연결해서 여기에 넣어둔다.
  - nginx.conf = sites-enabled에 있는 파일들을 호출하는 파일이다. 서버 실행에 관한 정보를 적어 둔다..
    
### 🛠 nginx x php-fpm 연동을 위한 설정변경
* `vim /etc/nginx/sites-available/default`해서
~~~
#location ~ \.php$ {
#	include snippets/fastcgi-php.conf;
#
#	# With php-fpm (or other unix sockets):
#	fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
#	# With php-cgi (or other tcp sockets):
#	fastcgi_pass 127.0.0.1:9000;
#}
~~~
를 아래와 같이 주석 해제. php**7.3**-fpm.sock; 이 부분이 설치한 PHP 버전과 일치하는지도 확인하기
~~~
location ~ \.php$ {
  include snippets/fastcgi-php.conf;
#
#	# With php-fpm (or other unix sockets):
  fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
#	# With php-cgi (or other tcp sockets):
#	fastcgi_pass 127.0.0.1:9000;
}
~~~
index.php를 자동 인식하게 하려면
~~~
index index.html index.htm index.nginx-debian.html;
~~~
에 index.php도 추가.

* `service nginx reload`
* 혹은 `service nginx restart`로 다시 로드하면 적용된다!

### 🕵‍♀ php-fpm 작동 확인
* `service php7.3-fpm start`
* `service php7.3-fpm status`

### 🕵‍♀ phpinfo() 함수로 nginx x php-fpm 연동 잘 되는지 확인
* /var/www/html/ 디렉토리에 phpinfo.php를 만들고(이름 다르게 해도됨) 아래 코드를 입력, 저장.
~~~
<?php phpinfo(); ?>
~~~
1. curl localhost:80/phpinfo.php 혹은
2. 웹브라우저로 내server아이피/phpinfo.php로 접속했을 때 phpinfo페이지가 나오면 제대로 된 것.

* phpinfo.php는 테스트 후에는 [**삭제**하는 것이 보안상 좋다고 한다.](https://avada.co.kr/webhosting/phpinfo-%ED%8E%98%EC%9D%B4%EC%A7%80%EC%97%90%EC%84%9C-php-%EC%84%A4%EC%A0%95%EC%9D%84-%ED%99%95%EC%9D%B8%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95/)
* 참고: [아파치설치 후 phpinfo가 정상적으로 출력되지 않을때, 체크해봐야 할 것들](https://idchowto.com/?p=16772)<br>
* 참고: [phpinfo()가 소스 그대로 나올 경우](https://medium.com/sjk5766/phpinfo-%EA%B0%80-%EC%86%8C%EC%8A%A4-%EA%B7%B8%EB%8C%80%EB%A1%9C-%EB%82%98%EC%98%AC-%EA%B2%BD%EC%9A%B0-f8993576adc5)

## 👇 도커 x 데비안 버스터 x nginx x php-fpm에  MariaDB 설치
* 데비안 9부터 [MySQL -> MariaDB](https://mariadb.com/kb/en/moving-from-mysql-to-mariadb-in-debian-9/)를 사용하게 한다는 거 같아서 (데비안 버스터는 데비안 10이다) mariadb로 설치했다.
* `apt-get -y install mariadb-server php-mysql`
* `service mysql start`
* [SQL 문법 알아보기](sql문법)

### 🛠 MariaDB root 유저 비밀번호 및  설정
~~~
mysql_secure_installation // root 계정 비밀번호 등 설정
mysql -uroot -p   // 웹에서 root 계정을 사용할 수 있게 수정

use mysql;
update user set plugin='' where user='root';
flush privileges;
quit;
~~~

### 🕵‍♀ 데이터베이스를 추가해보자

[예제로 익히는 SQL 문법](sql문법) 바로가기

## phpmyadmin 설치

* 데비안에 phpmyadmin을 바로 다운로드 할 수 있게하는 패키지는 현재 없음.
* `wget`으로 직접 다운로드 하면 된다. (phpmyadmin 다운로드 사이트에서 다운로드 버튼의 링크 주소를 복사, wget [주소])

~~~
apt-get install wget
wget https://files.phpmyadmin.net/phpMyAdmin/5.0.2/phpMyAdmin-5.0.2-all-languages.tar.gz
tar -xvf phpMyAdmin-5.0.2-all-languages.tar.gz
mv phpMyAdmin-5.0.2-all-languages /var/www/localhost/
apt-get install -y php-mbstring php-curl

~~~

* https://www.itzgeek.com/how-tos/linux/debian/how-to-install-phpmyadmin-with-nginx-on-debian-10.html
(uncomment the phpMyAdmin storage settings.)

cp -pr config.sample.inc.php config.inc.php

vim config.inc.php

* [Blowfish Password 제너레이터1](http://www.passwordtool.hu/blowfish-password-hash-generator)
* [Blowfish Password 제너레이터2](https://phpsolved.com/phpmyadmin-blowfish-secret-generator/?g=5cecac771c51c)

service mysql start
mysql < sql/create_tables.sql -u root -p
mysql -u root -p


grant all privileges on phpmyadmin.* to 'pma'@'localhost' identified by 'pmapass';
Query OK, 0 rows affected (0.009 sec)

MariaDB [(none)]> flush privileges;
Query OK, 0 rows affected (0.001 sec)

MariaDB [(none)]> exit
Bye
















# Config MYSQL
echo "CREATE DATABASE wordpress;" | mysql -u root --skip-password
echo "GRANT ALL PRIVILEGES ON wordpress.* TO 'root'@'localhost' WITH GRANT OPTION;" | mysql -u root --skip-password
echo "update mysql.user set plugin='mysql_native_password' where user='root';" | mysql -u root --skip-password
echo "FLUSH PRIVILEGES;" | mysql -u root --skip-password

service mysql start

# Config Access
~~~
chown -R www-data /var/www/*
chmod -R 755 /var/www/*
~~~
* chown: 리눅스에서 소유자를 변경하는 커맨드.
  - -R은 --recursive. 에러 메시지가 있어도 출력하지 않게 하는 커맨드.
  - www-data는 우분투에서 `Apache`,`PHP` 실행시 수정이 가능한 루트 권한(?)
* chmod: 읽기, 쓰기, 실행에 대한 권한(permission)을 변경하는 커맨드.


# Making SSL Certification
mkdir /etc/nginx/ssl
openssl req -newkey rsa:4096 -x509 -sha256 -days 365 -nodes -out /etc/nginx/ssl/monsupersite.pem -keyout /etc/nginx/ssl/monsupersite.key -subj "/C=FR/ST=Paris/L=Paris/O=42 School/OU=rchallie/CN=monsupersite"

req -new -key
rsa:4096
-sha256
-subj "/C=KR/CN=yeosong/O=42seoul/OU=yeosong/L=seoul/S=gaepo"

| 사용시 표기 | 의미 | 내용 |
|:---|:---|:---|
| CN | Common Name | 일반 이름 (인증서 고유 이름).<br>대부분의 인증기관 CA에서는 SSL인증서 신청시에 도메인명을 CN으로 지정.|
| O | Organization | 기관명 |
| OU | Organization Unit | 회사/기관 내의 '사업부, 부문, 부서, 본부, 과, 팀' 정도. |
| L | City/Locality | 시/도 |
| S | State/County/Region | 구/군 |
| STREET | Street | 나머지 상세 주소. (OV,EV 인증시에만 필요) |
| C | Country | 국가를 나타내는 ISO 코드를 지정. 한국은 KR, 미국은 US 등 2자리 코드 |



   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
