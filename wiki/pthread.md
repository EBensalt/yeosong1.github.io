# pthread

Posix 스레드.
C 프로그램에서 스레드를 조작하는 표준 인터페이스.
모든 리눅스 시스템에서 사용 가능

## 예제 1

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

출처: 책 [컴퓨터 시스템](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791185475219&orderClick=LAG&Kc=) 12.3.2
