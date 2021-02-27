# ft_containers 서브젝트

## Objecives 목표

- C++ STL의 여러가지 컨테이너 타입을 구현하게 됩니다.
- 각 컨테이너마다 적절한 이름을 붙인 클래스 파일을 제출하세요.
- **네임스페이스는 항상 `ft`를 사용**
- 컨테이너는 `ft::<contatiner>`로 테스트 됩니다.
- C++98로 코딩하는 거니까 컨테이너의 새로운 기능들은 **구현하지 마시고**, 대신
- 모든 옛날 기능들(도태된 기능 포함)은 구현하세요.

## 필수 파트

- 아래 컨테이너들을 구현하고, 필요한 파일들 `<container>.hpp`을 제출하세요.
- main.cpp
- 멤버 함수 get_allocator는 안해고 되고
  - 나머지 멤버 함수들은 하세요.
  - Non-member 오버로드도 하세요.
- 만약 컨테이너에 iterator 시스템이 있으면 반드시 재구현 하세요.
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

## 보너스 파트

- Deque
- Set
- Muliset
- Multimap
