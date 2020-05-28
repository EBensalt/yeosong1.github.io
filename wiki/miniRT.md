# miniRT 서브젝트 
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
- miniRT는 C로 코딩 된 당신의 첫 번째 레이 트레이서이며, 표준이고, 소박합니다. 하지만 기능적입니다.
- miniRT의 주된 목표는 당신이 수학자가 되지 않고도 수학공식이나 물리공식을 사용할수 있다는 것을 스스로에게 증명하는 것입니다.
- 우리는 아주 기본적인 레이 트레이싱의 특징들만 넣어두었으니 KEEP CALM, DONT PANIC.
- 이 프로젝트가 끝나면 학교에서 보내는 시간들을 정당화하기 위해 멋진 그림을 내보일 수 있습니다...
  
## 일반지침
평소와 같음..

## 필수파트
- Makefile에 all, clean, fclean, re, bonus 룰 넣기
- 장면은 *.rt 포맷 인자로 받음.
- 허용 함수: open, close, read, write, malloc, free, perror, strerror, exit
- 허용 함수: math library에 있는 모든 함수 (-lm man man 3 math)
- 허용 함수: MinilibX 에 있는 모든 함수
- **Description**
  - 목표는 레이 트레이싱을 사용해서 이미지를 생성하는 것입니다
  - 이 이미지들은 특정 앵글/포지션에서 간단한 기하학적 물체들과 조명 시스템에 의해 정의될 것입니다. 
- **💥 제한조건**  
  - miniLibX 쓰기
  - The management of your window must remain smooth: changing to another window, minimizing, etc.
  - 최소한 평면, 구, 원통, 정사각형, 삼각형 5가지 간단한 기하학적 객체가 필요합니다.
  - 해당되는 경우, 모든 가능한 교차점과 물체 내부가 반드시 잘 처리되어 있어야 합니다.
  
### 구체적인 지시사항들..

- 개체의 고유 한 속성 크기를 조정할 수 있어야합니다: 구, 사각형의 측면 크기 및 원통의 너비 및 높이
- 객체, 빛, 카메라에 대해 translation, rotation 변형이 가능해야함. (구, 삼각형, 빛은 빼고. 이것들은 rotation 안됨)
- 빛 관리: 스팟 밝기, 하드 쉐도우, 주변광(물체들은 절대 완전 어둠 속에 있을 수 없음). 채색, 멀티 스팟 라이트가 반드시 잘 처리되어야 함.
- Deepthought가 눈이 생겨서 언젠가 너의 프로젝트를 평가할 때를 대비해서, 그리고 아름다운 배경화면이 되도록 렌더링하고 싶다면.. 렌더링 된 이미지를 bmp 포맷으로 저장. 두번째 아규먼트에 `--save`.

- 만약 두번째 아규먼트가 제공되지 않으면, 프로그램은 창에 이미지를 띄울 것이다. 아래의 규칙을 따라라.
  [ ] ESC 누르기: 창을 닫고 프로그램을 완전히 종료.
  [ ] 창 맨 위에 빨간 십자가 누르기: 창을 닫고 프로그램을 완전히 종료
  [ ] 선언된 장면 크기가 디스플레이 해상도보다 크면: 창 크기는 디스플레이 해상도를 따라 설정.
  [ ] 하나 이상의 카메라가 있는 경우 (내가 정한)키보드 키를 눌러서 카메라를 전환할 수 있어야합니다.
  [ ] minilibX에 있는 images의 사용을 적극 권장합니다. (??? 모르겠는데..)

- 당신의 프로그램은 첫번째 인자로 장면.rt 파일을 받아야 합니다.
  - [ ] .rt에는 창과 렌더링 된 이미지의 size가 포함됩니다. miniRT가 아무 양의 정수인 size로 렌더링 될 수 있어야 함을 의미합니다.
  - [ ] 각 타입은 1개 이상의 \n(뉴라인)으로 구분할 수 있습니다.
  - [ ] 각 정보 타입은 하나 이상의 ' '(공백)으로 분리 할 수 있습니다.
  - [ ] 각 요소의 타입은 파일 내에서 임의의 순서로 설정할 수 있습니다.
  - [ ] 대문자로 정의 된 요소는 장면에서 한 번만 선언 할 수 있습니다. 
  - [ ] 각 요소의 첫 번째 정보는 type identifier(1 ~ 2 개의 문자로 구성됨)와 그에 따르는 각 개체에 대한 모든 구체적인 정보가 순서에 맞게 적혀있습니다. 예시 : ...
  아래는 유형 식별자(R,A,c,l,sp,pl,sq,cy,tr)와 그에 따라 각 함수에서 파싱되어야 할 요소들이다.
    - R 해상도
      - [ ] x 
      - [ ] y
    - A 주변광
      - [ ] 밝기 [0.0 ~ 1.0]
      - [ ] 색 R,G,B [0 ~ 255]
    - c 카메라
      - [ ] 시각 view point x,y,z
      - [ ] 방향..? 3d normalized orientation vector [-1 ~ 1] x,y,z
      - [ ] 화각 FOV [0 ~ 180]
    - l 스포트라이트
      - [ ] 좌표 x,y,z
      - [ ] 밝기 [0.0 ~ 1.0]
      - [ ] 색 R,G,B [0 ~ 255]
    - sp 구
      - [ ] 구의 중심점 좌표 x,y,z
      - [ ] 지름
      - [ ] 색 R,G,B [0 ~ 255]
    - pl 평면
      - [ ] 좌표 x,y,z
      - [ ] 방향..? 3d normalized orientation vector [-1 ~ 1] x,y,z
      - [ ] 색 R,G,B [0 ~ 255]
    - sq 사각형
      - [ ] 사각형의 중심점 좌표 x,y,z
      - [ ] 방향..? 3d normalized orientation vector [-1 ~ 1] x,y,z
      - [ ] 사이드 사이즈?? 
      - [ ] 색 R,G,B [0 ~ 255]
    - cy 원기둥
      - [ ] 좌표 x,y,z
      - [ ] 방향..? 3d normalized orientation vector [-1 ~ 1] x,y,z
      - [ ] 원기둥 지름
      - [ ] 원기둥 높이
      - [ ] 색 R,G,B [0 ~ 255]
    - tr 삼각형
      - [ ] 꼭지점 1의 좌표 x,y,z
      - [ ] 꼭지점 2의 좌표 x,y,z
      - [ ] 꼭지점 3의 좌표 x,y,z
      - [ ] 색 R,G,B [0 ~ 255]
   
   
