# libasm


- 서브젝트 [libasm.en-kr.subject.pdf](https://github.com/yeosong1/yeosong1.github.io/files/5485720/en.subject.2.pdf)

## 🤓 일반 지침..을 따르기 위한 배경지식

- [x] **64bit ASM 을 써야합니다. (호출 규약 calling convention 주의)**
  - [System V AMD64 ABI 호출 규약](https://en.wikipedia.org/wiki/X86_calling_conventions#System_V_AMD64_ABI) = 코드가 호출자(caller)로부터 변수를 받고, 어떻게 결과를 반환하는지에 대한 규약. 
  - [64비트 리눅스 vs 64비트 윈도우 호출 규약 차이(Calling Convention)](https://kkamagui.tistory.com/811)
  
- [x] **.s (O) / inline ASM(X)**
  - `*.s` = 어셈블리 코드의 확장자 - [출처: 블로그 C언어 강좌 1편](https://blog.hexabrain.net/2)
  - `inline 어셈블리` = C 소스 코드의 중간에 어셈블리 명령어를 추가하는 관행 - [출처: SO documentation](https://sodocumentation.net/ko/c/topic/4263/%EC%9D%B8%EB%9D%BC%EC%9D%B8-%EC%96%B4%EC%85%88%EB%B8%94%EB%A6%AC)
- [x] **nasm으로 컴파일해야 합니다.**
  - brew 설치(클러스터 맥에) =  `curl -fsSL https://rawgit.com/kube/42homebrew/master/install.sh | zsh`
  - nasm 설치 = `brew install nasm`
  - 사용법 확인 = 터미널에 `nasm -h` = 우리 환경은 `nasm -f macho64 *.s`로 컴파일
- [x] **문법: Intel(O) / AT&T(X)**
  - `어셈블러` 종류에 따라 채택 가능한 문법이 다르다.
  	- GAS(GNU Assembler) - AT&T 문법
	- NASM(Netwide Assembler) - Intel 문법
	- MASM(Microsoft Macro Assembler) - Intel 문법(NASM과 완전히 같지는 않지만 거의 비슷)등..
	- 이외에도 `어셈블러`는 TASM, YASM 등 여러 종류가 있다
  - `어셈블러` = 어셈블리 컴파일러 
  	- `컴파일러` = 소스를 특정 컴퓨터 아키텍쳐에 맞는 기계어 코드로 바꾸어 주는 프로그램

- [x] **평가 항목은 아니지만, 테스트 프로그램 만들기를 권장**
- [x] **할당된 깃 레포에 과제 제출 하세요. Deepthought의 채점 중 작업 섹션에서 오류가 발생하면 평가가 중지됩니다.**

- **..이 자료 저 자료 봤는데 지금까지는 [tutorialspoint.com/assembly_programming](https://www.tutorialspoint.com/assembly_programming/index.htm) 이게 제일 좋은 거 같다..** 

### 레지스터

`레지스터` = 변수처럼 사용하는 그릇(인데 속도가 매우 빠른 임시 저장장치) <br>
C언어에서 변수의 이름을 자유롭게 사용할 수 있는 반면, <br>
어셈블리에서는 용도와 이름이 이미 지정되어있는 레지스터를 변수로 사용한다고 보면 될듯..?

- 범용(주소, 데이터 둘다 가능) 레지스터 (64bit - 32bit - 16bit - up 8bit - down - 8bit)
  - rax - eax - ax - ah - al
  - rbx - ebx - bx - bh - bl
  - rcx - ecx - cx - ch - cl
  - rdx - edx - dx - dh - dl
    
- 인덱스 레지스터
  - si (src index)
  - di (dest index)

- 포인터 레지스터(주소): 기계어 스택에 들어 있는 데이터의 위치를 가리킨다. 
  - bp (base pointer 기준 포인터. 스택 포인터의 최저점)
  - sp (stack pointer 스택 포인터. 스택 포인터의 최고점)

    
- 세그먼트 레지스터: 프로그램의 각 부분에서 메모리의 어떤 부분이 사용되는지 가리킨다.
    - cs 코드 세그먼트
    - ds 데이타 세그먼트
    - ss 스택 세그먼트
    - es 보조 extra 세그먼트

- 명령 포인터(주소): CS 레지스터와 함께 사용하는데, 다음으로 실행될 명령의 주소를 저장한다.
  - ip index pointer?
    
- 플래그 레지스터: 이전에 실행된 명령들의 중요한 결과값들은 저장하고 있다.
  - flags
    


### 어셈블리 기초 명령 정리

출처: [Paul A. Carter의 [PC 어셈블리어]에서 1.3.4](https://pacman128.github.io/static/pcasm-book-korean.pdf)

| 명령어 | 수행 내용 | 참고 |
| --- | --- | --- | 
| mov | `dest, src` src에 있는 데이터를 dest에 복사 | |
| add | 정수 a, b 일때 a + b | |
| sub | 정수 a, b 일때 a - b | |
| inc | ecx 1을 증가 시킨다. ecx++ | 기계코드 크기가 ADD, SUB보다 더 작다. | 
| dec | dl  1을 감소 시킨다. dl--  | 기계코드 크기가 ADD, SUB보다 더 작다. |

### 제어 구조

출처: [Paul A. Carter의 [PC 어셈블리어]에서 2.2](https://pacman128.github.io/static/pcasm-book-korean.pdf)

참고: 세트 set = 1 / 언세트 unset = 0 

비교 명령
| 명령어 | 수행 내용 | 참고 |
| --- | --- | --- | 
| cmp | `vleft, vright` vleft - vright에 따라 플래그들이 세팅된다 |  ZF(제로 플래그) CF(캐리 플래그) |

무조건 분기 명령
| 명령어 | 수행 내용 | 
| --- | --- |  
| jmp | 무조건 분기 명령을 실행한다 |

이름들
|이름|축약된 내용|
|---|---|
|JE||
|JNGE|Jump Not Greater than or Equal to|
|JL|Jump Less than|
|...||
|...||


<details>
<summary> <b> 조건 분기 명령 </b>  </summary>
<div markdown="1">
  
| 명령어 | 요약(?) | 수행 내용 |
| --- | --- | --- |
| JZ | if(ZF == 1) | ZF 가 세트일 때만 분기 | 
| JNZ | if(ZF == 0) | ZF 가 언세트일 때만 분기 |
| JO | if(OF == 1) | OF 가 세트일 때만 분기 |
| JNO | if(OF == 0) | OF 가 언세트일 때만 분기 |
| JS | if(SF == 1) | SF 가 세트일 때만 분기 |
| JNS | if(SF == 0) | SF 가 언세트일 때만 분기 |
| JC | if(CF == 1) | CF 가 세트일 때만 분기 |
| JNC | if(CF == 0) | CF 가 언세트일 때만 분기 |
| JP | if(PF == 1) | PF 가 세트일 때만 분기 |
| JNP | if(PF == 0) | PF 가 언세트일 때만 분기 |

연산 결과에 관여하는 플래그
(출처 : [레지스터의 종류](https://blog.naver.com/mjnms/220460825993))
| 플래그 | 설명 |
| --- | --- |
| ZF | Zero Flag | 연산 결과 가 0이면 1로 설정 | 
| OF | Overflow Flag | 연산 결과가 용량을 초과하였을 경우 1로 설정 |
| SF | Sign Flag | 연산 결과 최상위 비트가 1인 경우 1로 설정 |
| CF | Carry Flag | 덧셈과 뺄셈에서 빌림수(Borrow)발생 시 1로 설정 |
| PF | Pairity Flag | 연산 결과가 짝수이면 1, 홀수면 0으로 설정 |
| AF | Auxiliary-carry Flag | 16(8)비트 연산시 빌림수(Borrow) 발생 때 1로 설정 |


</div>
</details>
<br><br>

<br>

부호 유무에 따른 조건 분기 명령

| 부호 있음 || 부호 없음 ||
| --- | --- | --- | --- |
| 명령어 | 수행 내용 | 명령어 | 수행 내용 |
| JE |vleft = vright 이면 분기  | JE |vleft = vright 이면 분기 
| JNE | vleft ≠ vright 이면 분기 |  JNE | vleft ≠ vright 이면 분기 |
| JL, JNGE | vleft < vright 이면 분기 | JB, JNAE | vleft < vright 이면 분기 |OF 가 세트일 때만 분기 ||
| JLE, JNG | vleft ≤ vright이면 분기 | JBE, JNA | vleft ≤ vright이면 분기 |
| JG, JNLE | vleft > vright 이면 분기 |  JA, JNBE | vleft > vright 이면 분기 | 
| JGE, JNL | vleft ≥ vright 이면 분기 | JAE, JNB | vleft ≥ vright 이면 분기 | 

### 스택

출처: [Paul A. Carter의 [PC 어셈블리어]에서 4.3](https://pacman128.github.io/static/pcasm-book-korean.pdf)

- 스택은 후입선출(Last-in First-Out)

| 명령어 | 수행 내용 | 참고 |
| --- | --- | --- | 
| push | 스택에 데이터 추가 | |
| pop | 스택에서 데이터 빼내기 | 빼내진 데이터는 물론 가장 마지막으로 추가된 데이터이다|

### CALL과 RET

출처: [Paul A. Carter의 [PC 어셈블리어]에서 4.4](https://pacman128.github.io/static/pcasm-book-korean.pdf)

| 명령어 | 수행 내용 |
| --- | --- | 
| call | 서브프로그램으로의 무조건 분기를 한 후, 그 다음에 실행될 명령의 주소를 스택에 푸시 (push) |
| ret | 그 주소를 팝 (pop) 한 후 그 주소로 점프 |


### 어셈블리 기본 구문

어셈블리 프로그램은 세 부분으로 나뉜다.

#### data section
- [초기화된initialized 데이터, 상수] 선언을 위한 공간 
- = 런타임에 변하지 않는 데이터들
- 상수, 파일 이름, 버퍼 사이즈 등을 여기에 선언할 수 있다.

문법

~~~
section.data
~~~

#### BSS section

- Block Starting Symbol
- [변수] 선언을 위한 공간

~~~
section.bss
~~~

#### text section
- [실제 코드] 저장을 위한 공간

~~~
section.text
  global _start
_start:
~~~

#### 주석

- 세미콜론 ; 으로 주석 처리.
- 공백을 포함한 모든 프린터블 문자

~~~
; 이렇게 주석을 쓴다 이렇게까지 설명할 필요는 없겠지만
add eax, ebx ; ebx랑 eax 더하기
~~~

#### 어셈블리 언어의 문법적 구성

~~~
[label] mnemonic [operands] [;주석]
        ~~~~~~~  ~~~~~~~
         실행될 명령어의  │
         이름(=니모닉)   └ = 피연산자operands 또는 매개변수parameters
~~~

- 그냥 mnemonic이 뭔지 궁금: [Mnemonic 어원과 어셈블리에서의 니모닉](https://medium.com/hexlant/mnemonic-%EC%9D%B4%EB%9E%80-7fb48106bd77)
  - 예를 들어 어셈블리의 add는 기계어로 000000인 것을 니모닉하게(기억하기 쉽게) 표현한 것이라고 말할 수 있다.
- [대괄호]는 선택사항
- mnemoic 부분은 지시의 이름이고, 두번째 부분은 오퍼랜드이거나 파라미터.

다음은 어셈블리 언어 명령문 예시이다.
~~~
INC COUNT        ; 메모리 변수 COUNT를 증가시켜라

MOV TOTAL, 48    ; 메모리 변수 TOTAL에 값 48을 전송해라.
					  
ADD AH, BH       ; BH 레지스터의 내용을 AH 레지스터에 추가해라.
					  
AND MASK1, 128   ; 변수 MASK1과 128에 대해 AND 연산을 수행해라.
					  
ADD MARKS, 10    ; 변수 MARKS에 10을 더해라
MOV AL, 10       ; 값 10을 AL 레지스터로 전송해라.
~~~

#### Hello, Wolrd를 쳐봐야겠죠

환경: MacOS Mojave 10.14.6 <br>
파일명: hello.s
~~~
section .text
    global _main

_main : 
    mov rax, 0x2000004
    mov rdi, 1
    mov rsi, msg
    mov rdx, 12
    syscall
    mov rax, 0x2000001
    mov rdi, 0
    syscall

section .data
    msg db "Hello World"
~~~

`nasm -f macho64 hello.s` <br>
`ld -lSystem hello.o -o hello` (<- warning이 몇 개 뜬다) <br>
`./hello`

<br>
<br>





## ✅ 체크리스트

### Makefile 체크리스트

- [x] $(NAME) (= libasm.a)
- [x] all
- [x] clean
- [x] fclean
- [x] 불필요한 리컴파일, 리링크 없어야 함.

### 필수 작성 및 체크리스트

- [ ] ft_strlen
- [ ] ft_strcpy
- [ ] ft_strcmp
- [ ] ft_write
- [ ] ft_read
- [ ] ft_strdup

- [ ] 시스템콜syscall 하는 중에 에러 있는지 체크
- [ ] 변수 errno 세팅 (extern ___error 사용 가능)

### 최종 점검

- [ ] segmentation fault
- [ ] bus error
- [ ] double free
- [ ] 가독성

### 보너스 작성

- [ ] ft_atoi_base
- [ ] ft_list_push_front
- [ ] ft_list_size
- [ ] ft_list_sort
- [ ] ft_list_remove_if

### 보너스 최종 점검

- [ ] rule bonus
- [ ] _bonus.c / _bonus.h
- [ ] segmentation fault
- [ ] bus error
- [ ] double free
- [ ] 가독성



