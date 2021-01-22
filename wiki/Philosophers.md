# 철학자 문제

![e4aaaceb97ced5d64e45c00cba625ced1581be4102444d75ddd2e71777585e19ece6c259fd810094d0795500c8dccc47586eff035f27ad3f5be6cf1c6d18ecaed60d637fbbdb7b7a8dba2cead18463f8cf9e55ab51cef02834d0b3bcf2f3b0708a0c6af7768c5d3d41b72f330fa20b1c](https://user-images.githubusercontent.com/53321189/105286724-38edeb00-5bfa-11eb-9012-8da0f629b870.png)


철학자 문제는 여러 프로세스 혹은 스레드가 동시에 작동할 때 발생하는 아래 두 문제를 이해하기 좋은 문제이다. <br>

  1) **동시성 문제(동기화 문제)** = **자원을 공유**하는 병렬 처리 시스템에서 의도하지 않은 **데이터 변질**이 발생하는 것.<br>
  2) **교착 상태(데드락)** = 오도가도 못하고 **아무것도 못하는 상태** = 아래 4가지 조건이 **동시에 충족**할 때를 말한다.<br>
  
  ~~~
    - 상호배제(Mutual exclusion) : 프로세스들이 필요로 하는 자원에 대해 배타적인 통제권을 요구한다.
    - 점유대기(Hold and wait) : 프로세스가 할당된 자원을 가진 상태에서 다른 자원을 기다린다.
    - 비선점(No preemption) : 프로세스가 어떤 자원의 사용을 끝낼 때까지 그 자원을 뺏을 수 없다.
    - 순환대기(Circular wait) : 각 프로세스는 순환적으로 다음 프로세스가 요구하는 자원을 가지고 있다.
  ~~~
  