- [ ] 필수파트 예시 with a minimalist .rt scene
<img width="560" alt="스크린샷 2020-05-29 오전 1 53 48" src="https://user-images.githubusercontent.com/53321189/83170296-47f5c980-a14f-11ea-8d27-a642e9c07925.png">

- [ ] 파일에 어떤 종류의 구성 오류가 발생하면 <프로그램 종료 + "Error\n" + 선택해 넣은 명시적인 오류 메시지>를 표시해야합니다.
- [ ] 디펜스 대비: 생성할 요소들을 쉽게 제어하기 위해, 기능적인 것에 중점을 둔 전체 장면을 갖는 편이 이상적입니다.

    
## 보너스 파트
물론 Ray-Tracing 기술은 많은 것들을 제어할 수 있습니다. 반사, 투명성, 굴절, 더 복잡한 개체, 부드러운 그림자, caustics(물, 유리등에 투과된 빛), 전역 조명(global illumination), 범프 매핑, .obj 파일 렌더링 등등 같은..

그러나 miniRT 프로젝트에서 우리는 간단한 선을 유지하고 싶었어. 이건 너의 첫 번째 레이 트레이서와 와 CGI에서의 첫 스텝이니까.

그래서 여기 니가 구현할 수 있는 몇 가지 간단한 보너스 리스트를 줄게. 더 큰 보너스를 원한다면 코드를 다시 쓰고 새로운 레이 트레이서를 만드는 걸 강력 추천할게. (이 작은 이번 프로젝트를 완전히 잘 작동하도록 완성하고나서, 그 이후에 너의 개발자 인생에서 말이지..)

~~~
지구 이미지..
~~~
- [ ] 스팟 1, a space skybox and a shiny 지구 텍스쳐의 구 with 범프 매핑.

~~~
💥 필수 파트가 말그대로 "완벽"해야지 보너스 부분 점수를 받는 거고, 그게 아니면 완전 "무시"할 것임.
~~~

* 보너스 리스트
  - [ ] Normal disruption e.g. using sine which gives a wave effect.
  - [ ] Color disruption: checkerboard.
  - [ ] Color disruption: rainbow effect using object’s normal.
  - [ ] Parallel light following a precise direction.
  - [ ] Compound element: Cube (6 squares).
  - [ ] Compound element: Pyramid (4 triangles, 1 square).
  - [ ] Putting caps on size-limited cylinders.
  - [ ] One other 2nd degree object: Cone, Hyperboloid, Paraboloid..
  - [ ] One color filter: Sepia, R/G/B filters..
  - [ ] Anti-aliasing.
  - [ ] Simple stereoscopy (like red/green glasses).
  - [ ] Multithreaded rendering.
  - [ ] Sphere texturing: uv mapping.
  - [ ] Handle bump map textures.
  - [ ] A beautiful skybox.
  - [ ] Keyboard interactivity (translation/rotation) with camera.
  - [ ] Keyboard interactivity (translation/rotation) with objects.
  - [ ] Changing the camera rotation with the mouse.
  
~~~
i: 보너스 기능을 다음과 같이 완성하기 위해 다른 기능을 사용할 수 있습니다
평가 중에 사용이 정당화되는 한. 당신은 또한 필요에 따라 예상 장면 파일 형식을 수정할 수 있습니다.
Be smart!


💡 모든 보너스 포인트를 받으려면 최소 14개를 검증해야하므로 현명하게 선택하되 시간을 낭비하지 않도록 주의하십시오!
~~~

## 예시

- [ ] 구 1개, 스팟 1, 약간의 shine(선택)
- [ ] 원기둥 1개, 스팟 1
- [ ] 삼각뿔 1개(선택), 평면 1개, 스팟 1
- [ ] 모든 것 약간씩.. 평면 2개 포함  
- [ ] 위와 같은 장면, 다른 카메라 각도
- [ ] 위와 같은 장면, 그림자와 함께
- [ ] 여러 개의 스팟
- [ ] 평면 1개, 삼각형 3개, 사각형 1개, 왼쪽에 어두운 스팟 1
- [ ] 위 장면 + 여러 개의 스팟, shine 받은 체크무늬의(선택) 구를 가운데에 놓기

## 구현과정
[바로가기](miniRT구현과정)

