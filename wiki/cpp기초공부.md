# cpp 생전 처음......

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
          
