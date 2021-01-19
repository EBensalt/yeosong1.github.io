# <pthread.h>

- pthread = Posix 스레드
- C 프로그램에서 스레드를 조작하는 표준 인터페이스
- 모든 리눅스 시스템에서 사용 가능
- 60여개의 함수 포함
- man 페이지 한글로 보기 -> [링크](http://neosrtos.com/docs/posix_api/pthread.html)
  - 정확히는 운영체제인 neos에서 사용하는 포식스 함수 man페이지..
  - POSIX 표준 호환이라고해서 내용이 거의 같을 거 같아 크게 훑어보는 용으로 썼다.

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
             
     int pthread_join(pthread_t thread, void **value_ptr);
     
     int pthread_mutex_init(pthread_mutex_t *mutex, const pthread_mutexattr_t *attr);
             
     int pthread_mutex_lock(pthread_mutex_t *mutex);

     int pthread_mutex_unlock(pthread_mutex_t *mutex);
     
     int pthread_mutex_destroy(pthread_mutex_t *mutex);
```
