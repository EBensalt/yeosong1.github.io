# CPP module 04 서브젝트

Subtype polymorphism 서브타입 다형성(런타임 다형성), abstract classes 추상 클래스, interface 인터페이스.

## 제너럴 룰

## ex00  다형성(변신마법), 또는 마법사가 당신이 양만큼 귀여울거라고 생각했을 때.


다형성은 고대의 전통으로 마법사 마법사 마법사들의 시대로 거슬러 올라갑니다. 우리가 먼저 생각했다고 생각하게 만들 수도 있지만 그건 거짓말입니다!

- [x] **Sorcerer 클래스** : 마법사
  - **멤버 변수**:
    - [x] name
    - [x] title
  - **멤버 함수**:
    - [x] `생성자(name, title) { cout << NAME, TITLE, is born! }`
      - [x] 인자 없이는 생성되지 않게 하기
      - [x] 근데 코플리언 폼은 지켜야 됨
   - [x] `소멸자(){ cout << NAME, TITLE, is dead. Consequences will never be the same! }`
   - [x] `<<` 연산자 오버로딩으로 어느 아웃풋 스트림에나 자기 소개 `I am NAME, TITLE, and I like ponies!`
    - friend 사용 금지
   - `void polymorph(Victim const &) const` 
 
 - [x] **Victim 클래스** : 피해자 1
  - **멤버 변수**:
  - **멤버 함수**:
    - [x] `생성자(name){ cout << Some random victim called NAME just appeared! }`
    - [x] `소멸자(){ cout << Victim NAME just died for no apparent reason! }`
    - [x] 마찬가지로 `<<` 연산자 오버로딩으로 자기소개: `I'm NAME and I like otters!`
    - `void getPolymorphed() const { NAME has been turned into a cute little sheep! } ` 
      - Sorcerer에 의해..
      
  - [x] **Peon 클래스** : 피해자 2 : Peon은 Victim이니까 상속
    - **멤버 변수**:
    - **멤버 함수**:
      - [x] `생성자(){ std::cout << "Zog zog."; }`
      - [x] `소멸자(){ std::cout << "Bleuark..."; }` 
        - 아래 예제를 참고해서 잘..
     - `void getPolymorphed() const{ cout << "NAME has been turned into a pink pony!" << endl; }`
     
  - [x] 메인
    - 파생된 클래스를 확인하는 내용 등 다양한 테스트를 포함할 것.
     
     
     
   아래의 코드는 반드시 컴파일 되어야하고,
   
   ~~~C++
   int main()
{
  Sorcerer robert("Robert", "the Magnificent");
  Victim jim("Jimmy");
  Peon joe("Joe");
  std::cout << robert << jim << joe;
  robert.polymorph(jim);
  robert.polymorph(joe);
  return 0;
}
~~~

아래의 결과처럼 나와야 합니다.

~~~C++
$> clang++ -W -Wall -Werror *.cpp
$> ./a.out | cat -e
Robert, the Magnificent, is born!$
Some random victim called Jimmy just appeared!$
Some random victim called Joe just appeared!$
Zog zog.$
I am Robert, the Magnificent, and I like ponies!$
I'm Jimmy and I like otters!$
I'm Joe and I like otters!$
Jimmy has been turned into a cute little sheep!$
Joe has been turned into a pink pony!$
Bleuark...$
Victim Joe just died for no apparent reason!$
Victim Jimmy just died for no apparent reason!$
Robert, the Magnificent, is dead. Consequences will never be the same!$
$>
~~~

## ex01 나는 세상을 불태우고 싶지 않아 - 무기 만들기

- 추상 클래스, 순수 함수
- 코플리언 폼 지켜주세요.

~~~C++
class AWeapon
{
  private:
          [...]
  public:
          AWeapon(std::string const & name, int apcost, int damage);
          [...] ~AWeapon();
          std::string [...] getName() const;
          int getAPCost() const;
          int getDamage() const;
          [...] void attack() const = 0;
};
~~~

