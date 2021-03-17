
- [https://www.cplusplus.com/reference/list/list/](https://www.cplusplus.com/reference/list/list/)
  
~~~
list는 시퀀스 내 어디서나 일정한 시간 삽입 및 지우기 작업을 허용하고 양방향에서 반복할 수 있는 시퀀스 컨테이너입니다.

list 컨테이너는 이중으로 연결된 목록으로 구현됩니다. 이중으로 연결된 list는 포함된 각 요소를 서로 다른 저장 위치와 관련 없는 저장 위치에 저장할 수 있습니다.
순서는 그 앞의 요소에 대한 링크와 그 뒤의 요소에 대한 링크의 각 요소에 대한 연결에 의해 내부적으로 보관됩니다.

Forward_list와 매우 유사합니다. 주요 차이점은 forward_list 객체는 단일 연결 list 이기 때문에 다소 작고 효율적인 대신 앞으로만 반복할 수 있다는 것입니다.

다른 기본 표준 시퀀스 컨테이너(어레이, 벡터 및 데크)와 비교하여 list는 일반적으로 iterator가 이미 얻어진 컨테이너 내의 모든 위치에서 요소를 삽입, 추출 및 이동하는데 더 나은 성능을 발휘하며,
따라서 정렬 알고리즘과 같이 이러한 요소를 집중적으로 사용하는 알고리듬에서도 더 나은 성능을 발휘합니다.

이러한 다른 시퀀스 컨테이너와 비교하여 list 및 forward_lists의 주요 단점은 위치에 의해 요소에 직접 접근할 수 없다는 것이다.
예를 들어 list의 여섯번째 요소에 액세스하려면 알려진 위치(시작 또는 끝과 같은)에서 해당 위치까지 반복해야 하며, 이 위치 사이의 거리에는 선형 시간이 걸린다.
또한 각 요소와 연결된 링크 정보를 유지하기 위해 약간의 추가 메모리를 사용합니다(작은 크기의 각 요소로 이루어진 아주 긴 목록일 경우 중요한 요소일 수 있겠죠).
~~~

- `template < class T, class Alloc = allocator<T> > class list;`

### list 특징

- 시퀀스 컨테이너다
- Doubly-linked list
  - 랜덤한 직접 접근이 불가능
  - --나 ++를 사용해서 이동
  - 링크드 리스트이므로 중간 삽입 및 삭제가 용이
    - 배열처럼 연속해서 이어진 형태는 중간 삽입 하려면 해당 공간을 밀어내면서 새로 한 세트를 만들어 옮기는 꼴이 되어서 더 오래걸림 
- Allocator-aware
  - 이 컨테이너는 allocator 객체를 사용해서 필요한 저장공간을 동적으로 제어함.

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

- begin 이터레이터를 맨 앞으로 리턴 함?
- end 
- rbegin
- rend

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

