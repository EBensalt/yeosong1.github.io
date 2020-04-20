---
published: true
---

# Format of printf

[위키피디아 printf 포맷 스트링 페이지 확인하기](https://en.wikipedia.org/wiki/Printf_format_string)

%[parameter] [flags] [width] [.precision] [length] type
<br><br>
%[파라미터][플래그][프린트 할 것들 총합의 너비][결과가 숫자일 때 너비(정밀도)][데이터 타입의 범위]데이터 타입
<br><br>
%[n] [-+ 0'#] [&#42; or 숫자] [. &#42; or 숫자] [h,hh,l,ll,w,I,I32,I64,q,L,z,j,t] csdiuox..
<br>

# flag
* [-] 좌측정렬
* [0] 너비에 맞게 빈 곳을 0으로 패딩
* [.] 프리시전 = 정밀도 (= 숫자 서식에만 관여)
* [  ] 너비에 맞게 빈 곳을 ' '로 채움
* [*] 숫자 와일드카드. 가변인자로 값이 들어옴
* [#] 0, 0x, 0X 붙이기
* [+] +,- 부호 표시하기

# width and precision
* [숫자] 10진수 숫자가 곧 width, precision
* [*] 가변인자로 width, precision의 크기가 들어옴
* [%d, %i 등 정수 서식지정자] 가변인자로 받은 값이 곧 width, precision이자 데이터 타입이 받는 값.

### 💥주의
* precision에 음수가 할당되면 무시(없는 것으로 간주)
* precision이 precision이면서 동시에 value일 때 주의(ex: %.d)
* width이면서 value인 자리에 올 때는 width 무시(없는 것으로 간주)
* width에 음수가 할당되면 '-'플래그(좌측정렬) + 너비로 간주.

# length

|length + data type | data type |
|:---|---:|
|hhd,hhi |   signed char |
|hhu,hhx,hhX |    unsigned char |
|hd,hi |   short |
|hu,hx,hX |   unsigned short |
|ld,li |   long |
|lld,lli |   long long |
|lu,lx,lX| unsigned long|
|llu,llx,llX|unsigned long long|
|lc | wint_t|
|ls|wchar_t *|

# type
<br>%t bool
<br>%b 2진수
<br>%c 문자
<br>%d 10진수
<br>%o 8진수
<br>%x 16진수
<br>%X 16진수
<br>%f  고정소수점
<br>%F 고정소수점
<br>%e 지수 표현, e
<br>%E 지수 표현, E
<br>%g 간단한 실수 표현
<br>%G 간단한 실수 표현
<br>%s 문자열
<br>%p 포인터
<br>%u 부호 없는 정수형. unsigned int
<br>%U 유니코드
**👉[UTF-8](UTF-8)**
<br>%T 타입
<br>%v 모든 형식
<br>.
<br>.
<br>.

# return
리턴: 출력한 글자 수
