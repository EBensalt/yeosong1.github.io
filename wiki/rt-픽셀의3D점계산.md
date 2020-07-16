# [3D 점의 픽셀 좌표 계산]

>[목차 돌아가기](rt-목차)<br>
>[원문 페이지 바로 가기](https://www.scratchapixel.com/lessons/3d-basic-rendering/computing-pixel-coordinates-of-3d-point)

구성
- 원근 투영
- 3D 점의 2D 좌표 계산을 위한 수학
- 소스코드

Keywords:
- rasterisation
- perspective projection
- wireframe
- perspective projection matrix
- camera coordinate system
- projective transformation
- world space
- world origin
- world coordinate system
- coordinate system
- transformation
- matrix
- matrices
- inverse matrix
- camera coordinate system
- camera space
- image plane
- perspective divide
- screen space
- raster space
- screen coordinate system
- NDC
- normalized device coordinate
- viewport

# 웹에서 3D 렌더링에 대한 가장 일반적인 질문 중 하나

**"3D 점의 2D 픽셀 좌표를 어떻게 찾아요?"** 이것은 웹에서 (3D 렌더링 관련해서) 가장 일반적인 질문 중 하나입니다.<br>
3D 장면의 이미지가 형성되는 실질적인 방법이기 때문에 중요한 질문입니다.<br>
이 레슨에서는 **래스터 화 rasterrization**라는 용어를 사용하여 3D 점의 2D 픽셀 좌표를 찾는 프로세스를 설명합니다.<br>
넓은 의미에서 래스터 화는 3D 모양을 래스터 이미지로 변환하는 프로세스를 말합니다.<br>
[이전 레슨](https://www.scratchapixel.com/lessons/3d-basic-rendering/rendering-3d-scene-overview)에서 설명한 래스터 이미지는 디지털 이미지에 부여되는 기술적 용어입니다.<br>
픽셀의 2 차원 배열 (또는 원하는 경우 사각형 격자)을 지정합니다.<br>

오해하지 마세요: 3D 장면의 이미지를 생성하기 위해 다른 렌더링 기술들이 존재합니다. 래스터 화는 그중 하나 일뿐입니다. 레이 트레이싱도 있고요.<br>
그러나 이런 모든 기술은 다음과 같은 동일한 개념을 사용하여 해당 이미지를 생성합니다 : **원근 투영 perspective projection** 개념.<br>
따라서, 주어진 카메라와 주어진 3D 장면에 대해 모든 렌더링 기술은 동일한 시각적 결과를 생성합니다.<br>
그들은 그 결과를 얻기 위해 다른 접근법을 사용하는 것 뿐입니다.<br>

또한 3D 점의 2D 픽셀 좌표를 계산하는 것은 사실적인 이미지를 생성하는 과정에서 두 단계 중 하나 일 뿐입니다.<br>
다른 단계는 음영 처리 과정이며, 이 점들의 색상은 객체의 모양을 시뮬레이션('모방'이라고 번역해야 할 수도 있겠네요)하기 위해 계산됩니다.<br>
"완전한" 이미지를 생성하려면 3D 점을 픽셀 좌표로 변환하는 것 이상이 필요합니다.<br>

래스터 화를 이해하려면 먼저 이 장에서 소개 할 일련의 매우 중요한 기술들 (예: <br>
로컬 vs 글로벌 좌표계 개념, 4x4 매트릭스를 좌표계로 해석하는 방법 배우기, 한 좌표계에서 다른 좌표계로 점 변환 등)<br>
에 대해 잘 알고 있어야합니다. 이 강의는 거의 모든 렌더링 기술이 기반을 둔 매우 기본적인 도구를 제공하므로 주의 깊게 읽으십시오.<br>

이 단원에서는 행렬(매트릭스)을 많이 사용하므로 좌표계와 행렬에 익숙하지 않은 경우 먼저 기하(Geometry) 수업을 읽으십시오.<br>

이 강의에서 연구한 기술을 적용하여 3D 물체의 **와이어 프레임** 이미지 (인접한 이미지)를 렌더링합니다.<br>
이 프로그램의 파일은 평소와 같이 수업의 소스 코드 장에서 찾을 수 있습니다.<br>


# 원근 투영 프로세스 빠르게 다시 보기

우리는 이미 몇 가지 레슨으로 원근 투사 프로세스에 대해 이야기했습니다.<br>
예를 들어 "3D 장면의 이미지 렌더링 : 개요" 단원의 "[가시성 문제](https://www.scratchapixel.com/lessons/3d-basic-rendering/rendering-3d-scene-overview/visibility-problem)"장을 확인하십시오.<br>
그러나 여기서 perspective projection 원근 투영이 무엇인지 빠르게 기억해 봅시다.<br>
간단히 말해, 이 기법은 3D인 장면을 해당 장면의 물체를 구성하는 점 또는 정점을 캔버스 표면에 투영해서 2D 이미지로 만들 수 있습니다.<br>
왜 그렇게 할까요? 이것은 인간의 눈이 작동하는 방식과 거의 비슷한데, 우리는 우리의 눈을 통해 세상을 보는 데 익숙하기 때문에<br>
그런 방식으로 생성된 이미지가 우리에게도 자연스럽게 보일 것이라고 생각하는 것이 자연스럽습니다. (이 방법을 사용하여 만든 이미지는 "실제"처럼 보입니다)<br>
인간의 눈은 공간에 있는 하나의 "점"이라고 볼 수 있습니다. (그림 2)<br>
우리가 보는 세계는 (물체에 의해 반사된) 광선의 결과로, 이 지점까지 이동해서 눈으로 들어갑니다.<br>
(물론 눈이 정확히 하나의 점은 아닙니다. 망막이라는 작은 표면으로 광선들을 수렴하는 광학 시스템입니다)<br>
그러니까 다시 말해, CG에서 3D 장면의 이미지를 만드는 한 가지 방법은 그와 똑같이,<br>
마치 정점 자체가 눈에 연결하는 직선을 따라 쭉 슬라이딩 하는 것처럼, 캔버스 표면(혹은 스크린 표면)에 점을 투영하는 것입니다.<br>

원근 투영은 2차원 표면에 3D 형상을 나타내는 임의의 방법 중 하나일 뿐이라는 것을 이해하는 것이 중요합니다.<br>
이는 인간 시력의 가장 중요한 속성 중 하나인 단축(멀리 있는 물체가 가까이 있는 물체보다 작게 보임)을 모방하는 것이기 때문에 가장 일반적으로 사용되는 방법입니다.
그럼에도 불구하고, Wikipedia 기사에서 [원근](https://en.wikipedia.org/wiki/Perspective_(graphical))에 언급된 것처럼 사실적인 이미지를 만드는 동안 원근은 "(종이 같은) 평평한 표면에 눈으로 보이는 이미지의 **대략적인 표현 approximate representation**"을 유지한다는 것을 이해하는 것이 중요합니다. 여기서 중요한 단어는 "대략 approximate"입니다.

![raystoeye](https://user-images.githubusercontent.com/53321189/87664937-0d003180-c7a1-11ea-8589-03eb6037fc5a.png)

~~~
그림 2 : 물체에 의해 반사된 모든 광선 중에서 이 광선 중 일부는 눈에 들어가고 이 물체의 이미지는 광선의 결과입니다.
~~~

![projection3](https://user-images.githubusercontent.com/53321189/87664943-0ec9f500-c7a1-11ea-8808-485da0f6c673.png)

~~~
그림 3 : 투영 프로세스는 투영하려는 점이 점 또는 정점 자체를 눈에 연결하는 선을 따라 아래로 이동 한 것처럼 보일 수 있습니다.
캔버스의 평면에 있을 때 해당 선을 따라 점 이동을 중지 할 수 있습니다. 분명히 우리는 이 선을 따라 점을 명시적으로 "슬라이드"하지 않지만,
이것은 투영 과정을 해석 할 수 있는 방법입니다.
~~~

앞에서 언급한 레슨에서는 카메라 앞에 위치한 (그리고 카메라의 시야 [절두체](https://ko.wikipedia.org/wiki/%EC%A0%88%EB%91%90%EC%B2%B4) 내에 포함되어 카메라에 표시되는) 점의 world 좌표를, 비슷한 삼각형의 특성 중 하나를 기반으로 한 간단한 기하학적 구조를 사용해서 계산하는 방법에 대해서도 설명했습니다.(그림 3). 이 레슨에서 이 기술을 한 번 더 검토하겠습니다. 투영된 점의 좌표를 계산하는 방정식은 실제로 어떻게든 4x4 행렬의 형태로 표현될 수 있습니다.

하는 중......
