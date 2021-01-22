# 스레드

- 프로세스 보다 작은 실행 단위
- pthread = Posix thread
  - C 프로그램에서 스레드를 조작하는 표준 인터페이스
  - 모든 리눅스 시스템에서 사용 가능
  - 60여개의 함수가 있음.
- man 페이지 한글로 보기 -> [링크](http://neosrtos.com/docs/posix_api/pthread.html)
  - 정확히는 운영체제인 neos에서 사용하는 포식스 함수 man페이지..
  - POSIX 표준 호환이라고해서 내용이 거의 같을 거 같아 크게 훑어보는 용으로 썼다.
- 컴파일 할 때 `-lpthread` 넣어야

<br><br>

## 기본 생성 예제

```C
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>

void *thread(void *vargp);

int main()                                  /* 메인 스레드가 시작되었다 */
{
  pthread_t tid;                            /* 피어 스레드의 스레드ID를 저장하는 데에 쓸 것이다 */
  pthread_create(&tid, NULL, thread, NULL); /* 피어 스레드 1개를 생성했다. 이제 메인 스레드와 피어 스레드는 동시에 돌고있다 */
  pthread_join(tid, NULL);                  /* 메인 스레드가 피어 스레드의 종료를 기다린다 */
  exit(0);                                  /* 현재 프로세스에 돌고있는 모든 스레드를 종료한다. 현재의 경우, 메인 스레드 1개가 전부다. */
}

void *thread(void *vargp)                   /* 스레드 루틴을 정의한다 */
{
  printf("Hello World\n");
  return (NULL);
}
```

출처: [컴퓨터 시스템](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791185475219&orderClick=LAG&Kc=) 12.3.2


<br><br>

## 사용 함수 

```C

int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine)(void *), void *arg);

     새로운 스레드를 스레드 속성 attr에 따라 생성한다.
     스레드 속성 객체 attr이 NULL이라면 기본 스레드 속성으로 스레드를 생성한다.
     스레드가 성공적으로 생성되면 생성된 스레드 ID는 thread에 저장된다.
     생성된 스레드는 start_routine을 arg 인자를 사용하여 실행한다.
     start_routine이 반환되면 내부적으로 pthread_exit() 함수가 호출되어 스레드가 종료된다.
     
int pthread_join(pthread_t thread, void **value_ptr);
     스레드 종료를 대기한다. 대기하는 스레드가 종료되면, value_ptr 인자의 값은 pthread_exit() 함수가 전달한 종료 값을 얻게된다.
     
int pthread_mutex_init(pthread_mutex_t *mutex, const pthread_mutexattr_t *attr);
     뮤텍스 객체를 초기화한다.
     
int pthread_mutex_lock(pthread_mutex_t *mutex);
     뮤텍스 객체를 잠근다.

int pthread_mutex_unlock(pthread_mutex_t *mutex);
     뮤텍스 객체의 잠금을 해지한다.
     
int pthread_mutex_destroy(pthread_mutex_t *mutex);
     뮤텍스 객체를 파괴한다.
     
int pthread_detach(pthread_t thread);
     인자 thread를 커널에서 분리 시킨다. 분리된 스레드는 수행을 종료 시키고, 할당된 자원을 회수한다.

```
