#  CPP Module 05 서브젝트

반복과 예외처리. try catch throw

## ex00: 엄마 나 크면 공무원이 될래

예외 클래스들은 코플리언 폼 안맞춰도 됩니다.
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

## ex01 
