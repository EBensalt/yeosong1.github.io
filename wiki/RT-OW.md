# Ray Tracing in One Weekend - by Peter Shirley
아래 내용은 [https://raytracing.github.io/books/RayTracingInOneWeekend.html](https://raytracing.github.io/books/RayTracingInOneWeekend.html)의 내용을 한글 번역하고, 추가로 C언어로 바꾼 코드를 첨부한 것이다.

## 1. 개요
**(개요는 그냥 번역기 돌린 그대로 입니다)**<br>
저는 수년 동안 많은 그래픽 수업을 가르쳤습니다. 종종 수업들을 ray tracing으로 하는데, 모든 코드를 (직접) 작성해야하지만 API 없이도 멋진 이미지를 얻을 수 있기 때문입니다. 나는 당신이 가능한 한 빨리 멋진 프로그램을 만들 수 있도록 내 코스 노트를 방법론으로 바꾸기로 결정했습니다. 그것은 완전한 기능을 갖춘 ray tracer는 아니지만, 영화에 ray tracing을 필수 요소로 만드는 간접적인 조명을 갖고 있습니다. 다음 단계를 따르십시오. 당신이 생산하는 ray tracer의 아키텍처는 당신이 흥미를 느끼고 그것을 추구하고자 한다면 더 광범위한 ray tracer로 확장하는 데 좋을 것 입니다.

누군가 "ray tracing"이라고 말하면 많은 것을 의미 할 수 있습니다. 제가 설명하려고하는 것은 기술적으로 경로 추적자이며 상당히 일반적인 것입니다. 코드는 매우 간단하지만 (컴퓨터가 작업을 수행하도록 합니다!) 만들 수 있는 이미지에 매우 만족할 것입니다.

몇 가지 디버깅 팁과 함께 내가하는 순서대로 ray tracer를 작성하는 과정을 안내합니다. 마지막에는 멋진 이미지를 생성하는 ray tracer가 생깁니다. 주말동안 이 일을 할 수 있어야 합니다. 더 오래 걸리더라도 걱정하지 마십시오. C++을 운전 언어로 사용하지만 그럴 필요는 없습니다. 그러나 빠르고 휴대 가능하며 대부분의 프로덕션 영화 및 비디오 게임 렌더러가 C++로 작성 되었기 때문에 그렇게 하는 것이 좋습니다. **C++의 대부분의 "최신 기능"은 피하지만 상속 및 연산자 오버로딩은 ray tracr가 전달하기에 너무 유용**합니다. 나는 온라인으로 코드를 제공하지 않지만 코드는 실제이며 vec3 클래스의 몇 가지 간단한 연산자를 제외하고는 모두 보여줍니다. 나는 그것을 배우기 위해 코드를 타이핑하는 것에 큰 신념을 가지고 있지만, 코드를 사용할 수 있을 때는 그것을 사용하므로 코드를 사용할 수 없을 때만 내가 설교하는 것을 연습하는 것 뿐입니다. 그러니 묻지 마세요!

나는 180이 한 일이 재미 있기 때문에 마지막 부분을 남겼습니다. 몇몇 독자들은 우리가 코드를 비교할 때 도움이 된 미묘한 오류로 끝났습니다. 따라서 코드를 입력하십시오. 하지만 내 것을 보고 싶다면 다음 위치에 있습니다:

[https://github.com/RayTracing/raytracing.github.io/](https://github.com/RayTracing/raytracing.github.io/)

**나는 벡터(내적과 벡터 덧셈과 같은)에 약간 익숙하다고 가정합니다.** 잘 모르겠다면 약간의 검토를 하세요.
검토가 필요하거나 처음으로 배우려면 Marschner와 저의 그래픽 텍스트, Foley, Van Dam 등 또는 McGuire의 그래픽 코덱을 확인하십시오.

문제가 발생하거나 누군가에게 보여주고 싶은 멋진 일을한다면 ptrshrl@gmail.com으로 이메일을 보내주세요.

이 책과 관련된 블로그 [https://in1weekend.blogspot.com/](https://in1weekend.blogspot.com/)의 추가 자료 및 리소스 링크를 포함하여 책과 관련된 사이트를 유지 관리 할 것입니다.

이 프로젝트에 도움을 주신 모든 분들께 감사드립니다. 이 책의 끝에 있는 감사의 말 섹션에서 찾을 수 있습니다.

시작합시다!
     
## 2. 이미지 출력
### 2.1 PPM 이미지 포맷
렌더러를 시작할 때마다 이미지를 볼 수 있는 방법이 필요합니다. 가장 간단한 방법은 파일에 쓰는 것입니다. 문제는 형식이 너무 많다는 것입니다. 그 중 다수는 복잡합니다. 저는 항상 일반 텍스트 ppm 파일로 시작합니다. 다음은 Wikipedia의 멋진 설명입니다.

![](https://raytracing.github.io/images/fig-1.01-ppm.jpg)
>그림 1: PPM Example

저런 것을 출력하는 C++ 코드를 만들어 보겠습니다:

```C++
#include <iostream>

int main() {

    // Image

    const int image_width = 256;
    const int image_height = 256;

    // Render

    std::cout << "P3\n" << image_width << ' ' << image_height << "\n255\n";

    for (int j = image_height-1; j >= 0; --j) {
        for (int i = 0; i < image_width; ++i) {
            auto r = double(i) / (image_width-1);
            auto g = double(j) / (image_height-1);
            auto b = 0.25;

            int ir = static_cast<int>(255.999 * r);
            int ig = static_cast<int>(255.999 * g);
            int ib = static_cast<int>(255.999 * b);

            std::cout << ir << ' ' << ig << ' ' << ib << '\n';
        }
    }
}
```
>목록 1 : [main.cc] 첫 번째 이미지 만들기

해당 코드에서 몇 가지 유의해야 할 사항이 있습니다.

1. 픽셀은 왼쪽에서 오른쪽으로 픽셀이 있는 행으로 기록됩니다.
2. 행은 위에서 아래로 기록됩니다.
3. 관례상 각 빨강 / 녹색 / 파랑 구성 요소의 범위는 0.0에서 1.0까지입니다. 나중에 내부적으로 높은 다이내믹 레인지를 사용할 때 이를 완화할 것이지만 출력 전에 0에서 1 범위로 톤 매핑하므로이 코드는 변경되지 않습니다.
4. 빨간색은 왼쪽에서 오른쪽으로 완전히 꺼짐(검정색)에서 완전히 켜짐(밝은 빨간색)으로 바뀌고, 녹색은 아래쪽의 검은 색에서 위쪽으로 완전히 켜집니다. 빨간색과 녹색이 함께 노란색을 만들므로 오른쪽 상단 모서리가 노란색이 될 것으로 예상해야 합니다.

### 2.2 Creating an Image File
파일이 프로그램 출력에 기록되기 때문에 이미지 파일로 리디렉션해야합니다. 일반적으로 다음과 같이 > 리디렉션 연산자를 사용하여 명령 줄에서이 작업을 수행합니다: 

~~~
build\Release\inOneWeekend.exe > image.ppm
~~~

이것이 Windows에서 어떻게 보이는지 입니다. Mac 또는 Linux에서는 다음과 같습니다.

~~~
build/inOneWeekend > image.ppm
~~~

출력 파일을 열면(내 Mac의 `ToyViewer`에서 열고, 지원하지 않는 경우 즐겨찾는 뷰어나 Google "ppm 뷰어"에서 시도)다음 결과가 표시됩니다:

![img-1 01-first-ppm-image](https://user-images.githubusercontent.com/53321189/89877582-40e14200-dbfb-11ea-8ce4-dd575868d8f1.png)

>이미지 1: 첫번째 PPM image

만세! **이것이 그래픽스의 “hello world”입니다.** 이미지가 저렇게 보이지 않으면 텍스트 편집기에서 출력 파일을 열고 어떻게 보이는지 확인합니다. 대충 다음과 같이 시작해야합니다: 

~~~
P3
256 256
255
0 255 63
1 255 63
2 255 63
3 255 63
4 255 63
5 255 63
6 255 63
7 255 63
8 255 63
9 255 63
...
~~~

>목록 2: 첫번째 이미지 아웃풋

그렇지 않은 경우 이미지 리더를 혼란스럽게하는 줄 바꿈이나 이와 유사한 것이 있을 수 있습니다.

PPM보다 더 많은 이미지 유형을 만들고 싶다면, 저는 헤더 전용 이미지 라이브러리인 stb_image.h의 팬입니다.
GitHub [https://github.com/nothings/stb](https://github.com/nothings/stb)에서 사용할 수 있습니다. 

#### 2.2.1 이 항목에 있는 C++ 코드👇 를 C언어 + mlx로 바꿔보았습니다. 

```C++
#include <iostream>

int main() {

    // Image

    const int image_width = 256;
    const int image_height = 256;

    // Render

    std::cout << "P3\n" << image_width << ' ' << image_height << "\n255\n";

    for (int j = image_height-1; j >= 0; --j) {
        for (int i = 0; i < image_width; ++i) {
            auto r = double(i) / (image_width-1);
            auto g = double(j) / (image_height-1);
            auto b = 0.25;

            int ir = static_cast<int>(255.999 * r);
            int ig = static_cast<int>(255.999 * g);
            int ib = static_cast<int>(255.999 * b);

            std::cout << ir << ' ' << ig << ' ' << ib << '\n';
        }
    }
}
```


이거를 C언어 + mlx 윈도우에 그려보자! 👇 

```C
#include "mlx.h"
#include <stdlib.h>

typedef	struct s_mlx
{
	void *mlx_ptr;
	void *win_ptr;
	void *img_ptr;
	int  *data;
	int		bpp;
	int	size_l;
	int	endian;

	int color[3];
} t_mlx;

t_mlx	app;

#include <stdio.h>

int	main()
{
	const int	image_width = 256;
	const int	image_height = 256;

	app.mlx_ptr = mlx_init();
	app.win_ptr = mlx_new_window(app.mlx_ptr, 700, 400, "rainbow");
	app.img_ptr = mlx_new_image(app.mlx_ptr, image_width, image_height);
	app.data = (int *)mlx_get_data_addr(app.img_ptr, &app.bpp, &app.size_l, &app.endian);

	int j = 0;
	while (j < image_height)
	{
		int i = 0;
		while (i < image_width)
		{
			float r = (double)i / (image_width - 1);
			float g = (image_heoght - (double)j - 1) / (image_height - 1);
			float b = 0.25;

			int	ir = 255.999 * r;
			int	ig = 255.999 * g;
			int	ib = 255.999 * b;

			app.color[0] = ir * 256 * 256;
			app.color[1] = ig * 256;
			app.color[2] = ib;

			int color = app.color[0] + app.color[1] + app.color[2];
			app.data[jj * 256 + i] = mlx_get_color_value(app.mlx_ptr, color);
			++i;
		}
		j++;
	}
	mlx_put_image_to_window ( app.mlx_ptr, app.win_ptr, app.img_ptr, 0, 0 );
	mlx_loop(app.mlx_ptr);
}
```

<img width="812" alt="스크린샷 2020-08-11 오후 5 49 59" src="https://user-images.githubusercontent.com/53321189/89877469-1d1dfc00-dbfb-11ea-93c6-dff71d96da85.png">

# 여기서 의문점. 400 * 400 하면 왜 깨지지?

### 2.3 진행률 표시기 추가
계속하기 전에 출력에 진행률 표시기를 추가해 보겠습니다. 이것은 긴 렌더의 진행 상황을 추적하고 무한 루프 또는 기타 문제로 인해 중단 된 실행을 식별하는 편리한 방법입니다.

프로그램은 이미지를 표준 출력 스트림 (std :: cout)으로 출력하므로 그대로두고 대신 오류 출력 스트림 (std :: cerr)에 씁니다:

```C++
        for (int j = image_height-1; j >= 0; --j) {
            std::cerr << "\rScanlines remaining: " << j << ' ' << std::flush;     //***** 추가됨
            for (int i = 0; i < image_width; ++i) {
                auto r = double(i) / (image_width-1);
                auto g = double(j) / (image_height-1);
                auto b = 0.25;

                int ir = static_cast<int>(255.999 * r);
                int ig = static_cast<int>(255.999 * g);
                int ib = static_cast<int>(255.999 * b);

                std::cout << ir << ' ' << ig << ' ' << ib << '\n';
            }
        }
        std::cerr << "\nDone.\n";						//***** 추가됨
```

>목록 3: [main.cc] 진행률 보고 기능이 있는 메인 렌더링 루프


## 3. vec3 class
거의 모든 그래픽 프로그램에는 기하학적 벡터와 색상을 저장하기 위한 클래스가 있습니다. 많은 시스템에서 이러한 벡터는 4D입니다 (지오메트리에 대한 3D + 동종 좌표, 색상에 대한 RGB + 알파 투명 채널). 우리의 목적을 위해서는 세 개의 좌표로 충분합니다. 색상, 위치, 방향, 오프셋 등에 대해 동일한 클래스 `vec3`을 사용합니다. 위치에 색상을 추가하는 것과 같은 어리석은 일을 하는 것을 막지 않기 때문에 어떤 사람들은 이것을 좋아하지 않습니다. 그들은 좋은 점을 가지고 있지만 명백히 잘못되지 않은 경우 항상 "더 적은 코드"경로를 택할 것입니다. 그럼에도 불구하고 `vec3`에 대한 두 가지 별칭 인 `point3`과 `color`를 선언합니다. 이 두 유형은 `vec3`의 별칭 일 뿐이므로, 예를 들어 `point3`를 예상하는 함수에 색상을 전달하면 경고가 표시되지 않습니다. 우리는 의도와 사용을 명확히 하기 위해서만 사용합니다.

### 3.1 변수와 메소드
내 `vec3` 클래스의 윗부분은 다음과 같습니다:

```C++
#ifndef VEC3_H
#define VEC3_H

#include <cmath>
#include <iostream>

using std::sqrt;

class vec3 {
    public:
        vec3() : e{0,0,0} {}
        vec3(double e0, double e1, double e2) : e{e0, e1, e2} {}

        double x() const { return e[0]; }
        double y() const { return e[1]; }
        double z() const { return e[2]; }

        vec3 operator-() const { return vec3(-e[0], -e[1], -e[2]); }
        double operator[](int i) const { return e[i]; }
        double& operator[](int i) { return e[i]; }

        vec3& operator+=(const vec3 &v) {
            e[0] += v.e[0];
            e[1] += v.e[1];
            e[2] += v.e[2];
            return *this;
        }

        vec3& operator*=(const double t) {
            e[0] *= t;
            e[1] *= t;
            e[2] *= t;
            return *this;
        }

        vec3& operator/=(const double t) {
            return *this *= 1/t;
        }

        double length() const {
            return sqrt(length_squared());
        }

        double length_squared() const {
            return e[0]*e[0] + e[1]*e[1] + e[2]*e[2];
        }

    public:
        double e[3];
};

// Type aliases for vec3
using point3 = vec3;   // 3D point
using color = vec3;    // RGB color

#endif
```

>목록 4: [vec3.h] vec3 class

여기서는 `double`을 사용하지만 일부 광선 추적기는 `float`를 사용합니다. 어느 쪽이든 괜찮습니다 - 자신의 취향을 따르십시오.

### 3.2 vec3 Utility 함수
헤더 파일의 두 번째 부분에는 벡터 유틸리티 함수가 포함되어 있습니다:

```C++
// vec3 Utility Functions

inline std::ostream& operator<<(std::ostream &out, const vec3 &v) {
    return out << v.e[0] << ' ' << v.e[1] << ' ' << v.e[2];
}

inline vec3 operator+(const vec3 &u, const vec3 &v) {
    return vec3(u.e[0] + v.e[0], u.e[1] + v.e[1], u.e[2] + v.e[2]);
}

inline vec3 operator-(const vec3 &u, const vec3 &v) {
    return vec3(u.e[0] - v.e[0], u.e[1] - v.e[1], u.e[2] - v.e[2]);
}

inline vec3 operator*(const vec3 &u, const vec3 &v) {
    return vec3(u.e[0] * v.e[0], u.e[1] * v.e[1], u.e[2] * v.e[2]);
}

inline vec3 operator*(double t, const vec3 &v) {
    return vec3(t*v.e[0], t*v.e[1], t*v.e[2]);
}

inline vec3 operator*(const vec3 &v, double t) {
    return t * v;
}

inline vec3 operator/(vec3 v, double t) {
    return (1/t) * v;
}

inline double dot(const vec3 &u, const vec3 &v) {
    return u.e[0] * v.e[0]
         + u.e[1] * v.e[1]
         + u.e[2] * v.e[2];
}

inline vec3 cross(const vec3 &u, const vec3 &v) {
    return vec3(u.e[1] * v.e[2] - u.e[2] * v.e[1],
                u.e[2] * v.e[0] - u.e[0] * v.e[2],
                u.e[0] * v.e[1] - u.e[1] * v.e[0]);
}

inline vec3 unit_vector(vec3 v) {
    return v / v.length();
}
```
>목록 5: [vec3.h] vec3 utility functions

### 3.3 Color Utility 함수
새로운 `vec3` 클래스를 사용하여 단일 픽셀의 색상을 표준 출력 스트림에 쓰는 유틸리티 함수를 만들 것입니다.

```C++
#ifndef COLOR_H
#define COLOR_H

#include "vec3.h"

#include <iostream>

void write_color(std::ostream &out, color pixel_color) {
    // Write the translated [0,255] value of each color component.
    out << static_cast<int>(255.999 * pixel_color.x()) << ' '
        << static_cast<int>(255.999 * pixel_color.y()) << ' '
        << static_cast<int>(255.999 * pixel_color.z()) << '\n';
}

#endif
```

>목록 6: [color.h] color utility functions

이제 이것을 사용하도록 메인을 변경할 수 있습니다:

```C++
#include "color.h"
#include "vec3.h"

#include <iostream>

int main() {

    // Image

    const int image_width = 256;
    const int image_height = 256;

    // Render

    std::cout << "P3\n" << image_width << ' ' << image_height << "\n255\n";

    for (int j = image_height-1; j >= 0; --j) {
        std::cerr << "\rScanlines remaining: " << j << ' ' << std::flush;
        for (int i = 0; i < image_width; ++i) {
            color pixel_color(double(i)/(image_width-1), double(j)/(image_height-1), 0.25);
            write_color(std::cout, pixel_color);
        }
    }

    std::cerr << "\nDone.\n";
}
```
>목록 7: [main.cc] Final code for the first PPM image

## 4. 레이, 간단한 카메라, 배경
### 4.1 ray class
모든 광선 추적기에 있는 한 가지는 광선 클래스와 광선을 따라서 어떤 색상이 보이는지에 대한 계산입니다. 광선을 함수 𝐏 (𝑡) = 𝐀 + 𝑡𝐛로 생각해 봅시다. 여기 𝐏는 3D상에서 선을 따라 갔을 때의 3차원 위치 입니다. 𝐀은 광선 원점이고 𝐛은 광선 방향입니다. 광선 매개 변수 𝑡은 실수입니다(코드에서 `double`). 다른 𝑡를 연결하면 𝐏(𝑡)가 광선을 따라 점을 이동 시킵니다. 음수 𝑡값을 더하면 3D 선상의 아무 곳이나 갈 수 있습니다. 양수 𝑡의 경우 𝐀 앞에 있는 부분만 얻습니다. 이것이 보통 반직선 또는 레이라고 하는 것입니다.


![](https://raytracing.github.io/images/fig-1.02-lerp.jpg)
> 그림 2: 선형 보간법(Linear interpolation)

좀 더 자세한 코드 형식의 𝐏(𝑡) 함수는 `ray :: at(t)`라고 부르겠습니다:

```C++
#ifndef RAY_H
#define RAY_H

#include "vec3.h"

class ray {
    public:
        ray() {}
        ray(const point3& origin, const vec3& direction)
            : orig(origin), dir(direction)
        {}

        point3 origin() const  { return orig; }
        vec3 direction() const { return dir; }

        point3 at(double t) const {
            return orig + t*dir;
        }

    public:
        point3 orig;
        vec3 dir;
};

#endif
```

>목록 8: [ray.h] The ray class


### 4.2 장면에 광선 쏘기

이제 코너를 돌아서 광선 추적기를 만들 준비가 되었습니다. 핵심에서 광선 추적기는 픽셀을 통해 광선을 보내고 광선 방향에서 보이는 색상을 계산합니다. 관련된 단계는 (1) 눈에서 픽셀까지의 광선을 계산하고, (2) 광선이 교차하는 물체를 결정하고, (3) 해당 교차점의 색상을 계산하는 것입니다. 처음 광선 추적기를 개발할 때 저는 항상 코드를 실행하고 실행하기 위해 간단한 카메라를 사용합니다. 또한 배경색(간단한 그라데이션)을 반환하는 간단한 `ray_color(ray)` 함수를 만듭니다.

디버깅에 정사각형 이미지를 사용하면 𝑥와 𝑦을 바꿔 넣는 일이 잦기 때문에 종종 문제가 발생하므로, 정사각형이 아닌 이미지를 사용합니다. 지금은 16 : 9 종횡비를 사용하겠습니다. 매우 일반적이니까요.

렌더링 된 이미지에 대한 픽셀 치수를 설정하는 것 외에도 장면 광선을 통과 할 가상 뷰포트를 설정해야 합니다. 표준 정사각형 픽셀 간격의 경우 뷰포트의 종횡비가 렌더링 된 이미지와 동일해야합니다. 높이가 2 단위인 뷰포트를 선택하겠습니다. 또한 투영 면과 투영 점 사이의 거리를 하나의 단위로 설정합니다. 이것을 "초점 거리focal length"라고하며 나중에 설명할 "초점 거리focus distance"와 혼동하지 마십시오.

"눈"(또는 카메라를 생각하면 카메라 중심)을 (0,0,0)에 배치합니다. y축은 위로, x축은 오른쪽으로 이동합니다. 오른손 좌표계의 규칙을 따르기 위해 화면에 음의 z축이 있습니다. 왼쪽 하단 모서리에서 화면을 가로 지르고 화면 측면을 따라 두 개의 오프셋 벡터를 사용하여 화면에서 광선 끝점을 이동합니다. 광선 방향을 단위 길이 벡터로 만들지 않는 점에 유의해주세요. 그렇게해서 코드가 더 간단하고 약간 빨라진다고 생각합니다.


![](https://raytracing.github.io/images/fig-1.03-cam-geom.jpg)
> 그림 3: 카메라 지오메트리

아래 코드에서 광선 `r`은 대략 픽셀 중심으로 이동합니다 (나중에 안티 앨리어싱을 추가 할 것이기 때문에 지금은 정확성에 대해 걱정하지 않겠습니다):

```C++
#include "color.h"
#include "ray.h"
#include "vec3.h"

#include <iostream>

color ray_color(const ray& r) {
    vec3 unit_direction = unit_vector(r.direction());
    auto t = 0.5*(unit_direction.y() + 1.0);
    return (1.0-t)*color(1.0, 1.0, 1.0) + t*color(0.5, 0.7, 1.0);
}

int main() {

    // Image
    const auto aspect_ratio = 16.0 / 9.0;
    const int image_width = 400;
    const int image_height = static_cast<int>(image_width / aspect_ratio);

    // Camera

    auto viewport_height = 2.0;
    auto viewport_width = aspect_ratio * viewport_height;
    auto focal_length = 1.0;

    auto origin = point3(0, 0, 0);
    auto horizontal = vec3(viewport_width, 0, 0);
    auto vertical = vec3(0, viewport_height, 0);
    auto lower_left_corner = origin - horizontal/2 - vertical/2 - vec3(0, 0, focal_length);

    // Render

    std::cout << "P3\n" << image_width << " " << image_height << "\n255\n";

    for (int j = image_height-1; j >= 0; --j) {
        std::cerr << "\rScanlines remaining: " << j << ' ' << std::flush;
        for (int i = 0; i < image_width; ++i) {
            auto u = double(i) / (image_width-1);
            auto v = double(j) / (image_height-1);
            ray r(origin, lower_left_corner + u*horizontal + v*vertical - origin);
            color pixel_color = ray_color(r);
            write_color(std::cout, pixel_color);
        }
    }

    std::cerr << "\nDone.\n";
}
```
> 목록 9: [main.cc] Rendering a blue-to-white gradient

`ray_color (ray)`함수는 광선 방향을 단위 길이로 조정한 후 𝑦 좌표의 높이에 따라 흰색과 파란색을 선형으로 혼합합니다 (따라서 −1.0 <𝑦 <1.0). 벡터를 정규화 한 후 𝑦 높이를보고 있기 때문에 수직 그라디언트 외에도 색상에 수평 그라디언트가 있음을 알 수 있습니다.
그런 다음 0.0 ≤ 𝑡 ≤ 1.0으로 확장하는 표준 그래픽 트릭을 수행했습니다. 𝑡 = 1.0이면 파란색을 원합니다. 𝑡 = 0.0이면 흰색을 원합니다. 그 사이에 블렌드를 원합니다. 이것은 두 가지 사이에 "선형 혼합linear blend", 또는 "선형 보간linear interpolation"또는 줄여서 "lerp"를 형성합니다. lerp는 항상 아래 형식입니다.

> blendedValue = (1 − 𝑡) * startValue + 𝑡 * endValue,

𝑡가 0에서 1로 이동합니다. 우리의 경우 다음을 생성합니다:

![](https://raytracing.github.io/images/img-1.02-blue-to-white.png)
> 이미지 2: A blue-to-white gradient depending on ray Y coordinate


#### 4.2.1 위 코드를 C언어로 옮겨 보았습니다.

<img width="868" alt="스크린샷 2020-08-14 오전 11 29 11" src="https://user-images.githubusercontent.com/53321189/90207518-6cde0c80-de21-11ea-8fd5-4ffb8708fa5b.png">

<details>
<summary> <b> 🛠 소스코드 </b>  </summary>
<div markdown="1">

```C
#include "mlx.h"
#include <stdlib.h>
#include <math.h>

typedef struct s_vec
{
	float	x;
	float	y;
	float	z;
} t_vec;

typedef	struct s_mlx
{
	void *mlx_ptr;
	void *win_ptr;
	void *img_ptr;
	int  *data;
	int		bpp;
	int	size_l;
	int	endian;

	t_vec	color;
	int		int_color;

} t_mlx;

t_vec	make_v(float n)
{
	t_vec v;
	v.x = n;
	v.y = n;
	v.z = n;

	return (v);
}

t_vec	v_mul_n(t_vec v1, float n)
{
	t_vec	result;

	result.x = v1.x * n;
	result.y = v1.y * n;
	result.z = v1.z * n;
	return (result);
}

t_vec	v_mul(t_vec v1, t_vec v2)
{
	t_vec	result;

	result.x = v1.x * v2.x;
	result.y = v1.y * v2.y;
	result.z = v1.z * v2.z;
	return (result);
}

float	dot(t_vec v1, t_vec v2)
{
	return (v1.x * v2.x + v1.y * v2.y + v1.z * v2.z);
}

t_vec	v_sub(t_vec v1, t_vec v2)
{
	t_vec	result;

	result.x = v1.x - v2.x;
	result.y = v1.y - v2.y;
	result.z = v1.z - v2.z;
	return (result);
}

t_vec	v_add(t_vec v1, t_vec v2)
{
	t_vec	result;

	result.x = v1.x + v2.x;
	result.y = v1.y + v2.y;
	result.z = v1.z + v2.z;
	return (result);
}

t_vec	v_div(t_vec v1, float d)
{
	t_vec	result;

	result.x = v1.x / d;
	result.y = v1.y / d;
	result.z = v1.z / d;
	return (result);
}

t_vec	origin(t_vec orig, t_vec dir)
{
	return (orig);
}

t_vec	direction(t_vec orig, t_vec dir)
{
	return (dir);
}
/*
t_vec	ray(t_vec orig, t_vec dir)
{
		
}

t_vec	at(t_vec orig, t_vec dir, double t)
{
	return (orig + t * dir);
}
*/

void	write_color(t_mlx *app, t_vec c) 
{
	int	ir = 255.999 * c.x;
	int	ig = 255.999 * c.y;
	int	ib = 255.999 * c.z;

	app->color.x = ir * 256 * 256;
	app->color.y = ig * 256;
	app->color.z = ib;
	app->int_color = app->color.x + app->color.y + app->color.z;
}

float	length_squared(t_vec e)
{
	return (e.x * e.x + e.y * e.y + e.z * e.z);
}

float length(t_vec e)
{
	return (sqrt(length_squared(e)));
}

t_vec	unit_vector(t_vec v)
{
	return (v_div(v, length(v)));
}

t_vec	ray_color(t_vec orig, t_vec dir)
{
	t_vec unit_direction = unit_vector(dir);
	float t = 0.5 * (unit_direction.y + 1.0);
	t_vec a= make_v(1.0);
	t_vec b; b.x = 0.5; b.y = 0.7; b.z = 1.0;
	return v_add(v_mul_n(a, 1.0 - t), v_mul_n(b, t));
}

#include <stdio.h>

int	main()
{
	// Image
	const float	aspect_ratio = 16.0 / 9.0;
	const int	image_width = 400;
	const int	image_height = image_width / aspect_ratio;

	// Start mlx
	t_mlx	*app;
	if (!(app = (t_mlx*)malloc(sizeof(t_mlx))))
		return (-1);
	app->mlx_ptr = mlx_init();
	app->win_ptr = mlx_new_window(app->mlx_ptr, 800, 600, "raytracer");
	app->img_ptr = mlx_new_image(app->mlx_ptr, image_width, image_height);
	app->data = (int *)mlx_get_data_addr(app->img_ptr, &app->bpp, &app->size_l, &app->endian);


	// Camera
	float	viewport_height = 2.0;
	float	viewport_width = aspect_ratio * viewport_height;
	float	focal_length = 1.0;

	t_vec	origin = {0,0,0};
	t_vec	horizontal = {viewport_width, 0, 0};
	t_vec	vertical = {0, viewport_height, 0}; t_vec any = {0, 0, focal_length};
	t_vec	lower_left_corner = v_sub(origin, v_add(v_add(v_div(horizontal, 2), v_div(vertical, 2)), any));

	// Render
	
	int j = 0;
	while (j < image_height)
	{
		int i = 0;
		while (i < image_width)
		{	
			float u = (double)i / (image_width - 1);
			float v = (image_height - (double)j - 1) / (image_height - 1);
			
			t_vec a = origin;
			t_vec b = v_add(lower_left_corner, v_add(v_mul_n(horizontal, u), v_mul_n(v_sub(vertical, origin), v)));
			t_vec pixel_color = ray_color(a, b);
			write_color(app, pixel_color);
			mlx_pixel_put(app->mlx_ptr, app->win_ptr, i, jj, app->int_color);	
			app->data[jj * image_width + i] = app->int_color;	
			++i;
		}
		++j;
	}
	mlx_put_image_to_window (app->mlx_ptr, app->win_ptr, app->img_ptr, 0, 0);
	mlx_loop(app->mlx_ptr);
}
```

</div>
</details>
<br>


## 5. 구를 더하기
레이 트레이서에 오브젝트를 한 개 추가해봅시다. 광선이 구에 닿는지 여부를 계산하는 것은 매우 간단하기 때문에 사람들은 종종 광선 추적기에서 구를 사용합니다.

### 5.1 광선-구 교차
반지름 𝑅의 원점을 중심으로 하는 구에 대한 방정식은 𝑥^2 + 𝑦^2 + 𝑧^2 = 𝑅^2 입니다.
달리 말하면,

~~~
if (주어진 점(𝑥, 𝑦, 𝑧)이 구체에 위에(on) 있으면)
	𝑥^2 + 𝑦^2 + 𝑧^2 = 𝑅^2
if (주어진 점 (𝑥, 𝑦, 𝑧)이 구 "안에" 있으면)
	𝑥^2 + 𝑦^2 + 𝑧^2 < 𝑅^2
if (주어진 점 (𝑥, 𝑦, 𝑧)이 구 "밖에" 있으면)
	𝑥^2 + 𝑦^2 + 𝑧^2 > 𝑧^2
~~~

입니다.

구 중심이 (𝐶𝑥, 𝐶𝑦, 𝐶𝑧)에 있으면 더 못생겨집니다.

> (𝑥 − 𝐶𝑥)^2 + (𝑦 − 𝐶𝑦)^2 + (𝑧 − 𝐶𝑧)^2 = 𝑟^2

그래픽스에서는 거의 항상 공식이 벡터로 표시되기를 원하므로 모든 x/y/z 항목은 `vec3` 클래스의 내부에 있습니다. 중심 𝐂 = (𝐶𝑥, 𝐶𝑦, 𝐶𝑧)에서 𝐏 = (𝑥, 𝑦, 𝑧) 지점까지의 벡터는 (𝐏 − 𝐂)이므로

> (𝐏 − 𝐂) * (𝐏 − 𝐂) = (𝑥 − 𝐶𝑥)^2 + (𝑦 − 𝐶𝑦)^2 + (𝑧 − 𝐶𝑧)^2

따라서 벡터 형태의 구의 방정식은 다음과 같습니다.

> (𝐏 − 𝐂) * (𝐏 − 𝐂) = 𝑟^2

우리는 이것을 "이 방정식을 만족하는 모든 점 𝐏가 구체에 있다(on the sphere)"라고 읽을 수 있습니다. 우리는 우리의 광선 𝐏(𝑡) = 𝐀 + 𝑡𝐛이 구체에 어느 곳에서건 닿았는지 알고 싶습니다. 구에 부딪히면 𝐏(𝑡)이 구 방정식을 충족하는 𝑡가 있습니다. 그래서 우리는 이것이 참인 𝑡을 찾고 있습니다:

> (𝐏(𝑡) − 𝐂) ⋅ (𝐏(𝑡) − 𝐂) = 𝑟^2

또는 광선 𝐏(𝑡)의 전체 형태를 전개하는 모든 𝑡를 찾고 있습니다:

> (𝐀 + 𝑡𝐛 − 𝐂) * (𝐀 + 𝑡𝐛 − 𝐂) = 𝑟^2

벡터 대수의 규칙이 여기서 우리가 원하는 모든 것입니다. 이 방정식을 전개하고 모든 항을 왼쪽으로 이동하면 다음과 같은 결과를 얻을 수 있습니다:

> 𝑡^2𝐛 * 𝐛 + 2𝑡𝐛 * (𝐀 − 𝐂) + (𝐀 − 𝐂) * (𝐀 − 𝐂) − 𝑟^2 = 0

그 방정식의 벡터와 𝑟는 모두 일정하고(상수) 알려져 있습니다(기지수). 미지수는 𝑡이고 방정식은 아마도 고등학교 수학 수업에서 보았던 것처럼 2차입니다. 𝑡에 대해 풀 수 있으며 양수(실제 솔루션 2 개를 의미), 음수(실수 솔루션 없음) 또는 0(실제 솔루션 1 개를 의미)인 제곱근 부분이 있습니다. 그래픽에서 대수는 거의 항상 기하학과 매우 직접적으로 관련됩니다. 우리가 가진 것은 :

![](https://raytracing.github.io/images/fig-1.04-ray-sphere.jpg)
> 그림 4: 광선-구 교차 결과

### 5.2 첫번째 레이 트레이싱 이미지 생성

그 수학을 우리 프로그램에 하드코딩하면 z 축의 −1에 배치한 작은 구에 닿는 픽셀을 빨간색으로 칠해 테스트 할 수 있습니다:

```C++
bool hit_sphere(const point3& center, double radius, const ray& r) {
    vec3 oc = r.origin() - center;
    auto a = dot(r.direction(), r.direction());
    auto b = 2.0 * dot(oc, r.direction());
    auto c = dot(oc, oc) - radius*radius;
    auto discriminant = b*b - 4*a*c;
    return (discriminant > 0);
}

color ray_color(const ray& r) {
    if (hit_sphere(point3(0,0,-1), 0.5, r))
        return color(1, 0, 0);
    vec3 unit_direction = unit_vector(r.direction());
    auto t = 0.5*(unit_direction.y() + 1.0);
    return (1.0-t)*color(1.0, 1.0, 1.0) + t*color(0.5, 0.7, 1.0);
}
```
> 목록 10: [main.cc] 빨간 구 렌더링


우리가 얻는 것은 다음과 같습니다:

![](https://raytracing.github.io/images/img-1.03-red-sphere.png)
> 이미지 3: 간단한 빨간색 구

이제 이것은 모든게 부족하지만 - 음영, 반사 광선, 하나 이상의 오브젝트 같은 거.. - 그래도 우리는 시작 단계에 비하면 절반쯤 가까이 왔습니다! 한 가지 알아야 할 것은 광선이 구에 닿는지 여부를 테스트했지만, 𝑡 < 0 솔루션이 잘 작동한다는 것입니다. 만약 구 중심을 𝑧 = + 1로 변경한다면 당신은 정확히 동일한 그림을 얻을 수 있을 것입니다. 당신이 당신 뒤에 있는 것들을 볼 수 있기 때문입니다. 이것은 기능이 아닙니다! 이 문제는 다음에 수정하겠습니다.


#### 5.2.1 이 것 을 C 코 드 로 ..

<img width="912" alt="스크린샷 2020-08-16 오후 5 40 09" src="https://user-images.githubusercontent.com/53321189/90330429-95affe80-dfe7-11ea-84c7-208f9aed0ba9.png">

<details>
<summary> <b> 🛠 소스코드 </b>  </summary>
<div markdown="1">
	
```C
#include "mlx.h"
#include <stdlib.h>
#include <math.h>

typedef struct s_vec
{
	float	x;
	float	y;
	float	z;
} t_vec;

typedef	struct s_mlx
{
	void *mlx_ptr;
	void *win_ptr;
	void *img_ptr;
	int  *data;
	int		bpp;
	int	size_l;
	int	endian;

	t_vec	color;
	int		int_color;

} t_mlx;

t_vec	make_v(float n)
{
	t_vec v;
	v.x = n;
	v.y = n;
	v.z = n;

	return (v);
}

t_vec	v_mul_n(t_vec v1, float n)
{
	t_vec	result;

	result.x = v1.x * n;
	result.y = v1.y * n;
	result.z = v1.z * n;
	return (result);
}

t_vec	v_mul(t_vec v1, t_vec v2)
{
	t_vec	result;

	result.x = v1.x * v2.x;
	result.y = v1.y * v2.y;
	result.z = v1.z * v2.z;
	return (result);
}

float	dot(t_vec v1, t_vec v2)
{
	return (v1.x * v2.x + v1.y * v2.y + v1.z * v2.z);
}

t_vec	v_sub(t_vec v1, t_vec v2)
{
	t_vec	result;

	result.x = v1.x - v2.x;
	result.y = v1.y - v2.y;
	result.z = v1.z - v2.z;
	return (result);
}

t_vec	v_add(t_vec v1, t_vec v2)
{
	t_vec	result;

	result.x = v1.x + v2.x;
	result.y = v1.y + v2.y;
	result.z = v1.z + v2.z;
	return (result);
}

t_vec	v_div(t_vec v1, float d)
{
	t_vec	result;

	result.x = v1.x / d;
	result.y = v1.y / d;
	result.z = v1.z / d;
	return (result);
}

t_vec	origin(t_vec orig, t_vec dir)
{
	return (orig);
}

t_vec	direction(t_vec orig, t_vec dir)
{
	return (dir);
}
/*
t_vec	ray(t_vec orig, t_vec dir)
{

}

t_vec	at(t_vec orig, t_vec dir, double t)
{
	return (orig + t * dir);
}
*/

void	write_color(t_mlx *app, t_vec c)
{
	int	ir = 255.999 * c.x;
	int	ig = 255.999 * c.y;
	int	ib = 255.999 * c.z;

	app->color.x = ir * 256 * 256;
	app->color.y = ig * 256;
	app->color.z = ib;
	app->int_color = app->color.x + app->color.y + app->color.z;
}

float	length_squared(t_vec e)
{
	return (e.x * e.x + e.y * e.y + e.z * e.z);
}

float length(t_vec e)
{
	return (sqrt(length_squared(e)));
}

t_vec	unit_vector(t_vec v)
{
	return (v_div(v, length(v)));
}

int	hit_sphere(t_vec center, double radius, t_vec origin, t_vec direction)
{
	t_vec oc = v_sub(origin, center);
	float a = dot(direction, direction);
	float b = 2.0 * dot(oc, direction);
	float c = dot(oc, oc) - radius * radius;
	float discriminant = b * b - 4 * a * c;
	return (discriminant > 0 ? 1 : 0);
}

t_vec	ray_color(t_vec orig, t_vec dir)
{
	t_vec sphere = {0, 0, -1};
	t_vec sphere_color = {1, 0, 0};
	if (hit_sphere(sphere, 0.5, orig, dir))
		return (sphere_color);
	t_vec unit_direction = unit_vector(dir);
	float t = 0.5 * (unit_direction.y + 1.0);
	t_vec a= make_v(1.0);
	t_vec b; b.x = 0.5; b.y = 0.7; b.z = 1.0;
	return (v_add(v_mul_n(a, 1.0 - t), v_mul_n(b, t)));
}

#include <stdio.h>

int	main()
{
	// Image
	const float	aspect_ratio = 16.0 / 9.0;
	const int	image_width = 400;
	const int	image_height = image_width / aspect_ratio;

	// Start mlx
	t_mlx	*app;
	if (!(app = (t_mlx*)malloc(sizeof(t_mlx))))
		return (-1);
	app->mlx_ptr = mlx_init();
	app->win_ptr = mlx_new_window(app->mlx_ptr, 800, 600, "raytracer");
	app->img_ptr = mlx_new_image(app->mlx_ptr, image_width, image_height);
	app->data = (int *)mlx_get_data_addr(app->img_ptr, &app->bpp, &app->size_l, &app->endian);


	// Camera
	float	viewport_height = 2.0;
	float	viewport_width = aspect_ratio * viewport_height;
	float	focal_length = 1.0;

	t_vec	origin = {0,0,0};
	t_vec	horizontal = {viewport_width, 0, 0};
	t_vec	vertical = {0, viewport_height, 0}; t_vec any = {0, 0, focal_length};
	t_vec	lower_left_corner = v_sub(origin, v_add(v_add(v_div(horizontal, 2), v_div(vertical, 2)), any));

	// Render

	int jj = 0;
	int j = image_height - 1;
	while (j >= 0 && jj < image_height)
	{
		int i = 0;
		while (i < image_width)
		{

			float u = (double)i / (image_width - 1);
			float v = (double)j / (image_height - 1);

			t_vec a = origin;
			t_vec b = v_add(lower_left_corner, v_add(v_mul_n(horizontal, u), v_mul_n(v_sub(vertical, origin), v)));
			t_vec pixel_color = ray_color(a, b);
			write_color(app, pixel_color);
			mlx_pixel_put(app->mlx_ptr, app->win_ptr, i, jj, app->int_color);
//			app->data[jj * 400 + i] = app->int_color;

			++i;
		}
		--j;
		++jj; //예제의 점 찍히는 순서를 똑같이 하려고 jj를 따로 만들었음..
	}
//	mlx_put_image_to_window (app->mlx_ptr, app->win_ptr, app->img_ptr, 0, 0);
	mlx_loop(app->mlx_ptr);
}
```

</div>
</details>
<br>





## 6. 표면 법선과 여러 오브젝트

### 6.1 표면 법선을 사용한 음영
먼저 음영 처리를 할 수 있도록 표면 법선을 만들어 보겠습니다. 이것은 교차점에서 표면에 수직인 벡터입니다. 법선에 대해 두 가지 설계 결정을 내릴 수 있습니다. 첫 번째는 이러한 법선이 단위 길이인지 여부입니다. 음영 처리에 편리하므로 예라고 말 하겠지만 코드에서는 적용하지 않겠습니다. 이것은 미묘한 버그를 허용 할 수 있으므로 대부분의 디자인 결정과 마찬가지로 이것이 개인적 선호도라는 점을 유의하십시오. 구의 경우 바깥쪽 법선은 히트 포인트에서 중심을 뺀 방향입니다:

![](https://raytracing.github.io/images/fig-1.05-sphere-normal.jpg)
> 그림 5: 표면 법선 지오메트리

지구로 치면 이것은 지구 중심에서 당신까지의 벡터가 똑바로 위를 향하고 있음을 의미합니다. 이제 코드에 넣어서 음영 처리를 하겠습니다. 아직 조명이나 뭐가 아무것도 없으므로 색상 맵으로 법선을 시각화 해봅시다. 법선을 시각화하는 데 사용되는 일반적인 트릭은(𝐧이 단위 길이 벡터unit length vector라고 가정하는 것이 쉽고 다소 직관적이기 때문에 — 그러므로 각 구성 요소는 -1과 1 사이에 있음) 각 구성 요소를 0에서 1 사이의 간격에 매핑 한 다음, x/y/z ~ r/g/b를 매핑하는 것입니다. 노멀? 법선?의 경우 히트 여부뿐만 아니라 히트 포인트가 필요합니다. 가장 가까운 히트 포인트(𝑡의 최소값)를 가정해 보겠습니다. 이러한 코드 변경을 통해 𝐧을 계산하고 시각화 할 수 있습니다:

```C++
double hit_sphere(const point3& center, double radius, const ray& r) {
    vec3 oc = r.origin() - center;
    auto a = dot(r.direction(), r.direction());
    auto b = 2.0 * dot(oc, r.direction());
    auto c = dot(oc, oc) - radius*radius;
    auto discriminant = b*b - 4*a*c;
    if (discriminant < 0) {
        return -1.0;
    } else {
        return (-b - sqrt(discriminant) ) / (2.0*a);
    }
}

color ray_color(const ray& r) {
    auto t = hit_sphere(point3(0,0,-1), 0.5, r);
    if (t > 0.0) {
        vec3 N = unit_vector(r.at(t) - vec3(0,0,-1));
        return 0.5*color(N.x()+1, N.y()+1, N.z()+1);
    }
    vec3 unit_direction = unit_vector(r.direction());
    t = 0.5*(unit_direction.y() + 1.0);
    return (1.0-t)*color(1.0, 1.0, 1.0) + t*color(0.5, 0.7, 1.0);
}
```
> 목록 11: [main.cc] 구에 표면 법선 렌더링

그러면 이 그림이 나옵니다:


![](https://raytracing.github.io/images/img-1.04-normals-sphere.png)
> 이미지 4: 법선 벡터에 따라 색상이 지정된 구

### 6.2 광선-구 교차 코드 단순화

광선-구 방정식을 다시 살펴 보겠습니다:

```C++
double hit_sphere(const point3& center, double radius, const ray& r) {
    vec3 oc = r.origin() - center;
    auto a = dot(r.direction(), r.direction());
    auto b = 2.0 * dot(oc, r.direction());
    auto c = dot(oc, oc) - radius*radius;
    auto discriminant = b*b - 4*a*c;

    if (discriminant < 0) {
        return -1.0;
    } else {
        return (-b - sqrt(discriminant) ) / (2.0*a);
    }
}
```
> 목록 12: [main.cc] Ray-sphere intersection code (before)

첫째, 자기 자신과 함께 찍혀진 벡터는 해당 벡터의 제곱의 길이와 같습니다.
둘째, `b`에 대한 방정식이 2의 인수를 갖는지 확인하십시오. 𝑏 = 2ℎ 인 경우 2차 방정식이 어떻게되는지 고려하십시오:

<img width="253" alt="스크린샷 2020-08-19 오후 9 26 51 1" src="https://user-images.githubusercontent.com/53321189/90634522-c6e83300-e262-11ea-90fb-5e4a9c98d3cb.png">

이러한 관찰을 사용하여 이제 구-교차 코드를 다음과 같이 단순화 할 수 있습니다:

```C++
double hit_sphere(const point3& center, double radius, const ray& r) {
    vec3 oc = r.origin() - center;
    auto a = r.direction().length_squared();
    auto half_b = dot(oc, r.direction());
    auto c = oc.length_squared() - radius*radius;
    auto discriminant = half_b*half_b - a*c;

    if (discriminant < 0) {
        return -1.0;
    } else {
        return (-half_b - sqrt(discriminant) ) / a;
    }
}
```
> 목록 13: [main.cc] Ray-sphere intersection code (after)

<details>
<summary> <b> 🛠 목록 13: [main.cc] Ray-sphere intersection code (after)를 C로 바꾸면..  </b>  </summary>
<div markdown="1">

```C++
double	hit_sphere(t_vec center, double radius, t_vec origin, t_vec direction)
{
	t_vec oc = v_sub(origin, center);
	float a = length_squared(direction);
	float half_b = dot(oc, direction);
	float c = length_squared(oc) - radius * radius;
	float discriminant = half_b * half_b - a * c;
	if (discriminant < 0)
		return (-1.0);
	else
		return ((-half_b - sqrt(discriminant)) / a);
}
```
</div>
</details>
<br>

### 6.3 Hittable 오브젝트에 대한 추상화
이제 여러 구체는 어떻습니까? 구의 배열을 갖고 싶은 유혹이 있지만, 매우 깨끗한 해결책은 광선이 맞을 수 있는 모든 것에 대해 "추상 클래스"를 만들고, 구와 구 목록을 모두 맞출 수 있는 것으로 만드는 것입니다. 이 클래스를 호출해야하는 것은 난처한 것입니다. "객체 지향"프로그래밍이 아니라면 "객체"라고 부르는 것이 좋습니다. "Surface"가 자주 사용되며 약점은 볼륨이 필요할 수 있다는 것입니다. "hittable"은 이들을 통합하는 멤버 함수를 강조합니다. 나는 이것들 중 어느 것도 좋아하지 않지만 “hittable”로 갈 것입니다.

이 적중 가능 추상 클래스에는 광선을 받는 적중 함수가 있습니다. 대부분의 광선 추적기는 𝑡𝑚𝑖𝑛에 유효한 간격을 𝑡𝑚𝑎𝑥에 추가하는 것이 편리하다는 것을 알았습니다. 초기 광선의 경우 이것은 양수 𝑡이지만 앞으로 보겠지만 코드에서 𝑡𝑚𝑖𝑛에서 𝑡𝑚𝑎𝑥까지의 간격을 갖도록 코드의 일부 세부 정보를 도울 수 있습니다. 한 가지 설계 질문은 우리가 무언가를 맞히면 법선을 계산하는 것과 같은 일을 할 것인지 여부입니다. 우리는 검색을 할 때 더 가까운 무언가에 부딪힐 수 있으며, 가장 가까운 것의 법선만 필요합니다. 간단한 솔루션으로 가서 어떤 구조에 저장할 물건 묶음을 계산할 것입니다. 추상 클래스는 다음과 같습니다.

XXXXXXX.............. 이거는 나중에 보자............. 

### 6.4 앞면 vs 뒷면
법선에 대한 두 번째 설계 결정은 항상 지적해야하는지 여부입니다. 현재 발견된 법선은 항상 중앙에서 교차점(법선이 가리키는)방향에 있습니다. 광선이 외부에서 구와 교차하는 경우 법선은 광선을 향합니다. 광선이 내부에서 구와 교차하면 법선(항상 지적)이 광선을 가리킵니다. 또는 법선이 항상 광선을 향하도록 할 수 있습니다. 광선이 구 밖에 있으면 법선이 바깥쪽을 가리키고 광선이 구 안에 있으면 법선이 안쪽을 가리킵니다.

![](https://raytracing.github.io/images/fig-1.06-normal-sides.jpg)

그림 6: 구 표면 법선 지오메트리의 가능한 방향들 

우리는 결국 광선이 나오는 표면의 측면을 결정하기를 원하기 때문에 이러한 가능성 중 하나를 선택해야합니다. 양면 종이의 텍스트와 같이 각면에서 다르게 렌더링되는 개체 또는 유리 공과 같이 내부와 외부가있는 개체에 중요합니다.

법선이 항상 가리 키도록 결정한 경우 색상을 지정할 때 광선이 어느쪽에 있는지 결정해야합니다. 광선과 법선을 비교하여이를 알아낼 수 있습니다. 광선과 법선면이 같은 방향이면 광선은 물체 내부에 있고 광선과 법선면이 반대 방향이면 광선은 물체 외부에 있습니다. 이는 두 벡터의 내적을 취하여 결정할 수 있습니다. 여기서 점이 양수이면 광선이 구 안에 있습니다.



### 6.5 Hittable 오브젝트 목록
### 6.6 C++의 몇 가지 새로운 기능
### 6.7 공통 상수 및 유틸리티 함수

<img width="912" alt="스크린샷 2020-08-17 오후 11 31 38" src="https://user-images.githubusercontent.com/53321189/90409277-ed339480-e0e3-11ea-9387-e77419c67135.png">


<details>
<summary> <b> 🛠 소스코드 </b>  </summary>
<div markdown="1">
	

```C
#include "mlx.h"
#include <stdlib.h>
#include <math.h>

typedef struct s_vec
{
	float	x;
	float	y;
	float	z;
} t_vec;

typedef	struct s_mlx
{
	void *mlx_ptr;
	void *win_ptr;
	void *img_ptr;
	int  *data;
	int		bpp;
	int	size_l;
	int	endian;

	t_vec	color;
	int		int_color;

} t_mlx;

t_vec	make_v(float n)
{
	t_vec v;
	v.x = n;
	v.y = n;
	v.z = n;

	return (v);
}

t_vec	v_mul_n(t_vec v1, float n)
{
	t_vec	result;

	result.x = v1.x * n;
	result.y = v1.y * n;
	result.z = v1.z * n;
	return (result);
}

t_vec	v_mul(t_vec v1, t_vec v2)
{
	t_vec	result;

	result.x = v1.x * v2.x;
	result.y = v1.y * v2.y;
	result.z = v1.z * v2.z;
	return (result);
}

float	dot(t_vec v1, t_vec v2)
{
	return (v1.x * v2.x + v1.y * v2.y + v1.z * v2.z);
}

t_vec	v_sub(t_vec v1, t_vec v2)
{
	t_vec	result;

	result.x = v1.x - v2.x;
	result.y = v1.y - v2.y;
	result.z = v1.z - v2.z;
	return (result);
}

t_vec	v_add(t_vec v1, t_vec v2)
{
	t_vec	result;

	result.x = v1.x + v2.x;
	result.y = v1.y + v2.y;
	result.z = v1.z + v2.z;
	return (result);
}

t_vec	v_div(t_vec v1, float d)
{
	t_vec	result;

	result.x = v1.x / d;
	result.y = v1.y / d;
	result.z = v1.z / d;
	return (result);
}

t_vec	origin(t_vec orig, t_vec dir)
{
	return (orig);
}

t_vec	direction(t_vec orig, t_vec dir)
{
	return (dir);
}

void	write_color(t_mlx *app, t_vec c)
{
	int	ir = 255.999 * c.x;
	int	ig = 255.999 * c.y;
	int	ib = 255.999 * c.z;

	app->color.x = ir * 256 * 256;
	app->color.y = ig * 256;
	app->color.z = ib;
	app->int_color = app->color.x + app->color.y + app->color.z;
}

float	length_squared(t_vec e)
{
	return (e.x * e.x + e.y * e.y + e.z * e.z);
}

float length(t_vec e)
{
	return (sqrt(length_squared(e)));
}

t_vec	unit_vector(t_vec v)
{
	return (v_div(v, length(v)));
}

t_vec	at(t_vec orig, t_vec dir, double t)
{
	return (v_add(orig, v_mul_n(dir, t)));
}

double	hit_sphere(t_vec center, double radius, t_vec origin, t_vec direction)
{
	t_vec oc = v_sub(origin, center);
	float a = dot(direction, direction);
	float b = 2.0 * dot(oc, direction);
	float c = dot(oc, oc) - radius * radius;
	float discriminant = b * b - 4 * a * c;
	if (discriminant < 0)
		return (-1.0);
	else
		return ((-b - sqrt(discriminant)) / (2.0 * a));
}

t_vec	ray_color(t_vec orig, t_vec dir)
{
	t_vec sphere = {0, 0, -1};
	float t = hit_sphere(sphere, 0.5, orig, dir);
	if (t > 0.0)
	{
		t_vec N = unit_vector(v_sub(at(orig, dir, t), sphere));
		t_vec color;
		color.x = N.x + 1;
		color.y = N.y + 1;
		color.z = N.z + 1;

		return (v_mul_n(color, 0.5));

	}
//	t_vec sphere_color = {1, 0, 0};
//	if (hit_sphere(sphere, 0.5, orig, dir))
//		return (sphere_color);
	t_vec unit_direction = unit_vector(dir);
	t = 0.5 * (unit_direction.y + 1.0);
	t_vec a= make_v(1.0);
	t_vec b; b.x = 0.5; b.y = 0.7; b.z = 1.0;
	return (v_add(v_mul_n(a, 1.0 - t), v_mul_n(b, t)));
}

#include <stdio.h>

int	main()
{
	// Image
	const float	aspect_ratio = 16.0 / 9.0;
	const int	image_width = 400;
	const int	image_height = image_width / aspect_ratio;

	// Start mlx
	t_mlx	*app;
	if (!(app = (t_mlx*)malloc(sizeof(t_mlx))))
		return (-1);
	app->mlx_ptr = mlx_init();
	app->win_ptr = mlx_new_window(app->mlx_ptr, 800, 600, "raytracer");
	app->img_ptr = mlx_new_image(app->mlx_ptr, image_width, image_height);
	app->data = (int *)mlx_get_data_addr(app->img_ptr, &app->bpp, &app->size_l, &app->endian);


	// Camera
	float	viewport_height = 2.0;
	float	viewport_width = aspect_ratio * viewport_height;
	float	focal_length = 1.0;

	t_vec	origin = {0,0,0};
	t_vec	horizontal = {viewport_width, 0, 0};
	t_vec	vertical = {0, viewport_height, 0}; t_vec any = {0, 0, focal_length};
	t_vec	lower_left_corner = v_sub(origin, v_add(v_add(v_div(horizontal, 2), v_div(vertical, 2)), any));

	// Render

	int jj = 0;
	int j = image_height - 1;
	while (j >= 0 && jj < image_height)
	{
		int i = 0;
		while (i < image_width)
		{

			float u = (double)i / (image_width - 1);
			float v = (double)j / (image_height - 1);

			t_vec a = origin;
			t_vec b = v_add(lower_left_corner, v_add(v_mul_n(horizontal, u), v_mul_n(v_sub(vertical, origin), v)));
			t_vec pixel_color = ray_color(a, b);
			write_color(app, pixel_color);
			mlx_pixel_put(app->mlx_ptr, app->win_ptr, i, jj, app->int_color);
//			app->data[jj * 400 + i] = app->int_color;

			++i;
		}
		--j;
		++jj; //예제의 점 찍히는 순서를 똑같이 하려고 jj를 따로 만들었음..
	}
//	mlx_put_image_to_window (app->mlx_ptr, app->win_ptr, app->img_ptr, 0, 0);
	mlx_loop(app->mlx_ptr);
}
```
</div>
</details>
<br>

## 7. 안티 앨리어싱
### 7.1 난수 유틸리티 몇 가지
### 7.2 여러? 다중? 샘플로 픽셀 생성

<details>
<summary> <b> 🛠 소스코드 </b>  </summary>
<div markdown="1">
	
	
</div>
</details>
<br>

## 8. 확산(diffuse) 재료

### 8.1 단순 확산 재료
### 8.2 Child ray 수 제한
### 8.3 정확한 색상 강도를 위한 감마 보정 사용
### 8.4 Shadow Acne 수정
### 8.5 진정한 Lambertian 반사
### 8.6 대안적 확산 공식

## 9. 금속
### 9.1 재료에 대한 추상 클래스
### 9.2 광선-객체 교차를 설명하기 위한 데이터 구조
### 9.3 빛의 산란과 반사율 모델링
### 9.4 반사광 반사
### 9.5 금속 구체가 있는 장면
### 9.6 퍼지 반사

## 10. 유전체
### 10.1 굴절
### 10.2 스넬의 법칙
### 10.3 내부 전반사
### 10.4 Schlick 근사치
### 10.5 중공 유리Hollow Glass 구 모델링

## 11. 포지셔닝 가능한 카메라
### 11.1 카메라보기 지오메트리
### 11.2 카메라 위치와 방향 지정하기

## 12. 디 포커스 블러
### 12.1 얇은 렌즈 근사치
### 12.2 샘플 광선 생성

## 13 다음은 어디?
### 13.1 최종 렌더링
### 13.2 다음 단계

## 14. 감사의 말
## 15. 이 책을 인용했어요
### 15.1 기본 데이터
### 15.2 스니펫
### 15.2.1 마크다운
### 15.2.2 HTML
### 15.2.3 LaTeX 및 BibTex
### 15.2.4 BibLaTeX
### 15.2.5 IEEE
### 15.2.6 MLA :
