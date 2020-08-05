# cpp 생전 처음......
c++로 작성된 코드를 읽다가 모르는 것을 검색해서 메모

# 출력 cout 

```C++
#include <iostream>

int main(int argc, char** argv)
{
  std::cout << "Hi world" << std::endl;
  return (0);
}
```

# argv 인자 받아 출력 + using namespace std;는 무엇인가

```C++
#include <iostream>
using namespcae std;

int main (int argc, char** argv)
{
  cout << "Have " argc << "argc: " << endl;

  int i = -1;

  while(++i < argc)
    cout << "argv[" << i << "] : " << argv[i] <<endl;

  return (0);
}
```
# c++은 구조체 안에 함수 정의 가능
[참고-클래스의 기본 by Jong Hyuk Park](http://www.parkjonghyuk.net/lecture/program2/chap03.pdf)

```C++
struct Account {
  char accID[20];
  char secID[20];
  char name[20];
  int balance;
  void Deposit(int money){
  balance+=money;
  }
  void Withdraw(int money){
  balance-=money;
  }
};

int main(void)
{
  Account yoon={"1234", "2321", "yoon", 1000};
  yoon.Deposit(100);
  cout<<"잔 액 : "<<yoon.balance<<endl;
  return 0;
}
```

# vector 컨테이너

- 자동으로 할당되는 배열.
- vector의 멤버 함수들과 함께 사용.
- 헤더
  - include <vector>
- 선언
  - vector<자료형>변수 이름
- [참고-벡터 컨테이너 정리 및 사용법](https://blockdmask.tistory.com/70)

~~~
#include <vector>

...

std::vector<int> m;  // 비어있는 m 생성
m.pushback(7); // 배열에서 맨 마지막 원소 뒤에 원소 7을 추가
~~~
          
# 다형성 polymorphism
다형성이라는 단어는 많은 형태를 갖는 것을 의미.<br>
일반적으로 다형성은 클래스의 계층 구조가 있고 상속 관계가 있을 때 발생.

# 오버로딩
하나의 객체에서 이름이 같은 메소드를 여러개 정의하여 사용하는 것.<br>
메소드에 전달되는 인자의 종류와 개수는 다름. 리턴 타입은 상관없음.

- 함수 오버로딩
- 생성자 오버로딩
- 연산자 오버로딩

# 오버라이딩
부모 클래스에서 정의한 메서드를 자식 클래스에서 재정의.<br>
메서드명, 매개변수, 리턴 타입이 모두 같음.

# 자료형 auto
초기화 값을 보고 알아서 자료형을 잡음 = 선언만 하는 것은 불가능

# 형변환 연산자
- static_cast
  - 컴파일(정적) 타임에 형변환이 가능한지 검사한다.
  - [https://mynameisdabin.tistory.com/20](https://mynameisdabin.tistory.com/20)
- dynamic_cast
- reinterpret_cast
- const_cast