- [x] **Awepon**
  - **멤버 변수**
    - name
    - damage (hit 당하면 손상되는)
    - apcost (사격하려면 드는 비용 - 액션 포인트)
  - **멤버 함수**
    - public:
    - AWeapon(std::string const & name, int apcost, int damage);
    - [...]  ~AWeapon();
    - std::string getName() const;
    - int getAPCost() const;
    - int getDmage() const;
    - [...] void attack() const = 0; (상속 받은 클래스들 별로 각각 1.무기 소리 2.빛 이펙트)
- [x] **PlasmaRifle** : public AWeapon
  - Name: "Plasma Rifle"
  - Damage: 21
  - AP cost: 5
  - Output of attack(): `* piouuu piouuu piouuu *`
- [x] **PowerFist** : public AWeapon
  - Name: "Power Fist"
  - Damage: 50
  - AP cost: 8
  - Output of attack(): `* pschhh... SBAM! *`
- [x] **Enemy**
  - **멤버 변수**
    - hit points
    - type
  - **멤버 함수**
    - public:
    - Enemy(int hp, std::string const & type);
    - [...] ~Enemy();
    - std::string [...] getType() const;
    - int getHP() const;
    - virtual void takeDamage(int);
      - 내용 : this->HP를 감소시킴
      -if (damage < 0) 암것도 안함
- [x] **SuperMutant** : public Enemy
  - HP: 170  
  - Type: "Super Mutant"  
  - On birth, displays: `Gaaah. Me want smash heads!`
  - Upon death, displays: `Aaargh...`
  - Overloads `takeDamage` to take 3 less damage points than normal
- [x] **RadScorpion** : public Enemy
  - HP: 80
  - Type: `RadScorpion`
  - On birth, displays: `* click click click *`
  - Upon death, displays: `* SPROTCH *`
- [x] **Character**
  - **멤버 변수**
    - name
    - AP(40) (무기 사용할때마다 잃고, 없으면 무기 못씀) if (AP > 40) AP = 40;
    - *weaponPtr (*AWeapon을 가리키며, 걔는 현재 무기임)
  - **멤버 함수**
    - Character(std::string const & name);
    - [...]
    - ~Character();
    - [x] void recoverAP(); (AP를 10 회복시킴)
    - [x] void equip(AWeapon*); weaponPtr = weapon; (복사는 안해요)
    - [x] void attack(Enemy*);
      - std::cout << "NAME attacks ENEMY_TYPE with a WEAPON_NAME\n";
      - enemy.HP -= weapon.damage
      - if (weaponPtr == 0) 장착 무기 없으면 암것도 안함
      - if (enemy.HP <= 0) delete enemy.
    - std::string [...] getName() const;
- **`<<` ostream 연산자 오버로딩** --- o << Character 하면
  - `o << "NAME has AP_NUMBER AP and wields a WEAPON_NAME\n";` 
  - if (weaponPtr == 0) `o << "NAME has AP_NUMBER AP and is unarmed\n";`
- 필요한 **getter 함수**를 모두 추가할 것

- **main**
 
```C++
void basic_test()
{
Character* me = new Character("me");
std::cout << *me;
Enemy* b = new RadScorpion();
AWeapon* pr = new PlasmaRifle();
AWeapon* pf = new PowerFist();
me->equip(pr);
std::cout << *me;
me->equip(pf);
me->attack(b);
std::cout << *me;
me->equip(pr);
std::cout << *me;
me->attack(b);
std::cout << *me;
me->attack(b);
std::cout << *me;
}
```

결과

