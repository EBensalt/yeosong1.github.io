### 도커파일 문법 ( 쓰는 중~~~~)

**FROM**
Docker Hub 에서 어떤 이미지를 가지고와서 작업할지 선언합니다. 작성법은 <이미지명>:<태그> 로 작성합니다.

**MAINTAINER**
DockerFile 제작한 사람의 정보를 기입합니다.

**RUN**
docker image 가 실행되고 container 내에서 실행될 명령어입니다.

해당 내용은 적으면 적을수록 container label 이 적게 생성되어 image 를 compact 하게 생성 할 수 있습니다.

**CMD**
CMD는 컨테이너가 시작되었을 때 스크립트 혹은 명령을 실행합니다.
즉 docker run 명령으로 컨테이너를 생성하거나, docker start 명령으로 정지된 컨테이너를 시작할 때 실행됩니다.
CMD는 Dockerfile에서 한 번만(!) 사용할 수 있습니다.

**EXPOSE**
Host 에 연결될 Port를 지정합니다.
