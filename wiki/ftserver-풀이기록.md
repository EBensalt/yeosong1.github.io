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
<br>      버추얼박스 깔려있으면 알아서 버추얼박스를 드라이버로 해서 만들어주네요?
<br>2. 깔고나면 시키는대로 `docker-machine env` 입력.
<br>입력 하면 쓰기 편하게 환경변수 설정을 알아서 해줍니다. 
~~~
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.100:2376"
export DOCKER_CERT_PATH="/Users/myloginname/.docker/machine/machines/mynewwwmachine"
export DOCKER_MACHINE_NAME="mynewwwmachine"
# Run this command to configure your shell:
# eval $(docker-machine env mynewwwmachine)
~~~
<br>3. 잘 읽어보셨나요?
<br>`eval $(docker-machine env mynewwwmachine)` 입력하라고 하죠?
<br>이걸 왜 하냐면.. 일단 입력하고 보면,
<br>  - `docker-machine ls` 해보면 ACTIVE칸에 *가 생겼죠?
    `eval $(docker-machine env)`명령을 해서 걔가 특정된 것입니다.
<br>  - 도커머신이 여러개일때 `docker ps`(=현재 가동중인 컨테이너를 보여줘) 같은 명령을 하면,
    ACTIVE *에 해당하는 도커머신에 대해서 명령한게 되는 것입니다.  

## 👇 도커로 데비안 버스터 이미지 만들기

<br>1. `docker pull debian:buster` 
<br>2. `docker images` 해보면? 짠 리스트 뜨죠

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
