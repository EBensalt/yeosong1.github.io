# 철학자 문제

유튜브에 철학자 문제를 검색해 보세요.

## 필수파트

당신은 3개의 다른 프로그램을 작성해야하지만 기본 규칙은 동일합니다.

- 이 프로젝트는 Norm에 따라 C로 코딩됩니다.
  - 모든 누수, crash, UB, 놈 에러는 0점 처리 됩니다.
- 많은 철학자들이 원탁에 앉아 세 가지(먹기, 자기, 생각하기) 중 하나를 수행하고 있습니다.
- 한 번에 한 동작만 할 수 있습니다. (먹는 동안에는 생각하거나 잠을 자지 않고, 잠자는 동안에는 먹지 않습니다. 또는 생각하고 물론 생각하는 동안 그들은 먹거나 자지 않습니다.)
- 자원을 공유하며 자원까지의 거리는 같습니다. (철학자들은 중앙에 커다란 스파게티 그릇이있는 원형 테이블에 앉아 있습니다.)
- 테이블에 약간의 포크가 있습니다.
- 스파게티는 포크 하나로 제공하고 먹기가 어렵기 때문에 철학자는 각 손에 하나씩 두 개의 포크로 먹어야합니다.
- 철학자들은 절대 굶주려서는 안됩니다.
- 모든 철학자는 먹어야합니다.
- 철학자들은 서로 이야기하지 않습니다.
- 철학자들은 다른 철학자가 언제 죽을지 모릅니다.
- 철학자는 식사를 마칠 때마다 포크를 떨어 뜨리고 잠을 자기 시작합니다.
- 철학자가 자고 나면 생각하기 시작합니다.
- 철학자가 사망하면 시뮬레이션이 중지됩니다.
- 각 프로그램에는 동일한 옵션이 있어야합니다.
  - `number_of_philosophers` : 철학자의 수이자 포크 수입니다.
  - `time_to_die` : 철학자가 `time_to_die`를 먹기 시작하지 않는 경우 밀리 초 단위입니다. 
    - 마지막 식사를 시작하고 time_to_die 밀리 초 안에 먹기 시작하지 않거나
    - 시뮬레이션을 시작한 후 time_to_die 밀리 초 안에 먹기 시작하지 않으면
  - `time_to_eat` : 밀리 초 단위이며 철학자가 먹는데에 걸리는 시간입니다. 그 시간동안 그는 두 개의 포크를 유지해야합니다.
  - `time_to_sleep` : 밀리 초 단위이며 철학자가 자는 데에 할애하는 시간입니다.
  - `number_of_times_each_philosopher_must_eat` : 이 인자는 선택 사항입니다.
    - 모든 철학자가 최소한 `number_of_times_each_philosopher_must_eat`을 먹으면 시뮬레이션이 중지됩니다.
    - 지정하지 않으면 시뮬레이션은 철학자가 죽었을 때만 중지됩니다.
    
- 각 철학자는 1부터 `number_of_philosophers`까지의 숫자를 지정해야합니다.
- 철학자 1은 철학자 `number_of_philosophers`옆에 있습니다.
- 다른 철학자 N은 철학자 N-1과 철학자 N+1사이에 앉아있습니다.
- 철학자의 status 변경은 다음과 같이 작성되어야합니다.(X랑) (X는 교체된
철학자 번호와 timestamp_in_ms를 사용하여 현재 타임 스탬프 (밀리 초) ???
  - timestamp_in_ms X가 포크를 가졌습니다.
  - timestamp_in_ms X가 먹는 중입니다.
  - timestamp_in_ms X가 자는 중입니다.
  - timestamp_in_ms X가 생각 중입니다.
  - timestamp_in_ms X가 죽었습니다.

- 출력된 status는 다른 철학자의 status와 뒤섞이거나 얽혀서는 안됩니다.
- 철학자의 죽음과 죽었다고 출력하기 사이는 10ms를 초과 할 수 없습니다. (딜레이 오차 허용 범위 10ms)
- 다시 말하지만 철학자들은 죽는 것을 피해야합니다!

### phillo_one
- Makefile
- 제출 파일: phillo_one/ 폴더
- 인자 :
  - number_of_philosophers
  - time_to_die
  - time_to_eat
  - time_to_sleep [number_of_times_each_philosopher_must_eat]
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
- 인자 :
  - number_of_philosophers
  - time_to_die
  - time_to_eat
  - time_to_sleep [number_of_times_each_philosopher_must_eat]
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
- 인자 :
  - number_of_philosophers
  - time_to_die
  - time_to_eat
  - time_to_sleep [number_of_times_each_philosopher_must_eat]
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
- [pthreads](pthread.md) 
- [원격평가시 출력오차 허용범위인 10ms를 초과하게 되는 오류](스레드대기.md)
- [새로운 함수(usleep, gettimeofday)](usleep_gettimeofday.md)
- 뮤텍스 + 사용함수
- 세마포어 + 사용함수
