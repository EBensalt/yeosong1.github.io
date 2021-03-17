
- [https://www.cplusplus.com/reference/list/list/](https://www.cplusplus.com/reference/list/list/)
- `template < class T, class Alloc = allocator<T> > class list;`

### list 특징

- Doubly-linked list 이므로
  - 랜덤한 직접 접근이 불가능
  - --나 ++를 사용해서 이동
  - 중간 삽입 및 삭제가 용이
    - (배열처럼 연속해서 이어진 형태는 중간 삽입 하려면 해당 공간을 밀어내면서 새로 한 세트를 만들어 옮기는 꼴이 되어서 더 오래 걸림) 
- Allocator-aware
  - 이 컨테이너는 allocator 객체를 사용해서 필요한 저장공간을 동적으로 제어함.

-> 운용중 데이터 추가 및 삭제는 많고, 검색 및 랜덤 위치 접근은 적은 경우에 사용하기 좋은 컨테이너.

### 템플릿 매개변수
- T
- Alloc

### public 멤버 변수
- value_type
- allocator_type
- reference
- const_reference
- pointer
- const_pointer
- iterator
- const_iterator
- reverse_iterator
- const_reverse_iterator
- difference_type - 보통 first와 last의 차이(= 요소의 개수)를 담는 데에 씀
- size_type

### public 멤버 함수
- 생성자
- 소멸자
- 연산자=

#### Iterators

- begin 맨 첫번째 요소를 가리키는 iterator를 리턴
- end 진짜 맨 마지막 위치를 가리키므로, 맨 마지막 요소가 아닌 그 뒤 - 즉 사이즈를 가리키는 iterator- 를 리턴함
- rbegin 맨 뒤를 시작점으로 가리키는 reverse_iterator를 리턴
- rend 맨 앞을 가리키는..

#### Capacity

- empty 컨테이너가 비어있는지 테스트 함
- size 사이즈를 리턴 함
- max_size 최대 사이즈를 리턴 함..

#### Element access

- front
- back

#### Modifiers
- assign
- push_front
- pop_front
- push_back
- pop_back
- insert
- erase
- swap
- resize
- clear

#### Operations
- splice 리스트에서 리스트로 요소를 전송함
- remove
- remove_if 조건 만족시 제거. 퍼블릭 멤버 함수 템플릿임!!
- unique 복제한 값을 제거?
- merge 머지 소트
- sort
- reverse

#### Observers
- get_allocator  --> 안해도 됨

### non-member function 오버로드

- relational operators(list) 관계 연산자(`>, >=, <, <=, ==, !=`)
- swap(list) 두 리스트의 내용을 스왑

항목별 자세한 설명.. [https://docs.microsoft.com/ko-kr/cpp/standard-library/list-class?view=msvc-160](https://docs.microsoft.com/ko-kr/cpp/standard-library/list-class?view=msvc-160)

