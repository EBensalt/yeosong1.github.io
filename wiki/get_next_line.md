## Tester
listed in 2020.<br>
[https://github.com/charMstr/GNL_lover](https://github.com/charMstr/GNL_lover)
<br>[https://github.com/DontBreakAlex/gnlkiller](https://github.com/DontBreakAlex/gnlkiller)
<br>[https://github.com/Hellio404/Get_Next_Line_Tester](https://github.com/Hellio404/Get_Next_Line_Tester)
<br>[https://github.com/mrjvs/42cursus_gnl_tests](https://github.com/mrjvs/42cursus_gnl_tests)
<br>[https://github.com/Mazoise/42TESTERS-GNL](https://github.com/Mazoise/42TESTERS-GNL)


## 목표
* 이 프로젝트를 통해 당신의 컬렉션에 매우 편리한 함수를 추가 할 수 있을뿐만 아니라<br>
C 프로그래밍에서 매우 흥미로운 새 개념인 [Static Variables](정적-변수)를 배울 수 있습니다.

## 일반 지침
* [Norm](https://meta.intra.42.fr/articles/norm-norminette-b1b74c82-5ba1-4e43-b02e-0101727e661c) 지키세요. 만약에 보너스에 Norm 에러 있으면 0점
* undefined behaviors와 별개로, 갑자기 프로그램 종료되면 안됩니다. (segmentation fault, bus error, double free 등)
만약에 그러면 0점.
* 힙에 할당한 모든 메모리는 필요할 때에 다 적절하게 free되어야 합니다. 누수는 용납되지 않습니다.
* 만약에 서브젝트가 요구한다면, Makefile은 컴파일 플래그 -Wall -Wextra -Werror 쓰고, 리링크 없게 해서 내야한다.
* 니 Makefile은 꼭 최소한 $(NAME), all, clean, fclean, re 에 대한 룰을 넣어야 한다.
* 프로젝트에 보너스를 제출하려면 Makefile에 룰 bonus를 포함시켜야 합니다.이 보너스는 프로젝트의 주요 부분에서 금지된 다양한 헤더, 라이브러리 또는 기능을 모두 추가합니다. 보너스는 다른 파일 _bonus. {c / h}에 있어야합니다. 필수 및 보너스 파트 평가는 별도로 수행됩니다.
* 프로젝트에서 libft를 사용할 수있는 경우 해당 소스 및 관련 Makefile을 관련 Makefile과 함께 libft 폴더에 복사해야합니다. 니 프로젝트의 Makefile은 프로젝트의 Makefile을 사용해서 라이브러리를 컴파일 하고, 그 다음 프로젝트를 컴파일해야합니다.
* 이 작품을 제출하거나 채점하지 않아도 프로젝트를위한 테스트 프로그램을 만드는 것이 좋습니다. 작업과 동료의 작업을 쉽게 테스트 할 수있는 기회를 제공합니다. 이러한 테스트는 방어 중에 특히 유용합니다. 실제로, 방어하는 동안 귀하는 귀하의 테스트 및 / 또는 귀하가 평가하는 동료의 테스트를 자유롭게 사용할 수 있습니다.
* 작업을 지정된 깃 저장소에 제출하십시오. 깃 리포지토리 안에 있는 작업만 채점됩니다. 만약 Deepthought가 작업의 등급을 매기도록 지정되면 그것은 동료 평가 후에 수행됩니다. Deepthought의 채점 중에 작업의 어떤 부분에서 오류가 발생하면 평가가 중지됩니다.


## 필수 파트
|||
|:---|:---|
| Function name | get_next_line |
| Prototype | int get_next_line(int fd, char **line); |
| Turn in files | get_next_line.c, get_next_line_utils.c,<br>get_next_line.h |
| Parameters | #1.  file descriptor for reading<br>#2.  The value of what has been read |
| Return value | 1 :  A line has been read<br>0 :  EOF has been reached<br>-1 :  An error happened |
| External functs. | read, malloc, free |
| Description | Write a function which returns a line read from a<br>file descriptor, without the newline. |

* Calling your function get_next_line in a loop will then allow you to read the text available on a file descriptor one line at a time until the EOF.
* Make sure that your function behaves well when it reads from a file and when it reads from the standard input.
* libft is not allowed for this project. You must add a get_next_line_utils.c file which will contain the functions that are needed for your get_next_line to work.
* Your program must compile with the flag -D BUFFER_SIZE=xx. which will be used as the buffer size for the read calls in your get_next_line. This value will be modified by your evaluators and by moulinette.
* Compilation will be done this way : gcc -Wall -Wextra -Werror -D BUFFER_SIZE=32 get_next_line.c get_next_line_utils.c
* Your read must use the BUFFER_SIZE defined during compilation to read from a file or from stdin.
* In the header file get_next_line.h you must have at least the prototype of the function get_next_line.

~~~
💡
Does your function still work if the BUFFER_SIZE value is 9999?
And if the BUFFER_SIZE value is 1?  And 10000000?  Do you know why?
~~~
**👆[스택 오버 플로우 피하기](https://github.com/yeosong-00/42/wiki/%EC%8A%A4%ED%83%9D-%EC%98%A4%EB%B2%84%ED%94%8C%EB%A1%9C%EC%9A%B0-%ED%94%BC%ED%95%98%EA%B8%B0)**

~~~
💡
You should try to read as little as possible each time get_next_line
is called.  If you encounter a newline, you have to return the
current line.  Don’t read the whole file and then process each line.
~~~
**👆ft_strchr(읽은 것, '\n')을 사용하였다.**

~~~
💡
Don’t turn in your project without testing. There are many tests to
run, cover your bases.  Try to read from a file, from a redirection,
from standard input.  How does your program behave when you send a
newline to the standard output?  And CTRL-D?
~~~
**👆[Go to Testers](https://github.com/yeosong-00/42/wiki/get_next_line#tester)**
<br><br>
* We consider that get_next_line has an undefined behavior if, between two calls, the same file descriptor switches to a different file before EOF has been reached on the first fd.
* lseek is not an allowed function. File reading must be done only once.
* Finally we consider that get_next_line has an undefined behavior when reading from a binary file. However, if you wish, you can make this behavior coherent.
* Global variables are forbidden.

<br><br>
💡 A good start would be to know what a static variable is: https://en.wikipedia.org/wiki/Static_variable


## 보너스 파트
The project get_next_line is straightforward and leaves very little room for bonuses, but I am sure that you have a lot of imagination. If you aced perfectly the mandatory part, then by all means complete this bonus part to go further. I repeat, no bonus will be taken into consideration if the mandatory part isn’t perfect.
Turn-in all 3 initial files with _bonus for this part.
* [Static Variables](https://github.com/yeosong-00/42/wiki/%EC%A0%95%EC%A0%81-%EB%B3%80%EC%88%98) 1개로 get_next_line을 성공시킵니다.
* To be able to manage multiple [file descriptor](file_descriptor) with your get_next_line. For ex- ample, if the file descriptors 3, 4 and 5 are accessible for reading, then you can call get_next_line once on 3, once on 4, once again on 3 then once on 5 etc. without losing the reading thread on each of the descriptors.
