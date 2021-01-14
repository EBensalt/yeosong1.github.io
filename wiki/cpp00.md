# CPP Module 00
네임스페이스, 클래스, 멤버 함수, stdio 스트림, 초기화 리스트, static, const, 그리고 많은 기본적인 것들.

## 제너럴 룰
## exercise 00: 메가폰
- 제출할 디렉토리: ex00/
- 제출할 파일: Makefile, megaphone.cpp
- 금지 함수: 없음

모두가 깨어있는 것을 확실히 하기 위해서
다음과 같은 동작을 하는 프로그램을 쓰세요

~~~
$>./megaphone "shhhhh... I think the students are asleep..."
SHHHHH... I THINK THE STUDENTS ARE ASLEEP...
$>./megaphone Damnit " ! " "Sorry students, I thought this thing was off."
DAMNIT ! SORRY STUDENTS, I THOUGHT THIS THING WAS OFF.
$>./megaphone
* LOUD AND UNBEARABLE FEEDBACK NOISE *
$>
~~~

- [ ] 대문자로 바꾸기
- [ ] if (argc == 1) 출력 * LOUD AND UNBEARABLE FEEDBACK NOISE *

## exercise 01: 내 어썸 폰북
- 제출할 디렉토리: ex01/
- 제출할 파일: Makefile, *.cpp, *.{h,hpp}
- 금지 함수: 없음

80년대와 놀라운 기술에 오신 것을 환영합니다!
`crappy`하고 멋진 전화번호부 소프트웨어처럼 동작하는 프로그램을 써라.
잠시 시간을 내어 실행 파일에 관련 이름을 지정하십시오.
프로그램이 시작되면 입력을 요구하는 메시지가 표시됩니다.
`ADD` 명령, `SEARCH` 명령 또는 `EXIT`를 수락해야 합니다. 다른 입력은 폐기됩니다.

프로그램이 비어 있거나(연락처 없음) 동적 할당을 사용하지 않으며 8개 이상의 연락처를 저장할 수 없습니다.
아홉 번째 연락처가 추가된 경우 관련 동작을 정의하십시오.

~~~
💡
http://www.cplusplus.com/reference/string/string/ 과 물론
http://www.cplusplus.com/reference/iomanip
~~~

• If the command is EXIT:
◦ The program quits and the contacts are lost forever.
• Else if the command is ADD:
◦ The program will prompt the user to input a new contact’s information, one
field at a time, until every field is accounted for.
◦ A contact is defined by the following fields: first name, last name, nickname,
login, postal address, email address, phone number, birthday date, favorite
meal, underwear color and darkest secret.
◦ A contact MUST be represented as an instance of a class in your code. You’re
free to design the class as you like, but the peer evaluation will check the
consistency of your choices. Go look at today’s videos again if you don’t
understand what I mean (and I don’t mean "use everything" before you ask).
• Else if the command is SEARCH:
◦ The program will display a list of the available non-empty contacts in 4
columns: index, first name, last name and nickname.
◦ Each column must be 10 chars wide, right aligned and separated by a ’|’
character. Any output longer than the columns’ width is truncated and the
last displayable character is replaced by a dot (’.’).
◦ Then the program will prompt again for the index of the desired entry and
displays the contact’s information, one field per line. If the input makes no
sense, define a relevant behavior.
• Else the input is discarded.
When a command has been correctly executed, the program waits for another ADD or
SEARCH command until an EXIT command.

## exercise 02: 꿈의 직장???
