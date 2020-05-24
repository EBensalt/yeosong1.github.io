# miniRT 풀기

## 주어진 라이브러리 파악하기
~~~
man minilibx_mms_20200219/man/man3/파일이름
~~~

### Simple Window Interface Library for students

~~~
void *    mlx_init ();
~~~

- 내 소프트웨어와 디스플레이를 연결해준다.
- 연결실패시 NULL 리턴
- 아니면 non-null pointer 리턴, as a connection identifier

### Handle events
그래픽 시스템은 양방향이다. 한쪽에서는 스크린에 디스플레이할 픽셀, 이미지 등을 명령하고, 한쪽에서는 키보드나 마우스로부터 "이벤트"를 받는다.
~~~
int       mlx_loop ( void *mlx_ptr );
~~~
- 이벤트를 받기위해서 꼭 써야하는 함수.
- 이 함수는 리턴 안함. 키보드나 마우스로부터 받은 이벤트를 기다리는 무한루프고, 이벤트에 연결되는 사용자정의 함수를 호출한다. 
- mlx_ptr 요 한 개의 파라미터가 필요함(see the mlx manual)

다음 세 가지 이벤트에 다른 함수를 할당 할 수 있습니다.
  - 키를 눌렀습니다
  - 마우스 버튼을 눌렀습니다
  - 창의 일부를 다시 그려야합니다
    - "expose"이벤트라고 함
    - 유닉스 / 리눅스 X11 환경에서 처리하는 것은 프로그램의 일입니다.
    - 그러나 반대로 MacOS에서는 발생하지 않습니다
각 창은 동일한 이벤트에 대해 다른 기능을 정의 할 수 있습니다.

-----------------------------------------

~~~
int       mlx_key_hook ( void *win_ptr, int (*funct_ptr)(), void *param );
int       mlx_mouse_hook ( void *win_ptr, int (*funct_ptr)(), void *param );
int       mlx_expose_hook ( void *win_ptr, int (*funct_ptr)(), void *param );
~~~

이 세가지 함수는 모두 같은 방식으로 작동합니다.
- funct_ptr는 이벤트 발생시 당신이 호출하고 싶은 함수를 가리키는 함수 포인터 입니다. 이 할당은
- win_ptr에 의해 특정된 윈도우에만 적용됩니다.
- param의 주소는 호출될 때마다 전달되고 필요한 매개 변수를 저장하는 데 사용해야합니다.

-----------------------------------------

~~~
int       mlx_loop_hook ( void *mlx_ptr, int (*funct_ptr)(), void *param );
~~~

mlx_loop_hook()는 앞의 함수들과 같은데, 아무 이벤트도 일어나지 않을 경우 인자로 받았던 함수가 호출됩니다.
이벤트를 포착했을 경우, MiniLibX는 아래와 같이 해당 함수를 고정 파라미터로 호출합니다.

- expose_hook(void *param);
- key_hook(int keycode, void *param);
- mouse_hook(int button, int x, int y, void *param);
- loop_hook(void *param);

함수 이름은 임의로 쓴 것임. 저것들은 이벤트에 따라 매개 변수를 구별하는 데 사용됩니다. 이 기능들은 MiniLibX의 일부가 *아닙니다*.
param은 mlx_어쩌구_hook 호출에 지정된 주소다. 이 주소는 MiniLibX에 의해서는 절대 사용되거나 수정되지 않는다.
키, 마우스 이벤트에 있어서 추가적인 정보가 전달된다. 그 정보는 아래와 같다:
  - keycode = 무슨 키가 눌렸는지
  - x, y = 창에서 눌린 마우스 클릭 좌표(X11의 경우, include 파일 "keysymdef.h"를 확인하시고, MacOS의 경우 그냥 해보세요 :) )
  - button = 어느 마우스 버튼이 눌렸는지

### Manipulating images

~~~
void *            mlx_new_image ( void *mlx_ptr, int width, int height );
~~~

- 새 이미지를 메모리에 생성시킨다.
- 나중에 이 이미지를 조작할 때 필요한 void* 식별자를 리턴 합니다.
- 이미지 사이즈(width, height)랑 mlx_ptr연결 식별자만 있으면 됩니다.

-------------------------------------

사용자는 이미지 내부를 그릴 수 있으며 (아래 참조) 언제든지 지정된 창 내에서 이미지를 덤프하여 화면에 표시 할 수 있습니다. 이것은 mlx_put_image_to_window ()를 사용하여 수행됩니다. 여기에는 디스플레이 연결, 사용할 창 및 이미지 (각각 mlx_ptr, win_ptr 및 img_ptr)에 대한 세 개의 식별자가 필요합니다. (x, y) 좌표는 창에서 이미지를 배치 할 위치를 정의합니다.



char *            mlx_get_data_addr ( void *img_ptr, int *bits_per_pixel, int *size_line, int *endian );
int               mlx_put_image_to_window ( void *mlx_ptr, void *win_ptr, void *img_ptr, int x, int y );
unsigned int      mlx_get_color_value ( void *mlx_ptr, int color );
void *            mlx_xpm_to_image ( void *mlx_ptr, char **xpm_data, int *width, int *height );
void *            mlx_xpm_file_to_image ( void *mlx_ptr, char *filename, int *width, int *height );
void *            mlx_png_file_to_image ( void *mlx_ptr, char *filename, int *width, int *height );
int               mlx_destroy_image ( void *mlx_ptr, void *img_ptr );


### Managing windows

void *    mlx_new_window ( void *mlx_ptr, int size_x, int size_y, char *title );
int       mlx_clear_window ( void *mlx_ptr, void *win_ptr );
int       mlx_destroy_window ( void *mlx_ptr, void *win_ptr );


### Drawing inside windows
int       mlx_pixel_put ( void *mlx_ptr, void *win_ptr, int x, int y, int color );
int       mlx_string_put  (  void *mlx_ptr, void *win_ptr, int x, int y, int color, char *string);
