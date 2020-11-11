# libasm


- 서브젝트 [libasm.en.subject.pdf](https://github.com/yeosong1/yeosong1.github.io/files/5485720/en.subject.2.pdf)

## 일반 지침

- [ ] 64 bit ASM 을 써야합니다. (호출 규약 calling convention 주의)
- [x] .s (O) / inline ASM(X)
  - *.s 는 어셈블리 코드의 확장자 입니다. [블로그 C언어 강좌 1편](https://blog.hexabrain.net/2)
  - 인라인 어셈블리는 C 소스 코드의 중간에 어셈블리 명령어를 추가하는 관행입니다. - [sodocumentation](https://sodocumentation.net/ko/c/topic/4263/%EC%9D%B8%EB%9D%BC%EC%9D%B8-%EC%96%B4%EC%85%88%EB%B8%94%EB%A6%AC)
- [x] nasm으로 컴파일해야 합니다.
  - `curl -fsSL https://rawgit.com/kube/42homebrew/master/install.sh | zsh`
  - `brew install nasm`
- [ ] 문법: Intel(O) / AT&T(X)
  - [어셈블리어 Intel / AT&T 문법](./asm_syntax.md)


- [ ] 평가 항목은 아니지만, 동료평가시 테스트를 편하게 만들어주는 테스트 프로그램을 만들기를 권장합니다.
- [x] 할당된 깃 레포에 과제 제출 하세요. Deepthought의 채점 중 작업 섹션에서 오류가 발생하면 평가가 중지됩니다.

### Makefile 체크리스트

- [ ] $(NAME)
- [ ] all
- [ ] clean
- [ ] fclean
- [ ] 불필요한 리컴파일, 리링크 없어야 함.




## 필수 작성 및 체크리스트

- [ ] strlen
- [ ] strcpy
- [ ] strcmp
- [ ] write
- [ ] read
- [ ] strdup

- [ ] 시스템콜syscall 하는 중에 에러 있는지 체크
- [ ] 변수 errno 세팅 (extern ___error 사용 가능)

## 최종 점검

- [ ] segmentation fault
- [ ] bus error
- [ ] double free
- [ ] 가독성

## 보너스 작성

- [ ] atoi_base
- [ ] list_push_front
- [ ] list_size
- [ ] list_sort
- [ ] list_remove_if

## 보너스 최종 점검

- [ ] rule bonus
- [ ] _bonus.c / _bonus.h
- [ ] segmentation fault
- [ ] bus error
- [ ] double free
- [ ] 가독성



