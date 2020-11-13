# libasm


- 서브젝트 [libasm.en-kr.subject.pdf](https://github.com/yeosong1/yeosong1.github.io/files/5485720/en.subject.2.pdf)

## 🤓 일반 지침에 따른 배경지식 정리

- [x] 64bit ASM 을 써야합니다. (호출 규약 calling convention 주의)
  - [호출 규약](https://ko.wikipedia.org/wiki/%ED%98%B8%EC%B6%9C_%EA%B7%9C%EC%95%BD)? 코드가 호출자(caller)로부터 변수를 받고, 어떻게 결과를 반환하는지에 대한 규약. 
  - 리눅스는 64비트 모드에서 정수 타입의 파라미터 전달시, 순서대로 RDI, RSI, RDX, RCX, R8, R9까지 6개의 레지스터를 사용하고 7개 이상이면 스택을 통해 전달합니다.
    - 32비트에서는 레지스터 이름들이 E로 시작한다. EDI, ESI, EDX ... 16bit, 8bit.. 등 비트별로 다르다.
  - 실수 타입의 파라미터의 경우는 XMM0 ~ XMM7까지 8개를 순서대로 사용하고 그 이상이면 스택으로 전달합니다.
  - [64비트 리눅스 vs 64비트 윈도우 호출 규약 비교(Calling Convention)](https://kkamagui.tistory.com/811)

- [x] .s (O) / inline ASM(X)
  - *.s 는 어셈블리 코드의 확장자이다. [출처: 블로그 C언어 강좌 1편](https://blog.hexabrain.net/2)
  - inline 어셈블리는 C 소스 코드의 중간에 어셈블리 명령어를 추가하는 관행이다. - [출처: SO documentation](https://sodocumentation.net/ko/c/topic/4263/%EC%9D%B8%EB%9D%BC%EC%9D%B8-%EC%96%B4%EC%85%88%EB%B8%94%EB%A6%AC)
- [x] nasm으로 컴파일해야 합니다.
  - brew 설치(클러스터 맥에): `curl -fsSL https://rawgit.com/kube/42homebrew/master/install.sh | zsh`
  - nasm 설치: `brew install nasm`
  - 터미널에 `nasm -h` 해서 사용법을 확인하자: 우리의 환경에 따르면 `nasm -f macho64 *.s`
- [x] 문법: Intel(O) / AT&T(X)
  - 어셈블리 컴파일러 = `어셈블러`
    - `컴파일러`: 소스를 특정 컴퓨터 아키텍쳐에 맞는 기계어 코드로 바꾸어 주는 프로그램
  - 어셈블러 종류에 따라 채택 가능한 문법이 다르다.
    - GAS(GNU Assembler) - AT&T 문법
    - NASM(Netwide Assembler) - Intel 문법
    - MASM(Microsoft Macro Assembler) - Intel 문법(NASM과 완전히 같지는 않지만 거의 비슷)등..
    - 이외에도 어셈블러는 TASM, YASM 등 여러 종류가 있다

- [x] 평가 항목은 아니지만, 동료평가시 테스트를 편하게 만들어주는 테스트 프로그램을 만들기를 권장합니다.
- [x] 할당된 깃 레포에 과제 제출 하세요. Deepthought의 채점 중 작업 섹션에서 오류가 발생하면 평가가 중지됩니다.

### 레지스터?

레지스터는 변수처럼 사용하는 그릇이다. <br>
C언어에서 변수의 이름을 자유롭게 사용할 수 있는 반면, <br>
어셈블리에서는 용도와 이름이 이미 지정되어있는 레지스터를 변수처럼 사용한다고 보면 될듯..?

- 범용 레지스터 (64bit - 32bit - 16bit - up 8bit - down - 8bit)
    - rax (- eax - ax - ah - al)
    - rbx (- ebx - bx - bh - bl)
    - rcx (- ecx - cx - ch - cl)
    - rdx (- edx - dx - dh - dl)
    
- 인덱스 레지스터
    - si (src index)
    - di (dest index)

- 포인터 레지스터: 기계어 스택에 들어 있는 데이터의 위치를 가리킨다. 
  - bp (기준 포인터. 스택 포인터의 최저점)
  - sp (스택 포인터. 스택 포인터의 최고점)

    
- 세그먼트 레지스터: 프로그램의 각 부분에서 메모리의 어떤 부분이 사용되는지 가리킨다.
    - cs 코드 세그먼트
    - ds 데이타 세그먼트
    - ss 스택 세그먼트
    - es 보조 extra 세그먼트

- 명령 포인터: CS 레지스터와 함께 사용하는데, 다음으로 실행될 명령의 주소를 저장한다.
  - ip
    
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


### 프로그램 만들기

출처: [Paul A. Carter의 [PC 어셈블리어]에서 1.4.1](https://pacman128.github.io/static/pcasm-book-korean.pdf)

>   **코드 세그먼트**는 역사적으로 **`.text`** 라 불려왔다. 이는 **명령어들이 저장되는 곳**이다. <br>
> (38 행)의 메인 루틴의 라벨에 **접두어 _(under score)**가 사용되었음을 유의하자. 이는 <br>
> **C 함수 호출 규약 (C calling convention)** 의 일부분이다. 이 규약은 코드 컴파일 시에 C 가 <br>
> 사용하는 규칙들을 명시해 준다. 이는 어셈블리어와 C 를 같이 사용시에 이 호출 규약 <br>
> 을 아는 것이 매우 중요하다. 나중에 이 호출 규약에 대해 속속 살펴 볼 것이다. 그러나, <br>
> 지금은 오직 모든 C 심볼들 (i.e., 함수와 전역 변수) 에는 C 컴파일러에 의해 _ 가 맨 앞에 <br>
> 붙는다는 사실만 알면 된다. (이 규칙은 오직 DOS/Windows 에만 해당하며 리눅스 C <br>
> 컴파일러에서는 C 심볼 이름에 아무것도 붙이지 않는다) <br>
>   37 행의 global 지시어는 어셈블러로 하여금 _asm_main 라벨을 전역으로 생성하 <br>
> 라고 알려준다. C 에서와는 달리 라벨들은 내부 지역 (internal scope)이 기본으로 된다. <br>
> 이 뜻은 오직 같은 모듈에서만 그 라벨을 사용할 수 있다는 뜻이다. 하지만 전역 (global) <br>
> 지시어를 이용하면 라벨을 외부 지역 (external scope) 에서도 사용할 수 있게 된다. 이러 <br>
> 한 형식의 라벨은 프로그램의 어떠한 다른 모듈에서도 접근이 가능하다. asm_io 모듈 <br>
> 은 print_int 등을 모두 전역으로 선언한다. 이 때문에 우리가 first.asm 모듈에서 <br>
> 이를 사용할 수 있었다.







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



