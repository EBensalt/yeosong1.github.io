# C++ 컨테이너 이지모드

## 제너럴 룰
- 모든 헤더에 구현된 함수(템플릿 뺴고)랑, protected 되지 않은 모든 헤더는 0점 처리
- 제시된 파일/클래스/함수/메소드 이름을 따를 것
- C++ 이니까 
  -`*alloc`, `*printf`, `free` 금지
 
... 생략 하겠음

- `-Wall -Wextra -Werror -std=c++98`

##  목표

이 프로젝트에서는 C++ STL의 다양한 컨테이너 유형을 구현합니다.
<br>각 컨테이너에 대해 적절하게 명명된 클래스 파일을 제출합니다.
<br>네임스페이스는 항상 `ft`이고 컨테이너는 `ft::<container>`를 사용하여 테스트됩니다.
<br>참조 컨테이너의 구조를 존중해야 합니다. orthodox canonical form 일부가 누락된 경우, 그것을 구현하지 마십시오.
<br>다시 말하지만, 우리는 C++98로 코딩하고 있으므로 컨테이너의 새로운 기능은 구현되지 않아야 하고, 모든 이전 기능(심지어 더 이상 사용되지 않는 것도)은 요구됩니다.

## 필수 파트

- 아래 컨테이너들을 구현하고, 필요한 파일들 <container>.hpp을 제출하세요.
- main.cpp과 + 테스트들
- 당신의 컨테이너만으로 하나의 바이너리를 생성하고, 동일한 테스트를 STL 컨테이너로 진행합니다.
- 결과물과 소요시간을 비교하세요 (20배까지 느려도 됨)
- 멤버 함수들, Non-member, 오버로드가 요구됩니다.
- 이름을 잘 지키고, 디테일에 신경쓰세요
- std::allocator를 써야합니다.
- 각 컨테이너에 대한 내부 데이터 구조를 정당화해야 합니다(simple array을 사용한 map은 괜찮지 않습니다).
- 컨테이너에 iterator 시스템이 있는 경우 구현해야 합니다.
- **iterators_traits, reverse_iterator, enable_if, is_integral, equal/lexicographicalcompare, std::pair, std::make_pair를 재구현해야 합니다.**
- https://www.cplusplus.com/ 및 https://cppreference.com/을 참고할 수 있습니다.
- 표준 컨테이너에서 제공하는 것보다 더 많은 public 함수를 구현할 수 없습니다. 다른 모든 것은 private이거나 protected여야 합니다. 각 public 함수/변수는 정당화되어야 합니다.
- non-member 오버로드의 경우 키워드 friend가 허용됩니다. friend를 사용할 때마다 정당화되어야 하며 평가 중에 확인됩니다.

아래와 같은 컨테이너들과 그와 관련된 함수들을 제출하세요.

- [Vector](std벡터.md)
- [Map](std맵.md)
- [Stack](std스택.md)

스택은 벡터 클래스를 default underlaying 컨테이너로 사용합니다. STL과 같은 다른 컨테이너와 호환됩니다.
<br>STL 컨테이너는 금지되어 있습니다.
<br>STD 라이브러리를 사용할 수 있습니다.

## 보너스 파트
Set 레드 블랙 트리로

### Vector C++98

#### member type: 	definition	/ notes
- [ ] value_type:	The first template parameter (T)	
- [ ] allocator_type:	The second template parameter (Alloc)	/ defaults to: allocator<value_type>
- [ ] reference:	allocator_type::reference	/ for the default allocator: value_type&
- [ ] const_reference:	allocator_type::const_reference	/ for the default allocator: const value_type&
- [ ] pointer:	allocator_type::pointer	/ for the default allocator: value_type*
- [ ] const_pointe:r	allocator_type::const_pointer	/ for the default allocator: const value_type*
- [ ] iterato:r	a random access iterator to value_type	/ convertible to const_iterator
- [ ] const_iterator:	a random access iterator to const value_type	
- [ ] reverse_iterator:	reverse_iterator<iterator>	
- [ ] const_reverse_iterator:	reverse_iterator<const_iterator>	
- [ ] difference_type:	a signed integral type, identical to: iterator_traits<iterator>::difference_type	/ usually the same as ptrdiff_t
- [ ] size_type:	an unsigned integral type that can represent any non-negative value of difference_type	/ usually the same as size_t
  
#### Member functions : explaination
- [ ] (constructor) : Construct vector (public member function )
- [ ] (destructor) : Vector destructor (public member function )
- [ ] operator= : Assign content (public member function )

##### Iterators:
- [ ] begin : Return iterator to beginning (public member function )
- [ ] end : Return iterator to end (public member function )
- [ ] rbegin : Return reverse iterator to reverse beginning (public member function )
- [ ] rend : Return reverse iterator to reverse end (public member function )

##### Capacity:
- [ ] size : Return size (public member function )
- [ ] max_size : Return maximum size (public member function )
- [ ] resize : Change size (public member function )
- [ ] capacity : Return size of allocated storage capacity (public member function )
- [ ] empty : Test whether vector is empty (public member function )
- [ ] reserve : Request a change in capacity (public member function )

##### Element access:
- [ ] operator[] : Access element (public member function )
- [ ] at : Access element (public member function )
- [ ] front : Access first element (public member function )
- [ ] back : Access last element (public member function )

##### Modifiers:
- [ ] assign : Assign vector content (public member function )
- [ ] push_back : Add element at the end (public member function )
- [ ] pop_back : Delete last element (public member function )
- [ ] insert : Insert elements (public member function )
- [ ] erase : Erase elements (public member function )
- [ ] swap : Swap content (public member function )
- [ ] clear : Clear content (public member function )

##### Allocator:
- [ ] get_allocator : Get allocator (public member function )

##### Non-member function overloads
- [ ] relational operators : Relational operators for vector (function template )
- [ ] swap : Exchange contents of vectors (function template )

##### Template specializations
- [ ] vector<bool> : Vector of bool (class template specialization )
