# ft_containers 서브젝트

- [**cplusplus**](https://www.cplusplus.com/reference/stl/), [**cpprefernce**](https://en.cppreference.com/w/cpp/container), [**microsoft**](https://docs.microsoft.com/en-us/cpp/standard-library/stl-containers?view=msvc-160)
- [**한빛 stl**](https://www.hanbit.co.kr/store/books/look.php?p_code=E6410226806) - 가입하고 pdf 무료 다운
  - cpp가 처음이면 추천
  - 공식문서 스타일 표 + 예제 + 한글 + 친절
- 자료구조와 알고리즘 기초라도 보고 시작하면 어디에 쓰일지 알고 만드는 거니까 덜 답답할듯
 

## 목표

- C++ STL의 여러가지 컨테이너 타입을 구현하게 됩니다.
- 각 컨테이너마다 적절한 이름을 붙인 클래스 파일을 제출하세요.
- **네임스페이스는 항상 `ft`를 사용**
- 컨테이너는 `ft::<contatiner>`로 테스트 됩니다.
- C++98로 코딩하는 거니까 컨테이너의 새로운 기능들은 **구현하지 마시고**, 대신
- 모든 옛날 기능들(도태된 기능 포함)은 구현하세요.

## 필수 파트

- 아래 컨테이너들을 구현하고, 필요한 파일들 `<container>.hpp`을 제출하세요.
- main.cpp
- 멤버 함수 get_allocator는 안해도 되고
  - **나머지 멤버 함수들은 하세요.**
  - **Non-member 오버로드도 하세요.**
- **만약 컨테이너에 iterator 시스템이 있으면 반드시 재구현 하세요.**
- 구현할 내용을 [https://www.cplusplus.com/](https://www.cplusplus.com/)에서 참고할 수 있습니다.
- 기존 컨테이너에서 제공되는 것 이외에 추가적인 public function을 구현해 넣지 마세요.
  - 그 이외의 모든 것은 반드시 private 아니면 protected여야 합니다.
  - 모든 public fuction, public variable은 근거가 있어야 합니다.
- non-member 오버로드는 friend 키워드 사용 가능!!!
  - 모든 friend의 사용이 근거가 있어야하며, 평가중에 확인할 것임.

아래와 같은 컨테이너들과 그와 관련된 함수들을 제출하세요.

- List
- Vector
- Map
- Stack
- Queue

물론 STL은 금지고요. 근데 STD 라이브러리 사용은 허용합니다.

- [필수 파트 구현](컨테이너필수.md)

## 보너스 파트

- Deque
- Set
- Muliset
- Multimap


----------------------


- [https://code-algalon.tistory.com/188](https://code-algalon.tistory.com/188)
