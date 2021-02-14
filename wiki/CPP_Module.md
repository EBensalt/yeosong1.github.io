# CPP
- 학습: [유튜브 [두들낙서 C/C++](https://www.youtube.com/watch?v=nYh7pEX9lAE&list=PLlJhQXcLQBJqywc5dweQ75GBRubzPxhAk&index=54)] + [구글링] + [윤성우 열혈 C++]

## [CPP Module 00](cpp00.md)
### 네임스페이스, 멤버변수/멤버함수, iostream, const, static, 초기화 목록

## [CPP Module 01](cpp01.md)
### 생성자/소멸자, 포인터/레퍼런스, this, new/delete, public/private, fstream(파일 열기/읽기/쓰기)

## [CPP Module 02](cpp02.md)
### 클래스 구성 표준, 복사생성자, 얕은/깊은 복사, 연산자 오버로딩, protected (+ 부동소수점/고정소수점)

## [CPP Module 03](cpp03.md)
### 상속/가상상속/다중상속

## [CPP Module 04](cpp04.md)

### 순수가상함수
- virtual 함수 () = 0;
- 구현 없이 선언만 있는 함수에 = 0을 붙여서 확실하게 표시해준다.

### 추상abstract 클래스
- 하나 이상의 순수가상함수를 포함하는 클래스

### 구상, 구체concrete 클래스
구체적인 구현이 있는 클래스..

### 인터페이스
- 순수가상함수만을 멤버로 갖는 클래스
- 앞으로 만들 것들에 대한 선언으로 기능한다.

### getter/setter함수(액세스 함수)

- 정보은닉 해둔 값으로의 접근을 도와주는 함수
- getter 함수 : private 멤버 변수의 값을 리턴 ex) getName()
- setter 함수 : private 멤버 변수의 값을 설정할 수 있는 함수이다 ex) setName(…)

## [CPP Module 05](cpp05.md)

### 예외처리 클래스 std::exception
c++ 기본 제공, 예외처리에 필요한 것들 보유
- what() 함수
  - std::exception 소속
  - 문자열을 리턴하는 함수
  - catch문 속에서 출력할 에러 문구를 리턴하는 식 (std::cerr << e.what() << '\n';)
  
### try/catch/throw

try
- 예외가 일어날 부분만 묶기 보다는 작업 단위로 블록을 묶는다
- try 블록 내(블록 내에서 실행된 함수 내부 포함)에서 예외가 발생하면, 발생 지점 이후 부분은 건너뜀(continue)

throw
- if(예외) throw 클래스/변수/함수 등;

catch (클래스/변수/함수 등)
- 값에 대한 예외문제 처리(조작 등)은 필요시 때에 따라 여기저기서 하는 듯. 
- catch 안에서 꼭 하는 거는 에러 문구 출력 파트.
- 타입 마다 다른 캐치문을 만들어서 골라 쓸 수도 / 한 캐치문에서 catch (...) 다 받을 수도 있고.

## [CPP Module 06](cpp06.md)

### C++의 형변환

#### static_cast : A 타입에서 B 타입으로 : 책임은 개발자가

형태 static_cast<T>(변환의_대상)
  
 베이스 클래스의 포인터 및 참조형 데이터 -> 파생 클래스의 포인터 및 참조형 데이터
 파생 클래스의 포인터 및 참조형 데이터 -> 베이스 클래스의 포인터 및 참조형 데이터

#### const_cast

#### dynamic_cast : 상속 관계에서의 안전한 형 변환 : 파생 클래스 형 -> 베이스 클래스 형으로의 변환
형태 dynamic_cast<T>(변환의_대상)

- 파생 클래스의 포인터 및 참조형 데이터 -> 베이스 클래스의 포인터 및 참조형 데이터


#### reinterpret_cast

| | dynamic_cast | static_cast | const_cast | reinterpret_cast |
|--|---|---|---|---|
| | 파생을 베이스로 변환, 안전 보장 | 책임은 개발자가 |const, volatile 성향 제거 | 책임은 개발자가, 상관없는 자료형으로 변환 가능 |
|케이스|베이스의 포인터 및 참조형 데이터 -> 파생의 포인터 및 참조형 데이터<br>파생의 포인터 및 참조형 데이터 -> 베이스의 포인터 및 참조형 데이터| 파생의 포인터 및 참조형 데이터 -> 베이스의 포인터 및 참조형 데이터||포인터와 관련이 있는 모든 유형의 형 변환을 허용| 
|형태|`dynamic_cast<T>(변환의_대상)`|`static_cast<T>(변환의_대상)`|`const_cast<T>(변환의_대상)`|`reinterpret_cast<T>(변환의_대상)`|
|| 런타임에 안전성 검사(=static보다 느림) | 컴파일 타임에 결정 | || 



## [CPP Module 07](cpp07.md)

### 템플릿



## [CPP Module 08](cpp08.md)
### STL / iterator / algorithm in C++
