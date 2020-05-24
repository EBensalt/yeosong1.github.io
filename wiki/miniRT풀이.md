# miniRT 풀기
" MiniLibX는 학생들을 위한 간단한 윈도우 인터페이스 라이브러리 입니다."
## 주어진 라이브러리 파악하기
~~~
man minilibx_mms_20200219/man/man3/파일이름
~~~

하나씩 살펴보자.

~~~
void *   mlx_init ();
~~~

- 내 소프트웨어와 디스플레이를 연결해준다.
- 연결실패시 NULL 리턴
- 아니면 non-null pointer 리턴, as a connection identifier

### 이벤트 제어
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

mlx_loop_hook()는 앞의 함수들과 같은데, 아무 이벤트도 일어나지 않을 경우 인자로 받았던 함수가 호출됩니다.<br>
이벤트를 포착했을 경우, MiniLibX는 아래와 같이 해당 함수를 고정 파라미터로 호출합니다.

- expose_hook(void *param);
- key_hook(int keycode, void *param);
- mouse_hook(int button, int x, int y, void *param);
- loop_hook(void *param);

함수 이름은 임의로 쓴 것임. 저것들은 이벤트에 따라 매개 변수를 구별하는 데 사용됩니다. 이 기능들은 MiniLibX의 일부가 *아닙니다*.<br>
param은 mlx_어쩌구_hook 호출에 지정된 주소다. 이 주소는 MiniLibX에 의해서는 절대 사용되거나 수정되지 않는다.<br>
키, 마우스 이벤트에 있어서 추가적인 정보가 전달된다. 그 정보는 아래와 같다:
  - keycode = 무슨 키가 눌렸는지
  - x, y = 창에서 눌린 마우스 클릭 좌표(X11의 경우, include 파일 "keysymdef.h"를 확인하시고, MacOS의 경우 그냥 해보세요 :) )
  - button = 어느 마우스 버튼이 눌렸는지

### 이미지 조작하기

~~~
void *            mlx_new_image ( void *mlx_ptr, int width, int height );
~~~

- 새 이미지를 메모리에 생성시킨다.
- 나중에 이 이미지를 조작할 때 필요한 void* 식별자를 리턴 합니다.
- 이미지 사이즈(width, height)랑 mlx_ptr연결 식별자만 있으면 됩니다.

-------------------------------------

사용자는 이미지 내부를 그릴 수 있으며 (아래 참조) 언제든지 지정된 창 내에서 이미지를 덤프하여 화면에 표시 할 수 있습니다.<br>
이것은 mlx_put_image_to_window ()를 사용하여 수행됩니다.
~~~
int               mlx_put_image_to_window ( void *mlx_ptr, void *win_ptr, void *img_ptr, int x, int y );
~~~
- 여기에는 디스플레이 연결, 사용할 창 및 이미지 (각각 mlx_ptr, win_ptr 및 img_ptr)에 대한 세 개의 식별자가 필요합니다.
- (x, y) 좌표는 창에서 이미지를 배치 할 위치를 정의합니다.

-----------------------------------------

~~~
char *            mlx_get_data_addr ( void *img_ptr, int *bits_per_pixel, int *size_line, int *endian );
~~~

- mlx_get_data_addr ()는 생성된 이미지에 대한 정보를 리턴해서 사용자가 나중에 이미지를 수정할 수 있도록 합니다.
- img_ptr는 사용할 이미지를 지정합니다.
- 다음 세 개의 매개 변수는 세 개의 다른 유효한 정수의 주소여야 합니다.
  - bits_per_pixel = 픽셀 색상 (이미지의 깊이라고도 함)을 나타내는 데 필요한 비트 수.
  - size_line =이미지의 한 줄을 메모리에 저장하는 데 사용되는 바이트 수. 이 정보는 이미지에서 한 줄에서 다른 줄로 이동하는 데 필요.
  - endian = 이미지의 픽셀 색상을 리틀 엔디안(endian == 0) 또는 빅 엔디안(endian == 1)으로 저장해야하는지 알려줍니다.
