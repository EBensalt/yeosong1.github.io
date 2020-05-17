# 도커파일 문법 (🚧 공사중........ )
[Dockerfile reference. 공식문서 바로가기](https://docs.docker.com/engine/reference/builder/)

## Usage
- `docker build .`
- .dockerignore
- `docker build -f /도커파일의/위치/Dockerfile .`
- `docker build -t 저장소이름/태그:버전 .`
- 💥 주의:
  - 니 컴퓨터의 루트 디렉토리, /를 경로로 쓰지 않는 편이 좋고,
  - Dockerfile만 있는 곳이 루트인 편이 좋다.
  - 도커 데몬이 니 다른 파일까지 다 전송해버리는 빌드를 할 수도 있거든..
  - (메이크파일도 적절한 파일이 없으면 알아서 비슷한 이름의 파일까지 찾아보는거같더라.. 맞나요?)
  
## Format 포맷
0. [FROM] 이전에 작성할 수 있는 지시문
  * Parser Directives(파서 지시어)
  * commernts(주석)
  * [ARG](변수)
1. 도커파일은 반드시 FROM으로 시작

**FROM**
Docker Hub에서 어떤 이미지를 가지고와서 작업할지 선언. 작성법은 <이미지명>:<태그>로 작성

**MAINTAINER**
DockerFile 제작한 사람의 정보

**ARG**

**ENV**

**RUN**
docker image가 실행되고 container 내에서 실행될 명령어를 적기
해당 내용은 적으면 적을수록 container label 이 적게 생성되어 image 를 compact 하게 생성 할 수 있다.

**CMD**
CMD는 컨테이너가 시작되었을 때 스크립트 혹은 명령을 실행.
즉 docker run 명령으로 컨테이너를 생성하거나, docker start 명령으로 정지된 컨테이너를 시작할 때 실행됨.
CMD는 Dockerfile에서 한 번만(!) 사용할 수 있다?

**EXPOSE**
Host에 연결될 Port 지정.

**ENTRYPOINT**
