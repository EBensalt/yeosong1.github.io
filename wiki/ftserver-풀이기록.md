---
published: true
tags: [nginx, docker, debian, php-fpm, phpmyadmin, wordpress, SSL, autoindex]
---

# ft_server 풀이 과정
집에서 풀어서 내 컴퓨터인 맥(모하비 10.14.6) 기준으로 기록..

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
1. `apt-get update` 해서 일단 패키지 목록을 최신으로 받는다.
2. `apt-get upgrade`도. 그러면 이렇게 물어본다.
~~~
After this operation, 8192 B of additional disk space will be used.
Do you want to continue? [Y/n] y
설치 하면 8192B 사용되는데 괜찮니? [네/아니오]
~~~
3. y 입력.
4. 다음부터는 y 입력하기 귀찮으니까 `apt-get -y upgrade` 이렇게 yes 옵션을 넣어서 명령하자.
5. `apt-get -y install nginx` 해서 nginx 설치 

### 🕵‍♀ nginx 서버 연결 확인

1. `service nginx start` 엔진엑스 시작
2. `service nginx status` 잘 도는지 또 확인.
3. 인터넷 브라우저로 확인해보자. [localhost](https://localhost/) 혹은 [localhost:80](localhost:80)에 들어가보자.
4. **Welcome to nginx!**가 나오면 성공
5. 안나오고 아래처럼 나온다면 다른 터미널 창에 `curl localhost`를 해보자.
<img width="462" alt="스크린샷 2020-05-18 오후 7 24 05" src="https://user-images.githubusercontent.com/53321189/82202671-3a656600-993d-11ea-8013-78186ad592a2.png">
6. `curl localhost` 했을 때 아래 내용이 나온다면 일단 넘어가자.
<img width="555" alt="스크린샷 2020-05-18 오후 7 26 26" src="https://user-images.githubusercontent.com/53321189/82202876-80222e80-993d-11ea-9a26-fe282457eb46.png">

### 💥 서버 응답 관련 오류 발생시 체크해볼 것들
* `service nginx status`하면 연결이 잘 되었는지 알려준다.
* `curl 127.0.0.1:80`, `curl localhost` 등 curl을 사용해서 터미널 창에서 해당 주소의 페이지 내용을 텍스트 형식으로 볼 수 있다.
* `lsof -Pni4 | grep LISTEN` 연결상태인 포트 확인
* `lsof -i :[포트 번호]` 특정 포트 사용 상태 보기. 비사용중이면 아무것도 안나온다.
* `lsof -i :[포트 번호]` 했을 때 아무것도 안나오는데 이미 할당중이라고 나온다면.. `sudo lsof -i :[포트 번호]`..
* `kill -9 [프로세스 번호]` 위 명령에서 발견한 활성 포트 죽이기
* `ping 127.0.0.1` 이런 식으로 특정 IP가 응답중인지 알 수 있다..

## 👇openssl로 self-signed SSL 인증서 만들기
계속 이렇게 curl해서 페이지 소스만 보면 답답하고 의욕도 안날 것이다..
셀프 사인한 인증서를 넣어서 약간 더 안전한 사이트인 것처럼 해서 브라우저를 설득해보자..

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
openssl 설치-개인키 생성-인증서생성-권한제한.
~~~
apt-get -y install openssl vim //vim은 앞으로 이것저것 수정할 때 쓰려고 같이 설치했다.
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

### 🛠 nginx에 ssl을 더하기 위한 default 파일 설정 변경

1. `vim etc/nginx/sites-available/default`해서 아래와 같이 수정하자

~~~
server {
	listen 80 default_server;
	listen [::]:80 default_server;
}

server {
		listen 443;

		ssl on;
		ssl_certificate /etc/ssl/certs/localhost.dev.crt;
		ssl_certificate_key /etc/ssl/private/localhost.dev.key;

		root /var/www/html;

		index index.php index.html index.htm;

		...	
	}
~~~

2. `service nginx reload` 혹은 `service nginx restart`해서 수정사항 적용시키고
3. localhost를 열어보면 이제 Advanced를 눌러서 아래와 같은 화면을 볼 수 있다..
<img width="477" alt="스크린샷 2020-05-18 오후 8 18 34" src="https://user-images.githubusercontent.com/53321189/82207515-e9597000-9944-11ea-9216-a7e257e67c47.png">

## 👇 도커 x 데비안 버스터 x nginx에 php-fpm 설치
* `apt-get -y install php-fpm`
* /etc/nginx/ 구성 살펴보기
  - sites-available = 설정 파일들이 들어있다.
  - sites-enabled = 실행시킬 파일들만 symlink로 연결해서 여기에 넣어둔다.
  - nginx.conf = sites-enabled에 있는 파일들을 호출하는 파일이다. 서버 실행에 관한 정보를 적어 둔다.
  
### 🛠 nginx x php-fpm 연동을 위한 default 파일 내용 수정
* `vim /etc/nginx/sites-available/default`해서 이 부분
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
을 아래와 같이 주석 해제. php**7.3**-fpm.sock; 이 부분이 설치한 PHP 버전과 일치하는지도 확인하기
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
PHP를 쓸거면 이 부분
~~~
index index.html index.htm index.nginx-debian.html;
~~~
에 index.php도 추가하라고 주석에 적혀있다. 맨 뒤에 추가한다. 맨 앞에 쓰면 오류남.

### 🕵‍♀ php-fpm 작동 확인
* `service php7.3-fpm start`
* `service php7.3-fpm status`

### 🕵‍♀ phpinfo() 함수로 nginx x php-fpm 연동 잘 되는지 확인
1. `/var/www/html/` 위치에 `phpinfo.php`를 만들고(이름 다르게 테스트해도 됨) 아래 코드를 입력, 저장.
~~~
<?php phpinfo(); ?>
~~~
(<? php phpinfo(); ?>라고 쓰는 등 사소한 실수하지 않도록 주의..)
2. `service nginx reload` 혹은 `service nginx restart`해서 수정사항 적용시키기.
3. 만약 restart, reload에 실패한다면 cat /var/log/nginx/error.log 해서 오류내역을 볼 수있다.
4. 웹브라우저로 내서버아이피/phpinfo.php로 접속했을 때 아래와 같이 phpinfo페이지가 나오면 된 것.
<img width="639" alt="스크린샷 2020-05-18 오후 8 30 35" src="https://user-images.githubusercontent.com/53321189/82208511-a00a2000-9946-11ea-89eb-8446ad11eb4e.png">

* phpinfo.php는 테스트 후에는 [**삭제**하는 것이 보안상 좋다고 한다.](https://avada.co.kr/webhosting/phpinfo-%ED%8E%98%EC%9D%B4%EC%A7%80%EC%97%90%EC%84%9C-php-%EC%84%A4%EC%A0%95%EC%9D%84-%ED%99%95%EC%9D%B8%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95/)

## 👇 도커 x 데비안 버스터 x nginx x php-fpm에  MariaDB(mysql) 설치
* 데비안 9부터 [MySQL -> MariaDB](https://mariadb.com/kb/en/moving-from-mysql-to-mariadb-in-debian-9/)를 디폴트로 사용하게 한대서 (데비안 버스터는 데비안 10이다) mariadb를 설치했다.
* `apt-get -y install mariadb-server php-mysql`

## 👇 phpmyadmin 설치 및 압축해제
참고 사이트
* [How To Install phpMyAdmin with Nginx on Debian 10](https://www.itzgeek.com/how-tos/linux/debian/how-to-install-phpmyadmin-with-nginx-on-debian-10.html)
* [phpMyAdmin 설치방법 (DB 관리용 웹 프로그램을 리눅스 우분투 서버에 설치하기)](https://swiftcoding.org/installing-phpmyadmin)

0. 데비안에 phpmyadmin을 바로 다운로드 할 수 있게하는 패키지는 현재 없다.
1. `wget`으로 직접 다운로드 한다. (phpmyadmin 다운로드 사이트에서 다운로드 버튼의 링크 주소를 복사, wget [주소])
2. 압축해제 후 폴더명을 phpmyadmind으로 바꿔서 /var/www/html/에 위치 시킨다.

~~~
apt-get install -y wget
wget https://files.phpmyadmin.net/phpMyAdmin/5.0.2/phpMyAdmin-5.0.2-all-languages.tar.gz
tar -xvf phpMyAdmin-5.0.2-all-languages.tar.gz
mv phpMyAdmin-5.0.2-all-languages phpmyadmin
mv phpmyadmin /var/www/html/
~~~

### 🛠 phpmyadmin 설정

1. phpmyadmin/config.sample.inc.php 파일을 복사해 config.inc.php를 만든다.
2. config.inc.php에 블로피시 암호를 만들어 넣는다.
3. create_tables.sql을 가져와서 phpMyAdmin을 위한 테이블을 만든다.
  
~~~
cp -rp var/www/html/phpmyadmin/config.sample.inc.php var/www/html/phpmyadmin/config.inc.php 
vim var/www/html/phpmyadmin/config.inc.php
~~~
  * [Blowfish 암호 생성기 1](http://www.passwordtool.hu/blowfish-password-hash-generator)
  * [Blowfish 암호 생성기 2](https://phpsolved.com/phpmyadmin-blowfish-secret-generator/?g=5cecac771c51c)

~~~
// 블로피시 암호 생성 사이트에서 생성한 암호를 복사해서

$cfg['blowfish_secret'] = '이 부분에 넣는다'; /* YOU MUST FILL IN THIS FOR COOKIE AUTH! */
~~~


~~~
service nginx reload
service mysql start
service php7.3-fpm restart (왜지?)

mysql < var/www/html/phpmyadmin/sql/create_tables.sql -u root --skip-password

mysqladmin -u root -p password
기존 패스워드 없으니 엔터 입력 
새 패스워드 입력
한 번 더 입력

mysql
show databases;

CREATE DATABASE IF NOT EXISTS wordpress; // 워드프레스를 위한 DB 만들기

show databases;
exit
~~~


### 🕵‍♀ phpMyAdmin 작동 확인

service mysql start
[localhost/phpmyadmin](http://localhost/phpmyadmin)
아이디 root, 비밀번호는 아까 만든 그 비밀번호. 로그인 해보기.

### 🕵‍♀ 데이터베이스를 추가해보자
[예제로 익히는 SQL 문법](sql문법) 바로가기


## 👇 Wordpress 설치하기

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


### 🛠 Wordpress 설정
1.  var/www/html/wordpress/wp-config.php에 설정 수정하기..
~~~
cp var/www/html/wordpress/wp-config-sample.php var/www/html/wordpress/wp-config.php 
vim var/www/html/wordpress/wp-config.php 

아래 부분을 내용에 맞게 바꿔준다. 

// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'wordpress' );

/** MySQL database username */
define( 'DB_USER', 'root' );

/** MySQL database password */
define( 'DB_PASSWORD', 'yeosong' );

/** MySQL hostname */
define( 'DB_HOST', 'localhost' );

/** Database Charset to use in creating database tables. */
define( 'DB_CHARSET', 'utf8' );

/** The Database Collate type. Don't change this if in doubt. */
define( 'DB_COLLATE', '' );
~~~

3. 생략 가능 ([워드프레스에 필요하거나 권장되는 추가 모듈들을 설치한다.](https://www.digitalocean.com/community/questions/php-curl-and-mbstring-extensions-enabled)
apt-get install -y php-mbstring php-curl )


### 🕵‍♀ Wordpress 작동 확인
service nginx reload
[localhost/wordpress](localhost/wordpress) 접속

## 🛠 nginx x autoindex를 추가하기................
vim etc/nginx/sites-available/default에 autoindex on;을 추가한다..

~~~

server_name _;

location / {
	# First attempt to serve request as file, then
	# as directory, then fall back to displaying a 404.
	autoindex on;
	try_files $uri $uri/ =404;
}

~~~
그리고나면 아래와 같이 오토 인덱싱 된 화면을 볼 수 있다.
var/www/html/index.nginx-debian.html은 지우고 phpinfo.php 삭제는 안했네..........
<img width="639" alt="스크린샷 2020-05-19 오전 3 04 17" src="https://user-images.githubusercontent.com/53321189/82245178-71f40280-997d-11ea-9532-bd2bc55c2325.png">




### 🕵‍♀  마지막 확인. [localhost](http://localhost)
- 모두 정상작동 한다면, 지금까지의 내용을 Dockerfile + srcs에 지시문 형태로 보기 좋게 정리하면 끝!
- sed 같은 걸 써서 설정파일 수정하는 내용까지 도커파일 안에 지시하는 풀이도 보았는데..
- 필요한 default, config.inc.php, wp-config.php 파일들을 수정해서 srcs에 넣어두고, 복사해서 가져다 쓰는 식으로 했다.



## 의문점

1. service mysql start 하면 작동 후 이거 왜 나오지

~~~
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)
~~~

1-1. service mysql stop도 안된다.

* 아직 정리 못한 내용...
* https://www.nemonein.xyz/2019/07/2254/
* https://bscnote.tistory.com/77
* https://tocsg.tistory.com/34
* https://github.com/phusion/baseimage-docker/issues/319
* https://recte.tistory.com/32
* cp -p 옵션: 원본파일의 소유주,그룹,권한,시간정보를 보존해서 복사하는 기능
