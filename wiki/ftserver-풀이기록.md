---
published: false
---

# ft_server 풀이 과정

## 👇 맥에서 도커 시작하기

0. 버추얼박스, 도커, 도커 머신 install.
~~~
brew install virtualbox
brew install docker
brew install docker-machine
~~~
1. 생처음이면 `docker-machine create 새머신이름`
<br>    - `docker-machine create --driver virtualbox 새머신이름`해도 되는데,
<br>      버추얼박스 깔려있으면 알아서 버추얼박스를 드라이버로 해서 만들어준다.
2. 깔고나면 시키는대로 `docker-machine env` 입력.
<br>입력 하면 쓰기 편하게 환경변수 설정을 알아서 해준다. 
~~~
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.100:2376"
export DOCKER_CERT_PATH="/Users/myloginname/.docker/machine/machines/mynewwwmachine"
export DOCKER_MACHINE_NAME="mynewwwmachine"
# Run this command to configure your shell:
# eval $(docker-machine env mynewwwmachine)
~~~
<br>3. 잘 읽어보면 `eval $(docker-machine env mynewwwmachine)` 입력하라고 한다.
<br>이걸 왜 하냐면.. 일단 입력하고 보면,
<br>  - `docker-machine ls` 해보면 ACTIVE칸에 *가 생긴다.
    `eval $(docker-machine env)`명령을 해서 걔가 특정된 것이다.
<br>  - 가동중인 도커머신이 여러개일때 `docker ps`(=현재 가동중인 컨테이너를 보여줘) 같은 명령을 하면,
        어느 머신에 대한 명령인지 알 수 없다. 이 때, ACTIVE *가 있으면, 해당하는 도커머신에 대해서만 특정해서 명령한게 되는 것.  

## 👇 도커로 데비안 버스터 이미지 만들기

<br>1. `docker pull debian:buster` 
<br>2. `docker images` 입력해서 확인.

## 👇 도커로 데비안 버스터 환경에 들어가기

<br>0. 일단 데비안은 우분투 같은 리눅스 OS 종류 중에 하나다.
<br>[참고: 리눅스 OS 종류, 어떤게 좋을까?](https://secretpoten.tistory.com/31)
<br>
<br>
1. `docker run -it -p 80:80 debian:buster`
  - 그냥 `docker run -it debian`이라고 쓰면 자동으로 `debian:latest`로 최신 버전을 불러온다.
  - `docker --name 컨테이너이름 run -it debian:buster` 이런 식으로 네임 옵션을 안주면 도커 데몬이 형용사+과학자이름?을 랜덤으로 짜서 지어준다.
  - -i 옵션은 입출력, -t는 tty활성화.. 잘 설명한 블로그가 정말 많아서 저는 생략 [docker run/volume 커맨드](https://tinkerbellbass.tistory.com/47)
  - -p는 포트를 80번 포트랑 연결해줄거라는 얘기. 설정 안해도 데비안 환경은 들어갈 수 있지만, 우리는 나중에 nginx를 서버에 올릴거라서 넣었어요.
<br>
2. 현재 위치가 `root@bda50ea6eb7e:/#` 이런 식으로 바뀌었죠? 짠 리눅스 환경에 들어가졌습니다. (여기서 `exit`하면 컨테이너가 종료됩니다.)

## 👇 도커 x 데비안 버스터에 nginx 깔기

<br>0. 데비안에서는 패키지 관리자로 `brew` 대신 `apt-get`을 씁니다.
<br>1. `apt-get update` 해서 일단 패키지 목록을 최신으로 받습니다. `apt-get upgrade`도 합시다.
<br>2. `apt-get install nginx` 입력. 그러면 이렇게 물어봅니다.
  - After this operation, 63.1 MB of additional disk space will be used. Do you want to continue? [Y/n]
  - 설치 하면 63.1메가 사용되는데 괜찮니? [네/아니오]
  - y 입력. 다음부터는 y 입력하기 귀찮으니까 `apt-get install -y nginx` 이렇게 yes 옵션을 넣어서 명령하세요.
<br>3. `service nginx start`
<br>4. 새로운 터미널 창을 하나 열어보세요. `docker-machine ip` 해서 나오는 IP를 웹브라우저에 넣어보세요.
   - 짠 **Welcome to nginx!** 나오죠? 성공~~~
   
   
   
   
   
   
   


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


# Generate website folder
phpinfo() 함수로 웹호스팅 환경 점검하기
~~~
mkdir /var/www/monsupersite && touch /var/www/monsupersite/index.php
echo "<?php phpinfo(); ?>" >> /var/www/monsupersite/index.php
~~~
테스트용 페이지를 연결.<br>
[아파치설치 후 phpinfo가 정상적으로 출력되지 않을때, 체크해봐야 할 것들](https://idchowto.com/?p=16772)<br>
[phpinfo()가 소스 그대로 나올 경우](https://medium.com/sjk5766/phpinfo-%EA%B0%80-%EC%86%8C%EC%8A%A4-%EA%B7%B8%EB%8C%80%EB%A1%9C-%EB%82%98%EC%98%AC-%EA%B2%BD%EC%9A%B0-f8993576adc5)


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

# Config NGINX
~~~
mv ./tmp/nginx-conf /etc/nginx/sites-available/monsupersite
ln -s /etc/nginx/sites-available/monsupersite /etc/nginx/sites-enabled/monsupersite
rm -rf /etc/nginx/sites-enabled/default
~~~
* sites-available
    - 설정 파일들이 들어있다.
* sites-enabled
    - 실행시킬 파일들만 symlink로 연결해서 여기에 넣어둔다.
* nginx.conf
    - sites-enabled에 있는 파일들을 호출하는 파일이다. 서버 실행에 관한 정보를 적어 둔다..

# Config MYSQL
echo "CREATE DATABASE wordpress;" | mysql -u root --skip-password
echo "GRANT ALL PRIVILEGES ON wordpress.* TO 'root'@'localhost' WITH GRANT OPTION;" | mysql -u root --skip-password
echo "update mysql.user set plugin='mysql_native_password' where user='root';" | mysql -u root --skip-password
echo "FLUSH PRIVILEGES;" | mysql -u root --skip-password


   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
