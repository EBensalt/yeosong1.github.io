# miniRT 창 띄우기

* 현재 환경: macOS Mojave 10.14.6


## 강의에 나오는대로 하기
0. intra 서브젝트 페이지에서 minilibx_opengl.tgz와  minilibx_mms_20200219_beta.tgz 다운로드, 압축 풀어서
1. 내 컴퓨터의 홈 디렉토리에 miniRT 폴더를 만들고 minilibx_mms_20200219와 minilibx_opengl_20191021를 넣었다. 
2. 두 폴더에 들어가서 make 해서 라이브러리 파일을 만든다.
3. /usr/local/include/에 헤더를 복사해 넣는다.

~~~
cp miniRT/minilibx_mms_20200219/*.h /usr/local/include/ 
cp miniRT/minilibx_opengl_20191021/*.h /usr/local/include/ 
~~~

3. /usr/local/lib/에 라이브러리 파일을 복사해 넣는다.

~~~
cp miniRT/minilibx_mms_20200219/libmlx.dylib /usr/local/lib/
cp miniRT/minilibx_opengl_20191021/libmlx.a /usr/local/lib/ 
~~~

4. [mlx 개발자 OL님이 인트라 강의에서 시키는대로](https://elearning.intra.42.fr/notions/minilibx/subnotions/mlx-introduction/videos/introduction-to-minilibx) start.c 파일 작성.

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

5. `cc -I /usr/local/include/ miniRT/start.c -L /usr/local/lib/ -lmlx -framework OpenGL -framework AppKit`

6. `./a.out` 해서 확인.

창이 뜬다.<br>
<img width="612" alt="스크린샷 2020-05-27 오후 8 47 32" src="https://user-images.githubusercontent.com/53321189/83015376-5498f580-a05b-11ea-9cfa-86d9b3c732bb.png">







## 내 루트에서 하기

0. intra 서브젝트 페이지에서 minilibx_opengl.tgz와  minilibx_mms_20200219_beta.tgz 다운로드, 압축 풀어서 내 miniRT 폴더에 넣었다.
1. 각 폴더 들어가서 make
2. 해서 나온 라이브러리 파일을 내 루트에 복사.

~~~
cp minilibx_mms_20200219/libmlx.dylib .
cp minilibx_opengl_20191021/libmlx.a .
~~~

3. [mlx 개발자 OL님이 인트라 강의에서 시키는대로](https://elearning.intra.42.fr/notions/minilibx/subnotions/mlx-introduction/videos/introduction-to-minilibx) start.c 작성

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

4. `gcc -I minilibx_mms_20200219/ -I minilibx_opengl_20191021/ libmlx.dylib libmlx.a start.c`

5. `./a.out` 해서 확인

창이 뜬다.<br>
<img width="612" alt="스크린샷 2020-05-27 오후 8 47 32" src="https://user-images.githubusercontent.com/53321189/83015376-5498f580-a05b-11ea-9cfa-86d9b3c732bb.png">

