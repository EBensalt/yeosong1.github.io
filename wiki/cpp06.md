# CPP Module 06 서브젝트

형변환 연산자<br><br>
각 연습문제에 대해, 모든 캐스팅은 특정한 하나의 캐스팅에 의해 해결됩니다.<br>
평가에서는 선택한 내용이 예상된 캐스팅에 해당하는지 여부를 확인합니다.

## ex00: Scalar conversion : 들어온 인자를 타입별로 변환하여 출력하는 프로그램


### 매개변수는
- C++ 리터럴 값의 문자열 표현(가장 일반적인 형태)
- 이 리터럴은 다음 스칼라 유형 중 하나에 속해야 합니다: char, int, float, double.
- 십진법만 사용합니다.(a나 0x61 이렇게 안들어오고 97 이렇게 들어온다는 얘기인듯)

#### 인자의 분류 후 출력 예시?

1. char literal values: 'c', 'a'...
 - 인자에 논프린터블은 안들어오는 걸로 칩니다.
 - 만약 변환한 숫자가 논프린터블이면 결과 대신 알림을 넣습니다.

2. int literal values: 0, -42, 42...

3. float literal values: 0.0f, -4.2f, 4.2f... -inff, +inff, nanf

4. double literal values: 0.0, -4.2, 4.2... -inf, +inf, nan

(nan: not a number, inf: infinity)

### 이 프로그램은

- 위에 나열한 예시들 같은 것들을 인자로 받는 프로그램이고, 
- 받은 인자의 리터럴의 타입이 무엇인지 감지해낼 수 있어야 한다.
- 올바른 유형의 리터럴을 획득한 다음(그럼 더 이상 문자열이 아님)
- 이를 다른 세 가지 유형으로 **명시적으로 explicitly** 변환하고
- 아래 예시처럼 출력 (변환 표기 가능한 건 하고, 변환이 안되거나 오버플로우는 display that the conversion is impossible)

#### 결과물 출력 예시
 
```cpp
/convert 0
char: Non displayable
int: 0
float: 0.0f
double: 0.0
./convert nan
char: impossible
int: impossible
float: nanf
double: nan
./convert 42.0f
char: '*'
int: 42
float: 42.0f
double: 42.0
```

### Allowed functions: 스트링을 -> int, float, double로 변환하는 함수들. 

- stoi(), stof(), stod()


## ex01: Serialization : 섞인 자료형으로 이어놓은 데이터를 원하는 자료형 크기별로 다시 쪼개는 내용

### void * serialize(void);

- 직렬화된serialized 데이터의 일부를 나타내는 일련의 바이트들의 힙에 할당된 주소를 반환합니다.
- 직렬화된 데이터는 [8개의 영숫자로 된 배열1, 랜덤 정수 1개, 8개의 영숫자로 된 배열2]의 연결이다.
- 임의의 값을 생성하는 방법으로는 원하는 것 뭐든 자유롭게 사용하십시오.

### Data * deserialize(void * raw);

- 이 함수는 raw 데이터를 힙에 할당된 "structData {std::string s1; int n; std::string s2; };"로 정의된 데이터 구조를 역직렬화합니다.

## ex02: Identify real type
- `<typeinfo>` 사용금지
 - typeinfo란?...

### Base class

- Base는 public : virtual destructor만 갖고 있습니다.
- Base를 public 상속으로 비어있는 class A, B, C 를 만드세요.

### Base * generate(void);

- A B C 중 랜덤으로 인스턴스화 하는 함수
- 만든 인스턴스를 Base 포인터로 리턴
- 랜덤 설정 방식 자유

### void identify_from_pointer(Base * p);

- p의 진짜 타입에 따라 A, B, C 중 하나를 디스플레이

### "void identify_from_reference( Base & p);

- p의 진짜 타입에 따라 A, B, C 중 하나를 디스플레이
