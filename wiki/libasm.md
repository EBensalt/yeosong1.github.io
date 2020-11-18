# libasm

- 서브젝트 [libasm.en-kr.subject.pdf](https://github.com/yeosong1/yeosong1.github.io/files/5485720/en.subject.2.pdf)

# 🤓 일반 지침..을 따르기 위한 배경지식

- [x] **64bit ASM 을 써야합니다. (호출 규약 calling convention 주의)**
	- 호출 규약 = 코드가 호출자(caller)로부터 변수를 받고, 어떻게 결과를 반환하는지에 대한 규약.
	- [System V AMD64 ABI 호출 규약](https://en.wikipedia.org/wiki/X86_calling_conventions#System_V_AMD64_ABI) = Mac OS가 따르는 호출 규약
	- [64비트 리눅스 vs 64비트 윈도우 호출 규약 차이(Calling Convention)](https://kkamagui.tistory.com/811) -> 그냥 참고차..
  
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

# Hello, Wolrd를 먼저 쳐봐야겠죠

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

~~~
nasm -f macho64 hello.s
ld -lSystem hello.o -o hello       (<- warning이 몇 개 뜬다)
./hello
~~~

## 프로세서 레지스터

출처 : [tutorialspoint.com/assembly_programming](https://www.tutorialspoint.com/assembly_programming/index.htm)

메모리에 접근해 데이터를 읽고 저장하는 것은 프로세서를 느리게 합니다. <br>
`레지스터`는 CPU 내부에 있는(= 속도가 매우 빠른) 임시 저장장치입니다. <br>
레지스터는 메모리에 액세스하지 않고도 처리할 데이터 요소를 저장합니다. <br>
제한된 수의 레지스터가 프로세서 칩에 내장됩니다.

### General Register 범용(주소, 데이터 둘다 가능한) 레지스터

#### 데이터 레지스터

- (64bit - 32bit - 16bit - up 8bit - down 8bit)
- rax - eax - ax - ah - al (**A**ccumulator 기본 누산기)
- rbx - ebx - bx - bh - bl (**B**ase 인덱싱된 주소 지정에 사용 가능해서 기본 레지스터라고 함)
- rcx - ecx - cx - ch - cl (**C**ount 루프카운트를 저장)
- rdx - edx - dx - dh - dl (**D**ata 입출력 작업에 사용/큰 값을 포함하는 곱셈 나눗셈 연산을 위해 DX,AX와 함께 사용)

#### 포인터 레지스터(주소): 기계어 스택에 들어 있는 데이터의 위치를 가리킨다. 

- 명령 포인터(주소): 

- IP (**I**nstruction Pointer CS 레지스터와 함께 사용하는데, 다음으로 실행될 명령의 주소를 저장한다.)
- BP (**B**ase Pointer 기준 포인터. 스택 포인터의 최저점)
- SP (**S**tack Pointer 스택 포인터. 스택 포인터의 최고점)

#### 인덱스 레지스터

- SI (Src Index)
- DI (Dest Index)


### Control Register 제어 레지스터 : 커널 제어 레지스터

32 비트 명령어 포인터 레지스터와 결합 된 32 비트 플래그 레지스터는 제어 레지스터로 간주됩니다.    


#### E-flag Register (Extended)

연산 결과에 관여하는 플래그들 (출처 : [레지스터의 종류](https://blog.naver.com/mjnms/220460825993), [어셈블리-레지스터](https://www.tutorialspoint.com/assembly_programming/assembly_registers.htm))

- "set" 된다 = 1로 설정된다.
- "unset" 된다 = 0으로 설정된다.

| 플래그 | 이름 | 설명 |
| --- | --- | --- |
| ZF | Zero Flag | 연산 결과 가 0이면 1로 설정 | 
| CF | Carry Flag | 덧셈과 뺄셈에서 빌림수(Borrow 혹은 carry)발생 시 1로 설정 |
|    |     예시    | 0000 - 0001 = 1111처럼 빌려오는 경우에 set 한다. |
|    |     사용    | a - b 가 음수가 되는 경우(unsinged 연산중 오류)를 잡을 수 있는 플래그 |
| OF | Overflow Flag | 연산 결과가 용량을 초과하였을 경우 1로 설정 |
|    |          예시  | 1000 + 1000 = (1)0000 되어 set 된다. | 
| SF | Sign Flag | 연산 결과 최상위 비트가 1인 경우 1로 설정 |
| PF | Pairity Flag | 연산 결과가 짝수이면 1, 홀수면 0으로 설정 |
| AF | Auxiliary-carry Flag | 16(8)비트 연산시 빌림수(Borrow) 발생 때 1로 설정 |
| DF | Direction Flag | 0이면 문자열 연산을 왼쪽에서 오른쪽 방향으로 1이면 오른쪽에서 왼쪽 방향 |
| IF | Interrupt Flag | 0이면 외부 인터럽트 비활성화(무시), 1이면 인터럽트 활성화 |
| TF | Trap Flag | single-step mode에서 프로세서의 작동을 설정할 수 있다. (???) |

    
### Segment Register 세그먼트 레지스터: 프로그램의 각 부분에서 메모리의 어떤 부분이 사용되는지 가리킨다.
- CS : 코드 세그먼트의 시작 주소를 담는다. (코드 세그먼트는 실행될 모든 명령이 있는 부분이다.)  
- DS : 데이타 세그먼트의 시작 주소를 담는다. (데이타 세그먼트는 데이터, 상수, 작업 영역이 있는 부분이다.)
- SS : 스택 세그먼트의 시작 주소를 담는다. (스택 세그먼트는 서브 루틴의 데이터와 리턴 주소가 있는 부분이다.)
- 이외에도 ES, FS, GS 등 데이터 저장을 위한 추가적인 세그먼트 레지스터가 있다.

## syscall 사용하기

보통은 `/usr/include/sys/syscall.h `에 들어가서 보면 된다는데 (출처 - [Mac OS X 어셈블리 코딩 - 시스템 파악 및 exit()](https://dhdhfl.tistory.com/144)) <br>
클러스터에서 syscall.h 찾아보니 위치가 `/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/System/Library/Frameworks/Kernel.framework/Versions/A/Headers/sys/syscall.h` ㅎㅎㅎ..  어쨌거나 들어가서 봤다.

<img width="700" alt="스크린샷 2020-11-16 오후 1 38 34" src="https://user-images.githubusercontent.com/53321189/99216464-149c1980-2819-11eb-9282-ea7edabe26c2.png">

## errno

___error 함수
리턴: 에러넘버의 포인터(int)

#include <sys/errno.h>에 있는 함수로




## 어셈블리 변수

| data type (자료형) | 크기 |
|---|---|
|BYTE|	|부호 없는 1 byte|
|WORD	|부호 없는 2 byte|
|DWORD	|부호 없는 4 byte|
|QWORD	|부호 없는 8 byte|


## 어셈블리 산술 연산

출처: [Paul A. Carter의 [PC 어셈블리어]에서 1.3.4](https://pacman128.github.io/static/pcasm-book-korean.pdf)
출처: [nasm 어셈블리 산술 연산](https://www.tutorialspoint.com/assembly_programming/assembly_arithmetic_instructions.htm)

| 명령어 | 수행 내용 | 참고 |
| --- | --- | --- | 
| **MOV** | `MOV dest, src` src에 있는 데이터를 dest에 복사 | |
| **ADD** | 정수 a, b 일때 a + b | |
| **SUB** | 정수 a, b 일때 a - b | |
| **INC** | `INC dest` dest를 1 증가 시킨다. dest++ | 기계코드 크기가 ADD, SUB보다 더 작다. | 
| **DEC** | `DEC dest` dest를 1 감소 시킨다. dest--  | 기계코드 크기가 ADD, SUB보다 더 작다. |
| MUL | `MUL multiplier` 부호 없는 데이터를 곱합니다. | |
| IMUL | `IMUL multiplier` Integer mul. 부호 있는 데이터를 곱합니다. | 
| DIV |||
| IDIV|||

## 어셈블리 Conditions (조건)

출처: [Paul A. Carter의 [PC 어셈블리어]에서 2.2](https://pacman128.github.io/static/pcasm-book-korean.pdf)

### 무조건 점프

- 명령어 JMP로 무조건 분기 명령을 실행한다.


### 조건 점프

- 명령어 J<조건> 이런 조합의 이름으로, 조건에 따라 수행된다.
- 참고: 세트 set = 1 / 언세트 unset = 0 

출처: [nasm 어셈블리 컨디션즈](https://www.tutorialspoint.com/assembly_programming/assembly_conditions.htm)

#### 부호있는 signed 데이터의 산술 연산

|지시|정의|플래그|
|---|:--|:--|
|JE/JZ	| Jump Equal or Jump Zero|	|ZF|
|JNE/JNZ| Jump not Equal or Jump Not Zero	|ZF|
|JG/JNLE| Jump Greater or Jump Not Less/Equal|	OF, SF, ZF|
|JGE/JNL| Jump Greater/Equal or Jump Not Less|	OF, SF|
|JL/JNGE| Jump Less or Jump Not Greater/Equal|	OF, SF|
|JLE/JNG| Jump Less/Equal or Jump Not Greater|	OF, SF, ZF|

#### 부호없는 unsigned 데이터의 논리 연산

|지시|정의|플래그|
|---|:--|:--|
|JE/JZ	|Jump Equal or Jump Zero|	ZF|
|JNE/JNZ	|Jump not Equal or Jump Not Zero|	ZF|
|JA/JNBE	|Jump Above or Jump Not Below/Equal|	CF, ZF|
|JAE/JNB	|Jump Above/Equal or Jump Not Below|	CF|
|JB/JNAE	|Jump Below or Jump Not Above/Equal|	CF|
|JBE/JNA	|Jump Below/Equal or Jump Not Above|	AF, CF|

#### 특별한 용도 - flag의 값을 확인 하는 경우

|지시|정의|플래그|
|---|:--|:--|
|JXCZ	|Jump if CX is Zero|	none|
|JC	|Jump If Carry	|CF|
|JNC	|Jump If No Carry|	CF|
|JO	|Jump If Overflow|	OF|
|JNO	|Jump If No Overflow|	OF|
|JP/JPE	|Jump Parity or Jump Parity Even|	PF|
|JNP/JPO|Jump No Parity or Jump Parity Odd|	PF|
|JS	|Jump Sign (negative value)	|SF|
|JNS	|Jump No Sign (positive value)	|SF|


#### 비교 명령 CMP
| 명령어 | 수행 내용 | 참고 |
| --- | --- | --- | 
| CMP | `dest, src` dest - srct에 따라 플래그들이 세팅된다 |

## 스택

출처: [Paul A. Carter의 [PC 어셈블리어]에서 4.3](https://pacman128.github.io/static/pcasm-book-korean.pdf)

- 스택은 후입선출(Last-in First-Out)

| 명령어 | 수행 내용 | 참고 |
| --- | --- | --- | 
| push | 스택에 데이터 추가 | |
| pop | 스택에서 데이터 빼내기 | 빼내진 데이터는 물론 가장 마지막으로 추가된 데이터임! 주의.|

## CALL과 RET

출처: [Paul A. Carter의 [PC 어셈블리어]에서 4.4](https://pacman128.github.io/static/pcasm-book-korean.pdf)

| 명령어 | 수행 내용 |
| --- | --- | 
| call | 서브프로그램으로 무조건 분기를 한 후, 그 다음에 실행될 명령의 주소를 스택에 푸시push |
| ret | 그 주소를 팝pop한 후 그 주소로 점프 |


## 어셈블리 기본 구문

어셈블리 프로그램은 세 부분으로 나뉜다.

### data section
- [초기화된initialized 데이터, 상수] 선언을 위한 공간 
- = 런타임에 변하지 않는 데이터들
- 상수, 파일 이름, 버퍼 사이즈 등을 여기에 선언할 수 있다.

~~~
section.data
~~~

### BSS section

- Block Starting Symbol
- [변수] 선언을 위한 공간

~~~
section.bss
~~~

### text section
- [실제 코드] 저장을 위한 공간

~~~
section.text
  global _start
_start:
~~~

### 주석

- 세미콜론 ; 으로 주석 처리.
- 공백을 포함한 모든 프린터블 문자

~~~
; 이렇게 주석을 쓴다 이렇게까지 설명할 필요는 없겠지만
add eax, ebx ; ebx랑 eax 더하기
~~~

<details>
<summary> <b> 어셈블리 언어의 문법적 구성 </b>  </summary>
<div markdown="1">

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

</div>
</details>
<br><br>






# ✅ 체크리스트

## Makefile 체크리스트

- [x] $(NAME) (= libasm.a)
- [x] all
- [x] clean
- [x] fclean
- [x] 불필요한 리컴파일, 리링크 없어야 함.

## 필수 작성 및 체크리스트

- [ ] ft_strlen
- [ ] ft_strcpy
- [ ] ft_strcmp
- [ ] ft_write
- [ ] ft_read
- [ ] ft_strdup

- [ ] 시스템콜syscall 하는 중에 에러 있는지 체크
- [ ] 변수 errno 세팅 (extern ___error 사용 가능)

## 최종 점검

- [ ] segmentation fault
- [ ] bus error
- [ ] double free
- [ ] 가독성

## 보너스 작성

- [ ] ft_atoi_base
- [ ] ft_list_push_front
- [ ] ft_list_size
- [ ] ft_list_sort
- [ ] ft_list_remove_if

## 보너스 최종 점검

- [ ] rule bonus
- [ ] _bonus.c / _bonus.h
- [ ] segmentation fault
- [ ] bus error
- [ ] double free
- [ ] 가독성


------------------------


?미등록?

~~~
objdump -S -d hello.o

hello.o:	file format Mach-O 64-bit x86-64

Disassembly of section __TEXT,__text:
_main:
       0:	b8 04 00 00 02 	movl	$33554436, %eax
       5:	bf 01 00 00 00 	movl	$1, %edi
       a:	48 be 00 00 00 00 00 00 00 00 	movabsq	$0, %rsi
      14:	ba 0c 00 00 00 	movl	$12, %edx
      19:	0f 05 	syscall
      1b:	b8 01 00 00 02 	movl	$33554433, %eax
      20:	bf 00 00 00 00 	movl	$0, %edi
      25:	0f 05 	syscall
c6r10s4% cat hello.s
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





