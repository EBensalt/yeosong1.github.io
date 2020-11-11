# libasm


- 서브젝트 [libasm.en.subject.pdf](https://github.com/yeosong1/yeosong1.github.io/files/5485720/en.subject.2.pdf)

## 필요 배경 지식

- [ ] 64 bit (호출 규약 calling convention 주의)
- [ ] .s (O) / inline(X)
- [ ] nasm으로 컴파일합니다
  - `curl -fsSL https://rawgit.com/kube/42homebrew/master/install.sh | zsh`
  - `brew install nasm`
- [ ] 문법: Intel(O) / AT&T(X)
  - [어셈블리어 Intel / AT&T 문법](./asm_syntax.md)

## 필수 작성 및 체크리스트

- [ ] strlen
- [ ] strcpy
- [ ] strcmp
- [ ] write
- [ ] read
- [ ] strdup

- [ ] 시스템콜syscall 하는 중에 에러 있는지 체크
- [ ] 변수 errno 세팅 (extern ___error 사용 가능)

## Makefile 체크리스트

- [ ] $(NAME)
- [ ] all
- [ ] clean
- [ ] fclean

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



