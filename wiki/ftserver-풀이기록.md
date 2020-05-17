---
published: true
tags: [nginx, docker, debian, php-fpm, phpmyadmin, wordpress, SSL]
---

# ft_server 풀이 과정
집에서 풀어서 내 컴퓨터인 맥(모하비 10.14.6) 기준으로 기록.. 하려고 했는데 계속 문제가 생겨서 vnc로............ 눈물이 나네..

## 👨‍💻 목표
LEMP 스택 + 워드프레스 + SSL, 오토인덱스 옵션이 있는 도커 컨테이너를 만들고 실행해보기!

## 👇 풀이 계획
* [seolim님의 가이드라인](https://42born2code.slack.com/archives/CU6MTFBNH/p1584448720494300?thread_ts=1584360693.432100&cid=CU6MTFBNH)을 따라가 볼 예정!
* 데비안 이미지에서 하나씩 쌓아가는 과정을 기록할 것임.
* 컨테이너를 껐다 켤 때마다 앞에 했던 과정을 모두 반복하면 시간이 너무 오래 걸리므로, 컨테이너 끌 때마다 저장을 할 것임
* 도커 이미지 만들기 (저장):
  - 컨테이너 종료 후 `docker ps -a`
  - 방금 닫힌 CONTAINER ID를 복사
  - `docker commit [CONTAINER ID] [이름]`
  - `docker images` 해보면 -> 방금까지 한 것이 다 담긴 이미지가 생겼다!

## 👇 42 클러스터 맥에 도커 설치
1. Managed Software Center에 들어간다..
2. Software 탭에서 Docker를 install 한다..
3. [42toolbox](https://github.com/alexandregv/42toolbox)를 이용`git clone https://github.com/alexandregv/42toolbox.git ~/42toolbox`
4. `echo "source ~/42toolbox/shell_utils.sh" >> ~/.zshrc`
5. `sh init_docker`
6. 🕵‍♀ 터미널 창에 `docker ps`해서 정상작동 하는지 확인해 보았다.

## 👇 개인 맥에 도커 설치
[Docker for mac 다운로드 하기](https://hub.docker.com/editions/community/docker-ce-desktop-mac/)
##### 💥주의: `brew` 말고 `Docker for mac`
* `brew`로 `docker`를 설치할 경우, `docker-machine`, `virtualbox`도 깔아야 원활하게 실행할 수 있다. (-> 유저 설정이 귀찮아짐)
* `Docker for mac`을 설치하면 프로그램만 켜면 그냥 바로 시작할 수 있다.

## 👇 도커로 데비안 버스터 이미지 만들기
0. 데비안은 우분투 같은 리눅스 OS 종류 중에 하나다. [참고: 리눅스 OS 종류, 어떤게 좋을까?](https://secretpoten.tistory.com/31)
<br>1. `docker pull debian:buster` 
<br>2. 🕵‍♀ `docker images` 입력해서 확인.

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
5. **Welcome to nginx!**가 나오면 성공

### 💥 서버 응답 관련 오류 발생시 체크해볼 것들
* `service nginx status`하면 연결이 잘 되었는지 알려준다.
* `curl 127.0.0.1:443` 이런 식으로 터미널 창에서 해당 주소의 페이지 내용을 텍스트로 볼 수 있다.
* `lsof -Pni4 | grep LISTEN` 연결상태인 포트 확인
* `lsof -i :[포트 번호]` 특정 포트 사용 상태 보기. 비사용중이면 아무것도 안나온다.
* `kill -9 [프로세스 번호]` 위 명령에서 발견한 활성 포트 죽이기
* `ping 127.0.0.1` 이런 식으로 특정 IP가 응답중인지 알 수 있다..
* `lsof -i :[포트 번호]` 했을 때 아무것도 안나오는데 이미 할당중이라고 나온다면.. `sudo lsof -i :[포트 번호]`..

## 👇 도커 x 데비안 버스터 x nginx에 php-fpm 설치
* `apt-get -y install php-fpm vim`
* /etc/nginx/ 구성 살펴보기
  - sites-available = 설정 파일들이 들어있다.
  - sites-enabled = 실행시킬 파일들만 symlink로 연결해서 여기에 넣어둔다.
  - nginx.conf = sites-enabled에 있는 파일들을 호출하는 파일이다. 서버 실행에 관한 정보를 적어 둔다..
  
### 🛠 nginx x php-fpm 연동을 위한 /etc/nginx/sites-available/default 파일 내용 수정
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
* /var/www/html/ 디렉토리에 phpinfo.php를 만들고(이름 다르게 테스트해도 됨) 아래 코드를 입력, 저장.
~~~
<?php phpinfo(); ?>
~~~
1. curl localhost:80/phpinfo.php 혹은
2. 웹브라우저로 내server아이피/phpinfo.php로 접속했을 때 phpinfo페이지가 나오면 제대로 된 것.

* phpinfo.php는 테스트 후에는 [**삭제**하는 것이 보안상 좋다고 한다.](https://avada.co.kr/webhosting/phpinfo-%ED%8E%98%EC%9D%B4%EC%A7%80%EC%97%90%EC%84%9C-php-%EC%84%A4%EC%A0%95%EC%9D%84-%ED%99%95%EC%9D%B8%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95/)
* 참고: [아파치설치 후 phpinfo가 정상적으로 출력되지 않을때, 체크해봐야 할 것들](https://idchowto.com/?p=16772)<br>
* 참고: [phpinfo()가 소스 그대로 나올 경우](https://medium.com/sjk5766/phpinfo-%EA%B0%80-%EC%86%8C%EC%8A%A4-%EA%B7%B8%EB%8C%80%EB%A1%9C-%EB%82%98%EC%98%AC-%EA%B2%BD%EC%9A%B0-f8993576adc5)

## 👇 도커 x 데비안 버스터 x nginx x php-fpm에  MariaDB 설치
* 데비안 9부터 [MySQL -> MariaDB](https://mariadb.com/kb/en/moving-from-mysql-to-mariadb-in-debian-9/)를 디폴트로 사용하게 한대서 (데비안 버스터는 데비안 10이다) mariadb를 설치했다.
* `apt-get -y install mariadb-server php-mysql`
* `service mysql start`

### 🛠 MariaDB(mysql) root 유저 비밀번호 및 설정
~~~
service mysql start
mysql -u root -p   // 웹에서 root 계정을 사용할 수 있게 수정
내 비밀번호
use mysql;
update user set plugin='' where user='root';
flush privileges;
quit;
~~~

~~~
mysql < var/www/localhost/phpMyAdmin-5.0.2-all-languages/sql/create_tables.sql -u root -p

mysql -u root -p -e "CREATE DATABASE IF NOT EXISTS wordpress;"

service nginx restart
service php7.3-fpm restart
~~~


### 🕵‍♀ 데이터베이스를 추가해보자
[예제로 익히는 SQL 문법](sql문법) 바로가기

## phpmyadmin 설치 및 압축해제
참고 사이트
* [How To Install phpMyAdmin with Nginx on Debian 10](https://www.itzgeek.com/how-tos/linux/debian/how-to-install-phpmyadmin-with-nginx-on-debian-10.html)
* [phpMyAdmin 설치방법 (DB 관리용 웹 프로그램을 리눅스 우분투 서버에 설치하기)](https://swiftcoding.org/installing-phpmyadmin)

0. 데비안에 phpmyadmin을 바로 다운로드 할 수 있게하는 패키지는 현재 없음.
1. `wget`으로 직접 다운로드 한다. (phpmyadmin 다운로드 사이트에서 다운로드 버튼의 링크 주소를 복사, wget [주소])
2. 압축해제 후 폴더명을 phpmyadmind으로 바꿔서 /var/www/html/에 위치 시킨다.
3. [워드프레스에 필요하거나 권장되는 추가 모듈들을 설치한다.](https://www.digitalocean.com/community/questions/php-curl-and-mbstring-extensions-enabled)
~~~
apt-get install wget
wget https://files.phpmyadmin.net/phpMyAdmin/5.0.2/phpMyAdmin-5.0.2-all-languages.tar.gz
tar -xvf phpMyAdmin-5.0.2-all-languages.tar.gz
mv phpMyAdmin-5.0.2-all-languages phpmyadmin
mv phpmyadmin /var/www/html/
apt-get install -y php-mbstring php-curl
~~~

### 🛠 phpmyadmin 설정

1. phpmyadmin/config.sample.inc.php 파일을 복사해 config.inc.php를 만든다.
2. 블로피시 암호를 만들어서 넣는다.
  * [Blowfish 암호 생성기 1](http://www.passwordtool.hu/blowfish-password-hash-generator)
  * [Blowfish 암호 생성기 2](https://phpsolved.com/phpmyadmin-blowfish-secret-generator/?g=5cecac771c51c)
3. phpMyAdmin storage setting을 주석해제한다.
4. create_tables.sql을 가져와서 phpMyAdmin을 위한 테이블을 만든다.
  
~~~
cp -pr config.sample.inc.php config.inc.php

vim config.inc.php
블로피시 부분 변경
...................

mysql < /usr/share/phpMyAdmin/sql/create_tables.sql -u root -p

service restart nginx 
service restart php7.3-fpm
~~~

### 🕵‍♀ phpMyAdmin 작동 확인

[localhost:443/phpmyadmin](localhost:443/phpmyadmin)

## Wordpress 설치하기

* 참고: [CentOS7 에 Nginx + PHP 7 + Mysql + Wordpress 설치](https://noonestaysthesame.tistory.com/6?category=632372)

~~~
wget https://wordpress.org/latest.tar.gz
tar -xvf latest.tar.gz
mv wordpress/ var/www/html/
chown -R www-data:www-data /var/www/html/wordpress
~~~
* chown: 리눅스에서 소유자를 변경하는 커맨드.
  - -R은 --recursive. 에러 메시지가 있어도 출력하지 않게 하는 커맨드.
  - www-data는 우분투에서 `Apache`,`PHP` 실행시 수정이 가능한 권한

### 🕵‍♀ Wordpress 작동 확인
localhost/wordpress 접속

## 👇openssl로 self-signed SSL 인증서 만들기
* 참고: [[홈서버 구축기] SSL 인증서 만들기 (연습)](https://blog.hangadac.com/2017/07/31/%ED%99%88%EC%84%9C%EB%B2%84-%EA%B5%AC%EC%B6%95%EA%B8%B0-ssl-%EC%9D%B8%EC%A6%9D%EC%84%9C-%EB%A7%8C%EB%93%A4%EA%B8%B0-%EC%97%B0%EC%8A%B5/)
* 참고: [생활코딩 HTTPS와 SSL 인증서](https://opentutorials.org/course/228/4894)

~~~
인증서 만드는 방법

1. Self-signed 인증서
* CSR 명시적 생성 -> 인증서에 self-sign -> 인증서 완성
* CSR을 명시적으로 생성하지 않고, key와 부가정보들을 입력하여 직접 self-sign 하여 인증서 완성
2. CSR (인증서 서명 요청)을 만들어 CA에 요청해서 발급받는 방법
* 유료
* 무료 (ex: Letsencrypt)
~~~
우리는 제일 첫번째 방법을 쓴다. 
~~~
openssl req -newkey rsa:4096 -days 365 -nodes -x509 -subj "/C=KR/ST=Seoul/L=Seoul/O=42Seoul/OU=Lee/CN=localhost" -keyout localhost.dev.key -out localhost.dev.crt
mv localhost.dev.crt etc/ssl/certs/
mv localhost.dev.key etc/ssl/private/
chmod 600 etc/ssl/certs/localhost.dev.crt etc/ssl/private/localhost.dev.key
~~~
옵션별 뜻
- [openssl 커맨드 옵션](openssl-커맨드)
- .csr 인증사인 요청파일
- .crt 인증서 파일
- -days 유효 일수
- -nodes [생략시 재부팅할때마다 수동으로 암호를 입력해야함](https://c10106.tistory.com/2364)
- [개인키, csr, crt 예제](개인키예제)

| 사용시 표기 | 의미 | 내용 |
|:---|:---|:---|
| CN | Common Name | 일반 이름 (인증서 고유 이름).<br>대부분의 인증기관 CA에서는 SSL인증서 신청시에 도메인명을 CN으로 지정.|
| O | Organization | 기관명 |
| OU | Organization Unit | 회사/기관 내의 '사업부, 부문, 부서, 본부, 과, 팀' 정도. |
| L | City/Locality | 시/도 |
| S | State/County/Region | 구/군 |
| STREET | Street | 나머지 상세 주소. (OV,EV 인증시에만 필요) |
| C | Country | 국가를 나타내는 ISO 코드를 지정. 한국은 KR, 미국은 US 등 2자리 코드 |

### 🛠 nginx에 ssl을 더하기 위한 etc/nginx/sites-available/default 파일 설정 변경
~~~
ssl on;
ssl 

~~~
### 🛠 autoindex를 더하기 위한 etc/nginx/sites-available/default 파일 설정 변경
~~~
autoindex on;
~~~
   
   
# 🚧 공사중...

   
   
   
   
   
