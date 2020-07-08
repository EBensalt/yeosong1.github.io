Contents
- [레이 정의 하기](rt-레이-정의-하기)
- [카메라 레이 만들기](#카메라-레이-만들기)
- [표준 좌표계](rt-표준좌표계)
- [소스 코드](rt-소스코드)

# 카메라 레이 만들기

1. 렌더러의 목표가 프레임의 각 픽셀에 색을 할당하는 것임을 기억해봅시다.
결과물은, 특정한 3D 관점에서 보았을 때 장면의 형상을 정확하게 표현해야 합니다.
2. FOV(화각)같은 매개변수에 따라 보이는 장면의 양이 바뀐다는 것도 이전 장에서 봐서 알고 있습니다.
3. 프레임의 각 픽셀에 대해 하나의 광선을 생성하여 레이 트레이싱한 이미지가 생성된다는 것도 레슨 1과 이번 레슨에서 설명했습니다. 
장면에서 광선이 물체와 교차할 때 그 교차점에서 물체의 색으로 픽셀의 색을 설정한다. 우리가 이미 레슨 1에서 얘기했던.. 하지만 다음 레슨에서 좀 더 자세하게 다시 설명할 이 프로세스는, **backward-tracing 역추적** 또는 **eye-tracing**이라고 부릅니다. (빛의 경로를 광원->물체->카메라가 아니라, 카메라->물체->광원으로 따라가기 때문이다)



![campixel](https://user-images.githubusercontent.com/53321189/85225141-fcf76b00-b409-11ea-88d3-6291612b2d4a.gif)
~~~
그림 1 : backward-tracing 역추적 또는 eye-tracing은 눈에서 이미지의 각 픽셀 중심을 통과하는 추적 광선으로 구성됩니다. 광선이 장면에서 객체와 교차하는 경우 광선이 통과하는 픽셀의 색상은 이 교차점에서 객체의 색상으로 설정됩니다.
~~~

자연스럽게 이미지 생성 프로세스는 primary rays, camera rays라고 부르는 이 광선을 구성하는 것으로 시작합니다. 
(primary 왜냐면 이것들이 우리가 장면에 쏘는 첫 광선들이니까. secondary는 예를 들어 그림자 광선으로 나중에 이야기할 것입니다..)(같은 얘기 반복이 많네요.......) 
광선 구성을 도와줄.. 이 광선들에 대해서 우리는 뭘 알고 있죠?
이 광선들이 카메라 원점origin에서 시작한다는 것을 알고 있습니다.
거의 모든 3D 어플리케이션에서, 카메라를 만들 때 카메라의 기본 위치를 세계의 원점origin(좌표 (0, 0, 0))으로 정의합니다.
[3D Viewing: the Pinhole Camera Model](https://www.scratchapixel.com/lessons/3d-basic-rendering/3d-viewing-pinhole-camera) 레슨을 기억해보세요. 카메라의 원점은 핀홀 카메라의 조리개(조리개이자 투사projection하는 중심)로 볼 수 있습니다. 
실제 핀홀 카메라의 필름은 조리개 뒤에 있어서 기하학적 구조에 의해 광선이 장면의 역상을 형성합니다. 그러나 필름면이 장면과 같은 면에 있는 경우, (조리개 뒤가 아니라 조리개 앞) 이 반전을 피할 수 있습니다. 관례적으로 ray tracing에서는 카메라 원점에서 정확히 1 단위 떨어진 곳에 배치하는 경우도 왕왕 있습니다(이 거리는 절대 바뀌지 않습니다. 왜 그런지 저 밑에서 설명할 것입니다).
또 관례적으로, 카메라는 음의 z축을 따라 방향을 조정합니다(카메라 기본 방향은 개발자의 선택에 달려 있지만, 일반적으로 카메라는 양 또는 음의 z축을 따라 방향이 지정됩니다. RenderMan, Maya, PBRT 및 OpenGL은 카메라를 음의 z축을 따라 정렬하므로, 동일한 규칙을 따르는 편을 개발자에게 제안하고 싶습니다). 마지막으로, 증명을 단순화하기 위해 렌더링된 이미지는 square(이미지의 너비와 높이가 픽셀 단위에서 똑같은 square)인 것으로 가정하겠습니다.

![cambasic](https://user-images.githubusercontent.com/53321189/85225229-860ea200-b40a-11ea-865b-f71613779406.png)
~~~
그림 2 : 왼쪽 : 기본 카메라. 원래 이미지 크기는 6x6 픽셀이며 눈의 기본 위치는 세계의 중심 (0, 0, 0)입니다.
카메라가 음의 z축을 따라 어떻게 가리키는지 확인하십시오. 이미지 평면은 원점에서 정확히 1 단위 떨어져 있습니다.
오른쪽 : y축 왼쪽과 x축 아래의 픽셀은 음의 world space 좌표를 갖습니다.
~~~

우리의 작업은 프레임 안의 각 픽셀에 대한 primary rays를 만드는 것으로 구성됩니다. 이는 카메라 원점에서 시작해서 각 픽셀의 가운데를 통과하는 선을 추적해서 쉽게 수행 할 수 있습니다 (그림 1). 이 선을 <origin은 카메라의 원점origin이고, direction은 카메라의 원점부터 픽셀 중심까지의 벡터인 광선>의 형태로 표현할 수 있습니다. 픽셀 중앙의 점의 위치를 계산하기 위해서, 원래 **raster space**(점 좌표는 좌표(0,0)가 프레임의 왼쪽 상단 모서리 인 픽셀로 표시됩니다)로 표현되었던 픽셀 좌표를 **world space**로 변환하는 작업이 필요합니다.


![raydisk](https://user-images.githubusercontent.com/53321189/85227091-12729200-b416-11ea-8af7-8fb7b6ce1113.png)
~~~
그림 3 : 레이-디스크 교차
~~~

왜 world space인가? world space는 기본적으로 장면의 모든 객체, 기하, 빛 및 카메라의 좌표가 표현되는 공간입니다. 
예를 들어, 디스크 형태가 음의 z축을 따라 월드 원점에서 5 단위 떨어져있는 경우, 디스크의 world space 좌표는 (0, 0, -5)입니다.
우리가 이 디스크와 광선의 교차점을 계산하기 위해 수학을 사용하고 싶다면, 광선의 원점과 방향도 같은 공간에서 정의되어야 합니다.
예를 들어, 광선의 원점(0, 0, 0)과 방향direction(0, 0, -1)이 있고, 이 숫자가 world space 좌표계의 좌표를 나타내는 경우,
광선은 (0, 0,- 5)에서 교차합니다. 이 내용이 그림 3에 나와 있습니다.

space 관련 정보는 [Computing the Pixel Coordinates of a 3D point](https://www.scratchapixel.com/lessons/3d-basic-rendering/computing-pixel-coordinates-of-3d-point)레슨에서 더 찾아보실 수 있습니다.

![rastertoworld](https://user-images.githubusercontent.com/53321189/85227427-61212b80-b418-11ea-98e5-5f822adf997f.gif)<br>그림 4 : 래스터에서 screen space으로

우리가 해결하려는 문제를 보는 또 다른 방법은 카메라에서 시작하는 것입니다. 이미지 평면이 world space의 원점에서 정확히 1 단위(1 유닛) 떨어진 곳에 위치하고, 음의 z축을 따라 정렬되어 있음을 우리는 알고 있습니다. 

또한 이미지가 정사각형이므로, 이미지가 투영되는 이미지 평면의 부분도 필연적으로 정사각형임을 알고 있어요. 앞으로 설명 할 이유 때문에, 이 projection 영역의 크기는 2 x 2 단위입니다 (그림 2). 또한 래스터 이미지가 픽셀로 구성되어 있음을 알고 있습니다. 우리가 필요로 하는 것은, raster space에서 이들 픽셀의 좌표와 동일한 픽셀의 좌표 사이에서 world space로 표현되는 관계를 찾는 것입니다. 이 프로세스에는 그림 5에 표시된 몇 가지 단계가 필요합니다.

![cambasic1A](https://user-images.githubusercontent.com/53321189/85227454-93cb2400-b418-11ea-8254-93f2b75d723d.png)
~~~
그림 5 : 픽셀 중간에 있는 점의 좌표를 표준 좌표로 변환하려면 몇 단계가 필요합니다.
이 점의 좌표는 먼저 raster space(픽셀 좌표에 오프셋 0.5를 더한 값)을 표시 한 다음,
NDC space로 변환(좌표를 [0,1]범위로 리매핑)한 다음,
screen space로(NDC 좌표를 [-1,1]범위로 리매핑)변환합니다.
최종 카메라-->world 변환 4x4 매트릭스를 적용하면
screen space의 좌표가 world space로 변환됩니다.
~~~












우리는 첫째로, 프레임 차원에서 픽셀 위치를 정규화 시켜야 합니다.
픽셀들의 정규화 된 새 좌표를 '**NDC space**에 정의되었다'고 말합니다. (NDC = Nomalized Device Coordinates)

**NDC space**: <br>
<img width="234" alt="스크린샷 2020-06-26 오후 3 04 56" src="https://user-images.githubusercontent.com/53321189/85825997-a2219300-b7be-11ea-9df1-b7783fae63c3.png">

우리는 최종 카메라 레이가 픽셀의 정중앙을 지나길 원하기 때문에 픽셀에 약간의 이동(0.5)을 준다는 점을 기억하세요.
NDC 공간으로 표현된 픽셀 좌표는 [0,1]범위에 있습니다.(네, 레이 트레이싱에서의 NDC 공간은, rasterization world의 NDC 공간(일반적으로 [-1,1] 범위로 매핑 됨)과 다릅니다. 그림 2에서 볼 수 있듯이, 필름 또는 이미지 평면은 world의 원점을 중심으로 합니다.
다시 말해서, 이미지 왼쪽에 있는 픽셀은 음의 x좌표를 가져야하고, 오른쪽에 있는 픽셀은 양의 x좌표를 가져야합니다.
동일한 논리가 y축에 적용됩니다. x축으로 정의된 선보다 위쪽에 있는 픽셀은 양의 y좌표를 가져야하고, 아래쪽에 있는 픽셀은 음의 y좌표를 가져야합니다.
현재 [0:1]부터 [-1:1] 범위에 있는 정규화 된 픽셀 좌표를 다시 매핑하여 이 문제를 보정할 수 있습니다.:

<img width="284" alt="스크린샷 2020-06-27 오후 6 36 43" src="https://user-images.githubusercontent.com/53321189/85919347-4b42b900-b8a5-11ea-9446-255d57cf93ca.png">

근데 이 방정식을 사용하면, 리매핑된 픽셀 y는 x축보다 위쪽에 위치한 픽셀에 대해 음수이고, 그 아래쪽에 위치한 픽셀에 대해서는 양수입니다.(반대로 되어야함) 다음 공식으로 이 문제를 보정할 수 있습니다.

<img width="284" alt="스크린샷 2020-06-27 오후 6 36 43" src="https://user-images.githubusercontent.com/53321189/85919347-4b42b900-b8a5-11ea-9446-255d57cf93ca.png">

픽셀 y가 0부터 ImageWidth 사이가 됨에 따라 이제 값은 1에서 -1사이가 됩니다.(? The value now varies from 1 to -1 as Pixely varies from 0 to *ImageWidth.*) 이러한 방식으로 표현된 좌표를 '**screen space**에 정의되었다'고 합니다. 

지금까지 우리는 이미지가 정사각형이라고 가정했습니다. 이미지 종횡비를 설명하는 것은 꽤 간단합니다.
이미지의 크기가 7 x 5 픽셀 인 경우를 살펴봅시다.(작은 이미지긴 하지만 이미지임)
너비를 이미지 높이로 나누면 1.4 입니다.
픽셀 좌표가 screen space에 정의되면 [-1, 1] 범위에 있습니다.
그러나 x 축 (7)을 따라 더 많은 픽셀이 있고 y 축 (5)을 따라 더 많은 픽셀이 있어서
픽셀은 세로 축을 따라 찌그러지고 늘어납니다 (그림 6 참조).

![camratio](https://user-images.githubusercontent.com/53321189/85942481-7c35f300-b964-11ea-825a-884650c5e76f.png)
~~~
그림 6. 왼쪽 그림: 이미지의 너비와 높이가 다르기 때문에 픽셀이 정사각형이 아닙니다. 이를 수정하기 위해 너비를 이미지의 높이(픽셀)로 나누어 계산할 수 있는 이미지 종횡비로 x축을 따라 이미지 평면을 보정해야 합니다.
~~~

픽셀을 다시 정사각형으로 만들려면(픽셀을 픽셀 답게..) 픽셀의 x좌표에 이미지 종횡비(화면비율)을 곱해야합니다. 이 경우 1.4입니다 (다시 그림 6 참조). 이 작업은 screen space에서 y픽셀 좌표를 변경하지 않은 상태로 둔다는 점을 숙지하세요. 그것들은 여전히 [-1,1] 범위에 있지만, x픽셀 좌표는 이제 [-1.4, 1.4] 범위에 있습니다(이는 일반적으로 [-종횡비, 종횡비]입니다).

<img width="453" alt="스크린샷 2020-06-28 오후 5 35 08" src="https://user-images.githubusercontent.com/53321189/85942680-c10e5980-b965-11ea-8505-9f5cf040af1b.png">

---------------

![camprofile](https://user-images.githubusercontent.com/53321189/85944283-94ac0a80-b970-11ea-93b8-71a63c583855.png)

~~~
그림 7 : 카메라 세팅의 측면도.
눈 위치에서 이미지 평면까지의 거리는 1 단위입니다(벡터 AB).
B에서 C까지의 거리도 1 단위입니다.
간단한 삼각법을 사용하여 각도 α를 쉽게 계산할 수 있습니다.
~~~

드디어 화각(FOV) 계산입니다.<br>
지금까지 screen space에 정의된 점의 y좌표는 [-1, 1] 범위에 있습니다.<br>
또한 이미지 평면이 카메라 원점에서 1 단위 떨어져 있다는 것을 알고 있습니다.<br>
측면에서 카메라 세팅을 본다 치면, {카메라 원점, 필름 평면의 맨위 끝, 필름 평면의 맨 아래 끝 가장자리}를 연결하여 삼각형을 그릴 수 있습니다.
카메라 원점에서 필름 평면까지의 거리(1 단위)와 필름 평면의 높이 (y = 1 에서 y = -1 까지 이동하니까 2 단위)를 알고 있기 때문에
우리는 간단한 삼각법을 사용해서 (수직각 α의 절반에 해당하는) 직각 삼각형 ABC의 각도를 찾을 수 있습니다.<br>
우리의 관심사인 직각 삼각형 ABC의 각도:

<img width="353" alt="스크린샷 2020-06-28 오후 6 51 47" src="https://user-images.githubusercontent.com/53321189/85944268-73e3b500-b970-11ea-81fb-1b8eb9eefd58.png">

즉, 특정 경우의 화각 또는 각도 α는 90도이다.<br>
이제 선 BC의 길이를 계산하려면 각도 α의 접선을 2로 나눈 값만 계산하면 됩니다:

<img width="174" alt="스크린샷 2020-06-28 오후 6 57 42" src="https://user-images.githubusercontent.com/53321189/85944383-451a0e80-b971-11ea-93ce-69acac826003.png">

기하학 레슨에서 배운 &#124;&#124;V&#124;&#124;는 벡터 V의 길이임을 기억해보자.<br> 
각도 α가 90도 보다 크면, ||BC||는 1보다 크고<br>
각도 α가 90도 보다 작으면, ||BC||는 1보다 작다.

~~~
ex)
α = 60 이면,
tan(60/2) = 0.57
α = 110 이면,
tan(110/2) = 1.43
~~~

따라서 이 숫자를 스크린 픽셀 좌표(현재는 [-1, 1] 범위에 포함됨)에 곱해서 스케일을 확대 또는 축소를 할 수 있습니다.
짐작할 수 있듯이, 이 작업은 우리가 보는 장면의 양을 변경합니다. 이 작업은 줌 인(화각 감소 = 보이는 장면 감소)및 줌 아웃(화각 증가 = 보이는 장면 증가)과 같습니다.
결론적으로 우리는 화각(카메라의 시야)을 다음 것들로 표현해 정의할 수 있습니다.
각도 α의 탄젠트 값을 2로 나눈 결과를 스크린 픽셀 좌표(이 각도가 각도법(degree)으로 표시되는 경우 [호도법(radians)](각도법과-호도법)으로 변환하기 잊지마세요)랑 곱한 것 :

<img width="555" alt="스크린샷 2020-06-28 오후 7 37 08" src="https://user-images.githubusercontent.com/53321189/85945158-cb851f00-b976-11ea-8f09-06aa0c0ea7fd.png">


이 시점에서, 원래의 픽셀 좌표는 카메라의 이미지 평면과 관련하여 표현됩니다.
그것들은
- 정규화되고,
- [-1 : 1] 사이에서 재 매핑되고,
- 이미지 종횡비에 의해 곱해지고,
- 시야각 α의 탄젠트를 2로 나눈 값에 의해 곱해집니다.

이 점은 좌표가 카메라의 좌표계와 관련하여 표현되기 때문에 '**camera space**에 있다'고 말합니다.<br>
카메라가 기본 위치에 있으면, 카메라의 좌표계와 world의 좌표계가 일렬로 정렬됩니다.<br>
이 점은 카메라 원점에서 1 단위 떨어진 이미지 평면에 있지만, 카메라 역시 음의 z축을 따라 정렬되어있음을 기억하세요.<br>
따라서 이미지 평면에서 픽셀의 최종 좌표를 다음과 같이 표현할 수 있습니다:

<img width="407" alt="스크린샷 2020-06-28 오후 8 16 12" src="https://user-images.githubusercontent.com/53321189/85945977-3dac3280-b97c-11ea-8cf3-df3a80ad1171.png">

이를 통해 카메라의 이미지 평면 위에 있는 이미지의 픽셀의 위치 P(PcameraSpace)를 얻을 수 있습니다.<br>
여기에서 광선의 원점을 카메라의 원점(이 점 O라고 부르기로 하자)으로 정의하고,<br>
광선의 방향을 정규화 된 벡터 OP로 정의하여 이 픽셀의 광선을 계산할 수 있습니다 (그림 8).<br>
벡터 OP는 단순히 이미지 평면에서 카메라 원점을 뺀 지점의 위치입니다.<br>
카메라가 기본 위치에 있을 때 카메라 원점과 세계 직교 좌표계는 동일하므로, 점 O는 단순히 (0, 0, 0)입니다.

pseudo 코드로는 다음과 같습니다(최종 C++ 구현 체크하세요):

~~~
float imageAspectRatio = imageWidth / (float)imageHeight; // width > height 인 걸로 가정하고 
float Px = (2 * ((x + 0.5) / imageWidth) - 1) * tan(fov / 2 * M_PI / 180) * imageAspectRatio; 
float Py = (1 - 2 * ((y + 0.5) / imageHeight) * tan(fov / 2 * M_PI / 180); 
Vec3f rayOrigin(0); 
Vec3f rayDirection = Vec3f(Px, Py, -1) - rayOrigin; // 이거는 Vec3f(Px, Py, -1);랑 똑같은 거임! 
rayDirection = normalize(rayDirection); // 이건 방향이니까 정규화 까먹지 말기
~~~

-----------------------------
![camtransform](https://user-images.githubusercontent.com/53321189/85946266-40a82280-b97e-11ea-85bb-1d069c266768.png)

~~~
그림 8 : 원하는대로 장면을 프레이밍하기 위해 공간 안에서 카메라를 옮길 수 있습니다.
카메라의 최종 위치와 방향(orientation)은 일반적으로 카메라에서 world로 변환 매트릭스라고하는 4x4 매트릭스로 나타낼 수 있습니다.
O(= 한 카메라의 원점이자 world 좌표계의 원점)과 P(선이 통과하는 픽셀의 world 공간 위치)를 알고 있으면
camera-to-world-matrix에 의한 O와 P를 곱하면 O'과 P'를 쉽게 얻을 수 있습니다.
마지막으로 광선 방향은 P'-O'로 계산될 수 있습니다.
~~~

마지막으로, 특정 시점에서 장면 이미지를 렌더링 할 수 있기를 원합니다.<br>
카메라를 원래 위치(world 좌표계의 중심에 위치하고 음의 z축을 따라 정렬)에서 이동 한 후,<br>
4x4 matrix로 카메라의 변환 및 회전 값을 표현할 수 있습니다.<br>
일반적으로 이 matrix를 **camera-to-world-matrix**라고 하며 그 반대의 경우 **world-to-camera-matrix**이라고 합니다.<br>
이 camera-to-world-matrix를 점 O와 P에 적용하면 벡터 ||O'P'||는 (여기서 O'와 P'는, 각각 camera-to-world-matrix에 의해 변환된 점 O와 점 P임) world 공간에서 광선의 정규화 된 방향을 나타냅니다 (그림 8 참고). camera-to-world 변환을 O와 P에 적용하면, 이 두 점은 카메라 공간에서 world 공간으로 변환됩니다. 다른 옵션은 카메라가 기본 위치(벡터 OP)에 있는 동안 광선 방향을 계산하고, 이 벡터에 camera-to-world-matrix를 적용하는 것입니다.

카메라 좌표계가 카메라와 어떻게 움직이는지 확인하세요. 우리의 psuedo 코드는 카메라 변형을 설명하기 위해 쉽게 수정할 수 있습니다(회전, 이동, 카메라 배율 조정은 특히 권장되지 않음(??)):

~~~
float imageAspectRatio = imageWidth / imageHeight; // assuming width > height 
float Px = (2 * ((x + 0.5) / imageWidth) - 1) * tan(fov / 2 * M_PI / 180) * imageAspectRatio; 
float Py = (1 - 2 * ((y + 0.5) / imageHeight) * tan(fov / 2 * M_PI / 180); 
Vec3f rayOrigin = Point3(0, 0, 0); 
Matrix44f cameraToWorld; 
cameraToWorld.set(...); // 매트릭스 설정하기
Vec3f rayOriginWorld, rayPWorld; 
cameraToWorld.multVectMatrix(rayOrigin, rayOriginWorld); 
cameraToWorld.multVectMatrix(Vec3f(Px, Py, -1), rayPWorld); 
Vec3f rayDirection = rayPWorld - rayOriginWorld; 
rayDirection.normalize(); // 이건 방향이니까 정규화 까먹지 말기
~~~

최종 이미지를 계산하려면 방금 설명한 방법을 사용해서 프레임의 각 픽셀에 대해 광선을 만들고<br>
이런 광선들이 장면의 형상과 교차하는지 테스트해야 합니다.<br>
안타깝지만 이 일련의 레슨에서 아직 결론(광선과 물체 사이의 교차점 계산)에 도달하지 않았지만.. 그건 다음 두 레슨의 주제입니다.

# 소스 코드
이번 레슨에서의 소스 코드는 '이미지의 각 픽셀에 광선을 생성하는 방법'의 간단한 예시일 뿐입니다.<br>
코드는 이미지의 모든 픽셀을 돌 동안 반복하고(13-14 행), 현재 픽셀의 광선을 계산합니다.<br>
이 장에서 설명한 모든 리매핑 단계를 한 줄의 코드로 결합했습니다.<br>
원래 x의 픽셀 좌표는 이미지 너비로 나누어 초기 좌표를 [0,1] 범위로 리매핑합니다.<br>
그런 다음 결과 값을 [-1,1] 범위로 리매핑하고, 변수 `scale`(9행)과 이미지 종횡비(10행)로 스케일을 조정합니다.<br>
픽셀 y의 좌표는 비슷한 방식으로 변환되지만, 정규화 된 y의 좌표는 뒤집혀야 한다는 점을 기억하십시오 (16행).<br>
이 프로세스가 끝나면 변환된 점 x와 점 y의 좌표를 사용하여 벡터를 만들 수 있습니다.<br>
이 벡터의 z의 좌표는 마이너스 1로 설정됩니다 (18행):<br>
기본적으로 카메라가 음의 z축을 내려다봐서 그렇습니다.<br>
결과 벡터는 최종적으로 camera-to-world 카메라에 의해 변환되고 정규화 됩니다.<br>
카메라의 원점은 camera-to-world-matrix에 의해 변형됩니다 (12 행).<br>
최종적으로 ray direction과 world 공간으로 변환된 원점을 `rayCast` 함수에 전달할 수 있습니다.

~~~
void render( 
    const Options &options, 
    const std::vector> &objects, 
    const std::vector> &lights) 
{ 
    Matrix44f cameraToWorld; 
    Vec3f *framebuffer = new Vec3f[options.width * options.height]; //7행. rayCast 함수의 결과를 저장하는 프레임 버퍼
    Vec3f *pix = framebuffer; 
    float scale = tan(deg2rad(options.fov * 0.5));  //9행. 스케일 변수와
    float imageAspectRatio = options.width / (float)options.height;  //10행. 이미지 종횡비로 스케일을 조정.
    Vec3f orig; 
    cameraToWorld.multVecMatrix(Vec3f(0), orig); // 12행. 카메라 원점은 camera-to-world-matrix에 의해 변형 됨.
    for (uint32_t j = 0; j < options.height; ++j) { 
        for (uint32_t i = 0; i < options.width; ++i) { 
            float x = (2 * (i + 0.5) / (float)options.width - 1) * imageAspectRatio * scale; 
            float y = (1 - 2 * (j + 0.5) / (float)options.height) * scale; //16행. 정규화 된 좌표를 뒤집어야 함을 기억!
            Vec3f dir; 
            cameraToWorld.multDirMatrix(Vec3f(x, y, -1), dir); //18행. 이 벡터의 z 좌표는 마이너스 1로 설정됩니다 
            dir.normalize(); 
            *(pix++) = castRay(orig, dir, objects, lights, options, 0); 
        } 
    } 
 
    // PPM 이미지에 결과 저장 (Windows에서 컴파일 할 경우 이 플래그 유지할 것)
    std::ofstream ofs("./out.ppm", std::ios::out | std::ios::binary); 
    ofs << "P6\n" << options.width << " " << options.height << "\n255\n"; 
    for (uint32_t i = 0; i < options.height * options.width; ++i) { 
        char r = (char)(255 * clamp(0, 1, framebuffer[i].x)); 
        char g = (char)(255 * clamp(0, 1, framebuffer[i].y)); 
        char b = (char)(255 * clamp(0, 1, framebuffer[i].z)); 
        ofs << r << g << b; 
    } 
 
    ofs.close(); 
 
    delete [] framebuffer; 
} 
~~~

다음 레슨에서는 `rayCast` 함수를 호출해서 primary rays를 장면에 캐스트 하는 법!!!을 보여줄 거에요!!<br>
`rayCast` 함수는 ray origin, ray direction을 인수로 하고,<br>
(물체 및 조명 목록 등의 기타 항목들도 인수로 쓰고요) color를 리턴 합니다.<br>
이 함수는 광선이 교차점에서 어떤 물체에도 닿지 않거나, 물체의 색에 닿지 않을 경우에는 배경색을 리턴할 거에요.<br>

이미지의 모든 픽셀을 반복하여 색상을 계산하기 전에, `rayCast` 함수의 결과를 저장하는 프레임 버퍼를 만듭니다 (7 행).<br>
이미지의 모든 픽셀에 대해 모든 광선이 추적되면 이 이미지의 결과를 디스크에 저장할 수 있습니다.<br>
아쉽지만 다음 레슨에 도달할 때까지 `rayCast` 함수를 구현할 수 없습니다.<br>
그동안 광선 방향을 색상으로 변환하고 대신 현재 픽셀에 대해 이 색상을 저장합니다 (아래 8-9 행).<br>
최종 이미지는 ppm rabbits 형식(이게 뭐지..) (위의 25-34 행)으로 디스크에 저장됩니다.<br>

~~~
Vec3f castRay( 
    const Vec3f &orig, const Vec3f &dir, 
    const std::vector<std::unique_ptr<Object>> &objects, 
    const std::vector<std::unique_ptr<Light>> &lights, 
    const Options &options, 
    uint32_t depth) 
{
    Vec3f hitColor = (dir + Vec3f(1)) * 0.5; 
    return hitColor; 
} 
~~~

----------------

컴퓨터 그래픽에서, 그것들은 종종 다른 접근법을 사용하여 동일한 결과를 얻는 다른 방법입니다.<br>
만약에 당신이 다른 렌더 엔진의 소스 코드를 보면, 광선을 이미지 space에서 wolrd space로 변환하는 문제를 여러 가지 방법으로 해결할 수 있다는 걸 알게될 겁니다. 그러나 취한 접근 방식에 관계없이 결과는 항상 동일해야합니다.

<img width="100" alt="trigosetup" src="https://user-images.githubusercontent.com/53321189/86926994-b7f36880-c16d-11ea-9949-381283fb3f1a.png">

예를 들어 다음과 같은 방식으로 문제를 볼 수 있습니다: 픽셀 좌표를 정규화 할 필요가 없습니다 (즉, 픽셀 좌표에서 NDC로 변환한 다음 screen space로 변환). 다음 방정식을 사용하여 광선 방향을 계산할 수 있습니다:<br>

<img width="298" alt="스크린샷 2020-07-08 오후 10 52 00" src="https://user-images.githubusercontent.com/53321189/86927125-e2ddbc80-c16d-11ea-910e-25d90d154e2f.png">

여기서 x와 y는 픽셀의 좌표이고 fov는 화각 입니다. 카메라는 기본적으로 음의 z축 방향을 향하기 때문에 dz는 음수입니다.<br>
그런 다음 이 벡터를 정규화하면 래스터에서 NDC로, NDC에서 화면으로 변환한 것과 같은 결과를 얻습니다.<br>
이 광선 방향을 world 공간으로 변환하면 (벡터 d에 camera-to-world matrix를 곱함) 다음과 같은 결과가 나타납니다:

<img width="502" alt="스크린샷 2020-07-08 오후 10 57 35" src="https://user-images.githubusercontent.com/53321189/86927672-96df4780-c16e-11ea-9965-dd31dbc64b3d.png">

식을 펼쳐서 다시 묶으면 다음과 같다:

<img width="513" alt="스크린샷 2020-07-08 오후 10 57 42" src="https://user-images.githubusercontent.com/53321189/86928002-f8071b00-c16e-11ea-8396-7347cdc943cf.png">

이렇게 쓸 수도 있다.(식 1) :

<img width="159" alt="스크린샷 2020-07-08 오후 10 57 53" src="https://user-images.githubusercontent.com/53321189/86928369-6cda5500-c16f-11ea-8c54-08af369437d4.png">

여기서 w'는 

<img width="464" alt="스크린샷 2020-07-08 오후 11 07 26" src="https://user-images.githubusercontent.com/53321189/86928670-d195af80-c16f-11ea-941c-07bb5e1d5bc6.png">

다시 말해, camera-to-world matrix을 알고 있다면<br>
w'벡터를 미리 계산하고 식 1을 사용하여 world 공간에서 광선 방향을 계산한 다음,<br>
결과 벡터를 정규화 할 수 있습니다.<br>
psuedo 코드로 보기:

~~~
Vec3f w_p = (-width / 2 ) * u + (height / 2) * v - ((height / 2) / tan(fov_rad * 0.5); 
Vec3f ray_dir = normalize(x * u + y * (-v) + w_p); 
~~~

벡터 w'는 한 번만 계산하면 되고 새로운 광선 방향을 계산할 때마다 재사용됩니다.<br>
벡터 u, v, w는 camera-to-world matrix의 첫 번째, 두 번째, 세 번째 벡터입니다 (row-major matrices을 사용하는 경우에는 처음 세 행).

# 요약: 우리는 첫 번째 이미지를 렌더링 할 준비가 거의 다 되었습니다.

이 일련의 레슨을 진행하면서, 지금까지 배운 모든 기술을 기능성 레이 트레이서에 의해 기본으로 만들기 위해 사용할 수 있을 것입니다.<br>
기능적으로 우리는 일부 지오메트리를 렌더링하고 결과 이미지를 디스크의 파일에 저장하는 것을 의미합니다).<br>
이제 우리가 그 결과를 얻기 위해 필요한데 빠진 것은, 광선-기하 교차(ray-geometry intersection)에 대해 배우는 것입니다.<br>
이게 다음 레슨의 주제입니다.




------------------------
[<- 이전 장](rt-레이-정의-하기)        Chapter 2 of 4         [다음 장 ->](rt-표준좌표계)
