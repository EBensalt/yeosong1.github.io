# cub3D

첫번째 레이캐스터 with miniLibX

## 요약:
- 이 프로젝트는 90년대의 세계적으로 유명하며 시초격인, 최초의 FPS장르 게임에서 영감을 받았습니다.
- 레이캐스팅(광선 투사)에 대해 알아봅니다.
- 목표는 미로 안에서 다이나믹 뷰를 만들어 길을 찾는 것입니다.

## 참고
* [Lode's Computer Graphics Tutorial](https://lodev.org/cgtutor/raycasting.html)



#### 레이 캐스팅:
- 컴퓨터 그래픽스와 계산기하학의 다양한 문제를 해결하기 위해 광선과 표면의 교차검사를 사용하는 기법.
- **레이 캐스팅** : 광선이 재귀적으로 반사되어 제 2의 광선을 만들지 않는다.
- **레이 트레이싱** : 광선이 계속적으로 반사되면서 렌더링 결과물에 영향을 끼친다.

#### FPS: 
- First person shooter 게임 장르. 둠 같은 거.

## 머리말
- 개발: John Carmack, John Romero
- 퍼블리시: 1992년 Apogee Software, Wolfenstein 3D
- 비디오 게임 역사상 최초의 "First Person Shooter"
>Wolfenstein 3D는 Doom (Id Software, 1993), Doom II(Id Software, 1994),
>Duke Nukem 3D (3D Realm, 1996) 및 Quake (Id Software, 1996)와 같은
>비디오 게임계의 영원한 마일스톤 같은 게임들의 조상이다.
- Now, it’s your turn to relive History...

## 목표
- 이 프로젝트의 목표는 첫 해의 모든 목표와 비슷합니다. 엄격함, C 사용, 기본 알고리즘의 사용, 정보 탐색 등.
- 그래픽 디자인 프로젝트로써 cub3D는 이러한 영역의 스킬을 향상시킬 수 있다
  - 창?, 색상, 이벤트, 모양 채우기 등. (windows, colors, events, fill shapes, etc.)
- 결론적으로 cub3D는 구체적인 내용을 이해하지 않고도 수학의 실제적인 응용을 탐구 할 수있는 놀라운 놀이터입니다
- 인터넷에서 사용할 수있는 수많은 문서의 도움으로, 수학을 쓸 수 있을 거야. 우아하고 효율적인 알고리즘을 만드는 도구로서의 수학을.

- 이 프로젝트를 시작하기 전에 오리지날 게임을 테스트해보길 추천: [http://users.atw.hu/wolf3d/](http://users.atw.hu/wolf3d/)

## 일반지침

- 놈 지키기. 보너스도
- 예기치 않은 종료 없어야 함(segmentation fault, bus error, double free 등)
- 힙 쓰면 해제 잘 하기. 누수 no
- 필요시 Makefile 제출. 리링크 no, -Wall -Wextra -Werror 사용
- Makefile에 $(NAME), all, clean, fclean, re 꼭 넣기
- 보너스 제출시 룰 bonus 넣기, 이름음 어쩌구_bonus.c/h
- libft 사용 가능시 꼭 libft 소스랑 libft의 Makefile 첨부.
- 평가 요소는 아니지만 테스트 프로그램 제작을 권장한다.
- 할당된 깃에 내시오. 깃에 있는 것만 채점한다.

## 필수파트 - cub3D
- Makefile : all, clean, fclean, re, bonus
- 허용함수:
  - open, close, read, write, malloc, free, perror, sterror, exit
  - math 라이브러리에 있는 모든 함수 (-lm man man 3 math)
  - MinilibX에 있는 모든 함수
  - libft
- Description: 
  - 미로 내부에 있는 1인칭 시점의 "현실적인"3D 그래픽 표현을 만들어야합니다.
  - 앞서 언급한 Ray-Casting 원칙을 사용해서 만들어야함.
  
#### 제약 조건은 다음과 같습니다.
* You must use the miniLibX
* window must remain smooth: 창 변환, 최소화 등
* 각기 다른 벽 텍스처(맘대로 골라)를 쓰기. 동서남북.
* Your program must be able to display an item (sprite) instead of a wall.
* 바닥과 천장 색상을 서로 다른 두 가지 색상으로 설정할 수 있어야합니다.
* In case the Deepthought has eyes one day to evaluate your project, your program
must save the first rendered image in bmp format when its second argument is
"––save".
...

## 보너스 파트
## 예시