* 빅 엔디언: 앞 주소에 큰 바이트부터 기록. 사람 생각과 비슷.
* 리틀 엔디언: 앞 주소에 작은 바이트부터 기록. 인텔 계열의 디폴트.

mlx_get_data_addr은 이미지가 저장된 메모리 영역의 시작을 나타내는 char * 주소를 리턴한다.

-> 이 주소로부터, 첫 번째 bits_per_pixel 비트가 이미지의 제일 첫 줄의 첫 번째 픽셀의 색상을 나타낸다.<br>
-> 두 번째 그룹의 bits_per_pixel 비트는 첫째 줄의 두번째 픽셀을 나타내고 그런 식으로 쭉 나간다.<br>
-> 주소에 size_line을 추가해서 두 번째 줄 시작점을 얻는다.<br>
-> 그런 식으로 이미지의 모든 픽셀에 도달 할 수 있습니다.

--------------------------------------
~~~
int               mlx_destroy_image ( void *mlx_ptr, void *img_ptr );
~~~

주어진 이미지(img_ptr)을 삭제한다.

--------------------------------------

### 이미지 내부에 색상 저장하기

~~~
unsigned int      mlx_get_color_value ( void *mlx_ptr, int color );
~~~

디스플레이에 따라 픽셀 색상을 저장하는 데 사용되는 비트 수가 변경 될 수 있습니다.
사용자는 일반적으로 각 구성 요소에 1 바이트를 사용하여 RGB 모드에서 색상을 나타냅니다.(mlx_pixel_put 설명서 참조)
이것은 이미지의 bits_per_pixel 요구사항에 맞게 변환되어야 하며, 디스플레이에서 색상을 이해할 수 있도록 해야 합니다.
이것이 mlx_get_color_value () 함수의 목적입니다.

표준 RGB 색상 매개 변수를 사용하고 unsigned int 값을 리턴합니다.
이 값의 bits_per_pixel 최하위 비트는 이미지에 저장 될 수 있습니다.
변환이 필요하지 않은 경우 (예 : 24 비트 깊이 또는 32 비트 깊이의 경우)이 기능을 사용하지 않아도 됩니다.

최하위 비트 위치는 로컬 컴퓨터의 엔디안에 따라 다릅니다.
이미지의 엔디안이 로컬 엔디안과 다른 경우(= X11 네트워크 환경일 때), 값을 사용하기 전에 변환해야 합니다.


### XPM AND PNG IMAGES

~~~
void *            mlx_xpm_to_image ( void *mlx_ptr, char **xpm_data, int *width, int *height );
void *            mlx_xpm_file_to_image ( void *mlx_ptr, char *filename, int *width, int *height );
void *            mlx_png_file_to_image ( void *mlx_ptr, char *filename, int *width, int *height );
~~~

이 세 함수는 같은 방식으로 새 이미지를 생성합니다.
어느 함수를 썼냐에 따라 각각 xpm_data, filename이 지정됩니다.
MinilibX는 xpm과 png를 다룰 때, 표준 Xpm과 pmg 라이브러리를 사용하지 않는다는 점을 유념해주세요.
모든 타입의 xpm과 png 이미지를 읽지는 못할 수도 있어요. 어쨌거나 transparency는 제어 됩니다.

### 창 관리하기

void *    mlx_new_window ( void *mlx_ptr, int size_x, int size_y, char *title );
int       mlx_clear_window ( void *mlx_ptr, void *win_ptr );
int       mlx_destroy_window ( void *mlx_ptr, void *win_ptr );


### 창에 그리기 Drawing inside windows.
int       mlx_pixel_put ( void *mlx_ptr, void *win_ptr, int x, int y, int color );
int       mlx_string_put  (  void *mlx_ptr, void *win_ptr, int x, int y, int color, char *string);
