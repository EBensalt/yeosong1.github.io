# usleep

이 함수는 obsolete이고 nanosleep(2)를 쓰라는데.. 우리 서브젝트는 그냥 이걸 쓰라고 해놨네..

- unist.h
- `int     usleep(useconds_t microseconds);`
- 1초 = 1000 x 1000 마이크로세컨드

지정한 마이크로 초가 경과하거나 신호가 스레드에 전달 될 때까지 호출 스레드의 실행을 일시 중단합니다.



# gettimeofday

- sys/time.h

```C
int     gettimeofday(struct timeval *restrict tp, void *restrict tzp);
                             날짜와 시간을 저장할 주소, 타임존인데..
                          
이 함수는 시스템 날짜와 시간을 읽어 tp에 저장한다. tzp 값은 사용하지 않는다.
                             
struct timeval
{
    time_t        tv_sec;     /* seconds */
    suseconds_t   tv_usec;    /* microseconds */
};

```

현재 시간을 초, 마이크로초 구조체에 저장한다.