유튜브, 구글에 [식사하는 철학자 문제](https://ko.wikipedia.org/wiki/식사하는_철학자들_문제)를 검색해보세요.

## 필수파트

당신은 3개의 다른 프로그램을 작성해야하지만 기본 규칙은 동일합니다.

- 이 프로젝트는 Norm에 따라 C로 코딩됩니다.
  - 모든 누수, crash, UB, 놈 에러는 0점 처리 됩니다.
- 많은 철학자들이 원탁에 앉아 세 가지(먹기, 자기, 생각하기) 중 하나를 수행하고 있습니다.
- 한 번에 한 동작만 할 수 있습니다. (먹는 동안에는 생각하거나 잠을 자지 않고, 잠자는 동안에는 먹지 않습니다. 또는 생각하고 물론 생각하는 동안 그들은 먹거나 자지 않습니다.)
- 자원을 공유하며 자원까지의 거리는 같습니다. (철학자들은 중앙에 커다란 스파게티 그릇이있는 원형 테이블에 앉아 있습니다.)
- 테이블에 약간의 포크가 있습니다.
- 스파게티는 포크 하나로 제공하고 먹기가 어렵기 때문에 **철학자는 각 손에 하나씩 두 개의 포크**로 먹어야합니다.
- [기아 상태](https://ko.wikipedia.org/wiki/기아_상태)가 되면 안됨. (철학자들은 절대 굶주려서는 안됩니다.)
- 모든 철학자는 먹어야합니다.
- 철학자들은 서로 이야기하지 않습니다. 
- 철학자들은 다른 철학자가 언제 죽을지 모릅니다.
- 철학자는 식사를 마칠 때마다 포크를 떨어 뜨리고 잠을 자기 시작합니다.
- 철학자가 자고 나면 생각하기 시작합니다.
- 철학자가 죽으면 시뮬레이션이 중지됩니다.
- 각 프로그램에는 동일한 옵션이 있어야합니다.
  - 인자 1 `number_of_philosophers` : n명의 철학자. n개의 포크.
  - 인자 2 `time_to_die` : 아래의 경우 죽습니다.
    - 마지막 식사를 시작하고 n 밀리 초 안에 먹기 시작하지 않거나
    - 시뮬레이션을 시작한 후 n 밀리 초 안에 먹기 시작하지 않으면
  - 인자 3 `time_to_eat` : 철학자가 먹는데에 걸리는 n 밀리 초. 그 시간동안 그는 두 개의 포크를 유지해야합니다.
  - 인자 4 `time_to_sleep` : 철학자가 자는 데에 할애하는 n 밀리 초.
  - 인자 5 `number_of_times_each_philosopher_must_eat` : 이 인자는 선택 사항입니다.
    - 모든 철학자가 n번을 다 먹으면 시뮬레이션이 중지됩니다.
    - 지정하지 않으면 시뮬레이션은 철학자가 죽었을 때만 중지됩니다.
- 각 철학자는 **1부터 `number_of_philosophers`까지**의 숫자를 지정해야합니다.
- 철학자 1은 철학자 `number_of_philosophers`옆에 있습니다.
- 다른 철학자 N은 철학자 N-1과 철학자 N+1사이에 앉아있습니다.
- 철학자의 status 변경은 다음과 같이 작성되어야합니다.
  - (X는 철학자 번호로 교체, `timestamp_in_ms`는 현재 타임 스탬프를 밀리 초로 나타낸 것)

  - timestamp_in_ms X has taken a fork
  - timestamp_in_ms X is eating
  - timestamp_in_ms X is sleeping
  - timestamp_in_ms X is thinking
  - timestamp_in_ms X died
  
  - 예시 : 122400 5 died
  - 근데 구현한 것들 보니 122400ms 5 died 이렇게 단위 붙였더라
  
- 출력된 status는 다른 철학자의 status와 뒤섞이거나 얽혀서는 안됩니다.
- '철학자의 죽음'과 '죽었다고 출력하기' 사이는 10ms를 초과 할 수 없습니다. (출력 오차 허용 범위 +10ms 이내)
- 다시 말하지만 철학자들은 죽는 것을 피해야합니다!

### phillo_one
- Makefile
- 제출 파일: phillo_one/ 폴더
- 들어올 인자 :
  - number_of_philosophers
  - time_to_die
  - time_to_eat
  - time_to_sleep
  - [number_of_times_each_philosopher_must_eat]
- external 함수 :
  - memset
  - alloc
  - free
  - write
  - usleep
  - gettimeofday
  - pthread_create
  - pthread_detach
  - pthread_join
  - pthread_mutex_init
  - pthread_mutex_destroy
  - pthread_mutex_lock
  - pthread_mutex_unlock
- libft 쓰지 말기
- 내용: 스레드, 뮤텍스 쓰는 철학자

이 버전에서 일반적이지 않은 규칙은 다음과 같습니다.

- 각 철학자 사이에 포크가 하나씩 있으므로 각 철학자의 왼쪽, 오른쪽에 포크가 있습니다.
- 철학자들이 포크를 복제하지 않도록 하려면 포크 각각에 대한 뮤텍스로 포크 state를 보호해야합니다.
- 각 철학자는 스레드가 되어야합니다.


### phillo_two
- Makefile
- 제출 파일: phillo_two/ 폴더
- 들어올 인자 :
  - number_of_philosophers
  - time_to_die
  - time_to_eat
  - time_to_sleep
  - [number_of_times_each_philosopher_must_eat]
- external 함수 :
  - memset
  - alloc
  - free
  - write
  - usleep
  - gettimeofday
  - pthread_create
  - pthread_detach
  - pthread_join
  - sem_open
  - sem_close
  - sem_post
  - sem_wait
  - sem_unlink
- libft 쓰지 말기
- 내용: 스레드, 세마포어 쓰는 철학자

이 버전에서 일반적이지 않은 규칙은 다음과 같습니다.

- 모든 포크는 테이블 중앙에 있습니다.
- 메모리에는 state가 없지만 사용 가능한 포크 수는 세마포어로 표시됩니다.
- 각 철학자는 스레드가 되어야 합니다.


### phillo_three
- Makefile
- 제출 파일: phillo_three/ 폴더
- 들어올 인자 :
  - number_of_philosophers
  - time_to_die
  - time_to_eat
  - time_to_sleep
  - [number_of_times_each_philosopher_must_eat]
- external 함수 :
  - memset
  - alloc
  - free
  - write
  - usleep
  - gettimeofday
  - pthread_create
  - pthread_detach
  - pthread_join
  - sem_open
  - sem_close
  - sem_post
  - sem_wait
  - sem_unlink
- libft 쓰지 말기
- 내용: 프로세스들, 세마포어 쓰는 철학자

이 버전에서 일반적이지 않은 규칙은 다음과 같습니다.

- 모든 포크는 테이블 중앙에 있습니다.
- 메모리에는 state가 없지만 사용 가능한 포크 수는 세마포어로 표시됩니다.
- 각 철학자는 프로세스여야하며 메인 프로세스는 철학자가 아니어야합니다.

## 배경 지식 정리
- [스레드 + 사용함수](pthread.md) 
- [원격평가시 출력오차 허용범위인 10ms를 초과하게 되는 오류](스레드대기.md)
- [새로운 함수(usleep, gettimeofday)](usleep_gettimeofday.md)
- [뮤텍스 + 사용함수](mutex.md)
- [세마포어 + 사용함수](semaphore.md)
- 초(s), 밀리초(ms), 마이크로초(us), 나노초(ns)
  - 1 초
  - 1 밀리초 = 1/1000 초
  - 1 마이크로초 = 1/(1000x1000) 초
  - 1 나노초 = 1/(1000x1000x1000) 초
