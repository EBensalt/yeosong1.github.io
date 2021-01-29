# CPP Module 02 서브젝트

- [Ad-hoc polymorphism](https://en.wikipedia.org/wiki/Ad_hoc_polymorphism)
  - [블로그..](https://yinjae.wordpress.com/2012/04/02/polymorphism/)
- 대입 연산자 오버로드
- canonical('표준' 정도로 해석하면 될 거 같죠?..) 클래스

## 제너럴 룰
<details>
<summary> <b> 제너럴 룰 보기 </b>  </summary><br>
<div markdown="1">
  
- 헤더 안에 구현된 모든 기능(템플릿의 경우는 제외) 및 보호되지 않은 헤더는 exercise 0점을 의미합니다.
- 모든 **출력은 표준 출력으로** 하며, 특별히 지정하지 않는 한 **개행(\n)으로 끝납니다.**
- 부과된 파일 이름 뒤에는 letter, 클래스 이름, 함수 이름, 메서드 이름이 와야합니다.
- 기억하십시오: 이제 더 이상 C가 아닌 C++로 코딩하고 있습니다. 따라서:
  - 다음 기능은 **금지**되어 있으며 사용시 0점 처리를 받습니다. 묻지도 따지지도 마시오: ***alloc, *printf, free*
  - 기본적으로 표준 라이브러리의 모든 것을 사용할 수 있습니다. **그러나** C++ 버전의 함수를 사용하는 것이 현명할 것입니다.
    당신은 C에 익숙합니다. 당신이 아는 것을 유지하는 대신, C++ 버전의 함수를 사용하는 것이 현명할 것입니다. 결국 이건 새로운 언어입니다.
  - 그리고 **네, 안돼요.** 써도 될 때까지는 [STL](https://www.cplusplus.com/reference/stl/)을 사용할 수 없습니다.(즉, 모듈08 전에는 안됨).
  - 이는 include <algorithm>을 필요로 하는 모든 것은--벡터/리스트/맵/등등--다 안된다는 뜻입니다.
- 명시적으로 금지된 기능 또는 기계의 사용은 묻지도 따지지도 않고 0점 처리됩니다.
- 또한, 달리 명시되지 않는 한 C++ 키워드 **using namespace**및 **friend**는 금지되어 있습니다.
  - 그들의 사용은 질문없이 **-42점**으로 처리 될 것입니다.
- 클래스와 관련된 파일은 달리 명시되지 않는 한 항상 **ClassName.hpp** 및 **ClassName.cpp**입니다.
- Turn-in 디렉토리는 **ex00/**, **ex01/**, ... , **exn/**.
- 예제를 철저하게 읽어야합니다. exercise의 설명에서는 명확하지 않았던 요구 사항을 포함하고 있을 수 있습니다.
  만약 뭔가 모호해 보인다면, 당신이 **C++**를 충분히 이해하지 못한 것입니다.
- 앞에서부터 배운 **C++** 도구는 사용할 수 있으므로, external 라이브러리는 사용할 수 없습니다. 그리고 물어보기 전에 말해드려요:
  - 그것은 또한 **C++11과 파생 모델**, **Boost** 또는 C++ 없으면 못사는 놀라운 기술을 갖춘 친구가 알려준 그 어떤 것도 안된다는 뜻입니다.
  - = 범위 기반 for문 C++11이니까 사용 금지..
- 상당한 양의 클래스들을 제출해야 할 수도 있습니다. 이것은 좋아하는 텍스트 편집기를 스크립팅할 수 없다면 지루해 보일 수 있습니다.
- 시작하기 전에 각 exercise를 **완전히** 읽으십시오! 진짜로요, 읽으세요.
- 사용할 컴파일러는 **clang++**입니다.
- 코드는 다음 플래그를 사용하여 컴파일해야합니다: **-Wall -Wextra -Werror**
- 당신의 각 includes는 다른 includes들과 독립적으로 포함될 수 있어야 합니다. Includes는 분명히 그들이 의존하는 다른 모든 include를 포함해야합니다.
- 궁금할까봐: **C++에서는 코딩 스타일이 적용되지 않습니다.** 원하는 스타일 아무거나 사용 가능, 제한 없음. **하지만, 동료 평가자가 읽을 수 없는 코드는 채점 받을 수 없겠죠**
- 이제 중요한 사항 : 서브젝트에 명시적으로 설명하지 않는 한 **프로그램에 의해 채점되지 않습니다**. 따라서, 여러분은 exercise를 선택하는 방법에 있어서 어느 정도의 자유가 주어집니다. 하지만, 각 exercise의 제한조건에 유의하고, **게으르지 마세요**, 연습문제들이 제공해야되는 **많은 것들을 놓치게 될거예요!**
- **제출하는 파일에 일부 관계없는 파일이 있는 것은 문제가 되지 않습니다.** 요청한 파일보다 더 많은 파일로 코드를 분리할 수도 있습니다.
  결과가 프로그램에 의해 채점되지 않는 한, **자유롭게 하세요.**
- 비록 서브젝트의 exercise가 짧더라도, 알아야 할 것을 확실히 이해하고, 가능한 최선의 방법으로 풀었다는 것을 확실히 하기 위해 시간을 들이는 것은 가치가 있습니다.
- 오딘의 이름으로, 토르의 이름으로! 머리를 쓰세요!!!
  
 </div> 
 </details>
 <BR>

## 보너스 룰
이제부터는 모든 클래스를 **반드시** 표준(Coplien) 형식을 따라 써야합니다:

- 최소 하나의 `default 생성자`
- 최소 하나의 `복사생성자`
- 최소 하나의 `연산자 오버로드`
- 최소 하나의 `소멸자`

한 번만 말할게.

- vscode에서 42 Canonical Class CPP 익스텐션을 깔면 시간이 절약된다!

## ex00 나의 첫 표준 클래스 -- 고정소수점 담는 클래스를 표준에 맞춰서 만들기

당신은 정수와 부동 소수점 숫자도 알고 있습니다. 귀여워.

이 3장의 아티클([1](https://www.cprogramming.com/tutorial/floating_point/understanding_floating_point.html), [2](https://www.cprogramming.com/tutorial/floating_point/understanding_floating_point_representation.html), [3](https://www.cprogramming.com/tutorial/floating_point/understanding_floating_point_printing.html))를 읽고 넌 모른다는 것을 확인하시기 바랍니다. 빨리 가서 읽어봐.

지금까지 프로그램에서 사용한 숫자는 기본적으로 정수이거나 부동소수점 수 아니면,
<br>그 변형(`short`, `char`, `long`, `double` 등) 중 하나였습니다.
<br>방금 읽은 내용으로, 정수와 부동소수점 수가 서로 반대의 특성을 가졌다고 가정해도 무방합니다.

하지만 오늘, 이것은 바뀔 것입니다.
<br>당신은 새롭고 놀라운 숫자 자료형을 발견하게 될 것입니다.
<br>그건 고정소수점 숫자 입니다!
<br>고정소수점 숫자는, 대부분의 언어 스칼라 유형에서 항상 누락됩니다.
<br>고정소수점 숫자는 퍼포먼스(성능), 정확성, 범위, 정밀도 사이의 중요한 균형을 제공함으로써
<br>이런 숫자가 그래픽스나 사운드, 몇 가지 과학 관련 프로그래밍에서 널리 사용되는 이유를 설명합니다.

C++에는 고정소수점 숫자가 없기 때문에 당신이 오늘 직접 추가할 예정입니다.
저는 버클리의 [이 기사](https://inst.eecs.berkeley.edu//~cs61c/sp06/handout/fixedpt.html)를 시작점으로 추천하고 싶습니다. 그들에게 좋은 거라면, 당신에게 좋은 것입니다.
만약 버클리란게 뭔지 전혀 모르겠다면, 위키백과 페이지의 [이 부분](https://en.wikipedia.org/wiki/University_of_California,_Berkeley#Notable_alumni.2C_faculty.2C_and_staff)을 읽어보세요.

고정소수점을 나타내는 표준 클래스를 쓰세요.

- private 멤버:
  - **int** -- 고정소수점의 value를 저장할 int
  - **static const int** -- 분수 부분 비트들을 저장하는 정적 상수 int입니다. 이 상수는 항상 말 그대로 8이 됩니다.
- public 멤버:
  - default 생성자 -- 고정소수점 값 0으로 초기화 
  - 소멸자
  - 복사생성자
  - 대입연산자 오버로드
  - **int get RawBits(void) const;** -- 고정소수점 값의 raw 값을 반환하는 함수
  - **void setRawBits(int const raw);** -- 고정소수점 값의 raw 값을 세팅하는 함수

- [접근자 포스트 public, private, protected](https://yeolco.tistory.com/115)


### 코드
~~~C++
#include <iostream>

int main( void ) {

    Fixed a;
    Fixed b( a );
    Fixed c;

    c = b;

    std::cout << a.getRawBits() << std::endl;
    std::cout << b.getRawBits() << std::endl;
    std::cout << c.g

    return 0;
}
~~~
 
 위 코드는 아래 코드랑 비슷하게 결과값이 나와야한다.
 
~~~C++
$> clang++ -Wall -Wextra -Werror Fixed.class.cpp main.cpp
$> ./a.out
Default constructor called
Copy constructor called
Assignation operator called // <-- This line may be missing depending on your implementation
getRawBits member function called
Default constructor called
Assignation operator called
getRawBits member function called
getRawBits member function called
0
getRawBits member function called
0
getRawBits member function called
0
Destructor called
Destructor called
Destructor called
$>
~~~

## ex01 더 유용한 고정 소수점 클래스를 향해

- 허용함수 roundf (camth에 있음)

좋아요, ex00은 좋은 시작이었지만, <br>
우리 클래스는 여전히 꽤 쓸모없고, <Br>
오직 고정소수점 값 0.0 만을 나타낼 수 있습니다. <br>
아래의 public 생성자와 public 멤버 함수들을 클래스에 더합시다:

- const int를 매개 변수로 받고, 해당 고정소수점(8) 값으로 변환하는 생성자
  - 분수 비트 값은 **ex00**에서처럼 초기화됩니다.
- const float를 매개 변수로 받고, 해당 고정소수점(8) 값으로 변환하는 생성자
  - 분수 비트 값은 **ex00**에서처럼 초기화됩니다.
- **float toFloat(void) const;** -- 고정소수점 값을 부동소수점으로 바꾼다.
- **int toInt(void) const;** -- 고정소수점 값을 정수 값으로 바꾼다.

그리고, 헤더(선언)파일과 소스(정의)파일에 아래의 함수 오버로드를 추가합니다.

- `<<` 연산자 오버로드 -- 고정소수점 값의 부동소수점 표현을 출력 스트림 파라미터에 입력합니다.


### 코드

~~~C++
#include <iostream>

int main( void ) {

    Fixed a;
    Fixed const b( 10 );
    Fixed const c( 42.42f );
    Fixed const d( b );

    a = Fixed( 1234.4321f );

    std::cout << "a is " << a << std::endl;
    std::cout << "b is " << b << std::endl;
    std::cout << "c is " << c << std::endl;
    std::cout << "d is " << d << std::endl;
    
    std::cout << "a is " << a.toInt() << " as integer" << std::endl;
    std::cout << "b is " << b.toInt() << " as integer" << std::endl;
    std::cout << "c is " << c.toInt() << " as integer" << std::endl;
    std::cout << "d is " << d.toInt() << " as integer" << std::endl;
    
    return 0;
}
~~~

위 코드는 아래와 비슷한 결과를 내야함.

~~~C++
$> clang++ -Wall -Wextra -Werror Fixed.class.cpp main.cpp
$> ./a.out
Default constructor called
Int constructor called
Float constructor called
Copy constructor called
Assignation operator called
Float constructor called
Assignation operator called
Destructor called
a is 1234.43
b is 10
c is 42.4219
d is 10
a is 1234 as integer
b is 10 as integer
c is 42 as integer
d is 10 as integer
Destructor called
Destructor called
Destructor called
Destructor called
$>
~~~

## ex02 이제 우리 얘기해 -- 다양한 연산자 오버로딩

## ex03 고정 소수점 표현 -- 계산기 만들기
