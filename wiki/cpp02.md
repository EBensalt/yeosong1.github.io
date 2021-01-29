# CPP Module 02 서브젝트

- [Ad-hoc polymorphism](https://en.wikipedia.org/wiki/Ad_hoc_polymorphism)
- 연산자 오버로드
- canonical('표준' 정도로 해석하면 될 거 같죠?..) 클래스

## 제너럴 룰

## 보너스 룰
이제부터는 모든 클래스를 **반드시** 표준(Coplien) 형식을 따라 써야합니다:

- 최소 하나의 `default 생성자`
- 최소 하나의 `복사 생성자`
- 최소 하나의 `연산자 오버로드`
- 최소 하나의 `소멸자`

한 번만 말할게.

- vscode에서 42 Canonical Class CPP 익스텐션을 깔면 시간이 절약된다!

## ex00 나의 첫 표준 클래스 -- 캐노니컬 클래스

당신은 정수와 부동 소수점 숫자도 알고 있습니다. 귀여워.

이 3장의 아티클([1](https://www.cprogramming.com/tutorial/floating_point/understanding_floating_point.html), [2](https://www.cprogramming.com/tutorial/floating_point/understanding_floating_point_representation.html), [3](https://www.cprogramming.com/tutorial/floating_point/understanding_floating_point_printing.html))를 읽고 넌 모른다는 것을 확인하시기 바랍니다. 빨리 가서 읽어봐.

지금까지 프로그램에서 사용한 숫자는 기본적으로 정수이거나 부동소수점 수 아니면 그 변형(`short`, `char`, `long`, `double` 등) 중 하나였습니다. qkd 정수와 부동 소수점 수가 반대라고 가정해도 무방합니다.
자동차 특성

하지만 오늘날, 이것은 바뀔 것이다. 당신은 새롭고 놀라운 숫자를 발견하게 될 것이다.
유형: 고정 포인트 번호! 대부분의 언어 스칼라 유형에서 항상 누락됨, 고정점
숫자는 성능, 정확도, 범위 및 정밀도 사이의 귀중한 균형을 제공합니다.
왜 이 숫자들이 그래픽, 사운드 또는 과학적 프로그래밍에 널리 사용되는지 설명한다.
몇 가지 이유를 대자면

C++에는 고정 포인트 번호가 없기 때문에 오늘 직접 추가할 예정입니다. 나는 이 기사를 버클리로부터의 시작으로 추천하고 싶다. 그들에게 좋은 것이라면, 당신에게 좋은 것입니다. 만약
버클리란 무엇인지 전혀 모르실 겁니다. 위키백과 페이지의 이 부분을 읽어보세요.


## ex01 더 유용한 고정 소수점 클래스를 향해
## ex02 이제 우리 얘기해
## ex03 고정 소수점 표현
