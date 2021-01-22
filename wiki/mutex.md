# 뮤텍스


- Mutex lock: 특정 코드 영역의 쓰레드를 실행할 때 한번에 하나의 쓰레드만 실행 가능하도록 하는 방법
- pthread.h

## 사용함수

```C

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
