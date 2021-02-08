# CPP module 04 서브젝트

Subtype polymorphism 서브타입 다형성(런타임 다형성), abstract classes 추상 클래스, interface 인터페이스.

## 제너럴 룰

## ex00  다형성, 또는 "When the sorcerer thinks you’d be cuter as a sheep" 마법사가 당신이 양만큼 귀여울거라고 생각했을 떄.


다형성은 고대의 전통으로 마법사 마법사 마법사들의 시대로 거슬러 올라갑니다. 우리가 먼저 생각했다고 생각하게 만들 수도 있지만 그건 거짓말입니다!

- [x] **Sorcerer 클래스**
  - **멤버 변수**:
    - name
    - title
  - **멤버 함수**:
    - `생성자(name, title) { cout << NAME, TITLE, is born! }`
      - 인자 없이는 생성되지 않도록 하기
      - 근데 코플리언 폼은 지켜야됨 =
   - `소멸자(){ cout << NAME, TITLE, is dead. Consequences will never be the same! }`
   - 마법사는 `<<` 연산자 오버로딩으로 어느 아웃풋 스트림에나 다음과 같이 자기 소개를 할 수 있어야 한다.
    - `I am NAME, TITLE, and I like ponies!`
    - friend 사용 금지
   - `void polymorph(Victim const &) const` 
 
 - [x] **Victim 클래스** :
  - **멤버 변수**:
  - **멤버 함수**:
    - `생성자(name){ cout << Some random victim called NAME just appeared! }`
    - `소멸자(){ cout << Victim NAME just died for no apparent reason! }`
    - 마찬가지로 자기소개: `I'm NAME and I like otters!`
    - `void getPolymorphed() const { NAME has been turned into a cute little sheep! } ` 
      - Sorcerer에 의해..
      
  - [x] **Peon 클래스** : peon은 Victim이니까 상속
    - **멤버 변수**:
    - **멤버 함수**:
      - `생성자(){ cout << "Zog zog."; }`
      - `소멸자(){ cout << "Bleuark..."; }` 
        - 아래 예제를 참고해서 잘..
     - `void getPolymorphed() const{ cout << "NAME has been turned into a pink pony!" << endl; }`
     
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

아래의 결과처럼 나와야함.

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
