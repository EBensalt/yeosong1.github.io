# Ray Tracing in One Weekend - by Peter Shirley
아래 내용은 [https://raytracing.github.io/books/RayTracingInOneWeekend.html#metal/mirroredlightreflection](https://raytracing.github.io/books/RayTracingInOneWeekend.html#metal/mirroredlightreflection)의 내용을 한글 번역하고, 추가로 C언어로 바꾼 코드를 첨부한 것이다.

## 1. 개요
(개요는 그냥 번역기 돌린 그대로 입니당)
저는 수년 동안 많은 그래픽 수업을 가르쳤습니다. 모든 코드를 작성해야하기 때문에 종종 수업들을 레이 트레이싱으로 하는데, API 없이도 멋진 이미지를 얻을 수 있습니다. 가능한 한 빨리 멋진 프로그램에 참여할 수 있도록 코스 노트를 방법에 적용하기로 결정했습니다. 완전한 기능을 갖춘 광선 추적기는 아니지만 영화에서 광선 추적을 필수 요소로 만든 간접 조명이 있습니다. 다음 단계를 따르십시오. 생산하는 광선 추적기의 아키텍처는 흥미를 느끼고 그것을 추구하려는 경우 더 광범위한 광선 추적기로 확장하는 데 좋습니다.

누군가 "레이 트레이싱"이라고 말하면 많은 것을 의미 할 수 있습니다. 제가 설명하려고하는 것은 기술적으로 경로 추적자이며 상당히 일반적인 것입니다. 코드는 매우 간단하지만 (컴퓨터가 작업을 수행하도록 합니다!) 만들 수 있는 이미지에 매우 만족할 것입니다.

몇 가지 디버깅 팁과 함께 내가하는 순서대로 광선 추적기를 작성하는 과정을 안내합니다. 마지막에는 멋진 이미지를 생성하는 레이 트레이서가 생깁니다. 주말에이 일을 할 수 있어야합니다. 더 오래 걸리더라도 걱정하지 마십시오. C++을 운전 언어로 사용하지만 그럴 필요는 없습니다. 그러나 빠르고 휴대 가능하며 대부분의 프로덕션 영화 및 비디오 게임 렌더러가 C++로 작성 되었기 때문에 그렇게하는 것이 좋습니다. C++의 대부분의 "최신 기능"은 피하지만 상속 및 연산자 오버로딩은 광선 추적기가 전달하기에 너무 유용합니다. 나는 온라인으로 코드를 제공하지 않지만 코드는 실제이며 vec3 클래스의 몇 가지 간단한 연산자를 제외하고는 모두 보여줍니다. 나는 그것을 배우기 위해 코드를 타이핑하는 것에 큰 신념을 가지고 있지만, 코드를 사용할 수있을 때 그것을 사용하므로 코드를 사용할 수 없을 때만 내가 설교하는 것을 연습합니다. 그러니 묻지 마세요!

나는 180이 한 일이 재미 있기 때문에 마지막 부분을 남겼습니다. 몇몇 독자들은 우리가 코드를 비교할 때 도움이 된 미묘한 오류로 끝났습니다. 따라서 코드를 입력하십시오. 하지만 내 것을 보고 싶다면 다음 위치에 있습니다:

[https://github.com/RayTracing/raytracing.github.io/](https://github.com/RayTracing/raytracing.github.io/)

나는 벡터(내적과 벡터 덧셈과 같은)에 약간 익숙하다고 가정합니다. 잘 모르겠다면 약간의 검토를 하세요.
검토가 필요하거나 처음으로 배우려면 Marschner와 저의 그래픽 텍스트, Foley, Van Dam 등 또는 McGuire의 그래픽 코덱을 확인하십시오.

문제가 발생하거나 누군가에게 보여주고 싶은 멋진 일을한다면 ptrshrl@gmail.com으로 이메일을 보내주세요.

이 책과 관련된 블로그 [https://in1weekend.blogspot.com/](https://in1weekend.blogspot.com/)의 추가 자료 및 리소스 링크를 포함하여 책과 관련된 사이트를 유지 관리 할 것입니다.

이 프로젝트에 도움을 주신 모든 분들께 감사드립니다. 이 책의 끝에 있는 [감사의 말](##14._감사의_말) 섹션에서 찾을 수 있습니다.

시작합시다!
     
## 2. 이미지 출력
### 2.1 PPM 이미지 포맷
렌더러를 시작할 때마다 이미지를 볼 수 있는 방법이 필요합니다. 가장 간단한 방법은 파일에 쓰는 것입니다. 문제는 형식이 너무 많다는 것입니다. 그 중 다수는 복잡합니다. 저는 항상 일반 텍스트 ppm 파일로 시작합니다. 다음은 Wikipedia의 멋진 설명입니다.

![](https://raytracing.github.io/images/fig-1.01-ppm.jpg) 그림 1: PPM Example

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
목록 1 : [main.cc] 첫 번째 이미지 만들기

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

이미지 1: 첫번째 PPM image

만세! 이것이 그래픽스의 “hello world”입니다. 이미지가 저렇게 보이지 않으면 텍스트 편집기에서 출력 파일을 열고 어떻게 보이는지 확인합니다. 대충 다음과 같이 시작해야합니다: 

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

목록 2: 첫번째 이미지 아웃풋

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


이거를 mlx 윈도우에서 나오게 해보자..

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
//	app.img_ptr = mlx_new_image(app.mlx_ptr, image_width, image_height);
//	app.data = (int *)mlx_get_data_addr(app.img_ptr, &app.bpp, &app.size_l, &app.endian);

	int jj = 0;
	int j = image_height - 1;
	while (j >= 0 && jj < image_height)
	{
		int i = 0;
		while (i < image_width)
		{
			float r = (double)i / (image_width - 1);
			float g = (double)j / (image_height - 1);
			float b = 0.25;

			int	ir = 255.999 * r;
			int	ig = 255.999 * g;
			int	ib = 255.999 * b;

			app.color[0] = ir * 256 * 256;
			app.color[1] = ig * 256;
			app.color[2] = ib;

			int color = app.color[0] + app.color[1] + app.color[2];

			mlx_pixel_put(app.mlx_ptr, app.win_ptr, i, jj, color);
//			app.data[jj * 255 + i] = mlx_get_color_value(app.mlx_ptr, color);

			++i;
		}
		--j;
		++jj; //예제의 점 찍히는 순서를 똑같이 하려고 jj를 따로 만들었음..
	}
//	mlx_put_image_to_window ( app.mlx_ptr, app.win_ptr, app.img_ptr, 0, 0 );
	mlx_loop(app.mlx_ptr);
}
```

<img width="812" alt="스크린샷 2020-08-11 오후 5 49 59" src="https://user-images.githubusercontent.com/53321189/89877469-1d1dfc00-dbfb-11ea-93c6-dff71d96da85.png">

나중에 성능을 위해서 data에 다 그리고 한번에 불러서 윈도우에 띄워야하는데(지금처럼 점 1개씩 즉석에서 찍게 짜면, 나중에 속도도 느리고 다중 개체 처리가 복잡해짐..) 근데 아직 안고쳤어요. 그래서 이거는 아직 0,0 부터 좌-우 상-하 순서로 1개씩 점을 찍는 코드입니다. 


### 2.3 진행률 표시기 추가

## 3. vec3 class
### 3.1 변수와 메소드
### 3.2 vec3 Utility 함수
### 3.3 Color Utility 함수

## 4. 레이, 간단한 카메라, 배경
### 4.1 ray class
### 4.2 장면에 광선 쏘기


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





## 5. 구를 더하기
### 5.1 광선-구 교차
### 5.2 첫번째 레이 트레이싱 이미지 생성

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

### 6.1 표면 법선을 사용한 음영
### 6.2 광선-구 교차 코드 단순화
### 6.3 Hittable 오브젝트에 대한 추상화
### 6.4 앞면 vs 뒷면
### 6.5 Hittable 오브젝트 목록
### 6.6 C++의 몇 가지 새로운 기능
### 6.7 공통 상수 및 유틸리티 함수

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
