# miniRT 과정

* 현재 환경: macOS Mojave 10.14.6

창을 띄워보자

0. intra 서브젝트 페이지에서 minilibx_opengl.tgz와  minilibx_mms_20200219_beta.tgz 다운로드, 압축 풀어서 내 miniRT 폴더에 넣기

~~~
현재 상태

$ ls
minilibx_mms_20200219		minilibx_opengl_20191021
~~~

1. 각 폴더 들어가서 make
2. 해서 나온 실행 파일을 내 루트에 복사.

cp minilibx_mms_20200219/libmlx.dylib .
cp minilibx_opengl_20191021/libmlx.a .

3. [mlx 개발자 OL님이 인트라 강의에서 시키는대로](https://elearning.intra.42.fr/notions/minilibx/subnotions/mlx-introduction/videos/introduction-to-minilibx) `vim start.c` 작성

~~~
#include "mlx.h"

int	main()
{
	void	*mlx_ptr;
	void	*win_ptr;


	mlx_ptr = mlx_init();
	win_ptr = mlx_new_window(mlx_ptr, 500, 500, "띄울_창_타이틀");
	mlx_loop(mlx_ptr);
}
~~~

~~~
gcc -I minilibx_mms_20200219/ -I minilibx_opengl_20191021/ libmlx.dylib libmlx.a start.c

./a.out
~~~

창이 뜬다.
<img width="612" alt="스크린샷 2020-05-27 오후 8 47 32" src="https://user-images.githubusercontent.com/53321189/83015376-5498f580-a05b-11ea-9cfa-86d9b3c732bb.png">