```C++
$> clang++ -W -Wall -Werror *.cpp
$> ./a.out | cat -e
me has 40 AP and is unarmed$
* click click click *$
me has 40 AP and wields a Plasma Rifle$
me attacks RadScorpion with a Power Fist$
* pschhh... SBAM! *$
me has 32 AP and wields a Power Fist$
me has 32 AP and wields a Plasma Rifle$
me attacks RadScorpion with a Plasma Rifle$
* piouuu piouuu piouuu *$
me has 27 AP and wields a Plasma Rifle$
me attacks RadScorpion with a Plasma Rifle$
* piouuu piouuu piouuu *$
* SPROTCH *$
me has 22 AP and wields a Plasma Rifle$
```
## ex02: 이 코드는 깨끗하지 않아. 정화시켜!
인터페이스 사용 

### Squad 클래스

- [x] **ISquad**

```C++
class ISquad
{
  public:
        virtual ~ISquad() {}
        virtual int getCount() const = 0;
        virtual ISpaceMarine* getUnit(int) const = 0;
        virtual int push(ISpaceMarine*) = 0;
};
```
- [x] **Squad**

- [x] int getCount()
  - return (현재 스쿼드에 있는 유닛수)
- [x] getUnit(int N)
  - return (N번째(0부터 셈)에 있는 유닛을 가리키는 포인터 or 인덱스 범위를 벗어나면 널 포인터)
- [x] int push(XXX)
  - 스쿼드 맨끝에 XXX 유닛을 추가합니다
  - (null 유닛이나 이미 있는 유닛은 추가 불가)
  - return (스쿼드 유닛의 수)

스쿼드는 SpaceMarines의 컨테이너. 군대를 구조화하기 위한 것.

- 모든 유닛은 new로 만들어야 합니다.
- [x] 복사생성자
  - **딥카피** 하게 만드세요. `*this를 new로 복사하는 clone 함수 사용`
- [x] 연산자 =
  - assignation할 때 Squad에 유닛이 이미 있으면 반드시 파괴한 후 교체하세요.
- [x] 소멸자
  - Squad 파괴시, 안에 있는 유닛들도 순서대로 파괴되어야 합니다.

### TacticalMarine 클래스

- [x] **ISpaceMarine**

```C++
class ISpaceMarine
{
  public:
        virtual ~ISpaceMarine() {}
        virtual ISpaceMarine* clone() const = 0;
        virtual void battleCry() const = 0;
        virtual void rangedAttack() const = 0;
        virtual void meleeAttack() const = 0;
};
```

- [x] **TacticalMarine**

- [x] clone() { return (a copy of the current object) }
- [x] 생성자 "Tactical Marine ready for battle!"
- [x] battleCry() "For the holy PLOT!"
- [x] rangedAttack() "* attacks with a bolter *"
- [x] meleeAttack()  "* attacks with a chainsword *"
- [x] 소멸자 "Aaargh..."

- [x] **AssaultTerminator**

- [x] clone() { return (a copy of the current object) }
- [x] 생성자 "* teleports from space *"
- [x] battleCry() "This code is unclean. PURIFY IT!"
- [x] rangedAttack() "* does nothing *"
- [x] meleeAttack() "* attacks with chainfists *"
- [x] 소멸자 "I’ll be back..."


### main.cpp

```c++
int main()
{
  ISpaceMarine* bob = new TacticalMarine;
  ISpaceMarine* jim = new AssaultTerminator;
  ISquad* vlc = new Squad;
  vlc->push(bob);
  vlc->push(jim);
  for (int i = 0; i < vlc->getCount(); ++i)
  {
    ISpaceMarine* cur = vlc->getUnit(i);
    cur->battleCry();
    cur->rangedAttack();
    cur->meleeAttack();
  }
  delete vlc;
  return 0;
 }
```

```c++
$> clang++ -W -Wall -Werror *.cpp
$> ./a.out | cat -e
Tactical Marine ready for battle!$
* teleports from space *$
For the holy PLOT!$
* attacks with a bolter *$
* attacks with a chainsword *$
This code is unclean. PURIFY IT!$
* does nothing *$
* attacks with chainfists *$
Aaargh...$
I'll be back...$
```

