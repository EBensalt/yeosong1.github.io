# 미니쉘 시작하기
히히 쉘 이름 띄우고, exit 오면 끄기

```C
#include <unistd.h>
#include <stdlib.h>
#define GR_SH "\x1B[32;1m""여름쉘> ""\x1B[m"
#include "get_next_line.h"
#include "ft_printf.h"

#include <string.h>

int		command(char *line)
{
	if (!strcmp(line,"exit"))
		exit(0);


	return (0);
}

int		main(void)
{
	int	r;
	char	*line;

	line = NULL;
	while (ft_printf(GR_SH)  && (r = get_next_line(&line)) > 0)
	{
		if (!command(line) && !!strcmp(line,"\0"))
		{
			ft_printf(GR_SH);
			ft_printf("%s: command not found\n", line);
		}
		free(line);
		line=NULL;
	}
}
```
