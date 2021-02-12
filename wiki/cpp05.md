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



