# 도커파일 문법 자주 쓰는 지시어 (🚧 공사중........ )
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
- [FROM] 이전에 작성할 수 있는 지시문
  * Parser Directives(파서 지시어)
  * commernts(주석)
  * [ARG](변수)
- 도커파일은 반드시 FROM으로 시작

### FROM
Docker Hub에서 어떤 이미지를 가지고와서 작업할지 선언. <이미지명>:<태그>로 작성

### MAINTAINER
DockerFile 제작한 사람의 정보

### ARG
빌드 시점에 쓰는 변수. 

### ENV
런타임 환경변수.

### EXPOSE
Host에 연결될 Port 지정.

### RUN
- docker image가 실행되고 container 내에서 실행될 명령어를 적는다.
- 주로 패키지 설치하는 명령에 씀
- 해당 내용은 적으면 적을수록 container label 이 적게 생성되어 image 를 compact 하게 생성 할 수 있다.

### CMD
- 컨테이너가 시작되었을 때 스크립트 혹은 명령을 실행.
- 만약 docker run 으로 실행될 때 사용자가 직접 실행할 명령어를 입력하면 CMD의 명령어는 실행되지 않음
- 컨테이너를 실행할때 인자값을 주면 Dockerfile에 지정된 CMD 값을 대신해 지정한 인자값으로 변경하여 실행

### ENTRYPOINT
- 해당 컨테이너가 수행될 때 반드시 ENTRYPOINT에 속한 명령을 수행하도록 지정된다
- 컨테이너가 시작되었을 때 스크립트 혹은 명령을 실행
- Dockerfile에서 한 번만 정의 가능

~~~
1) RUN
- 새로운 layer를 생성하거나, 생성된 layer 위에서 command를 실행. 
- (package를 설치하는 명령어를 주로 사용)

2) CMD
- docker가 실행될때 실행되는 명령어 정의
- 만약 docker run 으로 실행될 때 사용자가 직접 실행할 명령어를 입력시
  CMD의 명령어는 실행되지 않는다.
- ex) docker run <image_name> echo world
  "world" 가 출력됨.
  Dockerfile에 있는 CMD는 실행되지 않음

3) ENTRYPOINT
- docker run으로 생성하거나, 
  docker start로 중지된 container를 시작할 때 실행되는 명령어
  (CMD와 동일한 역할)
- Dockerfile 내에서 1번만 정의 가능함.
- CMD와 다른 점으로 
  docker run으로 실행시 command를 입력하면, 
  ENTRYPOINT의 파라미터로 인식한다. 
- ex) docker run <image_name> echo world
  "echo world"를 파라미터로 인식함
  
[출처] [docker] RUN vs CMD vs ENTRYPOINT | 작성자 freepsw
~~~


참고
- [RUN vs CMD vs ENTRYPOINT](http://blog.naver.com/PostView.nhn?blogId=freepsw&logNo=220982529575)
- [Docker RUN vs CMD vs ENTRYPOINT](https://goinbigdata.com/docker-run-vs-cmd-vs-entrypoint/)
- [ENV와 ARG 비교](https://knight76.tistory.com/entry/docker-ENV%EC%99%80-ARG-%EB%B9%84%EA%B5%90)
- [RUN / CMD / Entry Point 차이점](https://show-me-the-money.tistory.com/46)
- [Dockerfile Entrypoint 와 CMD의 올바른 사용 방법](https://bluese05.tistory.com/77)
