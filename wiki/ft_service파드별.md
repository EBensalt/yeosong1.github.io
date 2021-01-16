# 🧘‍♂ ft_service 쉘 스크립트에서 할 일......

## 시작

**[1] vm 환경에서 minikube를 깐다?**

~~~
minikube start --driver=virtualbox
~~~

**[2] 나에게 필요한 플러그인을 추가한다.**

~~~
minikube addons metallb
minikube addons metrics-server   -> 대시보드에서 CPU랑 램 사용량 보여주기
minikube addons dashboard        -> 굳이 안해도 되는데 그냥 추가.. (왜?)
~~~

## IP 세팅

**[1] 미니큐브 IP가 뭐가 할당될지 모르는데, 추후 여러 세팅 yaml 파일에서 동일한 IP가 쓰이므로 변수화 시켜둔다.**

~~~
MINIKUBE_IP=$(minikube ip)    또는
MINIKUBE_IP=`minikube ip`     이렇게 써도 된다. 둘 다 같은 기능이다.    
~~~

이제부턴 yaml 파일에 MINIKUBE_IP라고 적은 부분은 현재 할당 받은 IP인, minikube ip의 결과값으로 치환 가능하다.

**[2]** 기본 폼에 해당하는 `*_ip.yml`파일을 복사해서 `서비스이름.yaml`을 만든다.

~~~
cp ./srcs/metallb/metallb_ip.yaml ./srcs/metallb/metallb.yaml
cp ./srcs/ftps/ftps_ip.yaml ./srcs/ftps/ftps.yaml
cp ./srcs/ftps/srcs/vsftpd_ip.conf ./srcs/ftps/srcs/vsftpd.conf
cp ./srcs/grafana/grafana_ip.yaml ./srcs/grafana/grafana.yaml
cp ./srcs/mysql/wp_db_ip.sql ./srcs/mysql/wp_db.sql
cp ./srcs/nginx/nginx_ip.yaml ./srcs/nginx/nginx.yaml
cp ./srcs/nginx/srcs/nginx_ip.conf ./srcs/nginx/srcs/nginx.conf
cp ./srcs/phpmyadmin/phpmyadmin_ip.yaml ./srcs/phpmyadmin/phpmyadmin.yaml
cp ./srcs/wordpress/wordpress_ip.yaml ./srcs/wordpress/wordpress.yaml
cp ./srcs/wordpress/wp-config_ip.php ./srcs/wordpress/wp-config.php 
~~~

[3] `서비스이름.yaml` 내용중 등장하는 모든 MINIKUBE_IP를 아까 저장한 $MINIKUBE_IP에 담긴 IP로 치환한다.

~~~
sed -i '' "s/MINIKUBE_IP/$MINIKUBE_IP/g" ./srcs/metallb/metallb.yaml
sed -i '' "s/MINIKUBE_IP/$MINIKUBE_IP/g" ./srcs/ftps/ftps.yaml
sed -i '' "s/MINIKUBE_IP/$MINIKUBE_IP/g" ./srcs/ftps/srcs/vsftpd.conf
sed -i '' "s/MINIKUBE_IP/$MINIKUBE_IP/g" ./srcs/grafana/grafana.yaml
sed -i '' "s/MINIKUBE_IP/$MINIKUBE_IP/g" ./srcs/mysql/wp_db.sql
sed -i '' "s/MINIKUBE_IP/$MINIKUBE_IP/g" ./srcs/nginx/nginx.yaml
sed -i '' "s/MINIKUBE_IP/$MINIKUBE_IP/g" ./srcs/nginx/srcs/nginx.conf
sed -i '' "s/MINIKUBE_IP/$MINIKUBE_IP/g" ./srcs/phpmyadmin/phpmyadmin.yaml
sed -i '' "s/MINIKUBE_IP/$MINIKUBE_IP/g" ./srcs/wordpress/wordpress.yaml
sed -i '' "s/MINIKUBE_IP/$MINIKUBE_IP/g" ./srcs/wordpress/wp-config.php 
~~~

한 번 새로 띄울 때마다 `서비스이름.yaml`파일은 새로 만들어지는 셈이다. <br>
할당되는 IP가 매번 다르기 때문이다.

## SSL 인증서 세팅

~~~
openssl req -x509 -nodes -days 365\
		-newkey rsa:2048\
		-keyout "./srcs/nginx/srcs/nginx-selfsigned.key"\
		-out "./srcs/nginx/srcs/nginx-selfsigned.crt"\
		-subj "/CN=nginx_yeosong/O=nginx_yeosong"

openssl req -x509 -nodes -days 365\
		-newkey rsa:2048\
		-keyout "./srcs/ftps/srcs/ftps-selfsigned.key"\
		-out "./srcs/ftps/srcs/ftps-selfsigned.crt"\
		-subj "/CN=ftps_yeosong/O=ftps_yeosong"
~~~

인증서는 ft_server에서 했다..

## 도커 빌드, 쿠버네티스 apply

[1] 내 로컬 도커를 미니큐브 속 도커로 포인팅 한다.

(이걸 안하면 minikube 속 도커에 pull하고 싶은 이미지들이 내 로컬 도커로 pull이 되어버린다)

~~~
eval $(minikube -p minikube docker-env)
~~~

입력!


~~~
minikube docker-env하면
미니큐브의 도커 env가 나오고 그 밑에 다음과 같은 메시지를 볼 수 있다.

# To point your shell to minikube's docker-daemon, run:
# eval $(minikube -p minikube docker-env)

이 내용이었고, 혹시 이리 저리 확인해보고 싶다면

1. minikube ssh 들어가서 docker images 해보고 로컬에서 docker images 해서 내역을 비교해 볼 수도 있다.
2. minikube가 켜져있으면 상단 바 도커 아이콘 눌러서 밑에서 세번째 항목 kubernetes 보면
   minikube에 체크가 되어있을 것이다. minikube delete 후에는 없을 것..
   근데 새 터미널 창에서 해야 그때 그때 적용..
~~~

[2] metallb 띄우기

~~~
kubectl apply -f srcs/metallb/metallb.yaml
~~~

[3] 각 파드별 도커 빌드

~~~
        docker build -qt nginx-image srcs/nginx
        docker build -qt ftps-image srcs/ftps
        docker build -qt mysql-image srcs/mysql
        docker build -qt wordpress-image srcs/wordpress
        docker build -qt phpmyadmin-image srcs/phpmyadmin
        docker build -qt influxdb-image srcs/influxdb
        docker build -qt telegraf-image srcs/telegraf
        docker build -qt grafana-image srcs/grafana
~~~

[4] 각 파드의 서비스 띄우기

~~~
        kubectl apply -f srcs/metallb/metallb.yaml
        kubectl apply -f srcs/nginx/nginx.yaml
        kubectl apply -f srcs/ftps/ftps.yaml
        kubectl apply -f srcs/mysql/mysql.yaml
        kubectl apply -f srcs/wordpress/wordpress.yaml
        kubectl apply -f srcs/phpmyadmin/phpmyadmin.yaml
        kubectl apply -f srcs/influxdb/influxdb.yaml
        kubectl apply -f srcs/telegraf/telegraf.yaml
        kubectl apply -f srcs/grafana/grafana.yaml
~~~




## FTPS

- vsftpd = very secure ftp daemon
  - ftp? = file transfer protocol
  - daemon? = 맥스웰의 도깨비처럼 백그라운드에서 이것저것 운영하는 역할 
  - 맥스웰의 도깨비? = 이걸 설명하면 너무 길어진다..
