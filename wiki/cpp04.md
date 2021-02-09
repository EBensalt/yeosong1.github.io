# CPP module 04 서브젝트

Subtype polymorphism 서브타입 다형성(런타임 다형성), abstract classes 추상 클래스, interface 인터페이스.

## 제너럴 룰

## ex00  다형성, 또는 "When the sorcerer thinks you’d be cuter as a sheep" 마법사가 당신이 양만큼 귀여울거라고 생각했을 때.


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
코플리언 폼 지켜주세요.

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

- **Awepon**
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
- **PlasmaRifle** : public AWeapon
  - Name: "Plasma Rifle"
  - Damage: 21
  - AP cost: 5
  - Output of attack(): "* piouuu piouuu piouuu *"
- **PowerFist** : public AWeapon
  - Name: "Power Fist"
  - Damage: 50
  - AP cost: 8
  - Output of attack(): "* pschhh... SBAM! *"
- **Enemy**
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
- **SuperMutant** : public Enemy
  - HP: 170  
  - Type: "Super Mutant"  
  - On birth, displays: "Gaaah. Me want smash heads!"
  - Upon death, displays: "Aaargh..."
  - Overloads takeDamage to take 3 less damage points than normal (Yeah, they’re kinda strong, these guys.)
- **RadScorpion** : public Enemy
  - HP: 80
  - Type: "RadScorpion"
  - On birth, displays: "* click click click *"
  - Upon death, displays: "* SPROTCH *"
- **Character**
  - **멤버 변수**
    - name
    - AP(40) (무기 사용할때마다 잃고, 없으면 무기 못씀) if (AP > 40) AP = 40;
    - *weaponPtr (*AWeapon을 가리키며, 걔는 현재 무기임)
  - **멤버 함수**
    - Character(std::string const & name);
    - [...]
    - ~Character();
    - void recoverAP(); (AP를 10 회복시킴)
    - void equip(AWeapon*); weaponPtr = weapon; (복사는 안해요)
    - void attack(Enemy*);
      - std::cout << "NAME attacks ENEMY_TYPE with a WEAPON_NAME\n";
      - enemy.HP -= weapon.damage
      - if (weaponPtr == 0) 장착 무기 없으면 암것도 안함
      - if (enemy.HP <= 0) delete enemy.
    - std::string [...] getName() const;
- **`<<`ostream 연산자 오버로딩** --- o << Character 하면
  - `o << "NAME has AP_NUMBER AP and wields a WEAPON_NAME\n";` 
  - if (weaponPtr == 0) `o << "NAME has AP_NUMBER AP and is unarmed\n";`
- 필요한 **getter 함수**를 모두 추가할 것 -> 게터 함수란? [아래 액세스 함수](#액세스-함수) 설명 참고!

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


## 액세스 함수
- private 멤버 변수의 값을 리턴하거나, (= getter 함수) ex) getName()
- private 멤버 변수의 값을 설정할 수 있는 함수이다 (= setter 함수) setName(...)
