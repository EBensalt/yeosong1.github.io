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
    - MASM(Microsoft Macro Assembler) - Intel 문법 등.. 이외에도 어셈블러는 여러가지 종류가 있다.

- [x] 평가 항목은 아니지만, 동료평가시 테스트를 편하게 만들어주는 테스트 프로그램을 만들기를 권장합니다.
- [x] 할당된 깃 레포에 과제 제출 하세요. Deepthought의 채점 중 작업 섹션에서 오류가 발생하면 평가가 중지됩니다.

### 어셈블리 기초 명령 정리

출처: [Paul A. Carter의 [PC 어셈블리어]에서 1.3](https://pacman128.github.io/static/pcasm-book-korean.pdf)

| 명령어 | 수행 내용 | 참고 |
| --- | --- | --- | 
| mov | `dest, src` src에 있는 데이터를 dest에 복사 | |
| add | 정수 a, b 일때 a + b | |
| sub | 정수 a, b 일때 a - b | |
| inc | ecx 1을 증가 시킨다. | 기계코드 크기가 ADD, SUB보다 더 작다. | 
| dec | dl  1을 감소 시킨다. | 기계코드 크기가 ADD, SUB보다 더 작다. |

### 어셈블리 입출력

| 루틴 | 수행 내용 | 
| --- | --- |
| print_int | RAX에 저장된 값을 화면에 출력한다. |
| print_char | |

### 프로그램 만들기

출처: [Paul A. Carter의 [PC 어셈블리어]에서 1.4.1](https://pacman128.github.io/static/pcasm-book-korean.pdf)

>   코드 세그먼트는 역사적으로 `.text` 라 불려왔다. 이는 명령어들이 저장되는 곳이다. <br>
> (38 행)의 메인 루틴의 라벨에 접두어 _(under score)가 사용되었음을 유의하자. 이는 <br>
> C 함수 호출 규약 (C calling convention) 의 일부분이다. 이 규약은 코드 컴파일 시에 C 가 <br>
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



