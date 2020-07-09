(프로세스 간 전반적인 이해를 위한 것으로, miniRT 풀이에 필수적인 부분은 아님)

Contents
- [레이 정의 하기](rt-레이-정의-하기)
- [카메라 레이 만들기](rt-카메라-레이-만들기)
- [표준 좌표계](#표준-좌표계-Standard-Coordinate-Systems)
- [소스 코드](rt-소스코드)

# 표준 좌표계 Standard Coordinate Systems

점 변환 [파이프라인](https://ko.wikipedia.org/wiki/%ED%8C%8C%EC%9D%B4%ED%94%84%EB%9D%BC%EC%9D%B8_(%EC%BB%B4%ED%93%A8%ED%8C%85))(vertex transformation pipeline)에 관련해서, 좌표계 점을 변환하는 데 사용되는 용어에는 몇 가지 차이점이 있습니다.
특히, 가장 일반적인 렌더링 API중 하나인 OpenGL과 RenderMan Interface(우리가 이 레슨에서 사용하는 좌표계 정의는 이 인터페이스의 정의를 따릅니다)이 그렇습니다. 당신이 만약 OpenGL 렌더링 pipeline에 익숙하다면 이전 장의 일부 용어가 잘못 사용 되었다고 생각 했을 수도 있습니다.
먼저, 이 레슨은 OpenGL이나 z-버퍼 기반 렌더러에서 사용하는 클래식한 점 변환 파이프라인이 아니라, ray tracer에서 primary rays 또는 camera rays를 생성하는 방법에 관한 것입니다. 이런 광선들을 생성하는 프로세스는 rasterization-based 렌더러에서 이미지 평면에 점을 투영하는 데 사용되는 프로세스와 반대되는 프로세스입니다. 둘째로, 점이 변환되는 공간을 정의하는 데에 사용되는 좌표계의 의미는 우리가 ray tracer를 사용하는지 아니면 래스터라이저를 사용하는지에 의존하는 것이 아니고 관습(convention)의 문제입니다.
일반적으로 world, camera, object, raster 공간은 모든 렌더링 API에서 동일한 정의를 갖지만
클리핑 좌표의 정의는 시스템마다 다를 수 있습니다(특히 OpenGL, NDC, 화면 공간).
primary rays를 만들기 위해 이 단원에서 설명한 시스템과 OpenGL vertex transformation pipeline([이전 레슨](https://www.scratchapixel.com/lessons/3d-basic-rendering/perspective-and-orthographic-projection-matrix/projection-matrix-GPU-rendering-pipeline-clipping)에서 설명한)사이의 차이를
보다 명확하게 이해하기 위해 다시 간략하게 설명하고 이 장에서 비교합니다. 이 연습이 이 수업을 읽었을 때 그래픽스 API 전문가의 마음에 발생할 수 있는 혼란을 제거 할 수 있기를 바랍니다.

레이 트레이싱에서 우리는 픽셀 위치에서 시작한다. (ray direction을 빌드할 수 있는 이미지 평면 상의 점으로 변환한 그 픽셀 위치)
이 과정은 원래의 "점 좌표"(픽셀 좌표 또는 **raster space**로 표현된 점 좌표)를 **NDC space**로 변환한 다음 **screen space**로 변환하는 작업이 포함됩니다.(이전 장에서 설명한 과정).
raster에서 NDC로의 변환 = 원래의 픽셀 좌표를 값 [0,1] 범위에 리매핑합니다.
NDC->screen 공간 변환 = 이미지가 정사각형인 경우 [0,1]에서 [-1,1]범위로 값을 리매핑하고,
x축을 따라 [-종횡비, 종횡비]범위에 리매핑합니다. 
이미지의 가로가 이미지의 세로보다 큰 경우 y축을 따라 [-1,1]범위에 리매핑합니다...........
그러면 이 좌표는 카메라의 화각의 탄젠트를 2로 나눈 값으로 조정됩니다.
요약하자면, 우리는 래스터에서 NDC로, 그런 다음에는 screen 공간으로 왔습니다.

래스터라이제이션을 위한 프로세스는 다릅니다.
그것의 목표는 3D 공간에 점을 찍고, 그 점을 이미지 평면에 쏘고, 결과 좌표를 픽셀 좌표로 변환하는 것입니다.
3D 공간에 있는 점이며, [원근 투상(또는 정사 투상) 행렬](https://www.scratchapixel.com/lessons/3d-basic-rendering/perspective-and-orthographic-projection-matrix/projection-matrix-introduction)을 일반적으로 사용하는 프로세스 인 이미지에 투영해야하는 점에서 시작합니다. 
첫 번째 점은 3D **world space**에서 **camera space**로 변환됩니다. 점 좌표는 카메라의 좌표계와 관련해서 정의됩니다.
일단 카메라 공간에 들어가면, 그러면 점은 이미지 평면에 투영된다. 예를 들면 원근 투상 행렬(perspective projection matrix)에. 
OpenGL의 vertex transformation pipeline의 이 단계에서 이 점을 가리켜 '**클리핑 좌표clipping coordinates**가 있다'고 말합니다.
그 점은 아직 z좌표로 나뉘지 않았지만 OpenGL은 이미 보이는지 아닌지 가시성 여부를 테스트 할 수 있습니다.
이 가시성 테스트 혹은 클리핑 테스트를 통과하면, [동차 좌표 homogeneous coordinates](https://ko.wikipedia.org/wiki/%EB%8F%99%EC%B0%A8%EC%A2%8C%ED%91%9C)에서 [직교 좌표 Cartesian coordinates](https://ko.wikipedia.org/wiki/%EC%A7%81%EA%B5%90_%EC%A2%8C%ED%91%9C%EA%B3%84)로 변환됩니다 (프로세스는 z 또는 원근 분할을 알고 있음). 그 점은 **normalized device coordinates**를 갖거나 **NDC space**에 있다고 말합니다.
OpenGL에서 NDC space에 있는 점의 좌표는 [-1,1] 범위에 포함됩니다.
RenderMan 인터페이스에서 NDC space는 점 좌표가 [0,1] 범위에 있는 것으로 정의합니다.
마지막으로 NDC 공간의 점은 최종 픽셀 좌표가 아닌 **래스터 좌표 raster coordinates** 또는 **창 좌표 window coordinates**로 변환됩니다.

다음 표에서 두 시스템의 서로 다른 단계들과 관련된 좌표계들을 요약했습니다:

| 픽셀 좌표로 ray direction을 계산하기 | OpenGL (vertex transformation pipeline) |
|:---|:---|
| 점은 픽셀의 위치 (픽셀 좌표). **Raster space**다. | 점의 좌표가 **world space**에 정의 되어있다. |
| 픽셀 좌표나 raster space에서 **NDC space**로 점을 변환. 점의 좌표는 [0:1] 범위에 리매핑 | world 에서 **camera space**로 변환 |
| Remap point's coordinates from NDC to screen space. Point's coordinates in screen space vary between [-aspect ratio, aspect ratio] along the x-axis, and [-1, 1] along the y-axis. | Project point onto the near clipping plane (the image plane) using a projection matrix. |
| Points in screen space are scaled by the tangent of the camera field of view divided by two. | Cli|pping coordinates (before the perspective divide). Point passes the clipping test: is it visible or not? |
| Coordinates of the resulting point are used to build the ray direction vector. | Perspective divide. The resulting point's coordinates are said to be in NDC space. They are now in the range [-1, 1] (including the z coordinate). |
| The ray origin and direction are transformed by the camera-to-world matrix. The ray can now be tested for interesection against the geometry of the scene. | Point is transformed from NDC space to raster space or window coordinates (pixel coordinates). |

Point is transformed from NDC space to screen space or window coordinates (pixel coordinates).

이하 생략..


------------------------
[<- 이전 장](rt-카메라-레이-만들기)        Chapter 3 of 4         [다음 장 ->](rt-소스코드)
