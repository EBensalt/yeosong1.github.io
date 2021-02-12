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

함수 템플릿
`void iter(T *배열의주소, unsigned int 배열의length, void(*f)() -- 배열의 각 요소에 적용 );`

함수 템플릿이 모든 유형의 배열 및/또는 인스턴스화된 함수 템플릿을
세 번째 매개 변수로 사용됨을 증명하는 실행 파일로 작업을 wrap 하세요.








## ex02 Array
