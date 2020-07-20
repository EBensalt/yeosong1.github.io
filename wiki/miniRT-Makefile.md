# miniRT Makefile

1. 빈 창 띄우기 까지 한 내용과 기본 룰(all, clean, fclean, re)을 작성해보았다.

~~~
NAME = miniRT
SRCS_LIST = start.c

CC = gcc
FLAGS = -Wall -Wextra -Werror
INCLS = ./includes/
MMS_DIR = minilibx_mms_20200219
OPENGL_DIR = minilibx_opengl_20191021

LMLX = libmlx.dylib libmlx.a
OBJS = $(SRCS:.c=.o)
SRCS = $(addprefix $(SRCS_DIR), $(SRCS_LIST))
SRCS_DIR = ./srcs/



all: $(NAME)

$(LMLX) :
	@$(MAKE) -C $(MMS_DIR)
	@$(MAKE) -C $(OPENGL_DIR)

$(NAME) : $(LMLX)
	@cp $(MMS_DIR)/libmlx.dylib .
	@cp $(OPENGL_DIR)/libmlx.a .
	@$(CC) $(FLAGS) -I $(INCLS) $(LMLX) $(SRCS) -o $(NAME)
	@echo "\x1B[32;1m"">>>>> miniRT is successfully made.""\x1B[m"
clean:
	rm -rf $(NAME)

fclean: clean
	$(MAKE) -C $(MMS_DIR) clean
	$(MAKE) -C $(OPENGL_DIR) clean

re: fclean all


bonus: all
~~~
