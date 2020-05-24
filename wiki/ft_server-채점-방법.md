---
published: true
---
# ft_server 채점 매뉴얼

## 🛠 클러스터 환경 설정

### 도커 설치
MSC(Managed Software Center)에서 카테고리 '소프트웨어' 접속 -> 도커 install

### 도커 설정
git clone https://github.com/alexandregv/42toolbox; cd 42toolbox; sh init_docker.sh

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
* [http://localhost](http://localhost) -> https로 리디렉션 되는지 확인
* [http://localhost:80](http://localhost:80)
* [https://localhost:443](https://localhost:443)
* [https://localhost/wordpress](https://localhost/wordpress)
* [https://localhost/phpmyadmin](https://localhost/phpmyadmin), 로그인
* SSL CA가 있는지
* 오토인덱스가 잘 작동하는지
* 등등..
