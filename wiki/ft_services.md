# ft_services 수정중

요약: 이것은 시스템 관리 및 네트워킹 프로젝트 입니다.

## 🌋 ㅇㅏ 일단 도커 업데이트 하지 마세요.... 그냥 쓰세요..

저 지금 **카탈리나** 쓰고있는데 **도커 최신**으로 업데이트 했다가 [문제](https://github.com/docker/for-mac/issues/5037)가 발생했고, (그냥 실행 자체가 안됨)<br>
[여기](https://docs.docker.com/docker-for-mac/release-notes/#docker-community-edition-17120-ce-mac49-2018-01-19) 들어가서 **Docker 19.03.12 (2020-07-27) 버전으로 다운그레이드**하고 이제 동작하네요..



## 소개
- Ft_services는 Kubernetes를 소개합니다.
- 클러스터 관리 및 Kubernetes를 사용한 배포를 알게됩니다.
- 당신은 네트워크를 가상화하고 "클러스터링"을 수행할 것입니다.

## General Instruction
- 서버 구성에 필요한 모든 파일을 srcs 폴더에 넣어야합니다. 
- setup.sh 파일은 저장소의 루트에 있어야합니다. 이 스크립트는 모든 응용 프로그램을 셋업합니다.
- 이 서브젝트는 기존의 / 새로운 practices 둘 다 요구 합니다.
- 따라서 많은 양의 문서를 읽는 것을 두려워하지 않는 편이 좋습니다.
- (문서: Docker, Kubernetes, 그외 기타 프로젝트에 유용한 모든 문서들)



**서브젝트 버전: 2020/11/19 다운로드*

## 필수 파트

이 프로젝트는 다양한 서비스의 인프라를 설정하는 것으로 구성됩니다. 이걸 하려면
- 당신은 `Kubernetes`를 사용해야합니다.
- 다중 서비스 클러스터를 설정해야합니다.
- **각 서비스는 전용 컨테이너에서 실행되어야합니다.**
- 각 컨테이너는 관련 서비스와 성능을 위해 반드시 **동일한 이름**을 가져야합니다. Alpine Linux를 사용하여 컨테이너를 빌드해야하기 때문입니다.
- 또한 setup.sh에서 호출되는 **Dockerfile**이 작성되어 있어야합니다.
- 사용할 이미지를 직접 구축해야합니다. 복용하는 것은 금지되어 있습니다.
- 이미 이미지를 빌드하거나 DockerHub와 같은 서비스를 사용합니다.

또한 다음을 설정해야합니다.
#### Kubernetes
  - Kubernetes 웹 `dashboard`가 클러스터 관리를 도와줄 것 입니다.
#### `로드 밸런서`
  - 서비스의 외부 액세스를 관리하는 `로드 밸런서`가 클러스터의 유일한 엔트리포인트가 될 것입니다.
  - 반드시 서비스에 맞는 포트를 유지해야합니다(Grafana는 IP:3000처럼).
  - `로드 밸런서`는 단일 IP를 갖습니다.
#### `워드프레스`
  - `워드프레스` 웹 사이트는 5050 포트를 수신
  - `워드프레스`는 MySQL 데이터베이스랑 같이 작동 
  - 두 서비스 모두 별도의 컨테이너에서 실행되어야합니다.
  - `워드프레스` 웹 사이트에는 여러 사용자와 관리자를 갖습니다.
  - `워드프레스` 자체 nginx 서버가 필요합니다.
  - `로드 밸런서`는 이 서비스로 직접 리디렉션 할 수 있어야 합니다.
#### `phpMyAdmin`
  - `phpMyAdmin`은 포트 5000 수신
  - `phpMyAdmin`은 MySQL 데이터베이스와 연결됩니다.
  - `PhpMyAdmin`에는 자체 nginx 서버가 필요합니다.
  - `로드 밸런서`는 이 서비스로 직접 리디렉션 할 수 있어야 합니다.
#### `nginx`
  - `nginx` 서버가 있는 컨테이너는 포트 80과 443을 리스닝합니다.
  - 포트 80은 http이고, https인 443으로의 시스템적인 리디렉션(301)이어야합니다.
  - 표시되는 페이지는 http 오류만 아니면 중요하지 않습니다.(=오류만 아니면 맘대로)
  - 이 컨테이너는 `/wordpress` 경로에 대한 접근을 허용합니다. IP:WPPORT로 리디렉션(307)하는 접근을.
  - 또한 `/phpmyadmin`로의 접근을 허용해야합니다. `리버스 프록시`로 IP:PMAPORT로.
#### `FTPS`
  - FTPS 서버는 포트 21을 리스닝(수신 대기).
#### `Grafana` 
  - `Grafana` 플랫폼은 포트 3000을 리스닝.
  - `Grafana` 플랫폼은 `InfluxDB` 데이터베이스와 연결됨
  - `Grafana`는 모든 컨테이너를 모니터링합니다.
  - 서비스 하나 당 대시 보드를 하나 만들어야합니다.
  - `InﬂuxDB`와 `Grafana`는 두 개의 별개 컨테이너에 있어야합니다.
#### data persist
  - 두 데이터베이스 컨테이너 중 하나가 터지거나 중지되는 경우에, 데이터가 지속되는지 확인해야합니다.
####  SSH
  - SSH에 로그인하여 nginx 컨테이너에 액세스 할 수 있어야합니다.
#### restart
  - 모든 컨테이너는 컴포넌트 중 하나가 터지거나 중지되는 경우에, 반드시 restart해야 합니다.

서비스에 대한 각 리디렉션이 로드 밸런서를 사용하여 수행되는지 확인하십시오. <br>
FTPS, Grafana, Wordpress, PhpMyAdmin, nginx의 kind는 "LoadBalancer"여야합니다. <br>
Inﬂuxdb MySQL의 kind는 "ClusterIP"여야합니다. <br>
다른 엔트리들이 있을 수 있지만 kind가 "NodePort"가 될 수는 없습니다. <br> <br>



다음은 설정해야하는 항목의 예입니다.

<img width="708" alt="스크린샷 2020-11-20 오후 1 05 25" src="https://user-images.githubusercontent.com/53321189/99756721-1a537100-2b31-11eb-94a1-0cec379337bd.png">

동그라미는 유저가 접근할 수 있는 것들이고, 네모는 유저는 불가, 관리자만 접근 가능합니다.



### ❗주의

~~~
Node Port 서비스, Ingress Controller 오브젝트, kubectl 포트 포워드 명령은 금지됩니다.
Load Balancer가 클러스터의 유일한 엔트리포인트여야 합니다.
~~~

### 💡참고

~~~
클라우드 제공 업체에서 제공하는 로드밸런서를 사용하려고 시도하는 건 쓸모 없어요.
당신은 MetalLB를 봐야합니다.
~~~


## 쿠버네티스 배경 쌓기

[44bits의 초보를 위한 쿠버네티스 안내서 유튜브](쿠버네티스_44초보.md)

## 시작하기

1. 홈브류가 없으면 홈브류 설치

~~~
curl -fsSL https://rawgit.com/gcamerli/42brew/master/set.sh | zsh
~~~

2. 미니큐브 설치

~~~
brew install minikube
~~~

3. 도커를 켠다. 화면 상단 바에 고래가 나오고, 눌렀을때 Docker Desktop is running 초록불이어야 된다. 주황불 no

4. 미니큐브를 시작해본다.

~~~
minikube start --driver=virtualbox
~~~

5. 버추얼박스를 켜서 창을 본다.

<img width="350" alt="kkkube" src="https://user-images.githubusercontent.com/53321189/104160662-d6942e00-5434-11eb-8a44-c15aa9d1627c.png">
<img width="350" alt="yskube" src="https://user-images.githubusercontent.com/53321189/104160670-da27b500-5434-11eb-80ee-5dd303f35cd2.png">

이렇게 실행중으로 나와야 잘 켜진 것

6.
