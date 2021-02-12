# CPP Module 06 서브젝트

형변환 캐스트

## ex00: Scalar conversion

### 매개변수는
- C++ 리터럴 값의 문자열 표현(가장 일반적인 형태)
- 이 리터럴은 다음 스칼라 유형 중 하나에 속해야 합니다: char, int, float, double.
- 십진수 표기법만 들어옵니다.

#### 인자의 분류 예시

1. char literal values: 'c', 'a'...
- 논프린터블은 인자로 안받는 걸로 치자
- 만약 변환한게 논프린터블이면 결과 대신 알림을 넣자

2. int literal values: 0, -42, 42...

3. float literal values: 0.0f, -4.2f, 4.2f... -inff, +inff, nanf

4. double literal values: 0.0, -4.2, 4.2... -inf, +inf, nan


### 이 프로그램은

- 위에 나열한 예시들 같은 것들을 인자로 받는 프로그램이고, 
- 받은 인자의 리터럴의 타입이 무엇인지 감지해낼 수 있어야 한다.
- 올바른 유형의 리터럴을 획득한 다음(그럼 더 이상 문자열이 아님)
- 이를 다른 세 가지 유형으로 **명시적으로 explicitly** 변환하고
- 아래 예시처럼 출력 (변환 표기 가능한 건 하고, 아닌 건 impossible)

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

### Allowed functions: 스트링 -> int, float, double로 변환하는 함수들. 

- stod(), stof(), stoi()



## ex01: Serialization


## ex02: Identify real type

