# Ray Tracing in One Weekend - by Peter Shirley

[scratchapixel](Scratchapixel번역)보다 [더 간단한 튜토리얼](https://raytracing.github.io/books/RayTracingInOneWeekend.html#rays,asimplecamera,andbackground/therayclass)을 해보기로 했다...

## 2.2 Creating an Image File
이 항목에 있는 C++ 코드를 C언어 + mlx로 바꿔보았다..

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
이 코드의 결과물을 ppm 파일로 저장하면 이렇게 된다. 

![img-1 01-first-ppm-image](https://user-images.githubusercontent.com/53321189/89877582-40e14200-dbfb-11ea-8ce4-dd575868d8f1.png)

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


이미지 계의 hello world가 완성되었다!

