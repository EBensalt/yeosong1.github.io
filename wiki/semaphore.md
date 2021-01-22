# 세마포어

- 세마포어: 동시에 리소스에 접근 허용이 가능한 개수를 의미하는 정수 변수
- semaphore.h

철학자 문제로 치면, 동시에 사용 가능한 포크 개수가 되는데, <br>
포크는 2개 한 세트로만 사용 가능하니까 (포크 개수 / 2)가 세마포어가 된다.

## semaphore.h

~~~
sem_getvalue — 세마포어 값 가져오기

sem_init — 이름이 없는 세마포어 초기화
sem_destroy — 이름이 없는 세마포어 제거

sem_open — 이름이 있는 세마포어 초기화 및 열기
sem_close — 이름이 있는 세마포어 종료
sem_unlink — 이름이 있는 세마포어 제거

sem_wait — 세마포어 잠그기
sem_post — 세마포어 잠금 해제

sem_timedwait — 주어진 시간 안에 세마포어를 잠금
sem_trywait — 세마포어 잠겨있지 않다면 잠금
~~~


## 사용 함수

```C
sem_t *sem_open(const char *name, int oflag, ...);
      name이라는 세마포어 객체를 oflag에 따라 생성 혹은 접근.

int sem_close(sem_t *sem);
      sem이 가리키는 세마포어를 사용하여 종료

int sem_wait(	sem_t *sem);
      sem이 가리키는 세마포어를 잠금

int sem_post(	sem_t *sem);
      sem이 가리키는 세마포어를 잠금 해제

int sem_unlink(	const char *name);
      name이라는 세마포어 객체를 제거
```
