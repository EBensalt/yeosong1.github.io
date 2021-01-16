# ft_service 파드별 내용 정리..

## 시작

[1] vm 환경에서 mk를 연다?
minikube start --driver=virtualbox

[2] 도커의 환경변수를 미니큐브랑 공유해준다
eval $(minikube -p minikube docker-env)

[3] 나에게 필요한 플러그인을 추가한다.
mk addons metallb
mk addons metrics-server   -> 대시보드에서 CPU랑 램 사용량 보여주기
mk addons dashboard        -> 굳이 안해도 되는데 그냥 추가.. (왜?)

## IP 세팅

[4] MINIKUBE_IP=$(minikube ip)


## FTPS

- vsftpd = very secure ftp daemon
  - ftp = file transfer protocol
  - daemon = 맥스웰의 도깨비처럼 백그라운드에서 이것저것 운영하는 역할 
  - 맥스웰의 도깨비 = 이걸 설명하면 너무 길어진다.
