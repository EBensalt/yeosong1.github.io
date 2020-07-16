# 3D 점의 픽셀 좌표 계산

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

**"3D 점의 2D 픽셀 좌표를 어떻게 찾아요?"** 이것은 웹에서 가장 일반적인 질문 중 하나입니다 (3D 렌더링 관련). 3D 장면의 이미지가 형성되는 실질적인 방법이기 때문에 중요한 질문입니다. 이 단원에서는 래스터 화라는 용어를 사용하여 3D 포인트의 2D 픽셀 좌표를 찾는 프로세스를 설명합니다. 넓은 의미에서 래스터 화는 3D 모양을 래스터 이미지로 변환하는 프로세스를 말합니다. 이전 단원에서 설명한 래스터 이미지는 디지털 이미지에 부여되는 기술적 용어입니다. 픽셀의 2 차원 배열 (또는 원하는 경우 사각형 격자)을 지정합니다.

실수하지 마십시오. 3D 장면의 이미지를 생성하기 위해 다른 렌더링 기술이 존재합니다. 래스터 화는 그중 하나 일뿐입니다. 레이트 레이싱도 있습니다. 그러나 이러한 모든 기술은 동일한 개념을 사용하여 해당 이미지를 생성합니다 : 원근 투영 개념. 따라서 주어진 카메라와 주어진 3D 장면에 대해 모든 렌더링 기술은 동일한 시각적 결과를 생성합니다. 그들은 그 결과를 얻기 위해 다른 접근법을 사용합니다.
