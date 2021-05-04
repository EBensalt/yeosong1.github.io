# select

~~~
select(int maxfd, fd_set *readset, fd_set *writeset, fd_set *exceptset, const struct timeval * timeout)
~~~

[man select](https://man7.org/linux/man-pages/man2/select.2.html)

select는 한 번에 여러 fd의 변동사항(이벤트 발생)들을 감지하여 그 수를 반환하는 함수이다. [비동기](동기블로킹.md) [블로킹](동기블로킹.md). 블로킹 없이 쓰거나 읽을 준비가 되면 '준비됨'으로 본다.

<br>
<br>1️⃣ 인자로 들어갈 내용 설정
  - 파일 디스크립터 설정 (fd_set *readset, fd_set *writeset, fd_set *exceptset)
    - 매크로 함수를 통해 관리
      - FD_ZERO
      - FD_SET
      - FD_CLR
      - FD_ISSET
        - 검사 범위 지정 (int maxfd)
            - 자료형 fd_set은 비트 단위(0과 1)로 이루어진 배열
            - fd는 순서대로 지정되므로, maxfd 값은 가장 큰(가장 마지막에 생성된) fd값 + 1으로 설정하면 된다.
        - 타임아웃 설정 (const struct timeval * timeout)
            - 이벤트가 발생 안하는데 영원히 블로킹 상태로 기다리기 싫으니까 타임아웃을 지정한다. 근데 우리는 NULL로 설정해서 타임아웃이 없고 무한대기. 이 부분은 나중에 fcntl NONBLOCK으로 설정하는 내용과도 관련이 있다.

<br>2️⃣ select 함수 호출
<br>3️⃣ 호출 결과 확인
- 관찰 중인 파일 디스크립터에 변화가 생겨야 반환한다 or 변화는 없지만 타임아웃 시간이 모두 지났다면 반환한다.
- 뭔가를 할 준비가 된(이벤트가 감지된) fd 개수를 반환 or 그러니까 타임아웃으로 아무 일 없이 함수가 반환된 경우에는 0을 반환.
