# miniRT
내 첫 레이 트레이서 with miniLibX

## 요약 :
- 이 프로젝트는 아름다운 Raytracing 세계에 대한 소개입니다.
- 완료되면 간단한 컴퓨터 생성 이미지를 렌더링 할 수 있으며
- 수학 공식을 사용하는 것을 다시는 두려워하지 않을 것 입니다.

## 참고자료
* [42 intra:  INTRODUCTION TO MINILIBX](https://elearning.intra.42.fr/notions/minilibx/subnotions/mlx-introduction/videos/introduction-to-minilibx)

* [nakim님의 가이드](https://42born2code.slack.com/archives/CU6MTFBNH/p1589845185409100)
![image](https://user-images.githubusercontent.com/53321189/82555785-671ab700-9ba3-11ea-9343-5dbc9a074f50.png)

~~~
https://raytracing.github.io/books/RayTracingInOneWeekend.html
위 사이트는 c++로 파트가 세개로 나뉘어져 있는데 클래스를 잘 옮 길 수 있으면 보너스 파트까지 할 수 있을 것 같습니다.
https://www.scratchapixel.com/
레이트레이싱에 대한 개념을 읽어 볼 수 있는 사이트 !!!
https://mrl.nyu.edu/~dzorin/cg05/lecture12.pdf
공식만 깔끔하게 나와있습니다. 그런데 음.. 이건 직접 코드로 변환하셔야 합니다!!
~~~



## 소개
컴퓨터로 만든 3차원 이미지를 렌더링 할 때는 2가지 접근법이 있다.
- Rasterization: 효율성 때문에 거의 모든 그래픽 엔진에서 사용된다.
- Ray Tracing
  - 1968년 처음 개발되었음.
  - 연산 비용이 래스터라이제이션보다 훨씬 비싸다.
  - 그래서 실시간 사용에는 잘 맞지 않음.
  - 훨씬 더 사실적인 비주얼을 만들 수 있음.
  
- 고퀄리티 그래픽 만들기를 시작하기 전에 당신은 기초를 마스터 해야한다 = miniRT 하세요
- miniRT는 C로 코딩 된 당신의 첫 번째 레이 트레이서이며, 표준이고, 하지만 기능적입니다.
- miniRT의 주된 목표는 당신이 수학자가 되지 않고도 수학공식이나 물리공식을 사용할수 있다는 것을 스스로에게 증명하는 것입니다.
- 우리는 아주 기본적인 레이 트레이싱의 특징들만 넣어두었으니 KEEP CALM, DONT PANIC.
- 이 프로젝트가 끝나면 학교에서 보내는 시간들을 정당화하기 위해 멋진 그림을 내보일 수 있습니다...
  
## 일반지침
평소와 같음..

## 필수파트
- all, clean, fclean, re, bonus
- a scene in format *.rt
- open, close, read, write, malloc, free, perror, strerror, exit
- All functions of the math library (-lm man man 3 math)
- All functions of the MinilibX
- **Description**
  - 목표는 레이 트레이싱을 사용해서 이미지를 생성하는 것입니다
  - 이 이미지들은 특정 앵글/포지션에서 간단한 기하학적 물체들과 조명 시스템에 의해 정의될 것입니다. 
- **제한조건**  
  - miniLibX 쓰기
  - The management of your window must remain smooth: changing to another window, minimizing, etc.
  - 최소한 평면, 구, 원통, 정사각형, 삼각형 5가지 간단한 기하학적 객체가 필요합니다.
  - 해당되는 경우, 모든 가능한 교차점과 물체 내부가 반드시 잘 처리되어 있어야 합니다.
### 구체적인 지시사항들..

- 개체의 고유 한 속성 크기를 조정할 수 있어야합니다: 구, 사각형의 측면 크기 및 원통의 너비 및 높이
- 객체, 빛, 카메라에 대해 translation, rotation 변형이 가능해야함. (구, 삼각형, 빛은 빼고. 이것들은 rotation 안됨)
- 빛 관리: 스팟 밝기, 하드 쉐도우, 주변광(물체들은 절대 완전 어둠 속에 있을 수 없음). 채색, 멀티 스팟 라이트가 반드시 잘 처리되어야 함.
- Deepthought가 눈이 생겨서 언젠가 너의 프로젝트를 평가할 때를 대비해서, 그리고 아름다운 배경화면이 되도록 렌더링하고 싶다면.. 렌더링 된 이미지를 bmp 포맷으로 저장. 두번째 아규먼트에 `--save`.

- 만약 두번쨰 아규먼트가 제공되지 않으면, 프로그램은 창에 이미지를 띄울 것이다. 아래의 규칙을 따라라.
  - ESC 누르기: 창을 닫고 프로그램을 완전히 종료.
  - 창 맨 위에 빨간 십자가 누르기: 창이 닫히고 프로그램을 깨끗하게 종료
  - 선언된 장면 크기가 디스플레이 해상도보다 큰 경우, 창 크기는 현재 디스플레이 해상도에 따라 설정됩니다.
  - 하나 이상의 카메라가 있는 경우 원하는 키보드 키를 눌러서 카메라를 전환할 수 있어야합니다.
  - minilibX에 있는 images의 사용을 적극 권장합니다.
  
- 당신의 프로그램은 첫번째 인자로 장면.rt 파일을 받아야 합니다.
  -  It will contain the window/rendered image size, which implies your miniRT must be able to render in any positive size.
  - Each type of element can be separated by one or more line break(s).
  - Each type of information from an element can be separated by one or more space(s).
  - Each type of element can be set in any order in the file.
  - Elements which are defined by a capital letter can only be declared once in the scene.
  - Each element first’s information is the type identifier (composed by one or two character(s)), followed by all specific information for each object in a strict order such as:

    
## 보너스 파트

## 예시

## 구현과정
[바로가기](miniRT풀이)
