---
published: true
---
# ft_server 채점 매뉴얼

## 클러스터 환경 설정
### 도커 설치
MSC(Managed Software Center)에서 카테고리 '소프트웨어' 접속 -> 도커 install

### 도커 설정
git clone https://github.com/alexandregv/42toolbox; cd 42toolbox; sh init_docker.sh

### Error response from daemon: Bad response from Docker engine
혹시 오류 발생시 sudo를 써보기.
~~~
$ docker ps
Error response from daemon: Bad response from Docker engine

$ sudo docker ps
$ Password:(인트라 비밀번호 입력)
$ intra_name is not in the sudoers file. This incident is will be reported.

저렇게 말하지만 다시 $ docker ps 해보면 정상작동 한다.
~~~

## 채점
이미지 생성
~~~
docker build . -t ft_server
~~~
도커 파일 실행
~~~
docker run -it -p80:80 -p443:443 ft_server
~~~

### 체크리스트
* [localhost](http://localhost)
* [localhost:80](localhost:80)
* [localhost:443](localhost:443)
* [localhost/wordpress](localhost/wordpress)
* [localhost/phpmyadmin](localhost/phpmyadmin)
* SSL CA
* 오토인덱스
