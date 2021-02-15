# CPP Module 07 서브젝트

템플릿

## ex00 몇 가지 함수

함수 템플릿을 만드세요

- void swap( , ) 
- int min( , )
- int max( , )

- 템플릿은 반드시 .hpp에 정의하세요
- main.cpp 내세요. 이 파일은 평가중에 수정될 수 있고, 수정될 것입니다.
- 위 함수들은 두 인수의 유형이 동일하고 모든 비교 연산자를 지원하는 조건 하에, 어떤 자료형의 인자랑도 호출 가능합니다.

```cpp

int main( void ) {

  int a = 2;
  int b = 3;

  ::swap( a, b );
  std::cout << "a = " << a << ", b = " << b << std::endl;
  std::cout << "min( a, b ) = " << ::min( a, b ) << std::endl;
  std::cout << "max( a, b ) = " << ::max( a, b ) << std::endl;
 
  std::string c = "chaine1";
  std::string d = "chaine2";

  ::swap(c, d);
  std::cout << "c = " << c << ", d = " << d << std::endl;
  std::cout << "min( c, d ) = " << ::min( c, d ) << std::endl;
  std::cout << "max( c, d ) = " << ::max( c, d ) << std::endl;

  return 0;
}
```

아웃풋

```cpp
a = 3, b = 2
min(a, b) = 2
max(a, b) = 3
c = chaine2, d = chaine1
min(c, d) = chaine1
max(c, d) = chaine2
```

## ex01 Iter

- [x] 함수 템플릿 iter 만들기
  - `void iter(T *배열의주소, unsigned int 배열의길이, void(*f)(T &배열));`
  - 3번째 인자는 배열의 각 요소에 적용할 함수

실행 파일에 내용을 다 녹이세요.
<br><br>
내용:
- 함수 템플릿 iter가 어느 타입의 배열이랑도 잘 작동함을 증명
- 인스턴스화된 함수 템플릿을 세번째 매개변수로 받을 때도 잘 작동함을 증명




## ex02 Array

- [x] 클래스 템플릿 Array 만들기
  - [x] type T 요소를 포함하고 다음 동작을 허용하는 클래스 템플릿을 작성합니다.
    - [x] 생성자(매개변수 없음) = 빈 배열 생성
    - [x] 생성자(unsigned int n) = 배열[n] 생성, 디폴트로 초기화
      - 팁: `int *a = new int();`을 컴파일 하고 `*a`를 디스플레이 해보세요.
    - [x] 복사생성자, 연산자= : rhs, lhs 어느 쪽을 수정해도 나머지 한 쪽에 아무 영향도 끼치지 않게 하세요.(=딥카피로 분리하기)
    - 할당시 반드시 new[] 연산을 쓸 것. 예방적 할당을 해서는 안 됩니다. 할당되지 않은 메모리에 액세스해서는 안 됩니다.
    - [x] 요소들은 연산자[]로 접근 가능해야 합니다.
      - 연산자[]로 접근할 때, 만약 이 요소가 범위(limits)바깥일 경우 std::exception을 throw
    - [x] 멤버함수 const unsigned int size(void);는 배열 요소의 개수를 리턴하며, 현재 인스턴스를 수정하지 않습니다. (=const다)
