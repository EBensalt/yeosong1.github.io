# minishell

[서브젝트 - 2020/11/20 에 다운로드 했음](https://github.com/yeosong1/yeosong1.github.io/files/5571503/en.subject.pdf)


요약 :
이 프로젝트의 목적은 간단한 셸을 만드는 것입니다.  <br>
네, 당신의 자신의 작은 bash 또는 zsh이요.       <br>
프로세스와 파일 디스크립터에 대해 많이 배우게 될 것입니다.

# [수동 테스트 체크리스트](미니쉘수동테스트.md) 바로가기

# 소개
쉘의 존재는 `IT`의 존재와 직결되어 있습니다.  <br>
당시 모든 코더는 *정렬된 1/0 스위치를 사용하여 컴퓨터와 통신하는 것은 진심으로 성가시다는 것* 에 동의했습니다.  <br>
*어떤 식으로든 영어에 가까운 대화형 명령줄을 사용해서 컴퓨터와 소통하겠다* 는 아이디어를 내놓은 것은 정말 논리적이었습니다.  <br>  <br>

Minishell을 통해, 당신은 시간을 거슬러올라가 Windows가 존재하지 않았을 때 사람들이 직면했던 문제를 만나는 여행을 할 수 있습니다.

# Common Instruction

<details>
<summary> <b> 평소랑 같아요(펼쳐보기)</b>  </summary>
<div markdown="1">

- 귀하의 프로젝트는 규범에 따라 작성되어야합니다. 보너스가있는 경우 파일 / 함수, 표준 검사에 포함되며 해당하는 경우 0을 받게됩니다.
내부의 표준 오류입니다.
- 기능이 예기치 않게 종료되지 않아야합니다 (세그먼트 오류, 버스 오류, 이중 free 등) 정의되지 않은 동작을 제외하고. 이런 일이 발생하면 프로젝트는
작동하지 않는 것으로 간주되며 평가 중에 0을 받게됩니다.
- 필요한 경우 모든 힙 할당 메모리 공간을 적절히 해제해야합니다. 누출 없음 용인 될 것입니다.
- 주제가 요구하는 경우, 당신의 컴파일을위한 Makefile을 제출해야합니다. -Wall, -Wextra 및 -Werror 플래그를 사용하여 필요한 출력에 소스 파일을 추가합니다.
Makefile **relink 안되게 하세요** ~~(평가하면 다들 리링크 되던데......................)~~ 
- Makefile에는 최소한 $ (NAME), all, clean, fclean, re.
- 프로젝트에 보너스를 적용하려면 Makefile에 규칙 보너스를 포함해야합니다. 금지 된 모든 다양한 헤더, 라이브러리 또는 기능을 추가합니다. 프로젝트의 주요 부분. 보너스는 다른 파일 _bonus. {c / h}에 있어야합니다.
필수 및 보너스 부품 평가는 별도로 수행됩니다.
- 프로젝트에서 libft를 사용할 수있는 경우 해당 소스와 libft 폴더의 관련 Makefile과 관련 Makefile. 프로젝트 Makefile은 Makefile을 사용하여 라이브러리를 컴파일 한 다음 프로젝트를 컴파일해야합니다.
- 이 작업에도 불구하고 프로젝트에 대한 테스트 프로그램을 만드는 것이 좋습니다. 제출할 필요가 없으며 채점되지 않습니다. 그것은 당신에게 기회를 줄 것입니다
자신의 작업과 동료의 작업을 쉽게 테스트 할 수 있습니다. 특히 그 테스트를 찾을 수 있습니다 수비 중에 유용합니다. 실제로 수비 중에는 테스트를 자유롭게 사용할 수 있습니다.
및 / 또는 평가중인 동료의 테스트.
- 할당 된 git 저장소에 작업을 제출합니다. git 저장소의 작업 만 등급이 지정됩니다. Deepthought가 작업 등급을 지정하면 완료됩니다. 동료 평가 후. 작업 중에 오류가 발생한 경우 Deepthought의 채점, 평가가 중지됩니다.
</div>
</details>
<br>

# 필수 파트

- [x] 프로그램 이름이 minishell 인가?
- [x] Makefile이 있는가?
- [x] Libft 사용 가능합니다.
- [x] 한 마디로: shell을 작성하세요.

<details>
<summary> <b> 🚩 사용 가능한 함수의 목록과 간단한 설명 (펼쳐 보기) </b>  </summary>
<div markdown="1">

| 이름 | 형태 | 사용처 | 헤더 |
| --- | --- | ---  | --- |
| **malloc**  | `void * malloc(size_t size);`                               | 메모리 할당할 때         |stdlib.h |
| **free**    | `void free(void *ptr);`                                     | 할당한 메모리 해제할 때    | stdlib.h |
| **write**   | `ssize_t write(int fildes, const void *buf, size_t nbyte);` | 콘솔 쓰기              | unistd.h |
| **open**| `int open(const char *path, int oflag, ...);`               | 시스템콜 파일 오픈       | fcntl.h  |
| **read**| `ssize_t read(int fildes, void *buf, size_t nbyte);`        | 시스템콜 파일 읽기       | sys/types.h sys/uio.h unistd.h |
| **close**   |`int  close(int fildes);`                                | 시스템콜 파일 닫기       | unistd.h|
| **fork**    |`pid_t fork(void);`                            | 부모 프로세스와 같은 자식 프로세스를 복제 |unistd.h |
|wait|`pid_t wait(int *stat_loc);`| 부모가 자식 프로세스를 끝나기 기다리는 함수, 자식 프로세스로부터 signal 받으면 종료되면서, 부모 프로세스에 SIGCHLD 시그널이 보내짐. 자식이 에러면 -1, 정상이면 0 리턴 | sys/wait.h |
|         |                         | 자식 프로세스 끝난 걸 확인하면 그 뒤에 줄에 있는 부모 프로세스의 남은 코드를 진행한다.||
| **waitpid** | `pid_t waitpid(pid_t pid, int *stat_loc, int options);` | 프로세스pid가 종료되길 기다리는 함수.  | sys/wait.h |
|             | 기다리는 PID가 종료되지 않아서 즉시 종료 상태를 회수 할 수 없는 상황에서 호출자는 차단되지 않고 반환값으로 0을 받음 | 세번째 인자로 사용 가능한 상수 중 WNOHANG에 대한 설명이다 |[예](https://codetravel.tistory.com/42) |
| wait3   | `pid_t wait3(int *statloc, int options, struct rusage *rusage);` |자식종료 기다리며, 종료된 프로세스의 상태/자원 사용량을 알려줌|sys/wait.h|
| wait4   | `pid_t wait4(pid_t pid, int *statloc, int options, struct rusage *rusage);`|위와 같음?|sys/wait.h|
| **signal**  | `void (*signal(int sig, void (*func)(int)))(int);` |시그널 = 프로세스에 뭔가 발생했음을 알리는 소프트웨어 인터럽트(software interrupt) |signal.h|
|         | 또는 읽기 쉽게 `typedef void (*sig_t) (int);` 해서 아래처럼 |함수를 사용하면 신호를 잡거나 무시하거나 인터럽트를 생성합니다. sig table은 <signal.h> 파일에 있다||
|         | `sig_t   signal(int sig, sig_t func);`               |||
|         | [사용 예제](https://www.joinc.co.kr/w/Site/system_programing/Book_LSP/ch06_Signal). 들어온 sig넘버를 인자로 받는 핸들링 함수를 만들어서 연결해주면 된다. |||
| kill    | |||
| **exit**    | `void exit(int status);`|||
| **getcwd**  |  `char *getcwd(char *buf, size_t size);` |Current Working Directory의 절대 경로 이름을 복사 | unistd.h |
| **chdir**   |`int chdir(const char *path);`|명명된 디렉토리가 현재 작업 디렉토리가 되도록 change. 경로 이름이 아닌 경로 검색의 시작점이어서 슬래시`/`로 시작한다. | unistd.h |
| **stat**    |`int stat(const char *restrict path, struct stat *restrict buf);` | path가 가리키는 파일에 대한 정보를 얻음 |sys/stat.h|
| lstat   | |||
| fstat   | |||
| **execve**|`int execve(const char *path, char *const argv[], char *const envp[]);` | 호출 프로세스를 새 프로세스로 변환 |unistd.h|
| dup     | `int dup(int fd);`| 받은 fd를 복제해서 반환. 접근 경로가 2개가 되는 거라서 한 쪽 닫아도 남은 fd는 개별적으로 사용 가능. |unistd.h|
| **dup2** | `int dup2(int fd, int fd2);` | fd2를 부르면, fd로 리디렉션 해줌.| [예](https://sosal.kr/186)| 
| **pipe**| `int pipe(int fildes[2]);` | 프로세스간 데이터를 주고받을 수 있게 해줌. 부모->자식 단방향 파이프를 생성. 부모가 fd[1]로 쓰고, 자식이 fd[0]로 읽고.|[패캠 설명](https://www.fastcampus.co.kr/courses/2466/clips/)|
| opendir | |||
| readdir | |||
| closedir | |||
| **strerror**|`char *strerror(int errnum);` | 에러넘버 인자인 errnum을 받아서 해당 메시지 문자열에 대한 포인터를 반환|stdio.h|
| errno   | |||

</div>
</details>
<br>

당신의 쉘은 반드시:

- [x] 새 명령을 기다릴 때 프롬프트 표시
- [x] 올바른 실행(executable) 파일을 검색하고 실행합니다(실행 파일 - PATH 변수 또는 상대 또는 절대 경로를 기반으로) (bash에서와 같이)
- [x] 내장 기능(빌트인)을 구현해야합니다. (bash에서와 같이)
  - [x] `echo` ’-n’옵션도
  - [x] `cd` 상대 또는 절대 경로만
  - [x] `pwd` 옵션 없이
  - [x] `export` 옵션 없이(=인자 있을 수 있음)
  - [x] `unset` 옵션 없이(=인자 있을 수 있음)
  - [x] `env` 옵션 및 인자 없이
  - [x] `exit` 옵션 없이(=인자 있을 수 있음)
- [x] 명령에서의 `;`는 명령을 분리해야합니다. (bash에서처럼)
- [x] `'`, `"`는 **multiple line 명령을 제외하고** bash에서처럼 작동해야합니다.
- [x] 리디렉션 `<`, `>`, `>>`은 **file descriptor aggregation를 제외하고** bash에서처럼 작동해야합니다.
- [x] 파이프 `|` (bash에서처럼)
- [x] 환경 변수 `$문자` (bash에서처럼)
- [x] `$?`(bash에서처럼)
- [x] `ctrl-C`, `ctrl-\`, `ctrl-D`는 bash와 동일한 결과를 가져야합니다.



# 보너스 파트

- 필수 부분이 완벽하지 않다면 보너스에 대해 생각조차하지 마세요
- 모든 보너스를 할 필요는 없습니다.

- [ ] `<<` 리디렉션 (bash에서와 같이)
- [ ] Termcaps를 사용한 히스토리 및 줄 편집 (예 : man tgetent)
  - [ ] 커서가 있는 줄을 편집합니다. 특정 위치에서 선을 편집 할 수 있도록 커서를 왼쪽과 오른쪽으로 이동합니다. 분명히 기존 문자 사이에 새 문자를 삽입해야합니다. 고전적인 껍질과 비슷합니다.
  - [ ] 위쪽 및 아래쪽 화살표를 사용하여 명령 내역을 탐색합니다. 그런 다음 원하면 편집 할 수 있습니다 (히스토리 말고 선).
  - [ ] 원하는 키 시퀀스를 사용하여 줄의 전체 또는 일부를 잘라 내고 복사하고 / 또는 붙여 넣습니다.
  - [ ] Ctrl + LEFT를 사용하여 왼쪽 또는 오른쪽으로 단어 단위로 직접 이동하고 Ctrl + 오른쪽.
  - [ ] 홈과 끝을 눌러 줄의 시작 또는 끝으로 바로 이동합니다.
  - [ ] 몇 줄에 걸쳐 명령을 작성하고 편집합니다. 이 경우 우리는 그것을 좋아할 것입니다 ctrl + UP 및 ctrl + DOWN을 사용하면 명령에서 한 줄에서 다른 줄로 이동할 수 있습니다. 동일한 열 또는 그렇지 않으면 가장 적합한 열에 남아 있습니다.
- [ ] &&, &#124;&#124; 우선 순위 괄호 (bash에서와 같이)
- [ ] wilcard * (bash에서와 같이)


------------------------------------

# 일반 파트 작성을 위한 배경 지식 & 구현 과정


##  새 명령을 기다릴 때 프롬프트 표시

## 올바른 실행(executable) 파일을 검색, 실행
실행 파일 - PATH 변수 또는 상대 또는 절대 경로를 기반으로

## 내장 기능(빌트인) - 만들어서 넣으라는 조건.

### echo
- 옵션 없이 : echo 후 `\n` 
- -n : echo 후 `\n`없음

### cd
- 상대 또는 절대 경로만
- 주어진 경로로 current working directory를 이동시킨다.
- chdir();

### pwd
- current working directory의 경로를 보여준다
- getcwd();

### export
- 옵션 없이: 옵션 없이는 그냥 목록을 보여준다.
- 옵션 없이 인자랑: 사용자 환경 변수를 전역 변수로 등록해준다.
  - export NAME=VALUE 하고 env 해서 목록을 확인해보기, echo $NAME 해서 확인 해보기
 
### unset
- 옵션 없이 인자랑: 환경변수 설정 했던 걸 지워줌
  - unset NAME 하고 잘 지워졌나 확인..

### env

쉘에서 쓸 때
1. **`env` ->  전역 환경 변수의 목록을 출력한다.** -> `int main( ... char **envp)`에서 받아온 걸 출력해주면 됨 
2. `env 프로그램이름` -> 환경 변수 목록에 설정된 
3. 이외에도 여러가지 옵션에 따른 기능이 있는데, -i 옵션(무시하기) 말고는 거의 쓸 일이 없다는 듯.

스크립트에서 쓸 때
1. [쉬뱅](https://ko.wikipedia.org/wiki/%EC%85%94%EB%B1%85)뒤에 범위를 제한/지정하기 위해 쓸 때.
- 예를 들면 `#!/usr/bin/env sh` 하면 $PATH에 있는 첫 번째 sh을 불러서 쓰게 됨. [참고](https://en.wikipedia.org/wiki/Shebang_(Unix)#Portability)

아무튼 우리가 이번에 구현할 것은 1번만!

#### 사용처
- env : 전역 변수 등록/조회
- set : 사용자 환경 변수 등록/조회
- export : 사용자 환경 변수를 전역 변수로 등록

### exit
옵션 없이


## `;` 로 명령 분리

## 따옴표 `'`, `"`
여러 줄 명령을 제외하고 bash 에서처럼 작동해야합니다.

## 리디렉션 `<`, `>`, `>>`
<< (file descriptor aggregation)을 제외하고 작동해야합니다.

[쉘에서 리디렉션](https://www.gnu.org/software/bash/manual/html_node/Redirections.html)

`>` : 표준 출력
  - 명령 > 파일 : 명령의 결과를 파일로 저장 (write or overwrite)
`>>` : 표준 출력
  - 명령 >> 파일 : 명령의 결과를 기존 파일 데이터에 추가 (append)
`<` : 표준 입력
  - 명령 < 파일 : 파일의 데이터를 명령에 입력

## 파이프 `|`

## 환경 변수 `$어쩌구`

## \$\?

## ctrl-C, ctrl-\\, ctrl-D
  - 이것은 키보드 인터럽트 처리 루틴을 잘 작동 시킬 수 있는가에 대한 부분이다.
  - `ctrl-C` == Ctrl+C (^C) 는 "interrupt" 를 의미한다. 실질적으로는 stop 명령을 내리는 SIGINT 를 보낸다.
  - `ctrl-\` == 비슷하게 Ctrl+\ (^\) 로 SIGQUIT을 보내서 종료할 수도 있다.
    - SIGINT, SIGQUIT 과 같은 인터럽트 시그널의 매크로 이름과 해당하는 번호는 signal.h에 정의되어 있다.
  - `ctrl-D` == Ctrl+D (^D) 는 "end of file"을 의미한다. 터미널이 입력 상태이고, 라인의 맨 처음일 때에만 작동한다.
  - 그냥 참고: `Ctrl+Z` (^Z) 는 현재 진행 중인 작업을 백그라운드로 보낸다. (종료되는 것이 아니다)
  - 출처 : [꿀벌개발일지](https://ohgyun.com/330)


- C main의 인자들
  - int argc 인자 개수
  - char *argv[] 인자 이름들
  - char *envp[] 환경변수들