## ex03: 보칼 판타지

### AMateria 클래스

```c++
class AMateria
{
  private:
          [...]
          unsigned int _xp;
  public:
          AMateria(std::string const & type);
          [...]
          [...] ~AMateria();
          
          std::string const & getType() const; //Returns the materia type
          unsigned int getXP() const; //Returns the Materia's XP
          
          virtual AMateria* clone() const = 0;
          virtual void use(ICharacter& target);
};
```

- xp
  - 0으로 초기화
  - use() 쓸 때마다 += 10
- type
  - ice
  - cure
- clone() 
  - 진짜 'Materia의 타입'의 새 인스턴스를 리턴.
- use(ICharacter&)
  - Ice: "* shoots an ice bolt at NAME *"
  - Cure: "* heals NAME’s wounds *"
  - NAME = Character.getName()
  
- 힌트 : Materia를 다른 Materia로 assigning 할 때 type을 복사하는 건 말이 안되죠..

### Character 클래스

- [x] ICharacter

```c++
class ICharacter
{
  public:
        virtual ~ICharacter() {}
        virtual std::string const & getName() const = 0;
        virtual void equip(AMateria* m) = 0;
        virtual void unequip(int idx) = 0;
        virtual void use(int idx, ICharacter& target) = 0;
};
```

- `AMateria *inventory[4]` 캐릭터는 최대 4개의 인벤토리를 가질 수 있음
  - 빈 인벤토리로 시작
  - 0부터 3 순서로 장착equip 할 수 있음
  - 인벤토리 꽉 찬 Materia를 equip(Materia)하면 아무것도 하지 말기
  - 존재하지 않는 Materia를 use(Materia), unequip(Materia) 시키면 아무것도 하지 말기
- unequip은 Materia를 delete하지 않습니다.
- use(int idx, ICharacter& target)
  - idx 슬롯 
  - target을 AMateria::use()에 넘김?
  
**주의: 캐릭터의 인벤토리에서 어떤 AMateria든 지원할 수 있어야 함.**

- 생성자 (name)
- 복사생성자 반드시 딥카피
- 연산자= 반드시 딥카피
- 캐릭터의 old Materia는 반드시 delete
- 소멸자도 마찬가지

### MateriaSource

- [x] IMateriaSource

```c++
class IMateriaSource
{
  public:
        virtual ~IMateriaSource() {}
        virtual void learnMateria(AMateria*) = 0;
        virtual AMateria* createMateria(std::string const & type) = 0;
};
```
- [x] learnMateria(AMateria*)
  - 인자를 복사, 클론될 메모리에 저장
  - 캐릭터 클래스처럼 4개까지 가능하고, 꼭 유니크할 필요는 없음.
- [x] createMateria(std::string const &) 
  - 리턴 new Materia (= 앞에 소스에서 배워서 복사된 Materia이고, 인자와 type 같음)
  - type이 unknown이면 리턴 0

- 한 마디로, 소스는 반드시 Materia 템플릿을 learn할 수 있어야 하고
- 필요에 따라 재생산할 수 있어야 한다.
- 진짜 실제 type을 모르고도 만들 수 있어야 한다. 그냥 문자열로 확인해서?

### main

```C++
int main()
{
IMateriaSource* src = new MateriaSource();
src->learnMateria(new Ice());
src->learnMateria(new Cure());
ICharacter* me = new Character("me");
AMateria* tmp;
tmp = src->createMateria("ice");
me->equip(tmp);
tmp = src->createMateria("cure");
me->equip(tmp);
ICharacter* bob = new Character("bob");
me->use(0, *bob);
me->use(1, *bob);
delete bob;
delete me;
delete src;
return 0;
}
```

```c++
$> clang++ -W -Wall -Werror *.cpp
$> ./a.out | cat -e
* shoots an ice bolt at bob *$
* heals bob's wounds *$
```
