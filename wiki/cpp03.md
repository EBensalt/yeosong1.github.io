
# CPP Module 03
상속 inheritance

## 제너럴 룰
<details>
<summary> <b> 제너럴 룰 보기 </b>  </summary><br>
<div markdown="1">
  
- 헤더 안에 구현된 모든 기능(템플릿의 경우는 제외) 및 보호되지 않은 헤더는 exercise 0점을 의미합니다.
- 모든 **출력은 표준 출력으로** 하며, 특별히 지정하지 않는 한 **개행(\n)으로 끝납니다.**
- 부과된 파일 이름 뒤에는 letter, 클래스 이름, 함수 이름, 메서드 이름이 와야합니다.
- 기억하십시오: 이제 더 이상 C가 아닌 C++로 코딩하고 있습니다. 따라서:
  - 다음 기능은 **금지**되어 있으며 사용시 0점 처리를 받습니다. 묻지도 따지지도 마시오: ***alloc, *printf, free*
  - 기본적으로 표준 라이브러리의 모든 것을 사용할 수 있습니다. **그러나** C++ 버전의 함수를 사용하는 것이 현명할 것입니다.
    당신은 C에 익숙합니다. 당신이 아는 것을 유지하는 대신, C++ 버전의 함수를 사용하는 것이 현명할 것입니다. 결국 이건 새로운 언어입니다.
  - 그리고 **네, 안돼요.** 써도 될 때까지는 [STL](https://www.cplusplus.com/reference/stl/)을 사용할 수 없습니다.(즉, 모듈08 전에는 안됨).
  - 이는 include <algorithm>을 필요로 하는 모든 것은--벡터/리스트/맵/등등--다 안된다는 뜻입니다.
- 명시적으로 금지된 기능 또는 기계의 사용은 묻지도 따지지도 않고 0점 처리됩니다.
- 또한, 달리 명시되지 않는 한 C++ 키워드 **using namespace**및 **friend**는 금지되어 있습니다.
  - 그들의 사용은 질문없이 **-42점**으로 처리 될 것입니다.
- 클래스와 관련된 파일은 달리 명시되지 않는 한 항상 **ClassName.hpp** 및 **ClassName.cpp**입니다.
- Turn-in 디렉토리는 **ex00/**, **ex01/**, ... , **exn/**.
- 예제를 철저하게 읽어야합니다. exercise의 설명에서는 명확하지 않았던 요구 사항을 포함하고 있을 수 있습니다.
  만약 뭔가 모호해 보인다면, 당신이 **C++**를 충분히 이해하지 못한 것입니다.
- 앞에서부터 배운 **C++** 도구는 사용할 수 있으므로, external 라이브러리는 사용할 수 없습니다. 그리고 물어보기 전에 말해드려요:
  - 그것은 또한 **C++11과 파생 모델**, **Boost** 또는 C++ 없으면 못사는 놀라운 기술을 갖춘 친구가 알려준 그 어떤 것도 안된다는 뜻입니다.
  - = 범위 기반 for문 C++11이니까 사용 금지..
- 상당한 양의 클래스들을 제출해야 할 수도 있습니다. 이것은 좋아하는 텍스트 편집기를 스크립팅할 수 없다면 지루해 보일 수 있습니다.
- 시작하기 전에 각 exercise를 **완전히** 읽으십시오! 진짜로요, 읽으세요.
- 사용할 컴파일러는 **clang++**입니다.
- 코드는 다음 플래그를 사용하여 컴파일해야합니다: **-Wall -Wextra -Werror**
- 당신의 각 includes는 다른 includes들과 독립적으로 포함될 수 있어야 합니다. Includes는 분명히 그들이 의존하는 다른 모든 include를 포함해야합니다.
- 궁금할까봐: **C++에서는 코딩 스타일이 적용되지 않습니다.** 원하는 스타일 아무거나 사용 가능, 제한 없음. **하지만, 동료 평가자가 읽을 수 없는 코드는 채점 받을 수 없겠죠**
- 이제 중요한 사항 : 서브젝트에 명시적으로 설명하지 않는 한 **프로그램에 의해 채점되지 않습니다**. 따라서, 여러분은 exercise를 선택하는 방법에 있어서 어느 정도의 자유가 주어집니다. 하지만, 각 exercise의 제한조건에 유의하고, **게으르지 마세요**, 연습문제들이 제공해야되는 **많은 것들을 놓치게 될거예요!**
- **제출하는 파일에 일부 관계없는 파일이 있는 것은 문제가 되지 않습니다.** 요청한 파일보다 더 많은 파일로 코드를 분리할 수도 있습니다.
  결과가 프로그램에 의해 채점되지 않는 한, **자유롭게 하세요.**
- 비록 서브젝트의 exercise가 짧더라도, 알아야 할 것을 확실히 이해하고, 가능한 최선의 방법으로 풀었다는 것을 확실히 하기 위해 시간을 들이는 것은 가치가 있습니다.
- 오딘의 이름으로, 토르의 이름으로! 머리를 쓰세요!!!
  
 </div> 
 </details>
 <BR>

### exercise 00: Aaaaand... OPEN!
- 제출할 디렉토리 : ex00/
- 제출할 파일 : FragTrap.cpp FragTrap.hpp main.cpp
- 금지 함수: 없음

여기서는 FR4G-TP 공격 로봇/슈박스를 모델링하는 클래스를 만들어야 합니다.

- [x] **FragTrap 클래스** :
  - **멤버 변수**: 다음의 지정된 값으로 초기화
    - Name (Parameter of constructor)
    - Hit points (100)
    - Max hit points (100)
    - Energy points (100)
    - Max energy points (100)
    - Level (1)
    - Melee attack damage (30)
    - Ranged attack damage (20)
    - Armor damage reduction (5)
  - **멤버 함수**: 작동한 내용을 모두 설명해서 출력할 것
    - rangedAttack(std::string const & target)
        - 예) `FR4G-TP <name> attacks <target> at range, causing <damage> points of damage!`
    - meleeAttack(std::string const & target)
    - takeDamage(unsigned int amount)
    - beRepaired(unsigned int amount)
    - 생성자
      - `Create Player <name>`!
    - 소멸자
      - `<name> GAME OVER`
    - 복사생성자(메시지 출력 없음)
    - 대입연산자 오버로딩(메시지 출력 없음)
  
- [ ] 보너스 포인트: 이러한 메시지가 재미있는 레퍼런스를 가진 경우
  - (FR4G-TP가 무엇인지 모른다면, 구글을 통해 최소한 몇 개의 잘 선택된 인용구를 사용하시길..) -> 보너스는 포기할래! `^0^`
- [x] `vaulthunter_dot_exe(std::string const & target)` 함수를 추가하여 종료합니다. 
  - [x] 반랜덤 공격을 가할 수 있습니다. 매번 호출될 때마다 그렇게 만드십시오.
  - [x] 최소한 5개의 함수 풀에서 무작위로 선택된 (이왕이면)재미있는 공격을 힌다.
  - [x] 이 기능 실행에 에너지 포인트 25 필요
  - [x] 에너지 포인트가 충분하지 않으면, 에너지가 다 떨어졌음을 알려주는 메시지만 인쇄
- [x] **main** 코드가 잘 기능하는지 입증할 수 있는 충분한 테스트를 담은 메인을 제공하세요.

몇 가지 제약 조건:
- [x] 항상 `Hit points <= Max hit points`
- [x] 항상 `Energy point <= Max energy points`
- [x] if (repaired_HP + HP > Max_HP) HP = Max_HP
- [x] if (HP < 0) HP = 0;
- [x] takeDamage를 당하면 Armor damage reduction이 계정에 적용되어야 합니다.

당신이 이것을 성취하기 위해 사용하기를 원하는 것은 무엇이든 좋습니다, 그러나 늘 그렇듯이, 우아한 방법일수록 좋습니다. 

### exercise 01: 세레나, 내 사랑!
- 제출: ex02 / 이전거 + ClapTrap.cpp ClapTrap.hpp

> Claptrap은 곧 악의 은신처가 될 문을 관리하고, 들어오고 싶은 사람들의 도전을 받습니다.

- **ScavTrap 클래스**
  - **멤버 변수**: 다음의 지정된 값으로 초기화
    - Name (Parameter of constructor)
    - Hit points (100)
    - Max hit points (100)
    - Energy points (50)
    - Max energy points (50)
    - Level (1)
    - Melee attack damage (20)
    - Ranged attack damage (15)
    - Armor damage reduction (3)
  - **멤버 함수**: Fragtrap와 같고, 생성자/소멸자/attack은 아웃풋이 달라야함.
  - ScavTrap의 다른 점은 한 가지: vaulthunter_dot_exe()이 없고 **challengeNewcomer()**가 있음.
    - **challengeNewcomer()** : ScavTrap이 '다양한 도전 세트'에서 랜덤으로 도전을 선택할 수 있게 합니다.
    - 도전들은 재밌으면 좋고.. 당신이 발명하고 표준 출력에 인쇄해야합니다.
- 메인: 두 클래스 모두를 테스트 할 수 있게 확장.  


### exercise 02: 반복적인 작업
아마 Claptrap들을 만드는게 거슬리기 시작했을 것입니다.
<br>음, 덜 일해도 되기 전에 일을 좀 더 해야합니다.
<br>이제 FragTrap과 ScavTrap이 상속받을 

- [x] **ClapTrap 클래스**를 만듭니다. FragTrap과 ScavTrap의 부모 클래스
  - 모든 공통함수는 부모인 ClapTrap클래스에 배치
  - ClapTrap에는 자체 생성/소멸 메시지가 있어야함
- [x] Frap, Scav클래스에 둘 다 공유되지 않는 항목만을 포함

- [x] main: 적절한 생성/소멸 체인을 만들고 보여줘야함
  - 예) FragTrap을 빌드하면 ClapTrap 빌드가 먼저 나오고, 소멸은 역순.

## exercise 03: 이제 더 쉬워!

- **NinjaTrap 클래스**: 앞에서 했던 모든 걸 써서 만들기
  - **멤버 변수**: 
    - Hit points (60)
    - Max hit points (60)
    - Energy points (120)
    - Max energy points (120)
    - Level (1)
    - Name (Parameter of constructor)
    - Melee attack damage (60)
    - Ranged attack damage (5)
    - Armor damage reduction (0)
- [x] **ninjaShoebox()** : 특별 공격
  1. 같은 시그니처로
    - 시그니쳐란? (출처 불확실)
      - 함수를 구분하기 위한 구성요소로, 매개변수 리스트와 반환 타입을 말함.
  2. 다른 ClapTrap 레퍼런스를 인자로(NinjaTrap 포함) 받고
  3. 각기 다른 작동을 하는 여러개의 함수임
  
아무 ClapTrap이나 받는데 동작은 특정하게 하는 방법이 없는게 아쉽다... <br>
뭐, 내일 보게 될거야. 그게 뭔지 정확히는 모르지만, 뭔가 재미있는 일을 하게 만드세요.

- 메인: 평소처럼

부모 클래스가 있으니까 이제 새 ClapTrap 만들기가 얼마나 쉬운지 알겠어?
<br>
내용 이따위야~!~!!!! 

### exercise 04: 궁극의 공격 슈박스

- [x] **SuperTrap 클래스** = FragTrap 반 NinjaTrap 반 양쪽의 장점을 상속 받은 ClapTrap이다. 
  - **멤버 변수**: 
    - Hit points (Fragtrap)
    - Max hit points (Fragtrap)
    - Energy points (Ninjatrap)
    - Max energy points (Ninjatrap)
    - Level (1)
    - Name (Parameter of constructor)
    - Melee attack damage (Ninjatrap)
    - Ranged attack damage (Fragtrap)
    - Armor damage reduction (Fragtrap)
    - rangedAttack (Fragtrap)
    - meleeAttack (Ninjatrap)
  - 양쪽의 special attack을 가집니다. 
- 메인으로 테스트

물론 SuperTrap의 ClapTrap 부분는 딱 한 번만 만들어져야 합니다... 네, 여기엔 트릭이 있습니다. 다시 읽어보세요.
