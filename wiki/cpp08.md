# CPP Module 08 서브젝트

## 오늘의 규칙

여러분은 이 특정 주제에서 여러분이 풀어야 할 많은 문제들이
표준 컨테이너를 사용하지 않고 표준 알고리즘을 사용하지 **않고**
풀 수 있다는 것을 알게 될 것입니다. 그러나, **이러한 것들을 사용하는 것이 바로 목표**이며,
적절한 곳에서 표준 컨테이너와 알고리즘을 사용하기 위해 모든 노력을 기울이지 않는다면,
아무리 기능적이더라도 매우 나쁜 점수를 받게 될 것입니다. 너무 게을러지지 마세요.

## ex00: Easy find

시작하기 좋은 쉬운 문제..

- [x] 템플릿 함수 easyfind 만들기
  - template <typename T>
  - easyfind(T, int)
    - T를 int의 컨테이너로 가정한다.
    - 인자1이 처음으로 등장하는 부분을 찾는 것으로 간주한다.
  - 찾았는데 없으면 error (exception 써도되고 error 리턴 밸류를 사용해도 됨)
    - 스탠다드 컨테이너에서 어떻게 작동하는지에서 아이디어를 얻으세요.
- main


## ex01: Span 폭. 범위.

- [x] 클래스 만들기
  - unsigned int N
  - 생성자(N)
  - addNumber() - 숫자 1개를 저장하는 함수
    - 이미 있는 숫자면 erro exception
- shortestSpan, longestSpan
  - 오브젝트에 들어있는 모든 숫자들 간의 span(차) 중 가장 짧은 span을 리턴.
  - 들어있는 숫자가 1개 or 0개면 span이 없으므로 exception


아래는 test main 예시와 출력값이다.
최소한 10000개는 테스트 해야함. 더 많으면 좋고.
그리고 iterator의 범위를 줘서 숫자를 추가할 수 있다면,
숫자를 추가하기 위해 addNumber를 수천번 호출하는 번거로움을 피할 수 있을 것입니다...


```cpp
int main()
{
  Span sp = Span(5);
  sp.addNumber(5);
  sp.addNumber(3);
  sp.addNumber(17);
  sp.addNumber(9);
  sp.addNumber(11);
  std::cout << sp.shortestSpan() << std::endl;
  std::cout << sp.longestSpan() << std::endl;
}

$> ./ex01
2
14
$>
```


## ex02:  Mutated abomination 돌연변이 혐오?

자, 에피타이저는 끝났고, 이제 좀 역겨운 걸 해보자.

std::stack 컨테이너는 짱 멋져 근데 iterable이 아닌 유일한 STL 컨테이너 입니다.
안됐군요. 하지만 우리가 신처럼 우리가 좋아하는 것들을 더하기 위해 잘라낼 수 있을 때 왜 괜찮을까요?

이 능력을 std::stack 컨테이너에 접목시켜 이 심각한 부정을 해결할 수 있습니다.
std::stack의 관점에서 구현될 MutantStack 클래스를 만들고, 모든 멤버 함수를 제공하지만, 오직 반복자만 제공한다.

아래는 코드의 예로서, 출력은 MuttentStack을 예를 들어 std::list로 교체한 경우와 동일해야 한다.
물론 주 기능에서 이 모든 것에 대한 테스트를 제공할 것입니다.










