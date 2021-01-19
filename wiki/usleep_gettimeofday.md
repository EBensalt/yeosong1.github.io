# usleep

이 함수는 obsolete이고 nanosleep(2)를 쓰라는데.. 우리 서브젝트는 그냥 이걸 쓰라고 해놨네..

- unist.h
- `int     usleep(useconds_t microseconds);`
- 1초 = 1000 x 1000 마이크로세컨드



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
