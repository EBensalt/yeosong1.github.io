### file descriptor
* 다음과 같이 open() 시스템 콜을 호출하면 반환값으로 파일 디스크립터를 얻을 수 있다.
<br>`int fd; fd = open("inout-file", O_RDONLY);`
* OPEN_MAX개 만큼 있는 이용자 파일 디스크립터 표의 구조체 중에서 몇 번째를 할당했는지를 나타내는 번호(정수).
* OPEN_MAX개의 구조체 중에서 0번째는 표준 입력, 1번째는 표준 출력, 2번째는 표준 에러 출력으로 처음부터 할당함.
* 즉, 프로그램을 실행시 3개의 표준 입출력이 자동적으로 설정되고, 새 파일을 액세스하는 경우엔 세 번째 이후의 구조체를 할당한다는 것.
* 그러니까 OPEN_MAX - 3 이 동시에 열 수 있는 파일 개수의 실제 최대값.

### OPEN_MAX 
* `getconf OPEN_MAX` can show your OS defined Environment Variables

지금 사용하고 있는 OS가 동시에 열 수 있는 최대 파일 개수를 알려주는 환경 변수이다.
