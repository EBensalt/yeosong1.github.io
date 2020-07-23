
# 광선-구 교차

Contents
- [Parametric and Implicit Surfaces](rt-A-Minimal-Ray-Tracer)
- [광선-구 교차](rt-광선-구-교차)
- [미니멀 레이 트레이서: 구 렌더링 하기](rt-미니멀레이트레이서)
- Ray-Plane과 Ray-Disk 교차
- Ray-Box 교차
- 소스 코드

구체와 광선을 교차시키는 것은 아마도 가장 간단한 형태의 광선-기하 도형 교차 테스트 일 것입니다.
이것이 많은 레이 트레이서가 구체의 이미지를 보여주는 이유입니다.
또한 (단순성으로 인해) 매우 빠르다는 이점이 있습니다.
그러나 안정적으로 작동하게 하려면 항상 주의를 기울여야하는 subtitle이 몇 개 있습니다.

이 테스트는 본질적으로 두 가지 방법을 사용하여 구현할 수 있습니다.
첫번째는 기하학을 사용하여 문제를 해결합니다.
두번째 기술은, 종종 더 선호되는 솔루션인데(2차quadric 표면이라고 부르는 더 넓은 범위의 표면에 재사용 할 수 있기 때문에 선호됨),
[해석적](http://www.ktword.co.kr/abbr_view.php?m_temp1=4333)인analytic(또는 대수적인algebraic, ex 닫힌 형태 표현을 사용하여 해석할 수 있음) 솔루션을 사용합니다.

## 기하학적 해법

광선-구 교차점 테스트에 대한 기하학적 솔루션은 간단한 수학에 의존합니다.
주로 기하학, 삼각법, **피타고라스 정리**이지요.
그림 1을 보면 광선이 구와 교차하는 점에 해당하는 점 P 와 P'의 위치를 찾으려면 t0 및 t1의 값을 찾아야 한다는 것을 이해할 것입니다.

![raysphereisect1](https://user-images.githubusercontent.com/53321189/87069125-a1264200-c251-11ea-92d2-752de15aaca1.png)

~~~
그림 1 : 구체와 교차하는 광선과 기하학적 및 분석 솔루션과 광선 구체 교차를 해결하는 데 사용할 다양한 용어.
~~~

광선은 다음과 같은 **파라메트릭 형식**을 사용하여 표현할 수 있습니다.

<img width="151" alt="스크린샷 2020-07-10 오전 2 04 15" src="https://user-images.githubusercontent.com/53321189/87069165-b00cf480-c251-11ea-8af0-3ef25510fcf0.png">

여기서 O는 광선의 원점을 나타내고, D는 광선 방향direction(보통 정규화 되어있음)입니다.<br>
t의 값을 변경하면 광선을 따라 위치를 정의 할 수 있습니다.<br>
t가 0보다 큰 경우, 점은 광선의 원점 앞에 위치하며(선을 내려다 보는)<br>
t가 0일 때 점은 광선의 원점(O)과 일치하고<br>
t가 0보다 작은 경우 점 원점 뒤에 있습니다.<br>
그림 1을 보면 t_ca에서 t_hc를 빼서 t0을 찾을 수 있고 t_hc를 t_ca에 더하면 t1을 찾을 수 있습니다.<br>
광선 파라메트릭 방정식을 사용하여 t0, t1을 찾은 다음, P와 P'를 찾을 수 있는<br>
이 두 값(t_hc 및 t_ca)을 계산하는 방법을 찾는 것만으로도 충분합니다.

<img width="149" alt="스크린샷 2020-07-10 오전 2 04 52" src="https://user-images.githubusercontent.com/53321189/87069219-c9ae3c00-c251-11ea-8087-270b401a3bc8.png">

우리는 가장자리 L, t_ca 및 d에 의해 형성된 삼각형이 직각 삼각형임을 주목하면서 시작할 것입니다.<br>
O(레이 원점)와 C(구 중심) 사이의 벡터인 L을 쉽게 계산할 수 있습니다.<br>
우리는 t_ca에 대해 아무것도 모르지만 삼각법을 사용하여 이 문제를 해결할 수 있습니다.<br>

((리모트 jekyll 테마 사용중인데 수식 쓸 수 있게 어떻게 바꾸는지 모르겠어요.....<br>
정확한 표기은 [원문](https://www.scratchapixel.com/lessons/3d-basic-rendering/minimal-ray-tracer-rendering-simple-shapes/ray-sphere-intersection)에서 봐주세요! 벡터 화살표 표시를 알파벳 위에 그어야 하는데 일단 b->로 표시 하겠습니다.....))

우리는 L을 알고 광선의 방향direction인 D를 알고 있습니다.<br>
또한 벡터 a->와 b->의 **dot(or scalar) product** (점곱, 내적)은 <br>
점 b->과 점 a->를 벡터 a->에 의해 정의된 선에 투영하는 것에 해당하며,<br>
이 투영 결과는 그림 2에 표시된 세그먼트 AB의 길이 입니다.<br>
(내적의 속성에 대한 자세한 내용은 기하학(https://www.scratchapixel.com/lessons/mathematics-physics-for-computer-graphics/geometry/math-operations-on-points-and-vectors) 레슨을 확인하십시오.)

![impsurf-proj-vectors](https://user-images.githubusercontent.com/53321189/88250758-85a44800-cce3-11ea-9f60-53a9de2abbe2.png)

~~~
그림 2: a-> ⋅ b-> =|a||b|cosθ.
~~~

다시 말해, L과 D의 내적은 단순히 t_ca를 제공합니다.<br>
t_ca가 양수인 경우 광선과 구 사이의 교차점 입니다.<br>
(음수이면 벡터 L과 벡터 D가 반대 방향을 가리킴을 의미합니다.<br>
교차점이 있는 경우 잠재적으로 광선의 원점 뒤에 있을 수 있지만<br>
광선의 원점 뒤에 발생하는 모든 것은 우리에게 아무 쓸모가 없습니다).<br>
이제 t_ca와 L이 있습니다.

<img width="193" alt="스크린샷 2020-07-21 오후 7 28 18" src="https://user-images.githubusercontent.com/53321189/88251012-6b1e9e80-cce4-11ea-835e-5548a5ab2782.png">

이 구조에는 d, t_hc 및 구의 반지름으로 정의되는 두 번째 직각삼각형이 있습니다.<br>
우리는 구의 반지름을 이미 알고 있으며, t0과 t1을 알면 알 수 있는 t_hc를 찾고 있습니다.<br>
그러려면 d를 계산해야합니다. d는 또한 d, t_ca 및 L로 정의된 직각삼각형의 반대편이라는 것을 기억하십시오.<br>
피타고라스 정리는 다음과 같이 말합니다:

<img width="365" alt="스크린샷 2020-07-21 오후 7 44 40" src="https://user-images.githubusercontent.com/53321189/88251121-c94b8180-cce4-11ea-82c3-ccf385549d00.png">

우리는 [대변, 인접변, 빗변](https://ko.khanacademy.org/math/trigonometry/trigonometry-right-triangles/intro-to-the-trig-ratios/a/opposite-adjacent-hypotenuse)을 각각 d, t_ca 및 L로 바꿀 수 있습니다:

<img width="272" alt="스크린샷 2020-07-21 오후 7 45 52" src="https://user-images.githubusercontent.com/53321189/88251192-f730c600-cce4-11ea-8a74-fc4b7d66a25e.png">

d가 구 반경보다 크면 광선이 구를 놓쳐서 교차가 없습니다(선이 구를 넘겨서 지나갑니다).
이제 우리는 t_hc 계산에 필요한 모든 걸 가지고 있습니다. 우리는 피타고라스 정리를 다시 사용할 수 있습니다:

<img width="181" alt="스크린샷 2020-07-21 오후 7 56 25" src="https://user-images.githubusercontent.com/53321189/88251289-5e4e7a80-cce5-11ea-82b2-191fb0fc2976.png">

이 섹션의 마지막 단락에서는 C++에서 이 알고리즘을 구현하는 방법을 보여주고 작업 속도를 높이기 위해 몇 가지 최적화를 수행합니다.

## 해석적인 해법

광선은 다음 함수를 사용하여 표현할 수 있습니다: **O + tD (식 1)**<br>
여기서 O는 점이고 광선의 원점에 해당하고, D는 벡터이며 광선의 방향(direction)에 해당하고, t는 a 함수의 매개변수입니다.<br>
t(양수 또는 음수 일 수 있음)를 변경하여 광선 원점과 방향(direction)으로 정의된 선의 모든 점을 계산할 수 있습니다.<br>
t가 0보다 크면 광선의 점이 광선 원점의 "정면"에 있습니다.<br>
t가 음수이면 점은 광선의 원점 뒤에 있습니다.<br>
t가 정확히 0이면 점과 광선의 원점이 동일합니다.<br>

광선-구 교차점 테스트를 해결하는 아이디어는 구도 대수 형태로 정의할 수 있다는 것 입니다.<br>
구의 방정식은 다음과 같습니다:

<img width="153" alt="스크린샷 2020-07-23 오후 1 20 30" src="https://user-images.githubusercontent.com/53321189/88251984-90f97280-cce7-11ea-9f4c-ccb578e0b11f.png">

여기서 x, y 및 z는 [직교좌표계](https://ko.wikipedia.org/wiki/%EC%A7%81%EA%B5%90_%EC%A2%8C%ED%91%9C%EA%B3%84)(cartesian)의 좌표이고 R은 원점을 중심으로 하는 구의 반경입니다.<br>
(나중에 중심이 원점이 아닌 구와 작동하도록 방정식을 변경하는 방법은 나중에 참조)<br>
그것은 위의 방정식이 참인 점의 세트가 있다고 말합니다.<br>
이 점 세트는 원점을 중심으로 하고 반경 R을 갖는 구의 표면을 정의합니다.<br>
또는 더 간단히 말하면, x, y, z가 점 P의 좌표라고 생각하면 다음과 같이 쓸 수 있습니다. **(식 2)**:

<img width="113" alt="스크린샷 2020-07-23 오후 1 20 40" src="https://user-images.githubusercontent.com/53321189/88258295-c8bde580-ccfa-11ea-86fd-4ef6210c2dde.png">

이 방정식은 우리가 수학과 CG에서 **implicit function**이라고 부르는 것의 전형이며<br>
이 형태로 표현된 구를 implicit 모양 또는 표면이라고도 합니다.<br>
implicit 셰이프는 서로 연결된 다각형(예: 만약 당신이 Maya 또는 Blender와 같은 3D 응용 프로그램에서<br>
객체를 모델링했다면 익숙할만한 타입의 기하 도형)이 아니라 방정식으로 간단히 정의할 수 있는 셰이프입니다.<br>
많은 모양(보통 많이 단순한 모양이지만)을 함수(큐브, 원뿔, 구 등)로 정의할 수 있습니다.<br>
단순하지만, 이러한 형태를 결합하여 보다 복잡한 형태를 만들 수 있습니다.<br>
이를테면 blob들을 사용해서(blobby 표면은 mataball로도 불림) 형상을 모델링 하는 것의 개념이 이와 같습니다.
그러나 여기서 너무 멀리 나가기 전에 광선-구 교차점 테스트로 돌아가 봅시다(**implicit 모델링**에 대한 강의는 advanced 섹션을 확인하십시오).

이제 우리가 해야 할 일은 식 2의 식 1을 대체하는 것입니다.
즉 식 2의 P를 광선의 방정식으로 대체합니다(O + tD는 광선을 따라가는 모든 점을 정의한다는 것을 기억하세요):

<img width="157" alt="스크린샷 2020-07-23 오후 1 20 50" src="https://user-images.githubusercontent.com/53321189/88259644-aaa5b480-ccfd-11ea-8558-20bcf1a9e250.png">

이 방정식을 디벨롭하면 다음과 같은 결과를 얻게됩니다. **(식 
3)**:

<img width="402" alt="스크린샷 2020-07-23 오후 1 20 57" src="https://user-images.githubusercontent.com/53321189/88259787-f8bab800-ccfd-11ea-8a79-e0b7287ded2f.png">

이는 그 자체로 다음 식의 방정식입니다 **(식 4)**:

<img width="171" alt="스크린샷 2020-07-23 오후 1 21 04" src="https://user-images.githubusercontent.com/53321189/88259832-16881d00-ccfe-11ea-9709-41a9f199e369.png">

a = D^2, b = 2OD, c = O^2-R^2 (**식 4**의 x는 미지인 **식 3**의 t에 해당함)를 기억하십시오.
**식 4**는 **2차 함수(quadratic function)**로 알려져 있기 때문에 **식 3**을 **식 4**로 다시 쓸 수있는 것이 중요합니다.
다음 식을 사용하여 근(x가 f(x) = 0 인 값을 취할 때)을 쉽게 찾을 수 있는 함수입니다. **(식 5)**:

<img width="177" alt="스크린샷 2020-07-23 오후 1 21 13" src="https://user-images.githubusercontent.com/53321189/88259983-6830a780-ccfe-11ea-9b18-b38bf7e9962a.png">

수식에서 +/- 기호를 확인하십시오.<br>
첫 번째 루트는 부호 +를 사용하고 두 번째 루트는 부호-를 사용합니다.<br>
문자 Δ(그리스 문자 델타)를 **판별자(discriminant)**라고 합니다.<br>
판별의 부호는 방정식에 근이 2 개, 1 개 또는 없는지 여부를 나타냅니다:

- Δ > 0 일때, 다음과 같이 계산할 수 있는 2개의 근이 있습니다.

<img width="240" alt="스크린샷 2020-07-23 오후 4 08 21" src="https://user-images.githubusercontent.com/53321189/88260140-c3629a00-ccfe-11ea-909b-382d44195c50.png">

이 경우, 광선이 구를 두 지점에서 교차합니다.(t0과 t1)

- Δ = 0 일때, 다음과 같이 계산할 수 있는 1개의 근이 있습니다.

<img width="62" alt="스크린샷 2020-07-23 오후 4 08 28" src="https://user-images.githubusercontent.com/53321189/88260233-ec832a80-ccfe-11ea-9549-71cc15b2ce13.png">

광선은 한 지점에서만 구와 교차합니다(t0 = t1)

- Δ < 0 일때, 근이 없습니다(= 광선이 구와 교차하지 않음).

a, b, c가 있으므로 구와 광선의 두 교점에 해당하는 t에 대한 값을 얻기 위해 이러한 방정식을 쉽게 계산할 수 있습니다 (그림 1의 t0 및 t1). 루트 값은 음수 일 수 있습니다. 이는 광선이 구와 교차하지만 원점 뒤에 있음을 의미합니다. 뿌리 중 하나는 음수 일 수 있고 다른 하나는 양수일 수 있으며 이는 광선의 원점이 구 안에 있음을 의미합니다. 2 차 방정식에 대한 해가 없을 수도 있는데, 이는 광선이 구와 전혀 교차하지 않음을 의미합니다 (광선과 구가 교차하지 않음).






------------------------
[이전 장 ->](rt-A-Minimal-Ray-Tracer)          Chapter 2 of 6         [다음 장 ->](rt-미니멀레이트레이서)
