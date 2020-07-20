# miniRT를 esc 키를 눌러서 종료해보기

[intra의 mlx 소개 강의 2](https://elearning.intra.42.fr/notions/minilibx/subnotions/mlx-events/videos/minilibx-events)를 보고 종료 키를 추가해보았다.

~~~
#include "minirt_main.h"

int	press_esc_key(int key, void *param)
{
	if (key == 53 && param)
	{
		write(1, "The Window is stopped.\n", 23);
		exit(0);
	}

	return (0);
}

int main()
{
	void	*mlx_ptr;
	void	*win_ptr;

	mlx_ptr = mlx_init();
	win_ptr = mlx_new_window(mlx_ptr, 500, 500, "Nice to meet you");
	mlx_key_hook(win_ptr, press_esc_key, win_ptr);
	mlx_loop(mlx_ptr);
}
~~~

강의에서 mlx_hook의 void* param 자리에 win_ptr이 들어갈지 mlx_ptr이 들어갈지 잘 고민해보라고 한다.<br>
메모리 뭐 할당하는 거 생기면 그때 해제 순서랑 맞춰서 다시 고민하기로 하고 일단 지금은 exit만 해봄
