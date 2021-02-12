#  CPP Module 05 서브젝트

반복과 예외처리. try catch throw

## ex00: 엄마 나 크면 공무원이 될래

예외 클래스들은 표준 폼 안맞춰도 됩니다.<br>
나머지 다른 클래스들은 맞추기

### Bureaucrat

- const name
- grade 1(highest)-150(lowest)
- attempt to creat invalid grade -> throw an exception
  - Bureaucrat::GradeTooHighException 
  - Bureaucrat::GradeTooLowException
- getName
- getGrade
- setIncreaseGrade
- setDecreaseGrade
- `<<`연산자 ostream "<name>, bureaucrat grade <grade>"


```cpp
try
{
/* do some stuff with bureaucrats */
}
catch (std::exception & e)
{
/* handle exception */
}
```

## ex01: 일해라, 굼벵이들아!

### Form class
- **private**
  - const name
  - boolean is_signed (0으로 초기화)
  - const signGrade (사인 가능 grade)
  - const executeGrade (실행 가능 grade)

- **public**
  - Form::GradeTooHighException
  - Form::GradeTooHighException
  - getName
  - getSignGrade
  - getExecuteGrade
  - getSigned
  - beSigned
  
- `<<`연산자 오버로드 form의 상태를 출력

```
beSigned(Bureaucrat)
{
  if (grade is high enough) 
    isSigned = 1;
  if (grade too low)
    throw Form::GradeTooLowException;
}
```


### Bureaucrat에 추가 내용

```
SignForm(Form)
{
  if (success)
     "<bureaucrat> signs <form>"
  else
    "<bureaucrat> cannot sign <form> because <reason>"
}
```

## ex02: 아니 28B form이 필요하지, 28C 말고..

- 아래 세 클래스 모두 생성자에 딱 하나의 매개변수(target of the form)만 사용함
- ex) home --> home에 shrubbery
- 모든 특징은 부모 클래스의 private에 있음
  
### ShrubberyCreationForm class

- signGrade(145)
- excuteGrade(137)
- 현재 디렉토리에 TARGET_shrubbery라는 파일을 만들고 그 안에 ASCII 트리를 적습니다.

### RobotomyRequestForm class

- signGrade(72)
- excuteGrade(45)
- 드릴 소음이 들리고 "Drrrrrr......."
- 50% 확률로 "TARGET has been robotomized successfully."
- 50% 확률로 "TARGET failed to be robotomize."

### PresidentialPardonForm class

- signGrade(25)
- excuteGrade(5)
- "TARGET has been pardoned by Zafod Beeblebrox."

### 기존 form 클래스에 추가

**execute(Bureaucrat const & executor) const** 추가
- execute form()
  - check wheather the form is signed
- 추상 클래스여야 쓸 수 있겠죠

### 기존 bureaucrat 클래스에 추가

**executeForm(Form const & form)**
- execute the form
- if (success) "<bureaucrat> executes <form>"
- else (explicit error message)
  
## ex03: 적어도 이건 커피 타는 것보단 낫지

### Intern class

- no name, no grade, no characteristics
- Form* makeForm(std::string formName, std::string targetForm)
  - 인자 1에 해당하는 구체 클래스이고, 인자 2로 초기화된 클래스를 가리키는 포인터를 리턴.
  - std::cout << "Intern creates <form>"
  - if, else if, else 같은 거 쓰지 말고 구현하세요.
  - 요구된 form이 unknown이면 "(some explicit error message)"

#### 용례

예를 들어 Bender를 타겟으로 하는 RobotomyRequestForm을 만들면 아래와 같다.

```cpp
{
  Intern someRandomIntern;
  Form* rrf;
  
  rrf = someRandomIntern.makeForm("robotomy request", "Bender");
}
```
